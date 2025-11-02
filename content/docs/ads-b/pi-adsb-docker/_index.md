---
date: '2024-07-01T23:41:49+02:00'
draft: false
title: 'Raspberry Pi ADS-B feeder'
tags: ["ADS-B", "Mode S", "Docker", "Raspberry Pi", "transponder", "surveillance", "ADS-B Exchange", "FlightRadar24", "FlightAware"]
---

### Raspberry Pi ADS-B receiver - feeder using Docker containers

Skip the messy manual installs. With Docker containers, you can run multiple aircraft feeder instances on a single Raspberry Pi using just a USB TV stick or a more professional Airspy receiver. Modular, flexible, and easy to manage.

### Inspired by the Work of Others

We build on the fantastic contributions from:

* Alex Ellis' blog post: [Get eyes in the sky with your Raspberry Pi](https://blog.alexellis.io/track-flights-with-rpi/)
* [Alex Ellis' original github project](https://github.com/alexellis/eyes-in-the-sky)
* [LoungeFlyZ enhanced github project which added the FlightRadar24 feed](https://github.com/LoungeFlyZ/eyes-in-the-sky)
* [Mike, mikenye - Docker images for readsb, tar1090](https://github.com/mikenye)
* [wiedehopf's tar1090](https://github.com/wiedehopf/tar1090)


### Building a Fully Operational multi ADS-B Feeder

Starting from a bare-bones Raspberry Pi, you’ll be guided step by step to a fully functional setup with Docker containers — each capable of feeding a popular ADS-B site. You can easily choose which ADS-B website to feed simply by selecting the containers you launch.

```mermaid
flowchart TB
    AIRCRAFT[Aircraft<br/>ADS-B Transmissions]
    
    DUMP1090[dump1090<br/>Signal Decoder]
    TAR1090[tar1090<br/>Web Interface]
    
    PLANEFINDER[Planefinder]
    FR24[FlightRadar24] 
    ADSBX[ADSB Exchange]
    
    USER[User<br/>Web Browser]
    
    AIRCRAFT -->|RF Signals| DUMP1090
    DUMP1090 -->|Decoded Data| TAR1090
    DUMP1090 -->|Beast/SBS Feed| PLANEFINDER
    DUMP1090 -->|Beast/SBS Feed| FR24
    DUMP1090 -->|Beast/SBS Feed| ADSBX
    TAR1090 -->|Port 8080| USER
    
    %% Simple styling that works
    style AIRCRAFT fill:#1e88e5,color:#fff
    style DUMP1090 fill:#7b1fa2,color:#fff
    style TAR1090 fill:#7b1fa2,color:#fff
    style PLANEFINDER fill:#388e3c,color:#fff
    style FR24 fill:#388e3c,color:#fff
    style ADSBX fill:#388e3c,color:#fff
    style USER fill:#f57c00,color:#fff
```

For complete instructions, check out the [project GitHub page]().

{{< callout >}}
  This project needs an update to the latest version of ads-b tools.
{{< /callout >}}
