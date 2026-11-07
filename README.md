<div align="center">

<img src="https://raw.githubusercontent.com/mazillka/deckyhub-plugin/main/docs/favicon.svg" width="80" height="80" alt="">

# DeckyHub

[![Support Development & Maintenance](https://img.shields.io/badge/Support_Development_%26_Maintenance-FF5E5B?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/mazillka)

**Your Steam Deck's plugin hub, inside Gaming Mode. Every DeckyHub repository in one place.**

[![Release](https://img.shields.io/github/v/release/mazillka/deckyhub-plugin?color=1d75cc)](https://github.com/mazillka/deckyhub-plugin/releases/latest)
[![Plugin tests](https://img.shields.io/github/actions/workflow/status/mazillka/deckyhub-plugin/ci.yml?branch=main&label=plugin%20tests)](https://github.com/mazillka/deckyhub-plugin/actions/workflows/ci.yml)
[![Registry validate](https://img.shields.io/github/actions/workflow/status/mazillka/deckyhub-registry/ci.yml?branch=main&label=registry%20validate)](https://github.com/mazillka/deckyhub-registry/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/license-GPL--3.0--or--later-blue)](LICENSE.md)

[**Download DeckyHub**](https://github.com/mazillka/deckyhub-plugin/releases/latest) · [Website](https://mazillka.github.io/deckyhub-plugin/) · [Plugin catalog](https://mazillka.github.io/deckyhub-registry/) · [Report an issue](https://github.com/mazillka/deckyhub-plugin/issues)

</div>

> [!CAUTION]
> **DeckyHub only fetches releases from their authors' GitHub repositories. It doesn't make, review or vouch for these plugins and isn't responsible for anything they do to your device or data. Install at your own risk.**

This is the umbrella repository for [DeckyHub](https://github.com/mazillka/deckyhub-plugin), a [Decky Loader](https://decky.xyz/) plugin for finding, installing and updating Steam Deck plugins with your controller. It holds no code of its own: it pins the two DeckyHub repositories as git submodules, so you can clone, browse and work on both together.

**Just want to install DeckyHub?** You don't need this repository. Follow the [install guide](https://github.com/mazillka/deckyhub-plugin#installing-deckyhub) in the plugin's README.

## Repositories

| Path | Repository | What it is |
| --- | --- | --- |
| [`deckyhub-plugin/`](https://github.com/mazillka/deckyhub-plugin) | `mazillka/deckyhub-plugin` | The Decky Loader plugin: a Python backend (`main.py`) and a React/TypeScript frontend (`src/`), its releases, website and tests. |
| [`deckyhub-registry/`](https://github.com/mazillka/deckyhub-registry) | `mazillka/deckyhub-registry` | The plugin list DeckyHub shows on Discover (`registry/apps.json`), the hourly release index and `stats.json`, plugin discovery, and the [catalog website](https://mazillka.github.io/deckyhub-registry/). |

How the two fit together: DeckyHub downloads the registry's `apps.json` at startup and reads every listed plugin's recent releases from the registry's release index, so it spends no GitHub API calls on listed plugins. A registry change shipped to its `main` reaches every installed DeckyHub on its next restart, with no plugin release needed. That's why changes to what the plugin reads from the registry must keep older plugin releases working. The details are in each repository's README.

## Getting the code

Clone with both submodules:

```bash
git clone --recurse-submodules https://github.com/mazillka/deckyhub.git
```

If you already cloned without them:

```bash
git submodule update --init --recursive
```

Each submodule is a full clone of its repository. To work in one, `cd` into it and follow that repository's own `CONTRIBUTING.md`, `AGENTS.md` and branch rules. Both start work from `develop`:

```bash
cd deckyhub-plugin
git fetch origin
git switch -c <topic> origin/develop
```

Changes are committed, pushed and reviewed in the submodule's own repository. This repository only records which commit of each one it points to.

## Updating the pinned commits

Both submodules track `main`, their stable branch (`branch = main` in `.gitmodules`). To move them to the newest `main`:

```bash
git submodule update --remote
git add deckyhub-plugin deckyhub-registry
git commit -m "chore: bump submodules"
```

`git submodule status` shows the commit each one is pinned to.

## Contributing

Issues and pull requests belong in the repository they're about:

- the plugin's behavior, UI, translations or releases → [deckyhub-plugin](https://github.com/mazillka/deckyhub-plugin/issues) ([CONTRIBUTING.md](https://github.com/mazillka/deckyhub-plugin/blob/main/CONTRIBUTING.md));
- adding, fixing or removing a plugin in the list, or the catalog website → [deckyhub-registry](https://github.com/mazillka/deckyhub-registry/issues).

This repository takes only changes to itself: this README, the agent guides, its skills and the submodule pointers.

## License

GPL-3.0-or-later, the same as both submodules. See [LICENSE.md](LICENSE.md).

Not affiliated with Valve or the Decky Loader project.
