# First-phase integration status — 2026-09-16

## Local implementation

The engine candidate is `v0.4.0`, contract/profile version 1, at source commit
`050d368a59d2be903605099fe3cd006e917507e9`. The tag is local only. No remote
release, deployment, or application-database migration was performed.

- Engine: complete 2D route, deterministic direction/order, time-balanced flight
  splitting, explicit camera actions, profile-owned intervals, known/complete
  distances and estimates, limits, warnings, and structured validation.
- Plugin: pinned source vendored through its verification tool; one planning
  result supplies 2D preview, statistics, flights, and consumer export. Splitting
  has an on/off control and defaults on for new missions. Imported routes remain
  unchanged until explicit Preview regeneration; dirty routes cannot be exported
  or uploaded with stale provenance.
- Website: `/api/plan/` adapts the same engine contract. Saved requests/results
  retain versions and provenance. Browser rendering and consumer KMZ serialization
  consume engine flights/actions; legacy corridor/terrain behavior is retained.
- Aircraft values: website active planning/validation/export paths use engine
  profiles. Website metadata remains editable. The old database calculation
  columns remain stored but are no longer calculation authority.
- Plugin defects: changing the area invalidates the previous cached route;
  terrain heights used for preview/export stay together; corridor sampling covers
  segment interiors; terrain failures block export, including failures during
  export preparation. Imported terrain routes require Preview to reacquire heights.

Legacy browser/plugin calculations are retained only where still required for
terrain, corridor, or legacy saved routes. A broad UI rewrite is deferred.

## Verification

- All six engine direct test scripts pass, including 12 planning acceptance cases
  and 14 route cases. The wheel builds and installs into the website environment.
- Website: full Django suite passed 315 tests using isolated in-memory SQLite
  and a test-only fast password hasher; JavaScript suite passed 119 tests.
  Migration drift and Git whitespace checks are clean.
- Plugin: 21 of 22 direct test scripts passed in the QGIS 3.44.14 / Python 3.12
  environment. The unchanged isolated credential-vault test stalled, including
  with the installed launcher; it is not reported as passed. Focused final adapter,
  saved-route/dialog, terrain, and WPML checks pass.
- `tools/check_consumer_parity.py` compares eight semi/full-auto, manual/automatic,
  and cross-hatch combinations. Both adapters produce identical complete results.
  The website accepts plugin provenance. Split-flight KMZ coordinates, camera
  actions, pitch/wait values, and turn flags match in both `template.kml` and
  `waylines.wpml` (coordinate serialization tolerance: 1e-8 degrees).
- The built plugin ZIP was extracted into an isolated directory and used offline
  under QGIS 3.44.14 to plan/export fixture 126: three flights, 177 photo actions,
  with matching counts in both KMZ members. This is an extracted-package smoke
  test, not a QGIS plugin-manager installation or aircraft execution test.

## Remaining work and explicit limits

1. Publish the reviewed engine tag before installing the website requirements
   from the remote repository. Both development integrations already use the
   matching local candidate.
2. Apply the website's additive planning-provenance migration during deployment.
   It was tested on an isolated database; existing application data was untouched.
3. Remove the 15 inert DroneConfig columns after approval to discard their stored
   values. Automatic approval review rejected adding that destructive migration;
   the non-destructive engine-profile adapter is implemented instead. The removed
   columns are `drone_enum`, `sensor_width_mm`, `sensor_height_mm`,
   `focal_length_mm`, `image_width_px`, `image_height_px`, `min_speed_ms`,
   `max_speed_ms`, `default_speed_ms`, `photo_interval_seconds`,
   `battery_safe_minutes`, `continuous_trigger`, `payload_enum`,
   `payload_sub_enum`, and `payload_position_index`. Mission records and saved
   routes are not intended to be deleted.
4. Resolve the credential-vault test environment, test plugin-manager installation,
   and validate exported missions on supported aircraft/controllers. Other QGIS,
   Python, and operating-system combinations are not newly verified here.
   Candidate plugin metadata is restricted to QGIS 3.44.14–3.44.x; the previous
   broad QGIS 3.16/4.x and Python 3.9 claims have been removed.
5. Enterprise native mapping export cannot yet serialize this complete consumer
   action contract; shared-result enterprise export is blocked explicitly.
6. Plugin-to-website sync rejects holes/multipart polygons because the website's
   current polygon storage cannot preserve them losslessly.
7. Launch/home coordinates are supported in the contract and retained in saved
   provenance; location-picker UI remains deferred. Terrain/corridor engine
   migration and controller delivery remain later phases.
