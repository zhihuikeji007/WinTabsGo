# WinTabsGo Usage

**Language:** **English** | [中文](使用说明.md)

Version: zh_2026.10.07.7  
Maintainer: zhihuikeji  
Systems: Windows 10 and Windows 11

Download the installer or the portable package from the release page. Use one of them.

| File | Purpose |
|------|---------|
| [WinTabsGo-zh_2026.10.07.7.msi](https://github.com/zhihuikeji007/WinTabsGo/releases/download/zh_2026.10.07.7/WinTabsGo-zh_2026.10.07.7.msi) | Installer. The default folder is `C:\Program Files\WinTabsGo` |
| [WinTabsGo-zh_2026.10.07.7.zip](https://github.com/zhihuikeji007/WinTabsGo/releases/download/zh_2026.10.07.7/WinTabsGo-zh_2026.10.07.7.zip) | Portable package. Unzip it and run `WinTabsGo.exe` |
| `README.md` | Release notes in Chinese. English is `README_English.md` |
| `Usage.md` | This file. Chinese is `使用说明.md` |
| `version.md` | Changelog in Chinese. English is `version_English.md` |
| `版权声明.md` | Copyright summary in Chinese |
| `LICENSE.txt` | Full MIT license, in English. This text is the license |

The portable zip also contains the Chinese and English notes, `LICENSE.txt`, and `version.md`.

## Installer

1. Double-click `WinTabsGo-zh_2026.10.07.7.msi`.
2. Finish the wizard.
3. Start WinTabsGo from the Start menu or the desktop shortcut.

Windows records the installer version as `26.10.07.7`. The tray icon and the Settings window show `zh_2026.10.07.7`. Windows Installer only accepts a numeric version.

## Portable package

1. Unzip `WinTabsGo-zh_2026.10.07.7.zip` into a folder you can write to.
2. Run `WinTabsGo.exe` in that folder.
3. Keep the `Settings` folder beside the exe. It holds the languages and the shipped defaults. Do not copy the exe alone.

## First run

After startup, a WinTabsGo icon appears in the notification area, at the right end of the taskbar.

1. Right-click the tray icon and open Settings.
2. On the Programs page, tick the programs that should get tabs.
3. Open those programs. Tabs appear on their windows.
4. Drag a tab to reorder it, to pull it out as its own window, or to drop it into another group.
5. Right-click a tab to snap, close, rename, pin, or color it, or to move the whole group into another group.

The tray menu changes the interface language without a restart. The choices include Simplified Chinese, Traditional Chinese, English, Japanese, and other language packs.

Right-click the tray icon and choose Exit to close WinTabsGo. If you turn on run at startup, Windows starts it when you sign in.

## Where settings are saved

Your settings are stored in:

`%APPDATA%\WinTabsGo\WinTabsGoSettings.txt`

That is:

`C:\Users\your-user-name\AppData\Roaming\WinTabsGo\WinTabsGoSettings.txt`

This version does not read or copy an old `%APPDATA%\WindowTabs` folder. Settings from an earlier product are not brought over.

The `Settings` folder beside the exe is the shipped default. To change one default, put a file of the same name under `%APPDATA%\WinTabsGo\Settings\`. WinTabsGo overlays that file entry by entry.

## Copyright and changelog

- The Chinese summary is `版权声明.md`. The license itself is the English `LICENSE.txt` in the same folder. WinTabsGo is released under the MIT license.
- Copyright holders: Copyright (c) 2018 Maurice Flanagan; Copyright (c) 2026 zhihuikeji.
- What this version adds and fixes on top of upstream is in `version_English.md`. That file does not include the upstream changelog. The Chinese text is `version.md`.
