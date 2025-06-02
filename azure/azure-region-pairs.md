---
tags: azure, az-900
---

# Azure Region Pairs

It may happen that an event is so big that it impacts a whole [[azure-region]]. In that case, [[azure-availability-zones]] are not enough to prevent data loss and application downtime. Therefore, we need to pair regions so that one acts as a fallback for the other.

Most Azure regions are paired with another region within the same geography (WestUS is paired with EastUS, for example). The minimum distance is 300 miles, so the likelihood of an event that affects both regions is small.

## Advantages of Region Pairs

- If an extensive Azure outage occurs, one region out of every pair is **prioritized** to make sure at least one is restored as quickly as possible for applications hosted in that region pair.
- Planned Azure **updates are rolled out to paired regions one region at a time** to minimize downtime and risk of application outage.
- **Data** continues to reside within the same geography as its pair for **tax- and law-enforcement jurisdiction purposes**.
