# EasyArabic — Ergonomic Arabic XKB layout

**Overview**
- **Purpose:** EasyArabic is an ergonomic Arabic keyboard layout designed to make frequently-used Arabic letters and diacritics more accessible. It repositions several characters that are inconveniently distant on standard layouts (for example: `د` (daal), `ذ` (dhaal), and `ط` (Taa')).
- **Design goals:** Reduce finger travel for common Arabic letters, place related consonants next to each other for faster typing, and give priority to Arabic diacritics (tashkeel) by placing them on the number row.

**Key changes and rationale**
- **Daal / Dhaal / Taa':** These characters are moved to more reachable keys so you no longer need large hand stretches to type them.
- **Haa' / Khaa' / 'Ayn / Jeem grouping:** The layout relocates `ه` (Haa) so `خ` (Khaa') sits closer to `ع` ('Ayn), followed by `ح` (Haa') and `ج` (Jeem). This grouping reflects common phonetic and typing patterns and speeds transitions between related characters.
- **Tashkeel on number row:** The number row contains tashkeel (diacritics) instead of digits by default, because in many Arabic writing contexts diacritics are used more frequently than numerals. Numbers remain available via Shift or with an alternative layer.
- **Extra characters:** The layout includes several additional Arabic characters and marks that are not present on most standard keyboards, useful for linguistic, academic, or regional typing needs.

**Files in this folder**
- The layout file included here is [arabic](arabic). If you instead use a file named `ara` (for example in `$HOME/.config/xkb/symbols/ara`), substitute that name in the examples below.

**Install into system files (append `ara` and `evdev.xml`)**
- **Back up current system files first:**

```sh
sudo cp /usr/share/X11/xkb/symbols/ara /usr/share/X11/xkb/symbols/ara.bak 2>/dev/null || true
sudo cp /usr/share/X11/xkb/rules/evdev.xml /usr/share/X11/xkb/rules/evdev.xml.bak 2>/dev/null || true
```

- **Append or install symbol file:**
	- Append local `ara` to the system `ara` symbols file:

```sh
# from this directory: append this file's contents to the system symbols
sudo bash -c 'cat ara >> /usr/share/X11/xkb/symbols/ara'
```

	- Or install as a separate file to avoid merging conflicts:

```sh
sudo cp ara /usr/share/X11/xkb/symbols/ara-custom
```

- **Add your layout to `evdev.xml`:** edit `/usr/share/X11/xkb/rules/evdev.xml` (requires sudo) and add a `<layout>` entry inside the existing `<layoutList>` element. Example fragment to add:

```xml
<layout>
	<configItem>
		<name>ara</name>
		<shortDescription>ara</shortDescription>
		<description>Arabic — custom ara</description>
		<languageList>
		<iso639Id>ara</iso639Id>
		</languageList>
	</configItem>
</layout>
```

- **Reload / test:** log out and log back in, or test immediately (X):

```sh
setxkbmap -layout ara
# if you used a custom file name: setxkbmap -I/usr/share/X11/xkb -layout ara
```

**Immutable distros (Silverblue / OSTree-style etc.)**
- If `/usr/share` is read-only, use one of these approaches:

- **Per-user XKB (no root needed, works on Xorg):**

```sh
mkdir -p $HOME/.local/share/xkb/symbols
cp ara $HOME/.local/share/xkb/symbols/ara
	# Load for the current X session
setxkbmap -I $HOME/.local/share/xkb -layout ara
```

- **/etc overrides (OSTree systems often allow /etc to be modified):** place the files under `/etc/X11/xkb/`:

```sh
sudo mkdir -p /etc/X11/xkb/symbols
sudo cp ara /etc/X11/xkb/symbols/ara
sudo mkdir -p /etc/X11/xkb/rules
sudo cp evdev.xml /etc/X11/xkb/rules/evdev.xml
```

	Note: some desktop components may still read only `/usr/share`; test GUI pickers and use the per-user method if needed.

- **Desktop GUI fallback:** if you cannot edit `evdev.xml` and need the layout visible in GNOME/KDE, add the layout via the DE input-sources UI or via `gsettings`:

```sh
# Example — careful: this replaces the list
gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'us'), ('xkb', 'ara')]"
```

**Fcitx5 configuration (bonus)**
- **User config (recommended):** put per-user fcitx5 config files in `$HOME/.local/share/fcitx5/inputmethod`. The main user config file is `$HOME/.local/share/fcitx5/inputmethod` 

```sh
mkdir -p $HOME/.local/share/fcitx5/inputmethod
cp keyboard-easy-ar.conf $HOME/.local/share/fcitx5/inputmethod
	# Restart fcitx5 to apply changes
fcitx5 -r || fcitx5 &
```

- **System-wide defaults:** place defaults under `/etc/xdg/fcitx5/` (for distribution-wide defaults):

```sh
sudo mkdir -p /etc/xdg/fcitx5
sudo cp my-fcitx5.conf /etc/xdg/fcitx5/config
```

- **Important:** Fcitx5 can override XKB settings by default. To stop it overriding your XKB layout, open `fcitx5-configtool` → Addons → XCB and uncheck `Allow Overriding System XKB Settings`.

**Notes & troubleshooting**
- If the layout does not appear in a GUI picker after editing `evdev.xml`, log out / reboot, and check compositor/DE caches. Use `setxkbmap -print -verbose 10` and `xkbcomp` to debug.
- For Wayland compositors, behavior varies — the per-user `setxkbmap` approach is X-specific; many compositors rely on the same XKB database but may read only `/usr/share`.

**Want automation?**
- I can add an `install.sh` script that performs safe backups and installs both the symbol file and the `evdev.xml` fragment, or add a small `evdev-fragment.xml` file to this repo. Tell me which you prefer.

---

**Author & contact**
- Author: 3ubaidUrRe7man — open an issue in this repo for questions or suggestions.

