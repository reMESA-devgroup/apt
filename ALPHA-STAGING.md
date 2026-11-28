# reMESA Alpha 5.10/6.1 — BootFix 1 staging

DKMS revision 7.6.9. This branch is staging only; no APT alpha suite is published yet. The existing preview suite and signing keyring remain unchanged.

## Qualification

The first kernel Oops in 7.6.8 was not fully captured. BootFix 1 is a candidate, not proof of a physical fix. GNU89 header test and modeled lazy-stage tests passed; five changed objects compiled against Linux 6.12 amd64. No physical 5.10/6.1 qualification or i386 module link is established by those tests.

Keep MSVDX disabled for the first dedicated boot; video testing requires explicit opt-in after graphics qualification. Keep stock boot and local recovery available. No automatic retries or module unloading after faults. Do not distribute as a qualified beta.

## Assembly and publication (maintainer)

Use the verified remesa-alpha61-bootfix1-apt-kit.tar.gz and the actual six-package assembly from the existing i386 build, not synthetic test binaries.

On the build host:
```sh
cd ~/remesa-bootfix1
sha256sum -c SHA256SUMS --quiet
bash assemble.sh ~/remesa-alpha61-debs-v2 ~/remesa-alpha61-bootfix1-debs
git clone --branch alpha61-bootfix1-staging https://github.com/reMESA-devgroup/apt.git ~/remesa-apt-staging
bash publish-apt.sh ~/remesa-alpha61-bootfix1-debs ~/remesa-apt-staging
```

The kit's publisher requires the existing official archive signing key locally and verifies signatures using the current public keyring. Never share the secret key. It stages pool/alpha and dists/alpha without touching preview. Review physical results, package dependencies and git diff before committing/pushing or merging. The alpha suite would be publicly downloadable if published from this public repository.

Expected packages: remesa, remesa-desktop, remesa-dkms-integrated, remesa-msvdx-firmware (all), remesa-hre-cross and remesa-vaapi (i386).
