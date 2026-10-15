# nexys-a7-pnr

Place-and-route jobs for Xilinx Artix-7 (XC7A100T, Nexys A7) netlists with the open-source
openXC7 flow (nextpnr-xilinx, openXC7 Apio release 2026-10-14). `nets/<variant>/` holds a
synthesised yosys netlist (`top.json.gz`) and its pin constraints (`pins.xdc`); the `pnr`
workflow routes it for one or more seeds and uploads `top.fasm`, the report and the log.
