---
title: "Multi Data Center Replication v3 Scheduling Full Sync"
project: riakee
version: 1.4.0+
document: cookbook
toc: true
audience: intermediate
keywords: [mdc, repl, schedule, fullsync]
---

## Scheduling Full Synchronization

The `fullsync_interval` parameter can be configured in the `riak-repl` section of app.config with:

* a single integer value representing the duration to wait *in minutes* between fullsyncs

    **OR**


* a list of `[{"clustername", time_in_minutes}, {"clustername", time_in_minutes},…]` pairs for each sink participating in fullsync replication. Note the commas separating each pair, and `[` `]` surrounding the entire list. Also, `%%` represent `app.config` comments.

####Examples

######Single integer time in minutes:

```
{riak_repl, [
   {data_root, "/configured/repl/data/root"},
   {fullsync_interval, 90} %% fullsync runs every 90 minutes
  ]}
```

######List of multiple sinks with separate times in minutes.

```
{riak_repl, [
   {data_root, "/configured/repl/data/root"},
   % clusters sink_boston + sink_newyork have difference intervals (in minutes)
   {fullsync_interval, [
                {"sink_boston", 120},  %% fullsync to sink_boston with run every 120 minutes
                {"sink_newyork", 90}]} %% fullsync to sink_newyork with run every 90 minutes
  ]}
```

### Additional fullsync stats

Additional fullsync stats per sink have been added in Riak EE 1.4.

* `fullsyncs_completed`

        The number of fullsyncs that have been completed to the specified sink cluster.

* `fullsync_start_time`

        The time the current fullsink to the specified cluster began.

* `last_fullsync_duration`

        The duration (in seconds) of the last completed fullsync.