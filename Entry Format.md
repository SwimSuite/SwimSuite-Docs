# SwimSuite Entry File Format

This file documents the SwimSuite Entry File Format. SwimSuite Entry File Format is used to interchange data between Swimming Competition Entry Managers and SwimSuite. It describes the files used, the fields and how the file should be organised.

# File

The Entry File Format is encoded using JSON. The nature of the JSON objects is described below.

The Entry File is packaged inside a gzip archive along with an MD5 checksum. The extension of a SwimSuite Entry File is `.ssen`.

The layout of the `.ssen` is as follows:

```
Name_of_Competition.ssen/
├── entries.json
└── entries.json.md5
```

# JSON Structure

## Root

The root of the `entries.json` file is a set of `team` objects (see [Team](#team)).

The root is a container for a set of `team` objects. Each `team` object contains the relevant details of the team and contains all the entry information for that team.

The root contains a set of `team`s to allow for multiple teams to use one file. It also makes downloading the entries from SwimSuite Entry possible in a single download.

### Example

```
[
  {
    /* Team 1... */
  },
  {
    /* Team 2... */
  }
]
```

## Team

Team is a collective grouping for entries.

Field    | Type                                | Description
-------- | ----------------------------------- | -------------------------------------------------------------------------
id       | uuid                                | Unique ID of Team (from SwimSuite Entry)                                  |
name     | string                              | Team Name                                                                 |
abbr     | string                              | Team abbreviation (max 4 chars)                                           |
address  | set of [address](Common.md#address) | Set of Addresses (see [Common](Commond.md)->[Address](Common.md#address)) |
phone    | set of string                       | Set of phone numbers (this may be meet secretary, coach, etc) (Optional)  |
email    | set of string                       | Set of email addresses                                                    |
athletes | set of [athlete](#athlete)          | Set of Athletes (see [Athlete](#Athlete))                                 |

### Example

```
{
  "id": "536e32bc-b5fa-496c-a2ef-c5f7ec99f889",
  "name": "Example Team",
  "abbr": "EXMP",
  "address": [{
    "line1": "Address Line 1",
    "city": "Exampletown",
    "county": "Exampleshire",
    "postcode": "EX4 1MP",
    "country": "UK"
  }],
  "phone": [
    "01234 567890"
  ],
  "email": [
    "entries@exampleclub.org"
  ],
  "athletes": [
    /* athletes */
    ...
  ]
}
```

## Athlete

Field       | Type                         | Description
----------- | ---------------------------- | -----------------------------------------------------------------
id          | uuid                         | Unique ID of Swimmer (from SwimSuite Entry)                       |
surname     | string                       | Surname                                                           |
firstname   | string                       | First Name (birth name)                                           |
middlenames | string                       | Middle names or initials. (Optional)                              |
prefname    | string                       | Preferred firstname (display name)                                |
gender      | string                       | Gender (m=male, f=female)                                         |
email       | string                       | Email Address                                                     |
phone       | string                       | Phone Number                                                      |
dob         | [date](Common.md#date)       | SwimSuite Date (see [Common](Commond.md)->[Date](Common.md#date)) |
swimid      | string                       | Swimmer ID as per official body (e.g.: UK, ASA ID number)         |
address     | [address](Common.md#address) | Address (see [Common](Commond.md)->[Address](Common.md#address))  |
entries     | set of [entry](#entry)       | Set of Entry objects (see [Entry](#entry) )                       |

### Example

```
{
  "id": "a0e5b64b-3807-4f53-8083-fd97fd3753c4",
  "surname": "Bloggs",
  "firstname": "Joseph",
  "middlenames": "Edwin",
  "prefname": "Joe",
  "gender": "m",
  "email": "joebloggs@example.com",
  "dob": "1997-01-01",
  "swimid": "0123456",
  "address": {
    "line1": "Address Line 1",
    "city": "Exampletown",
    "county": "Exampleshire",
    "postcode": "EX4 1MP",
    "country": "UK"
  }
}
```

## Entry

Field  | Type    | Description
------ | ------- | ----------------------------------------------------------------------------------------------------------
id     | uuid    | UUID of Event (see [Event File Format](Event%20File%20Format.md)->[Event](Event%20File%20Format.md#event)) |
number | integer | Event Number (see [Event File Format](Event%20File%20Format.md)->[Event](Event%20File%20Format.md#event))  |
time   | decimal | Entry time in seconds.                                                                                     |

### Examples

```
{
  "id": "4bbfc6c7-891a-4e48-a6d7-47503221395e",
  "number": 101,
  "time": 61.01
}
```

```
{
  "id": "...",
  "number": 103,
  "time": 29.35
}
```
