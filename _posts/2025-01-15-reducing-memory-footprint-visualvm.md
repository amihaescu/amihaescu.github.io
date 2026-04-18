---
layout: post
title: "Reducing memory footprint by 6x — a VisualVM deep dive"
date: 2025-01-15
category: System Design
excerpt: "How profiling an apparently healthy Java service revealed surprising heap allocations, and the systematic approach we took to cut resource usage dramatically."
read_time: 8
---

A service that had passed all performance tests in CI was quietly consuming 6× more heap than it needed in production. This is how we found it and fixed it.

## The symptom

Our team noticed that one of the Nexus Dashboard backend services was being OOM-killed in production roughly once a week. The JVM was configured with a 2 GB heap limit — reasonable for the workload — but heap utilisation would creep up steadily until the GC couldn't keep pace.

The service handled aggregation queries over OpenSearch, transforming raw search results into structured analytics payloads for the React frontend. On paper, nothing in that description should produce an unbounded memory leak.

## Setting up VisualVM

VisualVM is bundled with the JDK but often overlooked in favour of paid APM tools. For a targeted heap investigation, it's more than sufficient.

Connect it to the running process via JMX — add these flags to your JVM startup:

```
-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.port=9010
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
```

Then open VisualVM, connect to the remote host, and navigate to the **Sampler** tab.

## What the heap dump revealed

After triggering a heap dump mid-load-test, the first thing that stood out in the object histogram was `byte[]` instances accounting for nearly 40% of retained heap. That alone isn't unusual — String internals are byte arrays. But the count was orders of magnitude higher than the number of active requests.

Drilling into the reference chains pointed to our response serialisation layer. We were using Jackson to serialise large OpenSearch result sets, and somewhere in the pipeline a `ByteArrayOutputStream` was being created *per result document* rather than per request, and none of them were being pooled.

The culprit: a utility method deep in a shared library that called `objectMapper.writeValueAsBytes(doc)` in a loop, accumulating byte arrays that weren't collected until the entire request completed.

## The fix

The fix was straightforward once located: replace the per-document serialisation with a single streaming write using `ObjectMapper.writer().writeValues(outputStream)`. This reduced peak heap allocation for a typical aggregation request from ~180 MB to ~28 MB.

Across the service fleet, sustained heap utilisation dropped by roughly 6×, and the OOM-kills stopped.

## Lessons

- **Profile before you optimise.** The leak was in a shared utility — nobody would have found it without tooling.
- **Heap histograms are fast.** You don't always need a full heap dump. A live histogram in VisualVM takes seconds and often points directly at the problem class.
- **Streaming serialisation is almost always better.** If you're handling lists of documents, write them as a stream rather than materialising the whole payload in memory.
