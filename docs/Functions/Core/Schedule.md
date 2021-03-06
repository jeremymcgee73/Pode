# Schedule

## Description

The `schedule` function lets you create long-running async tasks. Unlike a `timer`, when a schedule is triggered its logic is run in its own runspace - so they don't affect each other if they take a while to process.

Schedule triggers are defined using [`cron expressions`](../../../Tutorials/Misc/CronExpressions), basic syntax is supported as well as some predefined expressions. They can start immediately, have a delayed start time, and also have a a defined end time.

## Examples

### Example 1

The following example will create a `schedule` that triggers every Tuesday at midnight, but will only trigger 4 times:

```powershell
Server {
    schedule 'tuesdays' '0 0 * * TUE' -limit 4 {
        # logic
    }
}
```

### Example 2

The following example will create a `schedule` that triggers every 5 past the hour, starting in 2hrs:

```powershell
Server {
    schedule 'hourly-start' '5 * * * *' -start ([DateTime]::Now.AddHours(2)) {
        # logic
    }
}
```

### Example 3

The following example will create a `schedule` using a predefined cron to trigger every minute:

```powershell
Server {
    schedule 'minutely' '@minutely' {
        # logic
    }
}
```

### Example 4

The following example will create a `schedule` using multiple predefined cron expressions to trigger every minute and every hour:

```powershell
Server {
    schedule 'minutely' @('@minutely', '@hourly') {
        # logic
    }
}
```

## Parameters

| Name | Type | Required | Description | Default |
| ---- | ---- | -------- | ----------- | ------- |
| Name | string | true | A unique name for the schedule | empty |
| Cron | string[] | true | A cron expression, or multiple cron expressions, that define when the schedule should trigger | empty |
| ScriptBlock | scriptblock | true | The main logic that will be invoked on each trigger | null |
| StartTime | datetime | false | Defines when the schedule should start | now |
| EndTime | datetime | false | Defines when the schedule should end | never |
| Limit | int | false | The number of times the schedule should trigger before it is removed; 0 is unlimited | 0 |
