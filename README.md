# Smart Shortcuts for Revit

Smart Shortcuts is a Revit add-in that learns how you work — the commands you use most, the sequences you repeat, and how far you usually extend, offset, move or copy — and turns that into one-click Quick Bar buttons, saved macros and keyboard shortcut suggestions.

**Downloads:** see [Releases](https://github.com/omarsaifuddin/RevitSmartShortcuts/releases) for the latest installer.

This repository hosts released installers only. The add-in's source code is not public.

## Features

- **Quick Bar** — six ribbon buttons that fill themselves with the commands and macros you use most. Pin the ones you want to keep.
- **Suggested macros** — repeated command sequences (such as `Duplicate View → Apply Template Properties → Rename`) are detected from live use and existing Revit journals, so suggestions can appear from the first day. Each suggestion shows how often it happens, when it was last seen, and how many command starts it would save.
- **Macros that remember amounts** — a saved step can carry a distance. The add-in learns how far you actually extend, offset, move and copy, then offers those amounts when the macro runs. Amounts follow the project's units.
- **Macro editing** — rename, reorder, add or remove steps, and set each step's amount in the panel.
- **Keyboard shortcut ideas** — commands that are started mostly by mouse and have no shortcut, each with a suggested non-clashing key sequence. Export a `KeyboardShortcuts.xml` for Revit's own import dialog.
- **Team sharing** — pool usage logs from a shared folder, and publish read-only team macros from one file.
- **Settings dialog** — tracking, analysis, amounts, shared folder and telemetry settings in the panel; the XML file remains available for IT.

Supports **Revit 2022–2027** on Windows. The installer registers the add-in for every installed version, for all users of the machine.

## Install

1. Download `SmartShortcuts-0.3.0.msi` from [Releases](https://github.com/omarsaifuddin/RevitSmartShortcuts/releases).
2. Close Revit.
3. Run the MSI and approve the administrator prompt.

Open Revit afterwards. A **Smart Shortcuts** tab appears on the ribbon; the **Shortcuts Panel** button opens the dockable panel.

The MSI is signed with a self-signed certificate, so Windows may show **Unknown Publisher** in the install prompt and Revit may ask whether to load the add-in. Choose **Always Load** in Revit. Verify the download against the SHA-256 published in the release notes.

To uninstall, use **Apps & Features**. Installing a newer MSI replaces the previous version.

### Silent install (RMM / Intune / SCCM)

```bat
msiexec /i SmartShortcuts-0.3.0.msi /qn
```

Useful optional properties:

```bat
rem Pool usage logs and team macros across a team
msiexec /i SmartShortcuts-0.3.0.msi /qn SHAREDFOLDER="\\fileserver\BIM\SmartShortcuts"

rem Report to your own telemetry server instead of the built-in one
msiexec /i SmartShortcuts-0.3.0.msi /qn TELEMETRYURL="https://telemetry.example.com" TELEMETRYKEY="<ingest key>"
```

## Privacy and telemetry

Usage history is kept on the machine by default, under `%APPDATA%\SmartShortcuts\`. Only command ids, timestamps, an anonymous per-person id (a hash of the Windows account) and — for the amount-learning feature — distances and directions are stored. No element ids, model names, element data or file paths are recorded. Settings can opt into logging Windows user names instead of anonymous ids.

**This 0.3.0 build reports usage to `oct.saifuddin.ca` by default.** A report includes each user's Windows user name, domain and machine name, plus saved macros with the amounts their steps run with and their run counts, Quick Bar contents, most used commands, suggestions with when they were first and last seen, the raw usage log and the error log. Reporting runs in the background and never blocks Revit.

The panel's **Settings** dialog and the MSI's `TELEMETRYURL` / `TELEMETRYKEY` properties select the server. Leaving the server empty keeps the built-in server for this build; it does not turn reporting off. If you need an installer that cannot report, contact the author.

## Settings and local files

The panel's **Settings** button covers tracking, analysis, amounts, team folder and telemetry. The file is at `%APPDATA%\SmartShortcuts\settings.xml` for IT; restart Revit after editing it by hand.

Other files in that folder: `library.xml` (saved macros and amounts), `observed-values.xml` (learned amounts), `logs\` (usage history), `errors.log`, and with telemetry `install-id.txt` and `telemetry-state.xml`.

## Limitations

- 0.3.0 has only been exercised on Revit 2026. The other versions are built against their own APIs but have not been tested.
- Live tracking covers Revit's built-in commands. Other add-ins' commands still appear in history through journal import and can still be run from Quick Bar slots and macros.
- Automatic macro steps act on the current selection: select the walls or elements first, then run the macro.
- Extend works on straight walls and lines. Move, Copy and Offset need a learned direction, which comes from running the command once by hand.
- 0.x releases may still change.
