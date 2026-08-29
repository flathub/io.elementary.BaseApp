# elementary BaseApp
This base application provides common libraries used by applications made for elementary OS, allowing these apps
published on Flathub easily.

## Supported Branches
Any branch based on a non-EOL base runtime should be supported. Branches are considered end-of-life
when the base runtime is EOL. The current list of supported branches is:

|Baseapp Branch      |Base Runtime           |
|:-------------------|:----------------------|
|`branch/circe-25.08`|org.gnome.Platform//50 |

## Features
This base application provides the following libraries:

- [libgee](https://gitlab.gnome.org/GNOME/libgee)
- [granite](https://github.com/elementary/granite/tree/master)
- [granite-7](https://github.com/elementary/granite/tree/granite-7)
- [stylesheet](https://github.com/elementary/stylesheet)
- [icons](https://github.com/elementary/icons)
- [libportal](https://github.com/flatpak/libportal) with GTK 3 and 4 backends

## Example Manifest
This must be a new file if you would like to publish your app both on AppCenter and Flathub
because AppCenter only accepts apps that uses the elementary runtime
(see [Publishing Requirements](https://docs.elementary.io/develop/appcenter/publishing-requirements#packaging)),
while it's not available on Flathub.

```yaml
id: io.github.yourusername.yourrepositoryname

base: io.elementary.BaseApp
base-version: circe-25.08

runtime: org.gnome.Platform
runtime-version: '50'
sdk: org.gnome.Sdk

command: io.github.yourusername.yourrepositoryname

finish-args:
  - --share=ipc
  - --socket=wayland
  - --socket=fallback-x11
  - --device=dri

cleanup:
  - /include
  - /lib/girepository-1.0
  - /lib/pkgconfig
  - /share/gir-1.0
  - /share/vala

cleanup-commands:
  - /app/cleanup-BaseApp.sh

modules:
  - name: yourrepositoryname
    buildsystem: meson
    sources:
      - type: dir
        path: .
```

## Supported Runtimes
You would typically use either `org.gnome.Platform` or `org.freedesktop.Platform` instead of `io.elementary.Platform`.
Other runtimes that available on Flathub might also work but we don't support them.

`runtime-version` doesn't need to match with the one that the BaseApp is based on.

```yaml
runtime: org.gnome.Platform
runtime-version: '50'
sdk: org.gnome.Sdk
```

Note that you would need to use `org.freedesktop.Sdk.Extension.vala` extension to build Vala source codes
when using `org.freedesktop.Platform`; see [its README](https://github.com/flathub/org.freedesktop.Sdk.Extension.vala)
for details.

```yaml
runtime: org.freedesktop.Platform
runtime-version: '25.08'
sdk: org.freedesktop.Sdk
sdk-extensions:
  - org.freedesktop.Sdk.Extension.vala

build-options:
  prepend-path: /usr/lib/sdk/vala/bin/
  prepend-ld-library-path: /usr/lib/sdk/vala/lib
```

## Cleanup
You can cleanup development-related files from the BaseApp to reduce package size of the app.

```yaml
cleanup-commands:
  - /app/cleanup-BaseApp.sh
```
