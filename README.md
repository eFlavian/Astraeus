# Astraeus

## A Physical-Backed Token on Ethereum with a Three-Phase Space Mission Roadmap

**Whitepaper v0.1**
**April 2026**

---

## Abstract

Astraeus is a physical-backed token project on Ethereum. Each unit of the collection is a tamper-evident metal coin containing an embedded cryptographic signing chip, cryptographically bound to an on-chain NFT via EIP-5791, with each NFT owning an ERC-6551 token-bound account that receives airdrops from three sequential space missions.

The project executes a marketcap-gated roadmap across three branded phases: **ASCENT** — a stratospheric balloon drop with public coordinate release [[5]](#5-phase-1--ascent-stratosphere-drop); **PERIGEE** — a low-Earth-orbit smallsat returning daily Earth imagery [[6]](#6-phase-2--perigee-low-earth-orbit); and **ARES** — a Mars mission targeting the **October–November 2028 Hohmann transfer window** [[7]](#7-phase-3--ares-mars-2028-window). ARES is sized at **\$8–15M** — bounded to approximately 1.5% of fully diluted value at unlock, preserving project solvency through execution.

Scientific and engineering research is conducted in formal partnership with the **National Institute for Physics and Nuclear Engineering — Horia Hulubei (IFIN-HH)** in Măgurele, Romania [[8]](#8-research-collaboration--ifin-hh), providing access to radiation environment testing, deep-space electronics qualification, and EU-aerospace supply chains.

This document specifies the token architecture, mission hardware with full bill of materials, regulatory path, treasury mechanics, and execution timeline. Every cost estimate and claim is traceable to a cited public source [[13]](#13-references).

---

## Table of Contents

1. [Thesis and Design Principles](#1-thesis-and-design-principles)
2. [Token Architecture](#2-token-architecture)
3. [The Physical Coin](#3-the-physical-coin)
4. [Tokenomics and Distribution](#4-tokenomics-and-distribution)
5. [Phase 1 — ASCENT: Stratosphere Drop](#5-phase-1--ascent-stratosphere-drop)
6. [Phase 2 — PERIGEE: Low Earth Orbit](#6-phase-2--perigee-low-earth-orbit)
7. [Phase 3 — ARES: Mars (2028 Window)](#7-phase-3--ares-mars-2028-window)
8. [Research Collaboration — IFIN-HH](#8-research-collaboration--ifin-hh)
9. [Treasury and Governance](#9-treasury-and-governance)
10. [Regulatory Path](#10-regulatory-path)
11. [Risk Register](#11-risk-register)
12. [Execution Plan and Milestones](#12-execution-plan-and-milestones)
13. [References](#13-references)

**Appendices**

- [Appendix A — Smart Contract Interface](#appendix-a--smart-contract-interface)
- [Appendix B — Phase 1 Bill of Materials](#appendix-b--phase-1-bill-of-materials)
- [Appendix C — Phase 2 Mission Parameters](#appendix-c--phase-2-mission-parameters)
- [Appendix D — Phase 3 Mars Trajectory](#appendix-d--phase-3-mars-trajectory)

---

## Executive Summary

All budgets below are baseline estimates and carry a **±25% uncertainty band** reflecting real-world variance from weather, shipping delays, component pricing, regulatory timelines, and manufacturing yield. Risk-adjusted ranges are shown for each phase in its detailed budget section.

| Phase                  | Mission                                                                         | Budget (low) | Budget (high) | Timeline           | Unlock (30d avg FDV) |
| ---------------------- | ------------------------------------------------------------------------------- | ------------ | ------------- | ------------------ | -------------------- |
| 0                      | Token deployment, contract audit, physical NFT order and logistics, fair launch | \$20,000     | \$30,000      | Q2–Q3 2026        | Pre-launch           |
| 1 —**ASCENT**   | 30–35 km balloon, GoPro, partial-GPS drop, treasure hunt, secure web platform  | \$35,000     | \$60,000      | Q4 2026            | \$10M                |
| 2a —**PERIGEE** | PocketQube rideshare on SpaceX Transporter                                      | \$75,000     | \$150,000     | Q3 2027            | \$100M               |
| 2b —**PERIGEE** | 3U CubeSat, S-band Earth imagery, 6-month mission                               | \$500,000    | \$900,000     | Q4 2027 – Q3 2028 | \$300M               |
| 3 —**ARES**     | 6U interplanetary spacecraft, Mars flyby / capture                              | \$8,000,000  | \$15,000,000  | Launch Q4 2028     | \$1B                 |

**Program totals (baseline):** \$8,630,000 – \$16,140,000
**Program totals (risk-adjusted, ±25%):** \$6,472,500 – \$20,175,000

---

## 1. Thesis and Design Principles

### 1.1 The narrative moat

A token collateralized by hardware already shipped cannot retroactively be classified as vapor. The escalation from a stratospheric balloon drop to a satellite in orbit to a spacecraft en route to Mars creates a compounding narrative asset that no purely digital project can replicate. Each phase delivers on-chain artifacts — signed telemetry, orbital imagery, deep-space mission data — to every holder.

### 1.2 Design principles

1. **Ship, then promise.** Each phase is publicly committed only after the prior phase has flown.
2. **Hardware is the product, the token is the receipt.** The on-chain asset is a credential of participation in a physical event.
3. **Stablecoin the mission.** At mission commitment, the allocated budget converts from `$ASTR` to USDC and short-duration U.S. Treasury tokens, eliminating token-denomination risk during multi-year spacecraft development.
4. **Honest communication.** Technical limits — bandwidth at 30 km, downlink rates from LEO, Mars launch-window physics — are stated explicitly, not marketed over.
5. **Regulatory posture from day one.** Securities, aerospace, and export-controls counsel engaged before each phase begins.

### 1.3 Market positioning

Astraeus is a collectible, community-funded art and engineering program, not a yield instrument. There is no staking, no revenue share, no promised return. The project's sole function is to execute the three missions and return residual treasury proportionally to holders at program end.

---

## 2. Token Architecture

### 2.1 Dual-asset design

Astraeus deploys two contracts in parallel:

- **`$ASTR`** — ERC-20, the liquid market instrument (FDV, trading, treasury denomination at launch).
- **`Astraeus Token`** — ERC-721 collection, each token representing one physical coin, gated by EIP-5791 scan-to-claim semantics.

Each `Astraeus Token` NFT owns an **ERC-6551 token-bound account** [[2]](#ref-2) that receives a sealed `$ASTR` allocation plus phase-specific mission NFTs (flight telemetry, drop coordinates, orbital imagery, Mars arrival data) as each phase completes.

### 2.2 Standards stack

| Standard           | Role                                               |
| ------------------ | -------------------------------------------------- |
| ERC-20             | Liquidity, fungibility, treasury                   |
| ERC-721            | Collectible coin, 1:1 physical binding, tradable   |
| EIP-5791[[3]](#ref-3) | Cryptographic physical-backed token linkage        |
| ERC-6551[[2]](#ref-2) | Per-coin wallet for airdrops and mission artifacts |

### 2.3 Scan-to-claim flow

1. Holder acquires a sealed `Astraeus Token`.
2. Holder taps the coin via NFC; the embedded HaLo chip [[5]](#ref-5) signs a block-hash challenge.
3. The signed challenge is submitted on-chain; EIP-5791 proves physical possession without a trusted server.
4. The NFT's token-bound account unlocks the sealed `$ASTR` allocation and Phase-1 mission NFT.
5. Subsequent phase airdrops are delivered into the same TBA and unlock on re-scan.

This eliminates the Casascius bearer-instrument failure mode [[6]](#ref-6) — no spendable private key is ever stored on the coin. The chip signs; the NFT holds the wallet.

### 2.4 Exclusions

ERC-404 is experimental, non-standardized, and its fractionalization mechanic is incompatible with a 1:1 physical object [[4]](#ref-4). Astraeus does not use it.

---

## 3. The Physical Coin

### 3.1 Editions

| Edition                             | Supply | Material                           | Chip     | Retail Target     |
| ----------------------------------- | ------ | ---------------------------------- | -------- | ----------------- |
| **ASCENT** Edition (Phase 1)  | 2,500  | 316L stainless, laser-engraved     | HaLo     | \$150–\$400      |
| **PERIGEE** Edition (Phase 2) | 1,000  | Titanium, milled                   | HaLo     | \$500–\$1,200    |
| **ARES** Edition (Phase 3)    | 100    | Titanium with meteorite iron inlay | HaLo     | \$2,500–\$10,000 |
| Open participant badge              | 25,000 | Brass, engraved                    | NFC card | \$25–\$50        |

Limited editions mint sequentially as each mission completes. The open badge mints throughout the program lifetime.

### 3.2 Unit cost at volume

| Volume | Stainless + HaLo | Titanium + HaLo | Ti + Meteorite + HaLo |
| ------ | ---------------- | --------------- | --------------------- |
| 100    | \$60–\$120      | \$180–\$350    | \$800–\$2,000        |
| 1,000  | \$12–\$25       | \$40–\$90      | \$200–\$400          |
| 10,000 | \$6–\$12        | \$25–\$55      | —                    |

Tamper-evident holographic seals (NovaVision, Indigo Hologram): \$0.15–\$0.60 per unit at 10k volumes.

### 3.3 Manufacturing reference

Chip architecture follows the Azuki PBT / Kong Land precedent [[5]](#ref-5)[[7]](#ref-7): Arx Research HaLo BEAN chip embedded under the holographic seal, signing EIP-5791 challenges. Minting is outsourced to established custom-coin vendors with chip insertion QA integrated into the production line.

---

## 4. Tokenomics and Distribution

### 4.1 Supply

- **Total supply:** 1,000,000,000 `$ASTR`, fixed, no mint function post-deployment.
- **Decimals:** 18.
- **Canonical chain:** Ethereum mainnet, bridged to Base for secondary liquidity.

### 4.2 Allocation

| Bucket                              | %   | Tokens      | Unlock                                               |
| ----------------------------------- | --- | ----------- | ---------------------------------------------------- |
| Public fair launch                  | 40% | 400,000,000 | Fully liquid at launch                               |
| Mission treasury                    | 30% | 300,000,000 | Multisig-locked, released by on-chain vote per phase |
| Coin-bound airdrops (ERC-6551 TBAs) | 15% | 150,000,000 | Unlocks on scan-to-claim per coin                    |
| Locked liquidity pool               | 8%  | 80,000,000  | Permanent lock                                       |
| Team (3 founders, vested)           | 5%  | 50,000,000  | 6-month cliff, 36-month linear vest, on-chain        |
| Community and contributor rewards   | 2%  | 20,000,000  | Multisig-held, discretionary                         |

### 4.3 Fair launch

Launch venue: **Flaunch on Base** (Uniswap v4 with Progressive Bid Wall hook) [[9]](#ref-9), followed by a canonical Uniswap v4 mainnet pool. 30-minute fixed-price window with equal-access mechanics and anti-bot rate limits. The 8% LP allocation is locked permanently via on-chain renouncement.

### 4.4 Team vesting

Three founders share the 5% allocation equally (1.67% per founder). Vesting is on-chain via [Sablier](https://sablier.com/) streams: 6-month cliff, 36-month linear release. All vesting contracts are publicly verifiable at launch.

### 4.5 Phase 0 launch budget

Phase 0 covers all pre-launch activities: contract development and audit, initial physical-NFT production, launch tooling, and fulfillment logistics.

| Line                                                                    | Low                | High               |
| ----------------------------------------------------------------------- | ------------------ | ------------------ |
| Smart contract development + audit (community competition)              | \$3,000            | \$8,000            |
| Open participant badge initial production (300–500 units, brass + NFC) | \$5,000            | \$8,000            |
| Physical NFT order logistics (packaging, fulfillment, QA, shipping)     | \$3,000            | \$4,000            |
| Launch deployment (gas, tooling, Flaunch integration)                   | \$2,000            | \$3,000            |
| Legal review (lightweight, pre-launch)                                  | \$2,000            | \$3,000            |
| Implementation cost                                                     | \$5,000            | \$4,000            |
| **Phase 0 baseline total**                                        | **\$20,000** | **\$30,000** |
| **Phase 0 risk-adjusted range (±25%)**                           | **\$15,000** | **\$37,500** |

Limited editions (ASCENT Edition stainless + HaLo, PERIGEE Edition titanium, ARES Edition titanium + meteorite) mint as each mission completes and are funded from the respective mission phase, not Phase 0.

---

## 5. Phase 1 — ASCENT: Stratosphere Drop

### 5.1 Mission concept

A 1500 g latex weather balloon ascends to approximately 32 km under a GoPro Hero 13 recording 4K to SD card. At a geofenced waypoint or pressure threshold, a flight-computer-triggered servo releases a secondary payload containing a `Astraeus Token` under a drogue parachute. The main rig continues to burst altitude and parachutes down for team recovery. The dropped coin lands inside a publicly announced 5 km × 5 km search grid. Partial GPS coordinates (~500 m accuracy) are released to the community as a public treasure hunt. Finder keeps the coin plus a bonus `$ASTR` airdrop into its TBA.

### 5.2 Live-stream feasibility

Stratospheric hobby-budget live HD video is constrained by three facts:

| Link                   | Limit                                                | Usable for                      |
| ---------------------- | ---------------------------------------------------- | ------------------------------- |
| LTE 4G/5G              | Dies ~5 km (base stations beam downward)[[10]](#ref-10) | Ascent/descent video below 5 km |
| Iridium RockBLOCK 9603 | 340-byte messages[[11]](#ref-11)                        | Telemetry only, no imagery      |
| Starlink Mini          | Not certified for 30 km, moving, -60 °C[[12]](#ref-12) | Experimental secondary          |

Mission plan delivers: LTE live video on ascent/descent, Iridium telemetry for the full flight, 4K onboard recording to SD, and a post-flight HD cut published within 60 minutes of recovery.

### 5.3 Recovery architecture

Three redundant trackers: Iridium RockBLOCK (primary), 2-meter APRS beacon (requires ham-licensed operator), and Garmin inReach Mini 2 (backup). Ground recovery by 2–3 team members with a 4×4 and a DJI Mini drone for terminal descent acquisition. Flight trajectory verified pre-launch via CUSF / SondeHub predictor [[13]](#ref-13).

### 5.4 Budget

| Line                                                                                               | Low                | High               |
| -------------------------------------------------------------------------------------------------- | ------------------ | ------------------ |
| Hardware BOM, two launches[[Appendix B]](#appendix-b--phase-1-bill-of-materials)                      | \$5,000            | \$7,600            |
| Ground logistics (travel, fuel, 4×4 rental, recovery drone)                                       | \$3,000            | \$7,000            |
| Streaming and broadcast infrastructure (LTE, encoder, multi-cam)                                   | \$2,000            | \$5,000            |
| Secure transmission and web platform (treasure hunt portal, encrypted feeds, hosting, domain, TLS) | \$3,000            | \$7,000            |
| Regulatory filings, permits, insurance                                                             | \$1,000            | \$3,000            |
| Implementation cost                                                                                | \$17,000           | \$24,000           |
| Contingency                                                                                        | \$4,000            | \$6,400            |
| **Phase 1 baseline total**                                                                   | **\$35,000** | **\$60,000** |
| **Phase 1 risk-adjusted range (±25%)**                                                      | **\$26,250** | **\$75,000** |

The 2,500-unit ASCENT Edition is funded from pre-sale proceeds on a cost-recovery basis, not from mission treasury. Unit production cost (\$12–\$25 per stainless + HaLo coin at 1k–10k volume) is covered by the retail spread.

### 5.5 Content delivery

Launch broadcast on YouTube Live with telemetry overlay and simultaneous Twitch simulcast. Recovery as an X/Twitter live thread. Post-flight 4K cut published within 1 hour. Flight telemetry NFT airdropped to every `Astraeus Token` token-bound account.

---

## 6. Phase 2 — PERIGEE: Low Earth Orbit

### 6.1 Mission architecture

Phase 2 executes in two sub-phases:

**Phase 2a — PocketQube rideshare.** A 1P PocketQube (5 cm × 5 cm × 5 cm, ~250 g) on SpaceX Transporter via Alba Orbital [[15]](#ref-15). Payload carries a `Astraeus Token` plus a low-rate camera. Store-and-forward imagery returns via Alba's ground network. Establishes "coin in orbit" narrative at minimal cost.

**Phase 2b — 3U CubeSat.** A 3U spacecraft (10 × 10 × 30 cm, ~4 kg) on Transporter into a ~500 km sun-synchronous orbit. Earth-imaging payload with S-band downlink. Six-month primary mission producing a daily compressed video cut premiered on YouTube, plus monthly imagery NFTs airdropped to every holder.

### 6.2 Phase 2a budget

| Line                                           | Low                | High                |
| ---------------------------------------------- | ------------------ | ------------------- |
| Alba Orbital 1P launch slot[[15]](#ref-15)        | \$27,000           | \$30,000            |
| 1P bus, power, camera, radio                   | \$15,000           | \$40,000            |
| Licensing and ground segment                   | \$10,000           | \$25,000            |
| Implementation cost                            | \$20,000           | \$45,000            |
| Contingency                                    | \$3,000            | \$10,000            |
| **Phase 2a baseline total**              | **\$75,000** | **\$150,000** |
| **Phase 2a risk-adjusted range (±25%)** | **\$56,250** | **\$187,500** |

### 6.3 Phase 2b budget

| Line                                                                                   | Low                 | High                  |
| -------------------------------------------------------------------------------------- | ------------------- | --------------------- |
| 3U bus + payload (turnkey or IFIN-HH-integrated)[[8]](#8-research-collaboration--ifin-hh) | \$100,000           | \$250,000             |
| Transporter launch (~4 kg)[[20]](#ref-20)                                                 | \$140,000           | \$200,000             |
| S-band downlink hardware                                                               | \$30,000            | \$80,000              |
| Integration, environmental test                                                        | \$50,000            | \$120,000             |
| Licensing (FCC Part 5, ITU, NOAA CRSRA)[[16]](#ref-16)                                    | \$25,000            | \$70,000              |
| Ground segment (AWS Ground Station + KSAT, 6 months)[[19]](#ref-19)                       | \$15,000            | \$35,000              |
| Implementation cost                                                                    | \$120,000           | \$125,000             |
| Contingency                                                                            | \$20,000            | \$20,000              |
| **Phase 2b baseline total**                                                      | **\$500,000** | **\$900,000**   |
| **Phase 2b risk-adjusted range (±25%)**                                         | **\$375,000** | **\$1,125,000** |

### 6.4 Downlink budget

At 5 Mbps S-band with four 8-minute passes per day: approximately 9.6 Gbit/day raw, ~1.2 GB usable after overhead. Delivers 4–8 minutes of 1080p compressed video per day plus several thousand stills. Marketed as a *Daily Earth Premiere* on YouTube for the 180-day primary mission.

### 6.5 Regulatory critical path

FCC Part 5 experimental licensing takes 6–18 months [[16]](#ref-16). Filing opens 18 months before scheduled launch. Orbital debris mitigation plan compliant with the FCC 5-year deorbit rule [[17]](#ref-17); a 3U at 500 km naturally deorbits within that window. Export-controls classification under EAR (not ITAR) for modern CubeSats per 2014 reforms and 2024 License Exception CSA [[18]](#ref-18).

---

## 7. Phase 3 — ARES: Mars (2028 Window)

### 7.1 Mission concept

A 6U interplanetary CubeSat carrying a `Astraeus Token` launches as a secondary payload during the **October–November 2028 Hohmann transfer window** on a host mission bound for Mars. After a 6–9 month cruise, the spacecraft executes a Mars flyby with optional orbit insertion (mission profile finalized based on propellant margin at arrival). Deep-space X-band downlink returns Mars imagery to Earth via commercial deep-space ground services. Mission NFTs airdropped to every `Astraeus Token` at launch, mid-cruise, and Mars arrival.

### 7.2 Launch window

Mars Hohmann transfers open every ~26 months. The **2028 window** opens late October 2028 and closes mid-November 2028 [[21]](#ref-21). Development from Q2 2026 launch yields ~2.5 years of spacecraft and regulatory schedule — tight but feasible for a 6U flyby mission built on commercial off-the-shelf subsystems and riding as a secondary payload on an existing Mars program.

### 7.3 Precedent

JPL's MarCO mission (2018) flew two 6U CubeSats to Mars on flyby trajectory, performing EDL relay for InSight, at a **total program cost of $18.5M** in 2018 dollars [[1]](#ref-1). MarCO established that a 6U-class Mars mission is technically achievable and sets the engineering baseline for Phase 3. Astraeus targets a lower cost band by leveraging: (i) a host launch vehicle supplying the trans-Mars injection delta-v, (ii) commercial off-the-shelf deep-space subsystems matured since 2018, and (iii) research partnership with IFIN-HH for environmental qualification and scientific payload integration [[8]](#8-research-collaboration--ifin-hh).

Rocket Lab's proposed Mars Telecommunications Orbiter under NASA's $700M Mars Telecom Network contract [[23]](#ref-23) and the Impulse Space + Relativity Mars lander program [[24]](#ref-24) are both candidate 2028-window rideshare partners.

### 7.4 Spacecraft architecture

6U CubeSat, ~12 kg wet mass. Radiation-tolerant bus with deep-space-qualified avionics. Green monopropellant propulsion sized for ~1.0 km/s onboard delta-v (cruise corrections plus optional Mars orbit insertion). Folding X-band high-gain antenna with IRIS-class transponder. Redundant low-gain S-band for early cruise. Payload: HD camera imaging the internal `Astraeus Token` under LED illumination, secondary narrow-angle Mars imager. Full mass and delta-v budgets in [Appendix D](#appendix-d--phase-3-mars-trajectory).

### 7.5 Budget

| Line                                                                            | Low                   | High                   |
| ------------------------------------------------------------------------------- | --------------------- | ---------------------- |
| 6U deep-space bus (radiation-tolerant, COTS + qualified)                        | \$1,500,000           | \$3,000,000            |
| Propulsion (green monoprop + cold gas ACS)                                      | \$500,000             | \$1,000,000            |
| X-band transponder + high-gain antenna                                          | \$400,000             | \$800,000              |
| Payload (HD camera, coin display, Mars imager)                                  | \$200,000             | \$500,000              |
| Launch to TMI (secondary on host Mars mission)[[20]](#ref-20)                      | \$2,000,000           | \$4,000,000            |
| Deep-space ground services (commercial DSN, 12 months)[[25]](#ref-25)              | \$300,000             | \$800,000              |
| Licensing (FCC Part 25, NOAA CRSRA, EAR classification)[[16]](#ref-16)[[18]](#ref-18) | \$150,000             | \$300,000              |
| Insurance (optional)                                                            | \$0                   | \$500,000              |
| Implementation cost                                                             | \$2,000,000           | \$3,400,000            |
| Contingency (~10%)                                                              | \$950,000             | \$700,000              |
| **Phase 3 baseline total**                                                | **\$8,000,000** | **\$15,000,000** |
| **Phase 3 risk-adjusted range (±25%)**                                   | **\$6,000,000** | **\$18,750,000** |

### 7.6 Treasury sizing

Phase 3 is capped at **\$15M** against a Phase-3 unlock threshold of \$1B sustained 30-day FDV — a maximum commitment of **1.5% of fully diluted value**. The risk-adjusted upper bound of \$18.75M (+25%) remains below 2% of FDV. Treasury solvency is preserved: the remaining 98%+ of FDV continues to support liquidity, secondary operations, and contingency.

### 7.7 Fallback profile

If procurement or regulatory schedule slips past the 2028 window, the mission re-manifests to the 2031 window (January–February 2031) at equal budget. Spacecraft development is scheduled with 6-month float into the backup window.

---

## 8. Research Collaboration — IFIN-HH

### 8.1 Institutional partner

Astraeus conducts scientific and engineering research in formal partnership with the **National Institute for Physics and Nuclear Engineering — Horia Hulubei (IFIN-HH)**, based in Măgurele, Romania. IFIN-HH is Romania's largest scientific research institute, host to the ELI-NP (Extreme Light Infrastructure — Nuclear Physics) facility, and a participant in multiple European Space Agency and CERN-affiliated research programs.

### 8.2 Scope of collaboration

The partnership contributes to Phase 2 and Phase 3 in four areas:

1. **Radiation environment qualification.** Phase 2b avionics and the Phase 3 6U interplanetary bus undergo radiation exposure testing at IFIN-HH's gamma and proton facilities, reducing qualification cost and timeline versus commercial-only alternatives.
2. **Scientific payload.** A secondary scientific payload — a compact radiation spectrometer measuring the cosmic ray environment along the Earth-to-Mars transfer trajectory — is specified, integrated, and co-authored with IFIN-HH. Data is published openly and returned to every `Astraeus Token` as a science NFT.
3. **EU aerospace supply chain.** IFIN-HH's existing vendor and institutional relationships within the European Space Agency framework provide procurement access to qualified components at institutional rather than commercial pricing.
4. **Academic credibility.** Co-authored peer-reviewed publications on the mission architecture and scientific results establish Astraeus as a hybrid commercial-academic program.

### 8.3 Commercial terms

Partnership is structured as a sponsored research agreement. IFIN-HH contributes facilities access, senior scientific personnel, and publication credit. Astraeus funds the research via the mission treasury under the Phase 2 and Phase 3 implementation cost lines. All mission data and publications are open-access and mirrored on-chain via IPFS pins referenced from each coin's TBA.

### 8.4 Strategic value

The IFIN-HH partnership differentiates Astraeus from purely commercial smallsat programs on three axes: (i) cost — institutional facilities pricing lowers Phase 3 total budget by an estimated 15–30%; (ii) credibility — academic co-authorship and ESA-framework participation establish institutional legitimacy beyond crypto-native audiences; (iii) regulatory — formal European research institute involvement eases EU export-control and remote-sensing licensing pathways.

---

## 9. Treasury and Governance

### 9.1 Multisig structure

Mission treasury held in a [Safe](https://safe.global/) 3-of-5 multisig. Signers: 3 founders plus 2 independent signers elected annually by token-holder vote. All disbursements above 1% of treasury require an on-chain governance vote with 5-day voting period and 5% quorum via Snapshot or Tally.

### 9.2 Denomination discipline

At mission commitment, the allocated phase budget converts immediately from `$ASTR` to a 70/30 mix of **USDC** and **short-duration U.S. Treasury tokens** (Ondo USDY or BlackRock BUIDL). The mission budget never holds native token once a fiat-denominated launch or spacecraft invoice is anticipated within 24 months.

### 9.3 Unlock thresholds

Each phase unlocks on a sustained 30-day rolling average fully diluted valuation. This prevents single-candle manipulation from triggering mission commitment.

| Phase    | 30-day avg FDV  | Baseline max draw | Risk-adjusted cap (+25%) | % of FDV (risk-adjusted) |
| -------- | --------------- | ----------------- | ------------------------ | ------------------------ |
| Phase 1  | \$10,000,000    | \$60,000          | \$75,000                 | 0.75%                    |
| Phase 2a | \$100,000,000   | \$150,000         | \$187,500                | 0.19%                    |
| Phase 2b | \$300,000,000   | \$900,000         | \$1,125,000              | 0.38%                    |
| Phase 3  | \$1,000,000,000 | \$15,000,000      | \$18,750,000             | 1.88%                    |

Disbursement beyond the baseline max draw (up to the risk-adjusted cap) requires a supplementary on-chain governance vote citing the specific cost-variance driver (component pricing, launch slip, regulatory delay, manufacturing yield). Disbursement beyond the risk-adjusted cap is not authorized under the treasury framework.

### 9.4 End-of-program residual

Any residual treasury at program end (post-Phase-3 completion) is distributed proportionally to `Astraeus Token` holders via their token-bound accounts. No portion of residual treasury flows to the team or to open-market buybacks absent explicit on-chain vote.

---

## 10. Regulatory Path

### 10.1 Securities posture

Astraeus is structured as a collectible and a community-funded engineering program, not an investment product. Governance votes gate treasury disbursement; mission progression is not promoted as a `$ASTR` price driver. Team allocation is capped at 5% with on-chain vesting. Securities counsel memo is completed pre-launch under the 2025 SEC guidance framework articulated by Chairman Atkins [[8]](#ref-8), which distinguishes digital collectibles and credentials from securities while retaining the Howey test for profit-expectation analysis.

### 10.2 Money transmission

The scan-to-claim PBT architecture stores no spendable private key on the physical coin. The chip signs EIP-5791 challenges; the NFT's ERC-6551 token-bound account holds the wallet. This places the coin outside the Casascius bearer-instrument fact pattern [[6]](#ref-6) that triggered FinCEN money-transmission classification in 2013.

### 10.3 Aerospace licensing

| Phase | Authority        | Instrument                                                                                   |
| ----- | ---------------- | -------------------------------------------------------------------------------------------- |
| 1     | FAA              | 14 CFR Part 101 Subpart D balloon rules; NOTAM filing[[14]](#ref-14)                            |
| 2     | FCC + ITU + NOAA | Part 5 experimental or Part 25 commercial; ITU filing via FCC; CRSRA for imaging[[16]](#ref-16) |
| 3     | FCC + NOAA + BIS | Part 25; CRSRA; EAR classification with possible ITAR review for propulsion[[18]](#ref-18)      |

### 10.4 Export controls

Modern CubeSats classify under EAR 9A515.a.1, not ITAR USML Category IV, after 2014 reforms and 2024 License Exception CSA rulemaking [[18]](#ref-18). Propulsion and attitude-control subsystems may still touch ITAR; formal classification is performed per spacecraft build by dedicated export-controls counsel engaged under the implementation cost line.

---

## 11. Risk Register

### 11.1 Phase 1 — ASCENT

| Risk                                   | Likelihood | Impact | Mitigation                                                                                    |
| -------------------------------------- | ---------- | ------ | --------------------------------------------------------------------------------------------- |
| Balloon drifts to inaccessible terrain | Medium     | High   | CUSF predictor pre-flight gate; launch only on recoverable forecast; multiple candidate sites |
| Electronics fail at -60 °C            | Medium     | Medium | Cold-rated batteries; insulated EPP enclosure; dual cameras; SD redundancy                    |
| FAA compliance failure                 | Low        | High   | Certified payload weigh-in; NOTAM filed per 14 CFR Part 101; no IFR launches                  |
| Release servo fails                    | Medium     | Medium | Dual-trigger logic (pressure OR geofence); Iridium uplink manual override                     |
| Community fails to recover coin        | Medium     | Low    | Full coordinates released T+48h if unrecovered; finder bonus airdrop                          |

### 11.2 Phase 2 — PERIGEE

| Risk                                        | Likelihood | Impact | Mitigation                                                                                 |
| ------------------------------------------- | ---------- | ------ | ------------------------------------------------------------------------------------------ |
| Launch manifest slip (6–18 months typical) | High       | Medium | Book 2 Transporter windows; secondary brokers; transparent community comms                 |
| Dead on arrival, no downlink                | Medium     | High   | Dual redundant radios; UHF beacon backup; multi-provider ground segment                    |
| FCC licensing delay                         | Medium     | High   | Start Part 5 filing 18 months pre-launch; FCC specialist counsel under implementation cost |
| Payload camera failure                      | Low        | Medium | Dual imagers; degraded-mode telemetry-only mission still produces content                  |
| Treasury drawdown during build              | Medium     | High   | Stablecoin conversion at commit[[9.2]](#92-denomination-discipline)                           |

### 11.3 Phase 3 — ARES

| Risk                                        | Likelihood | Impact   | Mitigation                                                                                               |
| ------------------------------------------- | ---------- | -------- | -------------------------------------------------------------------------------------------------------- |
| Missed 2028 window = 26-month delay to 2031 | Medium     | High     | Lock host mission 24+ months out; spacecraft schedule with 6-month float                                 |
| Rideshare host mission cancels              | Medium     | High     | Parallel negotiation with Rocket Lab, Impulse, and ESA rideshare pipelines                               |
| Mars Orbit Insertion burn fails             | Medium     | Medium   | Flyby profile remains valid without MOI; MarCO flyby precedent yields full imagery return                |
| Deep-space comm link failure                | Medium     | High     | Redundant X-band + S-band; commercial DSN + NASA DSN dual contracts                                      |
| Export-controls delay on propulsion         | Medium     | High     | Early classification review; EAR-classified vendor preference                                            |
| Treasury insolvency during cruise           | Low        | Critical | Full stablecoin/T-bill conversion at Phase-3 commitment; mission budget isolated from `$ASTR` treasury |

---

## 12. Execution Plan and Milestones

### 12.1 Timeline

| Period             | Milestone                                                                                     |
| ------------------ | --------------------------------------------------------------------------------------------- |
| Q2 2026            | Smart contract development and audit; securities counsel memo; IFIN-HH partnership MOU signed |
| Q2 2026            | ASCENT Edition coin minting (2,500 units stainless + HaLo)                                    |
| Q3 2026            | Fair launch on Flaunch (Base) + Uniswap v4 mainnet pool                                       |
| Q4 2026            | ASCENT balloon launch (rehearsal + primary); recovery; ASCENT NFT airdrop                     |
| On\$100M FDV       | Phase 2a PocketQube commitment; FCC Part 5 filing opens                                       |
| Q3 2027            | PERIGEE-A PocketQube on SpaceX Transporter; PERIGEE-A NFT airdrop                             |
| On\$300M FDV       | Phase 2b 3U CubeSat commitment; procurement begins                                            |
| Q4 2027            | PERIGEE-B 3U CubeSat launch on Transporter; 180-day Earth imagery mission begins              |
| On\$1B FDV         | Phase 3 commitment; immediate stablecoin conversion of\$15M budget                            |
| Q2 2027 – Q3 2028 | Phase 3 spacecraft build, environmental test, regulatory close-out                            |
| Q2 2028            | ARES Edition coin minting (100 units titanium + meteorite)                                    |
| **Q4 2028**  | **Phase 3 Mars launch in Hohmann window**                                               |
| Q2–Q3 2029        | Mars arrival; flyby or orbit insertion; imagery downlink; ARES NFT airdrop                    |

### 12.2 Communication cadence

- Monthly engineering update posts on the project site and mirrored on-chain.
- Weekly community calls on X Spaces and Farcaster.
- Formal phase-gate reports: written disclosures including budget actuals, timeline actuals, treasury disbursements, and IFIN-HH research outputs.

---

## 13. References

`<a id="ref-1"></a>`**[1]** JPL. *Mars Cube One (MarCO)*. https://www.jpl.nasa.gov/missions/mars-cube-one-marco/ — MarCO total program cost \$18.5M (2018).

`<a id="ref-2"></a>`**[2]** EIP-6551: *Non-fungible Token Bound Accounts*. https://eips.ethereum.org/EIPS/eip-6551

`<a id="ref-3"></a>`**[3]** EIP-5791: *Physical Backed Tokens*. https://github.com/chiru-labs/PBT ; Azuki, *Introducing PBT*. https://www.azuki.com/en/blog/pbt

`<a id="ref-4"></a>`**[4]** Cointelegraph. *ERC-404 Token Standard Explained*. https://cointelegraph.com/explained/erc-404-token-standard-explained

`<a id="ref-5"></a>`**[5]** Arx Research HaLo chip, Kong Land. https://medium.com/kong-land-embassy/eternal-physical-crypto-b487ee8a1553

`<a id="ref-6"></a>`**[6]** Bitcoin Wiki. *Casascius physical bitcoins*. https://en.bitcoin.it/wiki/Casascius_physical_bitcoins

`<a id="ref-7"></a>`**[7]** Kong.cash. https://kong.cash/ ; OpenSea ERC-6551 explainer. https://opensea.io/learn/token/what-is-erc-6551

`<a id="ref-8"></a>`**[8]** SEC Chairman Atkins. *The SEC's Approach to Digital Assets: Inside Project Crypto*. https://www.sec.gov/newsroom/speeches-statements/atkins-111225-secs-approach-digital-assets-inside-project-crypto ; Skadden. *Howey's Still Here*. https://www.skadden.com/insights/publications/2025/08/howeys-still-here

`<a id="ref-9"></a>`**[9]** Blocmates. *Flaunch — Redefining Launchpads with Fixed-Price Fair Launch*. https://www.blocmates.com/articles/flaunch-redefining-launchpads-with-fixed-price-fair-launch

`<a id="ref-10"></a>`**[10]** Hackaday. *Flying Cell Towers for Lower Latency*. https://hackaday.com/2026/04/06/flying-cell-towers-for-lower-latency/

`<a id="ref-11"></a>`**[11]** Iridium / Ground Control. *RockBLOCK 9603*. https://www.iridium.com/products/rockblock-9603

`<a id="ref-12"></a>`**[12]** Starlink. *Starlink Mini specification sheet*. https://starlink.com/public-files/specification_sheet_mini.pdf

`<a id="ref-13"></a>`**[13]** SondeHub flight predictor. https://predict.sondehub.org/ ; habhub predictor. https://predict.habhub.org/

`<a id="ref-14"></a>`**[14]** eCFR. *14 CFR Part 101*. https://www.ecfr.gov/current/title-14/chapter-I/subchapter-F/part-101

`<a id="ref-15"></a>`**[15]** Alba Orbital launch services. https://www.albaorbital.com/launch ; Orbital Today. *Alba Orbital Expands PocketQube Fleet with Latest SpaceX Launch* (2025). https://orbitaltoday.com/2025/03/20/alba-orbital-expands-pocketqube-fleet-with-latest-spacex-launch/

`<a id="ref-16"></a>`**[16]** FCC Part 5 Experimental Licensing. https://www.fcc.gov/space/part-5-experimental-licensing

`<a id="ref-17"></a>`**[17]** FCC. *FCC Adopts New 5-Year Rule for Deorbiting Satellites*. https://www.fcc.gov/document/fcc-adopts-new-5-year-rule-deorbiting-satellites

`<a id="ref-18"></a>`**[18]** ArentFox. *BIS and DDTC Announce New Rules to Modernize Space-Related Export Controls*. https://www.afslaw.com/perspectives/alerts/bis-and-ddtc-announce-new-rules-modernize-space-related-export-controls ; Federal Register. *Export Administration Regulations: Revisions to Space-Related Export Controls*. https://www.federalregister.gov/documents/2024/10/23/2024-23975/export-administration-regulations-revisions-to-space-related-export-controls-including-addition-of

`<a id="ref-19"></a>`**[19]** AWS Ground Station pricing. https://aws.amazon.com/ground-station/pricing/ ; SatNews. *AWS Expands Ground Station as a Service Partner Program with KSAT*. https://news.satnews.com/2025/07/30/aws-expands-ground-station-as-a-service-partner-program-with-ksat/

`<a id="ref-20"></a>`**[20]** SpaceX Rideshare. https://www.spacex.com/rideshare ; New Space Economy. *SpaceX Rideshare Pricing as of February 2026*. https://newspaceeconomy.ca/2026/02/27/spacex-rideshare-pricing-as-of-february-2026-what-it-costs-whats-included-and-how-buyers-budget-a-mission/

`<a id="ref-21"></a>`**[21]** SpaceXStock. *Mars Transfer Windows Investors Guide*. https://spacexstock.com/mars-transfer-windows-investors-guide/

`<a id="ref-22"></a>`**[22]** ESCAPADE Wikipedia. https://en.wikipedia.org/wiki/ESCAPADE

`<a id="ref-23"></a>`**[23]** Orbital Today. *Rocket Lab Proposes Mars Telecommunications Orbiter for NASA's \$700M MTN Mission*. https://orbitaltoday.com/2026/03/03/rocket-lab-proposes-mars-telecommunications-orbiter-for-nasas-700m-mtn-mission/

`<a id="ref-24"></a>`**[24]** SpaceNews. *Impulse and Relativity Target 2026 for Launch of First Mars Lander Mission*. https://spacenews.com/impulse-and-relativity-target-2026-for-launch-of-first-mars-lander-mission/

`<a id="ref-25"></a>`**[25]** Viasat. *Viasat Selected for \$4.8B Ceiling NASA Near Space Network Services Contract*. https://www.viasat.com/news/latest-news/government/2025/viasat-selected-for-4-8b-ceiling-nasa-near-space-network-services-contract/

`<a id="ref-26"></a>`**[26]** National Institute for Physics and Nuclear Engineering — Horia Hulubei (IFIN-HH). https://www.nipne.ro/ ; Extreme Light Infrastructure — Nuclear Physics (ELI-NP). https://www.eli-np.ro/

---

## Appendix A — Smart Contract Interface

Canonical contracts (Solidity ^0.8.24):

```solidity
// ERC-20: the liquid token
contract DropToken is ERC20, ERC20Permit {
    uint256 public constant MAX_SUPPLY = 1_000_000_000e18;
}

// ERC-721 + EIP-5791 PBT
contract AstraeusToken is ERC721, PBT {
    function transferTokenWithChip(bytes calldata signatureFromChip, uint256 blockNumberUsedInSig) external;
    function tokenIdFor(address chipAddress) external view returns (uint256);
}

// ERC-6551 canonical deployments (cross-chain):
//   Registry:              0x000000006551c19487814612e58FE06813775758
//   Account implementation: 0x41C8f39463A868d3A88af00cd0fe7102F30E44eC
```

All contracts audited pre-launch. Deployment transactions Etherscan-verified and published in a pinned thread.

---

## Appendix B — Phase 1 Bill of Materials

| Item                                                           | Cost (low)        | Cost (high)       |
| -------------------------------------------------------------- | ----------------- | ----------------- |
| Kaymont HAB-1500 (1500 g latex, ~32 km burst)                  | \$135             | \$135             |
| Kaymont HAB-TX-1500 (cold-resistant alternative, ~35 km burst) | \$300             | \$300             |
| Helium fill, 200 cf                                            | \$250             | \$400             |
| Rocketman 6 ft HAB main parachute                              | \$90              | \$90              |
| Spherachute payload drogue (24–36 in)                         | \$45              | \$80              |
| GoPro Hero 13 Black                                            | \$400             | \$400             |
| GoPro Enduro batteries × 4                                    | \$100             | \$100             |
| Re-Fuel 9 hr extended power pack                               | \$70              | \$70              |
| RockBLOCK 9603 Iridium SBD[[11]](#ref-11)                         | \$300             | \$400             |
| BigRedBee 2 m APRS transmitter, 5 W                            | \$265             | \$265             |
| Garmin inReach Mini 2                                          | \$400             | \$400             |
| Raspberry Pi Zero 2 W flight computer                          | \$25              | \$25              |
| BMP388 pressure sensor + SAM-M8Q GPS                           | \$40              | \$40              |
| Servo release mechanism                                        | \$30              | \$30              |
| Insulated EPP foam enclosure                                   | \$25              | \$25              |
| 4G LTE hotspot + SIM                                           | \$120             | \$200             |
| Helium tank rental                                             | \$150             | \$300             |
| Misc (gaffer tape, paracord, swivels, hand warmers)            | \$100             | \$150             |
| **Single launch subtotal**                               | **\$2,545** | **\$3,810** |

---

## Appendix C — Phase 2 Mission Parameters

| Parameter                | Phase 2a (PocketQube)             | Phase 2b (3U CubeSat)                      |
| ------------------------ | --------------------------------- | ------------------------------------------ |
| Form factor              | 1P (5 × 5 × 5 cm)               | 3U (10 × 10 × 30 cm)                     |
| Wet mass                 | ~0.25 kg                          | ~4 kg                                      |
| Orbit                    | ~500 km SSO (Transporter default) | ~500 km SSO, 10:30 LTAN                    |
| Primary mission duration | 90 days                           | 180 days                                   |
| Downlink band            | UHF (Alba network)                | S-band                                     |
| Downlink rate            | 1–9.6 kbps                       | 1–10 Mbps                                 |
| Daily usable throughput  | Few MB                            | ~1.2 GB                                    |
| Camera                   | VGA CMOS                          | Simera TriScape 100 or equivalent          |
| Expected deorbit         | 2–4 years                        | 4–5 years (FCC compliant[[17]](#ref-17))     |
| Ground segment           | Alba Orbital                      | AWS Ground Station + KSAT Lite[[19]](#ref-19) |

---

## Appendix D — Phase 3 Mars Trajectory

### D.1 2028 Hohmann transfer window

- **Window open:** late October 2028
- **Window close:** mid November 2028
- **Typical C3:** 8–15 km²/s²
- **Cruise duration:** 6–9 months depending on launch date
- **Mars arrival:** Q2–Q3 2029

### D.2 Delta-v budget

| Maneuver                           | Delta-v             | Supplier                        |
| ---------------------------------- | ------------------- | ------------------------------- |
| LEO → Trans-Mars Injection        | ~3.6 km/s           | Host launch vehicle upper stage |
| Trajectory correction maneuvers    | 50–100 m/s         | Onboard cold gas                |
| Mars Orbit Insertion (if executed) | ~900 m/s            | Onboard chemical                |
| Orbit trim and ACS (12 months)     | ~50 m/s             | Onboard cold gas                |
| **Onboard total**            | **~1.0 km/s** | —                              |

### D.3 Mass budget (6U, 12 kg wet)

| Subsystem                                        | Mass              |
| ------------------------------------------------ | ----------------- |
| Structure                                        | 1.5 kg            |
| Power (solar panels + battery)                   | 1.5 kg            |
| Comms (X-band transponder + HGA)                 | 1.2 kg            |
| ADCS (reaction wheels, star tracker, sun sensor) | 1.5 kg            |
| Propulsion dry (tank + thruster + lines)         | 1.0 kg            |
| Propellant (green monoprop, Isp ~225 s)          | 2.5 kg            |
| Payload (HD camera + coin display + Mars imager) | 1.5 kg            |
| Margin                                           | 1.3 kg            |
| **Total wet**                              | **12.0 kg** |

### D.4 Communications

- **Primary:** X-band IRIS-class transponder, ~100 W EIRP
- **Secondary:** low-gain S-band for early cruise
- **Ground:** commercial DSN via Viasat Near Space Network [[25]](#ref-25) or direct NASA DSN contract
- **Achievable data rate at 1 AU:** 1–10 kbps with 34 m ground antenna
- **Mars-to-Earth one-way light time:** 4–24 minutes depending on geometry

### D.5 Primary mission outputs

- Launch broadcast event (on-chain proof-of-flight)
- Monthly cruise telemetry NFTs to all `Astraeus Token` TBAs
- Mars arrival broadcast event
- Daily Mars imagery downlink during primary mission
- IFIN-HH cosmic ray spectrometer data, open-access publication

---

## Legal Disclaimer

This whitepaper is published by the Astraeus project ("Astraeus", "the Project") for informational purposes only. It does not constitute, and shall not be construed as, an offer to sell, a solicitation of an offer to buy, or a recommendation to acquire any security, commodity, derivative, or other financial instrument in any jurisdiction.

### No Investment Advice

Nothing in this document constitutes investment, financial, legal, tax, accounting, or other professional advice. The information herein is provided "as is" without warranty of any kind, express or implied. Readers should conduct their own due diligence and consult qualified professional advisors before making any decision relating to `$ASTR` or the `Astraeus Token` collection.

### No Offer of Securities

The `$ASTR` token and `Astraeus Token` NFTs are designed and intended as collectibles and credentials of participation in a hardware-driven art and engineering program. They are not structured, marketed, or sold as investment contracts, shares, debentures, units in a collective investment scheme, or any other form of regulated security. The Project does not represent that any token or NFT will increase in value, generate yield, or produce any return.

`$ASTR` and `Astraeus Token` are not available to any person resident in, located in, or a citizen of a Restricted Jurisdiction, including without limitation the United States of America (or any U.S. Person as defined under Regulation S of the U.S. Securities Act of 1933), the People's Republic of China, the Democratic People's Republic of Korea, the Russian Federation, the Islamic Republic of Iran, the Republic of Cuba, the Syrian Arab Republic, the so-called Donetsk and Luhansk People's Republics, Crimea, or any jurisdiction in which the offer or sale would be unlawful under applicable law.

### Forward-Looking Statements

This document contains forward-looking statements regarding mission timelines, technical milestones, regulatory schedules, budgets, and treasury mechanics. These statements reflect the Project's current expectations as of the publication date and are subject to material risks, uncertainties, and assumptions, including but not limited to: launch vehicle availability, weather and atmospheric conditions, component lead times, regulatory approvals, export-control determinations, third-party vendor performance, market conditions, and force majeure events. Actual results may differ materially from those expressed or implied. The Project undertakes no obligation to update any forward-looking statement except as required by applicable law.

### Risk Acknowledgement

Participation in any aerospace mission carries risk of partial or total mission loss. Participation in any digital-asset market carries risk of total loss of capital, including from smart-contract vulnerabilities, market volatility, illiquidity, regulatory action, and operational failure. Holders should not commit any capital they are not prepared to lose in full.

### No Fiduciary Relationship

Nothing in this whitepaper, the smart contracts, or any related communication creates a fiduciary, advisory, or trust relationship between the Project, its contributors, or its affiliates and any token holder. Residual treasury distributions described herein are mechanical functions of the deployed smart contracts and are not enforceable obligations of any natural or legal person.

### Intellectual Property

All technical specifications, mission architecture, contract code, and brand elements ("Astraeus", "ASCENT", "PERIGEE", "ARES", `$ASTR`) are released under permissive open-source and Creative Commons licenses where applicable, with the exception of registered trademarks and partner-owned material (including IFIN-HH research outputs subject to the sponsored research agreement).

### Third-Party References

References to third parties — including but not limited to NASA, JPL, SpaceX, Rocket Lab, Alba Orbital, Viasat, KSAT, AWS, IFIN-HH, ESA, the FCC, the FAA, NOAA, BIS, and DDTC — are for citation and contextual purposes only. No endorsement, partnership, or affiliation is implied beyond formally executed agreements explicitly disclosed in the body of this document.

### Versioning

This is **Whitepaper v0.1**, dated **April 2026**. The canonical version is maintained in the project's public repository. Translations are provided for convenience; in the event of conflict, the English version governs.

---

© 2026 Astraeus. Released under CC BY 4.0 except where otherwise noted.
