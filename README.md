# DozenOS live-boot

This package is required (even in 1.4) to make the entire system boot.

This is a clone of the Debian package extending it with features required to make
our system boot. It mount's the DozenOS config directories during system startup.
See https://github.com/dozenos/live-boot/blob/current/components/9990-dozenos.sh

## Ideas

The main idea is to drop this fork and consume Debian upstream live-build package
and add our required files to dozenos-build chroot directory
