# FlyPath Engine

Shared Python planning engine used by the FlyPath Django website and QGIS
plugin. The package contains no Django, QGIS, UI, persistence, or KMZ code.

Releases through `v0.3.0` provide 2D survey-grid generation, automatic
direction selection, route ordering, split helpers, WGS84 ellipsoidal area and
distance measurements, route-level statistics, and versioned drone profiles.
Complete split policy, recovery/per-flight totals, photo actions, and the full
versioned `plan_2d` result remain to be migrated.

Run the checks with a Python environment containing Shapely and pyproj:

```powershell
python tests/test_route.py
python tests/test_grid.py
python tests/test_measurements.py
python tests/test_statistics.py
python tests/test_profiles.py
```

CI runs on Linux x64, Windows x64, and macOS ARM with Python 3.10 and
3.13 coverage. macOS Intel remains part of the QGIS installation matrix because
GitHub does not provide a standard Intel runner for this private repository.
