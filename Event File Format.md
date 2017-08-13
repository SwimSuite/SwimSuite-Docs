# SwimSuite Event File Format

This file documents the SwimSuite Event File Format. SwimSuite Event File Format is used to interchange data between SwimSuite and Swimming Competition Entry Managers. It describes the files used, the fields and how the file should be organised.

# File

The Event File Format is encoded using JSON. The nature of the JSON objects is described below.

The Event File is packaged inside a gzip archive along with an MD5 checksum. The extension of a SwimSuite Event File is `.ssev`.

The layout of the `.ssev` is as follows:

```
Name_of_Competition.ssev/
├── competition.json
└── competition.json.md5
```

# Date/Time

`.ssev` files have to take into consideration different time zones. To enable this, dates and times are stored in a specific format.

SwimSuite uses a subset of the ISO 8601 standard. This is because dates and/or times are stored separately from each other.

## Date

Dates are stored in the following format, `yyyy-mm-dd`, where:

Field | Description            | Example
----- | ---------------------- | -------
yyyy  | 4 digit year           | 2017
mm    | left zero-padded month | 05
dd    | left zero-padded date  | 01

## Time

Times are stored in the following format, `hh:mm±HH:MM`, where:

Field | Description             | Example
----- | ----------------------- | -------
hh    | Hours (in local time)   | 17
mm    | Minutes (in local time) | 00
HH    | Hour offset from UTC    | 01
MM    | Minute offset from UTC  | 00

## Datetime

Datetime is combination of both [Date](#date) and [Time](#time). It consists of the _date_ element, followed by `T`, and then the _time_ element.

Format: `yyy-mm-ddThh:mm±HH:MM`

## Examples

- 5th March 2017: `2017-03-05`
- April 2nd 2017: `2017-04-02`
- 17:00 in BST (British Summer Time): `17:00+01:00`
- 17:00 in GMT (Greenwich Mean Time): `17:00+00:00`
- 17:00 in PST (Pacific Standard Time): `17:00-08:00`
- 12:00 (GMT) April 2nd 2017: `2017-04-02T12:00+00:00`
- 14:00 (BST) 6th May 2017: `2017-05-06T14:00+01:00`

# JSON Structure

## Competition

The root node in `competition.json` is the `competition` node.

Although the `session` node contains a set of sessions, the order of sessions is calculated using the `date` and `start` properties.

Field      | Type                       | Description
---------- | -------------------------- | -----------------------------------------------------------------------------------------
version    | string                     | Version of Event File Format                                                              |
name       | string                     | Name of Competition                                                                       |
licence    | set of string              | Licence IDs/Numbers for the competition (Optional)                                        |
start      | [date](#date)              | Start date of competition (see [Date](#date))                                             |
end        | [date](#date)              | End date of competition (see [Date](#date))                                               |
ageat      | [date](#date)              | Date that ages are calculated from (see [Date](#date)) (Optional)                         |
timeage    | [date](#date)              | Date after which times must have been performed (Optional)                                |
deadline   | [datetime](#datetime)      | Deadline for entries (host may permit entries after deadline) (see [Datetime](#datetime)) |
max_rentry | integer                    | Maximum relay entries per athlete                                                         |
max_ientry | integer                    | Maximum individual entries per athlete                                                    |
max_entry  | integer                    | Total maximum entries per athlete (independent of `max_ientry` & `max_rentry`)            |
software   | string                     | Software that produced this file                                                          |
softver    | string                     | Version of software that produced this file                                               |
export     | [datetime](#datetime)      | Date/Time file was generated (see [Datetime](#datetime))                                  |
host       | [host](#host)              | Competition Host's details (see [Host](#host))                                            |
venue      | [venue](#venue)            | Competition Venue's details (see [Venue](#venue))                                         |
session    | set of [session](#session) | Competition Session details (see [Session](#session))                                     |

### Example

```
{
  "competition": {
    "version": "1.0.0",
    "name": "Example Competition",
    "licence": ["licence1", "licence2"],
    "start": "2017-05-01",
    "end": "2017-05-02",
    "ageat": "2017-05-02",
    "deadline": "2017-03-31T23:59+01:00",
    "max_entry": 3,
    "max_rentry": 1,
    "max_ientry": 4,
    "software": "SwimSuite Race",
    "softver": "0.1.0",
    "export": "2017-02-04T18:35+00:00",
    "entries": {}
    "host" : { ... },
    "venue" : { ... },
    "session" : [ ... ]
  }
}
```

## Address

The address object is consistently used across the Event File Format.

Field    | Type   | Description
-------- | ------ | -----------------------------------
line1    | string | Address Line 1                      |
line2    | string | Address Line 2 (Optional)           |
county   | string | County/State/Province (Optional)    |
postcode | string | Post Code/Zip Code                  |
country  | string | ISO Alpha-2 Country Code (Optional) |

For example:

```
{
  "line1": "Address Line 1",
  "city": "Exampletown",
  "county": "Exampleshire",
  "postcode": "EX4 1MP",
  "country": "UK"
}
```

## Host

This structure details the competition host.

Field   | Type                | Description
------- | ------------------- | ----------------------------------------------
id      | uuid                | Unique ID of host (tied to host's licence key) |
name    | string              | Name of host                                   |
address | [address](#address) | Address of host (see [Address](#address))      |
website | string              | Website of host (optional)                     |
email   | array of string     | Array of Email Addresses                       |
phone   | array of string     | Array of Phone Numbers                         |

Example:

```
{
  "id" : "24a3be9a-3010-4e69-9f26-34df03da297c",
  "name": "Example Swimming Club",
  "address": {
    "line1": "Address Line 1",
    "city": "Exampletown",
    "county": "Exampleshire",
    "postcode": "EX4 1MP",
    "country": "UK"
  },
  "website": "https://www.exampleclub.org.uk"
  "email": [
    "meet@club.org",
    "secretary@club.org"
  ],
  "phone": [
    "01234 567890",
    "07654 321098"
  ]
}
```

## Venue

Every competition needs a venue. A venue comprises of the venue details and the pool information (some venues have multiple pools). Multi-pool events are built in to SwimSuite.

### Pool

Field  | Type    | Description
------ | ------- | ----------------------------------------------------------------------------------------------
id     | uuid    | Unique ID of pool                                                                              |
name   | string  | Name of venue                                                                                  |
lanes  | integer | Number of Lanes                                                                                |
length | string  | Length of pool. Distance as an integer. Units as a character where: "m" = metres, "y" = yards. |

#### Examples

##### 6 Lane 25m

```
{
  "id": "..."
  "name": "Pool",
  "lanes": 6,
  "length": "25m"
}
```

##### 8 Lane 50m

```
{
  "id": "..."
  "name": "Pool",
  "lanes": 8,
  "length": "50m"
}
```

##### 10 Lane 50y

```
{
  "id": "..."
  "name": "Pool",
  "lanes": 10,
  "length": "50y"
}
```

### Venue Details

Field   | Type                 | Description
------- | -------------------- | ------------------------------------------
name    | string               | Name of venue                              |
address | [address](#address)  | Address of venue (see [Address](#address)) |
website | string               | Website of venue (Optional)                |
email   | array of string      | Array of Email Addresses (Optional)        |
phone   | array of string      | Array of Phone Numbers (Optional)          |
pool    | set of [pool](#pool) | Set of pools (see [Pool](#pool))           |

#### Example

```
{
  "name": "Ponds Forge International Sports Centre",
  "address": {
    "line1": "Ponds Forge International Sports Centre",
    "line2": "Sheaf Street",
    "city": "Sheffield",
    "postcode": "S1 2BP",
    "country": "UK"
  },
  "website": "https://siv.org.uk/ponds_forge/",
  "phone": [ "0114 223 3400" ],
  "email": [ "info@ponds-forge.co.uk" ],
  "pool" : [
    {
      "id": "3753cd88-57f2-43bf-895e-2289ae05d2f3",
      "name": "Main Pool",
      "lanes": 10,
      "length": "50m"
    }
  ]
}
```

## Session

A normal competition is broken into sessions and events. The container for events is a session.

Field  | Type                   | Description
------ | ---------------------- | ----------------------------------------------
id     | uuid                   | Unique ID of session                           |
name   | string                 | Name/Number of session                         |
date   | [date](#date)          | Date of session (see [Date](#date))            |
start  | [time](#time)          | Start time of session (see [Time](#time))      |
warmup | [time](#time)          | Warmup time of session (see [Time](#time))     |
signin | [time](#time)          | Sign-In time of session (see [Time](#time))    |
events | set of [event](#event) | Set of Events in session (see [Event](#event)) |

### Example

```
{
  "id": "3a8a57e5-0476-4d3e-bb28-166e65098eb7",
  "name": "1",
  "date": "2017-05-01",
  "start": "17:00+01:00",
  "warmup": "16:00+01:00",
  "signin": "16:15+01:00",
  "events": [
    ...
  ]
}
```

## Event

Field    | Type                       | Description
-------- | -------------------------- | ------------------------------------------------------------------
id       | uuid                       | Unique ID of event                                                 |
number   | integer                    | Number of event                                                    |
title    | string                     | Title of Event                                                     |
gender   | [gender](#gender)          | Gender of event (see [Gender](#gender))                            |
distance | integer                    | Distance of event (unit is determined by the [pool](#pool) object) |
stroke   | [stroke](#stroke)          | Stroke of event (see [Stroke](#stroke))                            |
round    | [round](#round)            | Round of event (see [Round](#round))                               |
price    | decimal                    | Price of event                                                     |
agegroup | set [agegroup](#age-group) | Set of Age Groups for event (see [Age Group](#age-group))          |

### Mappings

The following tables detail the mappings between the codes used in the Event File Format and the real-world meaning

#### Gender

Code | Gender
---- | ------
m    | Male   |
f    | Female |
b    | Boys   |
g    | Girls  |
n    | Mens   |
w    | Womens |
x    | Mixed  |

#### Stroke

Code | Stroke
---- | -----------------
fc   | Freestyle         |
bc   | Backstroke        |
br   | Breastroke        |
bf   | Butterfly         |
im   | Individual Medley |
fr   | Freestyle Relay   |
mr   | Medley Relay      |

#### Round

Code | Stroke
---- | --------------
tf   | Timed Finals   |
h    | Heats          |
q    | Quarter Finals |
s    | Semi Finals    |
f    | Finals         |

### Examples

#### Event 101: Boys 11/O 100m Freestyle (Timed Finals)

```
{
  "id": "...",
  "number": 101,
  "title": "Boys 11/O 100m Freestyle",
  "gender": "b",
  "distance": 100,
  "stroke": "fc",
  "round": "tf",
  "price": "5.50",
  "agegroup": [
    ...
  ]
}
```

## Age Group

When SwimSuite calculates what age group an entry fits into, the `lage` and `uage` fields are used. Where the athletes age is denoted as `age`, SwimSuite uses the following formula to determin whether an athlete is to be included in an age group.

```
in_agegroup = (lage <= age) && (age < uage)
```

A similar formula is used to calculate whether an athlete qualifies for the age group in the event they wish to enter. Where the atheletes entry time is denoted as `time`.

```
can_enter = (lqt <= time) && (time < uqt)
```

Obviously if the upper or lower bound is not included (the field is optional), then SwimSuite will not compare to the upper or lower bounds.

For example, an "Open" age group would have a `lage` defined, but no `uage`. This means the upper bound of this age group is open.

Field | Type    | Description
----- | ------- | --------------------------------------------------
id    | uuid    | Unique ID of age group                             |
name  | string  | Name of age group (e.g.: Open, Senior, U/11, 11/O) |
lage  | integer | Lower Age (no younger than) (Optional)             |
uage  | integer | Upper Age (no older than) (Optional)               |
lqt   | decimal | Lower Qualifying Time (no slower than) (Optional)  |
uqt   | decimal | Upper Qualifying Time (no faster than) (Optional)  |

### Examples

#### Open Age Group (16/O)

```
{
  "id": "...",
  "name": "Open",
  "lage": 16
}
```

#### 11 and Under Age Group (11/U)

```
{
  "id": "...",
  "name": "11/U",
  "uage": 12
}
```

Note that the `uage` is 12 because this group is 11 and under. If `uage` was set to 11, SwimSuite would only accept athletes aged 10 or younger.
