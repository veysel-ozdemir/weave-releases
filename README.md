# Weave

Weave is an editor for [Typst](https://typst.app) documents with a live preview, for Windows,
macOS and Linux. It works offline: your projects stay on your computer, and Weave connects to
the internet only for what you ask, such as downloading a Typst package or pushing to a Git
server.

This repository holds Weave's downloads. **Get the latest version under
[Releases](https://github.com/veysel-ozdemir/weave-releases/releases/latest).**

## Which file?

| System | File | First start |
|---|---|---|
| Windows 10 and 11 (x64) | `Weave-…-windows-x64-setup.exe` | Not signed: Windows shows "Windows protected your PC"; choose **More info → Run anyway**. It installs for you alone, without admin rights. With **Smart App Control** on (Windows 11), Weave doesn't start: turn it off first, as the [install guide](docs/install.md#smart-app-control) explains. |
| macOS 12 or newer (Apple silicon and Intel) | `Weave-…-macos.dmg` | Open it and drag Weave to Applications. |
| Linux (x86_64, glibc 2.35 or newer, e.g. Ubuntu 22.04) | `Weave-…-linux-x86_64.AppImage` | Make it executable (`chmod +x`), then run it. Where AppImages don't run, use the `.tar.gz` and start `./weave` in its folder. It needs OpenGL ES (Mesa), which desktops have; on WSL: `sudo apt install libgles2`. |

## Guides

- [Installing Weave](docs/install.md): each system step by step, what Windows and macOS show on
  the first start, updating, uninstalling, and where Weave keeps its data.
- [Getting started](docs/getting-started.md): a first project, the window, writing, saving,
  packages, fonts and exporting a PDF.
- [Bibliographies and Zotero](docs/bibliography.md): citations, styles, and a bibliography that
  keeps itself up to date.
- [History and Git](docs/history-and-git.md): snapshots to go back to, and Git to share a project.

## Checking a download

Each release has `SHA256SUMS`, the SHA-256 of every file in it, and `SHA256SUMS.asc`, its
signature, made with the GPG key of Veysel Ozdemir that GitHub shows at
https://github.com/veysel-ozdemir.gpg:

```sh
curl -sSL https://github.com/veysel-ozdemir.gpg | gpg --import
gpg --verify SHA256SUMS.asc SHA256SUMS
sha256sum --check --ignore-missing SHA256SUMS   # macOS: shasum -a 256 --check --ignore-missing
```

GPG then says "Good signature from Veysel Ozdemir", and also warns that the key "is not certified
with a trusted signature". The warning is normal for a key you have just imported, and the good
signature is what counts.

## Problems and ideas

Open an [issue](https://github.com/veysel-ozdemir/weave-releases/issues): what you did, what you
expected, what happened, and your system and Weave's version (Settings → About).

## Notices

Weave is © 2026 Veysel Ozdemir; all rights reserved. The third-party software in it is listed in
the app, under Settings → About → Licenses. Each Linux release also carries the source of the
AppImage's runtime (`Weave-…-linux-appimage-runtime-sources.tar`).
