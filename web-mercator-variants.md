# Web Mercator: vendor-specific and deprecated code variants

The "Web Mercator" projection used by virtually all web mapping platforms (Google Maps, OpenStreetMap,
Bing Maps, Mapbox, etc.) has been assigned several different codes over the years, by different
vendors and standards bodies, before the EPSG authority settled on one official, current code.

| Code                | Name                                          | Authority | Status |
|:---------------------|:-----------------------------------------------|:-----------|:---------|
| **EPSG:3857**         | WGS 84 / Pseudo-Mercator                       | EPSG       | ✅ current, official |
| EPSG:900913           | Google Maps Global Mercator                    | EPSG       | ❌ deprecated (informal "GOOGLE" leetspeak code, assigned by the OSGeo community because Google itself never registered an official EPSG code) |
| EPSG:3785             | Popular Visualisation CRS / Mercator           | EPSG       | ❌ deprecated (an earlier EPSG attempt at registering this, superseded by 3857) |
| ESRI:102100           | WGS_1984_Web_Mercator_Auxiliary_Sphere         | ESRI       | ❌ deprecated (Esri's own vendor-specific code, used throughout older ArcGIS versions/services) |

All four codes describe (essentially) the same coordinate system, and most modern GIS software
transparently treats them as equivalent to EPSG:3857. When working with older data, tile servers,
or ArcGIS services you may still encounter `900913` or `102100` in the wild — always prefer
**EPSG:3857** for new work.

See also [`countries-without-epsg.md`](countries-without-epsg.md) for a related phenomenon: countries
without a dedicated EPSG-registered CRS, for which a commercial vendor (mainly Esri) has sometimes
defined its own non-EPSG code.
