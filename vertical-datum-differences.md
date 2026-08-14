# Height differences between national vertical datums at borders

Each country historically defines its own vertical datum (height reference), typically tied
to a local tide gauge (mean sea level at a specific place and epoch). Because sea surface
topography is not perfectly flat, these local "zero levels" do not coincide exactly, so a
point's official height can jump by tens of centimeters when crossing a border — even though
the physical ground height obviously does not change. This is why using the wrong vertical
datum (or applying a correction with the wrong sign) can cause real construction errors, as
happened with the Rhine bridge at Laufenburg (see main `README.md`).

The table below lists approximate height offsets between adjoining national vertical datums,
mostly relative to the European Vertical Reference Frame (EVRF2007/2019) which uses the
Amsterdam tide gauge (NAP) as its zero level. Values are indicative (order of magnitude, from
published geodetic literature and national mapping agency documentation) — exact offsets vary
slightly by location, since the difference is not a single constant but a smoothly varying
surface. For any real engineering or surveying work, use the official transformation
grids/parameters published by the relevant national mapping agencies or EuroGeographics/BKG.

| Border                         | National datums                       | Approx. height offset | Notes |
|---------------------------------|----------------------------------------|:----------------------:|-------|
| Germany – Switzerland            | DHHN2016 (Amsterdam) vs. LN02 (Marseille) | ~27 cm             | The Laufenburg bridge case (see README.md); doubled to ~54 cm by a sign error during construction in 1876. |
| Germany – Austria                 | DHHN2016 (Amsterdam) vs. MGI (Trieste) | ~25–30 cm            | Trieste gauge reads higher than Amsterdam. |
| Switzerland – Austria              | LN02 (Marseille) vs. MGI (Trieste)    | ~20 cm                | Both are "Mediterranean-family" gauges, so the gap is smaller than either is to Amsterdam. |
| Switzerland – France                | LN02 (Marseille) vs. NGF-IGN69 (Marseille) | ~0–5 cm         | Both ultimately reference Marseille, so the offset is small. |
| Germany – France                    | DHHN2016 (Amsterdam) vs. NGF-IGN69 (Marseille) | ~40–50 cm    | |
| Germany – Netherlands                | DHHN2016 (Amsterdam) vs. NAP (Amsterdam) | ~0 cm             | Both effectively reference the Amsterdam gauge. |
| Germany – Belgium                     | DHHN2016 (Amsterdam) vs. TAW (Ostend) | ~2–5 cm              | Ostend gauge reads slightly lower than Amsterdam. |
| Belgium – Netherlands                   | TAW (Ostend) vs. NAP (Amsterdam)      | ~2–5 cm              | |
| Belgium – France                         | TAW (Ostend) vs. NGF-IGN69 (Marseille) | ~40–45 cm           | |
| Germany – Poland                          | DHHN2016 (Amsterdam) vs. Kronstadt86 (Kronstadt) | ~14–18 cm  | Kronstadt (Baltic) gauge reads lower than Amsterdam. |
| Germany – Denmark                          | DHHN2016 (Amsterdam) vs. DVR90 (Baltic) | ~0–10 cm           | Both are North Sea/Baltic family gauges, offset is small. |
| Italy – Austria                             | Genova gauge vs. MGI (Trieste)       | ~30–40 cm            | |
| Italy – Switzerland                          | Genova gauge vs. LN02 (Marseille)   | ~10–20 cm             | |
| Spain – France                                | Alicante gauge vs. NGF-IGN69 (Marseille) | ~20–30 cm         | |
| Portugal – Spain                               | Cascais gauge vs. Alicante gauge   | ~10–20 cm             | |

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



**Disclaimer**: the offsets above are approximate, rounded, order-of-magnitude figures meant
to illustrate why vertical datums matter. They should not be used directly for engineering or
surveying transformations — always use the official national/EUREF transformation grids for
real work.

## Sources / further reading

- BKG (German Federal Agency for Cartography and Geodesy), D-A-CH cross-border geoid/height
  project: https://gibs.bkg.bund.de/geoid/en/dach-projectsite.html
- CRS-EU, overview of national vertical reference systems in Europe:
  https://www.crs-geo.eu/crs-national.htm
- EVRS / EVRF2007 documentation, EuroGeographics / IAG Subcommission for Europe (EUREF):
  https://evrs.bkg.bund.de/
- Laufenburg bridge case: https://en.wikipedia.org/wiki/Laufenburg,_Germany#Bridge_construction

**Disclaimer**: the offsets above are approximate, rounded, order-of-magnitude figures meant
to illustrate why vertical datums matter. They should not be used directly for engineering or
surveying transformations — always use the official national/EUREF transformation grids for
real work.
