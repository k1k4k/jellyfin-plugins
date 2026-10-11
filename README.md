# k1k4k Jellyfin plugins

One plugin repository for all k1k4k plugins. Add it once to Jellyfin, and every plugin, with its updates, shows up in the catalog.

*Un seul dépôt de plugins pour tous les plugins k1k4k. Ajoutez-le une fois dans Jellyfin : chaque plugin et ses mises à jour apparaissent dans le catalogue.*

## Repository URL / Adresse du dépôt

**Dashboard → Plugins → Manage Repositories → `+`** / **Tableau de bord → Extensions → Gérer les dépôts → `+`**

| Jellyfin | URL |
|---|---|
| 12.x | `https://k1k4k.github.io/jellyfin-plugins/manifest.json` |
| 10.11.x | `https://k1k4k.github.io/jellyfin-plugins/manifest-10.11.json` |

Then / puis **Plugins → Available / Disponible**.

## Plugins

| | |
|---|---|
| [**Jellyfin Background Play**](https://github.com/k1k4k/jellyfin-background-play) | Leaving the web video player no longer stops the video: it keeps playing with the music mini player bar. / Quitter le lecteur ne coupe plus la vidéo : elle continue en fond avec la mini barre de la musique. |
| [**Jellyfin Player Queue**](https://github.com/k1k4k/jellyfin-player-queue) | The play queue the web player was missing: seasons, reorder, add, remove, without leaving the video. / La file de lecture qui manquait au lecteur web : saisons, réordonner, ajouter, retirer, sans quitter la vidéo. |

Already using the Player Queue repository URL? It keeps working and lists the other plugins too.

*Vous utilisez déjà l'adresse du dépôt de Player Queue ? Elle reste valable et liste aussi les autres plugins.*

<details>
<summary><b>How it works / Fonctionnement</b></summary>

Each plugin publishes its own manifests (`manifest.json`, `manifest-10.11.json`) from its **Release** workflow. The **Merge manifests** workflow here rebuilds the common manifests from the plugins listed in [`sources.json`](sources.json): every hour, by hand (*Run workflow*), or right away when a plugin release sends the `plugin-released` event (plugin repository secret `PLUGINS_REPO_TOKEN`: a token with *Contents: write* on this repository).

Each plugin entry always comes from its own repository (matched by GUID). An unreachable repository doesn't drop its plugin: the previous entry is kept.

Adding a plugin: add `{ "guid": "…", "repo": "k1k4k/…" }` to `sources.json`, and a card to `index.html` (the page served at https://k1k4k.github.io/jellyfin-plugins/).

```bash
python scripts/merge_manifests.py build --sources sources.json
```

The same script, in `sync` mode, lets a plugin repository list the other plugins in its own manifests (used by `jellyfin-player-queue`, whose URL was published first).

</details>

## License

MIT, see [LICENSE](LICENSE).
