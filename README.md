# tinted-zen

A [tinty](https://github.com/tinted-theming/tinty) template that maps a Base16 color scheme to [Zen Browser](https://zen-browser.app/)'s UI, replacing the browser chrome colors via `userChrome.css`.


<img width="2414" height="1328" alt="image" src="https://github.com/user-attachments/assets/1af6146d-c202-4ce1-bacf-8acff6c32920" />


## NOTE: Currently only supports base16. Other schemas to come soon.

## How it works

Zen Browser (Firefox-based) exposes a set of `--zen-*` CSS custom properties that control its sidebar, toolbar, panels, and accent system, plus inherits Firefox's standard toolbar variables. This template generates a `userChrome.css` file that overrides those variables with colors drawn from the active Base16 scheme.

## Color mapping

| Zen / Firefox variable | Base16 | Role |
|---|---|---|
| `--zen-primary-color` | `base0D` | Main accent (drives Zen's `color-mix` derivations) |
| `--zen-colors-primary` | `base0D` | Primary accent (explicit override) |
| `--zen-colors-secondary` | `base02` | Secondary UI elements |
| `--zen-colors-tertiary` | `base01` | Subtle background tint |
| `--zen-colors-hover-bg` | `base02` | Hover state background |
| `--zen-colors-primary-foreground` | `base05` | Text on primary surfaces |
| `--zen-colors-border` | `base02` | Subtle border |
| `--zen-colors-border-contrast` | `base04` | High-contrast border |
| `--zen-colors-input-bg` | `base01` | Input field background |
| `--zen-main-browser-background` | `base00` | Main window background |
| `--zen-dialog-background` | `base01` | Dialog / panel background |
| `--zen-toolbar-element-bg` | `base01` | Toolbar button background |
| `--zen-toolbar-element-bg-hover` | `base02` | Toolbar button hover |
| `--zen-urlbar-background` | `base01` | Address bar |
| `--zen-branding-dark` | `base00` | Dark brand baseline |
| `--zen-branding-paper` | `base07` | Light brand baseline |
| `--zen-sidebar-notification-bg` | `base0D` | Sidebar notification accent |
| `--zen-sidebar-themed-icon-fill` | `base05` | Sidebar icon color |
| `--toolbar-bgcolor` | `base01` | Toolbar background (Firefox) |
| `--toolbar-color` | `base05` | Toolbar text (Firefox) |
| `--tab-selected-bgcolor` | `base02` | Active tab background |
| `--tab-selected-color` | `base05` | Active tab text |
| `--arrowpanel-background` | `base01` | Popup / dropdown background |

## Setup

### 1. Enable userChrome.css in Zen Browser

In the address bar, navigate to `about:config`. Search for and set:

```
toolkit.legacyUserProfileCustomizations.stylesheets = true
```

### 2. Create the chrome directory

Find your profile folder via `about:profiles` → **Open Folder** for your active profile, then:

```sh
# Standard install
mkdir -p ~/.zen/<your-profile>/chrome

# Flatpak install
mkdir -p ~/.var/app/app.zen_browser.zen/.zen/<your-profile>/chrome
```

### 3. Add to your tinty `config.toml`

**Standard install:**

```toml
[[items]]
path = "https://github.com/GooseRooster/base16-tinted-zen"
name = "base16-tinted-zen"
themes-dir = "output"
hook = "cp \"$TINTY_THEME_FILE_PATH\" \"$HOME/.zen/$(ls ~/.zen | grep -i 'Default' | head -1)/chrome/userChrome.css\""
supported-systems = ["base16"]
```

**Flatpak install:**

```toml
[[items]]
path = "https://github.com/GooseRooster/base16-tinted-zen"
name = "base16-tinted-zen"
themes-dir = "output"
hook = "cp \"$TINTY_THEME_FILE_PATH\" \"$HOME/.var/app/app.zen_browser.zen/.zen/$(ls ~/.var/app/app.zen_browser.zen/.zen | grep -i 'Default' | head -1)/chrome/userChrome.css\""
supported-systems = ["base16"]
```

> **Multiple profiles:** The glob above picks the first match alphabetically. If you have more than one profile (e.g. `Default (release)` and `Default Profile`), hard-code the directory name instead — check `about:profiles` to confirm which one is active.

### 4. Apply a scheme

```sh
tinty apply base16-dracula
```

Restart Zen Browser (or reload the userChrome by navigating to `about:profiles` → Restart) to see the new colors.

## Notes

- Dark Base16 schemes are the primary use case and are well-tested. Light schemes will apply correctly (base00 being a light background is handled naturally) but are less thoroughly validated.
- Zen's accent-derived variables (`--zen-colors-primary`, `--zen-colors-secondary`, etc.) are explicitly overridden here so that the full Base16 palette drives the UI rather than Zen's `color-mix()` derivations from a single accent color.
- Profile directory names use a capital `D` in `Default` on Linux, which matters because the filesystem is case-sensitive — the hooks above use `grep -i` to match either casing.

## Team

This theme is maintained by the following person(s) and a bunch of [awesome contributors](https://github.com/GooseRooster/base16-tinted-zen/graphs/contributors).

| [![GooseRooster](https://github.com/GooseRooster.png?size=100)](https://github.com/GooseRooster) |
| --- |
| [GooseRooster](https://github.com/GooseRooster) |

## Community

- [Tinted Theming Home](https://github.com/tinted-theming/home)
- [#tinted-theming:matrix.org](https://matrix.to/#/#tinted-theming:matrix.org) on [Matrix](https://matrix.org/)

## License

[MIT License](./LICENSE)
