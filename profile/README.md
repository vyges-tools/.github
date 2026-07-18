# Vyges Tools

**Open, reproducible, local-first silicon tooling — the EDA toolchain, sign-off engines, and open PDKs behind Vyges™.**

vyges-tools is the home for the tooling that turns RTL into silicon: the Vyges command-line interface, the **Loom** open EDA sign-off suite, reproducible from-source builds of the open-source EDA tools, and a tool-agnostic catalog of open process design kits. Everything here runs **on your machine** — no cloud lock-in, no per-seat licenses — and is **pinned** so a build today reproduces the same result tomorrow.

## Vyges CLI

One binary for the whole flow: install and manage IP, PDKs, and the Loom sign-off engines, then drive them from the command line or an MCP-aware assistant.

- [`cli`](https://github.com/vyges-tools/cli) — prebuilt binaries + one-line installers (macOS · Linux · Windows), a Homebrew tap, and the catalog / PDK / Loom-engine installers.
- Docs: <https://docs.vyges.com>

## Loom — open EDA sign-off suite

Loom is a suite of fast, deterministic, **local** sign-off engines that share one parse-once design database. Run each as `vyges loom <engine>`.

- **Timing & SI** — [`sta-si`](https://github.com/vyges-tools/sta-si): WNS/TNS sign-off with crosstalk + statistical OCV
- **Power, IR & thermal** — [`power`](https://github.com/vyges-tools/power) · [`em-ir`](https://github.com/vyges-tools/em-ir) · [`thermal`](https://github.com/vyges-tools/thermal)
- **Physical verification** — [`drc`](https://github.com/vyges-tools/drc) · [`lvs`](https://github.com/vyges-tools/lvs) · [`extract`](https://github.com/vyges-tools/extract) (routed layout → SPEF)
- **Characterization** — [`char`](https://github.com/vyges-tools/char): SPICE + PDK models → Liberty (NLDM + CCS)
- **Timing ECOs** — [`hold-fix`](https://github.com/vyges-tools/hold-fix) · [`buffer-insert`](https://github.com/vyges-tools/buffer-insert) · [`resize`](https://github.com/vyges-tools/resize) · [`vt-swap`](https://github.com/vyges-tools/vt-swap)
- **Foundation** — [`loom`](https://github.com/vyges-tools/loom): shared readers (Verilog · Liberty · SDC · SPEF) + in-memory design database

More engines (CDC, glitch, LEC, layout, geometry, remap, structured events) live across the org — browse the [repository list](https://github.com/orgs/vyges-tools/repositories).

- Product page: <https://vyges.com/products/loom>

## EDA tool distros

Headless, multi-arch (amd64 + arm64) builds of the open-source EDA tools — pinned to exact upstream commits, built from source on CI, published to GHCR.

- [`vyges-yosys`](https://github.com/vyges-tools/vyges-yosys) — Yosys + ABC + OpenSTA (synthesis + timing)
- [`vyges-openroad`](https://github.com/vyges-tools/vyges-openroad) — OpenROAD (RTL → GDS)
- [`vyges-klayout`](https://github.com/vyges-tools/vyges-klayout) — headless, Qt-free KLayout + GDS renderer
- [`vyges-ngspice`](https://github.com/vyges-tools/vyges-ngspice) — OSDI / OpenVAF-enabled ngspice
- [`vyges-sim`](https://github.com/vyges-tools/vyges-sim) — Verilator + cocotb simulation substrate
- [`vybox-eda`](https://github.com/vyges-tools/vybox-eda) — one light container composing them all, for digital + analog + mixed-signal flows

## Open PDKs

A tool-agnostic catalog of open process design kits, presented uniformly so any flow can consume them.

- [`pdk-catalog`](https://github.com/vyges-tools/pdk-catalog) — the index + full PDK descriptors
- [`pdk-releases`](https://github.com/vyges-tools/pdk-releases) — Vyges-built, ciel-compatible open-PDK releases
- Mirrors: sky130 (open_pdks) · gf180mcu · IHP SG13G2 · IHP SG13CMOS-5L · ASAP7 · Nangate45 · ICsprout55

## Operator

vyges-tools is operated by **TrustStix Inc** (California, USA — C Corporation). The tools here are free and open; operational costs (build infrastructure, hosting, curation) are absorbed by TrustStix. We treat this toolchain as public infrastructure for the open-silicon ecosystem.

## Links

- Main site: <https://vyges.com>
- Products: <https://vyges.com/products>
- Loom: <https://vyges.com/products/loom>
- VyCatalog: <https://vyges.com/products/vycatalog>
- Docs: <https://docs.vyges.com>
- Contact: <https://vyges.com/contact>
