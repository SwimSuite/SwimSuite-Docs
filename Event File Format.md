# SwimSuite Event File Format #
This file documents the SwimSuite Event File Format.
SwimSuite Event File Format is used to interchange data between SwimSuite and Swimming Competition Entry Managers.
It describes the files used, the fields and how the file should be organised.

## File ##
The Event File Format is encoded using JSON.
The nature of the JSON objects is described below.

The Event File is packaged inside a gzip archive along with an MD5 checksum.
The extension of a SwimSuite Event File is `.ssev`.

The layout of the `.ssev` is as follows:

```
Name_of_Competition.ssev/
├── events.json
└── events.json.md5
```

## JSON Structure ##

### Host Definition ###
This structure details the competition host.
| Field | Type | Description |
|---|---|---|
|   |   |   |
|   |   |   |
|   |   |   |
