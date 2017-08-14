# Common SwimSuite objects

## Address

The address object is consistently used across the Event File Format.

Field    | Type   | Description
-------- | ------ | -----------------------------------
line1    | string | Address Line 1                      |
line2    | string | Address Line 2 (Optional)           |
county   | string | County/State/Province (Optional)    |
postcode | string | Post Code/Zip Code                  |
country  | string | ISO Alpha-2 Country Code (Optional) |

### Example

```
{
  "line1": "Address Line 1",
  "city": "Exampletown",
  "county": "Exampleshire",
  "postcode": "EX4 1MP",
  "country": "UK"
}
```

## Date/Time

SwimSuite Interchange files have to take into consideration different time zones. To enable this, dates and times are stored in a specific format.

SwimSuite uses a subset of the ISO 8601 standard. This is because dates and/or times are stored separately from each other.

### Date

Dates are stored in the following format, `yyyy-mm-dd`, where:

Field | Description            | Example
----- | ---------------------- | -------
yyyy  | 4 digit year           | 2017
mm    | left zero-padded month | 05
dd    | left zero-padded date  | 01

### Time

Times are stored in the following format, `hh:mm±HH:MM`, where:

Field | Description             | Example
----- | ----------------------- | -------
hh    | Hours (in local time)   | 17
mm    | Minutes (in local time) | 00
HH    | Hour offset from UTC    | 01
MM    | Minute offset from UTC  | 00

### Datetime

Datetime is combination of both [Date](#date) and [Time](#time). It consists of the _date_ element, followed by `T`, and then the _time_ element.

Format: `yyy-mm-ddThh:mm±HH:MM`

### Examples

- 5th March 2017: `2017-03-05`
- April 2nd 2017: `2017-04-02`
- 17:00 in BST (British Summer Time): `17:00+01:00`
- 17:00 in GMT (Greenwich Mean Time): `17:00+00:00`
- 17:00 in PST (Pacific Standard Time): `17:00-08:00`
- 12:00 (GMT) April 2nd 2017: `2017-04-02T12:00+00:00`
- 14:00 (BST) 6th May 2017: `2017-05-06T14:00+01:00`
