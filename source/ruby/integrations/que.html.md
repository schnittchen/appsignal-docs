---
title: "Que"
---

[Que](https://github.com/chanks/que/) is a high-performance alternative to DelayedJob or QueueClassic that improves the reliability of your application by protecting your jobs with the same ACID guarantees as the rest of your data.

AppSignal supports Que since version `2.4.1` of the [AppSignal Ruby gem](/ruby). Only direct Que usage is supported right now. The ActiveJob wrapper is not.

No manual integration is necessary. If Que is detected when AppSignal starts we automatically hook into Que to track exceptions and performance issues.

Job names are automatically detected based on the Que worker class name and are suffixed with the `run` method name, resulting in something like: `MyWorker#run`.

You can recognize events from Que with the name `perform_job.que` in the event timeline on the performance incident detail page.
