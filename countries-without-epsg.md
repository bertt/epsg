# Countries and territories without a dedicated EPSG CRS

Most countries have their own nationally registered horizontal (and often vertical) coordinate
reference system in the EPSG Geodetic Parameter Dataset (see the [main table](README.md)).
However, a number of countries and territories do **not** have any dedicated CRS registered
under the **EPSG** authority at all — this list was verified directly against `proj.db`
(the SQLite database PROJ/QGIS/GDAL distribute from the official EPSG dataset, v12.049): for
each country below there is no `geodetic_crs`, `projected_crs`, `vertical_crs` or
`compound_crs` row whose area of use (`extent`) matches that country.

In practice, these countries use a generic global system instead — almost always **WGS84
(EPSG:4326)** in geographic form, or a **WGS84 / UTM zone** projected CRS (EPSG:326xx for the
northern hemisphere, EPSG:327xx for the southern hemisphere) for mapping and GIS work. The UTM
zone that is actually used depends on the exact longitude of the area of interest within the
country; the zone(s) below are the ones that cover most of the country's territory.

| Country / territory                     | Most used alternative                              |
|:-----------------------------------------|:----------------------------------------------------|
| Antigua and Barbuda                      | WGS84 / UTM zone 20N (EPSG:32620) |
| Armenia                                  | WGS84 / UTM zone 38N (EPSG:32638) |
| Aruba                                    | WGS84 / UTM zone 19N (EPSG:32619) |
| Bahamas                                  | WGS84 / UTM zone 17N–18N (EPSG:32617 / 32618) |
| Belarus                                  | WGS84 / UTM zone 35N (EPSG:32635) |
| Benin                                    | WGS84 / UTM zone 31N (EPSG:32631) |
| Burkina Faso                             | WGS84 / UTM zone 30N (EPSG:32630) |
| Burundi                                  | WGS84 / UTM zone 35S (EPSG:32735) |
| Cabo Verde                                | WGS84 / UTM zone 26N (EPSG:32626) |
| Cambodia                                 | WGS84 / UTM zone 48N–49N (EPSG:32648 / 32649) |
| Central African Republic                 | WGS84 / UTM zone 33N–35N |
| Chad                                     | WGS84 / UTM zone 33N–34N |
| Cook Islands                             | WGS84 / UTM zone 5S–6S |
| Côte d'Ivoire                            | WGS84 / UTM zone 29N–30N |
| Curaçao                                  | WGS84 / UTM zone 19N (EPSG:32619) — ESRI:37000 (Curacao_1951) also used |
| Dominican Republic                       | WGS84 / UTM zone 19N (EPSG:32619) |
| Eswatini                                 | WGS84 / UTM zone 36S (EPSG:32736) |
| Haiti                                    | WGS84 / UTM zone 18N (EPSG:32618) |
| Honduras                                 | WGS84 / UTM zone 16N (EPSG:32616) |
| Lesotho                                  | WGS84 / UTM zone 35S (EPSG:32735) |
| Malawi                                   | WGS84 / UTM zone 36S (EPSG:32736) |
| Mali                                     | WGS84 / UTM zone 29N–31N |
| Marshall Islands                         | WGS84 / UTM zone 58N–59N |
| Mongolia                                 | WGS84 / UTM zone 46N–50N |
| Myanmar                                  | WGS84 / UTM zone 46N–47N |
| Nauru                                    | WGS84 / UTM zone 58N (EPSG:32658) |
| Niue                                     | WGS84 / UTM zone 2S (EPSG:32702) |
| North Korea                              | WGS84 / UTM zone 52N (EPSG:32652) |
| Palau                                    | WGS84 / UTM zone 53N (EPSG:32653) |
| Rwanda                                   | WGS84 / UTM zone 35S–36S |
| Saint Kitts and Nevis                    | WGS84 / UTM zone 20N (EPSG:32620) |
| Saint Lucia                              | WGS84 / UTM zone 20N (EPSG:32620) |
| Saint Pierre and Miquelon                | WGS84 / UTM zone 21N (EPSG:32621) |
| Saint Vincent and the Grenadines         | WGS84 / UTM zone 20N (EPSG:32620) |
| Samoa                                    | WGS84 / UTM zone 2S (EPSG:32702) |
| Sint Maarten                             | WGS84 / UTM zone 20N (EPSG:32620) |
| South Sudan                              | WGS84 / UTM zone 35N–36N |
| Sudan                                    | WGS84 / UTM zone 35N–36N |
| Tajikistan                               | WGS84 / UTM zone 41N–43N |
| Tanzania                                 | WGS84 / UTM zone 36S–37S |
| Timor-Leste                              | WGS84 / UTM zone 51S–52S (EPSG:32751 / 32752) |
| Tokelau                                  | WGS84 / UTM zone 1S (EPSG:32701) |
| Turkmenistan                             | WGS84 / UTM zone 40N–42N |
| Turks and Caicos Islands                 | WGS84 / UTM zone 19N (EPSG:32619) |
| Tuvalu                                   | WGS84 / UTM zone 60N (EPSG:32660) |
| Zambia                                   | WGS84 / UTM zone 35S–36S |
| Zimbabwe                                 | WGS84 / UTM zone 35S–36S |

## Countries using a regional EPSG CRS in practice

The countries below also have no EPSG CRS registered specifically for their own territory, but
unlike the countries above they commonly use a well-known **regional** EPSG-registered CRS in
practice — usually **ETRS89 / UTM zone** (Europe) or **SIRGAS 2000 / UTM zone** (South America)
— rather than a generic WGS84/UTM zone.

| Country / territory                     | Regional EPSG CRS commonly used                    |
|:-----------------------------------------|:----------------------------------------------------|
| Andorra                                  | EPSG:25831 (ETRS89 / UTM zone 31N) |
| Gibraltar                                | EPSG:25830 (ETRS89 / UTM zone 30N) |
| Guyana                                   | EPSG:31975 (SIRGAS 2000 / UTM zone 21N) |
| Malta                                    | EPSG:25833 (ETRS89 / UTM zone 33N) |
| Monaco                                   | EPSG:25832 (ETRS89 / UTM zone 32N) |
| Montenegro                               | EPSG:25834 (ETRS89 / UTM zone 34N) |
| Paraguay                                 | EPSG:31981 (SIRGAS 2000 / UTM zone 21S) |
| San Marino                               | EPSG:25833 (ETRS89 / UTM zone 33N) |

Notes:
- Where a UTM zone range is listed, the exact zone depends on the longitude of the specific
  part of the country.
- Several of these countries do have a legacy local datum from historical surveys, but these
  are not registered in the EPSG dataset, so WGS84/UTM is the de facto standard used today.
- Curaçao has a coordinate system registered under the **ESRI** authority (`Curacao_1951`),
  not under EPSG.
- This list was generated by querying `proj.db` (bundled with QGIS/PROJ) for every country
  without any EPSG `geodetic_crs`, `projected_crs`, `vertical_crs` or `compound_crs` entry
  whose area of use matches that country. See the [main table](README.md) for all countries
  that do have a dedicated EPSG registration, including small/less common ones added following
  the same method.
