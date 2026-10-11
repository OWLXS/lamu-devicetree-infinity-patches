# lamu-devicetree-infinity-patches

ProjectInfinity-X-specific device tree patches for `motorola/lamu`, kept out
of the base device tree repo so it stays clean enough to build any ROM.

Base repo (clean, structural-only):
- [OWLXS/android_device_motorola_lamu](https://github.com/OWLXS/android_device_motorola_lamu) — apply `lamu/*.patch` on top of commit `241fbb3`

## Apply

```bash
cd device/motorola/lamu && git am /path/to/lamu-devicetree-infinity-patches/lamu/*.patch
```

## Contents

- `lamu/0001-*.patch` — wires `infinity_lamu` + `infinity_lamu_gapps` into `AndroidProducts.mk`
- `lamu/0002-*.patch` — `BoardConfig.mk` ext4 override + reserved space for `infinity_lamu`
  (vanilla flavor needs `/system`, `/product`, `/system_ext` writable post-boot so
  MindTheGapps can be flashed separately later — erofs has no rw path)
- `lamu/0003-*.patch` — `system.prop` import of `infinity_sku_<sku>.prop` (About-phone identity,
  Infinity-X's own namespace only — `ro.product.marketname` lives in the base repo instead,
  it's a standard AOSP prop every ROM's Settings reads, not specific to this one)
- `lamu/0004-*.patch` — `infinity_lamu.mk` (vanilla) + `infinity_lamu_gapps.mk` (GApps baked in)
  product flavor makefiles
- `lamu/0005-*.patch` — `sku/system/infinity_sku_*.prop`: `ro.infinity.soc` + `ro.infinity.camera`
  only (read by Infinity-X's own About-phone UI), per-SKU for all 13 real Motorola SKUs this
  device tree covers (XT2521-\*, XT2523-\*)

## Not included (local-only, never committed anywhere)

`device/motorola/lamu/security/*.pk8` — the actual signing keys referenced by
`PRODUCT_DEFAULT_DEV_CERTIFICATE` (set in the base repo). These must be
provisioned locally on whatever machine builds `infinity_lamu`/`infinity_lamu_gapps`;
they are intentionally never shipped in any repo, base or patch set.
