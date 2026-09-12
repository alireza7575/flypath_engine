# FlyPath Engine

Shared Python planning engine used by the FlyPath Django website and QGIS
plugin. The package contains no Django, QGIS, UI, persistence, or KMZ code.

The first migration slice provides 2D survey-grid generation, automatic
direction selection, route ordering, flight splitting, and WGS84 ellipsoidal
area and distance measurements.

Run the checks with a Python environment containing Shapely and pyproj:

```powershell
python tests/test_route.py
python tests/test_grid.py
python tests/test_measurements.py
```
