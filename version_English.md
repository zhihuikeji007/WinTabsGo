# WinTabsGo

**Language:** **English** | [中文](version.md)

Version zh_2026.10.07.7. Maintainer: zhihuikeji.

This file records only what zh_2026.10.07.7 adds and fixes on top of upstream WindowTabs (through ss_2026.09.21; original author Maurice Flanagan; ss_ line maintained by Satoshi Yamamoto). It does not include the upstream changelog.

## version zh_2026.10.07.7

- Fixed the whole program freezing for several seconds on a tab click or a settings page switch, then answering every queued click at once. Three stacked causes were found. First, the main thread's window scan made one virtual-desktop query per window, and that query is a remote call into the shell - about 17 ms per window with a busy explorer, over four seconds for 261 windows; cheap in-memory checks now run first and the shell call is made only for windows that actually need it. Second, every tab switch refreshed taskbar membership synchronously inside the click handler (also shell calls, 100-400 ms); the taskbar now catches up a tick later, off the input path. Third, the drag preview asked the target window to render itself (PrintWindow), which stalls for seconds on a busy window; the preview is now an instant bit-block copy from the screen.
- Fixed the settings window occasionally stalling when clicking the left navigation: a page switch forced a full synchronous repaint, and heavy pages (the program tree, the color grid) blocked the whole interface; the switch now only invalidates and lets the message loop paint.
- Fixed the loading card's content being shifted and clipped on high-DPI screens: a form created on a background thread was silently enlarged by WinForms' process-wide DPI snapshot while the rounded-corner region kept the design size; auto-scaling is now disabled and the region is rebuilt from the real size once the handle exists.
- The loading card gains a soft shadow close to a native Windows window in light mode (dark mode unchanged); the loading text reads "Loading..." and matches the interface font style.
- Fixed field captions being bold on some settings pages but not others: the Appearance page's caption host was never attached to the displayed form tree, so the bold pass never ran; captions are now uniformly 14 px bold with a self-healing retry.
- Settings bottom bar: the version label and the language switch button are vertically centered together; an over-long version string is ellipsized in English and the sidebar is wider to prevent overflow.

## version zh_2026.10.07.6

- Fixed the About page in Settings not responding to the mouse wheel when the cursor hovered over card text. Wheel messages were captured by the focused RichTextBox and never reached the page canvas; a thread-level message filter now routes them to the page canvas, so scrolling works anywhere over the card.
- Fixed the About page "jumping up and down, cannot reach the bottom" issue. The card height was hard-coded to a fixed design-unit value; the text content overflowed and was physically clipped, so the scroll range always fell short. The card now auto-sizes to its text content via the RichEdit EM_REQUESTRESIZE event, and re-measures on dialog resize or word-wrap changes.
- Fixed smooth-wheel / touchpad scrolling being discarded entirely when a single notch is split into multiple sub-120 delta messages. A fractional accumulator now carries the remainder forward to the next message instead of being truncated by integer division.

## version zh_2026.10.07.5

- Modern redesign of the settings window, rolled out page by page without changing any behavior. The old top tab strip is replaced by a left vertical navigation rail with icons and labels; the active page is marked with a blue accent pill. Every page now starts with a large title followed by rounded white cards grouped by topic — Appearance splits into "Tab Layout & Dark Mode" and "Theme Colors"; Behavior into "General & Tab Behavior", "Tab Dock Position" and "Auto Hide & Window"; Shortcut Keys into "Activate Tab" and "Switch Tabs"; Workspaces and Programs each get one card.
- New design-system controls: iOS-style toggle switches (replacing the square check boxes), a filled accent Save button, rounded card panels and navigation buttons, all with matching dark-theme colors. Pages are still laid out in 96-DPI design units, so high-DPI scaling behaves exactly as before.

## version zh_2026.10.07.4

- Fixed an unhandled-exception dialog ("'0' is not a valid value for 'Value'") when clicking "Reset:0" (Tab Overlap) and other zero-default items on the Appearance page — the numeric editor's Minimum was hard-coded to 1 and is now 0.
- Global flat restyle of the light-theme settings window: form, tab pages and panels now share one seamless white surface instead of a patchwork of SystemColors.Control gray; the top tab headers (Programs / Appearance / Behavior…) are owner-drawn — white background, centered labels, a blue underline on the selected tab, muted gray for the rest; all buttons get uniform flat white chrome with a hairline border and soft hover/press fills; text boxes and numeric up-downs switch from 3D to single-pixel modern borders. The dark-mode pipeline is untouched.

## version zh_2026.10.07.3

- Flat visual redesign: tabs are now modern rounded rectangles instead of slanted trapezoids with flared feet, and the default palette moves to soft neutral tabs with a white, hairline-outlined active pill (in the spirit of current Chrome / Edge), replacing the saturated blue and hard outlines. Tabs are taller (25→30), no longer overlap (20→0), and the maximum width grew (200→240) for an airier strip.
- Larger fonts: strip text goes from 12 px to 14 px in the locale's menu face (Microsoft YaHei UI on Chinese Windows); tab tooltips, the rename box, and the screen-ratio button scale with it. The Settings window moves from the 8.25-pt bitmap font (SimSun on zh-CN) to 13-px Segoe UI / Microsoft YaHei UI.
- Old configurations migrate automatically: a saved appearance still equal to the legacy blue defaults is switched to the new flat theme, while any configuration with at least one customized appearance field keeps the user's values untouched.

## version zh_2026.10.07.2

- Fixed pressing PrtSc while an application window has focus not opening the screen-snipping overlay (it only worked after clicking the desktop or taskbar first). The system component watching the key runs at a lower integrity, so UAC's UIPI silently drops the keystroke whenever an elevated window has focus. WinTabsGo now runs elevated and watches the bare PrintScreen key itself with a low-level keyboard hook: with no modifier held and the Windows "print screen key opens screen capture" switch on, it opens the same overlay and swallows the key, no matter which window has focus. Win+Shift+S and the Windows switch are unaffected, and every other keystroke passes through untouched.
- Each PrtSc press is logged to `%APPDATA%\WinTabsGo\PrintScreenFix.log` to keep regressions diagnosable.

## version zh_2026.10.07.1

- Fixed memory and GDI handle leaks that grew over time: context-menu icon bitmaps, settings subscriptions, per-group drag-drop registrations, the tooltip rounded region, preview brushes and the hover timer are now released when a group exits or its views are destroyed.
- Fixed tab monitoring coming back too early while several detach or move operations overlap. Suspend and resume are now counted, and monitoring resumes only after the last operation finishes.
- Fixed corner cases where an exception left the interface update counter stuck so nothing refreshed afterwards, and where removing a window aborted halfway through.
- Fixed wrong program icons in the process list for paths with non-ASCII characters such as Chinese.
- The temp file for the elevated scheduled task is now created under a random name, exclusively. The task is deleted on uninstall. The cmd.exe used for restarting after elevation comes from the system directory by full path.

## version zh_2026.09.26.8

- Task Manager can show a tab. It is no longer kept off the tab list.

## version zh_2026.09.26.7

- The first start can choose administrator permission. Allow asks Windows once. After that approval, later starts use administrator permission on their own. Not now continues with ordinary permission, and tab features stay limited.
- The consent window explains that an elevated window covers the tab strip, and says to end the WinTabsGo process and start again if authorization fails.

## version zh_2026.09.26.6

- The tab context menu ends with About WinTabsGo, and the tray menu shows the same item under Close WinTabsGo. The Chinese interface says 关于WinTabsGo.
- The About window follows the black or white theme. It is larger, with section headings, version lines, and space around the text.
- The About window keeps a single thin scrollbar. Its track uses the same color as the text, with no white frame.

## version zh_2026.09.26.3


- After the screen-ratio menu opens, moving the pointer away without entering the menu closes it. Moving onto another tab then switches by hover, without an extra click.

## version zh_2026.09.26.2

- The screen-ratio menu adds full screen. When the window is not full screen, the item says Full screen and uses the window's own top-right maximize button. When it is already full screen, the item says Exit full screen and uses that button's restore command. If the group still has a Remote Desktop in exclusive mode, leaving full screen is blocked and the existing warning is shown. The Chinese interface says 全屏 and 退出全屏.

## version zh_2026.09.26.1

- When an ordinary page goes full screen, Remote Desktop windows in the same group only stretch their frames over that area. They stay ordinary windows and do not enter exclusive mode.
- Switch to a Remote Desktop, then click its own top-right button, to put that one window into exclusive mode. The other Remote Desktop windows in the group do not follow. Each one has to be selected and clicked on its own.
- While any Remote Desktop in the group is exclusive, restoring another page or dragging its title bar does not take the group out of full screen. A warning asks you to leave exclusive mode first. The warning follows the interface language, plays the system alert sound, and appears in the center of the current screen. The heading is larger than the body, and the text is centered. Left open, it closes after six seconds. Closing it with the title-bar button or OK lets the next drag show it again immediately.
- After the group enters full screen, the other windows in the group fill the same area. Remote Desktop windows only stretch. One enters exclusive mode only when its own top-right button is clicked. When one does, the other windows still follow into full screen.
- Leaving Remote Desktop exclusive mode does not resize the other windows in the group.
- The tray language menu has two items. In Chinese it shows 语言 / 英文 / 中文. In English it shows Language / English / Chinese.
- Each tab group has a screen-ratio tab at the end of the strip. It is drawn like the other tabs and follows them to the left or the right. Click it, or rest the pointer on it, to choose 25%, 50%, or 75%. With no exclusive Remote Desktop, the window is centered on the current screen, and both its width and its height take that percentage. If the group still has an exclusive window, the size stays and the existing warning is shown. Dragging the title bar or resizing on the same screen clears the ratio. Dragging onto another screen recenters the window there.

## version zh-2026092501

### Product

- The product name is WinTabsGo. The tray icon, the Settings window, and the settings file all show zh-2026092501.
- Settings are stored in `%APPDATA%\WinTabsGo`. An old `%APPDATA%\WindowTabs` folder is not read or copied.
- The online update check has been removed, so the program can run on a private network or with no network.

### Added on top of the upstream program

- **Tabs can dock on four edges.** In Settings, on the Behavior page, choose top, bottom, left, or right. Top matches the upstream behavior. Bottom is a horizontal strip inside the window, along the bottom edge. Left and right are vertical strips inside the window, and clicks line up with the rotated tabs. The default is still top. Choosing a different edge for each tab group is not available yet.
- **The close button on a tab is off by default.** You can turn it on from the Behavior page. On that page, the dock edge and the options under it sit on one row.
- **Hovering a tab activates it.** You can set how long the pointer must stay. The default is 200 milliseconds.
- **Remote Desktop full screen.** The tab strip stays visible in full screen, and you can choose not to hide tabs in full screen. After leaving full screen, windows in the same group follow back into place, so the Remote Desktop connection bar is not pushed out of layout.
- **Settings are saved by hand.** Changes in the dialog stay in a draft until you click Save. Save writes them to disk and applies them to windows and shortcut keys. Closing with unsaved changes asks you to save, discard, or cancel.
- **Explanations in Settings.** The Appearance, Behavior, Shortcut keys, and Workspace pages have explanation lines. Numeric fields are more compact, and the Settings window is wider. On the Shortcut keys page, the number keys line up with the hotkey boxes, and the Ctrl and Alt modes sit on one row.

### Fixed on top of the upstream program

- Saving settings now applies the appearance and custom tab colors. Saving is less likely to freeze the window. Opening Settings no longer crashes on the case that used to fail. Switching dark mode no longer leaves a stale cache. Shortcut-key numbers are read from more JSON value types.
- Tab tooltips on all four edges are anchored to the edge they belong to, and they stay inside the window's client area.
- Dragging the title bar while a window is full screen no longer flashes back to full screen. After the drag ends, the group is laid out again.
