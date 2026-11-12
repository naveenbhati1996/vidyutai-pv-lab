# Solar PV Lab · SPEL, IIT Gandhinagar

Interactive model of a 10.2 kWp rooftop bifacial PV plant (18 × 565 W modules, 3 strings × 6) at IIT Gandhinagar.

**Smart Power Electronics Laboratory (SPEL), IIT Gandhinagar**

## Open it

| Page | What it shows |
|---|---|
| `index.html` | Demo player (~83 s): ratings, measured lab data, then healthy / shading / dust / hot spot / line-line fault / shorted diode / open string, each followed by its I–V result and power loss |
| `lab.html` | Interactive lab (MPPT tab: STC reference, variable-step tracking, shading demo with local and global peaks): Plant, Curves, Shading, **Sun & shadow**, Hot spot, Faults, MPPT, Test bench, Life, **Code**, Standards |
| `VidyutAI_Solar_PV_Lab_Demo.mp4` | The demo as a video |
| `data/VidyutAI_Load_Resistance_Table.xlsx` | Rheostat / DC e-load resistance settings (live formulas) |

## How it works

Plain HTML + CSS + JavaScript, no server, no build step, no data leaves the browser.

1. **Module model** – explicit one-diode approximation per bypass section:
   `V = C2·Voc·ln(1 + (1 − I/Isc)/C1)`, with `C2 = (Vmp/Voc − 1)/ln(1 − Imp/Isc)` and `C1 = (1 − Imp/Isc)·exp(−Vmp/(Voc·C2))`.
   Isc and Voc are corrected for front + rear (bifacial) irradiance and cell temperature (NMOT).
2. **Bypass diodes** clamp a section at −0.45 V when it cannot carry the string current.
3. **Strings and array** – section voltages are added; string current is solved by bisection; string currents are added for the array I–V and P–V curves and the maximum power point.
4. **Sun & shadow** – sun position for Gandhinagar (declination, equation of time, hour angle), clear-sky irradiance on the tilted module, and ray tracing from 24 points per module to a building box and a tree crown. Repeated every 10 min to get daily energy and loss.
5. **Hot spot** – reverse-biased low-shunt cell heating with a first-order thermal model.

The **Code** tab in `lab.html` shows the live source of each function and lets you run the model.

## Change the ratings

- Demo: edit the `CONFIG` block at the top of the first `<script>` in `index.html`.
- Lab: edit `var MOD = {voc:50, isc:14, vmp:42, imp:13.2, ...}` in `lab.html`.
- Load table: edit the yellow cells on the **Inputs** sheet.

## Publish with GitHub Pages

Settings → Pages → Source: *Deploy from a branch* → branch `main`, folder `/ (root)` → Save.
The site appears at `https://<username>.github.io/vidyutai-pv-lab/`.

## Standards referenced

IEC 61215, IEC TS 62446-3, IEC 62446-1, IEC 60904-1, IEC 60891, IEC 61853-1, IEC 61724-1, IEC 62548, IEC 62979, EN 50530.

Model results are indicative (teaching and research), not a certified rating.
