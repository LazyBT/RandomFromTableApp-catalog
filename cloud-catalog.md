# Cloud catalog

The app can sync its internal Room catalog from a public JSON file hosted on GitHub.
The URL is compiled into `BuildConfig.CLOUD_CATALOG_URL` in `app/build.gradle.kts`.

Recommended layout:

```text
RandomFromTableApp-catalog/
  catalog.json
```

Public raw URL format:

```text
https://raw.githubusercontent.com/<user-or-org>/<repo>/main/catalog.json
```

JSON format:

```json
{
  "version": 1,
  "sources": {
    "waves": [
      {
        "name": "Pop",
        "deepLink": "yandexmusic://play-vibe/?stationId=genre:pop",
        "tags": ["Pop", "Hits"],
        "description": "Short optional description"
      }
    ],
    "mixes": [],
    "at_vibe": []
  }
}
```

Update rules:

- Increase `version` every time `catalog.json` changes.
- Keep `deepLink` stable. The app uses it to recognize renamed waves and repair favorites.
- Remove an item from JSON to remove it from the internal database on the next sync.
- Add a new object to add a new wave.
- Use UTF-8 without BOM.
- Commit directly to the `main` branch if you want updates to become available immediately.

The app checks the file at startup, but not more than once every six hours. If GitHub or the network is unavailable, the app keeps using the last local database.
