# Height differences between national vertical datums at borders

Each country historically defines its own vertical datum (height reference), typically tied
to a local tide gauge (mean sea level at a specific place and epoch). Because sea surface
topography is not perfectly flat, these local "zero levels" do not coincide exactly, so a
point's official height can jump by tens of centimeters (sometimes meters) when crossing a
border — even though the physical ground height obviously does not change. This is why using
the wrong vertical datum (or applying a correction with the wrong sign) can cause real
construction errors, as happened with the Rhine bridge at Laufenburg (see main `README.md`).

**All figures below were computed, not taken from published literature or tables.** For each
border, a coordinate near the actual crossing point was transformed directly between the two
countries' EPSG compound CRS (horizontal + vertical) using `cs2cs` from PROJ/GDAL, with a
round input height (100.0000 m); the difference between input and output height is the
computed local offset at that point. See the worked examples further down for the exact
commands. `projinfo` was used to confirm that PROJ selected a real, non-"ballpark" operation
for each pair (i.e. an actual published transformation/grid, not a placeholder). Because the
offset is a smoothly-varying surface rather than a constant, the value only applies near the
chosen coordinate — moving along the border can change it somewhat.

Pairs involving Austria, Poland, Denmark and Italy–Switzerland are omitted here: the local
PROJ/proj.db install used for these computations does not have a non-ballpark
(non-placeholder) operation available for those specific compound CRS combinations, so no
reliable computed value could be produced for them.

| Border                    | Compound CRS pair              | Computed offset at test point | Notes |
|----------------------------|----------------------------------|:-------------------------------:|-------|
| Germany – Switzerland       | EPSG:5555 → EPSG:2056+EPSG:5729 | **+32 cm** (CH higher)         | Near Laufenburg (47.567°N, 8.070°E); same order of magnitude as the historical 27 cm Laufenburg bridge figure. |
| Germany – France             | EPSG:5555 → EPSG:5698           | **+50 cm** (FR higher)         | Near 48.58°N, 7.77°E (Rhine, Strasbourg area). |
| Germany – Netherlands          | EPSG:5555 → EPSG:7415         | **+2 cm** (NL higher)          | Near 51.80°N, 6.10°E. |
| Germany – Belgium                | EPSG:5555 → EPSG:6190       | **+2.33 m** (BE higher)        | Near 50.65°N, 6.05°E (Eupen area). |
| Belgium – Netherlands               | EPSG:6190 → EPSG:7415     | **−2.31 m** (NL lower)         | Near Baarle-Nassau (51.44°N, 4.93°E) — see worked example below. |
| Belgium – France                     | EPSG:6190 → EPSG:5698   | **−1.82 m** (FR lower)         | Near 50.30°N, 3.85°E; consistent with the DE–BE and DE–FR rows above. |
| France – Switzerland                    | EPSG:5698 → EPSG:2056+EPSG:5729 | **−34 cm** (CH lower)  | Near Geneva (46.20°N, 6.15°E). |
| France – Spain                            | EPSG:5698 → EPSG:9505 | **−4 cm** (ES lower)           | Near 42.43°N, 2.87°E (Pyrenees, eastern end). |
| Spain – Portugal                            | EPSG:9505 → EPSG:10545 | **−4 cm** (PT lower)        | Near 42.05°N, −8.64°E. |
| France – Italy                                | EPSG:5698 → EPSG:9723 | **−15 cm** (IT lower)      | Near 44.13°N, 7.55°E (Alps, Col de Tende area). |

## Worked example: computing the CH–DE offset yourself with GDAL/PROJ

You can compute the local height-datum offset yourself with the `cs2cs` tool from PROJ
(shipped with GDAL), by transforming a coordinate near the border from the German compound
CRS directly to the Swiss compound CRS and comparing the input/output height.

Example, near the Laufenburg bridge on the Rhine (~47.567°N, 8.070°E), from the German
compound `EPSG:5555` (ETRS89 / UTM zone 32N + DHHN92 height) to the Swiss compound
`EPSG:2056` + `EPSG:5729` (CH1903+ / LV95, projected + LHN95 height):

```bash
# 1. Get UTM32N easting/northing for the point (from lon/lat 8.070 / 47.567):
echo "8.070 47.567" | cs2cs -d 4 EPSG:4326 EPSG:25832
# 430047.9381  5268594.9262

# 2. Transform the German compound CRS coordinate (with height 400.0000 m) directly
#    to the Swiss compound CRS:
echo "430047.9381 5268594.9262 400.0000" | cs2cs -d 4 EPSG:5555 "EPSG:2056+EPSG:5729"
# 2647512.3500  1268667.7093  400.3188
```

An input height of 400.0000 m (German DHHN92) becomes 400.3188 m (Swiss LHN95) at this
location — a local offset of about **+32 cm** between the German and Swiss height references
at this point on the border, in the same order of magnitude as the historical 27 cm
Marseille/Amsterdam figure quoted for the Laufenburg bridge (see above).

## Worked example: computing the BE–NL offset at Baarle-Nassau

Same approach, this time at the Belgian–Dutch border near Baarle-Nassau/Baarle-Hertog
(~51.44°N, 4.93°E), from the Belgian compound `EPSG:6190` (BD72 / Belgian Lambert 72 + Ostend
height) to the Dutch compound `EPSG:7415` (Amersfoort / RD New + NAP height):

```bash
# 1. Get Belgian Lambert 72 easting/northing for the point (from lon/lat 4.93 / 51.44):
echo "51.44 4.93" | cs2cs -d 4 EPSG:4326 EPSG:31370
# 189026.4341  236853.1764

# 2. Transform the Belgian compound CRS coordinate (with height 40.0000 m) directly
#    to the Dutch compound CRS:
echo "189026.4341 236853.1764 40.0000" | cs2cs -d 4 EPSG:6190 EPSG:7415
# 104400.2492  379794.8889  37.6881
```

An input height of 40.0000 m (Belgian Ostend height) becomes 37.6881 m (Dutch NAP height) at
this location — a local offset of about **−2.3 m**: the Ostend reference reads about 2.3 m
higher than NAP at this point. This is a much larger offset than the DE–CH or DE–EU examples
above, because Ostend and Amsterdam are both North Sea tide gauges but with a long-known,
comparatively large mean-level difference (this is also the exact reason the Netherlands and
Belgium each maintain their own separate national height network rather than sharing one).

Check with `projinfo -s EPSG:6190 -t EPSG:7415 --spatial-test intersects --summary` that the
operation `cs2cs` picked (accuracy "2.2 m", combining "Ostend height to EVRF2000 height" and
"NAP height to EVRF2000 height") is a real, non-ballpark operation and not a fallback.

## Worked example: computing the DE–FR offset near Strasbourg

From the German compound `EPSG:5555` to the French compound `EPSG:5698` (ETRS89-FRA /
Lambert-93 + NGF-IGN69 height), near 48.58°N, 7.77°E:

```bash
echo "48.58 7.77" | cs2cs -d 6 EPSG:4326 EPSG:25832
# 409281.548370  5381498.333341

echo "409281.548370 5381498.333341 100.000000" | cs2cs -d 6 EPSG:5555 EPSG:5698
# 1051636.527967  6841711.593217  100.501332
```

Offset: **+50.1 cm** (France reads higher than Germany at this point).

## Worked example: computing the DE–NL offset near Kranenburg

From `EPSG:5555` to `EPSG:7415`, near 51.80°N, 6.10°E:

```bash
echo "51.80 6.10" | cs2cs -d 6 EPSG:4326 EPSG:25832
# 300047.453806  5742772.163340

echo "300047.453806 5742772.163340 100.000000" | cs2cs -d 6 EPSG:5555 EPSG:7415
# 204166.929573  423725.518099  100.018595
```

Offset: **+1.9 cm** (Netherlands reads slightly higher than Germany at this point) — much
smaller than the DE–BE or BE–NL offsets, consistent with Germany and the Netherlands both
being North Sea/Amsterdam-family references.

## Worked example: computing the DE–BE offset near Eupen

From `EPSG:5555` to `EPSG:6190`, near 50.65°N, 6.05°E:

```bash
echo "50.65 6.05" | cs2cs -d 6 EPSG:4326 EPSG:25832
# 291464.224373  5615057.764988

echo "291464.224373 5615057.764988 100.000000" | cs2cs -d 6 EPSG:5555 EPSG:6190
# 268888.625361  150166.216124  102.330185
```

Offset: **+2.33 m** (Belgium reads higher than Germany here). Combined with the DE–NL and
BE–NL results above, this is consistent: DE→NL (+1.9 cm) and BE→NL (−2.31 m) together roughly
predict DE→BE ≈ +2.33 m, as found directly.

## Worked example: computing the BE–FR offset near Tournai/Lille

From `EPSG:6190` to `EPSG:5698`, near 50.30°N, 3.85°E:

```bash
echo "50.30 3.85" | cs2cs -d 6 EPSG:4326 EPSG:31370
# 113042.448346  110016.702034

echo "113042.448346 110016.702034 100.000000" | cs2cs -d 6 EPSG:6190 EPSG:5698
# 760637.485776  7022787.760177  98.178591
```

Offset: **−1.82 m** (France reads lower than Belgium here).

## Worked example: computing the FR–CH offset near Geneva

From `EPSG:5698` to the Swiss compound `EPSG:2056+EPSG:5729`, near 46.20°N, 6.15°E:

```bash
echo "46.20 6.15" | cs2cs -d 6 EPSG:4326 EPSG:2154
# 942837.352007  6571528.337741

echo "942837.352007 6571528.337741 100.000000" | cs2cs -d 6 EPSG:5698 "EPSG:2056+EPSG:5729"
# 2500532.753932  1117323.350006  99.659801
```

Offset: **−34 cm** (Switzerland reads lower than France here).

## Worked example: computing the FR–ES offset in the eastern Pyrenees

From `EPSG:5698` to the Spanish compound `EPSG:9505` (ETRS89-ESP [REGENTE] + Alicante height),
near 42.43°N, 2.87°E:

```bash
echo "42.43 2.87" | cs2cs -d 6 EPSG:4326 EPSG:2154
# 689285.836580  6147796.544781

echo "689285.836580 6147796.544781 100.000000" | cs2cs -d 6 EPSG:5698 EPSG:9505
# 42.430000  2.870000  99.960534
```

Offset: **−4 cm** (Spain reads lower than France here). Note the output is geographic
(lon/lat), since `EPSG:9505` uses a geographic (not projected) horizontal component.

## Worked example: computing the ES–PT offset near Tui/Valença

From `EPSG:9505` to the Portuguese compound `EPSG:10545` (ETRS89-PRT [1995] + Cascais
height), near 42.05°N, −8.64°E:

```bash
echo "42.05 -8.64 100.000000" | cs2cs -d 6 EPSG:9505 EPSG:10545
# 42.050000  -8.640000  99.958202
```

Offset: **−4 cm** (Portugal reads lower than Spain here).

## Worked example: computing the FR–IT offset near Col de Tende

From `EPSG:5698` to the Italian compound `EPSG:9723` (ETRS89-ITA [RDN2008] + Genoa 1942
height), near 44.13°N, 7.55°E:

```bash
echo "44.13 7.55" | cs2cs -d 6 EPSG:4326 EPSG:2154
# 1063904.419594  6347264.614296

echo "1063904.419594 6347264.614296 100.000000" | cs2cs -d 6 EPSG:5698 EPSG:9723
# 44.130000  7.550000  99.847989
```

Offset: **−15 cm** (Italy reads lower than France here).


**Disclaimer**: the offsets above are computed with `cs2cs`/PROJ at one specific test
coordinate per border, using the official EPSG dataset's registered transformations/grids —
they are real computed values, not estimates from published tables. However, they are still
only valid near the chosen test point (the offset is a smoothly-varying surface, not a
constant), and PROJ's chosen operation is not necessarily the most accurate one available for
every location. For real engineering or surveying work, use the official national/EUREF
transformation grids and verify the operation and its accuracy with `projinfo` for your exact
coordinates.

## Tools / further reading

- PROJ / `cs2cs`, `projinfo` (used to compute all values above): https://proj.org/
- EPSG dataset (via `proj.db`, the source of all EPSG codes and transformation grids used):
  https://epsg.org/home.html
- Laufenburg bridge case: https://en.wikipedia.org/wiki/Laufenburg,_Germany#Bridge_construction
to illustrate why vertical datums matter. They should not be used directly for engineering or
surveying transformations — always use the official national/EUREF transformation grids for
real work.
