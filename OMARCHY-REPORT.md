# Preserve custom libfprint providers during fingerprint setup (Goodix 27c6:55b4)

Omarchy's fingerprint setup explicitly requires libfprint-git. On a machine
with a working custom libfprint provider, the wizard therefore attempts to
replace that driver. For Goodix USB 27c6:55b4, the replacement loses the
sensor support supplied by a community fork.

## Reproduction

1. Install a custom libfprint provider with Goodix 27c6:55b4 support.
2. Enroll with fprintd-enroll and confirm fprintd-verify returns verify-match.
3. Run Omarchy's fingerprint setup. Its package check requires libfprint-git
   specifically; pacman is invoked with --ask 4 to accept replacement.

## Proposed change

Preserve an installed libfprint provider and install fprintd/usbutils as
needed. Use libfprint-git when no provider is installed. Continue to enroll
and verify before changing PAM. This lets users retain hardware-specific
drivers without making this experimental fork Omarchy's default.

## Working driver reference

Base: https://github.com/jedbillyb/libfprint/tree/goodix-55b4-fixes
Commit: f7b06e4c737ba82d5d002678d017c400b982e4f3
Device: USB 27c6:55b4, firmware family GF3208_RTSEC_APP_.

The accompanying source archive, PKGBUILD and local-fixes.patch capture the
working local build. Changes include OpenCV 5 build support, host-side touch
detection, image-based release polling, and the additional firmware prefix.
The host-side touch detection builds on the community fork's published work.

Recorded verification: successful package build, enrollment, verify-match,
and sudo authentication without password input. Password fallback and the
Omarchy lid gate remain in the sudo configuration. Only sudo was configured;
lock screen and polkit were not verified. The driver reprovisions the sensor
pairing key, so this should remain opt-in experimental hardware support.

Confirmed against Omarchy commit ef930018396bb48bfcc72393628375b067ac3188,
`bin/omarchy-setup-security-fingerprint`, package installation block.

Build review also found an existing fork warning in goodixtls.c: err_from_ssl
allocates strlen(msg) bytes then uses strcpy, which needs an extra byte for
the terminator. This is outside the local patch and needs correction before
considering this fork for distribution as a default driver.
