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
