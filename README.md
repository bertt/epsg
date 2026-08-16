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

For countries and territories that have no dedicated EPSG-registered CRS at all, see
[`countries-without-epsg.md`](countries-without-epsg.md).

## Europe

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Albania               | 2462 (Albanian 1987 / Gauss-Kruger zone 4); 6870 (ETRS89-ALB [KRGJSH] / Albania TM 2010); 6962 (ETRS89-ALB [KRGJSH] / Albania LCC 2010) | 5777 (Durres height) | — |
| Austria               | 31287 (MGI / Austria Lambert, projected)         | 5778 (GHA height)                        | 9501 (MGI + EVRF2000 Austria height) |
| Belarus               | — (no EPSG-registered CRS; WGS84/UTM used in practice) | — | — |
| Belgium               | 31370 (BD72 / Belgian Lambert 72, projected)     | 5710 (Ostend height)                     | 6190 (BD72 / Belgian Lambert 72 + Ostend height) |
| Bosnia and Herzegovina| 10329 (ETRS89-BIH [BH_ETRS89] / TM, projected)   | —                                        | — |
| Bulgaria              | 7801 (ETRS89-BGR [BGS2005] / CCS2005, projected) | 9669 (BGS2005 height)                    | — |
| Croatia               | 3765 (ETRS89-HRV [HTRS96] / Croatia TM, projected) | 5610 (HVRS71 height)                   | — |
| Cyprus                | 6312 (CGRS93 / Cyprus Local Transverse Mercator, projected) | 7446 (Famagusta 1960 height)     | 10865 (CGRS93 + Famagusta 1960 height) |
| Czechia               | 5515 (S-JTSK/05 / Modified Krovak, projected)    | — (Baltic 1957 height not separately registered) | 11311 (ETRS89-CZE [2007] + Baltic 1957 height) |
| Denmark               | 4095 (ETRS89 / DKTM3, projected)                 | 5799 (DVR90 height)                      | 4099 (ETRS89 / DKTM3 + DVR90 height) |
| Estonia               | 3301 (Estonian CS of 1997, projected)            | 9663 (EH2000 height)                     | — |
| Europe (ETRS89)       | 4936 (ETRS89, geocentric) / 4258 (geo 2D)        | 5621 (EVRF2007 height)                    | 4937 (ETRS89, geographic 3D) / 7423 (ETRS89 + EVRF2007 height) |
| Faroe Islands         | 3144 (FD54 / Faroe Lambert, projected); 3145 (ETRS89-FRO [2008] / Faroe Lambert, projected) | 5317 (FVR09 height) | 11120 (ETRS89-FRO [2008] + FVR09 height) |
| Finland               | 3067 (ETRS89-FIN / TM35FIN(E,N), projected)      | 3900 (N2000 height)                      | 3903 (ETRS89-FIN / TM35FIN(N,E) + N2000 height) |
| France (mainland)     | 2154 (ETRS89-FRA / Lambert-93, projected)        | 5720 (NGF-IGN69 height)                  | 5698 (ETRS89-FRA / Lambert-93 + NGF-IGN69 height) |
| France - Corsica      | 2154 (ETRS89-FRA / Lambert-93, projected)        | 5721 (NGF-IGN78 height)                  | 5699 (ETRS89-FRA / Lambert-93 + NGF-IGN78 height) |
| Germany               | 25832 (ETRS89 / UTM zone 32N, projected)         | 5783 (DHHN92 height)                     | 5555 (ETRS89 / UTM zone 32N + DHHN92 height) |
| Greece                | 2100 (GGRS87 / Greek Grid, projected)            | 5716 (Piraeus height)                    | — |
| Greenland             | 2216 (Qoornoq 1927 / UTM zone 22N, projected); 2217 (Qoornoq 1927 / UTM zone 23N, projected) | 10565 (GLLMSL(2022) height) | 10652 (GR96 + GLLAT(2023) depth) |
| Guernsey              | 3108 (ETRS89 / Guernsey Grid, projected)         | —                                        | — |
| Hungary               | 23700 (HD72 / EOV, projected)                    | 5787 (EOMA 1980 height)                  | 10660 (HD72 / EOV + EOMA 1980 height) |
| Iceland               | 3057 (ISN93 / Lambert 1993, projected)           | 8089 (ISH2004 height)                    | 9951 (ISN93 / Lambert 1993 + ISH2004 height) |
| Ireland               | 29902 (TM65 / Irish Grid, projected)             | —            | — |
| Isle of Man           | — (no EPSG-registered CRS; WGS84/UTM used in practice) | 5750 (Douglas height) | 9429 (ETRS89 + Douglas height) |
| Italy                 | 6706 (ETRS89-ITA [RDN2008], geo 2D)              | 5214 (Genoa 1942 height)                 | 9723 (ETRS89-ITA [RDN2008] + Genoa 1942 height) |
| Jersey                | 3109 (ETRS89 / Jersey Transverse Mercator, projected) | — | — |
| Latvia                | 3059 (ETRS89-LVA [LKS-92] / Latvia TM, projected); 10306 (ETRS89-LVA [LKS-2020] / Latvia TM, projected) | 7700 (Latvia 2000 height) | 10839 (ETRS89-LVA [LKS-2020] + Latvia 2000 height) |
| Liechtenstein         | 21782 (CH1903 / LV03C-G, projected)              | —                                        | — |
| Lithuania             | 3346 (ETRS89-LTU [LKS94] / Lithuania TM, projected) | 9666 (LAS07 height)                    | — |
| Luxembourg            | 2169 (LUREF / Luxembourg TM, projected)          | 5774 (NG95 height)                       | 9897 (LUREF / Luxembourg TM + NG95 height) |
| Moldova               | 4026 (ETRS89-MDA [MOLDREF99] / Moldova TM, projected); 4037 (WGS 84 / TMzn35N, projected) | — | — |
| Netherlands           | 28992 (Amersfoort / RD New, projected)           | 5709 (NAP height)                        | 7415 (Amersfoort / RD New + NAP height) |
| North Macedonia       | 6204 (Macedonia State Coordinate System, projected); 9945 (Macedonia State Coordinate System truncated, projected) | — | — |
| Norway                | 25832 (ETRS89-NOR, projected) / 10875 (geo 2D)   | 5941 (NN2000 height)                     | 5942 (ETRS89-NOR [EUREF89] + NN2000 height) |
| Poland                | 2180 (ETRS89 / PL-1992, projected)               | 9650 (Baltic 1986 height)                | 9656 (ETRS89-POL [PL-ETRF2000] + Baltic 1986 height) |
| Portugal (mainland)   | 3763 (ETRS89-PRT [1995] / Portugal TM06)         | 5780 (Cascais height)                    | 10545 (ETRS89-PRT [1995] + Cascais height) |
| Romania               | 3844 (Pulkovo 1942(58) / Stereo70, projected)    | 5781 (Constanta height)                  | — |
| Russia                | 7683 (GSK-2011, geo 2D)                          | —            | — |
| Serbia                | 8682 (ETRS89-SRB [STRS00] / UTM zone 34N, projected) | 8691 (SRB_VRS12 height)              | — |
| Slovakia              | 8352 (S-JTSK [JTSK03] / Krovak, projected)       | — (Baltic 1957 height not separately registered) | 11312 (ETRS89-SVK [SKTRF09] + Baltic 1957 height) |
| Slovenia              | 4765 (ETRS89-SVN [D96], geo 2D)                  | 8690 (SVS2010 height)                    | 10245 (ETRS89-SVN [D96] + SVS2010 height) |
| Spain (mainland)      | 11134 (ETRS89-ESP [REGENTE], geo 2D)             | 5782 (Alicante height)                   | 9505 (ETRS89-ESP [REGENTE] + Alicante height) |
| Spain - Canary Islands| 4081 (REGCAN95, geo 2D)                          | 9397 (Gran Canaria height)               | 9512 (REGCAN95 + Gran Canaria height) |
| Sweden                | 3006 (ETRS89-SWE [SWEREF 99 TM], projected)      | 5613 (RH2000 height)                     | 5628 (ETRS89-SWE [SWEREF 99] + RH2000 height) |
| Switzerland           | 2056 (CH1903+ / LV95, projected)                 | 5729 (LHN95 height)                      | — |
| Turkey                | 5636 (TUREF / LAEA Europe, projected)            | 5775 (Antalya height)                    | — |
| Ukraine               | 5561 (UCS-2000, geo 2D)                          | —            | — |
| United Kingdom        | 27700 (OSGB36 / British National Grid, projected)| 5701 (ODN height)                        | 7405 (OSGB36 / British National Grid + ODN height) |

## Americas

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Anguilla              | 2000 (Anguilla 1957 / British West Indies Grid, projected) | — | — |
| Argentina             | 5342 (POSGAR 2007, geo 3D)                       | 9255 (SRVN16 height)                     | 9521 (POSGAR 2007 + SRVN16 height) |
| Barbados              | 21291 (Barbados 1938 / British West Indies Grid, projected); 21292 (Barbados 1938 / Barbados National Grid, projected) | — | — |
| Belize                | 5589 (Sibun Gorge 1922 / Colony Grid, projected) | — | — |
| Bermuda               | 3769 (Bermuda 1957 / UTM zone 20N, projected); 3770 (BDA2000 / Bermuda 2000 National Grid, projected) | — | — |
| Bolivia               | 5354 (MARGEN, geo 2D)                            | —            | — |
| Brazil                | 4674 (SIRGAS 2000, geo 2D)                       | —            | — |
| Canada                | 3979 (NAD83(CSRS) / Canada Atlas Lambert)        | 6647 (CGVD2013(CGG2013) height)          | 6649 (NAD83(CSRS) + CGVD2013(CGG2013) height) |
| Cayman Islands        | 6128 (Grand Cayman National Grid 1959); 6129 (Sister Islands National Grid 1961); 6391 (Cayman Islands National Grid 2011) | 6132 (CBVD61 height (ft)) | 9504 (CIGD11 + LCVD61 height (ft)) |
| Chile                 | 20041 (SIRGAS-Chile 2021, geo 2D)                | —            | — |
| Colombia              | 20046 (MAGNA-SIRGAS 2018, geo 2D)                | —            | 9377 (MAGNA-SIRGAS 2018 / Origen-Nacional, projected) |
| Costa Rica            | 5456 (Ocotepeque 1935 / Costa Rica Norte, projected); 5457 (Ocotepeque 1935 / Costa Rica Sur, projected) | 8911 (DACR52 height) | 8912 (CR-SIRGAS epoch 2014.59 / CRTM05 + DACR52 height) |
| Cuba                  | 3795 (NAD27 / Cuba Norte, projected); 3796 (NAD27 / Cuba Sur, projected) | — | — |
| Dominica              | 2002 (Dominica 1945 / British West Indies Grid, projected) | — | — |
| Ecuador               | 4248 (PSAD56, geo 2D, shared Andean datum) / 24817 & 24877 (PSAD56 / UTM zone 17N/17S) | —  | — |
| El Salvador           | 5460 (Ocotepeque 1935 / El Salvador Lambert, projected) | — | — |
| French Guiana         | 2972 (RGFG95 / UTM zone 22N, projected)          | 5755 (NGG1977 height)                    | 9530 (RGFG95 + NGG1977 height) |
| Grenada               | 2003 (Grenada 1953 / British West Indies Grid, projected) | — | — |
| Guadeloupe            | 2969 (Fort Marigot / UTM zone 20N, projected); 2970 (Guadeloupe 1948 / UTM zone 20N, projected) | 9130 (IGN 2008 LD height) | 9542 (RRAF 1991 + IGN 2008 LD height) |
| Guatemala             | 5459 (Ocotepeque 1935 / Guatemala Sur, projected); 5559 (Ocotepeque 1935 / Guatemala Norte, projected) | — | — |
| Jamaica               | 3448 (JAD2001 / Jamaica Metric Grid, projected); 3449 (JAD2001 / UTM zone 17N, projected) | — | — |
| Martinique            | 2973 (Martinique 1938 / UTM zone 20N, projected) | 5794 (Martinique 1955 height)            | 10633 (RGAF09 / UTM zone 20N + Martinique 1987 height) |
| Mexico                | 6365 (Mexico ITRF2008, geo 2D)                   | —            | — |
| Montserrat            | 2004 (Montserrat 1958 / British West Indies Grid, projected) | — | — |
| Nicaragua             | 5461 (Ocotepeque 1935 / Nicaragua Norte, projected); 5462 (Ocotepeque 1935 / Nicaragua Sur, projected) | — | — |
| Panama                | 5469 (Panama-Colon 1911 / Panama Lambert, projected); 5472 (Panama-Colon 1911 / Panama Polyconic, projected) | — | — |
| Peru                  | 5373 (Peru96, geo 2D)                            | —            | — |
| Puerto Rico           | 6318 (NAD83(2011), geo 2D)                       | 6641 (PRVD02 height)                     | 9522 (NAD83(2011) + PRVD02 height) |
| Suriname              | 31121 (Zanderij / UTM zone 21N, projected); 31154 (Zanderij / TM 54 NW, projected) | — | — |
| Trinidad and Tobago   | 2066 (Mount Dillon / Tobago Grid, projected); 2067 (Naparima 1955 / UTM zone 20N, projected) | — | — |
| United States (CONUS) | 4269 (NAD83, geo 2D) / 5070 (NAD83 / Conus Albers, projected) | 5703 (NAVD88 height)        | 5498 (NAD83 + NAVD88 height) |
| United States - Hawaii| 6628–6632 (NAD83(PA11) / Hawaii zone 1–5)        | 5703 (NAVD88 height, generic US)         | — |
| Uruguay               | 5381 (SIRGAS-ROU98, geo 2D)                      | —            | — |
| Venezuela             | 4189 (REGVEN, geo 2D)                            | —            | — |

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
| Algeria               | 22229 (RGSH2020 / UTM zone 29N, projected); 22230 (RGSH2020 / UTM zone 30N, projected) | 10190 (NGA 2022 height) | — |
| Angola                | 5842 (WGS 84 / TM 12 SE, projected); 9156 (RSAO13 / UTM zone 32S, projected) | — | — |
| Botswana              | 22234 (Cape / UTM zone 34S, projected); 22235 (Cape / UTM zone 35S, projected) | — | — |
| Cameroon              | 2215 (Manoca 1962 / UTM zone 32N, projected); 2312 (Garoua / UTM zone 33N, projected) | — | — |
| Comoros               | 2999 (Grand Comoros / UTM zone 38S, projected)   | — | — |
| Djibouti              | 4713 (Ayabelle Lighthouse, geo 2D)               | — | — |
| Egypt                 | 4199 (Egypt 1930, geo 2D)                        | —            | — |
| Equatorial Guinea     | 6883 (Bioko, geo 2D)                             | — | — |
| Eritrea               | 26237 (Massawa / UTM zone 37N, projected)        | — | — |
| Ethiopia              | 20138 (Adindan / UTM zone 38N, projected)        | — | — |
| Gabon                 | 5223 (WGS 84 / Gabon TM, projected); 26632 (M'poraloko / UTM zone 32N, projected) | — | — |
| Gambia                | 6894 (Gambia, geo 2D)                            | — | — |
| Ghana                 | 2136 (Accra / Ghana National Grid, projected); 2137 (Accra / TM 1 NW, projected) | — | — |
| Guinea                | 3461 (Dabola 1981 / UTM zone 28N, projected); 3462 (Dabola 1981 / UTM zone 29N, projected) | — | — |
| Guinea-Bissau         | 2095 (Bissau / UTM zone 28N, projected)          | — | — |
| Kenya                 | 4210 (Arc 1960, geo 2D, shared with Burundi/Rwanda/Tanzania/Uganda) / 21036 & 21037 (Arc 1960 / UTM zone 36S/37S) | — | — |
| Liberia               | 10801 (LibRef21 / UTM zone 28N, projected); 10802 (LibRef21 / UTM zone 29N, projected) | — | — |
| Libya                 | 2077 (ELD79 / UTM zone 32N, projected); 2078 (ELD79 / UTM zone 33N, projected) | — | — |
| Madagascar            | 8441 (Tananarive / Laborde Grid, projected); 29701 (Tananarive (Paris) / Laborde Grid, projected) | — | — |
| Mauritania            | 3343 (Mauritania 1999 / UTM zone 28N, projected); 3344 (Mauritania 1999 / UTM zone 29N, projected) | — | — |
| Mauritius             | 3337 (Le Pouce 1934 / Mauritius Grid, projected) | — | — |
| Mayotte               | 2980 (Combani 1950 / UTM zone 38S, projected); 4471 (RGM04 / UTM zone 38S, projected) | 11157 (IGN 2023 Mayotte height) | 11158 (RGM23 + IGN 2023 Mayotte height) |
| Morocco               | 4261 (Merchich, geo 2D, shared with Western Sahara) / 26191 & 26192 (Merchich / Nord Maroc, Sud Maroc) | — | — |
| Mozambique            | 2736 (Tete / UTM zone 36S, projected); 2737 (Tete / UTM zone 37S, projected) | 5722 (Maputo height) | — |
| Namibia               | 29333 (Schwarzeck / UTM zone 33S, projected)     | — | — |
| Niger                 | 2931 (Beduaram / TM 13 NE, projected)            | — | — |
| Nigeria               | 4263 (Minna, geo 2D)                             | 5796 (Lagos 1955 height)                 | — |
| Reunion               | 2975 (RGR92 / UTM zone 40S, projected)           | 5758 (Reunion 1989 height)               | — |
| Sao Tome and Principe | 4824 (Principe, geo 2D)                          | — | — |
| Senegal               | 31028 (Yoff / UTM zone 28N, projected)           | — | — |
| Seychelles            | 6915 (South East Island 1943 / UTM zone 40N, projected) | — | — |
| Sierra Leone          | 2159 (Sierra Leone 1924 / New Colony Grid, projected); 2160 (Sierra Leone 1924 / New War Office Grid, projected) | — | — |
| Somalia               | 20538 (Afgooye / UTM zone 38N, projected); 20539 (Afgooye / UTM zone 39N, projected) | — | — |
| South Africa          | 2047–2055 (Hartebeesthoek94 / Lo17–Lo33)         | 9279 (SA LLD height)                     | 9543 (ITRF2005 + SA LLD height) |
| Togo                  | 25231 (Lome / UTM zone 31N, projected)           | — | — |
| Tunisia               | 2088 (Carthage / TM 11 NE, projected); 22300 (Carthage (Paris) / Tunisia Mining Grid, projected) | — | — |
| Uganda                | 10792 (UGRF / UTM zone 35N, projected); 10793 (UGRF / UTM zone 36N, projected) | — | — |
| Western Sahara        | 26194 (Merchich / Sahara Nord, projected); 26195 (Merchich / Sahara Sud, projected) | — | — |

## Asia

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Afghanistan           | 4255 (Herat North, geo 2D)                       | — | — |
| Armenia               | — (no EPSG-registered CRS; WGS84/UTM used in practice) | — | — |
| Azerbaijan            | — (no EPSG-registered CRS; WGS84/UTM used in practice) | 5797 (AIOC95 height) | — |
| Bahrain               | 20499 (Ain el Abd / Bahrain Grid, projected)     | — | — |
| Bangladesh            | 3106 (Gulshan 303 / TM 90 NE, projected); 9678 (Gulshan 303 / Bangladesh Transverse Mercator, projected) | 9681 (NVD 1992 height) | — |
| Bhutan                | 5266 (DRUKREF 03 / Bhutan National Grid, projected); 5292 (DRUKREF 03 / Bumthang TM, projected) | — | — |
| China                 | 4490 (China Geodetic Coordinate System 2000, geo 2D) | 5737 (Yellow Sea 1985 height)         | — |
| Georgia               | 10833 (Georgia Geodetic Datum / Lambert, projected); 10836 (Georgia Geodetic Datum / UTM zone 37N (N-E), projected) | 5735 (Black Sea height) | — |
| Hong Kong             | 2326 (Hong Kong 1980 Grid System, projected); 3407 (Hong Kong 1963 Grid System, projected) | 5738 (HKPD height) | — |
| India                 | 4146 (Kalianpur 1975, geo 2D) / 24342–24347 (Kalianpur 1975 / UTM zone 42N–47N) | — | — |
| Indonesia             | 9470 (SRGI2013, geo 2D)                          | 20036 (INAGeoid2020 v2 height)           | 20043 (SRGI2013 + INAGeoid2020 v2 height) |
| Iran                  | 4154 (ED50(ED77), geo 2D) / 2058–2061 (ED50(ED77) / UTM zone 38N–41N) | 5752 (Bandar Abbas height) | — |
| Iraq                  | 3391 (Karbala 1979 / UTM zone 37N, projected); 3392 (Karbala 1979 / UTM zone 38N, projected) | 3886 (Fao 1979 height) | — |
| Israel                | 2039 (Israel 1993 / Israeli TM Grid, projected)  | —             | — |
| Japan                 | 6668 (JGD2011, geo 2D)                           | 6695 (JGD2011 (vertical) height)         | 6697 (JGD2011 + JGD2011 (vertical) height) |
| Jordan                | 3066 (ED50 / Jordan TM, projected)                | — | — |
| Kazakhstan            | 10942–10949 (QazTRF-23 / Gauss-Kruger zone 8–15, projected) | — | — |
| Kuwait                | 24600 (KOC Lambert, projected); 31838 (NGN / UTM zone 38N, projected) | 7979 (KOC WD height) | — |
| Kyrgyzstan            | 7692–7696 (Kyrg-06 / zone 1–5, projected)        | — | — |
| Laos                  | 4678 (Lao 1997, geo 2D)                          | — | — |
| Lebanon               | 6882 (Bekaa Valley 1920, geo 2D)                 | — | — |
| Macao                 | 8433 (Macao 1920 / Macao Grid, projected)        | 8434 (Macao height)                      | — |
| Malaysia              | 4742 (GDM2000, geo 2D)                           | —             | — |
| Maldives              | 4684 (Gan 1970, geo 2D)                          | — | — |
| Nepal                 | 6207 (Nepal 1981, geo 2D)                        | — | — |
| Oman                  | 3439 (PSD93 / UTM zone 39N, projected); 3440 (PSD93 / UTM zone 40N, projected) | 5725 (Fahud HD height) | 7410 (PSHD93) |
| Pakistan              | 4145 (Kalianpur 1962, geo 2D) / 24311–24313 (Kalianpur 1962 / UTM zone 41N–43N) | —  | — |
| Philippines           | 4683 (PRS92, geo 2D)                             | —             | — |
| Qatar                 | 2099 (Qatar 1948 / Qatar Grid, projected); 2932 (QND95 / Qatar National Grid, projected) | — | — |
| Saudi Arabia          | 9333 (KSA-GRF17, geo 2D)                         | 9335 (KSA-VRF14 height)                  | 9520 (KSA-GRF17 + KSA-VRF14 height) |
| Singapore             | 3414 (SVY21 / Singapore TM, projected)           | 6916 (SHD height)                        | 6927 (SVY21 / Singapore TM + SHD height) |
| South Korea           | 5179 (KGD2002 / Unified CS, projected)           | 5193 (KVD1964 height)                    | 10365 (KGD2002 + KVD1964 height) |
| Sri Lanka             | 5234 (Kandawala / Sri Lanka Grid, projected)     | 5237 (SLVD height)                       | — |
| Syria                 | 22770 (Deir ez Zor / Syria Lambert, projected)   | —                                         | — |
| Taiwan                | 3829 (Hu Tzu Shan 1950 / UTM zone 51N, projected) | 8904 (TWVD 2001 height)                  | — |
| Thailand              | 4240 (Indian 1975, geo 2D) / 24047–24048 (Indian 1975 / UTM zone 47N–48N) | — | — |
| United Arab Emirates  | 4303 (TC(1948), geo 2D) / 30339–30340 (TC(1948) / UTM zone 39N–40N) | 5843 (Ras Ghumays height) | — |
| Uzbekistan            | 10726–10729 (UZGD2024 / UzREF24 zone 40–43, projected) | — | — |
| Vietnam               | 4756 (VN-2000, geo 2D)                           | 5727 (Hon Dau 1992 height)               | — |
| Yemen                 | 2089 (Yemen NGN96 / UTM zone 38N, projected); 2090 (Yemen NGN96 / UTM zone 39N, projected) | — | — |

## Oceania

| Area                  | Horizontal (name)                                | Vertical (name)                          | Compound (name) |
| ---------------------:|:---------------------------------------------------|:--------------------------------------------|:-----------------|
| Australia (GDA94)     | 4283 (GDA94, geo 2D)                             | 5711 (AHD height)                        | 9464 (GDA94 + AHD height) |
| Australia (GDA2020)   | 7844 (GDA2020, geo 2D)                           | 9458 (AVWS height)                       | 9462 (GDA2020 + AVWS height) |
| American Samoa        | 6322 (NAD83(PA11), geo 2D)                       | 6643 (ASVD02 height)                     | 9526 (NAD83(PA11) + ASVD02 height) |
| Fiji                  | 3139 (Vanua Levu 1915 / Vanua Levu Grid, projected); 3140 (Viti Levu 1912 / Viti Levu Grid, projected) | — | — |
| French Polynesia      | 2976 (Tahiti 52 / UTM zone 6S, projected); 2977 (Tahaa 54 / UTM zone 5S, projected) | 5607 (Bora Bora SAU 2001 height) | — |
| Guam                  | 6637 (NAD83(MA11) / Guam Map Grid, projected)    | 6644 (GUVD04 height)                     | 9524 (NAD83(MA11) + GUVD04 height) |
| Kiribati              | 4716 (Phoenix Islands 1966, geo 2D)              | — | — |
| Micronesia            | 3295 (Guam 1963 / Yap Islands, projected)        | — | — |
| New Caledonia         | 2981 (IGN56 Lifou / UTM zone 58S, projected); 2995 (IGN53 Mare / UTM zone 58S, projected) | 9351 (NGNC08 height) | 10318 (RGNC15 (lon-lat) + NGNC08 height) |
| New Zealand           | 4167 (NZGD2000, geo 2D) / 2193 (NZGD2000 / NZTM2000, projected) | 7839 (NZVD2016 height)     | 9528 (NZGD2000 + NZVD2016 height) |
| Northern Mariana Islands | — (no EPSG-registered CRS; WGS84/UTM used in practice) | 6640 (NMVD03 height) | 9525 (NAD83(MA11) + NMVD03 height) |
| Papua New Guinea      | 5550–9875 (PNG94 / PNGMG94 zone 54–58, projected) | 7841 (POM08 height)                     | — |
| Solomon Islands       | 4718 (Solomon 1968, geo 2D)                      | — | — |
| Tonga                 | 5887 (TGD2005 / Tonga Map Grid, projected)       | — | — |
| Vanuatu               | 4730 (Santo 1965, geo 2D)                        | — | — |
| Wallis and Futuna     | 2988 (MOP78 / UTM zone 1S, projected); 8903 (RGWF96 / UTM zone 1S, projected) | — | — |

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
