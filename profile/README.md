# Vyges Tools

**Open, reproducible, local-first silicon tooling — the EDA toolchain, sign-off engines, and open PDKs behind Vyges™.**

[![Sponsor Vyges](https://img.shields.io/badge/Sponsor-%E2%9D%A4-ea4aaa?logo=githubsponsors&logoColor=white&style=for-the-badge)](https://github.com/sponsors/vyges-ip)

vyges-tools is the home for the tooling that turns RTL into silicon: the Vyges command-line interface, the **Loom** open EDA sign-off suite, reproducible from-source builds of the open-source EDA tools, and a tool-agnostic catalog of open process design kits. Everything here runs **on your machine** — no cloud lock-in, no per-seat licenses — and is **pinned** so a build today reproduces the same result tomorrow.

## Vyges CLI

One binary for the whole flow: install and manage IP, PDKs, and the Loom sign-off engines, then drive them from the command line or an MCP-aware assistant.

- [`cli`](https://github.com/vyges-tools/cli) — prebuilt binaries + one-line installers (macOS · Linux · Windows), a Homebrew tap, and the catalog / PDK / Loom-engine installers.
- Docs: <https://docs.vyges.com>

## Loom — open EDA sign-off suite

Loom is a suite of fast, deterministic, **local** engines that share one design-data spine: the engines that **build** the block, the optimizers that **close the loop**, and the checks that **prove it**. One declarative job file in, one standard artifact out, and the next engine reads it directly — no glue scripts, no silent copy-drift. Run each as `vyges loom <engine>`.

### Foundation — the design data everything else stands on

- [`opendb`](https://github.com/vyges-tools/opendb) — safe Rust API over **OpenROAD's OpenDB**: ECO surgery, DEF I/O, a typed query surface, and **2.5D/3D chiplet assembly** (read a 3Dblox stack, check its structure, verify the die-to-die interfaces connect, draw it)
- [`opendb-lib`](https://github.com/vyges-tools/opendb-lib) — standalone from-source build + FFI for libodb, pinned to an exact OpenROAD commit
- [`loom`](https://github.com/vyges-tools/loom) — shared readers (Verilog · Liberty · SDC · SPEF) + the in-memory design database
- [`layout`](https://github.com/vyges-tools/layout) — clean-room geometry kernel: **GDSII and OASIS** read/write, polygon booleans, flatten. A foundation library, not a sign-off engine — it emits geometry, not a verdict
- [`geom`](https://github.com/vyges-tools/geom) — shared layout primitives + uniform-grid spatial index
- [`events`](https://github.com/vyges-tools/events) — canonical structured events + tracing bridge, so every engine reports the same way

### Physical construction — build the block

The floorplan and its rows, the well taps and endcaps, the power grid, the IO pins and pad ring, and the fill that meets density. They write **straight into the same design database** the rest of the suite reads — no DEF round-trip, no exchange format in between — so the block you sign off is the block you built. Placement, CTS and routing stay the flow's job.

- [`ifp`](https://github.com/vyges-tools/ifp) — **floorplan & rows**: die/core area, the site grid, and the rows every later stage places into (hybrid sites and row parity included)
- [`tap`](https://github.com/vyges-tools/tap) — **well taps & endcaps**: cuts rows around macros and places the physical cells that keep wells tied and row ends legal
- [`pdn`](https://github.com/vyges-tools/pdn) — **power grid**: rings, straps, follow pins and the vias that stitch the layers, then publishes the supply nets as block terminals
- [`ppl`](https://github.com/vyges-tools/ppl) — **IO pin placement**: optimal matching per section, honouring excluded regions, region constraints, pin groups and mirrored pairs
- [`pad`](https://github.com/vyges-tools/pad) — **IO ring & pads**: the ring of IO rows around the die, with legality checked layer by layer rather than by bounding box
- [`fin`](https://github.com/vyges-tools/fin) — **density fill**: metal fill until every layer meets its density floor; clears existing fill first, so re-running is idempotent

### Sign-off — analyze

- **Timing & SI** — [`sta-si`](https://github.com/vyges-tools/sta-si): WNS/TNS sign-off with crosstalk + statistical OCV
- **Power, IR & thermal** — [`power`](https://github.com/vyges-tools/power) · [`em-ir`](https://github.com/vyges-tools/em-ir) · [`thermal`](https://github.com/vyges-tools/thermal)
- **Parasitics** — [`extract`](https://github.com/vyges-tools/extract): routed layout → SPEF
- **Characterization** — [`char`](https://github.com/vyges-tools/char): SPICE + PDK models → Liberty (NLDM + CCS)
- **Layout correctness** — [`lvs`](https://github.com/vyges-tools/lvs): name-independent graph match, with the divergence named
- **Analog measurement** — [`meas`](https://github.com/vyges-tools/meas): SNR/SINAD/THD/SFDR from a tone; gain, bandwidth, phase margin from a sweep
- **Process damage** — [`ant`](https://github.com/vyges-tools/ant): antenna ratios on the **routed** database, naming the gate and the layer

### Optimizers — act

Netlist in, better netlist out, every move scored by the same `sta-si` timer.

- [`resize`](https://github.com/vyges-tools/resize) — gate sizing · [`vt-swap`](https://github.com/vyges-tools/vt-swap) — leakage/speed trade (iso-footprint)
- [`buffer-insert`](https://github.com/vyges-tools/buffer-insert) — transition fixup · [`hold-fix`](https://github.com/vyges-tools/hold-fix) — post-route hold
- [`remap`](https://github.com/vyges-tools/remap) — logic-level area recovery via multi-output covering, equivalence-checked

### Verification — prove / view

- [`drc`](https://github.com/vyges-tools/drc) — width, spacing, area, density, antenna, enclosure on GDS/OASIS (+ metal fill generation)
- [`cdc`](https://github.com/vyges-tools/cdc) — unsynchronized clock-domain crossings, named
- [`glitch`](https://github.com/vyges-tools/glitch) — reconvergent-fanout hazards a cycle-based simulator steps over
- [`lec`](https://github.com/vyges-tools/lec) — proves two netlists compute the same function, or hands you the input where they differ
- [`gds-view`](https://github.com/vyges-tools/gds-view) — headless GDS/OASIS → layered SVG, overlaying exactly where DRC or LVS flagged a problem

- **See it run** → the [AI-driven sign-off dashboard](https://vyges.github.io/vyges-loom-testbench/): a stock, general-purpose LLM drives the real Loom engines through `vyges mcp`, picking each tool and forming its arguments from the engine descriptors alone.
- Product page: <https://vyges.com/products/loom>

## EDA tool distros

Headless, multi-arch (amd64 + arm64) builds of the open-source EDA tools — pinned to exact upstream commits, built from source on CI, published to GHCR.

- [`vyges-yosys`](https://github.com/vyges-tools/vyges-yosys) — Yosys + ABC + OpenSTA (synthesis + timing)
- [`vyges-openroad`](https://github.com/vyges-tools/vyges-openroad) — OpenROAD (RTL → GDS)
- [`vyges-klayout`](https://github.com/vyges-tools/vyges-klayout) — headless, Qt-free KLayout + GDS renderer
- [`vyges-ngspice`](https://github.com/vyges-tools/vyges-ngspice) — OSDI / OpenVAF-enabled ngspice
- [`vyges-sim`](https://github.com/vyges-tools/vyges-sim) — Verilator + cocotb simulation substrate
- [`vyges-mockturtle`](https://github.com/vyges-tools/vyges-mockturtle) — reproducible builds of the multi-output technology mapper behind `remap`
- [`vybox-eda`](https://github.com/vyges-tools/vybox-eda) — one light container composing them all, for digital + analog + mixed-signal flows
- [`vyges-iic-osic-tools`](https://github.com/vyges-tools/vyges-iic-osic-tools) — IIC-OSIC-TOOLS composed with the Vyges CLI + Loom engines, for analog and mixed-signal work

## Open PDKs

A tool-agnostic catalog of open process design kits, presented uniformly so any flow can consume them.

- [`pdk-catalog`](https://github.com/vyges-tools/pdk-catalog) — the index + full PDK descriptors
- [`pdk-releases`](https://github.com/vyges-tools/pdk-releases) — Vyges-built, ciel-compatible open-PDK releases
- Mirrors: [`open_pdks`](https://github.com/vyges-tools/open_pdks) (sky130 · gf180mcu) · [`ihp-open-pdk`](https://github.com/vyges-tools/ihp-open-pdk) (SG13G2) · [`ihp-sg13cmos5l`](https://github.com/vyges-tools/ihp-sg13cmos5l) · [`asap7`](https://github.com/vyges-tools/asap7) · [`nangate45`](https://github.com/vyges-tools/nangate45) · [`icsprout55`](https://github.com/vyges-tools/icsprout55)

## Operator

vyges-tools is operated by **TrustStix Inc** (California, USA — C Corporation). The tools here are free and open; operational costs (build infrastructure, hosting, curation) are absorbed by TrustStix. We treat this toolchain as public infrastructure for the open-silicon ecosystem.

## Links

- Main site: <https://vyges.com>
- Products: <https://vyges.com/products>
- Loom: <https://vyges.com/products/loom>
- Loom testbench (live): <https://vyges.github.io/vyges-loom-testbench/>
- VyCatalog: <https://vyges.com/products/vycatalog>
- Docs: <https://docs.vyges.com>
- Contact: <https://vyges.com/contact>
- Sponsor: <https://github.com/sponsors/vyges-ip>
