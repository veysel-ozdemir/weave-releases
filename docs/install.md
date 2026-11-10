# Installing Weave

Weave runs on Windows, macOS and Linux. Download it from the
[latest release](https://github.com/veysel-ozdemir/weave-releases/releases/latest) and pick the
file for your system:

| System | You need | File |
|---|---|---|
| Windows | Windows 10 or 11, 64-bit (x64) | `Weave-…-windows-x64-setup.exe` |
| macOS | macOS 12 or newer, Apple silicon or Intel | `Weave-…-macos.dmg` |
| Linux | 64-bit (x86_64), glibc 2.35 or newer (Ubuntu 22.04, Debian 12, Fedora 36 or newer), a desktop with GTK 3 and OpenGL ES (Mesa) | `Weave-…-linux-x86_64.AppImage`, or `Weave-…-linux-x86_64.tar.gz` |

Weave works offline. It goes online only for what you do: a Typst package that your document
imports and that isn't on your computer yet, the list of all packages (in the **Packages** tab),
or a Git pull or push. It has no telemetry and doesn't check for updates.

To check that a download is complete and comes from Weave's author, see
[Checking a download](https://github.com/veysel-ozdemir/weave-releases#checking-a-download).

## Windows

1. **Download** the setup file. Your browser may say the file isn't commonly downloaded; choose
   to keep it.
2. **Run it.** The beta isn't signed yet, so Windows shows **Windows protected your PC**. Choose
   **More info**, check that the app is `Weave-…-windows-x64-setup.exe`, and choose **Run
   anyway**.
3. **The setup** installs Weave for you alone, in `%LOCALAPPDATA%\Programs\Weave`, without
   asking for an administrator. It adds Weave to the Start menu, and a desktop icon if you tick
   it. At the end it can start Weave.

### Smart App Control

On Windows 11, **Smart App Control** runs only apps that Microsoft already trusts or that carry a
code signature from a paid certificate. Weave's beta isn't signed, so with Smart App Control on,
Weave doesn't start: Windows shows **Bad Image** (in German **Ungültiges Bild**) naming one of
Weave's files, such as `file_selector_windows_plugin.dll`, with the status `0xc0e90002`. Unlike
the warning in step 2, this one has no way past it.

To use the beta, turn Smart App Control off: **Windows Security** → **App & browser control** →
**Smart App Control settings** → **Off**. Then start Weave again; you don't need to reinstall it.

Smart App Control is a protection of Windows, so decide for yourself. On current versions of
Windows 11 (with the updates from April 2026) you can turn it back on in the same place; on older
versions, turning it back on needs a reset of Windows. Many PCs have it off anyway, for example
most of those upgraded from Windows 10.

**Updating:** run the new version's setup file. It replaces the old version and keeps your
settings. If Weave is open, the setup offers to close it.

**Uninstalling:** Settings → Apps → **Installed apps** (Windows 10: **Apps & features**) →
**Weave** → **Uninstall**. This removes the program; your data stays (see
[Where Weave keeps its data](#where-weave-keeps-its-data)).

## macOS

1. **Open** the `.dmg` file, and drag **Weave** onto **Applications**.
2. **Start Weave** from Applications. As for any app from the internet, macOS asks once: "Weave"
   is an app downloaded from the Internet. Are you sure you want to open it? Choose **Open**.
   Weave is signed and checked by Apple (notarized), so macOS shows no other warning.
3. **Eject** the disk image in the Finder sidebar; you can delete the `.dmg` file.

Weave is sandboxed, like apps from the Mac App Store: it reads and writes the folders you open
or create in it, and remembers them for the recent projects. Two kinds of files outside your
projects need your permission once: fonts you installed yourself (**Fonts** tab → **Use the
fonts in your Library…**) and packages the official Typst program downloaded (**Packages** tab →
**Use the Typst CLI’s packages…**).

**Updating:** drag the new version onto Applications and choose **Replace**.

**Uninstalling:** move Weave from Applications to the Trash. Your data stays (see
[Where Weave keeps its data](#where-weave-keeps-its-data)).

## Linux

### The AppImage

One file, nothing to install:

1. **Make it executable:** in your file manager, the file's **Properties** → **Permissions** →
   **Allow executing file as program**, or in a terminal:
   `chmod +x Weave-…-linux-x86_64.AppImage`.
2. **Start it** with a double-click, or `./Weave-…-linux-x86_64.AppImage`.

If it says **Cannot mount AppImage, please check your FUSE setup**, your system lacks FUSE.
Install your distribution's `fuse3` package (for example `sudo apt install fuse3` or
`sudo dnf install fuse3`), or start Weave without it:
`./Weave-…-linux-x86_64.AppImage --appimage-extract-and-run`. Old versions of FUSE (`libfuse2`)
aren't needed.

The AppImage doesn't add itself to your desktop's menu. Tools such as Gear Lever or
AppImageLauncher do that, or keep the file in a folder of your own, for example
`~/Applications`.

Weave draws its window with **OpenGL ES**, which desktops have through Mesa. Minimal systems and
WSL (Linux on Windows) may lack it: then Weave stops at once with
`Couldn't open libGLESv2.so.2`, and the AppImage says what to install. On Debian, Ubuntu and WSL:
`sudo apt install libgles2 libegl1 libgl1-mesa-dri`; on Fedora: `sudo dnf install libglvnd-gles`.

Started from a terminal, Weave may print messages that need no action:

- `libEGL warning: …` and `MESA: error: ZINK: failed to choose pdev`: the graphics library found
  no GPU driver (usual in WSL and virtual machines) and draws in software instead.
- `Using the Impeller rendering backend`: Flutter, which Weave is built with, names how it draws.
- `Gdk-Message: Unable to load … from the cursor theme`: a mouse pointer shape is missing from the
  system's theme; the normal pointer is used.

### The .tar.gz

Where AppImages don't run: unpack it (`tar xzf Weave-…-linux-x86_64.tar.gz`) and start `weave`
in the folder it creates. It needs GTK 3 and OpenGL ES, which desktops have; on a minimal system,
install your distribution's packages (for example `libgtk-3-0` and `libgles2`).

**Updating:** replace the AppImage (or the folder) with the new one.

**Uninstalling:** delete the AppImage (or the folder). Your data stays (see
[Where Weave keeps its data](#where-weave-keeps-its-data)).

## Where Weave keeps its data

Your projects stay where you put them; Weave adds a `.weave` folder to a project only for its
history and, if you change them, the project's settings. Weave's own data is in these folders:

| | Settings, recent projects, unsaved-changes copies, imported fonts and dictionaries | Downloaded packages and the list of fonts on the computer |
|---|---|---|
| Windows | `%APPDATA%\dev.ozdemirsoftware\Weave` | `%LOCALAPPDATA%\dev.ozdemirsoftware\Weave` |
| macOS | `~/Library/Containers/dev.ozdemirsoftware.weave` (both) | |
| Linux | `~/.local/share/dev.ozdemirsoftware.weave` | `~/.cache/dev.ozdemirsoftware.weave` |

Git sign-ins you asked Weave to remember are in your system's password store, under
`dev.ozdemirsoftware.weave.git`: Keychain Access on macOS, the Credential Manager (Windows
Credentials) on Windows, the Secret Service (such as GNOME Keyring or KWallet) on Linux.

To remove Weave completely, delete these folders and sign-ins after uninstalling it. Settings →
Storage removes downloaded packages without uninstalling.
