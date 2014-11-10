obs-git-update
==============
Used to easily build nightly packages on [Open Build Service (OBS)](http://openbuildservice.org/) (and could be adapted for stable releases using tags).

usage
-----
The script depends on the `.spec` file being setup to read the version information from two files: `{PACKAGE}.version` (git describe) and `{PACKAGE}.hash` (git commit hash).

```sh
$ obs-git-update PACKAGE SOURCE_DIR PACKAGING_DIR
# run from openage git clone directory
$ obs-git-update openage . ~/path/to/obs/checkout/openage
```

spec file
---------
The spec file should be setup similarly to the following (example for package `openage`).

```spec
# Big numbers to abvoid other entries, must precede version declaration.
Source17: openage.version
Source18: openage.hash

# Load version information from files. This allows for sharing for deb
# building, cp to openage_version file, and removes needless changes to spec.
Version:     %(tr -d '\n' < %{SOURCE17})
%define hash %(tr -d '\n' < %{SOURCE18})

Source: https://github.com/SFTtech/openage/archive/%{hash}.tar.gz

# rest of .spec
```

obs service file
----------------
Open Build Service (OBS) supports `_service` files for retrieving source code and verifying instead of checking the source directly into the repository. The script makes use of this mechanism for including source tarball.

To trigger manually `osc add {URL}`.
