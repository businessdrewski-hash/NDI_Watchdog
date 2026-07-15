# Sync Guardian v0.3.4

This hotfix expands Adaptive Soft Sync for controlled fast catch-up. The maximum correction control now permits up to 5000 ppm (0.5%), while remaining at a conservative 50 ppm by default. Existing corrected error uses a smooth nonlinear catch-up curve, and correction brakes toward neutral at twice the selected slew rate to reduce overshoot.

At 5000 ppm, correction can accumulate at up to 300 ms per minute. This high range may be audible on pitch-sensitive material and should be used as a catch-up ceiling rather than a normal steady-state setting.
