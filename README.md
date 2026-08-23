# StainMC Public Assets

This repo hosts public assets for the StainMC network that need to be downloadable from the public internet (the main `saxkeymc/StainCore` repo is private, so its release assets aren't directly downloadable).

## Downloads

### Latest StainCore jar
- **Direct download (raw.githubusercontent.com):** https://raw.githubusercontent.com/saxkeymc/CobraLobby/main/jars/StainCore-v1.30.0.jar
- **CDN download (jsDelivr, faster global):** https://cdn.jsdelivr.net/gh/saxkeymc/CobraLobby@main/jars/StainCore-v1.30.0.jar.zip
  - Note: this file has a `.zip` extension because jsDelivr blocks `.jar` files for security. After downloading, rename it from `StainCore-v1.30.0.jar.zip` to `StainCore-v1.30.0.jar` (or just drag-drop into your `plugins/` folder — most panels will accept either extension).

### Latest resource pack (tablist logo)
- **CDN download (jsDelivr):** https://cdn.jsdelivr.net/gh/saxkeymc/CobraLobby@main/resource-packs/stain-tablist-pack.zip
- This is the URL the StainCore plugin uses by default (`resource-pack.url` in `config.yml`).

## Update workflow

When releasing a new StainCore version:
1. Build the jar locally with `mvn clean package`
2. Copy the jar to `jars/` in this repo (with `.jar.zip` extension for jsDelivr)
3. Push to `main`
4. Purge jsDelivr cache via `curl -X POST -H 'Content-Type: application/json' -d '{"path":["/gh/saxkeymc/CobraLobby@main/jars/StainCore-vX.Y.Z.jar.zip"]}' https://purge.jsdelivr.net/`
5. Verify the download works with `curl -sL -o /tmp/test.jar "https://cdn.jsdelivr.net/gh/saxkeymc/CobraLobby@main/jars/StainCore-vX.Y.Z.jar.zip" && sha1sum /tmp/test.jar`

When updating the resource pack:
1. Replace `resource-packs/stain-tablist-pack.zip`
2. Compute new SHA1: `sha1sum resource-packs/stain-tablist-pack.zip`
3. Push to `main`
4. Purge jsDelivr cache (same as above, change path)
5. Update the `resource-pack.hash` field in `saxkeymc/StainCore/src/main/resources/config.yml` with the new SHA1
6. Bump `config-version` so the auto-migration rewrites the on-disk config
