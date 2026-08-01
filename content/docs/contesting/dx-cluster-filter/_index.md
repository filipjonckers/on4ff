---
date: '2025-12-12T20:00:00+02:00'
draft: false
title: 'DX Cluster contest filters'
weight: 200
sidebar:
    open: true
tags: ["cluster filter", "dx", "HAM", "Contesting", "Radio Amateur", "RadioSports", "QSO", "CQWW", "DXCluster", "N1MM+", "N1MM", "DXLog", "RumlogNG", "dxspider", "DX-Spider"]
---

This page contains a number of useful DX Cluster filters for HAM radio contesting. The filter format is specifically targeted towards DX-Spider clusters, used by our local [DX cluster](telnet://nolcluster.on8ar.eu:7300).

## DX Cluster commands

Show current active filters:

```vim
sh/filter
```

Remove all active filters:

```vim
clear/spots all
clear/rbn all
```

## Filter Line Numbers (Slots)

When writing complex filtering rules in DXSpider, you can assign an explicit line number (or slot position) directly after the command name (e.g., reject/spots 1 ... or accept/spots 2 ...). DXSpider evaluates your active filter rules sequentially based on these line numbers, starting from lowest to highest. If you do not specify a line number when entering a command, DXSpider defaults to line number 1.

*The Filter Flow:* If a spot matches a reject rule on line 1, it is immediately discarded. The cluster engine will never evaluate line 2 for that specific spot.

*The Layered Logic:* This allows you to chain rules together logically. For example, you can drop digital modes on line 1, and then exclusively accept CW or SSB on line 2 from the surviving spots.

*The Risk:* Typing a new filter command without a slot number will overwrite whatever is currently occupying line 1.

*The Fix:* Explicitly naming your lines (1, 2, 3) allows you to stack multiple independent filter strings without accidentally deleting your previous rules.

You can use line numbers to cleanly separate your commands:

```vim
clear/spots all
reject/spots 1 on hf/data or on hf/rtty or info ft8,ft4,ft2
accept/spots 2 on hf/cw or on hf/ssb
```

In this scenario, line 1 acts as a bulletproof shield that instantly drops digital data, while line 2 acts as a gatekeeper that only opens for voice and CW traffic.

To keep this tutorial simple we only use line 1 but don't hesitate to write more complex combinations.

## Using skimmer spots

Use the ```set/skimmer``` and ```unset/skimmer``` commands to enable/disable skimmer spots, optionally for a specific mode:

```vim
set/skimmer cw
set/skimmer rtty
set/skimmer FT8
set/skimmer cw rtty
```

This will show/hide ALL skimmer spots:

```vim
set/skimmer
unset/skimmer
```

## Filter by band

To see only spots on contest bands use ```contesthf``` or a list of bands:

```vim
accept/spots on contesthf
accept/spots on 160m,80m,40m,20m,15m,10m
accept/rbn on contesthf
accept/rbn on 160m 80m 40m 20m 15m 10m
```

To see only spots on specific bands:

```vim
accept/spots on 10m
accept/rbn on 10m
```

## Filter by CQ zone

To see only spots from spotters in CQ zones 14 and 15 (Western Europe):

```vim
accept/spots by_zone 14,15
accept/rbn by_zone 14,15
```

To find your own CQ zone, go to (mapability.com)[https://www.mapability.com/ei8ic/maps/cqzone.php] where EI8IC is doing an amazing job in providing all kind of maps.

## Filter out FT8 and other data modes

By rejecting all data subbands:

```vim
reject/spots on hf/data
```

Or more detailed by rejecting all data subbands and info containing data modes:

```vim
reject/spots on hf/data or on hf/rtty or info ft8,ft4,ft2,rtty
```

## Practical examples

### CW contest

Accepts only CW spots and RBNs from zones 14 and 15 but wide enough to also show contest stations outside CW segments:

```vim
clear/spots all
clear/rbn all
reject/spots on hf/data or on hf/rtty or info ft8,ft4,ft2,rtty
accept/spots on contesthf and by_zone 14,15
accept/rbn on contesthf and by_zone 14,15
set/skimmer cw
sh/filter
```

Or more specifically on the HF CW subbands:

```vim
clear/spots all
clear/rbn all
reject/spots on hf/data or on hf/rtty or info ft8,ft4,ft2,rtty
accept/spots on contesthf/cw and by_zone 14,15
accept/rbn on contesthf and by_zone 14,15
set/skimmer cw
sh/filter
```

Or only use RBN spots:

```vim
clear/spots all
clear/rbn all
reject/spots on all
reject/rbn not on contesthf
set/skimmer cw
sh/filter
```

### CW contest on 20m

Accepts only 20m CW spots and RBNs from CQ zones 14 and 15:

```vim
clear/spots all
clear/rbn all
reject/spots on hf/data or on hf/rtty or info ft8,ft4,ft2,rtty
accept/spots on 20m and by_zone 14,15
accept/rbn on 20m and by_zone 14,15
set/skimmer cw
sh/filter
```

### RTTY contest

Accepts only RTTY spots and RBNs spotted worldwide

```vim
clear/spots all
clear/rbn all
accept/spots on contesthf and info rtty
accept/rbn on contesthf
set/skimmer rtty
sh/filter
```

Or only use RBN spots:

```vim
clear/spots all
clear/rbn all
reject/spots on all
accept/rbn on contesthf
set/skimmer rtty
sh/filter
```

### SSB contest

Accepts only SSB spots from CQ zones 14 and 15:

```vim
clear/spots all
clear/rbn all
reject/spots on hf/data or on hf/rtty or info ft8,ft4,ft2,rtty
accept/spots on contesthf/ssb and by_zone 14,15
unset/skimmer
sh/filter
```

### Mixed CW and SSB contest

Accepts only spots from CQ zones 14 and 15 (replace with your own CQ zone):

```vim
clear/spots all
clear/rbn all
reject/spots on hf/data or on hf/rtty or info ft8,ft4,ft2,rtty
accept/spots on contesthf and by_zone 14,15
accept/rbn on contesthf and by_zone 14,15
set/skimmer cw
sh/filter
```

### DXpedition

Accepts only spots from zones 14 and 15 but no WARC bands for specific DXpeditions

```vim
clear/spots all
clear/rbn all
reject/spots on warc
accept/spots call 3Y0K,TX5EU and by_zone 14,15
reject/rbn on warc
accept/rbn call 3Y0K,TX5EU and by_zone 14,15
set/skimmer
sh/filter
```
