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

## Wi-Fi

The stock `wpa_supplicant` and `hostapd` binaries ship unchanged. The stock
supplicant init script is not shipped: it starts the daemon as root to `setuid`
to `wifi`, which Android 17 SELinux denies. The device tree provides an rc that
starts it as `wifi`.

## MiraVision

MediaTek's MiraVision app, `mediatek-framework.jar` and the system_ext picture
quality JNI and client libraries are not shipped. Stock does not expose
MiraVision, its JNI libraries cannot load on Android 17, and nothing else uses
them. The vendor picture quality HAL is unchanged.

## Storage proxy

MediaTek's `mtk_storageproxyd`, its init script and `libisetrusty.so` are not
shipped. Neither of its RPMB peers exists on this device (no
`/dev/trusty-ipc-dev1`; `rpmb-mtk` registers no Trustonic netlink family
without `trustonic,real-drv` in the DTB), so it only retried forever. The
Microtrust TEE handles RPMB itself.

## Unused stock overlays

MediaTek's `FrameworkResOverlay`, `FrameworkResOverlayExt`,
`MtkSettingsResOverlay` and `SettingsProviderResOverlay` are not shipped. The
two Settings overlays carry no resources, and the framework overlays never got
an idmap on this build. The device tree's `FrameworkResDeviceOverlay` carries
their values that still apply on Android 17.

The overlays for Google's module packages (`NetworkStackGoogleGoResOverlay`,
`NetworkStackGoogleResOverlay`, `TetheringGoogleResOverlay`,
`WifiResGoogleOverlay`, `WifiResMainlineOverlay`,
`WifiSystemResMainlineOverlay`) are not shipped either. Their targets
(`com.google.android.wifi.resources`, `com.google.android.networkstack` and
friends) do not exist on LineageOS, with or without gapps.

## Git LFS

One blob exceeds GitHub's 100 MB per-file limit and is stored with Git LFS:
`proprietary/vendor/lib64/mt6897/libaibc_tuning.so`. Install Git LFS before
cloning or syncing so it materialises as the real file instead of a pointer:

```
git lfs install
```

`repo sync` then fetches the LFS object automatically.
