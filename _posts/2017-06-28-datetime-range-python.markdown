---
title:  "Generating recurring calendar blocks in python"
date:  2017-07-06
categories: [python]
tags: [python, dates, recurring, schedules, rrule]
---


Recently I came across an interesting problem, where I had to create recurring datetime blocks on a calendar.

So the problem was, the user would select a block on the calendar and the same time block had to be replicated for the next one year:  
1. **Daily** - 365/366 blocks (based on whether its a leap year or not)  
2. **Weekly** - 52 blocks  
3. **Fortnightly/Bi-weekly** - 26 blocks  
4. **Monthly** - 12 blocks

I had the following two approaches in mind:  
1. Iteratively use the serializer to save block by block, thus creating the blocks at runtime  
2. Form a json with all the objects and then use ```many=True``` in the serializer. (later looking at the queries that were fired, DRF was actually doing an INSERT one by one.)

Okay, so the 2nd approach is the better one to follow, but then how to create all those objects from the one that the user sends?  

Again two options:
1. Use ```timedelta``` with the user input ```datetime_from``` and ```datetime_to```, and generate blocks for the next year using list comprehension or a for loop

```
datetime_from = datetime.now()
for day in xrange(366):
   print datetime_from + timedelta(days=day)
```

**OR**

```
datetime_from_list = [today - datetime.timedelta(days=day) for day in range(0, 366)]
```

But then, after I was digging deeper I came across the [dateutil](https://dateutil.readthedocs.io/en/stable/) package that I was already using in my project. More specifically [rrule](https://dateutil.readthedocs.io/en/stable/rrule.html).

What [rrule](https://dateutil.readthedocs.io/en/stable/rrule.html) promised, is very fast implementation of the recurrence rules as documented in [iCalendarRFC](http://www.ietf.org/rfc/rfc2445.txt). (I skipped going through the RFC and wanted to give it a shot) and below is the code that I came up with.


The imports
```
from calendar import isleap

from datetime import datetime
from dateutil.rrule import rrule, DAILY, WEEKLY, MONTHLY

```

For generating datetimes every 2 weeks.

```
def bi_weekly(start_date=datetime.now()):
    # returns the datetime for an year and calculates them for 14 days/2 weeks
    return rrule(WEEKLY, dtstart=start_date, count=24, interval=2)

```

For generating datetimes every 2 weeks.
```
def daily(start_date=datetime.now()):
    # returns 366/365 datetimes as the case maybe;
    count = 366 if isleap(start_date.year) else 365
    return rrule(DAILY, dtstart=start_date, count=count, interval=1)

```

For generating datetimes every week.
```
def weekly(start_date=datetime.now()):
    # returns 52 count of datetimes as there are 52 weeks in an year
    return rrule(WEEKLY, dtstart=start_date, count=52, interval=1)

```

For generating datetimes every 4 weeks.
```
def monthly(start_date=datetime.now()):
    # returns 12 count of datetimes as 12 months in an year, and caculates them
    # for every 4 weeks or 28 days
    return rrule(WEEKLY, dtstart=start_date, count=12, interval=4)
```

Is there a better way, to do this?  
Thoughts, please share in the comments section.
