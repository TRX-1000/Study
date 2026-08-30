# Computer Networks — Unit 1 Study Guide
### Computer Networks & the Internet + Application Layer (HTTP/HTTPS)

> Textbook reference: *Computer Networking: A Top-Down Approach*, Kurose & Ross, 8th Ed.

---

## Table of Contents

1. [What Is the Internet?](#1-what-is-the-internet)
2. [The Network Edge](#2-the-network-edge)
3. [The Network Core](#3-the-network-core)
4. [Internet Structure — A Network of Networks](#4-internet-structure--a-network-of-networks)
5. [Performance: Delay, Loss & Throughput](#5-performance-delay-loss--throughput)
6. [Protocol Layers & Reference Models](#6-protocol-layers--reference-models)
7. [Network Devices](#7-network-devices)
8. [Application Layer Fundamentals](#8-application-layer-fundamentals)
9. [The Web, HTTP & HTTPS](#9-the-web-http--https)
10. [Cookies & Web Caching](#10-cookies--web-caching)
11. [HTTP Evolution: HTTP/1.1 → HTTP/2 → HTTP/3](#11-http-evolution-http11--http2--http3)
12. [Securing HTTP: TLS/SSL](#12-securing-http-tlsssl)
13. [Quick-Reference Formula Sheet](#13-quick-reference-formula-sheet)

---

## 1. What Is the Internet?

### 1.1 Two ways to define it

**The "nuts-and-bolts" view** — the Internet is physical stuff:
- **Hosts / end systems**: billions of connected devices (PCs, servers, phones, IoT devices — light bulbs, watches, cars, toasters). By 2027, an estimated 41 billion IoT devices will exist.
- **Packet switches**: routers and switches that forward chunks of data (packets).
- **Communication links**: fiber, copper, radio, satellite — each with a **bandwidth** (transmission rate).
- **Networks**: collections of devices/links managed by one organization (an ISP, a company, a university).

**The "service" view** — the Internet is an infrastructure that provides services to *applications* (Web, video streaming, email, gaming, e-commerce). It gives programmers a **programming interface** (sockets) so distributed apps can "hook in" to the network's transport service — much like a postal service offers a standard way to send a letter without you needing to know how mail trucks are routed.

### 1.2 What is a protocol?

A protocol is a set of rules governing the **format** and **order** of messages exchanged between two or more communicating entities, and the **actions taken** on transmission/receipt of a message.

**Analogy — human protocols vs. network protocols:**

| Human protocol | Network protocol |
|---|---|
| "Hi" → "Hi" → "Got the time?" → "2:00" | TCP connection request → TCP connection response → GET request → file response |

Just as saying "Hi" first is the expected *opening move* before asking a question, a client must complete a TCP handshake before it can send an HTTP GET. If the "protocol" isn't followed (e.g., you ask a stranger the time without greeting them, or a client sends GET before the TCP handshake completes), the exchange breaks down.

Protocols are documented as **RFCs (Request for Comments)** by the **IETF (Internet Engineering Task Force)** — these are open standards, unlike **proprietary protocols** (e.g., early Skype) that only one vendor controls.

---

## 2. The Network Edge

The **network edge** is where end systems (hosts) live — clients and servers. Hosts are called "end systems" because they sit at the logical edge of the network and run application programs (a browser, a mail server, etc.).

### 2.1 Access networks

The **access network** physically connects an end system to its first router (the "edge router"). Three main types:

| Type | Example |
|---|---|
| Residential | Home Wi-Fi router + fiber/cable modem |
| Institutional | University/company Ethernet + Wi-Fi |
| Mobile | 4G/5G cellular |

**What matters when evaluating an access network:**
- **Transmission rate** (bits/sec) it offers
- Whether bandwidth is **shared** among many users or **dedicated** to you

**Home network** typical setup: a combined router/firewall/NAT box, wired Gigabit Ethernet to a desktop, and Wi-Fi (54–450 Mbps) to phones/laptops — all connecting upstream to fiber/cable back to the ISP's "headend."

**Enterprise networks**: mix of Ethernet switches (100 Mbps–10 Gbps) and Wi-Fi access points, feeding an institutional router that connects out to the ISP.

**Data center networks**: hundreds to thousands of servers linked by very high-bandwidth (10s–100s of Gbps) internal links.

### 2.2 Physical media

A **bit** propagates between a transmitter and receiver over a **physical link**.

- **Guided media** — signal is confined to a solid medium:
  - **Twisted pair (copper)**: Cat-5 → 100 Mbps/1 Gbps Ethernet; Cat-6 → 10 Gbps Ethernet.
  - **Coaxial cable**: two concentric copper conductors, bidirectional, broadband (many frequency channels, 100s of Mbps each).
  - **Fiber optic**: glass fiber carrying light pulses (each pulse = a bit). Very high speed (10s–100s of Gbps), very low error rate, immune to electromagnetic interference — this is why fiber has become the backbone medium of choice.
- **Unguided media** — signal propagates freely through the air:
  - **Radio**: no physical wire, broadcast, affected by reflection/obstruction/interference.
  - Types: terrestrial microwave (~45 Mbps), Wi-Fi (100s Mbps), cellular (~10s Mbps), satellite (~45 Mbps/channel, but a huge **280 ms** end-to-end delay because of the distance to a geostationary satellite).

**Half-duplex vs. full-duplex** — think of a **walkie-talkie vs. a telephone call**:
- *Half-duplex* (walkie-talkie): only one party can transmit at a time; you must say "over" and let go of the button.
- *Full-duplex* (phone call): both parties can talk and listen simultaneously, because there are effectively two separate channels.

---

## 3. The Network Core

The **network core** is the mesh of interconnected routers that ties access networks together. Its job: move packets from a source end system, hop by hop, to a destination end system.

### 3.1 Packet switching

A sending host breaks an application message into smaller chunks called **packets**, each of length *L* bits, and injects them into the network at the link's transmission rate *R*.

**Store-and-forward**: a router must receive the **entire** packet before it can start forwarding even the first bit of it onward. This is like a relay race where a runner cannot pass the baton until they have received the *whole* baton — no partial transmission.

> **Worked example (one-hop transmission delay):**
> L = 10 Kbits, R = 100 Mbps → transmission delay = L/R = 10,000 / 100,000,000 = **0.1 ms**
> For an end-to-end path with zero propagation delay, a two-hop trip costs **2L/R**.

**Two key core functions** (easy to conflate — keep them separate):
| | Scope | What it does |
|---|---|---|
| **Forwarding** | Local, per-router | Moves an arriving packet from an input link to the correct output link, using a local forwarding table (lookup destination address → output port) |
| **Routing** | Global, network-wide | Runs algorithms that figure out the end-to-end paths packets should take, and builds the forwarding tables |

Think of forwarding as a signpost at a single road junction telling you which lane to take, while routing is the process (map-making, surveying) that decided where every signpost across the country should point.

**Queuing delay & loss**: if the arrival rate of packets to a link (in bps) exceeds the link's transmission capacity for a sustained period, packets queue up in the router's buffer, waiting their turn. If the buffer is full, arriving packets are **dropped (lost)**.

### 3.2 Circuit switching

Circuit switching **reserves** a fixed slice of resources end-to-end, for the entire duration of a "call," whether or not there's data flowing at every instant — this is how the traditional telephone network works.

- Dedicated resources → **guaranteed, circuit-like performance**, but resources sit idle when the caller pauses.
- Each call knows the *entire path* in advance (decided by the source at setup); in packet switching, each packet only carries the *final destination* — intermediate routers decide the path hop by hop.

**Multiplexing methods for sharing a circuit-switched link:**
- **FDM (Frequency Division Multiplexing)**: divide the frequency spectrum into narrow bands, one per call — like several radio stations broadcasting simultaneously on different frequencies, never interfering.
- **TDM (Time Division Multiplexing)**: divide time into frames and slots; each call gets a periodic slot — like a shared classroom projector where each group gets a fixed 5-minute turn every hour, whether they need it or not.

### 3.3 Why packet switching won: statistical multiplexing

In a typical voice call, **50–60% of the time is silence**. Circuit switching wastes that idle capacity because the slot is reserved regardless. Packet switching lets many *bursty* users share one link — bandwidth is only consumed when someone is actually sending — this is **statistical multiplexing**.

> **Worked example:**
> Link capacity: 1 Gbps. Each active user needs 1 Mbps. Only ~10% of users are active at any instant.
> - Circuit switching: max **1000 users** (hard guaranteed cap — 1 Gbps / 1 Mbps).
> - Packet switching: **~5000+ users** can be supported (statistically, with low blocking probability), since not everyone transmits at once.

This is exactly why Internet backbone links are provisioned as big shared trunks (Gbps/Tbps), not divided into millions of tiny dedicated per-subscriber circuits.

### 3.4 Packet switching vs. circuit switching — side by side

| Packet Switching | Circuit Switching |
|---|---|
| Connectionless | Connection-oriented (needs call setup) |
| Designed for bursty data | Designed for continuous voice |
| Flexible | Inflexible |
| Packets may arrive out of order, reassembled at destination | Data arrives in the same order sent |
| Store-and-forward | FDM & TDM |
| Network layer | Physical layer |
| Bandwidth used dynamically (saved) | Bandwidth reserved (can be wasted) |
| Risk: congestion → delay & loss (needs reliability/congestion-control protocols) | Risk: call setup delay, and no sharing when idle |

**Is packet switching a "slam dunk winner"?** Not entirely — it is excellent for bursty data and simple (no call setup), but it can suffer excessive congestion (delay & packet loss from buffer overflow), which is exactly *why* the Internet needs extra protocols on top for reliable transfer and congestion control (later units: TCP).

---

## 4. Internet Structure — A Network of Networks

The Internet connects **millions of access ISPs**. Connecting every access ISP directly to every other would need O(N²) links — utterly unscalable (imagine every person in a city having a private road straight to every other person's house).

The structure evolved in stages:

1. **One global transit ISP** — every access ISP buys connectivity from one big ISP (a customer–provider economic relationship).
2. **Competition** — multiple global-scale ISPs (A, B, C...) emerge, since being "the one ISP" is a profitable business.
3. **Peering** — competing ISPs want to reach each other's customers, so they connect directly at **IXPs (Internet Exchange Points)**, via **peering links** (often with no money changing hands, since it benefits both).
4. **Regional ISPs** — sit between small access networks and the big Tier-1s, aggregating traffic.
5. **Content provider networks** — companies like Google, Meta, and Akamai build their *own* private backbone to carry their traffic close to users, often bypassing Tier-1/regional ISPs entirely.

**The resulting real-world hierarchy** (top to bottom):
```
   Tier-1 ISP  <—peering (IXP)—>  Tier-1 ISP  <—>  Google's private network
        |
   Regional ISP
        |
   Access ISP  (your home/college connects here)
```
- **Tier-1 ISPs** (AT&T, NTT, Tata Communications, T-Mobile...) have national/international reach and don't pay anyone above them for transit.
- **Content provider networks** (Google, Meta, Amazon) run private backbones connecting *their own* data centers straight into the Internet's core.

### 4.1 NIXI — peering at scale (India example)

**NIXI (National Internet Exchange of India)**, a non-profit under MeitY (est. 2003), runs ~77 exchange nodes across India (Delhi, Mumbai, Chennai, Bengaluru, Hyderabad, etc.).

- **Before local peering**: a request from Bengaluru to Chennai could physically round-trip via Singapore or the US — a long, wasteful detour purely because there was no local interconnection.
- **With NIXI**: Indian ISPs exchange India-to-India traffic locally, cutting latency and cost. NIXI now handles **over 1.5 Tbps** of peak traffic.
- You can verify this yourself: `traceroute` from a campus machine to another Indian site and see whether the path stays within India.

### 4.2 CDNs — why Netflix doesn't stream from California

In practice, the **access network — not the backbone — is almost always the end-to-end bottleneck**. A **Content Delivery Network (CDN)** solves this by placing copies of popular content physically close to access ISPs.

- **Without a CDN**: request goes all the way to an origin server in the US → ~230 ms round trip for a user in Bengaluru.
- **With a CDN** (e.g., Netflix Open Connect, Akamai, Cloudflare, Google Global Cache): a cache box sits *inside* the ISP's own data center (even inside Jio's or Airtel's network in India) → ~8 ms round trip.

*(Numbers are illustrative — the point is the order-of-magnitude drop from "crossing an ocean" to "hopping to a nearby edge cache.")*

---

## 5. Performance: Delay, Loss & Throughput

### 5.1 The four sources of nodal delay

For each router (node) a packet passes through:

$$d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$$

| Component | What it is | Typical size | Depends on |
|---|---|---|---|
| $d_{proc}$ — processing delay | Router checks bit errors, decides output link | microseconds | Router hardware speed |
| $d_{queue}$ — queueing delay | Packet waits in buffer for its turn on the output link | µs to ms (highly variable) | Congestion level |
| $d_{trans}$ — transmission delay | Time to *push* all L bits of the packet onto the link | $L/R$ | Packet length L, link rate R |
| $d_{prop}$ — propagation delay | Time for a bit to physically travel the link | $d/s$ | Physical distance d, propagation speed s (~2×10⁸ m/s in most media) |

**Transmission delay vs. propagation delay — don't confuse them:**

| | Transmission Delay | Propagation Delay |
|---|---|---|
| What it measures | Time to *push* the packet out | Time for a bit to *travel* the wire |
| Depends on | Packet length & link rate | Physical distance between routers |
| Independent of | Distance | Packet length & link rate |

### 5.2 The caravan analogy (for transmission vs. propagation delay)

Picture a **10-car caravan** (= a 10-bit packet) approaching a **toll booth** (= a router), then driving 100 km to the next toll booth.

- Cars "propagate" at 100 km/hr.
- The toll booth takes 12 seconds to service (let through) each car.
- **Time to push the whole caravan through the booth** = 12 sec × 10 cars = **120 sec** (this is the transmission delay).
- **Time for one car to travel the 100 km** = 100 km ÷ 100 km/hr = **1 hr = 60 min** (this is the propagation delay).
- Because of store-and-forward, the *entire* caravan must clear the first booth before the front car reaches the second booth's queue — so the total time until the caravan is fully lined up at the second booth is **120 sec + 60 min = 62 minutes**.

**Twist**: if cars now propagate at 1000 km/hr and the booth takes 1 minute per car, do cars arrive at booth 2 before all cars finish being serviced at booth 1? **Yes** — after 7 minutes the first car reaches booth 2, while 3 cars are still waiting at booth 1. This shows that transmission and propagation delay can overlap in time (the first bits of a packet may already be propagating down link 2 while the tail of the packet is still being pushed out on link 1, in some architectures) — the two delays are genuinely independent quantities.

**Which delay wins? A rule of thumb:**
> Downloading a 5 MB image: (A) a 1 Gbps fiber link 12,000 km away, vs. (B) a 1 Mbps dial-up link next door.
> - **Scenario A**: bandwidth is huge, so the transmission delay is tiny — but the physical distance is enormous, so **propagation delay dominates**.
> - **Scenario B**: distance is negligible, but the link is slow, so **transmission delay dominates**.
>
> Rule of thumb: **short link / slow link → transmission delay wins. Long link / fast link → propagation delay wins.**

### 5.3 Queueing delay & traffic intensity

Let *R* = link bandwidth (bps), *L* = average packet length (bits), *a* = average packet arrival rate (packets/sec).

**Traffic intensity**: $I = \dfrac{La}{R}$

- $I > 1$: bits are arriving faster than they can be serviced → **queue grows without bound, delay → ∞**.
- $I \to 0$: queue is almost always empty, delay is small.
- Design rule: **keep traffic intensity ≤ 1**, ideally comfortably below it.

**Queueing delay formula** (for $I < 1$):
$$d_{queue} = \frac{I \cdot L}{R(1-I)} = d_{trans} \times \frac{I}{1-I}$$

Notice $L/R$ is exactly the transmission delay — so queueing delay is just the transmission delay **scaled by a "congestion multiplier"** $\frac{I}{1-I}$, which depends *only* on how close you are to saturation:

| I (intensity) | Multiplier I/(1−I) | Meaning |
|---|---|---|
| 0 | 0 | No wait |
| 0.5 | 1× | Delay = 1 transmission time |
| 0.8 | 4× | Delay = 4 transmission times |
| 0.9 | 9× | Delay = 9 transmission times |
| 0.99 | 99× | Delay = 99 transmission times |
| →1 | →∞ | Delay blows up |

> **Worked example:** R = 10 Mbps, L = 1000 bits → $d_{trans}$ = 100 µs.
> - At a = 1000 pkt/s: I = 0.1 → $d_{queue}$ ≈ 11.1 µs.
> - Raise arrivals 9× to a = 9000 pkt/s: I = 0.9 → $d_{queue}$ = 900 µs.
> A **9× increase in arrival rate produced an 81× increase in delay** — this non-linear blow-up near I = 1 is the single most important intuition about queueing: *congestion doesn't degrade gracefully, it collapses sharply.*

> **Worked example (from the interactive exercises):** R = 1,900,000 bps, L = 4700 bits.
> - a = 35 pkts/s → I = La/R = 4700×35/1,900,000 = 0.0865 → $d_{queue}$ = 0.23 ms.
> - a = 77 pkts/s → I = 0.19 → $d_{queue}$ = 0.58 ms.
> Notice more than doubling the arrival rate (35→77) more than doubled the delay — the non-linearity is already visible even at low intensity.

### 5.4 Throughput

**Throughput** = rate (bits/time unit) at which bits actually flow from sender to receiver.

- If a file crosses links with different rates $R_s$ (server-side) and $R_c$ (client-side), the end-to-end throughput is the **minimum** of the two: $\text{Throughput} = \min\{R_s, R_c\}$. This slowest link is the **bottleneck link**.

> **Worked example:** F = 32 million bits, $R_s$ = 2 Mbps, $R_c$ = 1 Mbps.
> Transfer rate = min(2,1) = 1 Mbps → Time = 32×10⁶ / 1×10⁶ = **32 seconds**.

> **Worked example (shared backbone link):** 10 client–server pairs share a backbone link R = 5 Mbps, with per-connection $R_s$ = 2 Mbps, $R_c$ = 1 Mbps.
> If the 10 connections *fairly* share R, each gets R/10 = 0.5 Mbps of the shared link — which is now smaller than either $R_s$ or $R_c$, so **each download's throughput drops to 500 kbps**, even though each individual access link could do more. This models exactly why a household's Netflix stream slows down when many devices are streaming simultaneously on a shared backbone.

> **Worked example (bottleneck + link utilization):** Four client–server pairs share a middle backbone link R = 300 Mbps; each server link $R_s$ = 50 Mbps; each client link $R_c$ = 60 Mbps.
> - Fair share of shared link per pair = 300/4 = 75 Mbps.
> - Max achievable end-to-end throughput per pair = min($R_s$, $R_c$, 75) = **50 Mbps** (the server link is the bottleneck).
> - Server-link utilization = 50/50 = **1.0** (fully used).
> - Client-link utilization = 50/60 = **0.83**.
> - Shared-link utilization = 50/75 = **0.67**.

### 5.5 Packet loss

Loss occurs when a packet arrives at a **full buffer** — there's no free space to queue it, so it's simply dropped. A lost packet might be retransmitted by the source, by a previous node, or not at all (depends on the protocol above).

---

## 6. Protocol Layers & Reference Models

### 6.1 Why layer at all? The airline analogy

Networks are complicated: hosts, routers, links, applications, protocols, hardware, and software all interact. **Layering** imposes explicit structure so we can reason about one piece at a time.

Think of **air travel**: ticketing, baggage, gates, runway takeoff/landing, and airplane routing are each handled by a separate **service**, and each layer only relies on the service *below* it, without needing to know its internal details.

- **Modularity**: if the "gate service" changes its boarding procedure, nothing about ticketing or baggage needs to change — the change is *transparent* to the rest of the system.
- This mirrors software engineering's separation of concerns: a change inside one module shouldn't ripple through the whole codebase.

### 6.2 The Internet protocol stack (5 layers)

| Layer | Job | Example protocols | Unit of data |
|---|---|---|---|
| **Application** | Supports network apps; gives them access to network resources | HTTP, SMTP, IMAP | Message |
| **Transport** | Process-to-process data transfer: segmentation, sockets, connections, flow/error control | TCP, UDP | Segment |
| **Network** | Routes datagrams from source to destination host: addressing, routing | IP, routing protocols | Datagram |
| **Link** | Data transfer between *neighboring* network elements: framing, addressing, flow/error control | Ethernet, 802.11 (Wi-Fi), PPP | Frame |
| **Physical** | Moves individual bits "on the wire" | — | Bit |

### 6.3 The OSI model (7 layers)

The OSI (Open Systems Interconnection) model, introduced by ISO in the late 1970s, adds **two extra layers** the Internet stack doesn't formally include:

| OSI Layer | Extra beyond TCP/IP stack |
|---|---|
| **Presentation** | Lets applications interpret data meaning (encryption, compression, machine-specific format conversions) |
| **Session** | Synchronization, checkpointing, and recovery of a data exchange |

The Internet stack simply pushes these responsibilities *up into the application* if a given app needs them (e.g., HTTPS handles "encryption" itself via TLS rather than a dedicated presentation layer).

**Grouping OSI's 7 layers:**
- **User support layers** (Application, Presentation, Session) — closest to the human/application.
- **Network support layers** (Network, Data Link, Physical) — closest to the wire.
- **Transport** sits in the middle as "the heart" connecting the two groups.

### 6.4 Encapsulation — the Matryoshka doll analogy

As a message travels *down* the sender's protocol stack, each layer **wraps** the data from the layer above inside its own header — exactly like **nesting Russian Matryoshka dolls**, one inside the next:

```
Application:  M                       (message)
Transport:    [Ht | M]                (segment)
Network:      [Hn | Ht | M]           (datagram)
Link:         [Hl | Hn | Ht | M]      (frame)
Physical:     ... bits on the wire ...
```

- **Transport layer** wraps message M with header $H_t$ → forms a **segment**. ($H_t$ is used by the transport protocol, e.g. TCP, to implement reliability/ordering.)
- **Network layer** wraps the segment with header $H_n$ → forms a **datagram**. ($H_n$ carries source/destination IP addresses, used for routing.)
- **Link layer** wraps the datagram with header $H_l$ → forms a **frame**. ($H_l$ carries MAC addresses, used for delivery to the next physical neighbor.)

At the receiver, the process runs in reverse — **decapsulation**: each layer strips off its corresponding header and hands the remaining payload up to the layer above, like un-nesting the dolls one shell at a time, until only the original message M is left.

**End-to-end view**: intermediate devices only decapsulate/re-encapsulate as far as *their own* layer needs to go:
- A **switch** (link-layer device) only needs to look at $H_l$ to forward a frame — it never needs to open the network or transport headers.
- A **router** (network-layer device) must decapsulate up through $H_n$ to read the destination IP address, then re-encapsulate in a new link-layer frame for the next hop.

### 6.5 OSI vs. TCP/IP — side by side

| OSI (7 layers) | Internet / TCP-IP (5 layers) |
|---|---|
| Application | Application |
| Presentation | *(folded into Application)* |
| Session | *(folded into Application)* |
| Transport | Transport |
| Network | Network |
| Data Link | Link |
| Physical | Physical |

---

## 7. Network Devices

Every device in a network can be pinned to a specific OSI layer based on what information it inspects to do its job.

| Device | OSI Layer | Core job |
|---|---|---|
| **Repeater** | Physical | Regenerates a weakening/corrupted signal so it can travel farther |
| **Hub** | Physical | Connects multiple devices, **broadcasts** everything to everyone, shared bandwidth, no intelligence, half-duplex |
| **Modem** | Physical | Converts digital ↔ analog signals (modulation/demodulation) |
| **Bridge** | Data Link | 2-port device joining two segments; filters/forwards based on MAC addresses; learns MAC-to-port mappings |
| **Switch** | Data Link | "Intelligent multiport bridge" — stores MAC address table, forwards only to the intended port, full-duplex, reduces collisions |
| **NIC** | Data Link | The network interface card itself — holds the device's MAC address, handles send/receive, error detection |
| **Router** | Network | Forwards packets based on **IP address**, maintains a dynamically-updated routing table; also does **NAT** (translating private ↔ public IP addresses) |
| **Gateway** | Any layer | Connects two *differently configured* networks; acts as a protocol converter |
| **Firewall** | Transport/Application | Inspects traffic and allows/blocks it based on security rules |

**"Beyond the basics" — modern devices:**

| Device | OSI Layer | Role |
|---|---|---|
| **Access Point (AP)** | Layer 2 | Bridges wireless clients onto the wired LAN (functionally a wireless hub/bridge) |
| **Wireless LAN Controller (WLC)** | Management | Centrally manages many APs across an office/campus — handles roaming, channel planning |
| **L3 Switch** | Layer 2 + 3 | Combines a switch and a router in one box — switches *within* a VLAN, routes *between* VLANs. This is the real-world default in campus/data-center networks today. |

### 7.1 Hub vs. Switch — the key distinction

| | Hub | Switch |
|---|---|---|
| Sends data to | **All** connected devices | Only the intended device (by MAC address) |
| Layer | Physical | Data Link |
| Collision domain | One shared domain | One domain **per port** |
| Bandwidth | Shared | Dedicated per device |
| Intelligence | None | Stores MAC address table |

**Analogy**: a hub is like shouting an announcement into a crowded room and hoping the right person hears it; a switch is like handing a sealed, addressed envelope directly to the one person it's meant for.

### 7.2 Switch vs. Router — the key distinction

| | Switch | Router |
|---|---|---|
| Connects | Devices **within** the same network | **Multiple** networks together |
| Uses | MAC addresses | IP addresses |
| Layer | Data Link | Network |
| Broadcast domain | Stays within one domain | Breaks up / creates separate domains |
| Typical use | Expanding a LAN | Connecting a LAN to the Internet, or to other LANs |

---

## 8. Application Layer Fundamentals

### 8.1 Client-server vs. peer-to-peer (P2P)

**Client-Server:**
- Server: always-on, permanent IP address, usually lives in a data center for scalability.
- Clients: contact the server; may be intermittently connected; may have dynamically-changing IPs; clients **don't talk directly to each other**.
- Examples: HTTP, IMAP, FTP.

**Peer-to-Peer (P2P):**
- No always-on central server — arbitrary end systems talk directly to each other.
- Peers request services from *and* provide services to other peers.
- **Self-scaling**: every new peer that joins brings both new demand *and* new capacity — unlike client-server, where adding users only adds load.
- Downsides: peers connect/disconnect unpredictably, change IPs → complex to manage.
- Example: BitTorrent file sharing.

### 8.2 Processes, sockets, and addressing

A **process** is a program running within a host. Two processes on the *same* host talk via inter-process communication (managed by the OS); processes on *different* hosts talk by exchanging messages over the network.

- **Client process**: initiates the communication.
- **Server process**: waits to be contacted.
- (Note: P2P apps have processes that act as *both* client and server.)

**Sockets — the "door" analogy:**
> A socket is like the *door* of a house. The sending process shoves its message out through the door, trusting the "transport infrastructure" (the postal system/road network outside) to carry it to the receiving process's door. There are always two doors involved — one on each side of the exchange. Everything on the *house* side of the door (the socket) is controlled by the app developer; everything on the *street* side (transport, network, link) is controlled by the operating system.

**Addressing a process** requires more than just a host's IP address, because **many processes can run on one host simultaneously**. The full identifier is:
$$\text{Process address} = \text{IP address} + \text{Port number}$$

Example: to reach the HTTP server at `gaia.cs.umass.edu`, you need IP `128.119.245.12` **and** port `80`. (Well-known ports: HTTP = 80, mail/SMTP = 25.)

### 8.3 What defines an application-layer protocol?

1. **Types of messages exchanged** (e.g., request vs. response).
2. **Message syntax** — what fields exist and how they're delimited.
3. **Message semantics** — what the information in each field *means*.
4. **Rules** for when/how a process sends and responds to messages.

Protocols are either:
- **Open** (defined in RFCs, anyone can implement them → interoperability) — e.g., HTTP, SMTP.
- **Proprietary** (controlled by one vendor) — e.g., early Skype.

### 8.4 What transport service does an app need?

| Requirement | Example apps that need it | Example apps that can skip it |
|---|---|---|
| **Reliability** (no data loss) | File transfer, Web pages, email | Audio calls tolerate some loss |
| **Throughput guarantee** | Multimedia needs a *minimum* bitrate to be usable | "Elastic" apps (file transfer) just use whatever they get |
| **Low timing / latency** | Internet telephony, interactive games | Non-real-time apps don't care |
| **Security** | Anything needing encryption/integrity | — |

| Application | Data loss tolerance | Throughput need | Time-sensitive? |
|---|---|---|---|
| File transfer/download | No loss | Elastic | No |
| E-mail | No loss | Elastic | No |
| Web documents | No loss | Elastic | No |
| Real-time audio/video | Loss-tolerant | Audio: 5 Kbps–1 Mbps; Video: 10 Kbps–5 Mbps | Yes, 10s of ms |
| Streaming audio/video | Loss-tolerant | Same as above | Yes, a few seconds |
| Interactive games | Loss-tolerant | Kbps+ | Yes, 10s of ms |
| Text messaging | No loss | Elastic | Sometimes |

### 8.5 TCP vs. UDP

| | TCP | UDP |
|---|---|---|
| Reliability | Reliable (guarantees delivery) | Unreliable — "best effort" |
| Flow control | Yes — sender won't overwhelm receiver | No |
| Congestion control | Yes — throttles sender if network is overloaded | No |
| Connection | Connection-oriented (setup required) | Connectionless |
| Timing/throughput guarantee | No | No |
| Security | No (needs TLS on top) | No |

**Mapping common apps to their transport protocol:**

| Application | Application protocol | Transport protocol |
|---|---|---|
| File transfer | FTP | TCP |
| E-mail | SMTP | TCP |
| Web documents | HTTP 1.1 | TCP |
| Internet telephony | SIP, RTP, or proprietary (Skype) | TCP or UDP |
| Streaming audio/video | DASH | TCP |
| Interactive games | Proprietary (WOW, FPS) | UDP or TCP |

---

## 9. The Web, HTTP & HTTPS

### 9.1 Basics

A **web page** = one base HTML file + several referenced **objects** (images, scripts, etc.), each identified by its own URL. E.g., an HTML file with 5 embedded JPEGs = **6 objects total**.

`www.someschool.edu/someDept/pic.gif` → host name + path name.

**HTTP (HyperText Transfer Protocol)**:
- The Web's application-layer protocol, defined in RFC 1945 / RFC 2616.
- **Client/server model**: browser = client (requests & displays objects); web server = server (responds with objects).
- **Runs over TCP**: client opens a TCP connection to the server (typically port 80), exchanges HTTP messages, then the connection closes.
- **HTTP is stateless**: the server keeps *no memory* of past client requests. Each request stands alone.
  - *Why this matters*: protocols that *do* maintain state are much more complex — if a client or server crashes mid-transaction, both sides' views of "state" could become inconsistent and need reconciling. Statelessness sidesteps that whole problem, at the cost of needing a separate mechanism (cookies) if you *do* want to remember something about a user.

### 9.2 Non-persistent vs. persistent HTTP connections

**Non-persistent HTTP**: open a TCP connection → send **at most one** object → close the connection. Downloading N objects needs N separate TCP connections (each with its own handshake overhead).

**Persistent HTTP (HTTP/1.1)**: one TCP connection stays open and **multiple objects** can be sent over it before it's closed.

**Response time formula:**
$$\text{Non-persistent HTTP response time (per object)} = 2 \times RTT + \text{file transmission time}$$
(1 RTT to set up the TCP connection, 1 RTT for the request + first bytes of the response.)

> **Worked example** (webpage = 1 HTML file + 6 images, RTT = 80 ms, negligible transmission time):
>
> | Approach | RTT calculation | Total RTTs | Total time | Relative speed |
> |---|---|---|---|---|
> | Non-persistent HTTP | 7 objects × 2 RTT | 14 RTT | **1120 ms** | 1.0× (baseline, slowest) |
> | Persistent, no pipelining | 1 (TCP) + 7 (req/resp per object) | 8 RTT | **640 ms** | 1.75× faster |
> | Persistent **with** pipelining | 1 (TCP) + 1 (HTML) + 1 (all 6 images batched) | 3 RTT | **240 ms** | 4.67× faster |
>
> **Why pipelining wins so much**: without pipelining, the client must wait for each response before requesting the next object — one RTT per object. With pipelining, the client fires off requests for *all* discovered images back-to-back without waiting, collapsing 6 separate round trips into essentially 1.

### 9.3 HTTP message formats

**Request message** — plain ASCII, human-readable:
```
GET /index.html HTTP/1.1\r\n
Host: www-net.cs.umass.edu\r\n
User-Agent: Firefox/3.6.10\r\n
Accept: text/html,application/xhtml+xml\r\n
Accept-Language: en-us,en;q=0.5\r\n
Connection: keep-alive\r\n
\r\n
```
General structure: `method  URL  version \r\n` (request line) → header lines (`field: value \r\n` each) → blank `\r\n` line → optional entity body.

**Other HTTP methods:**
| Method | Purpose |
|---|---|
| **GET** | Retrieve an object; can also smuggle small data in the URL after `?` (e.g. `...?monkeys&banana`) |
| **POST** | Send form/user data in the request's *entity body* |
| **HEAD** | Ask for just the headers that a GET would return, without the body |
| **PUT** | Upload a file, completely replacing whatever exists at that URL |

**Response message:**
```
HTTP/1.1 200 OK\r\n
Date: Sun, 26 Sep 2010 20:09:20 GMT\r\n
Server: Apache/2.0.52 (CentOS)\r\n
Content-Length: 2652\r\n
Content-Type: text/html; charset=ISO-8859-1\r\n
\r\n
<data data data...>
```
Structure: status line (protocol, status code, status phrase) → header lines → blank line → data.

**Common status codes:**
| Code | Meaning |
|---|---|
| 200 OK | Request succeeded |
| 301 Moved Permanently | Object moved; new location given in `Location:` header |
| 400 Bad Request | Server couldn't parse the request |
| 404 Not Found | Requested document doesn't exist on this server |
| 505 HTTP Version Not Supported | — |

---

## 10. Cookies & Web Caching

### 10.1 Cookies — giving a stateless protocol a memory

Since HTTP itself is stateless, **cookies** are the mechanism used to layer state on top. Four moving parts:
1. A `Set-Cookie:` header line in the server's HTTP **response**.
2. A `Cookie:` header line in the client's subsequent HTTP **request**.
3. A **cookie file** kept on the user's machine, managed by the browser.
4. A **backend database** at the website mapping cookie IDs to user data.

**How it starts**: when a new visitor's first request arrives, the site generates a unique ID (the cookie) and stores an entry for it in its backend DB. Every future request from that browser carries the cookie, letting the site "recognize" the returning visitor — much like a coat-check ticket: you don't have to re-describe your coat every time, you just show the numbered ticket.

**First-party vs. third-party cookies:**
- **First-party**: set by the site you *chose* to visit (e.g., nytimes.com remembering your login).
- **Third-party (tracking cookies)**: set by an embedded advertiser (e.g., AdX.com) that appears across *many* different sites. Because the same ad network is embedded on nytimes.com **and** socks.com, it can stitch together a single browsing profile across sites you never chose to visit directly — this is how you get eerily "targeted" ads. Many browsers now disable/restrict third-party cookies by default because of this privacy implication.

**What cookies are used for**: tracking browsing history, remembering logins, visit counters, shopping carts, recommendations, saved coupons.

### 10.2 Web caches (proxy servers)

**Goal**: satisfy a client's request without bothering the origin server at all, if possible.

A web cache sits **between** client and origin server, acting as *both* a server (to the client) and a client (to the origin). If the requested object is already cached, it's returned immediately; otherwise, the cache fetches it from the origin, stores a copy, *then* returns it.

**Why cache?**
- **Speed**: cache is physically closer to the client → shorter RTT.
- **Bandwidth savings**: reduces traffic on an institution's access link.
- Enables "poor" content providers to deliver content effectively without huge infrastructure.

> **Worked numerical example — the value of caching:**
> Access link rate = 1.54 Mbps, RTT to origin = 2 sec, object size = 100 Kbits, request rate = 15/sec (→ avg. data rate needed = 1.50 Mbps).
>
> **Without any fix:** access-link utilization = 1.50/1.54 ≈ **0.97** (dangerously close to saturation) → end-to-end delay balloons into *minutes* due to queueing, even though the "raw" Internet delay is only 2 sec.
>
> **Option A — buy a faster (154 Mbps) access link:** utilization drops to ~0.0097 (plenty of headroom), delay shrinks to milliseconds — but this is **expensive**.
>
> **Option B — install a local web cache (assume 40% hit rate):**
> - Only the 60% of requests that *miss* the cache use the access link → data rate on that link = 0.6 × 1.50 Mbps = 0.9 Mbps → utilization = 0.9/1.54 = **0.58**.
> - Average end-to-end delay = 0.6 × (delay from origin, ≈2.01 s) + 0.4 × (delay from cache, ≈ms) ≈ **1.2 seconds**.
>
> **Conclusion**: the cache achieves *lower* delay than the expensive faster link, at a *fraction* of the cost — this is the central economic argument for caching infrastructure (and, by extension, for CDNs).

### 10.3 Conditional GET — browser-side caching

**Goal**: don't re-download an object the browser *already has*, if it hasn't changed.

- The browser's request includes `If-modified-since: <date>` (the date of its cached copy).
- If the object hasn't changed since that date, the server replies **`HTTP/1.0 304 Not Modified`** with **no body** — saving the full transmission delay and link bandwidth.
- If it *has* changed, the server sends the usual **`200 OK`** with the fresh object.

This is like calling ahead to ask "has anything changed since I last checked?" instead of re-reading the entire document every single time.

---

## 11. HTTP Evolution: HTTP/1.1 → HTTP/2 → HTTP/3

### 11.1 The problem HTTP/1.1 pipelining still has: Head-of-Line (HOL) blocking

HTTP/1.1 introduced pipelining (multiple GETs over one TCP connection), but the server still responds strictly **in-order (FCFS)**. If a large object is requested just before a small one, the small object must wait behind the large one — even though it could have finished first. Retransmitting a lost TCP segment also stalls *everything* behind it on that connection.

### 11.2 HTTP/2 (RFC 7540, 2015)

- Methods, status codes, and most headers are unchanged from HTTP/1.1.
- Objects are broken into **frames**, and the server can interleave/schedule frames based on **client-specified priority**, rather than strict arrival order — mitigating HOL blocking.
- The server can even **push** objects the client hasn't explicitly requested yet (anticipating what it will need next).

### 11.3 HTTP/3 — moving off TCP entirely

HTTP/2 still runs over a **single TCP connection**, so:
- A lost packet anywhere still stalls *all* objects on that connection (this is why browsers historically opened *multiple parallel* TCP connections under HTTP/1.1 — to work around exactly this).
- Plain HTTP/2 over TCP has no built-in security.

**HTTP/3** solves both by running over **UDP** instead, adding its own security and **per-object** error/congestion control — so one lost packet only stalls the *one object* it belongs to, not the whole connection.

---

## 12. Securing HTTP: TLS/SSL

### 12.1 HTTP vs. HTTPS

**HTTPS = HTTP + TLS encryption.** All communication between browser and server is encrypted, bidirectionally. Runs on **port 443** (vs. HTTP's port 80). Based on **public/private-key cryptography**: the public key encrypts, only the matching private key can decrypt.

### 12.2 What TLS actually provides

| Property | How it's achieved |
|---|---|
| **Confidentiality** | Symmetric encryption — eavesdroppers can't read the data |
| **Integrity** | Cryptographic hashing — detects if data was altered in transit |
| **Authentication** | Public-key cryptography — proves you're really talking to the genuine server, not an impostor |

### 12.3 SSL → TLS: a brief history

| Protocol | Year | Status |
|---|---|---|
| SSL 1.0 | — | Never publicly released (major flaws) |
| SSL 2.0 / 3.0 | 1995 / 1996 | Later broken by critical vulnerabilities |
| TLS 1.0 / 1.1 | 1999 / 2006 | Now deprecated |
| TLS 1.2 / 1.3 | — | The only secure, actively-used standards today |

Note: an "SSL Certificate" today almost always actually runs on **TLS** — the name "SSL" persists purely out of 30 years of marketing/consumer familiarity, not technical accuracy.

### 12.4 The digital certificate — "a passport for a server"

A server's certificate (technically an **X.509 certificate**) works like a **digital passport**, containing:
- Domain name (e.g., `bank.com`)
- The server's public key
- Issuer info (which Certificate Authority — CA — issued it)
- Expiration date
- The CA's digital signature (an encrypted hash, proving the CA vouches for this certificate)

**How authentication actually works:**
1. **Certificate presentation** — the server sends its certificate (containing its public key + the CA's signature).
2. **CA trust verification** — the browser checks that signature against its built-in list of trusted root CAs (DigiCert, Let's Encrypt, etc.).
3. **Proof of ownership** — the server signs a unique handshake message using its **private** key, to prove it *actually holds* the private key matching the certificate (and isn't just replaying a stolen public certificate).
4. **Public key verification** — the browser decrypts that signature using the certificate's public key; if it decrypts cleanly, the server's identity is confirmed.

### 12.5 What changed from SSL to modern TLS

| Aspect | Old (SSL / early TLS) | Modern (TLS 1.3) |
|---|---|---|
| Handshake speed | 2-RTT (multiple round trips) | **1-RTT** (key exchange parameters sent in the very first message) |
| Cryptography | Allowed weak ciphers (RC4, RSA key transport, MD5) | Only strong **AEAD** ciphers (AES-GCM, ChaCha20) |
| Forward secrecy | Not guaranteed — a stolen server private key years later could retroactively decrypt *old* recorded traffic | **Mandatory** — uses ephemeral (temporary, per-session) Diffie-Hellman keys, so a later-stolen key can't unlock past sessions |
| Certificate visibility | Sent in **plain text** — anyone sniffing Wi-Fi could see which site's certificate was exchanged | **Encrypted** — the temporary session key is set up *before* the certificate is sent, hiding it from eavesdroppers |

### 12.6 TLS 1.3 handshake — 1-RTT

1. **Client Hello**: client sends supported cipher suites AND *guesses* a key-agreement protocol/parameters (e.g., Diffie-Hellman), sending its own key share proactively — no need to wait for the server to ask.
2. **Server Hello**: server picks a cipher suite + key agreement parameters, sends its **signed certificate** (now encrypted, since the shared secret is already established), and computes the session key.
3. **Client**: verifies the server's certificate, generates the matching key, and can immediately make its application request (e.g., an HTTPS GET) — all in essentially **one round trip**.

A **"cipher suite"** is a bundled set of cryptographic algorithms (for key generation, encryption, message authentication, digital signatures) negotiated together as a package.

### 12.7 TLS 1.3 — 0-RTT (for *returning* visitors)

If the client has a **Pre-Shared Key (PSK)** left over from a previous connection, it can send **encrypted application data (e.g., `GET /home`) in the very first packet**, resuming the earlier session with essentially zero added latency.

⚠️ **Trade-off**: 0-RTT data is vulnerable to **replay attacks** (an attacker can capture and resend that first encrypted packet). It's generally considered acceptable only for requests that don't change server state (like a simple GET), not for anything like a payment or state-modifying POST.

---

## 13. Quick-Reference Formula Sheet

| Quantity | Formula |
|---|---|
| Transmission delay | $d_{trans} = L/R$ |
| Propagation delay | $d_{prop} = d/s$ |
| Total nodal delay | $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$ |
| Traffic intensity | $I = La/R$ |
| Queueing delay (for I < 1) | $d_{queue} = \dfrac{IL}{R(1-I)} = d_{trans} \times \dfrac{I}{1-I}$ |
| End-to-end throughput (single path) | $\min\{R_1, R_2, ..., R_n\}$ (the bottleneck link) |
| Link utilization | $\dfrac{\text{throughput actually carried}}{\text{link capacity}}$ |
| Non-persistent HTTP response time | $2 \times RTT + \text{transmission time}$ |
| Persistent HTTP, no pipelining (N objects) | $(1 + N) \times RTT$ (roughly) |
| Persistent HTTP, with pipelining | $\approx 3 \times RTT$ (1 for TCP setup, 1 for base file, 1 for all remaining objects batched) |

---

### How the pieces fit together (big picture)

A request for a web page in Bengaluru touches almost every topic above in sequence:
1. Your laptop is an **end system** on the **network edge**, reaching the Internet through your **access network** (Wi-Fi → home router → ISP).
2. The HTTP request is **encapsulated** — wrapped in a TCP segment, then an IP datagram, then an Ethernet/Wi-Fi frame — before it ever leaves your NIC.
3. It's forwarded **hop by hop** through the **network core** (packet-switched, not circuit-switched), possibly through your **local ISP → regional ISP → an IXP like NIXI → a CDN edge cache** rather than crossing an ocean to the origin server.
4. Every hop adds **processing, queueing, transmission, and propagation delay** — and if a link is congested, packets may be dropped.
5. At the server, the request is **decapsulated** back up the stack, and if it's HTTPS, a **TLS handshake** first established a secure, authenticated channel.
6. The server's **HTTP response** (possibly served straight from a **cache**, or short-circuited by a **conditional GET**) comes back down through the same layered encapsulation, and your browser **decapsulates** it, renders the HTML, and repeats the process for every embedded object — as **persistent, pipelined HTTP/2 (or HTTP/3)** connections, hopefully finishing in a couple of RTTs rather than dozens.

