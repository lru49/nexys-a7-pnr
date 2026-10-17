# nexys-a7-pnr

Place-and-route jobs for Xilinx Artix-7 (XC7A100T, Nexys A7) netlists with the open-source
openXC7 flow (nextpnr-xilinx, openXC7 Apio release 2026-10-14). `nets/<variant>/` holds a
synthesised yosys netlist (`top.json.gz`) and its pin constraints (`pins.xdc`); the `pnr`
workflow routes it for one or more seeds and uploads the bitstreams, the reports and the logs.

The `replay` workflow places and routes a reference netlist for one seed and writes placement-replay pin lists
(`nets/<target>/preplaced_<tag>.txt`) for target netlists, committed back to this repo; `pnr` then takes them through
`extra: -o preplaced=../nets/<target>/preplaced_<tag>.txt` with `nextpnr: r43`.

`pnr` runs each seed group (input `groups`, a JSON list such as `["1 2 3 4","5 6 7 8"]`; without it, `seeds` forms one
group) as one job, its seeds in parallel with `--threads 1` (input `tune`: `threads=1 per_job=4 wallmin=45`). A seed is
killed after `maxiter` router iterations or `wallmin` minutes of wall time (logged as `early-kill: ...`), the job after
90 minutes. Per group it uploads `log-<variant>-g<first seed>` (`s<seed>/pnr.log`, `time.log` with the peak memory,
`report.json`) and `bit-<variant>-g<first seed>` (`s<seed>/top.bit.gz` of every routed seed).
