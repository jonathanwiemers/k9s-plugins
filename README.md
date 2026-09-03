# K9s Plugins

Portable K9s plugins for common Flux, Helm, and Kubernetes workflows.

## Included actions

| Scope | Shortcut | Action |
| --- | --- | --- |
| HelmRelease | `Shift-R` | Reconcile |
| Kustomization | `Shift-K` | Reconcile |
| GitRepository | `Shift-G` | Reconcile source |
| HelmRepository | `Shift-H` | Reconcile source |
| OCIRepository | `Ctrl-O` | Reconcile source |
| Bucket | `Shift-B` | Reconcile source |
| HelmRelease | `Ctrl-F` / `Shift-U` | Suspend / resume |
| Kustomization | `Ctrl-P` / `Ctrl-U` | Suspend / resume |
| Kustomization | `Shift-D` | Diff |
| Any resource | `Shift-V` | Related events |
| Ingress | `Shift-I` | Open first configured host |
| HelmRelease | `Shift-Y` | Helm release history |
| Pod | `Shift-C` | Copy container images |

Reconcile, suspend, and resume actions always require confirmation.

## Install

```bash
git clone https://github.com/jonathanwiemers/k9s-plugins.git "$HOME/src/k9s-plugins"
export REPO_DIR="$HOME/src/k9s-plugins"
export PATH="$REPO_DIR/scripts:$PATH"
```

Add the `PATH` export to your shell profile. K9s must inherit it when started.

Run `k9s info` and identify the directory containing `plugins.yaml`. Create a
`plugins` directory alongside it, then link this repository into that directory:

```bash
mkdir -p "<k9s-config-dir>/plugins"
ln -s "$REPO_DIR/plugins" "<k9s-config-dir>/plugins/k9s-plugins"
```

For example, if `k9s info` reports `.../k9s/plugins.yaml`, then
`<k9s-config-dir>` is `.../k9s`.

Existing plugin definitions in the primary `plugins.yaml` must be removed or
moved before linking this repository, otherwise duplicate shortcuts prevent
plugin loading.

Restart K9s after changing `PATH` or the symlink. K9s may reload YAML changes
while running, but restarting is the reliable way to pick up environment changes.

## Update

```bash
git -C "$REPO_DIR" pull --ff-only
```

Restart K9s if scripts changed. Review each update before using write actions.

## Requirements

- `k9s` and `kubectl`
- FluxCD CLI for Flux actions. It must provide `flux reconcile`; the InfluxData
  query-language binary named `flux` is not compatible.
- `helm` for release history
- A browser opener: `open` (macOS) or `xdg-open` (Linux)
- A clipboard tool: `pbcopy` (macOS), `wl-copy`, or `xclip` (Linux)

## Security and behavior

This repository contains no credential, cluster, or provider-login setup. It
uses the active K9s kubeconfig and context through K9s runtime variables.

All commands operate on the active K9s context. Read descriptions and confirm
dialogs carefully before approving a mutating action.
