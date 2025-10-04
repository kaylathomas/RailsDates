
# RailsDates

RailsDates wraps Python datetime with fluent, readable utilities inspired by Rails' ActiveSupport. It aims for developer joy and parity with common Rails idioms while remaining timezone-aware (configurable).

## Key Features
- Fluent durations: `5.minutes`, `2.days`, `1.week`  
- Relative helpers: `.ago()`, `.from_now()`  
- Boundary helpers: `beginning_of_day()`, `end_of_week()`  
- Humanization: `distance_of_time_in_words(dt)`  
- Timezone support (configurable default)

## Installation
```bash
pip install RailsDates
```

## Core Classes

### TimeInt
Enables Rails-style time duration syntax by wrapping integers with time properties.

```python
from RailsDates import TimeInt

# Create time durations
five_minutes = TimeInt(5).minutes
two_days = TimeInt(2).days
one_week = TimeInt(1).week

# Use .ago() to get past times
last_week = TimeInt(1).week.ago()
```

### Time
Provides comprehensive date/time utilities similar to Rails' Time class.

```python
from RailsDates import Time

# Current time utilities
now = Time.current()
today = Time.today()
current_month = Time.current_month()

# Day boundaries
start_of_day = Time.beginning_of_day()
end_of_day = Time.end_of_day()

# Week boundaries  
start_of_week = Time.beginning_of_week()  # Monday
end_of_week = Time.end_of_week()          # Sunday

# Date navigation
yesterday = Time.yesterday()
tomorrow = Time.tomorrow()
```

## Time Duration Properties

Both `TimeInt` and `TimeDeltaBuilder` support these time units:

- `.minute` / `.minutes` - Time in minutes
- `.hour` / `.hours` - Time in hours  
- `.day` / `.days` - Time in days
- `.week` / `.weeks` - Time in weeks
- `.month` / `.months` - Time in months (30-day approximation)
- `.decade` / `.decades` - Time in decades (10 years)
- `.leapyear` / `.leapyears` - Time in leap years (366 days)

## Date Formatting & Conversion

```python
from RailsDates import Time
from datetime import datetime

dt = datetime.now()

# String formatting
Time.to_s(dt)                    # "2023-12-25 15:30:45"
Time.to_s(dt, "%B %d, %Y")      # "December 25, 2023"
Time.to_fs(dt, "%Y-%m-%d")      # "2023-12-25"

# Conversions
Time.to_i(dt)        # Integer timestamp
Time.to_a(dt)        # Array: [year, month, day, hour, minute, second, weekday]

# RFC3339 format
Time.rfc3339()       # "2023-12-25T15:30:45Z"
```

## Date Information & Calculations

```python
from RailsDates import Time
from datetime import datetime

dt = datetime(2023, 12, 25)

# Day/month names
Time.full_day_of_week(dt)        # "Monday"
Time.abbreviated_day(dt)         # "Mon"
Time.abbreviated_month(dt)       # "Dec"

# Month boundaries
Time.beginning_of_month(dt)      # First day of month
Time.end_of_month(dt)           # Last day of month

# Calendar calculations
Time.days_in_month(2023, 12)    # 31
Time.days_in_year(2023)         # 365
Time.days_in_year(2024)         # 366 (leap year)
```

## Human-Friendly Time Differences

```python
from RailsDates import Time
from datetime import datetime, timedelta

now = datetime.now()
past_time = now - timedelta(hours=2, minutes=30)

# Get human-readable time difference
Time.distance_of_time_in_words(past_time, now)  # "about 2 hours"
Time.distance_of_time_in_words(past_time)       # Uses current time as reference
```

## Timezone Support

```python
from RailsDates import Time

# Get current timezone
Time.zone()              # Local timezone name

# Find timezone by name  
tz = Time.find_zone("UTC")
```

## Quickstart Examples

```python
from RailsDates import TimeInt, Time

# Rails-style time calculations
five_minutes_ago = TimeInt(5).minutes.ago()
deadline = Time.current() + TimeInt(7).days.delta

# Date boundaries
week_start = Time.beginning_of_week()
month_end = Time.end_of_month(Time.today())

# Human-friendly formatting
time_diff = Time.distance_of_time_in_words(five_minutes_ago)
print(f"Created {time_diff} ago")  # "Created about 5 minutes ago"
```

## Dependencies
- Python 3.6+
- datetime (built-in)
- calendar (built-in)
- time (built-in)
