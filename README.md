# EasyArabic — ergonomic Arabic XKB layout

**Overview**
- **Purpose:** EasyArabic is an ergonomic Arabic keyboard layout designed to make frequently-used Arabic letters and diacritics more accessible. It repositions several characters that are inconveniently distant on standard layouts (for example: `د` (daal), `ذ` (dhaal), and `ط` (Taa')).
- **Design goals:** Reduce finger travel for common Arabic letters, place related consonants next to each other for faster typing, and give priority to Arabic diacritics (tashkeel) by placing them on the number row.

**Key changes and rationale**
- **Daal / Dhaal / Taa':** These characters are moved to more reachable keys so you no longer need large hand stretches to type them.
- **Haa' / Khaa' / 'Ayn / Jeem grouping:** The layout relocates `ح` (Haa') so `خ` (Khaa') sits closer to `ع` ('Ayn), followed by `ح` (Haa) and `ج` (Jeem). This grouping reflects common phonetic and typing patterns and speeds transitions between related characters.
- **Tashkeel on number row:** The number row contains tashkeel (diacritics) instead of digits by default, because in many Arabic writing contexts diacritics are used more frequently than numerals. Numbers remain available via Shift or with an alternative layer.
- **Extra characters:** The layout includes several additional Arabic characters and marks that are not present on most standard keyboards, useful for linguistic, academic, or regional typing needs.

**Files**
- **Layout file:** The XKB symbol file for this layout is included as [arabic](arabic) in this directory.

**Quick install (temporary)**
- **User-local (temporary, no root):**

	```bash
	setxkbmap -I$HOME/.config/xkb -layout arabic
	```

	This tells X to include your local XKB symbols directory and load the `arabic` layout for the current session.

**Install system-wide (persistent)**
- **System install (requires root):**

	```bash
	sudo cp arabic /usr/share/X11/xkb/symbols/
	sudo dpkg-reconfigure xkb-data   # Debian/Ubuntu; other distros may vary
	```

	After copying, you can select the layout using your desktop environment's keyboard settings or with `setxkbmap -layout arabic`.

**Usage and examples**
- **What changed visually and functionally:**
	- The `د`/`ذ`/`ط` cluster is moved to keys you can reach without large hand movement.
	- `خ` (Khaa') is positioned adjacent to `ع` ('Ayn), then `ح` (Haa'), then `ج` (Jeem), which matches common Arabic finger motions.
	- The number row contains common tashkeel marks (fatha, damma, kasra, sukun, shadda, etc.). Use `Shift` (or your system modifier) to access digits if needed.
	- Several extra glyphs not normally present on the standard layout are available directly on accessible keys.

**Tips**
- **Find the active layout name:** If the layout doesn't activate as shown, the internal layout name may differ. Use `setxkbmap -print -verbose 10` to inspect the current XKB configuration.
- **Reloading XKB rules:** If you install the layout system-wide and don't see it in the GUI, log out and back in or restart Xorg/your desktop session.

**Troubleshooting**
- **Layout not loading:** Verify the file path and syntax. Validate the loaded map with:

	```bash
	setxkbmap -print | xkbcomp -xkb - $DISPLAY
	```

- **Check for syntax errors:** If X refuses the layout, try compiling and checking the file with `xkbcomp` and review `/var/log/Xorg.0.log` for errors.

**Compatibility notes**
- This layout is provided as an XKB symbols file and is intended for X11-based environments. Wayland compositors may use XKB too, but behavior can vary by compositor. Desktop environment keyboard settings can override or affect how the layout is exposed in GUI pickers.

**Attribution & license**
- **Author:** (add your name or handle here)
- **License:** Add a license if you want this layout to be re-used (e.g., MIT, CC0). If no license is included, default copyright applies.

**Contributing**
- If you have suggestions for improved mappings, additional characters, or regional variants, please open an issue or a pull request. Include a short rationale and example use-cases for any proposed change.

**Contact**
- For questions or help, please open an issue in this repository.
