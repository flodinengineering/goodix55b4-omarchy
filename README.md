# Goodix 27c6:55b4 contribution for Omarchy

Portable source snapshot and patch, based on jedbillyb/libfprint commit
f7b06e4c737ba82d5d002678d017c400b982e4f3 (LGPL-2.1-or-later; license included
in the source archive). The original community driver and host-side touch
detection are credited to that fork and its device documentation:
https://github.com/jedbillyb/linux-fingerprint-drivers/tree/main/devices/27c6:55b4

The patch accepts GF3208_RTSEC_APP_ firmware, detects touches against a
calibration frame, polls images for finger release, and builds with OpenCV 5.
This is experimental, sensor-specific support, not a replacement driver for
all Omarchy hardware. Pairing reprovisions the sensor key; replacing the
package does not restore that key.

Build from this directory on Arch with base-devel installed:

    makepkg -s

Review the package before installing. Installation replaces libfprint or
libfprint-git. Do not run the stock Omarchy fingerprint wizard afterward:
it explicitly installs libfprint-git, replacing the custom driver.

Recorded local validation: package build, fingerprint enrollment,
fprintd verification (verify-match), and sudo authentication without password
input. Only sudo PAM was configured. Lock screen, polkit, suspend/resume,
cancellation during polling, and other sensor revisions were not tested in
this session. The existing fork emits compiler warnings.

The original working tree and installed system were left unchanged.
See OMARCHY-REPORT.md for the proposed upstream report.
