---
date: '2025-12-12T20:00:00+02:00'
draft: false
title: 'DX Cluster contest filters'
weight: 200
sidebar:
    open: true
tags: ["HAM", "Contesting", "Radio Amateur", "RadioSports", "QSO", "CQWW", "DXCluster", "N1MM+", "N1MM", "DXLog", "RumlogNG", "dxspider", "DX-Spider"]
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

## Filter by zone

To see only spots from spotters in zones 14 and 15 (Western Europe):

```vim
accept/spots by_zone 14,15
accept/rbn by_zone 14,15
```

## Practical examples

### CW contest

Accepts only CW spots and RBNs from zones 14 and 15 but wide enough to also show contest stations outside CW segments:

```vim
clear/spots all
clear/rbn all
reject/spots info ft8,ft4,rtty
accept/spots on contesthf and by_zone 14,15
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

Accepts only 20m CW spots and RBNs from zones 14 and 15:

```vim
clear/spots all
clear/rbn all
reject/spots info ft8,ft4,rtty
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

Accepts only SSB spots from zones 14 and 15

```vim
clear/spots all
clear/rbn all
reject/spots info ft8,ft4,rtty
accept/spots on contesthf/ssb and by_zone 14,15
unset/skimmer
sh/filter
```
