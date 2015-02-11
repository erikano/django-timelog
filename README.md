# tl (timelog)

Stack-oriented time tracking.

## Description

`tl` is a command-line utility for logging time and generating reports.

`tl` supports a set of seven commands; `tl init`, `tl timepoint`,
`tl pending`, `tl pop-drop`, `tl merge-add`, `tl unlog` and `tl report`.

### Timestamp format

The timestamp format of `tl timepoint` is
`[<YYYY>-<mm>-<dd>T]<HH>:<MM>`. (Similar to, but not quite, ISO 8601.)

### Time zone

Timestamps are not good for much without a time zone.

Time zone is included in the timestamp as provided by your OS.

The time zone recorded by `tl timepoint` will be used when presenting
timepoints (such as by `tl pending` and `tl report`).

## Supported platforms

`tl` is being developed on:

  * FreeBSD/armv6
  * FreeBSD/amd64
  * OpenBSD/i386
  * Mac OS X/powerpc

## Known issues

### Ticketed

* [#4](https://github.com/saas-by-erik/timelog/issues/4):
  The tests only check `tl` execution return values.
  They need to be rewritten so that they additionally test the actual outcome.
* [#5](https://github.com/saas-by-erik/timelog/issues/5):
  Timelogs are tied to the endianness of the host they were generated on.
  Endianness to use when reading and writing timelog files should be chosen so
  that timelogs are "portable" between little-endian and big-endian hosts.

### Not ticketed

These are issues for which no tickets have been made since
it has not yet been determined whether tickets
should be created for these issues.

* Some operating systems, when faced with an invalid `$TZ`, will
  silently ignore it and use UTC.

## Building

```
make test
```
