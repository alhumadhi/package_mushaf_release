# Mushaf Release Backup

Public backup/recovery repository for the 604-page Mushaf package used by the app.

- **Primary copy:** embedded in the app and works offline.
- **Backup copy:** this repository, for recovery or future server delivery.
- The app should **not** depend on this repository for normal offline reading.

The package is generated from the main project with:

```bash
bash tools/package_mushaf_release.sh --push
```

The script creates two archive parts (pages 1–302 and 303–604), plus `manifest.json` and `SHA256SUMS.txt`, then publishes them under `releases/v1/`.
