# GUI Application Features

**Date:** 2026-10-20 0506 CDT

A catalog of the features a desktop GUI application is expected to have, according to the **standards** and **specifications** covered in JAUs-Reference-Docs. Each feature lists where it comes from, so it is clear whether it rests on a standard (ISO, ANSI) or on a company specification (IBM CUA, Microsoft, Apple, GNOME, KDE).

For a quick definition of each named element (title bar, toolbar, menu item, etc.), jump to **§19, UI Element Glossary**.

For details of each source, see [Application-UI-Standards-and-Specs.md](Application-UI-Standards-and-Specs.md). For the terms *standard*, *specification*, and *product*, see [Specifications-Standards-and-Products.md](Specifications-Standards-and-Products.md).

> **A note on the title:** These are often called "standard GUI features." Under the terminology in these docs, most of them are **specification** conventions (mainly from IBM CUA) that a **standard** (ISO 9241) later recorded. This document calls them *expected features* and tags each with its actual source.

## Contents

1. Source Key
2. Feature Summary
3. Main Window
4. Menu Bar and Standard Menus
5. Menu Item Conventions
6. Context Menus
7. Toolbars and Icons
8. Status Bar
9. Keyboard Interaction
10. Dialogs
11. Controls
12. Selection Model
13. Messages and Feedback
14. Help and Documentation
15. Document Handling and Undo
16. Preferences
17. Implementing These Features in PyQt
18. Checklist
19. UI Element Glossary
- References to Verify

---

## 1. Source Key

Tags used in this document:

| Tag | Source | Kind |
|-----|--------|------|
| **ISO-14** | ISO 9241-14, *Menu dialogues* (1997) | Standard |
| **ISO-161** | ISO 9241-161, *Guidance on visual user interface elements* | Standard |
| **ISO-11581** | ISO/IEC 11581, *User interface symbols and functions* | Standard |
| **ISO-15910** | ISO/IEC 15910, *Software user documentation process* | Standard |
| **HFES-200** | ANSI/HFES 200, U.S. adoption of ISO 9241 software ergonomics | Standard |
| **P1201.2** | IEEE P1201.2, *GUI Drivability* (draft project, never approved) | Draft standard project |
| **CUA** | IBM Common User Access (1987–1992) | Specification |
| **Motif/CDE** | OSF/Motif Style Guide; Common Desktop Environment | Specification |
| **Win** | Microsoft Windows App Design guidelines | Specification |
| **Mac** | Apple Human Interface Guidelines | Specification |
| **GNOME** | GNOME Human Interface Guidelines | Specification |
| **KDE** | KDE Human Interface Guidelines | Specification |

Because Motif, CDE, Windows, GNOME, and KDE all inherited CUA conventions, a feature tagged **CUA** is generally also found in those specifications unless noted otherwise.

---

## 2. Feature Summary

| # | Feature | Standards | Specifications |
|---|---------|-----------|----------------|
| 3.1 | Main window with title bar, menu bar, work area | ISO-161 | CUA, Motif/CDE, Win, Mac, GNOME, KDE |
| 4.1 | Horizontal menu bar ("action bar") with pull-down menus | ISO-14, ISO-161 | CUA, Motif/CDE, Win, Mac, KDE |
| 4.2 | Standard menu order: File first, Help last | ISO-14 | CUA, Motif/CDE, Win, Mac, KDE |
| 4.3 | Standard File menu items (New, Open..., Save, Save as..., Print..., Exit) | — | CUA, Win, Mac, KDE |
| 4.4 | Standard Edit menu items (Undo, Cut, Copy, Paste, Delete) | — | CUA, Win, Mac, KDE |
| 5.1 | Ellipsis (`...`) on items that open a dialog | — | CUA, Win, Mac, GNOME, KDE |
| 5.2 | Unavailable items grayed out, not removed | ISO-14 | CUA, Win, Mac, KDE |
| 5.3 | Mnemonics (underlined access letters) | ISO-14, P1201.2 | CUA, Motif/CDE, Win, KDE |
| 5.4 | Accelerator keys shown beside menu items | ISO-14, P1201.2 | CUA, Win, Mac, GNOME, KDE |
| 5.5 | Separators, check marks, and cascading submenus | ISO-14 | CUA, Win, Mac, KDE |
| 6.1 | Context (pop-up) menus | ISO-161 | Win, Mac, GNOME, KDE |
| 7.1 | Toolbar duplicating common menu commands | ISO-11581 | Win, Mac, GNOME, KDE |
| 7.2 | Standard icons for common functions | ISO-11581 | Win, Mac, GNOME, KDE |
| 8.1 | Status bar for state and short messages | — | Win, KDE |
| 9.1 | Full keyboard operation | ISO-14, HFES-200, P1201.2 | CUA, all platform specs |
| 9.2 | Standard function and navigation keys (F1, F10/Alt, Tab, Esc, Enter) | ISO-14 | CUA, Win, KDE |
| 9.3 | Standard clipboard and command shortcuts | — | CUA, Win, Mac, GNOME, KDE |
| 10.1 | Dialog boxes with OK / Cancel / Help push buttons | ISO-161 | CUA, Win, Mac, GNOME, KDE |
| 10.2 | Default button on Enter, Cancel on Esc | — | CUA, Win, Mac, GNOME, KDE |
| 11.1 | Standard control set (entry field, radio, check box, list, combo box, push button) | ISO-161 | CUA, all platform specs |
| 12.1 | Object–action selection model | — | CUA |
| 13.1 | Messages classed as Information / Warning / Error, each with its own icon | — | CUA, Win, Mac, GNOME, KDE |
| 13.2 | Messages say what happened and what to do next | HFES-200 | CUA, all platform specs |
| 14.1 | Help menu, context-sensitive help on F1, About box | ISO-14 | CUA, Win, KDE |
| 14.2 | User documentation produced alongside the interface | ISO-15910 | — |
| 15.1 | Prompt to save unsaved changes | — | Win, Mac, GNOME, KDE |
| 15.2 | Undo / Redo | — | CUA, Win, Mac, GNOME, KDE |
| 16.1 | Preferences / Options dialog | — | CUA (Options menu), Win, Mac, GNOME, KDE |

A dash (—) means none of the sources in these docs is known to address the feature directly. Where a standard is listed beyond what the companion docs state, see *References to Verify*.

---

## 3. Main Window

### 3.1 Window Layout

The primary application window has these parts, from top to bottom:

| Part | Purpose | Source |
|------|---------|--------|
| **Title bar** | Shows the application name and, for document apps, the document name | CUA, Win, Mac, GNOME, KDE |
| **Menu bar** | Access to every command | CUA, ISO-14, ISO-161 |
| **Toolbar** (optional) | Quick access to frequent commands | Win, Mac, GNOME, KDE |
| **Work area** | The document, data, or main content | CUA |
| **Status bar** (optional) | Current state and brief messages | Win, KDE |

- Document applications commonly put the document name before the application name in the title, e.g. `report.txt — MyEditor`, and mark unsaved changes (e.g. a leading `*`). *(Win, GNOME, KDE)*
- The window can be moved, resized, minimized, maximized, and closed through the window manager. Closing the main window is equivalent to **File > Exit**. *(CUA, all platform specs)*
- **Platform difference:** On macOS the menu bar is at the top of the screen, not in the window. GNOME's current guidelines replace the menu bar with a header bar and a primary ("hamburger") menu. Windows and KDE keep the in-window menu bar.

---

## 4. Menu Bar and Standard Menus

### 4.1 Menu Bar and Pull-Downs

A horizontal menu bar runs along the top of the window. Selecting an item opens a pull-down menu beneath it. *(ISO-14, ISO-161, CUA)*

This is the oldest and most consistently specified feature in the whole set. CUA called it the **action bar**. ISO 9241-14 later recorded it as a standard.

### 4.2 Menu Order

| Position | Menu | Notes | Source |
|----------|------|-------|--------|
| First | **File** | Document and application-level actions | CUA, ISO-14 |
| Second | **Edit** | Clipboard, undo, selection | CUA, ISO-14 |
| Third | **View** | Display options; doesn't change data | CUA |
| Middle | *Application-specific menus* | Go between View and Help | CUA |
| Near end | **Options** / **Tools** / **Settings** | Name varies by platform | CUA (Options), Win (Tools), KDE (Settings) |
| Last | **Help** | Always last | CUA, ISO-14 |

On macOS an **application menu** (named after the app, holding About, Settings, and Quit) comes before File. *(Mac)*

### 4.3 Standard File Menu Items

| Item | Typical shortcut | Source |
|------|------------------|--------|
| New | Ctrl+N | CUA (item); Win, Mac, GNOME, KDE (shortcut) |
| Open... | Ctrl+O | CUA (item); Win, Mac, GNOME, KDE (shortcut) |
| Save | Ctrl+S | CUA (item); Win, Mac, GNOME, KDE (shortcut) |
| Save as... | Ctrl+Shift+S | CUA (item); KDE, GNOME (shortcut) |
| Print... | Ctrl+P | CUA (item); Win, Mac, GNOME, KDE (shortcut) |
| Exit / Quit | Alt+F4 (Win); Ctrl+Q (GNOME, KDE); Cmd+Q (Mac) | CUA ("Exit"); Mac, GNOME, KDE ("Quit") |

Many applications also add **Close** (Ctrl+W) and **Recent Files** to the File menu. *(Win, Mac, GNOME, KDE)*

### 4.4 Standard Edit Menu Items

| Item | Modern shortcut | Original CUA shortcut |
|------|-----------------|-----------------------|
| Undo | Ctrl+Z | Alt+Backspace |
| Redo | Ctrl+Y (Win) / Ctrl+Shift+Z (Mac, GNOME, KDE) | — |
| Cut | Ctrl+X | Shift+Del |
| Copy | Ctrl+C | Ctrl+Ins |
| Paste | Ctrl+V | Shift+Ins |
| Delete / Clear | Del | Del |
| Select All | Ctrl+A | — |
| Find... | Ctrl+F | — |

The Ctrl+X / C / V bindings came from Apple's Macintosh specification and were later added alongside the CUA bindings in Windows. Most Windows applications still accept both sets. *(CUA, Mac, Win)*

### 4.5 Help Menu Items

| Item | Source |
|------|--------|
| *Application* Help / Contents (F1) | CUA, Win, KDE |
| About *Application* | Win, KDE (in the app menu on Mac) |

---

## 5. Menu Item Conventions

### 5.1 Ellipsis for Dialogs

An **ellipsis (`...`)** after a menu item means it opens a dialog that needs more input before the action runs. Items that act immediately have no ellipsis. *(CUA, Win, Mac, GNOME, KDE)*

- `Save` acts immediately (no ellipsis).
- `Save as...` opens a dialog to ask for a file name (ellipsis).

### 5.2 Disabled Items

Unavailable choices are shown **grayed out** (disabled), not removed, so the menu layout stays the same and users can learn where commands are. *(CUA, ISO-14)*

Example: **Edit > Paste** is grayed out when the clipboard is empty.

### 5.3 Mnemonics

Every menu and menu item has a **mnemonic**, an underlined letter that selects it from the keyboard. *(CUA, ISO-14, P1201.2)*

- **Alt + mnemonic** opens a top-level menu (e.g. Alt+F for File).
- Once a menu is open, the mnemonic alone selects an item.
- Mnemonics must be unique within each menu.
- Conventional mnemonics: **F**ile, **E**dit, **V**iew, **H**elp, **N**ew, **O**pen, **S**ave, Save **A**s, **P**rint, E**x**it.
- macOS does not use mnemonics. *(Mac)*

### 5.4 Accelerator Keys

An **accelerator** (shortcut key) runs a command directly without opening the menu. The accelerator is shown to the right of the menu item so users can learn it. *(ISO-14, P1201.2, CUA, all platform specs)*

Mnemonics and accelerators are different: a mnemonic works only while navigating menus; an accelerator works anywhere in the window.

### 5.5 Other Menu Item Conventions

| Convention | Meaning | Source |
|------------|---------|--------|
| **Separator line** | Groups related items | ISO-14, CUA |
| **Check mark** | On/off setting (e.g. View > Status Bar) | CUA, all platform specs |
| **Radio mark** | One choice from a group (e.g. View > Zoom level) | CUA, all platform specs |
| **Arrow (▸)** | Opens a cascading submenu | ISO-14, CUA |

Submenus should be kept shallow, ideally one level. *(ISO-14, Win)*

---

## 6. Context Menus

### 6.1 Pop-Up Menus

A **context menu** (pop-up menu) appears at the pointer when the user right-clicks, or presses **Shift+F10** or the **Menu** key. It holds the most relevant commands for the selected object. *(ISO-161, Win, Mac, GNOME, KDE)*

- Every command in a context menu must also be available elsewhere (menu bar or toolbar), because context menus are not discoverable. *(Win, KDE)*
- Shift+F10 traces back to CUA '91's object-oriented Workplace model, where users acted directly on objects. *(CUA)*

---

## 7. Toolbars and Icons

### 7.1 Toolbar

A **toolbar** holds buttons for the most frequent commands. *(Win, Mac, GNOME, KDE)*

- Toolbar buttons duplicate menu commands; they are shortcuts, not the only path to a command.
- Each button has a **tooltip** naming the command (and its shortcut).
- Toolbar buttons follow the same enable/disable state as the matching menu items.
- Users can usually hide the toolbar (View > Toolbar). *(Win, KDE)*

### 7.2 Standard Icons

ISO/IEC 11581 standardizes icons and symbols for common functions so that users recognize them across applications. *(ISO-11581)* In practice, applications use the platform's icon theme (e.g. freedesktop.org icon names on GNOME and KDE) for New, Open, Save, Print, Cut, Copy, Paste, Undo, Redo, and Help.

---

## 8. Status Bar

### 8.1 Status Bar

A **status bar** along the bottom of the window shows the application's current state (line/column, zoom, mode) and brief, non-blocking messages, such as a description of the menu item under the pointer. *(Win, KDE)*

- Status messages must not be the only place important information appears.
- The status bar can usually be hidden (View > Status Bar).

---

## 9. Keyboard Interaction

### 9.1 Full Keyboard Operation

Every function must be reachable from the keyboard alone, through mnemonics, accelerators, and focus navigation. *(ISO-14, HFES-200, P1201.2, CUA, all platform specs)*

This was CUA's central requirement, since CUA had to work on terminals with no mouse. It is now also an accessibility requirement.

### 9.2 Standard Keys

| Key | Function | Source |
|-----|----------|--------|
| F1 | Help (context-sensitive) | CUA |
| F10 / Alt | Move focus to / from the menu bar | CUA |
| Alt + mnemonic | Open a menu directly | CUA |
| Shift+F10 / Menu key | Open the context menu | CUA, Win |
| Tab / Shift+Tab | Move between fields and controls | CUA |
| Arrow keys | Move within a group or list | CUA |
| Enter | Activate the default push button | CUA |
| Esc | Cancel / back out one level | CUA |
| Space | Toggle a check box or press the focused button | CUA, Win |
| Alt+F4 | Close the window | Win |
| Ctrl+F4 / Ctrl+W | Close the current document or tab | Win, GNOME, KDE |

### 9.3 Focus and Tab Order

- Exactly one control has **keyboard focus** at a time, and it is visibly marked (focus rectangle or highlight). *(CUA, all platform specs)*
- **Tab order** follows the visual reading order: left to right, top to bottom. *(CUA, Win, KDE)*
- When a dialog opens, focus goes to the first input field or the default control. *(CUA, Win)*

---

## 10. Dialogs

### 10.1 Dialog Boxes

A **dialog box** collects input or asks for confirmation before an action runs. *(ISO-161, CUA, all platform specs)*

| Element | Convention | Source |
|---------|-----------|--------|
| **Push buttons** | **OK**, **Cancel**, **Help**, placed consistently in every dialog | CUA |
| **Button order** | OK then Cancel (Windows, KDE); Cancel then OK (macOS, GNOME) | Win, KDE, Mac, GNOME |
| **Title** | Names the command that opened it (e.g. *Save As*) | CUA, Win |
| **Modality** | Modal dialogs block the parent window until closed; use them only when the user must respond first | Win, Mac, GNOME, KDE |

Action-specific button labels (e.g. **Save**, **Delete**) are often clearer than a generic **OK**. *(Win, Mac, GNOME, KDE)*

### 10.2 Default and Cancel Buttons

- **Enter** activates the **default button**, which is visually emphasized. *(CUA)*
- **Esc** activates **Cancel** and closes the dialog with no changes. *(CUA)*
- The default should be the safe choice when an action is destructive. *(Win, Mac, GNOME, KDE)*

### 10.3 Common Dialogs

Applications should use the platform's common dialogs instead of building their own, so users see familiar layouts: **File Open / Save**, **Print**, **Font**, **Color**, and **Find / Replace**. *(Win, Mac, GNOME, KDE)*

---

## 11. Controls

### 11.1 Standard Control Set

| Control | Use | Source |
|---------|-----|--------|
| **Entry field** (text box) | Free text input | CUA, ISO-161 |
| **Radio buttons** | Choose exactly one of a small, mutually exclusive set | CUA, ISO-161 |
| **Check boxes** | Independent on/off choices | CUA, ISO-161 |
| **List box** | Choose one or more from a longer list | CUA, ISO-161 |
| **Combination box** (combo / drop-down) | Choose from a list, or type a value, in little space | CUA, ISO-161 |
| **Push button** | Run an action | CUA, ISO-161 |
| **Spin box**, **slider** | Numeric values | Win, Mac, GNOME, KDE |
| **Tabs** (notebook) | Divide a large dialog into pages | Win, Mac, GNOME, KDE |
| **Group box** | Visually group related controls | CUA, Win, KDE |

Each control has a **label**, and the label carries the control's mnemonic. *(CUA, Win, KDE)*

---

## 12. Selection Model

### 12.1 Object–Action

The user selects an **object** first, then chooses an **action** to apply to it from the menu, toolbar, or context menu. *(CUA)*

- Menu items that need a selection are grayed out until something is selected (see 5.2).
- Standard selection gestures: click to select; Shift+click to extend a range; Ctrl+click to add or remove one item; Ctrl+A to select all. *(Win, GNOME, KDE; Cmd instead of Ctrl on Mac)*

---

## 13. Messages and Feedback

### 13.1 Message Types

| Type | Use | Source |
|------|-----|--------|
| **Information** | Tells the user something; no decision needed | CUA |
| **Warning** | An action may have unwanted results; user can continue or cancel | CUA |
| **Action / Error** | Something failed or the user must act before continuing | CUA |
| **Question** / confirmation | Asks the user to choose (e.g. *Save changes?*) | Win, KDE |

Each type has its own icon. *(CUA, all platform specs)*

### 13.2 Message Content

- Every message says **what happened** and **what to do next**. *(CUA)*
- Use plain language, not error codes alone. *(HFES-200, all platform specs)*
- Destructive actions (delete, overwrite, discard) ask for confirmation. *(Win, Mac, GNOME, KDE)*

### 13.3 Progress and Busy Feedback

Long operations show a busy pointer or a **progress indicator**, and offer **Cancel** when possible. *(Win, Mac, GNOME, KDE)*

---

## 14. Help and Documentation

### 14.1 Help in the Application

- A **Help** menu is always the last menu. *(CUA, ISO-14)*
- **F1** opens help for the current context (the focused window, dialog, or field). *(CUA)*
- Dialogs that need explanation have a **Help** button. *(CUA)*
- **Help > About** shows the application name, version, copyright, and license. *(Win, KDE)*
- **Tooltips** briefly describe controls and toolbar buttons. *(Win, Mac, GNOME, KDE)*

### 14.2 User Documentation

ISO/IEC 15910 covers the **process** for producing user documentation (help and manuals) that accompanies an interface: planning it, writing it, and reviewing it. It does not prescribe the UI of the help system itself. *(ISO-15910)*

---

## 15. Document Handling and Undo

### 15.1 Unsaved Changes

- The title bar marks a document with unsaved changes. *(Win, GNOME, KDE)*
- Closing a window, closing a document, or exiting with unsaved changes prompts: **Save**, **Don't Save** (or **Discard**), **Cancel**. *(Win, Mac, GNOME, KDE)*
- **Save** on a never-saved document behaves like **Save as...**. *(Win, Mac, GNOME, KDE)*

### 15.2 Undo and Redo

Actions that change data can be undone (**Edit > Undo**) and redone (**Edit > Redo**). The menu item ideally names the action, e.g. *Undo Typing*. Undo lets users explore safely and reduces the need for confirmation messages. *(CUA, Win, Mac, GNOME, KDE)*

---

## 16. Preferences

### 16.1 Preferences / Options

User-configurable settings live in one dialog. Its location and name vary by specification:

| Specification | Location |
|---------------|----------|
| CUA | **Options** menu |
| Windows | **Tools > Options...** (traditional) |
| macOS | *App* menu **> Settings...** (Cmd+,), formerly *Preferences* |
| GNOME | Primary menu **> Preferences** |
| KDE | **Settings > Configure *App*...** |

The application should remember settings, and window size and position, between sessions. *(Win, Mac, GNOME, KDE)*

---

## 17. Implementing These Features in PyQt

Qt follows most of these conventions by default. The main points for PyQt applications:

| Feature | PyQt approach |
|---------|---------------|
| Main window parts (3.1) | `QMainWindow` with `menuBar()`, `addToolBar()`, `statusBar()`, `setCentralWidget()` |
| Mnemonics (5.3) | `&` in the text: `QAction("&Open...", self)` |
| Ellipsis (5.1) | Put `...` in the action text for dialog-opening items |
| Accelerators (5.4, 9.3) | `action.setShortcut(QKeySequence.StandardKey.Open)`; standard keys map to each platform's specification automatically |
| Disabled items (5.2) | `action.setEnabled(False)`; the same `QAction` in a menu and a toolbar stays in sync |
| Check marks (5.5) | `action.setCheckable(True)`; `QActionGroup` for radio-style groups |
| Tooltips and status tips (7.1, 8.1) | `action.setToolTip()`, `action.setStatusTip()` |
| Context menus (6.1) | `widget.setContextMenuPolicy(Qt.ContextMenuPolicy.ActionsContextMenu)` or handle `customContextMenuRequested` |
| Standard icons (7.2) | `QIcon.fromTheme("document-open")` (freedesktop.org icon names) |
| Dialog buttons (10.1) | `QDialogButtonBox` orders OK/Cancel per platform automatically |
| Default / Esc (10.2) | `QDialog` handles Enter (default button) and Esc (reject) |
| Common dialogs (10.3) | `QFileDialog`, `QPrintDialog`, `QFontDialog`, `QColorDialog` |
| Message types (13.1) | `QMessageBox.information()`, `.warning()`, `.critical()`, `.question()` |
| Unsaved changes (15.1) | `setWindowModified(True)` with `[*]` in the window title; override `closeEvent()` to prompt |
| Undo / Redo (15.2) | `QUndoStack` with `createUndoAction()` / `createRedoAction()` |
| Tab order (9.3) | `QWidget.setTabOrder()` |
| Settings (16.1) | `QSettings`; save geometry with `saveGeometry()` / `restoreGeometry()` |

---

## 18. Checklist

A quick review list for a new desktop application:

- [ ] Menu bar with **File** first and **Help** last
- [ ] File menu: New, Open..., Save, Save as..., Print... (if applicable), Exit/Quit
- [ ] Edit menu: Undo, Redo, Cut, Copy, Paste, Delete, Select All (as applicable)
- [ ] Every menu and item has a unique mnemonic
- [ ] Common commands have standard accelerators, shown in the menu
- [ ] `...` on every item that opens a dialog, and only those
- [ ] Unavailable items are grayed out, not hidden
- [ ] Toolbar buttons duplicate menu commands and have tooltips
- [ ] Every function works from the keyboard; tab order is logical; focus is visible
- [ ] Dialogs: Enter = default button, Esc = Cancel; platform button order
- [ ] Platform common dialogs for files, printing, fonts, colors
- [ ] Messages use the right type and icon, and say what to do next
- [ ] Destructive actions confirm or can be undone
- [ ] Unsaved changes are marked in the title and prompted on close/exit
- [ ] F1 help, and Help > About with version information
- [ ] Settings in one Preferences/Options dialog; settings and window geometry persist

---

## 19. UI Element Glossary

Each named element of a desktop GUI application, what it is, and where its conventions are described in this document. Alternate names used by different specifications are shown in parentheses.

### 19.1 Window Elements

| Element | Description | Source | See |
|---------|-------------|--------|-----|
| **Main window** (primary window, application window) | The top-level window that holds the application's menus, toolbars, and work area | CUA, ISO-161, all platform specs | §3 |
| **Title bar** | Strip across the top of a window showing the application and document name, with window controls; dragging it moves the window | CUA, all platform specs | §3.1 |
| **Window controls** (minimize, maximize/restore, close buttons) | Buttons in the title bar that change or close the window | CUA, all platform specs | §3.1 |
| **System menu** (window menu, control menu) | Menu opened from the title bar icon or Alt+Space, with Move, Size, Minimize, Maximize, Close | CUA, Win, KDE | — |
| **Header bar** | GNOME's combined title bar and toolbar, holding the window title, main buttons, and primary menu | GNOME | §3.1 |
| **Work area** (client area, central widget) | The main content region: the document, data, or view | CUA | §3.1 |
| **Scroll bar** | Bar along the edge of a view for moving content that does not fit; has a slider (thumb) and often arrow buttons | CUA, ISO-161, all platform specs | — |
| **Splitter** (pane divider) | Draggable divider between two resizable panes | Win, Mac, GNOME, KDE | — |
| **Dock / side panel** | Secondary panel (e.g. file tree, properties) attached to the main window edges; can often be moved, hidden, or detached | Win, KDE | — |
| **Status bar** | Strip along the bottom of the window showing state and brief messages | Win, KDE | §8 |
| **Size grip** | Corner area for resizing the window | Win | — |

### 19.2 Menu Elements

| Element | Description | Source | See |
|---------|-------------|--------|-----|
| **Menu bar** (action bar in CUA) | Horizontal bar of top-level menu titles across the top of the window | ISO-14, ISO-161, CUA | §4.1 |
| **Menu title** | A name in the menu bar (File, Edit, ...) that opens a pull-down | ISO-14, CUA | §4.2 |
| **Pull-down menu** (drop-down menu) | The list of items that opens below a menu title | ISO-14, ISO-161, CUA | §4.1 |
| **Menu item** (choice, command) | One entry in a menu that runs a command or opens a dialog or submenu | ISO-14, CUA | §4.3–4.5, §5 |
| **Cascading menu** (submenu) | A menu opened from a menu item marked with an arrow (▸) | ISO-14, CUA | §5.5 |
| **Separator** | Horizontal line grouping related menu items | ISO-14, CUA | §5.5 |
| **Check item** (toggle item) | Menu item with a check mark showing an on/off setting | CUA, all platform specs | §5.5 |
| **Radio item** | Menu item in a group where exactly one is chosen | CUA, all platform specs | §5.5 |
| **Mnemonic** (access key) | Underlined letter in a menu title, item, or label that selects it from the keyboard | ISO-14, P1201.2, CUA | §5.3 |
| **Accelerator** (shortcut key, keyboard shortcut) | Key combination that runs a command directly; shown to the right of the menu item | ISO-14, P1201.2, CUA | §5.4, §9.2 |
| **Ellipsis** (`...`) | Marker on a menu item or button that opens a dialog for more input | CUA, all platform specs | §5.1 |
| **Context menu** (pop-up menu, shortcut menu) | Menu at the pointer, opened by right-click, Shift+F10, or the Menu key | ISO-161, Win, Mac, GNOME, KDE | §6 |
| **Primary menu** (hamburger menu, ☰) | Single button-opened menu that replaces the menu bar in some designs | GNOME | §3.1 |
| **Application menu** | macOS menu named after the app, before File, holding About, Settings, and Quit | Mac | §4.2 |

### 19.3 Toolbar Elements

| Element | Description | Source | See |
|---------|-------------|--------|-----|
| **Toolbar** (tool bar) | Row of buttons for frequent commands, usually below the menu bar | Win, Mac, GNOME, KDE | §7.1 |
| **Tool button** | Icon button on a toolbar that runs the same command as a menu item | Win, Mac, GNOME, KDE | §7.1 |
| **Toggle tool button** | Tool button that stays pressed to show an on state (e.g. Bold) | Win, Mac, GNOME, KDE | §7.1 |
| **Drop-down tool button** (split button) | Tool button with an attached arrow opening a menu of related commands | Win, KDE | — |
| **Icon** | Small picture representing a command, object, or message type | ISO-11581 | §7.2 |
| **Tooltip** | Small pop-up label naming a control when the pointer rests on it | Win, Mac, GNOME, KDE | §7.1, §14.1 |

### 19.4 Dialog Elements

| Element | Description | Source | See |
|---------|-------------|--------|-----|
| **Dialog box** (secondary window) | Window that collects input or confirmation before an action runs | ISO-161, CUA, all platform specs | §10.1 |
| **Modal dialog** | Dialog that blocks its parent window until closed | Win, Mac, GNOME, KDE | §10.1 |
| **Modeless dialog** | Dialog that stays open while the user keeps working (e.g. Find) | Win, Mac, GNOME, KDE | §10.1 |
| **Message box** (alert) | Small dialog with a type icon, a message, and response buttons | CUA, all platform specs | §13 |
| **Common dialog** | Platform-supplied dialog for files, printing, fonts, or colors | Win, Mac, GNOME, KDE | §10.3 |
| **Push button** (command button) | Button that runs an action: OK, Cancel, Help, Apply, etc. | CUA, ISO-161 | §10.1, §11.1 |
| **Default button** | Emphasized push button activated by Enter | CUA | §10.2 |
| **Button box** (action area) | The row of push buttons at the bottom or side of a dialog | CUA, all platform specs | §10.1 |
| **Property sheet / tabbed dialog** | Dialog whose settings are divided into tab pages | Win, KDE | §11.1, §16.1 |
| **Wizard** (assistant) | Sequence of dialog pages with Back / Next / Finish for multi-step tasks | Win, GNOME, KDE | — |

### 19.5 Controls

| Element | Description | Source | See |
|---------|-------------|--------|-----|
| **Label** | Static text naming a control; carries the control's mnemonic | CUA, all platform specs | §11.1 |
| **Entry field** (text box, line edit) | Single-line text input | CUA, ISO-161 | §11.1 |
| **Multi-line text area** | Text input for several lines | CUA, all platform specs | — |
| **Check box** | Independent on/off choice | CUA, ISO-161 | §11.1 |
| **Radio button** (option button) | One choice from a mutually exclusive group | CUA, ISO-161 | §11.1 |
| **List box** | Scrollable list for choosing one or more items | CUA, ISO-161 | §11.1 |
| **Combination box** (combo box, drop-down list) | Compact list choice, optionally with typed input | CUA, ISO-161 | §11.1 |
| **Spin box** (spin button) | Numeric field with up/down arrows | Win, Mac, GNOME, KDE | §11.1 |
| **Slider** | Choose a value by dragging along a range | Win, Mac, GNOME, KDE | §11.1 |
| **Tabs** (notebook, tab control) | Row of tabs switching between pages in the same space | Win, Mac, GNOME, KDE | §11.1 |
| **Group box** (frame) | Bordered, titled box grouping related controls | CUA, Win, KDE | §11.1 |
| **Tree view** | Hierarchical list with expandable nodes | Win, Mac, GNOME, KDE | — |
| **Table / list view** | Rows and columns of items, often sortable by column header | Win, Mac, GNOME, KDE | — |
| **Progress bar** (progress indicator) | Shows how far a long operation has gone | Win, Mac, GNOME, KDE | §13.3 |

### 19.6 Pointer and Focus Elements

| Element | Description | Source | See |
|---------|-------------|--------|-----|
| **Pointer** (mouse cursor) | On-screen arrow moved by the mouse; changes shape for busy, text, resize, etc. | CUA, all platform specs | §13.3 |
| **Text cursor** (caret, insertion point) | Blinking bar showing where typed text will go | CUA, all platform specs | — |
| **Focus indicator** (focus rectangle) | Visible mark on the control that receives keyboard input | CUA, all platform specs | §9.3 |
| **Selection highlight** | Visual marking of selected objects or text | CUA, all platform specs | §12 |

---

## References to Verify

This document builds on the companion docs. Items beyond what those docs state were written from general knowledge of the specifications and should be checked against primary sources before citing:

- **ISO 9241-14 and ISO 9241-161 coverage:** which specific features in §2 each part addresses (e.g. separators, submenu depth, context menus). The companion docs confirm only menu bars, pull-downs, menu organization, mnemonics, shortcuts, and access paths (Part 14), and action bars, pull-downs, context menus, and dialog boxes (Part 161).
- **ANSI/HFES 200:** which parts cover keyboard access and message wording.
- **IEEE P1201.2:** scope as described in the companion docs (mouse buttons, mnemonics, menu structures, accelerators); never approved, so cite only as a draft project.
- **CUA Undo binding (Alt+Backspace)** and **Shift+F10** for pop-up menus: confirm in the CUA '91 publications.
- **Platform specifics:** dialog button order, Redo shortcuts, Preferences locations, and GNOME's header-bar guidance change over time; check the current Microsoft, Apple, GNOME, and KDE guidelines.

---

**Last Updated:** 2026-10-20 0506 CDT
