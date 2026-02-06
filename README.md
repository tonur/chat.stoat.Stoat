# Stoat

Stoat is a free all-in-one messaging, voice and video client that's available on your computer and phone.

This repo hosts the flatpak wrapper for [Stoat](https://stoat.chat/), available at [Flathub](https://flathub.org/apps/details/chat.stoat.Stoat).

```sh
flatpak install flathub chat.stoat.Stoat
flatpak run chat.stoat.Stoat
```

## Reporting bugs

If you're experiencing a problem exclusive to the Flatpak version of Stoat, i.e. it doesn't happen when running the official deb or tar.gz packages unsandboxed, feel free to open an issue about it here.

Otherwise, please use the [official bug report form](https://support.stoat.chat/hc/en-us/articles/1500006052822-How-to-Report-a-Bug) instead, as it increases the chances of a bug getting fixed.

## Differences in flatpak version

The flatpak version runs in a sandbox to provide better safety and privacy for users.

However, this sandboxing prevents the following features from working:

- **Game Activity**: This flatpak version of Stoat cannot scan running processes to detect running games.  
  There is currently no workaround or solution for this limitation.
- **Unrestricted File Access**: Default sandbox permissions for this package limit Stoat to only certain directories, so you can't access your entire Home directory. Currently, this limits which file directories you can attach files from and impacts drag and drop functionality.  
  This limitation will likely be overcomed eventually, when Electron give us a file picker portal which will allow full access to the filesystem while still restricting unauthorized access.  
  To work around this now, you can change sandbox permissions of installed flatpak applications (for example, with [Flatseal](https://flathub.org/apps/details/com.github.tchx84.Flatseal) or with `flatpak override --user --filesystem=home chat.stoat.Stoat`) to give Stoat broader file system access, allowing file attachments from more locations.
- **Rich Presence**: See [this page](https://github.com/flathub/chat.stoat.Stoat/wiki/Rich-Precense-(stoat-rpc)) if you want to expose Stoat's rich presence interface for other applications.


### Wayland

Wayland support is enabled by default since Stoat 1.1.12 (released in 2025-12-29).

Please note that native window decorations are not enabled by default. To do so, add the following lines to your [persistent launch options](#persistent-launch-options):

```
--enable-features=WaylandWindowDecorations
--ozone-platform-hint=auto
```

To disable Wayland support permanently, run:

```
flatpak override --user --nosocket=wayland chat.stoat.Stoat
```

### Persistent launch options

To make Stoat's launch options persistent, add them to `~/.var/app/chat.stoat.Stoat/config/stoat-flags.conf` (one option per line):

```conf
# This line will be ignored
--enable-features=WaylandWindowDecorations

# https://chromium.googlesource.com/chromium/src/+/master/docs/linux/password_storage.md
--password-store=basic
```

### Opening Stoat automatically at system startup

The Stoat app has a built-in option for that, but it doesn't work on Flatpak mostly because of sandbox restrictions.

But we can still do that manually, with these two commands:

```sh
mkdir -p ~/.config/autostart
ln -s "$(flatpak info -l chat.stoat.Stoat//stable | sed 's#/app/.*#/exports/share/applications/chat.stoat.Stoat.desktop#')" ~/.config/autostart
```

To undo that, run:

```sh
rm ~/.config/autostart/chat.stoat.Stoat.desktop
```

## Legal

The Stoat app itself is **proprietary** (closed source).
