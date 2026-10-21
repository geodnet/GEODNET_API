# Knowledge Center

## Table of Contents


- [Knowledge Center](#knowledge-center)
- 1 Foundations
  - [Overview](#section-1)
  - [1.1 What is GNSS?](#1-1-what-is-gnss)
  - [1.2 Why Standard GPS Isn't Accurate Enough](#1-2-why-standard-gps-isnt-accurate-enough)
  - [1.3 Frequencies, Bands, and Why Multi-Band Matters](#1-3-frequencies-bands-and-why-multi-band-matters)
  - [1.4 What "Centimeter-Level Accuracy" Actually Means](#1-4-what-centimeter-level-accuracy-actually-means)
  - [1.5 SBAS — The Free Sub-Meter Layer](#1-5-sbas-the-free-sub-meter-layer)

- 2 How RTK Works
  - [Overview](#section-2)
  - [2.1 What Is RTK?](#2-1-what-is-rtk)
  - [2.2 Base, Rover, and Correction Stream](#2-2-the-three-actors-base-rover-and-the-correction-stream)
  - [2.3 Carrier-Phase, Fix vs. Float](#2-3-carrier-phase-measurement-fix-vs-float)
  - [2.4 Initialization Time and Cycle Slips](#2-4-initialization-time-and-cycle-slips)
  - [2.5 Epoch Rate](#2-5-epoch-rate-how-often-does-your-receiver-output-a-position)
  - [2.6 RTK vs. PPK vs. PPP](#2-6-rtk-vs-ppk-vs-ppp)

- 3 How RTK Is Delivered
  - [Overview](#section-3)
  - [3.1 What Is NTRIP?](#3-1-what-is-ntrip)
  - [3.2 What Is RTCM?](#3-2-what-is-rtcm)
  - [3.3 Caster, Mountpoint, Client](#3-3-the-ntrip-architecture-caster-mountpoint-client)
  - [3.4 GEODNET Connection Settings](#3-4-what-goes-in-each-field-geodnet-connection-settings)
  - [3.5 What Is an NTRIP Account?](#3-5-what-is-an-ntrip-account)
  - [3.6 Choosing a Mountpoint](#3-6-choosing-a-mountpoint-coordinate-reference-frames)
  - [3.7 NMEA GGA](#3-7-nmea-gga-why-your-device-has-to-talk-back)
  - [3.8 Age of Corrections](#3-8-age-of-corrections)

- 4 Network Architecture
  - [Overview](#section-4)
  - [4.1 Single-Base RTK vs. Network RTK](#4-1-single-base-rtk-vs-network-rtk)
  - [4.2 Nearest-Base RTK vs. VRS vs. MAC](#4-2-nearest-base-rtk-vs-vrs-vs-mac)
  - [4.3 Baseline Length and Density](#4-3-baseline-length-and-why-density-matters)
  - [4.4 What Is a CORS Network?](#4-4-what-is-a-cors-network)
  - [4.5 Network Quality and Validation](#4-5-network-quality-and-validation)

- 5 Practical Use
  - [Overview](#section-5)
  - [5.1 What Kind of Receiver Do I Need?](#5-1-what-kind-of-receiver-do-i-need)
  - [5.2 Setting Up Your First Connection](#5-2-setting-up-your-first-connection)
  - [5.3 Reading Your Status](#5-3-reading-your-status)
  - [5.4 Common Troubleshooting](#5-4-common-troubleshooting)
  - [5.5 Cellular and Internet Requirements](#5-5-cellular-and-internet-requirements)
  - [5.6 Surveying and Measurement Concepts](#5-6-surveying-and-measurement-concepts)
  - [5.7 Applications: Where RTK Is Used](#5-7-applications-where-rtk-is-used)

- 6 Comparing Providers
  - [Overview](#section-6)
  - [6.1 Categories of RTK Service](#6-1-the-categories-of-rtk-service)
  - [6.2 What Differentiates RTK Services](#6-2-what-differentiates-rtk-services)

- 7 Regulatory & Legal
  - [Overview](#section-7)
  - [7.1 Surveying and Land Records](#7-1-surveying-and-land-records)
  - [7.2 Aviation Use](#7-2-aviation-use)
  - [7.3 Frequency Use and Radio Licensing](#7-3-frequency-use-and-radio-licensing)
  - [7.4 Privacy and Data Protection](#7-4-privacy-and-data-protection)
  - [7.5 Standards Bodies](#7-5-standards-bodies-and-where-to-read-more)

- 8 Reference & Glossary
  - [Overview](#section-8)
  - [8.1 GNSS Constellations and Frequencies](#8-1-gnss-constellations-and-frequencies)
  - [8.2 RTCM 3 Message Types](#8-2-rtcm-3-message-types-common-reference)
  - [8.3 RTK Status / Quality Codes](#8-3-rtk-status-quality-codes)
  - [8.4 Coordinate Reference Systems](#8-4-common-coordinate-reference-systems)
  - [8.5 Glossary](#8-5-glossary)


- [RTK Service FAQ](#rtk-service-faq)


---


<a id="knowledge-center"></a>

## Knowledge Center

### Table of Contents

1. Foundations: GNSS, Standard GPS, and the Path to Centimeters
2. How RTK Works
3. How RTK Is Delivered: NTRIP, RTCM, and the Plumbing
4. Network Architecture: How RTK Networks Are Built
5. Practical Use: Hardware, Setup, Surveying Concepts, and Applications
6. Comparing RTK Correction Service Providers
7. Regulatory, Legal, and Standards Considerations
8. Reference Tables and Glossary




---

<a id="section-1"></a>

## Section 1: Foundations

### Section 1: Foundations

Before we get to RTK, it helps to know what we are correcting and why standard GPS is not enough on its own. This section covers the basics: how satellite positioning works, why it has errors, and what centimeter accuracy actually means.




---

<a id="1-1-what-is-gnss"></a>

## 1.1 What is GNSS?

### 1.1 What is GNSS?

**GNSS** stands for **Global Navigation Satellite System**. It is a general term used to describe any satellite constellation that provides positioning, navigation, and timing (PNT) services on a global or regional scale. GNSS allows a receiver on the ground to determine its location (latitude, longitude, altitude), speed, and precise time by processing signals from multiple satellites.


The most well-known GNSS systems include:


- **GPS** (Global Positioning System, United States) — 31 active satellites, the original system, fully operational since 1995
- **GLONASS** (Russia) — 24 active satellites, restored to full coverage in 2011
- **Galileo** (European Union) — 28 satellites, declared fully operational in 2024
- **BeiDou** (China) — 35+ satellites, the most recent of the global systems
- **QZSS** (Japan, regional) — focused on the Asia-Pacific
- **NavIC** (India, regional) — focused on the Indian subcontinent


Together, these systems are widely used in navigation, surveying, aviation, shipping, agriculture, autonomous driving, and many scientific fields.


> [!WORTH KNOWING: HOW GNSS SATELLITES ACTUALLY WORK]
> All GNSS satellites operate in **Medium Earth Orbit (MEO)** at roughly 20,000-23,000 km altitude. Each satellite carries multiple atomic clocks and continuously broadcasts its precise time and orbit position. A receiver on the ground works out where it is by measuring how long the signals took to arrive from at least four satellites. With four time-of-flight measurements and four known satellite positions, geometry yields a unique 3D location plus a clock correction for the receiver.


> [!GEODNET NOTE: FULL-CONSTELLATION REFERENCE STATIONS]
> Every GEODNET reference station is a **full-constellation** receiver, meaning it tracks GPS, GLONASS, Galileo, and BeiDou simultaneously. More satellites in view means faster fixes and better reliability in tough environments like urban canyons or under tree cover.




---

<a id="1-2-why-standard-gps-isnt-accurate-enough"></a>

## 1.2 Why Standard GPS Is Not Accurate Enough

### 1.2 Why Standard GPS Is Not Accurate Enough

A standalone GNSS receiver is typically accurate to within **1 to 5 meters** under good conditions, and worse in cities or under tree cover. That's fine for turn-by-turn driving directions, but it's nowhere near precise enough for surveying a property line, guiding a tractor down a row of crops, or landing a drone on a specific spot. The reason isn't that GPS is "broken." It's that the satellite signals have to travel roughly **20,000 kilometers** through space and atmosphere before reaching your receiver, and a lot can happen along the way:



| Error Source | What Happens |
| :--- | :--- |
| **Ionospheric delay** | The ionosphere (60-1,000 km altitude) bends and slows GNSS signals. The amount changes with solar activity, time of day, and geographic location. |
| **Tropospheric delay** | Water vapor and air pressure in the lower atmosphere also slow signals down, varying with weather. |
| **Satellite clock errors** | Tiny timing drifts in the satellite's atomic clock translate directly into position errors (light travels ~30 cm per nanosecond). |
| **Satellite orbit errors** | The broadcast position of each satellite is a prediction; the real orbit can deviate slightly. |
| **Multipath** | Signals bounce off buildings, vehicles, or terrain before reaching the receiver, arriving slightly late. |
| **Receiver noise** | All electronic receivers introduce a small amount of measurement noise. |



Together, these errors stack up to several meters of uncertainty. **The job of any RTK service including GEODNET is to measure these errors at a known location and pass the corrections to your device, so they can be cancelled out in real time.**




---

<a id="1-3-frequencies-bands-and-why-multi-band-matters"></a>

## 1.3 Frequencies, Bands, and Why Multi-Band Matters

### 1.3 Frequencies, Bands, and Why Multi-Band Matters

GNSS satellites broadcast on multiple frequencies, called **bands**. The most common are:


- **L1** (~1575 MHz) — the original GPS frequency, available on every receiver including phones
- **L2** (~1227 MHz) — a second frequency primarily used by professional receivers
- **L5** (~1176 MHz) — a newer, more robust frequency now broadcast by GPS, Galileo, BeiDou, and others


You'll see receivers described as **single-band (L1 only)**, **dual-band (L1 + L2)**, or **triple-band (L1 + L2 + L5)**.


**Why this matters:** RTK relies on a technique called carrier-phase ambiguity resolution (more on this in Section 2.3). On a single-band receiver, this process is fragile — it can take minutes to lock in, and even small disturbances can lose the fix. With multiple bands, the receiver has independent measurements from the same satellite, which dramatically speeds up the fix and makes it far more robust to interference, atmospheric noise, and brief signal blockages.


In practical terms:


- **Single-band RTK** can technically work in ideal conditions but is unreliable in the field.
- **Dual-band RTK** is the working standard for most prosumer and professional use.
- **Triple-band RTK** is the most resilient — fastest fix times, best performance under tree cover or near buildings.


> [!WORTH KNOWING: WHY L5 IS A BIG DEAL]
> L5 is a newer, higher-power signal designed from the ground up for safety-of-life applications. It uses a wider bandwidth than L1, which makes it more resistant to interference, jamming, and multipath. It's also broadcast by GPS, Galileo (as E5a), BeiDou (B2a), and QZSS — meaning a triple-band receiver can track an L5-equivalent signal from dozens of satellites simultaneously. As more L5-capable satellites have been launched throughout the 2020s, triple-band receivers have meaningfully outperformed dual-band in difficult environments, and the gap is still widening.


> [!GEODNET NOTE: UNIFORM TRIPLE-BAND HARDWARE EVERYWHERE]
> Every station in the GEODNET network is a **triple-frequency, full-constellation receiver**. This is uniform across the entire network — there are no "lite" stations with weaker hardware. Whatever you experience in California, you'll experience in Spain, Australia, or Japan.




---

<a id="1-4-what-centimeter-level-accuracy-actually-means"></a>

## 1.4 What "Centimeter-Level Accuracy" Actually Means

### 1.4 What "Centimeter-Level Accuracy" Actually Means

Accuracy numbers can feel abstract. Here is what each level looks like in practice:



| Accuracy | What you can do with it |
| :--- | :--- |
| ~3 meters (standard GPS) | Navigate to an address. Find a restaurant. Track a delivery truck on a city map. |
| ~30 cm (DGPS / SBAS) | Stay roughly in a lane. Coarse field mapping. |
| ~10 cm (good RTK conditions, far from base) | Lane-level vehicle positioning. Rough construction layout. |
| ~1-2 cm (RTK Fix, GEODNET typical) | Survey a property boundary. Auto-steer a tractor row-by-row. Stake out construction points. Photogrammetry control points. Robotic mowing without missing strips or overlapping. |
| Sub-centimeter (PPK with long observation, geodetic equipment) | Monitor structural deformation. Earthquake research. Tectonic plate movement. |



**The jump from "meters" to "centimeters" is roughly a 100x improvement in precision — and that's the threshold at which entirely new applications become possible.** A robotic mower with 3-meter GPS would mow your neighbor's yard. With 2-centimeter RTK, it can follow the same line every week without straying.


#### Horizontal vs. vertical accuracy

One detail surprises many new users: **RTK vertical accuracy is usually about 1.5–2x worse than horizontal accuracy**. So a system delivering 1 cm horizontal might deliver 2–3 cm vertical.


This isn't a flaw — it's geometry. Satellites can be all around you horizontally, giving strong constraint on east/west and north/south. But all the satellites are above you (you can't see ones below the horizon), so the geometry for solving height is weaker. Add ionospheric and tropospheric error — which mostly affects the vertical component because the signal path through the atmosphere is shorter at high elevation angles and longer at low ones — and you get the typical 1.5–2x penalty.


If your application is height-sensitive (machine control, drainage, structural monitoring), plan for this. Long static observations and PPK can do better than real-time RTK.




---

<a id="1-5-sbas-the-free-sub-meter-layer"></a>

## 1.5 SBAS — The Free Sub-Meter Layer

### 1.5 SBAS — The Free Sub-Meter Layer

Between standard GPS (meters) and RTK (centimeters) there is an intermediate tier of accuracy enabled by **Satellite-Based Augmentation Systems (SBAS)**. These are free, public services that broadcast wide-area corrections via geostationary satellites. The major SBAS systems are:


- **WAAS** (Wide Area Augmentation System) — North America, operated by the FAA
- **EGNOS** (European Geostationary Navigation Overlay Service) — Europe
- **MSAS** (Multi-functional Satellite Augmentation System) — Japan
- **GAGAN** — India
- **SDCM** — Russia


SBAS gets you to roughly **0.5–1 meter accuracy**, uses no additional infrastructure on your end (most consumer GPS chips support it automatically), and was designed primarily for aviation. It's a useful baseline to know about — it's what your phone uses when it shows "GPS-quality" position — but it isn't a substitute for RTK when you need centimeter precision.




---

<a id="section-2"></a>

## Section 2: How RTK Works

### Section 2: How RTK Works

This section gets into the mechanics. We'll cover what the base, rover, and correction stream actually do; the difference between code and carrier phase; what "Fix" and "Float" really mean; and the small but important details (cycle slips, initialization time, epoch rate) that affect day-to-day RTK performance.




---

<a id="2-1-what-is-rtk"></a>

## 2.1 What Is RTK?

### 2.1 What Is RTK?

**RTK** stands for **Real-Time Kinematic** positioning.


The core idea is simple: if you have one GNSS receiver at a known, surveyed location (a **base station**) and another receiver whose location you want to determine (a **rover**), then both receivers are seeing roughly the same atmospheric and satellite errors at the same time. By comparing the two, you can cancel out almost all of the shared error and extract a very precise relative position between them.


Because the base's location is known to high precision, that relative position gives you the rover's absolute location — accurate to about 1-2 centimeters.


The "Real-Time" in RTK means the corrections are sent to the rover continuously, usually multiple times per second, so the rover can produce a high-precision position while it's moving. This is the key difference between RTK and post-processing methods (covered in Section 2.6).




---

<a id="2-2-the-three-actors-base-rover-and-the-correction-stream"></a>

## 2.2 The Three Actors: Base, Rover, and the Correction Stream

### 2.2 The Three Actors: Base, Rover, and the Correction Stream

Every RTK setup has the same three parts:


- **Base station** — A GNSS receiver permanently installed at a precisely surveyed location. It tracks the same satellites your rover sees and calculates the difference between what the satellites say its position should be and what its position actually is. That difference is the correction data.
- **Rover** — Your mobile GNSS receiver — handheld, on a drone, on a tractor, on a robot. It receives the correction data and applies it to its own measurements.
- **Correction stream** — The data link between them. Historically this was a UHF radio. Today, it's almost always the internet, using a protocol called **NTRIP** (Section 3.1).


> [!GEODNET NOTE: YOU ONLY HANDLE THE ROVER SIDE]
> With GEODNET, you only ever interact with the rover side. The base stations and the correction stream are GEODNET's responsibility — there are roughly 20,000+ stations across 150+ countries, monitored 24/7 and uniformly equipped. You point your rover at our server, log in, and start receiving corrections. No base station to install, power, or babysit.




---

<a id="2-3-carrier-phase-measurement-fix-vs-float"></a>

## 2.3 Carrier-Phase Measurement, "Fix" vs. "Float"

### 2.3 Carrier-Phase Measurement, "Fix" vs. "Float"

Standard GPS uses what's called the **code phase** of the signal — the relatively coarse data pattern the satellite is transmitting. This is good for meter-level accuracy.


RTK uses the **carrier phase** — the actual radio wave underneath the data, with a wavelength of about 19 centimeters on L1. By measuring fractions of that wavelength, the receiver can theoretically resolve position to millimeters.


The catch is something called **carrier-phase ambiguity**. The receiver knows where it is within a wavelength, but not which wavelength out of millions it's on. Resolving that ambiguity — figuring out the exact integer number of wavelengths between satellite and receiver — is the hard part of RTK.


You'll see your device report one of these states:



| Status | What It Means |
| :--- | :--- |
| Single / No Fix | Standard GPS, no corrections applied. ~1-5 m accuracy. |
| DGPS / Differential | Code-based corrections only. ~30 cm - 1 m accuracy. |
| RTK Float | The receiver is using carrier-phase corrections but hasn't yet resolved the ambiguity. ~10–50 cm accuracy. |
| RTK Fix | Full carrier-phase ambiguity resolved. ~1-2 cm accuracy. **This is the goal.** |



A healthy RTK workflow usually moves through Single → Float → Fix within seconds (with multi-band hardware) to a couple of minutes (with single-band). Once you have a Fix, you should hold it as long as you keep clear sky view and a stable correction stream.


> [!WORTH KNOWING: ]
> How does the receiver actually solve the ambiguity? The math behind RTK has been a research field for decades. The most widely used algorithm is called LAMBDA — the Least-squares AMBiguity Decorrelation Adjustment, developed at TU Delft in the 1990s. Without going into the linear algebra, the idea is: the receiver has many possible integer combinations that could explain the carrier-phase observations. LAMBDA efficiently searches through those combinations to find the one that's mathematically most consistent across all satellites and all observation epochs. Once a single combination wins by a clear margin, the receiver declares an RTK Fix. If no single combination is clearly best, the receiver stays in Float and keeps gathering data.




---

<a id="2-4-initialization-time-and-cycle-slips"></a>

## 2.4 Initialization Time and Cycle Slips

### 2.4 Initialization Time and Cycle Slips

Two terms you'll encounter that affect daily RTK use:


**Initialization time (Time-To-Fix, TTF)** is how long it takes from a cold start to your first RTK Fix. Several factors determine this:


- **Hardware band count** — Triple-band typically initializes in 5–15 seconds; dual-band in 15–60 seconds; single-band can take minutes or never reliably fix at all.
- **Sky visibility** — More visible satellites (above 30° elevation) means faster ambiguity resolution.
- **Baseline length** — Shorter baseline (rover close to base) means faster fix.
- **Ionospheric activity** — Disturbed conditions slow initialization.
- **Multipath environment** — A clean, open site initializes much faster than near buildings or under trees.


**Cycle slips** are momentary losses of carrier-phase tracking on a satellite — caused by the rover passing under a tree, behind a vehicle, or through a brief signal blockage. When a slip happens, the receiver loses count of wavelengths on that satellite and has to re-resolve the ambiguity for it. With dual or triple-band hardware, this usually happens silently in under a second and you never notice. With single-band hardware, a cycle slip can push you back to Float and force a full re-initialization.




---

<a id="2-5-epoch-rate-how-often-does-your-receiver-output-a-position"></a>

## 2.5 Epoch Rate — How Often Does Your Receiver Output a Position?

### 2.5 Epoch Rate — How Often Does Your Receiver Output a Position?

The **epoch rate** (or update rate) is how many position fixes per second your receiver outputs, expressed in Hz. Common rates and where they're used:



| Rate | Use Case |
| :--- | :--- |
| 1 Hz | Standard surveying, GIS data collection, asset mapping. Plenty for any application where the rover is stationary or moving slowly. |
| 5 Hz | Drones, robots, machine control. Smooth tracking at typical operational speeds. |
| 10 Hz | Faster vehicles, agricultural auto-steer at high ground speeds, dynamic monitoring. |
| 20 Hz | High-speed dynamics — motorsport, dynamic structural testing, aerospace applications. |



Higher rates aren't automatically better. Each fix is computed from the satellite observations available at that moment, so a 20 Hz output isn't 20 independent measurements per second — it's the same underlying data sampled more frequently. For most users, **1 Hz or 5 Hz is the right choice**. The correction stream from GEODNET is delivered at 1 Hz; receivers running faster than that interpolate between corrections, which is normal and expected.




---

<a id="2-6-rtk-vs-ppk-vs-ppp"></a>

## 2.6 RTK vs. PPK vs. PPP

### 2.6 RTK vs. PPK vs. PPP

You'll occasionally see two related terms. Knowing the difference saves confusion:


- **RTK (Real-Time Kinematic)** — Corrections are applied as you work. You see the precise position immediately. Requires a live data link to the network.
- **PPK (Post-Processed Kinematic)** — Same underlying math, but corrections are applied after the fact, in software, once you're back at the office. Used when a real-time link isn't available (e.g., remote drone flights). The rover and base both log raw data; you combine them later.
- **PPP (Precise Point Positioning)** — A different technique that uses ultra-precise satellite orbit and clock data (computed globally) to achieve high accuracy with a single receiver, no nearby base needed. PPP is slower to converge (often 15–30 minutes) and typically less accurate than real-time RTK, but works anywhere on Earth.


> [!GEODNET NOTE: PPP AS INTERNAL QUALITY CONTROL]
> GEODNET's customer-facing service is RTK. PPP is used internally as a quality-control tool to validate every reference station's position. When a new station comes online, GEODNET uploads its raw observations to NRCan's CSRS-PPP service after 3 hours of continuous data; the station is only added to the active RTK service list once the PPP solution returns a 3D sigma below 5 cm. This process repeats periodically, so any station whose position drifts is automatically re-evaluated.




---

<a id="section-3"></a>

## Section 3: How RTK Is Delivered

### Section 3: How RTK Is Delivered

This is the section most people get stuck on, because it has the most acronyms. The good news: once you understand the four players (caster, mountpoint, RTCM, NMEA GGA), you understand 95% of NTRIP setup.




---

<a id="3-1-what-is-ntrip"></a>

## 3.1 What Is NTRIP?

### 3.1 What Is NTRIP?

**NTRIP** stands for **Networked Transport of RTCM via Internet Protocol**.


In plain English: NTRIP is the standard way that RTK correction data gets delivered to your device over the internet. It was created by the German Federal Agency for Cartography and Geodesy in 2004, and it's now the universal protocol used by virtually every commercial RTK service in the world.


A useful analogy: **NTRIP is like a streaming service.** Just as Netflix delivers video over the internet using HTTP, NTRIP delivers GNSS corrections over the internet using a similar protocol. The corrections themselves are the "content" — and that content is formatted in a standard called **RTCM** (next section).


> [!WORTH KNOWING:]
> NTRIP v1 vs. NTRIP v2 There are two versions of NTRIP in active use. NTRIP v1 (2004) is a simple HTTP-style protocol that works almost everywhere but doesn't fully follow modern HTTP standards, which can confuse some firewalls and proxies. NTRIP v2 (2007) updates it to be fully HTTP-compliant, supports chunked transfer encoding, and adds optional features like authentication via standard HTTP auth headers. Most modern receivers and casters support both, and most clients negotiate automatically. If you have a choice in your client settings, NTRIP v2 is the cleaner default; fall back to v1 only if you have connection issues.




---

<a id="3-2-what-is-rtcm"></a>

## 3.2 What Is RTCM?

### 3.2 What Is RTCM?

**RTCM** stands for **Radio Technical Commission for Maritime Services** — the standards body that defined the data format. The format is simply called **RTCM**, and the current version most networks use is **RTCM 3.2** (or the newer **RTCM 3.3**).


RTCM is the actual data your rover consumes — the messages that say "satellite #12 is off by this much, satellite #15 is off by this much," etc. NTRIP is the pipe that delivers the messages. The two are often spoken about together, but they're separate things:


- **RTCM** = the format of the corrections
- **NTRIP** = the protocol that streams them over the internet


> [!WORTH KNOWING: ]
> RTCM message types you'll see in source tables RTCM 3 organizes its data into numbered message types. You don't usually need to think about these as a user, but they show up in source tables and connection diagnostics. The main families: Station info: MT1005, MT1006, MT1033 — describe the base station's antenna and position. Multi-Signal Messages (MSM): MT1077 (GPS), MT1087 (GLONASS), MT1097 (Galileo), MT1127 (BeiDou). These are the modern "all-in-one" observation messages that include carrier phase, code, Doppler, and signal strength. MSM4 contains the standard set; MSM7 adds full resolution and is what GEODNET delivers. Network corrections: MT1030, MT1031 are residual messages used in network RTK. MT1015–1017 are auxiliary messages for VRS and similar services. If your client's diagnostics show MT1077, 1087, 1097, 1127, plus MT1005 or MT1006 — you're getting a complete modern correction stream.


> [!GEODNET NOTE: STANDARD RTCM 3.2 — UNIVERSAL COMPATIBILITY]
> GEODNET delivers corrections in RTCM 3.2 format over standard NTRIP. This means GEODNET works with essentially any RTK-capable receiver on the market — there's no proprietary format you need to support.




---

<a id="3-3-the-ntrip-architecture-caster-mountpoint-client"></a>

## 3.3 The NTRIP Architecture: Caster, Mountpoint, Client

### 3.3 The NTRIP Architecture: Caster, Mountpoint, Client

NTRIP has three components. You'll see all three on your device's setup screen:


- **Caster** — The central server that distributes correction streams. You connect to the caster.
- **Mountpoint** — A named correction stream living on the caster. One caster can host many mountpoints (e.g., one per region, one per coordinate system, one per service tier). When you connect, you have to tell the caster which mountpoint you want.
- **Client** — Your rover (or the software running on it). You are the client.


Think of it like tuning into a radio station: the caster is the broadcaster, the mountpoint is the specific station/channel, and your rover is the radio.


#### The source table

When your client first connects to a caster, it can request a **source table** — a directory listing all the mountpoints that caster offers. The **source table** is plain text and contains entries like:


STR;AUTO;GEODNET;RTCM 3.2;1004(1),1005(5),1019(5)...;2;GPS+GLO+GAL+BDS;NETWORK;USA;...


Each field describes the stream: mountpoint name, identifier, data format, message types broadcast, NMEA GGA requirement, supported constellations, network type, country, and more. Most NTRIP clients parse the source table for you and present a friendly mountpoint picker, but if you're troubleshooting at a low level, the source table is where you'll find what's actually on offer.




---

<a id="3-4-what-goes-in-each-field-geodnet-connection-settings"></a>

## 3.4 What Goes In Each Field — GEODNET Connection Settings

### 3.4 What Goes In Each Field — GEODNET Connection Settings

Here is exactly what you will enter on your device. GEODNET operates three regional servers; use the one closest to where you are working for the lowest latency.



| Field | USA | Europe | Australia |
| :--- | :--- | :--- | :--- |
| **Caster / Server / Host** | rtk.geodnet.com | eu.geodnet.com | aus.geodnet.com |
| **IP (if numeric only)** | 13.56.117.10 | 3.73.41.96 | 54.206.56.130 |
| **Port** | 2101 | 2101 | 2101 |
| **Mountpoint** | AUTO (recommended) | AUTO | AUTO |
| **Username** | Your GEODNET account username | — | — |
| **Password** | Your GEODNET account password | — | — |





---

<a id="3-5-what-is-an-ntrip-account"></a>

## 3.5 What Is an NTRIP Account?

### 3.5 What Is an NTRIP Account?

An NTRIP account is just the **username and password** that authenticate you to the caster. When your rover connects, it sends those credentials in a standard HTTP-style request. The caster checks them, confirms your subscription is active, and starts streaming corrections.


For GEODNET, your credentials are tied to your subscription. You can view your historical usage and top up service time directly through the GEODNET User Center.




---

<a id="3-6-choosing-a-mountpoint-coordinate-reference-frames"></a>

## 3.6 Choosing a Mountpoint — Coordinate Reference Frames

### 3.6 Choosing a Mountpoint — Coordinate Reference Frames

GEODNET offers four mountpoints, and the choice affects what coordinate system your output is in:



| Mountpoint | What It Outputs | When to Use |
| :--- | :--- | :--- |
| AUTO | Regional Geodetic Coordinate System (e.g. NAD83 in the US, ETRS89 in Europe, GDA2020 in Australia) | **Default choice for most users.** Your output matches the local survey datum, which is what land records, GIS systems, and construction drawings use. |
| AUTO_ITRF2020 | ITRF2020 at epoch 2015.0 | Scientific/global use, or if your downstream software expects ITRF2020. |
| AUTO_ITRF2014 | ITRF2014 at the current epoch | Compatibility with workflows built on ITRF2014. |
| AUTO_WGS84 | WGS84 (G2139) at mid-current-year | Generic global compatibility. Closest to what consumer GPS reports. |



**The short version:** if you do not have a specific reason to pick something else, use AUTO. It hands you coordinates in your country's standard datum, which is almost always what you actually want.


#### A deeper look at coordinate systems

Coordinate systems are one of those topics where every layer hides another layer. Here's enough to make sense of what your software is doing.


**Geographic vs. projected coordinates.** Latitude and longitude (geographic coordinates) are angles on a curved Earth. To draw on a flat map, surveyors and engineers use projected coordinates with units of meters or feet — Universal Transverse Mercator (UTM), State Plane (US), British National Grid, and many others. Your RTK device may output either or both. Construction drawings almost always use projected grid coordinates; legal land records often use geographic.


**The Earth is not a sphere.** It's an ellipsoid (slightly flattened at the poles), and it has a bumpy gravity field called the geoid. A **datum** is the mathematical model that defines the shape and orientation of the ellipsoid for a given coordinate system. Different **datums** put the same physical point at slightly different coordinates.


**Plate tectonics is why coordinate frames have epoch dates.** The Earth's tectonic plates move several centimeters per year. A point in California is in a slightly different absolute position today than it was in 2010, even though it hasn't physically moved relative to the surrounding land. National datums like NAD83 are anchored to the local plate, so coordinates stay stable for surveying and mapping purposes — your house's coordinates today are the same as they were 20 years ago. Global datums like ITRF and WGS84 are anchored to the Earth as a whole, which is more "correct" scientifically but means your house's coordinates technically change every year. The epoch — like "NAD83(2011) at 2010.0" — is the reference date that fixes when those coordinates were computed.


**Heights are weird.** GPS gives you **ellipsoidal height** — the height above the mathematical ellipsoid. But most people want **orthometric height**, the height above mean sea level (technically, above the geoid). The two can differ by 20–50 meters, depending on where you are. Surveying software converts between them using a geoid model like GEOID18 (US) or EGM2008 (global). If your reported altitude looks wildly off from the elevation on a topo map, you're probably looking at **ellipsoidal height** instead of orthometric.


**Practical advice for choosing a coordinate system:**


- **Working in your home country, on land records or construction:** use the regional datum (AUTO mountpoint).
- **Working with international engineering software, GIS aligned to global tiles, or scientific data:** ITRF2020 or WGS84.
- **Migrating an old project:** match the datum the project was originally surveyed in. Mixing datums silently is the #1 cause of "why is everything 1.5 meters off?"




---

<a id="3-7-nmea-gga-why-your-device-has-to-talk-back"></a>

## 3.7 NMEA GGA — Why Your Device Has to Talk Back

### 3.7 NMEA GGA — Why Your Device Has to Talk Back

This is the single most common cause of "I'm connected but not getting corrections."


GEODNET (and any **Nearest-Base** RTK service — see Section 4) needs to know your approximate location to pick the closest base station to you. Your rover communicates this by sending up an **NMEA GGA sentence** — a short, standard GPS data string containing a rough position fix.


> [!CRITICAL]
> If your device does not upload NMEA GGA, GEODNET cannot send you corrections. You will see a connection but no RTCM data flowing.


Most NTRIP clients have a setting like "Send GGA" or "Send NMEA position" — make sure it's enabled. Some clients send it once at connection; others send it every 10–30 seconds. Either is fine.




---

<a id="3-8-age-of-corrections"></a>

## 3.8 Age of Corrections

### 3.8 Age of Corrections

Once you're getting corrections, your device will report an "age of corrections" or "correction age" value, in seconds. This is the time elapsed between when the base station made its measurement and when your rover received it.


- **≤ 2 seconds** — Healthy. RTK Fix is achievable and stable.
- **2–5 seconds** — Marginal. May still hold Fix but accuracy can drift.
- **> 5 seconds** — Likely connection issue. Reconnect or check your cellular signal.


If age of corrections is climbing steadily, your rover is still connected but isn't receiving fresh data — usually a network problem on your end, not the caster's.




---

<a id="section-4"></a>

## Section 4: Network Architecture

### Section 4: Network Architecture






---

<a id="4-1-single-base-rtk-vs-network-rtk"></a>

## 4.1 Single-Base RTK vs. Network RTK

### 4.1 Single-Base RTK vs. Network RTK

There are two fundamentally different ways to deliver RTK:


**Single-Base RTK** — Your rover gets corrections from one specific base station. This is the original RTK model and still common for users who own their own base or use a small private network. The accuracy depends almost entirely on how close you are to that base — typically excellent within 10 km, degrading gradually beyond.


**Network RTK (NRTK)** — Your rover gets corrections derived from a network of base stations. The network knows where all its stations are and can either send you the nearest one's data, or compute a synthetic correction tailored to your exact location.


Network RTK has obvious advantages: wider coverage, redundancy if one station goes offline, and consistent accuracy across larger work areas.




---

<a id="4-2-nearest-base-rtk-vs-vrs-vs-mac"></a>

## 4.2 Nearest-Base RTK vs. VRS vs. MAC

### 4.2 Nearest-Base RTK vs. VRS vs. MAC

Within Network RTK, there are several different methods. Three you'll see referenced in the industry:


- **Nearest-Base** — The network looks at your reported NMEA GGA position, picks the geographically closest physical base station, and streams its corrections to you. Simple, robust, well-understood. Works best when station density is high enough that "nearest" is always close (typically within 20–40 km).
- **VRS (Virtual Reference Station)** — The network synthesizes a "virtual" base station at your exact location by interpolating data from several real stations around you. This makes the effective baseline near zero, even when real stations are far apart. More complex backend; useful for sparser networks.
- **MAC (Master-Auxiliary Concept)** — Similar to VRS but uses a different message format that gives the rover more raw data to work with. Less common in modern services.


> [!GEODNET NOTE: NEAREST-BASE, MADE VIABLE BY DENSITY]
> GEODNET currently provides a Nearest-Base RTK service. The reason this works well — and why dense networks don't necessarily need VRS — is station density. With ~20,000+ stations globally, the nearest one is usually well within the 20–40 km range where Nearest-Base performs strongly. In areas with dense coverage, you'll often have a station within 10 km, which is excellent.




---

<a id="4-3-baseline-length-and-why-density-matters"></a>

## 4.3 Baseline Length and Why Density Matters

### 4.3 Baseline Length and Why Density Matters

The **baseline** is the distance from your rover to the base station providing your corrections. It is the single biggest factor in RTK performance:



| Baseline | Typical Behavior |
| :--- | :--- |
| **0–10 km** | Fastest fix, most stable, best vertical accuracy. Atmospheric errors at the base and rover are nearly identical, so they cancel almost perfectly. |
| **10–20 km** | Still excellent for most work. Slight degradation in vertical accuracy. |
| **20–40 km** | Acceptable for many applications, but fix time gets longer and the solution is more sensitive to atmospheric activity. |
| **> 40 km** | Possible but unreliable. Fix may not hold during ionospheric disturbances. |



This is why a network's station density matters more than its total station count for marketing purposes. A network with 5,000 stations evenly distributed beats a network with 20,000 stations clustered in one country.




---

<a id="4-4-what-is-a-cors-network"></a>

## 4.4 What Is a CORS Network?

### 4.4 What Is a CORS Network?

**CORS** stands for **Continuously Operating Reference Station**. It refers to any GNSS station that is permanently running and logging data. Every government-operated geodetic network (the US's NGS CORS, Europe's EUREF, etc.) is a **CORS** network. So is every commercial RTK provider.


When people say "CORS network", they usually mean a publicly operated one — typically free to use but with patchy coverage, inconsistent equipment across stations, and no service-level guarantees.


> [!GEODNET NOTE: HOW GEODNET IS DIFFERENT FROM A TRADITIONAL CORS]
> GEODNET is technically a CORS network — every station operates continuously and is monitored 24/7. The differences from a traditional government CORS are uniformity (every station uses the same triple-band geodetic-grade hardware with NGS-calibrated antennas), density, global reach, and standardized RTCM 3.2 / NTRIP delivery. The backbone tier of the network — the high-reliability stations that provide redundancy — uses Hemisphere P40 receivers with cellular 4G failover and battery backup.




---

<a id="4-5-network-quality-and-validation"></a>

## 4.5 Network Quality and Validation

### 4.5 Network Quality and Validation

How does a network ensure that the data flowing to your rover is correct? GEODNET layers several validation mechanisms:


- **Position validation via PPP.** Every station's coordinates are independently verified using Precise Point Positioning, processed through NRCan's CSRS-PPP service. New stations only become active after PPP confirms a 3D sigma below 5 cm, and the check is repeated periodically — any station whose position drifts is automatically caught and re-evaluated.
- **24/7 monitoring.** Stations that drop offline, drift, or report bad data are flagged and excluded from the active service.
- **Hardware identity and integrity.** Every station ships with a unique hardware identifier so that the network can verify which physical device any given data stream is coming from. This prevents data spoofing and ensures every observation can be traced back to its source.
- **Spoofing detection.** Network-level analysis flags signal anomalies that could indicate someone trying to inject fake data.
- **Two-tier reliability.** Backbone stations target greater than 99% uptime; the broader station tier targets greater than 98%, with the network's redundancy filling any gaps.


For you as a customer, this means the corrections you receive are validated against multiple independent checks before they ever reach your rover.




---

<a id="section-5"></a>

## Section 5: Practical Use — Hardware, Setup, Surveying Concepts, and Applications

### Section 5: Practical Use — Hardware, Setup, Surveying Concepts, and Applications



---

<a id="5-1-what-kind-of-receiver-do-i-need"></a>

## 5.1 What Kind of Receiver Do I Need?

### 5.1 What Kind of Receiver Do I Need?

Any GNSS receiver that supports:


- **Multi-band** GNSS (dual-band minimum; triple-band recommended)
- **RTK firmware** (carrier-phase processing, not just code)
- **NTRIP client** capability (built-in or via a paired app)
- **RTCM 3.2** message format


…will work with GEODNET. Compatibility is broad — any device built around modern GNSS modules supports all four.


#### Antennas matter as much as the chipset

The receiver chip gets the attention, but the **antenna** often determines real-world performance more than the silicon does. Three things to know:


- **Multi-band antennas are required for multi-band receivers.** An L1-only antenna paired with a triple-band receiver gives you single-band performance.
- **Ground plane and choke-ring antennas reject multipath.** These are the larger, heavier antennas you see on surveyor poles and reference stations. The shape physically blocks reflected signals from below.
- **Antenna phase center matters at the centimeter level.** The electrical "center" of an antenna isn't always at its physical center, and it can shift with signal frequency and direction. Survey-grade antennas are calibrated by NGS to characterize this; entry-level antennas are not.


#### Tilt compensation and IMU integration

Modern handheld RTK receivers often include an **IMU (Inertial Measurement Unit)** — accelerometers, gyroscopes, and sometimes magnetometers — that lets the device track its tilt angle. With tilt compensation, you don't have to perfectly level the survey pole over a point; the device computes the correction automatically. This is a major productivity win in field work and is increasingly standard in prosumer-grade hardware.


> [!GEODNET NOTE: GEO-MEASURE]
> GEO-MEASURE is GEODNET's own handheld RTK receiver — a triple-band, full-constellation device built specifically to work seamlessly with the GEODNET network. It's a good reference example for what a modern RTK rover looks like: a survey-grade GNSS receiver paired with the GEO-MEASURE mobile app, designed for prosumer use (surveying, GIS data collection, asset mapping, drone ground control points). If you're standardizing on a device for your team and want the most streamlined experience, GEO-MEASURE is the reference platform.




---

<a id="5-2-setting-up-your-first-connection"></a>

## 5.2 Setting Up Your First Connection

### 5.2 Setting Up Your First Connection

A generic walkthrough that works on any NTRIP-capable device or app:


1. **Power on your receiver and acquire a basic GPS fix.** You need a Single fix before NTRIP can do anything (your device needs to know roughly where it is to send NMEA GGA).
2. **Open the NTRIP / corrections settings** on your device or paired app.
3. **Enter the caster details:** server rtk.geodnet.com (or your regional server — see Section 3.4), port 2101, and the username and password from your GEODNET account.
4. **Request the source table.** Most clients do this automatically when you tap "Connect" or "Get Mountpoints." This populates a list of available mountpoints.
5. **Select your mountpoint.** AUTO is the right default for almost everyone.
6. **Confirm "Send GGA / Send NMEA" is enabled.** Without this, no corrections will flow.
7. **Connect.** You should see an active data stream within a few seconds.
8. **Watch your fix status climb:** Single → DGPS → Float → Fix. With multi-band hardware in open sky, this typically takes 5–30 seconds.


> [!GEODNET NOTE: GEO-MEASURE WORKS OUT OF THE BOX]
> If you are using GEO-MEASURE, the NTRIP connection is built in — there is no caster, port, or mountpoint to enter manually. Your GEODNET account credentials are linked to the device, and corrections start flowing as soon as you connect it to our app.




---

<a id="5-3-reading-your-status"></a>

## 5.3 Reading Your Status

### 5.3 Reading Your Status

Your device will display a status indicator that reflects what is happening:


- **Single** — No corrections being applied.
- **DGPS / Differential** — Code-only corrections. Sub-meter, not centimeter.
- **RTK Float** — Carrier corrections active but ambiguity unresolved. ~10-50 cm.
- **RTK Fix / Fixed** — Full RTK solution. ~1-2 cm. **This is your target.**


If you stay on Float for more than a couple of minutes, see troubleshooting below.




---

<a id="5-4-common-troubleshooting"></a>

## 5.4 Common Troubleshooting

### 5.4 Common Troubleshooting

#### I'm connected but not getting corrections




- **Check NMEA GGA is being sent.** This is the #1 cause. Look for "Send GGA," "Send NMEA position," or similar in your client settings.
- **Check the mountpoint is correct.** Mountpoint names are case-sensitive.
- **Check firewall.** If you're on a corporate or restricted network, port 2101 may be blocked. Try cellular instead.


#### I can't get past Float to Fix




- **Sky view.** RTK needs a clear view of the sky — at least 30° above the horizon, ideally more. Trees, buildings, and metal structures all degrade performance.
- **Single-band hardware.** If you're using an L1-only receiver, Fix is unreliable. Multi-band hardware will solve this.
- **Baseline too long.** If the nearest GEODNET station is >40 km away, fix may not hold. Check the coverage map for your area.
- **Active ionospheric disturbance.** Solar storms degrade RTK temporarily. Usually resolves within hours.


#### I get Fix but lose it intermittently




- **Cellular dropouts.** Check your data connection. NTRIP is forgiving of brief gaps but minutes of dead air will cost the fix.
- **Multipath.** Working near reflective surfaces (metal sheds, vehicles, water) causes signals to bounce. Move to a more open area if possible.
- **Age of corrections climbing.** Check Section 3.8 — this points to a network/connection issue on your side.


#### Age of corrections keeps growing

Your rover is connected but not receiving fresh data. Disconnect and reconnect. If the issue persists, check your cellular signal or switch networks.




---

<a id="5-5-cellular-and-internet-requirements"></a>

## 5.5 Cellular and Internet Requirements

### 5.5 Cellular and Internet Requirements

NTRIP is extremely lightweight. RTCM 3.2 streams typically run at **300 bytes/second to 2 KB/second** — well under any modern cellular plan's limits. Even a weak 2G/3G signal can carry NTRIP if it is stable.


What matters more than bandwidth is **stability and latency**. Brief drops (less than 30 seconds) are usually survivable; longer outages will lose your Fix and require reacquisition.




---

<a id="5-6-surveying-and-measurement-concepts"></a>

## 5.6 Surveying and Measurement Concepts

### 5.6 Surveying and Measurement Concepts

RTK is widely used in surveying, but a high-precision number on the screen isn't automatically a high-precision result. A few concepts worth knowing.


#### Standard deviations and what your accuracy number means

Most RTK devices show a horizontal and vertical **standard deviation** (sigma, σ) — typically a few millimeters to a few centimeters when you have a good Fix. This is the receiver's internal estimate of how precise its solution is, based on satellite geometry, signal quality, and how clean the carrier-phase observations look.


Two important caveats. First, sigma is **precision, not accuracy** — it tells you how well-determined the position is given the model the receiver is using, but it doesn't catch errors in the underlying model (like a wrong base station coordinate). Second, **sigma can be optimistic.** A receiver may report 1 cm sigma while actually being 3 cm off. The way to catch this is independent verification — re-occupy points, tie to known monuments, and compare sessions.


#### Static, kinematic, and stop-and-go observation

RTK supports several measurement styles, and choosing the right one for the job affects results:


- **Kinematic (continuous):** The rover moves continuously and records positions at the chosen epoch rate. Used for tracking machinery, drones, and vehicles, and for dynamic applications like topographic strips.
- **Static:** The rover stays on a single point for a specified duration, and the device averages many epochs to produce a single high-quality position. Standard for control surveys and high-precision work.
- **Stop-and-go:** The rover walks between points but pauses on each measurement target for a few seconds while the device records multiple epochs. The standard workflow for boundary surveys, asset mapping, and topographic survey with discrete points.


#### Control points and re-occupation

Professional surveyors don't trust a single RTK measurement. Standard practice is to:


- Tie new work to **known control monuments** — government-published benchmarks with high-precision coordinates — at the start and end of a session.
- Re-occupy critical points twice, ideally with a different satellite geometry (separated in time by an hour or more), and check that the two observations agree.
- Compare the device's reported sigma with the actual repeatability — if they don't match, treat the work as suspect.


This isn't paranoia; RTK can occasionally produce a confident-but-wrong fix (a "false fix"), especially with single-band hardware or in difficult environments. Independent checks catch these.




---

<a id="5-7-applications-where-rtk-is-used"></a>

## 5.7 Applications: Where RTK Is Used

### 5.7 Applications: Where RTK Is Used

GEODNET RTK is in production use across many industries. Below is a survey of the main application areas, what makes RTK valuable in each, and the typical workflow.


#### Land surveying & construction

The original RTK use case. Boundary surveys, topographic mapping, stake-out for construction, and as-built documentation all benefit from centimeter accuracy. Modern construction sites also use **machine control** — excavators, dozers, and graders with on-board RTK and CAD models that automatically guide the blade or bucket to design grade. This eliminates the traditional staking workflow for many projects and dramatically speeds construction.


#### Precision agriculture

RTK auto-steer keeps a tractor or combine on a planned path with centimeter accuracy across field passes. This enables **controlled traffic farming** (always driving on the same wheel tracks to protect soil), variable-rate application of inputs (fertilizer, seed, pesticide), and elimination of overlap and skips in field operations. The ROI is straightforward: a 60-foot sprayer doing a 12-hour day with 6 inches of overlap wastes thousands of dollars per season in chemicals; RTK pays for itself in a single year on a moderate-sized farm.


#### Robotics and autonomous mowing

Consumer robotic mowers and commercial autonomous turf equipment have moved decisively to RTK. The reason: standard GPS isn't accurate enough to keep a robot on the correct side of a property line, and the perimeter wire systems used by older mowers are expensive to install and prone to damage. With RTK, the user defines a property polygon once and the robot mows it autonomously, year after year, with no perimeter wire and no dead zones. Delivery robots, sidewalk-following robots, and warehouse/yard robots use RTK for the same reason — predictable lane-level positioning outdoors.


#### Drones and UAVs

Two main uses. First, **flight control:** stable hover, precise way-point navigation, and reliable return-to-home. Second, **aerial mapping** — RTK-equipped drones can geotag photos to centimeter accuracy, dramatically reducing or eliminating the need for ground control points (GCPs) in photogrammetry workflows. PPK is often used for survey-grade drone mapping where the drone records raw data and corrections are applied post-flight; RTK is more common for real-time inspection, BVLOS operations, and delivery.


#### Autonomous vehicles and ADAS

Lane-level positioning for highway driving, intersection geometry for V2X, and HD-map georeferencing all need centimeter-class accuracy that lane-marking cameras alone can't provide. RTK is one input in a multi-sensor stack (GNSS + IMU + cameras + lidar + map) — but for the GNSS portion, it's the difference between knowing which road you're on and knowing which lane.


#### HD mapping and digital twins

High-definition maps for autonomous driving and 3D city models for digital-twin applications require the underlying geometry to be measured to centimeter accuracy. Mobile mapping vehicles equipped with RTK + lidar build these maps by driving the road network at survey speeds.


#### AR/VR and spatial computing

Outdoor augmented reality — placing virtual objects in real-world locations so that they appear consistent across users and time — needs precise geolocation to align virtual content with physical space. Location-based games, outdoor AR experiences, and real-world asset markers all become more believable when positioning is centimeter-class instead of meter-class.


#### Maritime, dredging, and hydrographic survey

RTK is used on boats and barges for dredging operations (knowing exactly where the bucket is, in the water and on the bottom), hydrographic surveying (mapping the seafloor with sonar referenced to surface position), and dynamic positioning for stationkeeping. Marine applications often use the GEODNET service combined with IMU and motion-reference sensors to handle the constant motion of the vessel.


#### Heavy machine control

Already mentioned in construction, but worth a separate note: machine control on excavators, bulldozers, motor graders, asphalt pavers, and milling machines is one of the highest-value RTK applications worldwide. Modern systems combine RTK position with on-board sensors (slope, blade angle, articulation) and a 3D CAD model to drive the cutting edge to design grade automatically.


#### Asset management and utilities

Utility companies use RTK to map underground pipe and cable networks as they're installed, build asset inventories of poles, valves, and access points, and locate buried infrastructure during repair work. Centimeter accuracy here saves both money (fewer accidental utility strikes during digging) and lives.


#### Scientific and geodetic monitoring

Long-term RTK and PPK observation enables monitoring of structural deformation (bridges, dams, tall buildings), tectonic plate motion, glacier and ice-sheet movement, and post-earthquake displacement. These applications often use observation periods of hours to days to extract sub-centimeter signals from longer-term GNSS data.




---

<a id="section-6"></a>

## Section 6: Comparing RTK Correction Service Providers

### Section 6: Comparing RTK Correction Service Providers

Customers choosing an RTK correction service typically have several options. This section is a factual orientation to the landscape — what kinds of providers exist, what each is good at, and how to think about which fits your work. Many of these providers are also collaborators with GEODNET in various ways; this isn't a competitive piece. It's a map.




---

<a id="6-1-the-categories-of-rtk-service"></a>

## 6.1 The Categories of RTK Service

### 6.1 The Categories of RTK Service

Most RTK correction services fall into one of four categories:


#### Public CORS networks

Operated by national geodetic agencies (NGS in the US, IGN in France, OS in the UK, GA in Australia, and many others). Stations are typically free to access for post-processing (PPK / RINEX downloads), and some agencies also offer free real-time RTK or NRTK services. Coverage is best in well-funded countries with active programs and patchy or absent in many parts of the world. Equipment quality is generally high but not always uniform across the network — different stations were installed in different decades with different hardware. Requires lengthy setup to acquire permissions and setup accounts.


**Best for:** Government work, academic and scientific use, post-processing where real-time corrections are not required, and as a reference baseline for comparison.


#### Commercial regional and global networks

Subscription-based RTK services from established surveying-and-positioning companies. Names you'll encounter include Trimble (VRS Now), Leica/Hexagon (HxGN SmartNet), Topcon (TopNET live), and others. These networks typically use VRS or MAC network RTK, are well-supported with established sales and support channels, and have decades of operational track record. Pricing varies by region and tier; multi-year contracts and per-seat licenses are common.


**Best for:** Established surveying firms with existing vendor relationships, machine control fleets aligned with a specific OEM, and customers who prioritize a single integrated workflow from receiver to office software.


#### PPP and PPP-RTK services

Services like Trimble RTX, Hexagon TerraStar, NovAtel TerraStar, and Swift Skylark deliver corrections via geostationary satellite or internet, often on a global basis with no need for a nearby base station. Convergence is slower than network RTK (minutes vs. seconds), and accuracy at full convergence is typically a few centimeters rather than the 1–2 cm of a dense network RTK service. PPP and PPP-RTK are particularly useful in remote regions, marine work, and applications where global consistency matters more than the lowest possible latency.


**Best for:** Marine and offshore work, remote-area operations, agricultural fleets crossing wide regions, and applications where coverage gaps are unacceptable.


#### Network RTK services covering large geographic areas

Services like GEODNET that aim for broad geographic coverage with a uniform service offering. The service model is straightforward: an active subscription, an NTRIP account, an active rover, and corrections start flowing. There's typically no long-term contract requirement and no proprietary data format.


**Best for:** Customers who want centimeter accuracy with broad coverage, hardware-agnostic compatibility, and straightforward subscription terms. Particularly well suited to robotics, drones, and prosumer surveying where the customer values flexibility over a single-vendor stack.




---

<a id="6-2-what-differentiates-rtk-services"></a>

## 6.2 What Differentiates RTK Services

### 6.2 What Differentiates RTK Services

When evaluating any RTK service for your work, the practical questions to ask:



| Criterion | What to Look For |
| :--- | :--- |
| Coverage | Does the service cover your operating area? How dense are the stations there? What is the typical baseline? |
| **Accuracy** | Stated centimeter accuracy at typical baselines, with realistic expectations for vertical (usually ~1.5-2× horizontal). |
| Latency | Correction age in seconds. Lower is better; under 2 seconds is typical for a healthy network. |
| Data format | RTCM 3.2 or 3.3 over standard NTRIP is hardware-agnostic. Proprietary formats lock you in. |
| Hardware compatibility | Does it work with your existing receivers? Multi-vendor support matters if your fleet is not standardized. |
| Pricing model | Subscription vs. usage vs. per-seat. Are there contract terms? Free trial? |
| Coordinate frame options | Does the service deliver in your local datum, ITRF, or WGS84 — and can you choose? |
| Network architecture | Single-base, Nearest-Base, VRS, MAC. Affects performance and supported workflows. |
| Reliability | Stated uptime, monitoring practices, redundancy. |



There is no single "best" RTK service for every customer. A high-volume surveying firm with established Trimble equipment and Trimble field software may rationally choose Trimble VRS Now even at higher cost, because the integration value is real. A robotics startup, a precision-ag operation, or a prosumer surveyor working across multiple device brands often values broad coverage, hardware-agnostic compatibility, and flexible pricing more — and that is where GEODNET fits well.


- **Coverage:** 20,000+ stations across 150+ countries, with consistently dense coverage in North America, Europe, and Australia, and growing networks in Asia, Africa, and South America.
- **Hardware uniformity:** Every station uses the same triple-band, full-constellation, geodetic-grade hardware with NGS-calibrated antennas. The experience is the same in California, Spain, or Tokyo.
- **Standards-based delivery:** RTCM 3.2 over standard NTRIP. Works with essentially any RTK-capable receiver — u-blox, Quectel, Septentrio, Trimble, Hemisphere, Topcon, Leica, and others.
- **Pricing flexibility:** Subscriptions and top-ups, no long-term contracts, free trials available.
- **Quality assurance:** PPP-validated station coordinates, 24/7 monitoring, two-tier reliability architecture — see Section 4.5.


If your work fits a category that is already deeply integrated with another vendor's stack, that integration may be more valuable than any other consideration. If you are choosing fresh — or if you value flexibility, broad coverage, and standards-based compatibility — GEODNET is built to serve that need.




---

<a id="section-7"></a>

## Section 7: Regulatory, Legal, and Standards Considerations

### Section 7: Regulatory, Legal, and Standards Considerations

RTK is a measurement tool, and like any measurement tool used for legal, safety-critical, or regulated work, it sits inside a framework of standards, licensing, and practice. This section is an orientation, not legal advice — for specifics, consult a licensed surveyor or counsel in your jurisdiction.




---

<a id="7-1-surveying-and-land-records"></a>

## 7.1 Surveying and Land Records

### 7.1 Surveying and Land Records

In most countries, formal land surveys — boundary determinations, subdivisions, recorded plats, and similar documents that affect legal property rights — must be performed by a **licensed surveyor** who is qualified in the relevant jurisdiction. The licensing requirement applies regardless of the equipment used: an RTK system that achieves centimeter accuracy does not change who is legally permitted to sign a survey.


RTK is a widely accepted tool in licensed surveying practice; many surveying boards have published guidance on using GNSS for cadastral and engineering work, including expectations for redundancy, control ties, and quality control. If you are using RTK in regulated surveying contexts:


- Tie new measurements to known control monuments, ideally at the start and end of each session.
- Re-occupy critical points with separated geometry (typically separated by at least 30–60 minutes) to verify repeatability.
- Document the network and base station used, the coordinate frame, and the standard deviations.
- Follow the published guidance of your jurisdiction's surveying authority.


For non-regulated uses — internal as-built records, asset mapping, agricultural operations, robotics, drone mapping for non-legal purposes — these formal requirements do not apply, and RTK can be used freely.




---

<a id="7-2-aviation-use"></a>

## 7.2 Aviation Use

### 7.2 Aviation Use

Aircraft navigation is heavily regulated. Civil aviation positioning — for instrument approaches, RNP procedures, and similar — is generally based on GPS augmented by SBAS (WAAS, EGNOS) and, near airports, Ground-Based Augmentation Systems (GBAS). RTK as delivered by a commercial network is **not certified for primary aircraft navigation** and is not used by airlines for that purpose.


RTK is, however, widely used in uncrewed aviation (drones / UAS) for non-certified applications: precision photogrammetry, mapping, inspection, and delivery missions where the operator is responsible for the position quality. Most national aviation authorities (FAA Part 107 in the US, EASA in Europe, CASA in Australia, etc.) have specific rules for commercial drone operations; RTK is one of the technologies that supports more advanced operational categories, but it does not exempt operators from regulatory requirements.




---

<a id="7-3-frequency-use-and-radio-licensing"></a>

## 7.3 Frequency Use and Radio Licensing

### 7.3 Frequency Use and Radio Licensing

Some RTK setups use UHF or VHF radios to broadcast corrections from a local base station to one or more rovers — particularly in agriculture, machine control, and remote field work where cellular coverage is poor. These radios **often require a frequency license** in the country of operation, and the rules vary widely.


- **United States:** Most surveying-grade UHF data radios require an FCC license under Part 90.
- **European Union:** National regulators control specific frequency bands; some are license-free for low-power use, others require coordination.
- **Other regions:** Generally similar — license-free bands exist but are usually too low-power for serious RTK ranges; longer-range work requires a licensed channel.


Internet-based RTK delivery (NTRIP over cellular or Wi-Fi) **requires no radio license**: the corrections travel as ordinary internet traffic. This is a meaningful advantage for users who want to avoid the licensing process entirely.




---

<a id="7-4-privacy-and-data-protection"></a>

## 7.4 Privacy and Data Protection

### 7.4 Privacy and Data Protection

RTK correction services involve some data flowing in both directions. Your rover sends an NMEA GGA position up to the network so the network can pick the right base station; the network sends RTCM corrections down. This means an RTK service has visibility into the approximate locations of its connected users.


In jurisdictions with strong data protection regimes (GDPR in Europe, CCPA in California, and similar laws elsewhere) service providers have specific obligations regarding what user data they collect, how long they retain it, and what they may do with it. If your work involves personally sensitive position data — for example, if rover positions could identify individuals' homes or movements — review the provider's data-handling practices and ensure they align with your obligations.




---

<a id="7-5-standards-bodies-and-where-to-read-more"></a>

## 7.5 Standards Bodies and Where to Read More

### 7.5 Standards Bodies and Where to Read More

If you want to dig into the standards underlying RTK and GNSS, the main organizations are:


- **RTCM SC-104** — Special Committee 104 of the Radio Technical Commission for Maritime Services. Maintains the RTCM 3 standard for differential GNSS.
- **IGS** — International GNSS Service. Coordinates the global tracking network and produces precise orbits and clocks used by PPP services worldwide.
- **BKG** — Bundesamt für Kartographie und Geodäsie (German Federal Agency for Cartography and Geodesy). Maintainer of the NTRIP standard.
- **NGS** — National Geodetic Survey (United States). Maintains NAD83, the US CORS network, and antenna calibration tables used industry-wide.
- **IAG** — International Association of Geodesy. Maintains the ITRF (International Terrestrial Reference Frame), and global geodetic reference standards.
- **ISO TC 211** — Geographic Information / Geomatics standards committee. Defines coordinate-reference-system standards used in GIS.


Most of the underlying GNSS and RTK technology is documented in open standards, which is part of why the industry has been able to grow quickly and interoperate broadly.




---

<a id="section-8"></a>

## Section 8: Reference Tables and Glossary

### Section 8: Reference Tables and Glossary

This section pulls together quick-reference material that is referenced throughout the document. Use it as a lookup when something in another section assumes a term you do not remember.




---

<a id="8-1-gnss-constellations-and-frequencies"></a>

## 8.1 GNSS Constellations and Frequencies

### 8.1 GNSS Constellations and Frequencies


| Constellation | Operator | Active Satellites (approx) | Main Civilian Frequencies |
| :--- | :--- | :--- | :--- |
| **GPS** | United States (USAF/USSF) | 31 | L1 (1575.42 MHz), L2 (1227.60 MHz), L5 (1176.45 MHz) |
| GLONASS | Russia (Roscosmos) | 24 | L1 (~1602 MHz), L2 (~1246 MHz), L3 (1202.025 MHz) |
| Galileo | European Union (EUSPA) | 28 | E1 (1575.42 MHz), E5a (1176.45 MHz), E5b (1207.14 MHz), E6 (1278.75 MHz) |
| BeiDou (BDS-3) | China (CSNO) | 35+ | B1I/B1C (1561.098/1575.42 MHz), B2a (1176.45 MHz), B3I (1268.52 MHz) |
| QZSS (regional) | Japan (Cabinet Office) | 4 (operational); planned to 7 | L1, L2, L5, L6 — same as GPS plus L6 augmentation |
| NavIC (regional) | India (ISRO) | 7 | L5 (1176.45 MHz), S-band (2492.028 MHz) |





---

<a id="8-2-rtcm-3-message-types-common-reference"></a>

## 8.2 RTCM 3 Message Types — Common Reference

### 8.2 RTCM 3 Message Types — Common Reference

A subset of RTCM 3 message types you will encounter in modern RTK source tables:



| Message Type | Content |
| :--- | :--- |
| MT1004 | GPS L1/L2 extended observations (legacy) |
| MT1005, 1006 | Stationary antenna reference point (ARP) coordinates |
| MT1007, 1008, 1033 | Antenna descriptor and serial number |
| MT1019 | GPS broadcast ephemeris |
| MT1020 | GLONASS broadcast ephemeris |
| MT1042, 1046 | BeiDou and Galileo ephemeris |
| MT1071–1077 | GPS Multiple Signal Messages (MSM1–MSM7) — modern observation messages |
| MT1081–1087 | GLONASS MSM |
| MT1091–1097 | Galileo MSM |
| MT1121–1127 | BeiDou MSM |
| MT4072 (proprietary) | u-blox station information; varies by vendor |



MSM7 messages contain full-resolution carrier phase, code, Doppler, and signal strength for all tracked signals from a constellation. GEODNET delivers MSM7 for all four major constellations.




---

<a id="8-3-rtk-status-quality-codes"></a>

## 8.3 RTK Status / Quality Codes

### 8.3 RTK Status / Quality Codes


| Status | Numeric Code (NMEA GGA) | Accuracy | Meaning |
| :--- | :--- | :--- | :--- |
| Invalid / No fix | 0 | — | No position available |
| GPS / Single | 1 | 1–5 m | Standard GNSS, no corrections |
| **DGPS** | 2 | 0.3–1 m | Code-based differential corrections |
| PPS | 3 | — | Precise Positioning Service (encrypted, military) |
| RTK Fixed | 4 | 1–2 cm | Carrier-phase ambiguity resolved — target state |
| **RTK Float** | 5 | 10–50 cm | Carrier-phase corrections, ambiguity unresolved |
| Estimated (DR) | 6 | varies | Dead reckoning from IMU during outage |
| Manual input | 7 | — | Manually entered position |
| Simulator | 8 | — | Simulated data |





---

<a id="8-4-common-coordinate-reference-systems"></a>

## 8.4 Common Coordinate Reference Systems

### 8.4 Common Coordinate Reference Systems


| System | Region | Notes |
| :--- | :--- | :--- |
| **WGS84** | Global | Default global reference frame; what consumer GPS reports. |
| ITRF (various, e.g., 2014, 2020) | Global | Scientific reference frame; updates account for plate motion. |
| NAD83 (2011) | USA, North America | Anchored to North American plate; stable coordinates over time. |
| NAD83(CSRS) | Canada | Canadian realization of NAD83. |
| ETRS89 / ETRF2000, 2010 | **Europe** | Anchored to European plate. |
| GDA2020 | **Australia** | Current Australian datum, replaced GDA94. |
| JGD2011 | Japan | Updated after 2011 Tōhoku earthquake. |
| CGCS2000 | China | Chinese national datum. |
| NZGD2000 | New Zealand | New Zealand national datum. |
| SIRGAS2000 | South America | South American national datum framework. |





---

<a id="8-5-glossary"></a>

## 8.5 Glossary

### 8.5 Glossary


| Term | Meaning |
| :--- | :--- |
| **Ambiguity (carrier-phase)** | The integer number of full carrier-wave cycles between satellite and receiver. Resolving it is the core problem of RTK. |
| **ARP (Antenna Reference Point)** | The physical reference point on an antenna whose coordinates are reported. |
| **Baseline** | Distance between rover and the base station providing its corrections. Shorter is better. |
| **Base station** | A GNSS receiver at a precisely known location, used as the reference for differential / RTK corrections. |
| **Carrier phase** | The radio wave underneath the GNSS data signal. Measuring it precisely is what enables centimeter accuracy. |
| **Caster** | The NTRIP server that distributes correction streams. |
| **Code phase** | The data pattern carried by a GNSS signal. Used for standard meter-level GPS. |
| **Constellation** | A set of GNSS satellites operated by a single agency (GPS, GLONASS, Galileo, or BeiDou). |
| **CORS** | Continuously Operating Reference Station — any permanently installed GNSS station. |
| **Cycle slip** | A momentary loss of carrier-phase tracking that requires re-resolving ambiguity for the affected satellite. |
| **Datum** | The mathematical model defining the shape and orientation of the Earth's reference ellipsoid for a coordinate system. |
| **DGPS** | Differential GPS. Code-phase corrections, sub-meter accuracy. Coarser than RTK. |
| **DOP (Dilution of Precision)** | A measure of how favorable the satellite geometry is for positioning. Lower is better. Variants include HDOP, VDOP, PDOP, GDOP. |
| **Ellipsoidal height** | Height above the reference ellipsoid (the mathematical Earth model). What raw GPS reports. |
| **Epoch** | (1) A reference date for a coordinate system, such as 2010.0. (2) A single instant in time at which observations are recorded. |
| **Ephemeris** | Satellite orbit information broadcast or computed. |
| **Fix (RTK Fixed)** | Full RTK solution with carrier-phase ambiguity resolved. ~1–2 cm accuracy. |
| **Float (RTK Float)** | Carrier-phase corrections active but ambiguity unresolved. ~10–50 cm accuracy. |
| **GBAS** | Ground-Based Augmentation System. Used near airports for precision aircraft approach. |
| **Geoid** | The shape the surface of the oceans would take under gravity alone — the reference for orthometric height. |
| **GGA** | An NMEA sentence containing time, position, and fix-status data. The rover sends this up to the network. |
| **GNSS** | Global Navigation Satellite System. Umbrella term covering GPS, GLONASS, Galileo, BeiDou, and others. |
| **GPS** | The U.S. GNSS constellation. Often used loosely as a synonym for GNSS. |
| **IGS** | International GNSS Service. Coordinates the global tracking network. |
| **IMU** | Inertial Measurement Unit — accelerometers and gyroscopes used for tilt compensation and dead reckoning. |
| **Ionospheric delay** | Signal delay from passing through the charged ionosphere; the largest variable error in GNSS. |
| **ITRF** | International Terrestrial Reference Frame. A globally consistent coordinate system used in science. |
| **L1, L2, L5** | The main GNSS frequencies. Multi-band receivers track two or more. |
| **LAMBDA** | The standard algorithm for resolving carrier-phase integer ambiguities. |
| **MAC** | Master-Auxiliary Concept. A network RTK method. |
| **Mountpoint** | A named correction stream on an NTRIP caster. |
| **MSM (Multi-Signal Message)** | Modern RTCM 3 message family carrying observations from a single constellation. |
| **Multipath** | Signals bouncing off surfaces before reaching the receiver, causing errors. |
| **NMEA** | The standard format for GNSS output sentences (GGA, RMC, GSA, etc.). |
| **NRTK** | Network RTK. RTK delivered from a network of stations rather than a single base. |
| **NTRIP** | Networked Transport of RTCM via Internet Protocol. The protocol that delivers RTCM corrections over the internet. |
| **Orthometric height** | Height above the geoid (approximately mean sea level). What topographic maps show. |
| **PCO / PCV** | Phase Center Offset / Phase Center Variation — antenna calibration parameters. |
| **PDOP** | Position Dilution of Precision. |
| **PPK** | Post-Processed Kinematic. Same idea as RTK but applied after the fact in software. |
| **PPP** | Precise Point Positioning. Single-receiver high-precision technique using global orbit/clock data. |
| **Rover** | The mobile receiver whose precise position you want. |
| **RTCM** | Radio Technical Commission for Maritime Services. The data format used for GNSS corrections. |
| **RTK** | Real-Time Kinematic. The technique that delivers centimeter-level positioning in real time. |
| **SBAS** | Satellite-Based Augmentation System (WAAS, EGNOS, or MSAS). Free sub-meter corrections from geostationary satellites. |
| **Sigma (σ)** | Standard deviation; the receiver's estimate of position precision. |
| **Source table** | The list of available mountpoints on an NTRIP caster. |
| **TTF (Time To Fix)** | How long the receiver takes from cold start to RTK Fix. |
| **Tropospheric delay** | Signal delay from passing through the troposphere; modeled rather than cancelled by multi-band. |
| **VRS** | Virtual Reference Station. A network RTK method that synthesizes a "nearby" base. |
| **WAAS / EGNOS / MSAS** | Regional SBAS systems for North America, Europe, and Japan respectively. |
| **WGS84** | World Geodetic System 1984. The default global coordinate system for consumer GPS. |





---

<a id="rtk-service-faq"></a>

## RTK Service FAQ

### Q: Why is the login failing?

A: Check that your credentials are valid and have not expired. Ensure that the username and password match the credentials provided. If the issue persists, contact support for further assistance.



### Q: Is there a limit to the number of simultaneous connections?

A: Yes, the limit depends on your account type. Typically, most accounts allow only one simultaneous connection. Refer to your account details for specific limitations.



### Q: Can I share my credentials across multiple devices, or is it restricted to one connection at a time?

A: Credentials are typically restricted to one device at a time. Attempting to share them across multiple devices may cause login issues or connection failures. Please refer to your specific account details for any exceptions.



### Q: Does the account allow access to multiple base stations simultaneously?

A: No, accounts are configured to access data from only one base station at a time. If you require data from multiple stations, you may need a different setup or account type.



### Q: Have I selected the correct mountpoint?

A: In most cases, selecting "AUTO" as the mountpoint is sufficient unless otherwise specified. If unsure, follow the instructions provided for your specific use case.



### Q: How do I find a list of available mountpoints, and which one is optimal for my use case?

A: After entering the NTRIP connection details in your device, a list of available mountpoints will be displayed. Typically, "AUTO" is the best choice unless you have been instructed otherwise.



### Q: Does the server provide correction data in a format compatible with my receiver, e.g., RTCM 3.x?

A: Most servers deliver correction data in standard formats like RTCM 3.x, which are widely compatible with modern GNSS receivers. Verify your receiver’s supported formats to ensure compatibility.



### Q: How far can I be from the base station to still receive corrections?

A: For optimal accuracy, maintain a distance of 25 km or less from the base station. Beyond this range, performance and accuracy may degrade.



### Q: How accurate are the RTK corrections at this distance?

A: Within 25 km of the base station, RTK corrections typically achieve 2 cm accuracy. Performance may vary when exceeding this distance, depending on environmental conditions and equipment quality.



### Q: I'm trying to connect to the RTK network using the provided IP and port, but the mountpoint won't load. What should I check?

A: If you're using the AUTO mountpoint, make sure that your GNSS receiver is configured to send GGA messages. These messages provide your location to the server, which is necessary for delivering accurate corrections. Without GGA data, the AUTO mountpoint will not respond properly.



### Q: What host or IP address should I use to connect to the RTK service?

A: The default host is rtk.geodnet.com. If you're experiencing connection issues, you can try using the numerical IP address: 13.56.117.10.



### Q: How do I find the nearest RTK base station?

A: To locate the nearest station:


1. Go to https://rtk.geodnet.com/map
2. Log in with your RTK service credentials
3. Enter your coordinates or navigate to your location on the map
4. The map will display the nearest available base stations



### Q: What should I do if there are no RTK stations near my location?

A: If there are no stations nearby, you have a couple of options:


1. Set up your own station — you can purchase a compatible station here: https://hyfix.ai/products/mgw310-multi-platform-station and contribute to the network while gaining access to corrections.
2. Contact us — reach out to our team to discuss the possibility of deploying a station in your area. We're always looking to expand coverage in high-demand regions.




---

