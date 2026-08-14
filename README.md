# epsg

Frequently used horizontal / vertical / compound EPSG codes per country or region.

## Map version 

see https://bertt.github.io/epsg/map.html

**Source**: this table is generated from the official EPSG Geodetic Parameter Dataset
(https://epsg.org/home.html), IOGP EPSG dataset **v12.049**. Since the epsg.org website search
and its public REST API do not reliably support scripted text/country filtering, the data was
retrieved from `proj.db`, the SQLite database that PROJ (and therefore QGIS/GDAL) distributes
directly from that same official EPSG dataset (see also https://epsg.org/download-dataset.html,
"EPSG Database / SQL scripts"). For every compound CRS, the horizontal and vertical components
were read directly from the `compound_crs` table, so the combinations are guaranteed to be
correct and consistent.

Other useful links:

- https://spatialreference.org/ , https://epsg.io/ (less official CRS lookup)
- https://crs-explorer.proj.org/ (CRS explorer tool)
- https://cdn.proj.org/ (geoid model grids)
- https://www.agisoft.com/downloads/geoids/ (geoid grids for some countries)

Notes:
- Where a country has several UTM/local zones (e.g. Argentina, Iran, Pakistan, South Africa,
  India, Morocco), the relevant range of zone codes is listed instead of a single code. Check
  the full EPSG registration for the exact zone boundaries.
- "—" means that the EPSG dataset (v12.049) does not register a separate vertical and/or
  compound CRS for that country/region.
- Where a country has both a classic (e.g. WGS84/ED50) and a modern (ETRS89/ITRF/SIRGAS)
  reference frame, the most modern, non-deprecated CRS is generally preferred.
- Some countries share a single regional geodetic datum with neighboring countries (e.g. Arc
  1960 for Kenya/Tanzania/Uganda, PSAD56 for several Andean countries). This is noted where
  relevant.

## Why this matters: the Laufenburg bridge

A classic, often-cited example of what can go wrong when two vertical datums (height
references) are mixed up is the **Rhine bridge at Laufenburg**, on the border between
Switzerland and Germany. When construction started in 1876, the Swiss and German engineers
each measured height relative to a different reference: Switzerland used mean sea level of
the Mediterranean (Marseille), while Germany used mean sea level of the North Sea
(Amsterdam/Hamburg). The known difference between these two references, about 27 cm, was
already taken into account — but a sign error in applying that correction meant it was
added instead of subtracted, doubling the discrepancy: the two halves of the bridge ended up
about 54 cm off vertically where they were meant to meet. The mismatch was caught before
completion and resolved by adjusting the German approach ramp to match the Swiss side, but it
remains a textbook illustration of why a project's vertical datum — and any correction applied
to it — must be stated and applied correctly and unambiguously, exactly the kind of ambiguity
that a well-defined EPSG vertical/compound CRS is meant to prevent.

See: https://en.wikipedia.org/wiki/Laufenburg,_Germany#Bridge_construction

For approximate height offsets between other neighboring countries' vertical datums, see
[`vertical-datum-differences.md`](vertical-datum-differences.md).

## Europe

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Austria               | 31287 (MGI / Austria Lambert, projected)         | 5778 (GHA height)                        | 9501 (MGI + EVRF2000 Austria height) |
| Belgium               | 31370 (BD72 / Belgian Lambert 72, projected)     | 5710 (Ostend height)                     | 6190 (BD72 / Belgian Lambert 72 + Ostend height) |
| Bulgaria              | 7801 (ETRS89-BGR [BGS2005] / CCS2005, projected) | 9669 (BGS2005 height)                    | — |
| Croatia               | 3765 (ETRS89-HRV [HTRS96] / Croatia TM, projected) | 5610 (HVRS71 height)                   | — |
| Czechia               | 5515 (S-JTSK/05 / Modified Krovak, projected)    | — (Baltic 1957 height not separately registered) | 11311 (ETRS89-CZE [2007] + Baltic 1957 height) |
| Denmark               | 4095 (ETRS89 / DKTM3, projected)                 | 5799 (DVR90 height)                      | 4099 (ETRS89 / DKTM3 + DVR90 height) |
| Estonia               | 3301 (Estonian CS of 1997, projected)            | 9663 (EH2000 height)                     | — |
| Europe (ETRS89)       | 4936 (ETRS89, geocentric) / 4258 (geo 2D)        | 5621 (EVRF2007 height)                    | 4937 (ETRS89, geographic 3D) / 7423 (ETRS89 + EVRF2007 height) |
| Finland               | 3067 (ETRS89-FIN / TM35FIN(E,N), projected)      | 3900 (N2000 height)                      | 3903 (ETRS89-FIN / TM35FIN(N,E) + N2000 height) |
| France (mainland)     | 2154 (ETRS89-FRA / Lambert-93, projected)        | 5720 (NGF-IGN69 height)                  | 5698 (ETRS89-FRA / Lambert-93 + NGF-IGN69 height) |
| France - Corsica      | 2154 (ETRS89-FRA / Lambert-93, projected)        | 5721 (NGF-IGN78 height)                  | 5699 (ETRS89-FRA / Lambert-93 + NGF-IGN78 height) |
| Germany               | 25832 (ETRS89 / UTM zone 32N, projected)         | 5783 (DHHN92 height)                     | 5555 (ETRS89 / UTM zone 32N + DHHN92 height) |
| Greece                | 2100 (GGRS87 / Greek Grid, projected)            | 5716 (Piraeus height)                    | — |
| Hungary               | 23700 (HD72 / EOV, projected)                    | 5787 (EOMA 1980 height)                  | 10660 (HD72 / EOV + EOMA 1980 height) |
| Iceland               | 3057 (ISN93 / Lambert 1993, projected)           | 8089 (ISH2004 height)                    | 9951 (ISN93 / Lambert 1993 + ISH2004 height) |
| Ireland               | 29902 (TM65 / Irish Grid, projected)             | — (no vertical CRS registered)           | — |
| Italy                 | 6706 (ETRS89-ITA [RDN2008], geo 2D)              | 5214 (Genoa 1942 height)                 | 9723 (ETRS89-ITA [RDN2008] + Genoa 1942 height) |
| Luxembourg            | 2169 (LUREF / Luxembourg TM, projected)          | 5774 (NG95 height)                       | 9897 (LUREF / Luxembourg TM + NG95 height) |
| Netherlands           | 28992 (Amersfoort / RD New, projected)           | 5709 (NAP height)                        | 7415 (Amersfoort / RD New + NAP height) |
| Norway                | 25832 (ETRS89-NOR, projected) / 10875 (geo 2D)   | 5941 (NN2000 height)                     | 5942 (ETRS89-NOR [EUREF89] + NN2000 height) |
| Poland                | 2180 (ETRS89 / PL-1992, projected)               | 9650 (Baltic 1986 height)                | 9656 (ETRS89-POL [PL-ETRF2000] + Baltic 1986 height) |
| Portugal (mainland)   | 3763 (ETRS89-PRT [1995] / Portugal TM06)         | 5780 (Cascais height)                    | 10545 (ETRS89-PRT [1995] + Cascais height) |
| Romania               | 3844 (Pulkovo 1942(58) / Stereo70, projected)    | 5781 (Constanta height)                  | — |
| Russia                | 7683 (GSK-2011, geo 2D)                          | — (no vertical CRS registered)           | — |
| Slovakia              | 8352 (S-JTSK [JTSK03] / Krovak, projected)       | — (Baltic 1957 height not separately registered) | 11312 (ETRS89-SVK [SKTRF09] + Baltic 1957 height) |
| Slovenia              | 4765 (ETRS89-SVN [D96], geo 2D)                  | 8690 (SVS2010 height)                    | 10245 (ETRS89-SVN [D96] + SVS2010 height) |
| Spain (mainland)      | 11134 (ETRS89-ESP [REGENTE], geo 2D)             | 5782 (Alicante height)                   | 9505 (ETRS89-ESP [REGENTE] + Alicante height) |
| Spain - Canary Islands| 4081 (REGCAN95, geo 2D)                          | 9397 (Gran Canaria height)               | 9512 (REGCAN95 + Gran Canaria height) |
| Sweden                | 3006 (ETRS89-SWE [SWEREF 99 TM], projected)      | 5613 (RH2000 height)                     | 5628 (ETRS89-SWE [SWEREF 99] + RH2000 height) |
| Switzerland           | 2056 (CH1903+ / LV95, projected)                 | 5729 (LHN95 height)                      | — |
| Turkey                | 5636 (TUREF / LAEA Europe, projected)            | 5775 (Antalya height)                    | — |
| Ukraine               | 5561 (UCS-2000, geo 2D)                          | — (no vertical CRS registered)           | — |
| United Kingdom        | 27700 (OSGB36 / British National Grid, projected)| 5701 (ODN height)                        | 7405 (OSGB36 / British National Grid + ODN height) |

## Americas

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Argentina             | 5342 (POSGAR 2007, geo 3D)                       | 9255 (SRVN16 height)                     | 9521 (POSGAR 2007 + SRVN16 height) |
| Bolivia               | 5354 (MARGEN, geo 2D)                            | — (no vertical CRS registered)           | — |
| Brazil                | 4674 (SIRGAS 2000, geo 2D)                       | — (no vertical CRS registered)           | — |
| Canada                | 3979 (NAD83(CSRS) / Canada Atlas Lambert)        | 6647 (CGVD2013(CGG2013) height)          | 6649 (NAD83(CSRS) + CGVD2013(CGG2013) height) |
| Chile                 | 20041 (SIRGAS-Chile 2021, geo 2D)                | — (no vertical CRS registered)           | — |
| Colombia              | 20046 (MAGNA-SIRGAS 2018, geo 2D)                | — (no vertical CRS registered)           | 9377 (MAGNA-SIRGAS 2018 / Origen-Nacional, projected) |
| Ecuador               | 4248 (PSAD56, geo 2D, shared Andean datum) / 24817 & 24877 (PSAD56 / UTM zone 17N/17S) | — (no vertical CRS registered) | — |
| French Guiana         | 2972 (RGFG95 / UTM zone 22N, projected)          | 5755 (NGG1977 height)                    | 9530 (RGFG95 + NGG1977 height) |
| Mexico                | 6365 (Mexico ITRF2008, geo 2D)                   | — (no vertical CRS registered)           | — |
| Peru                  | 5373 (Peru96, geo 2D)                            | — (no vertical CRS registered)           | — |
| Puerto Rico           | 6318 (NAD83(2011), geo 2D)                       | 6641 (PRVD02 height)                     | 9522 (NAD83(2011) + PRVD02 height) |
| United States (CONUS) | 4269 (NAD83, geo 2D) / 5070 (NAD83 / Conus Albers, projected) | 5703 (NAVD88 height)        | 5498 (NAD83 + NAVD88 height) |
| United States - Hawaii| 6628–6632 (NAD83(PA11) / Hawaii zone 1–5)        | 5703 (NAVD88 height, generic US)         | — |
| Uruguay               | 5381 (SIRGAS-ROU98, geo 2D)                      | — (no vertical CRS registered)           | — |
| Venezuela             | 4189 (REGVEN, geo 2D)                            | — (no vertical CRS registered)           | — |

## Caribbean (Dutch Antilles)

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Bonaire               | 10762 (Bonaire 2004, geo 2D)                     | 10763 (Bonaire height)                   | 10765 (Bonaire 2004 + Bonaire height) |
| Sint Eustatius        | 10736 (Sint Eustatius, geo 2D)                   | 10740 (Sint Eustatius height)             | 10741 (Sint Eustatius + Sint Eustatius height) |
| Saba                  | 10636 (Saba, geo 2D)                             | 10642 (Saba height)                      | 10643 (Saba + Saba height) |
| Curaçao               | ESRI:37000 (Curacao_1951, geo 2D) / ESRI:103983 (UTM zone 19N, projected) — no EPSG registration | — | — |
| Aruba                 | no dedicated CRS registered; WGS84 (4326) / UTM zone 19N-20N is used in practice | — | — |
| Sint Maarten          | no dedicated CRS registered; WGS84 (4326) / UTM zone 20N is used in practice | — | — |

Notes:
- Bonaire, Sint Eustatius and Saba ("Caribbean Netherlands" / BES islands) each have their own
  modern EPSG-registered geodetic and vertical reference systems (also available as older
  "BES2020" realizations and local DPnet projected grids, not listed here for brevity).
- Curaçao only has a coordinate system registered under the **ESRI** authority (`Curacao_1951`),
  not under EPSG.
- Aruba and Sint Maarten have no dedicated coordinate reference system registered under EPSG or
  ESRI in this dataset; WGS84-based UTM zones are commonly used in practice instead.

## Africa

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Egypt                 | 4199 (Egypt 1930, geo 2D)                        | — (no vertical CRS registered)           | — |
| Kenya                 | 4210 (Arc 1960, geo 2D, shared with Burundi/Rwanda/Tanzania/Uganda) / 21036 & 21037 (Arc 1960 / UTM zone 36S/37S) | — | — |
| Morocco               | 4261 (Merchich, geo 2D, shared with Western Sahara) / 26191 & 26192 (Merchich / Nord Maroc, Sud Maroc) | — | — |
| Nigeria               | 4263 (Minna, geo 2D)                             | 5796 (Lagos 1955 height)                 | — |
| Reunion               | 2975 (RGR92 / UTM zone 40S, projected)           | 5758 (Reunion 1989 height)               | — |
| South Africa          | 2047–2055 (Hartebeesthoek94 / Lo17–Lo33)         | 9279 (SA LLD height)                     | 9543 (ITRF2005 + SA LLD height) |

## Asia

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| China                 | 4490 (China Geodetic Coordinate System 2000, geo 2D) | 5737 (Yellow Sea 1985 height)         | — |
| India                 | 4146 (Kalianpur 1975, geo 2D) / 24342–24347 (Kalianpur 1975 / UTM zone 42N–47N) | — | — |
| Indonesia             | 9470 (SRGI2013, geo 2D)                          | 20036 (INAGeoid2020 v2 height)           | 20043 (SRGI2013 + INAGeoid2020 v2 height) |
| Iran                  | 4154 (ED50(ED77), geo 2D) / 2058–2061 (ED50(ED77) / UTM zone 38N–41N) | 5752 (Bandar Abbas height) | — |
| Israel                | 2039 (Israel 1993 / Israeli TM Grid, projected)  | — (no vertical CRS registered)            | — |
| Japan                 | 6668 (JGD2011, geo 2D)                           | 6695 (JGD2011 (vertical) height)         | 6697 (JGD2011 + JGD2011 (vertical) height) |
| Malaysia              | 4742 (GDM2000, geo 2D)                           | — (no vertical CRS registered)            | — |
| Pakistan              | 4145 (Kalianpur 1962, geo 2D) / 24311–24313 (Kalianpur 1962 / UTM zone 41N–43N) | — (no vertical CRS registered) | — |
| Philippines           | 4683 (PRS92, geo 2D)                             | — (no vertical CRS registered)            | — |
| Saudi Arabia          | 9333 (KSA-GRF17, geo 2D)                         | 9335 (KSA-VRF14 height)                  | 9520 (KSA-GRF17 + KSA-VRF14 height) |
| Singapore             | 3414 (SVY21 / Singapore TM, projected)           | 6916 (SHD height)                        | 6927 (SVY21 / Singapore TM + SHD height) |
| South Korea           | 5179 (KGD2002 / Unified CS, projected)           | 5193 (KVD1964 height)                    | 10365 (KGD2002 + KVD1964 height) |
| Thailand              | 4240 (Indian 1975, geo 2D) / 24047–24048 (Indian 1975 / UTM zone 47N–48N) | — | — |
| United Arab Emirates  | 4303 (TC(1948), geo 2D) / 30339–30340 (TC(1948) / UTM zone 39N–40N) | 5843 (Ras Ghumays height) | — |
| Vietnam               | 4756 (VN-2000, geo 2D)                           | 5727 (Hon Dau 1992 height)               | — |

## Oceania

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Australia (GDA94)     | 4283 (GDA94, geo 2D)                             | 5711 (AHD height)                        | 9464 (GDA94 + AHD height) |
| Australia (GDA2020)   | 7844 (GDA2020, geo 2D)                           | 9458 (AVWS height)                       | 9462 (GDA2020 + AVWS height) |
| American Samoa        | 6322 (NAD83(PA11), geo 2D)                       | 6643 (ASVD02 height)                     | 9526 (NAD83(PA11) + ASVD02 height) |
| Guam                  | 6637 (NAD83(MA11) / Guam Map Grid, projected)    | 6644 (GUVD04 height)                     | 9524 (NAD83(MA11) + GUVD04 height) |
| New Zealand           | 4167 (NZGD2000, geo 2D) / 2193 (NZGD2000 / NZTM2000, projected) | 7839 (NZVD2016 height)     | 9528 (NZGD2000 + NZVD2016 height) |

## World

| Description                       | Code |
| ----------------------------------:|:----:|
| World geocentric (x,y,z)          | 4978 |
| World Web mercator                | 3857 |
| World geographic 2D - WGS84        | 4326 |
| World geographic 3D - WGS84        | 4979 |
| EGM84 height                       | 5798 |
| EGM96 height                       | 5773 |
| EGM2008 height                     | 3855 |
| EGM2020 height                     | ?    |
