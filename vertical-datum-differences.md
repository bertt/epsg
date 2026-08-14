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
(shipped with GDAL) by transforming the same point from the compound CRS of one country to
the compound CRS of the other, keeping the same input height, and comparing the output height.

Example, near the Laufenburg bridge on the Rhine (WGS84/ETRS89 ~47.567°N, 8.070°E), from the
pan-European compound `EPSG:7423` (ETRS89 + EVRF2007 height) to the Swiss compound
`EPSG:4258+EPSG:5729` (ETRS89 + LHN95 height):

```bash
echo "47.567 8.070 400" | cs2cs -d 4 EPSG:7423 EPSG:4258+EPSG:5729
# 47.5670  8.0700 400.1594
```

An input height of 400.0000 m (EVRF2007) becomes 400.1594 m (Swiss LHN95) at this location —
i.e. a local offset of about **+16 cm** between the pan-European EVRF2007 reference and the
Swiss LHN95 reference at this specific point. Note this is smaller than, and not directly
comparable to, the historical 27 cm Marseille/Amsterdam difference quoted for the Laufenburg
bridge story: LHN95 is a modern, GNSS/geoid-based Swiss realization (not the 1876 Marseille
gauge value), and the offset is a smoothly-varying surface (via `+proj=vertoffset` and geoid
grids) rather than a single constant, so the result depends on exactly which coordinate you
pick and which grids/realizations are involved.

Steps to reproduce or adapt this for other borders:
1. Look up the compound EPSG code for each country/area (horizontal + vertical CRS), e.g. from
   `README.md`.
2. Pick a coordinate near the border of interest.
3. Run `cs2cs -d 4 <source compound CRS> <target compound CRS>` with `lat lon height` as input
   (check axis order with `projinfo -s <src> -t <dst> -o PROJ` if unsure).
4. The difference between input and output height is the local datum offset at that point.
5. Repeat at a few points along the border, since the offset is not constant — use
   `projinfo -s <src> -t <dst>` to see which operation/grids PROJ selected, and make sure the
   required grids are installed locally or reachable via `PROJ_NETWORK=ON`
   (grids are served from https://cdn.proj.org/).

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
