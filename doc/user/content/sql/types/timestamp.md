---
title: "Timestamp Data Types"
description: "Expresses a date and time"
menu:
  main:
    parent: 'sql-types'
aliases:
    - /sql/types/timestamptz
---

`timestamp` and `timestamp with time zone` data expresses a date and time in
UTC.

## `timestamp` info

Detail | Info
-------|------
**Quick Syntax** | `TIMESTAMP WITH TIME ZONE '2007-02-01 15:04:05+06'`
**Size** | 8 bytes
**Catalog name** | `pg_catalog.timestamp`
**OID** | 1083
**Min value** | 4713 BC
**Max value** | 294276 AD
**Resolution** | 1 microsecond / 14 digits

## `timestamp with time zone` info

Detail | Info
-------|------
**Quick Syntax** | `TIMESTAMPTZ '2007-02-01 15:04:05+06'`
**Aliases** | `timestamp with time zone`
**Size** | 8 bytes
**Catalog name** | `pg_catalog.timestamptz`
**OID** | 1184
**Min value** | 4713 BC
**Max value** | 294276 AD
**Resolution** | 1 microsecond / 14 digits

## Syntax

{{< diagram "type-timestamp.svg" >}}

Field | Use
------|-----
**WITH TIME ZONE** | Apply the _tz&lowbar;offset_ field. If not specified, don't.
**TIMESTAMPTZ** | Apply the _tz&lowbar;offset_ field.
_date&lowbar;str_ | _date&lowbar;str_ | A string representing a date in `Y-M-D`, `Y M-D`, `Y M D` or `YMD` format.
_time&lowbar;str_ | A string representing a time of day in `H:M:S.NS` format.
_tz&lowbar;offset_ | The timezone's distance, in hours, from UTC.

## Details

- `timestamp` and `timestamp with time zone` store data in
  [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).
- The difference between the two types is that `timestamp with time zone` can read or write
  timestamps with the offset specified by the timezone. Importantly,
  `timestamp with time zone` itself doesn't store any timezone data; Materialize simply
  performs the conversion from the time provided and UTC.
- Materialize assumes all clients expect UTC time, and does not currently
  support any other timezones.

### Valid casts

#### From `timestamp`

You can [cast](../../functions/cast) `timestamp` or `timestamp with time zone` to:

- [`date`](../date)
- [`text`](../text)
- `timestamp`
- `timestamp with time zone`

#### To `timestamp`

You can [cast](../../functions/cast) the following types to `timestamp` or
`timestamp with time zone`:

- [`date`](../date)
- [`text`](../text)
- `timestamp`
- `timestamp with time zone`

### Valid operations

`timestamp` and `timestamp with time zone` data (collectively referred to as
`timestamp/tz`) supports the following operations with other types.

Operation | Computes
----------|------------
[`date`](../date) `+` [`interval`](../interval) | [`timestamp/tz`](../timestamp)
[`date`](../date) `-` [`interval`](../interval) | [`timestamp/tz`](../timestamp)
[`date`](../date) `+` [`time`](../time) | [`timestamp/tz`](../timestamp)
[`timestamp/tz`](../timestamp) `+` [`interval`](../interval) | [`timestamp/tz`](../timestamp)
[`timestamp/tz`](../timestamp) `-` [`interval`](../interval) | [`timestamp/tz`](../timestamp)
[`timestamp/tz`](../timestamp) `-` [`timestamp/tz`](../timestamp) | [`interval`](../interval)

## Examples

```sql
SELECT TIMESTAMP '2007-02-01 15:04:05' AS ts_v;
```
```nofmt
        ts_v
---------------------
 2007-02-01 15:04:05
```

<hr/>

```sql
SELECT TIMESTAMPTZ '2007-02-01 15:04:05+06' AS tstz_v;
```
```nofmt
         tstz_v
-------------------------
 2007-02-01 09:04:05 UTC
```
