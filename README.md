# xBeauGaming Website

Static GitHub Pages site files for `https://github.com/Safken/xbeaugaming`.

## Files

- `index.html` - xBeauGaming home page
- `dungeon-hauler-support.html` - Dungeon Hauler support page
- `dungeon-hauler-version.json` - Dungeon Hauler Android update check metadata
- `privacy-policy.html` - Dungeon Hauler privacy policy
- `style.css` - shared styling
- `app-ads.txt` - prepared AdMob publisher verification entry (see the domain-root requirement below)

## AdMob verification

AdMob's September 18, 2026 console setup supplied the publisher entry in `app-ads.txt`. It must be served at `https://safken.github.io/app-ads.txt`, the root of the developer website's domain. Publishing it only inside this project's `/xbeaugaming/` path does not satisfy that requirement.

With explicit owner authorization, the root-site repository `https://github.com/Safken/Safken.github.io` was created and published from `main` (commit `d8a9f12`). Both raw GitHub and the live domain-root file were verified against the local entry. Play's published Website link uses this same domain. AdMob's immediate September 18 recheck failed. On September 21, the owner supplied Google's app-review approval email confirming that review-related ad-serving restrictions were lifted. The separate app-ads.txt crawler status was not rechecked in the production build session. Maintain the domain-root file in that root-site repository when changing publishers.

## Upload To GitHub

1. Open `https://github.com/Safken/xbeaugaming`.
2. Upload these files to the repository root.
3. Commit directly to `main`.
4. Go to `Settings > Pages`.
5. Set:
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/root`
6. Save.

GitHub Pages should publish at:

```text
https://safken.github.io/xbeaugaming/
https://safken.github.io/xbeaugaming/dungeon-hauler-support.html
https://safken.github.io/xbeaugaming/privacy-policy.html
```

Use the support and privacy URLs in Google Play Console until a custom domain is configured.

## Version metadata safety contract

The version file is fail-open. Invalid, stale, oversized, non-HTTPS, wrong-package, or incomplete metadata must never block a compatible player from their save. The app accepts only schema version 1, package `com.xbeau.dungeonhauler`, the canonical Google Play listing URL, and the canonical support URL.

To retire an old build safely:

1. Publish the new build and verify its Play listing from an installed build.
2. Set `release_verified_available` to `true`, record the verified version/time, set `pending_minimum_supported_android_version_code`, and set `minimum_enforcement_unix` at least 259200 seconds (72 hours) after verification. Keep `minimum_supported_android_version_code` unchanged during this grace period so older clients also fail open.
3. Refresh `metadata_published_unix` and `metadata_valid_until_unix`, publish, and verify both the raw GitHub file and GitHub Pages with a cache-busting query.
4. Only after the full 72 hours, copy the pending minimum into `minimum_supported_android_version_code` and republish.

Emergency rollback: immediately lower `minimum_supported_android_version_code` to the last safe value, set the pending minimum to that value, set `release_verified_available` to `false`, clear the three verification/enforcement timestamps and verified version, refresh the metadata publication/expiry window, publish, then verify raw GitHub and Pages. Record the rollback evidence in the release checklist.
