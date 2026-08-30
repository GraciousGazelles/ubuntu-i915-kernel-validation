# Ubuntu i915 kernel validation

This private repository builds a disposable Ubuntu Questing kernel candidate for
Launchpad bug 2161098. It does not install a kernel or change any machine.

The workflow pins the Ubuntu source commit, the Launchpad attachment checksum,
and the GitHub Actions used to retain the resulting Debian packages and build
manifest. Installation and reboot are deliberately separate operator actions.

## Results

The exact candidate built successfully on a free public GitHub-hosted runner:

- Ubuntu tag object: `f86737eef0da36deab294903e859265785a0feda`
- Peeled source commit: `a399ea490b86e68c36c9c35d26c47c1866611239`
- Launchpad attachment 5993118 SHA-256:
  `b237a20b8787272cf977c45a8f46ad22be3096306ae12a12db8e16bf4876f900`
- Candidate package version: `6.17.0-999.41~lp2161098.1`
- [Kernel build run 33318403922](https://github.com/GraciousGazelles/ubuntu-i915-kernel-validation/actions/runs/33318403922)
- [Artifact validation run 33342966040](https://github.com/GraciousGazelles/ubuntu-i915-kernel-validation/actions/runs/33342966040)

The retained artifact has GitHub archive digest
`sha256:00ea4de808170a9db469d7dfdd2680bded209149bf9690cf9829825c5f68d8ab`.
Every file also passed the artifact's internal `SHA256SUMS` manifest.

Validation confirmed:

- the image, headers, modules, build information, and tools use the distinct
  `6.17.0-999` ABI;
- the packaged i915 module has vermagic for `6.17.0-999-generic`;
- ZFS `spl.ko.zst` and `zfs.ko.zst` are present with the same ABI;
- `CONFIG_KEXEC`, `CONFIG_CRASH_DUMP`, `CONFIG_PROC_VMCORE`, and
  `CONFIG_DEBUG_INFO` remain enabled; and
- Ubuntu's `broadcom-sta-dkms` package builds and installs successfully against
  the candidate headers.

## Scope of the evidence

These runs prove that the supplied patch applies to the exact Ubuntu source,
compiles, packages successfully, and preserves the host's critical module and
crash-capture prerequisites. They do not yet prove runtime behavior on the
affected Intel GPU. Hardware boot and the triggering graphics workload remain
separate tests, with the existing distribution kernels retained for rollback.
