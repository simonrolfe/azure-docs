---
title: Build complex schedules and advanced recurrence with Azure Scheduler
description: Learn how to build complex schedules and advanced recurrence with Azure Scheduler.
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''

ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli

---
# Build complex schedules and advanced recurrence with Azure Scheduler

The core of an Azure Scheduler job is the schedule. The schedule determines when and how Scheduler executes the job. 

You can use Scheduler to set multiple one-time and recurring schedules for a job. One-time schedules fire once at a specified time. One-time schedules effectively are recurring schedules that execute only once. Recurring schedules fire on a predetermined frequency.

With this flexibility, you can use Scheduler for a wide variety of business scenarios:

* **Periodic data cleanup**. For example, every day, delete all tweets that are older than three months.
* **Archiving**. For example, every month, push invoice history to a backup service.
* **Requests for external data**. For example, every 15 minutes, pull a new ski weather report from NOAA.
* **Image processing**. For example, every weekday, during off-peak hours, use cloud computing to compress images that were uploaded that day.

In this article, we walk through example jobs that you can create by using Scheduler. We provide the JSON data that describes each schedule. If you use the [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx), you can use this same JSON to [create a Scheduler job](https://msdn.microsoft.com/library/mt629145.aspx).

## Supported scenarios
The examples in this article illustrate the breadth of scenarios that Scheduler supports. The examples broadly illustrate how to create schedules for many behavior patterns, including:

* Run once at a specific date and time.
* Run and recur a specific number of times.
* Run immediately and recur.
* Run and recur every *n* minutes, hours, days, weeks, or months, starting at a specific time.
* Run and recur at a weekly or monthly frequency, but only on specific days of the week or on specific days of the month.
* Run and recur multiple times in a period. For example, on the last Friday and last Monday of every month, or at 5:15 AM and at 5:15 PM every day.

## Date and date-time
Date references in Scheduler jobs follow the [ISO 8601 specification](http://en.wikipedia.org/wiki/ISO_8601), and include only the date.

Date-time references in Scheduler jobs follow the [ISO 8601 specification](http://en.wikipedia.org/wiki/ISO_8601), and include both date and time. A date-time that doesn't specify a UTC offset is assumed to be UTC.  

## Use JSON and the REST API to create a schedule
To create a basic schedule by using the [Scheduler REST API](https://msdn.microsoft.com/library/mt629143), first [register your subscription with a resource provider](https://msdn.microsoft.com/library/azure/dn790548.aspx). The provider name for Scheduler is **Microsoft.Scheduler**. Then, [create a job collection](https://msdn.microsoft.com/library/mt629159.aspx). Finally, [create a job](https://msdn.microsoft.com/library/mt629145.aspx). 

When you create a job, you can specify scheduling and recurrence by using JSON, like in this excerpt:

    {
        "startTime": "2012-08-04T00:00Z", // Optional
         …
        "recurrence":                     // Optional
        {
            "frequency": "week",     // Can be "year", "month", "day", "week", "hour", or "minute"
            "interval": 1,                // How often to fire
            "schedule":                   // Optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // Optional (default to recur infinitely)
            "endTime": "2012-11-04",      // Optional (default to recur infinitely)
        },
        …
    }

## Job schema basics
The following table provides a high-level overview of the major elements that you use to set recurrence and scheduling in a job:

| JSON name | Description |
|:--- |:--- |
| **startTime** |A date-time value. For basic schedules, **startTime** is the first occurrence. For complex schedules, the job starts no sooner than **startTime**. |
| **recurrence** |Specifies recurrence rules for the job, and the recurrence at which the job runs. The recurrence object supports the elements **frequency**, **interval**, **endTime**, **count**, and **schedule**. If **recurrence** is defined, **frequency** is required. Other **recurrence** elements are optional. |
| **frequency** |A string that represents the frequency unit at which the job recurs. Supported values are "minute", "hour", "day", "week", and "month". |
| **interval** |A positive integer. **interval** denotes the interval for the **frequency** value that determines how often the job runs. For example, if **interval** is 3 and **frequency** is "week", the job recurs every three weeks.<br /><br />Scheduler supports a maximum **interval** of 18 for monthly frequency, 78 for weekly frequency, and 548 for daily frequency. For hour and minute frequency, the supported range is 1 <= **interval** <= 1000. |
| **endTime** |A string that specifies the date-time beyond which the job doesn't run. You can set a value for **endTime** that's in the past. If **endTime** and **count** aren't specified, the job runs infinitely. You can't include both **endTime** and **count** in the same job. |
| **count** |A positive integer (greater than zero) that specifies the number of times the job runs before it's completed.<br /><br />**count** represents the number of times the job runs before being determined completed. For example, for a job that's executed daily with a **count** of 5 and a start date of Monday, the job completes after execution on Friday. If the start date is in the past, the first execution is calculated from the creation time.<br /><br />If no **endTime** or **count** is specified, the job runs infinitely. You can't include both **endTime** and **count** in the same job. |
| **schedule** |A job with a specified frequency alters its recurrence based on a recurrence schedule. A **schedule** value contains modifications based on minutes, hours, week days, month days, and week number. |

## Job schema defaults, limits, and examples
Later in the article, we discuss each of the following elements in detail:

| JSON name | Value type | Required? | Default value | Valid values | Example |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** |string |No |None |ISO 8601 date-times |`"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** |object |No |None |The recurrence object |`"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **frequency** |string |Yes |None |"minute", "hour", "day", "week", "month" |`"frequency" : "hour"` |
| **interval** |number |Yes |None |1 to 1000 |`"interval":10` |
| **endTime** |string |No |None |Date-time value that represents a time in the future |`"endTime" : "2013-02-09T09:30:00-08:00"` |
| **count** |number |No |None |>= 1 |`"count": 5` |
| **schedule** |object |No |None |The schedule object |`"schedule" : { "minute" : [30], "hour" : [8,17] }` |

## Deep dive: startTime
The following table describes how **startTime** controls the way that a job runs:

| startTime value | No recurrence | Recurrence, no schedule | Recurrence with schedule |
|:--- |:--- |:--- |:--- |
| **No start time** |Run once immediately. |Run once immediately. Run subsequent executions calculated from the last execution time. |Run once immediately.<br /><br />Run subsequent executions based on a recurrence schedule. |
| **Start time in the past** |Run once immediately. |Calculate the first future execution time after start time, and run at that time.<br /><br />Run subsequent executions calculated from the last execution time. <br /><br />For more information, see the example that follows this table. |Job starts *no sooner than* the specified start time. The first occurrence is based on the schedule calculated from the start time.<br /><br />Run subsequent executions based on a recurrence schedule. |
| **Start time in the future or the current time** |Run once at the specified start time. |Run once at the specified start time.<br /><br />Run subsequent executions calculated from the last execution time.|Job starts *no sooner than* the specified start time. The first occurrence is based on the schedule, calculated from the start time.<br /><br />Run subsequent executions based on a recurrence schedule. |

Let's look at an example of what happens when **startTime** is in the past, with recurrence, but with no schedule.  Assume that the current time is 2015-04-08 13:00, **startTime** is 2015-04-07 14:00, and **recurrence** is every two days (defined with **frequency**: day and **interval**: 2.) Note that **startTime** is in the past, and occurs before the current time.

Under these conditions, the first execution will be on 2015-04-09 at 14:00\. The Scheduler engine calculates execution occurrences from the start time. Any instances in the past are discarded. The engine uses the next instance that occurs in the future. In this case, **startTime** is 2015-04-07 at 2:00 PM, so the next instance is two days from that time, which is 2015-04-09 at 2:00 PM.

Note that the first execution would be the same whether **startTime** is 2015-04-05 14:00 or 2015-04-01 14:00\. After the first execution, subsequent executions are calculated by using the schedule. They will be on 2015-04-11 at 2:00 PM, then on 2015-04-13 at 2:00 PM, then on 2015-04-15 at 2:00 PM, and so on.

Finally, when a job has a schedule, if hours and minutes aren’t set in the schedule, the values default to the hours and minutes of the first execution, respectively.

## Deep dive: schedule
You can use **schedule** to *limit* the number of job executions. For example, if a job with a **frequency** of "month" has a schedule that runs only on day 31, the job runs only in months that have a thirty-first day.

You can also use **schedule** to *expand* the number of job executions. For example, if a job with a **frequency** of "month" has a schedule that runs on month days 1 and 2, the job runs on the first and second days of the month instead of only once a month.

If you specify multiple schedule elements, the order of evaluation is from the largest to smallest: week number, month day, week day, hour, and minute.

The following table describes schedule elements in detail:

| JSON name | Description | Valid values |
|:--- |:--- |:--- |
| **minutes** |Minutes of the hour at which the job runs. |An array of integers. |
| **hours** |Hours of the day at which the job runs. |An array of integers. |
| **weekDays** |Days of the week the job runs. Can be specified only with a weekly frequency. |An array of any of the following values (maximum array size is 7):<br />- "Monday"<br />- "Tuesday"<br />- "Wednesday"<br />- "Thursday"<br />- "Friday"<br />- "Saturday"<br />- "Sunday"<br /><br />Not case-sensitive. |
| **monthlyOccurrences** |Determines which days of the month the job runs. Can be specified only with a monthly frequency. |An array of **monthlyOccurrences** objects:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **day** is the day of the week the job runs. For example, *{Sunday}* is every Sunday of the month. Required.<br /><br />**occurrence** is  the occurrence of the day during the month. For example,  *{Sunday, -1}* is the last Sunday of the month. Optional. |
| **monthDays** |Day of the month the job runs. Can be specified only with a monthly frequency. |An array of the following values:<br />- Any value <= -1 and >= -31<br />- Any value >= 1 and <= 31|

## Examples: Recurrence schedules
The following examples show various recurrence schedules. The examples focus on the schedule object and its subelements.

These schedules assume that **interval** is set to 1\. The examples also assume the correct **frequency** values for the values in **schedule**. For example, you can't use a **frequency** of "day" and have a **monthDays** modification in **schedule**. We describe these restrictions earlier in the article.

| Example | Description |
|:--- |:--- |
| `{"hours":[5]}` |Run at 5 AM every day.<br /><br />Scheduler matches up each value in "hours" with each value in "minutes", one by one, to create a list of all the times at which the job runs. |
| `{"minutes":[15], "hours":[5]}` |Run at 5:15 AM every day. |
| `{"minutes":[15], "hours":[5,17]}` |Run at 5:15 AM and 5:15 PM every day. |
| `{"minutes":[15,45], "hours":[5,17]}` |Run at 5:15 AM, 5:45 AM, 5:15 PM, and 5:45 PM every day. |
| `{"minutes":[0,15,30,45]}` |Run every 15 minutes. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Run every hour.<br /><br />This job runs every hour. The minute is controlled by the value for **startTime**, if it is specified. If no **startTime** value is specified, the minute is controlled by the creation time. For example, if the start time or creation time (whichever applies) is 12:25 PM, the job runs at 00:25, 01:25, 02:25, …, 23:25.<br /><br />The schedule is equivalent to a job with a **frequency** of "hour", an **interval** of 1, and no **schedule** value. The difference is that you can use this schedule with different **frequency** and **interval** values to create other jobs. For example, if **frequency** is "month", the schedule runs only once a month instead of every day (if **frequency** is "day"). |
| `{minutes:[0]}` |Run every hour on the hour.<br /><br />This job also runs every hour, but on the hour (12 AM, 1 AM, 2 AM, and so on). This is equivalent to a job with a **frequency** of "hour", a **startTime** value of zero minutes, and no **schedule**, if the frequency is "day". However, if the **frequency** is "week" or "month", the schedule executes only one day a week or one day a month, respectively. |
| `{"minutes":[15]}` |Run at 15 minutes past the hour every hour.<br /><br />Runs every hour, starting at 00:15 AM, 1:15 AM, 2:15 AM, and so on. It ends at 11:15 PM. |
| `{"hours":[17], "weekDays":["saturday"]}` |Run at 5 PM on Saturday every week. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Run at 5 PM on Monday, Wednesday, and Friday every week. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Run at 5:15 PM and 5:45 PM on Monday, Wednesday, and Friday every week. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Run at 5 AM and 5 PM on Monday, Wednesday, and Friday every week. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Run at 5:15 AM, 5:45 AM, 5:15 PM, and 5:45 PM on Monday, Wednesday, and Friday every week. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Run every 15 minutes on weekdays. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Run every 15 minutes on weekdays, between 9 AM and 4:45 PM. |
| `{"weekDays":["sunday"]}` |Run on Sundays at start time. |
| `{"weekDays":["tuesday", "thursday"]}` |Run on Tuesdays and Thursdays at start time. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Run at 6 AM on the twenty-eighth day of every month (assuming a **frequency** of "month"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Run at 6 AM on the last day of the month.<br /><br />If you want to run a job on the last day of a month, use -1 instead of day 28, 29, 30, or 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Run at 6 AM on the first and last day of every month. |
| `{monthDays":[1,-1]}` |Run on the first and last day of every month at start time. |
| `{monthDays":[1,14]}` |Run on the first and fourteenth day of every month at start time. |
| `{monthDays":[2]}` |Run on the second day of the month at start time. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Run on the first Friday of every month at 5 AM. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Run on the first Friday of every month at start time. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Run on the third Friday from the end of the month, every month, at start time. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Run on the first and last Friday of every month at 5:15 AM. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Run on the first and last Friday of every month at start time. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Run on the fifth Friday of every month at start time.<br /><br />If there is no fifth Friday in a month, the job doesn't run. You might consider using -1 instead of 5 for the occurrence if you want to run the job on the last occurring Friday of the month. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Run every 15 minutes on the last Friday of the month. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Run at 5:15 AM, 5:45 AM, 5:15 PM, and 5:45 PM on the third Wednesday of every month. |

## See also

- [What is Scheduler?](scheduler-intro.md)
- [Azure Scheduler concepts, terminology, and entity hierarchy](scheduler-concepts-terms.md)
- [Get started using Scheduler in the Azure portal](scheduler-get-started-portal.md)
- [Plans and billing in Azure Scheduler](scheduler-plans-billing.md)
- [Azure Scheduler REST API reference](https://msdn.microsoft.com/library/mt629143)
- [Azure Scheduler PowerShell cmdlets reference](scheduler-powershell-reference.md)
- [Azure Scheduler high availability and reliability](scheduler-high-availability-reliability.md)
- [Azure Scheduler limits, defaults, and error codes](scheduler-limits-defaults-errors.md)
- [Azure Scheduler outbound authentication](scheduler-outbound-authentication.md)

