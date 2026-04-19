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

# Base on the elementary BaseApp to use libraries like granite.
# You can find all available base-versions at:
# https://github.com/flathub/io.elementary.BaseApp/branches/all?query=branch%2F
base: io.elementary.BaseApp
base-version: circe-25.08

# You would typically use either GNOME or freedesktop platform instead of the elementary one.
# runtime-version doesn't need to match with the runtime-version that the BaseApp is based on.
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

# Cleanup development-related files inherited from the BaseApp to reduce package size of the app.
cleanup-commands:
  - /app/cleanup-BaseApp.sh

modules:
  - name: yourrepositoryname
    buildsystem: meson
    sources:
      - type: dir
        path: .
```
