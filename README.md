# Ubuntu i915 kernel validation

This private repository builds a disposable Ubuntu Questing kernel candidate for
Launchpad bug 2161098. It does not install a kernel or change any machine.

The workflow pins the Ubuntu source commit, the Launchpad attachment checksum,
and the GitHub Actions used to retain the resulting Debian packages and build
manifest. Installation and reboot are deliberately separate operator actions.

