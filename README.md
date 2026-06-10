# klog

Quickly pick a Kubernetes service and follow its logs, using `kubectl` under the hood.

![klog demo](assets/demo.gif)

It lists the services in a namespace, ordered **most-recently-used first** (so the
last service you viewed is the default selection). Choose one with the **↑/↓ arrow
keys** and press Enter — `klog` then runs:

```sh
kubectl logs --all-pods -n <namespace> services/<service> -f
```

The **namespace is remembered** between runs: pass `-n` once and later runs default
to it (until you pass a different one). Falls back to `default` if none was ever set.

## Requirements

- `kubectl` (configured with a reachable cluster)
- `fzf` (provides the arrow-key picker) — `brew install fzf`

## Installation

Clone the repo and symlink the `klog` script into a directory on your `PATH`:

```sh
git clone https://github.com/seesharper/klog.git
cd klog
ln -s "$PWD/klog" /usr/local/bin/klog   # or /opt/homebrew/bin on Apple Silicon
```

Using a symlink means a `git pull` updates the installed command automatically.
Alternatively, just copy the single script anywhere on your `PATH`:

```sh
cp klog /usr/local/bin/klog && chmod +x /usr/local/bin/klog
```

Verify it's installed:

```sh
klog --help
```

## Usage

```sh
klog                 # use the remembered namespace (or "default")
klog -n my-namespace # use, and remember, this namespace
klog my-namespace    # same, positional form
klog --help
```

The most-recently-used service is highlighted at the top, so pressing **Enter**
immediately re-opens your last service's logs.

## Notes

- The last-used namespace is stored in `~/.klog_namespace` (override with
  `KLOG_NAMESPACE_FILE`).
- Service usage history is stored per-namespace in `~/.klog_history` (override with
  `KLOG_HISTORY_FILE`), kept de-duplicated, most-recent-first, and capped at 200 entries.
- Written for the stock macOS bash 3.2 (no `mapfile`/associative arrays needed).

## License

[MIT](LICENSE)
