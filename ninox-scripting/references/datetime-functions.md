# Date and Time Functions in Ninox

## Overview

Ninox has dedicated types for dates, times, and datetimes. These are NOT strings — they are distinct value types with their own comparison and arithmetic semantics.

**Source**: Ninox Documentation, https://docs.ninox.com/en/script/dates-and-times

---

## Current Date/Time

```ninox
today()   "Returns: date — today's date, no time component"
now()     "Returns: datetime — current date and time"
```

**Example**:
```ninox
let d := today();         "e.g. 2024-03-15"
let dt := now();          "e.g. 2024-03-15 14:30:00"
let yr := year(today());  "e.g. 2024"
```

---

## Creating Dates

### date(year, month, day)
Creates a date value from numeric components.
```ninox
let d := date(2024, 3, 15);   "March 15, 2024"
let d2 := date(2024, 12, 31); "December 31, 2024"
```

**Special behaviors of `date()`**:
```ninox
"Extract date from a datetime/timestamp"
date(now())        "→ today's date (removes time component)"
date(start(Appointment))  "→ date part of appointment start"

"Day=0 gives last day of previous month"
date(2024, 8, 0)   "→ 2024-07-31 (last day of July)"

"Convert Unix milliseconds to date"
date(1617873118285)  "→ date from Unix timestamp"
```

### time(hour, minute, second)
Creates a time value.
```ninox
let t := time(14, 30, 0);  "14:30:00"
let t2 := time(9, 0, 0);   "09:00:00"
```

### datetime(date, time)
Combines a date and time into a datetime.
```ninox
let d := date(2024, 3, 15);
let t := time(14, 30, 0);
let dt := datetime(d, t);  "2024-03-15 14:30:00"
```

---

## Extracting Components

### From Date
```ninox
year(date)     "→ number: e.g. 2024"
month(date)    "→ number: 1-12"
day(date)      "→ number: 1-31"
weekday(date)  "→ number: 0=Monday, 6=Sunday"
week(date)     "→ number: calendar week (1-53)"
quarter(date)  "→ number: 1-4"
```

**⚠️ Important**: `weekday()` returns **0=Monday, 6=Sunday** (NOT 1=Monday, 7=Sunday!)

**Examples**:
```ninox
let d := date(2024, 3, 15);    "Friday March 15, 2024"
year(d)     "→ 2024"
month(d)    "→ 3"
day(d)      "→ 15"
weekday(d)  "→ 4 (Friday — 0=Mon, 1=Tue, 2=Wed, 3=Thu, 4=Fri, 5=Sat, 6=Sun)"
week(d)     "→ 11"
quarter(d)  "→ 1"
```

**Additional date component functions**:
```ninox
weekdayIndex(date)  "⚠️ Not in standard docs — verify before use"
weekdayName(date)   "⚠️ Not in standard docs — verify before use"
yearmonth(date)     "⚠️ Not in standard docs — combined year+month"
yearweek(date)      "⚠️ Not in standard docs — combined year+week"
yearquarter(date)   "⚠️ Not in standard docs — combined year+quarter"
```

### From Datetime
All date extraction functions work on datetime values too:
```ninox
let dt := now();
year(dt)    "→ current year"
month(dt)   "→ current month"
day(dt)     "→ current day"
```

---

## Formatting Dates

### format(value, formatString)
Converts a date/time/datetime to a text representation.

```ninox
format(date, "formatString")
format(datetime, "formatString")
```

**Format String Tokens (Official)**:
| Token | Meaning | Example |
|---|---|---|
| `YYYY` | 4-digit year | 2024 |
| `YY` | 2-digit year | 24 |
| `MMMM` | Full month name | February |
| `MMM` | 3-char month | Feb |
| `MM` | 2-digit month | 03 |
| `M` | Month without leading zero | 3 |
| `Mo` | Month ordinal | 3rd |
| `DD` | 2-digit day | 05 |
| `D` | Day without leading zero | 5 |
| `Do` | Day ordinal | 5th |
| `dddd` | Full weekday name | Friday |
| `ddd` | 3-char weekday | Fri |
| `dd` | 2-char weekday | Fr |
| `d` | Weekday number (0=Sun) | 5 |
| `W` or `w` | Calendar week | 11 |
| `Q` | Quarter | 1 |
| `HH` | 2-digit hour (24h) | 14 |
| `H` | Hour (24h, no leading zero) | 14 |
| `hh` | 2-digit hour (12h) | 02 |
| `h` | Hour (12h, no leading zero) | 2 |
| `a` | am/pm | pm |
| `mm` | 2-digit minutes | 30 |
| `m` | Minutes no leading zero | 30 |
| `ss` | 2-digit seconds | 00 |
| `s` | Seconds no leading zero | 0 |
| `x` | Unix milliseconds | 1617873118285 |
| `X` | Unix seconds | 1617873118 |
| `Z` | UTC offset | -04:00:00 |
| `L` | MM/DD/YYYY | 03/05/2024 |
| `l` | M/D/YYYY | 3/5/2024 |

**With language parameter**:
```ninox
format(myDate, "MMMM YYYY", "de")   "→ 'März 2024' (German)"
format(myDate, "MMMM YYYY", "en")   "→ 'March 2024' (English)"
format(myDate, "dddd", "fr")        "→ 'vendredi' (French)"
```

**Examples**:
```ninox
let d := date(2024, 3, 5);
format(d, "DD.MM.YYYY")    "→ '05.03.2024'"
format(d, "YYYY-MM-DD")    "→ '2024-03-05'"
format(d, "MM/DD/YYYY")    "→ '03/05/2024'"
format(d, "D.M.YY")        "→ '5.3.24'"

let dt := now();
format(dt, "DD.MM.YYYY HH:mm")   "→ '05.03.2024 14:30'"
format(dt, "YYYY-MM-DD HH:mm:ss") "→ '2024-03-05 14:30:00'"
```

**Important**: `format()` always returns **text**, not a date. You cannot compare the result with a date value.

---

## Date Arithmetic

### Adding and Subtracting Days
Ninox dates support arithmetic with numbers (representing days):

```ninox
"⚠️ Not explicitly documented as arithmetic, but works"
let tomorrow := today() + 1;          "Add 1 day"
let yesterday := today() - 1;         "Subtract 1 day"
let nextWeek := today() + 7;          "Add 7 days"
let in30Days := today() + 30;         "Add 30 days"
```

### Date Difference
```ninox
"Difference between dates returns number of days"
let diff := date2 - date1;
"diff is a number of days"

"Example: How many days until deadline?"
let daysLeft := Deadline - today();

"Example: How old is a record in days?"
let age := today() - 'Created Date';
```

### Age Calculation
```ninox
"Calculate age in years"
let birthDate := this.'Date of Birth';
let ageYears := year(today()) - year(birthDate);
"Adjust if birthday hasn't occurred yet this year"
if month(today()) < month(birthDate) or
   (month(today()) = month(birthDate) and day(today()) < day(birthDate)) then
    ageYears := ageYears - 1;
end;
ageYears
```

---

## Date Comparison

### Correct Date Comparison
```ninox
"✅ CORRECT: Compare date values directly"
if Deadline < today() then
    "Overdue!";
end

if 'Start Date' <= today() and 'End Date' >= today() then
    "Currently active";
end
```

### Common Date Comparison Mistakes

```ninox
"❌ WRONG: Comparing date with string"
if Deadline < "2024-03-15" then  "DOES NOT WORK"
    "Error!";
end

"✅ CORRECT: Create a date value to compare"
if Deadline < date(2024, 3, 15) then
    "Correct comparison";
end
```

```ninox
"❌ WRONG: Using format() output for comparison"
let formattedToday := format(today(), "YYYY-MM-DD");
if formattedToday < format(Deadline, "YYYY-MM-DD") then  "RISKY - string comparison!"
    "String comparison, not date comparison";
end

"✅ CORRECT: Compare date values directly"
if today() < Deadline then
    "Safe date comparison";
end
```

```ninox
"❌ WRONG: Comparing datetime with date"
"now() is a datetime, today() is a date — they are different types"
if now() = today() then  "Likely FALSE even on the same day!"
    "This may not work as expected";
end

"✅ CORRECT: Extract date part from datetime for comparison"
"Or compare only dates"
if today() = date(year(now()), month(now()), day(now())) then
    "Same day check";
end
```

---

## Date Filtering in Select

```ninox
"Find all records where deadline is today or in the past"
select Tasks where Deadline <= today()

"Find records created this month"
select Records where year('Created Date') = year(today()) and month('Created Date') = month(today())

"Find records in a specific year"
select Records where year('Created Date') = 2024

"Find records in last 30 days"
select Records where 'Created Date' >= today() - 30
```

---

## Datetime Ranges (Appointment Fields)

Ninox supports datetime range fields (for appointments). These are handled with:

```ninox
"⚠️ Not in official documentation, but works"
let appointment := this.Appointment;  "datetime range field"
let startTime := start(appointment);  "start of range"
let endTime := endof(appointment);    "end of range"
let duration := endTime - startTime;  "duration"
```

See also `references/undocumented-features.md` — Feature #5.

---

## Parsing Dates from Text

Ninox does NOT have a built-in `parseDate()` function.

**Workaround for ISO format (YYYY-MM-DD)**:
```ninox
"Parse a date string like '2024-03-15'"
"⚠️ Workaround - no built-in parseDate"
let dateStr := "2024-03-15";
let yr := number(substr(dateStr, 0, 4));
let mo := number(substr(dateStr, 5, 2));
let dy := number(substr(dateStr, 8, 2));
let parsedDate := date(yr, mo, dy);
```

---

## Null/Empty Date Handling

```ninox
"Check if a date field is empty"
if not DateField then
    "DateField is null/empty";
end

"Or explicitly:"
if DateField = null then
    "DateField is null";
end

"Safe date difference (returns 0 if field empty)"
let diff := if DateField then today() - DateField else 0 end;
```

---

## Age and Working Days

### age(date) — Calculate Age in Full Years
Returns the number of complete years between the given date and today.

```ninox
age(Birthday)   "→ number of full years (e.g. 45)"
age(this.'Date of Birth')  "→ current age"
```

**Note**: This is simpler than the manual age calculation. `age()` handles birthday-hasn't-occurred-yet automatically.

### workdays(date, date) — Working Days Between Dates
Returns the number of working days (Monday–Friday) between two dates.

```ninox
workdays(StartDate, EndDate)   "→ number of working days"
workdays(today(), Deadline)    "→ working days remaining"
```

**Note**: Does not account for public holidays — only excludes weekends.

---

## Practical Examples

### Days Until Event
```ninox
let daysUntil := EventDate - today();
if daysUntil < 0 then
    "Event was " + text(abs(daysUntil)) + " days ago"
else if daysUntil = 0 then
    "Event is today!"
else
    "Event is in " + text(daysUntil) + " days"
end
```

### First Day of Month
```ninox
"⚠️ Workaround for first day of current month"
let firstOfMonth := date(year(today()), month(today()), 1);
```

### Last Day of Month
```ninox
"⚠️ Workaround for last day of current month"
"Get first day of next month, subtract 1"
let nextMonth := if month(today()) = 12 then 1 else month(today()) + 1 end;
let nextYear := if month(today()) = 12 then year(today()) + 1 else year(today()) end;
let lastOfMonth := date(nextYear, nextMonth, 1) - 1;
```

### ISO Week Number
```ninox
"Current calendar week"
let cw := week(today());
"e.g. 'KW 11 / 2024'"
let cwLabel := "KW " + text(cw) + " / " + text(year(today()));
```

---

## Sources

- Official Documentation: https://docs.ninox.com/en/script/dates-and-times
- Community Forum: https://forum.ninox.de
- Undocumented Features: `references/undocumented-features.md`

**Note**: Date arithmetic (adding/subtracting numbers) works in practice but may not be explicitly documented. Mark with ⚠️ when using.
