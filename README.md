# FlyPath Engine

Shared Python planning engine used by the FlyPath Django website and QGIS
plugin. The package contains no Django, QGIS, UI, persistence, or KMZ code.

The unreleased `v0.4.0` candidate adds the versioned `plan_2d(request)` boundary for 2D routes,
time-balanced flights, explicit capture actions, complete/known estimates,
warnings, and export validation. The lower-level geometry, measurement,
statistics, split-helper, and profile APIs remain available.

The local website and plugin adapters consume the `v0.4.0` candidate. The plugin
vendors the tagged source; the website development environment uses its built
wheel. The tag has not been published remotely. See
[integration status](docs/integration-status.md) for tested behavior and remaining
release, database-cleanup, and installation work.

Run the checks with a Python environment containing Shapely and pyproj:

```powershell
python tests/test_route.py
python tests/test_grid.py
python tests/test_measurements.py
python tests/test_statistics.py
python tests/test_profiles.py
python tests/test_planning.py
```

With the website virtual environment, compare both local adapters and their
consumer KMZ output without accessing the application database:

```powershell
python tools/check_consumer_parity.py --website ../flypath.io --plugin ../FlyPath
```

CI runs on Linux x64, Windows x64, and macOS ARM with Python 3.10 and
3.13 coverage. macOS Intel remains part of the QGIS installation matrix because
GitHub does not provide a standard Intel runner for this private repository.
