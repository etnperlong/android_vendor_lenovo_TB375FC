# TB375FC vendor blobs - LineageOS 24.0

Proprietary vendor files for the Lenovo Xiaoxin Pad Pro 12.7 (2025), `TB375FC`
(MediaTek Dimensity 8300 / MT6897), for LineageOS 24.0 (Android 17).

Vendor updates are sourced from PRC ZUX `1.5.10.103` / P16.
Existing Lineage adaptations and retained components are preserved.

## DRM

The device tree builds AOSP Android 17 ClearKey as the official service APEX.
The stock ClearKey DRM service and its MediaDrm plugins are no longer shipped;
the extraction fixup also removes the old META init import.

The `.103` Widevine APEX, OEMCrypto, TEE libraries and trusted-app files remain
unchanged. Their extraction integrity has been checked against the local stock
dump; Android 17 linkage, provisioning, L1 and playback still require a build
and device validation.

The stock ClearKey CAS plugin and the `libprotobuf-cpp-{full,lite}-21.7` copies
it needed are no longer shipped. Android 17 does not build that protobuf
version; `base_vendor.mk` installs the AOSP CAS plugin instead.

Regenerate active makefiles from `device/lenovo/TB375FC/extract-files.py -m`.

## Git LFS

One blob exceeds GitHub's 100 MB per-file limit and is stored with Git LFS:
`proprietary/vendor/lib64/mt6897/libaibc_tuning.so`. Install Git LFS before
cloning or syncing so it materialises as the real file instead of a pointer:

```
git lfs install
```

`repo sync` then fetches the LFS object automatically.
