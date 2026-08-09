---
'@mastra/observability': patch
---

Fixed workflow traces breaking apart after suspend/resume: a resumed span whose parent span is no longer live (it ended when the run suspended, possibly in another process) now correctly reports `isRootSpan: false` when it carries a persisted `parentSpanId`. The base definition changed from `!this.parent` to `!this.parent && !this.parentSpanId`, so a span with a persisted parent ID continues its original trace instead of becoming a new root — restoring trace continuity for every exporter (Braintrust, Datadog, Cloud, OTLP, Platform, Sentry, PostHog, Langfuse, Console) at once. Fixes #20771.
