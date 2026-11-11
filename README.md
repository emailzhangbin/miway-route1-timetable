# MiWay Route 1 — derived timetable

`timetable.json` holds departure/arrival times for **MiWay Route 1 (Dundas)** at four
stops, derived from the City of Mississauga's public GTFS feed
(`https://www.miapp.ca/GTFS/google_transit.zip`).

Served over GitHub Pages so a small iOS app can refresh its schedule without shipping a
new build every time MiWay changes seasons:

```
https://emailzhangbin.github.io/miway-route1-timetable/timetable.json
```

## Format

```jsonc
{
  "feedStart": "20261110",        // first date covered (yyyyMMdd, America/Toronto)
  "feedEnd":   "20270103",        // last date covered
  "directions": {
    "toWork": {
      "board":  "1378",           // GTFS stop_id to board at
      "alight": "0827",           // GTFS stop_id to get off at
      "patterns": [[[485, 497]]], // patterns[i] = one service day: [[departMin, arriveMin], ...]
                                  //   minutes from midnight of the service day; may exceed 1440
      "datePattern": { "20261110": 0 }   // yyyyMMdd -> index into patterns
    },
    "toHome": { "...": "same shape" }
  }
}
```

Identical service days are de-duplicated into `patterns`, so the file stays a few KB.

## Regenerating

See `tools/extract_timetable.py` and `tools/publish_timetable.py` in the app's own
repository. The generator also works around a quirk in the upstream feed: on some
holidays MiWay marks both the weekday service and the holiday service as running
(`exception_type=1`), which would double the day's trips; holiday service replaces
regular service rather than adding to it.

## Data source and attribution

Schedule data © City of Mississauga, used under the terms published with its
[developer download](https://www.mississauga.ca/miway-transit/developer-download/).
This repository holds a derived subset and is not affiliated with or endorsed by
MiWay or the City of Mississauga. Always check posted signage for the authoritative
schedule.
