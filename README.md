Dogecoin Core release attestations
-----------------------------------

This repository contains gitian signatures for Dogecoin Core. It is updated on
each release with attestations from people that have reproduced the release
binaries using the process described in the Gitian Building
[guide](https://github.com/dogecoin/dogecoin/blob/master/doc/gitian-building.md).
You can use these attestations to verify that the release files you downloaded
are not meddled with and compare them to your own binaries built with gitian.

Note that although attestations are used in the decision making for the Dogecoin
Core release process, these attestations do not all have to match. Most often,
mismatches are caused by software misconfigurations, but it is always possible
that there is a problem with the released software, then these attestations can
help in detecting and documenting that.

### Structure

For each release and each system target, a folder is created. Inside it are
folders for each attester.

```
+ version1-os1
|    attester1
|    attester2
|    attester3
+ version1-os2
|    attester1
|    attester2
|    attester3
+ version2-os1
|    attester3
|    attester4
|    attester5
...
```

Inside each attester's folder, an `.assert` and a `.sig` file exists. `.assert`
files contain a description of the built binaries, their shasums and that of
their outputs. You can for example find the `sha256sum` of released tarballs
here at the top of each file.

The `.sig` file contains a detached PGP signature that authenticates the `.assert`
file with the attester's PGP key.

### Keys

Attesters use a PGP key which most often can be downloaded from pgp keysites, or
be found on [the Dogecoin Core repository](https://github.com/dogecoin/dogecoin/tree/master/contrib/gitian-keys)

### Validating

If you've created your own binaries and attestation with gitian, you'll want to
create an attester directory for yourself (gitian creates the structure for you)
and validate your build outcome with that of others.

Assuming linux, to validate consistency between attestations, the following
script can be used, from the parent directory that contains the `gitian.sigs`
directory:

```bash
git clone https://github.com/dogecoin/dogecoin.git
git clone https://github.com/devrandom/gitian-builder.git
VERSION=<version you built>

pushd dogecoin
git checkout v${VERSION}
popd

for platform in osx linux win; do
  gitian-builder/bin/gverify -r ${VERSION}-${platform} \
    --destination ./gitian.sigs/ \
    ./dogecoin/contrib/gitian-descriptors/gitian-${platform}.yml;
done
```

### Contributing your attestation

The more attestations there are, the better, so please consider providing your
attestations here with a pull request. If your checksums do not match, please
open an issue or draft pull request and describe your observations, so that we
can try and explain any differences and their causes.
