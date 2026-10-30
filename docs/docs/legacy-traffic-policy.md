---
sidebar_position: 21
title: Legacy traffic policy
---

# Legacy traffic policy

:::caution Deprecated
Legacy is deprecated. Only entries that used it in 2.x keep it. A future release will remove it. After you change an entry to Governed, the UI does not offer Legacy again.
:::

Legacy keeps the budget and the retry timing of 2.x. New entries use Governed. See [Mesh Traffic Policy](traffic-policy.md).

At each startup or reload, an entry on Legacy logs this warning:

```
The Legacy traffic policy is deprecated and will be removed in a future release. Select Governed in Global Settings > Mesh Traffic Policy.
```

## How Legacy differs from Governed

| Item | Legacy behavior |
|---|---|
| Budget | One bucket of 20 credits. It gets 1 credit every 120 seconds (approximately 30 each hour). |
| Cost | 1 credit for each automatic request. A full neighbor scan costs 1 credit. |
| Service calls, forms and buttons | Not metered |
| Empty budget | A denied status or telemetry poll counts as a failure, and the node backs off. A denied login also starts a backoff. The integration skips a denied neighbor scan. |
| Retry spacing | `interval / 62 x 2^failures`, never more than the poll interval, for all retries |
| Route reset | After 3 failures, the integration resets the route. The next poll floods. There is no route healing. |
| Auto-disable | Repeaters only, and status polls only. The log line ends with `This will reset on restart.` To resume the node, restart Home Assistant or reload the entry. |
| State across restarts | Not saved. A restart resets the budget, the schedules and the auto-disabled nodes. |
| Request Rate Limiter sensor | Shows the single bucket. It has no attributes. |

## Current behavior

Traffic budgets and auto-disable are disabled in current releases. The former policy setting is retained on existing entries for compatibility and no longer appears in Global Settings.
