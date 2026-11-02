# DeckyHub

Umbrella repository for DeckyHub. It pins the two DeckyHub repositories as git submodules:

| Path | Repository |
| --- | --- |
| [`deckyhub-plugin`](deckyhub-plugin) | [mazillka/deckyhub-plugin](https://github.com/mazillka/deckyhub-plugin) – the Decky Loader plugin |
| [`deckyhub-registry`](deckyhub-registry) | [mazillka/deckyhub-registry](https://github.com/mazillka/deckyhub-registry) – the plugin registry |

## Clone

```bash
git clone --recurse-submodules https://github.com/mazillka/deckyhub.git
```

Already cloned without submodules:

```bash
git submodule update --init --recursive
```

## Update submodules to their latest commits

```bash
git submodule update --remote --merge
git commit -am "Bump submodules"
```
