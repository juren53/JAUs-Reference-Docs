# Application UI Standards and Specifications

Reference notes on the formal **standards** and company **specifications** that define how desktop application menus, dialogs, and keyboard interaction should behave.

## Contents

- Terminology: Standards vs. Specifications
- **Part A — Standards**
  1. ISO Standards for Pull-Down Menus & GUIs
  2. ANSI and IEEE
- **Part B — Specifications**
  3. IBM Common User Access (CUA)
  4. Current Platform Specifications
- **Part C — Comparison**
  5. How the Standards and Specifications Relate

---

## Terminology: Standards vs. Specifications

This document keeps a strict distinction between two kinds of documents:

| | **Standard** | **Specification (spec)** |
|---|---|---|
| **Created by** | A recognized standards body (ISO, IEC, ANSI, IEEE, etc.) | A company or other non-standards organization (IBM, Microsoft, Apple, OSF, GNOME, KDE, etc.) |
| **Process** | Open, consensus-based development with public review and formal balloting | Decided internally by the organization that owns it |
| **Purpose** | Neutral requirements that anyone can adopt | Defines how *that organization's* products, or products built for its platform, should behave |
| **Changed by** | The standards body, through formal revision | The owner, at its discretion |

**Why the distinction matters:** A company's specification is often called a "standard" just because it is widely used, for example "the CUA standard" or "the Windows standard." Being widely used does not make something a standard. It stays a specification controlled by one company, which can change or drop it at any time. In this document:

- **Standard** is used *only* for documents published by a recognized standards body.
- **Specification** (or **spec**) is used for everything published by a company or other non-standards organization, however influential.
- The loose phrase "de facto standard" is avoided. A widely adopted spec is called a **widely adopted specification**.

---

# Part A — Standards

## 1. ISO Standards for Pull-Down Menus & GUIs

### 1.1 ISO 9241-14: *Ergonomic requirements for office work with visual display terminals — Part 14: Menu dialogues* (1997)

This is the primary formal standard for menu systems. It explicitly defined recommendations for:

- **Pull-down and drop-down menus:** Mechanics for expanding options upon selection.

- **Menu Organization & Hierarchy:** How top-level menu bars should group application commands logically (including rules for positioning system, editing, and help functions).

- **Keyboard Navigation:** Requirements for mnemonics (underlined hotkeys), keyboard shortcuts, and access paths.

### 1.2 ISO/IEC 15910 & ISO/IEC 11581 Series

- **ISO/IEC 15910 (*Information technology — Software user documentation process*):** Covers the process for producing user documentation (help, manuals) that accompanies an interface.

- **ISO/IEC 11581 (*Information technology — User interface symbols and functions*):** Standardized common desktop icons and visual controls used within menus and toolbars.

### 1.3 ISO 9241-161: *Guidance on visual user interface elements*

As part of the restructured ISO 9241 series, **Part 161** specifically details visual control elements—including action bars, pull-down menus, context menus, and dialog boxes—to ensure consistency across software implementations.

## 2. ANSI and IEEE

- **ANSI:** Rather than creating a competing standard, ANSI adopted ISO 9241 in the United States via **ANSI/HFES 200** (*Human Factors Engineering of Software User Interfaces*), which incorporated ISO's menu dialogue requirements for American software development.

- **IEEE:** IEEE primarily focused on lower-level POSIX standard interfaces and software engineering documentation standards, though IEEE working groups routinely referenced ISO 9241 for software human-factors compliance.

---

# Part B — Specifications

## 3. IBM Common User Access (CUA)

### 3.1 Overview

**Common User Access (CUA)** is IBM's user interface **specification**, introduced in **1987** as part of **Systems Application Architecture (SAA)**. Its goal was to make every IBM application, whether on a 3270 terminal, a PC under DOS, or OS/2 Presentation Manager, look and behave the same way, so users could move between programs without retraining.

CUA is a company specification, not a standard: IBM wrote it, owned it, and revised it on its own. It is often called "the CUA standard," but that is incorrect. It was, however, a **widely adopted specification**: many of its conventions are still used in Windows, Linux desktops, and Qt/GTK applications today.

### 3.2 Key Publications

| Year | Title | IBM Pub. No. |
|------|-------|--------------|
| 1987 | *SAA Common User Access: Panel Design and User Interaction* | SC26-4351 |
| 1989 | *SAA CUA Basic Interface Design Guide* | SC26-4583 |
| 1989 | *SAA CUA Advanced Interface Design Guide* | SC26-4582 |
| 1991–92 | *Object-Oriented Interface Design: IBM Common User Access Guidelines* (CUA '91) | SC34-4399 |

### 3.3 Interface Models

CUA defined levels of capability so that the same principles could apply on very different hardware:

- **Entry Model:** Basic panel-based interaction for non-programmable (3270) terminals.
- **Text Subset of the Graphical Model:** Character-mode PC applications using action bars and pull-downs (e.g., DOS full-screen programs).
- **Graphical Model:** Full windowed GUI with mouse support (OS/2 Presentation Manager, Windows).
- **Workplace Model (CUA '91):** An object-oriented desktop where users act on objects such as folders and documents. It was implemented in the OS/2 2.0 Workplace Shell.

### 3.4 Core Conventions

**Menu bar ("action bar") and pull-downs**

- A horizontal menu bar along the top of the window, with pull-down menus beneath each item.
- Standard ordering: **File, Edit, View, Options, Help** (application-specific menus go between View and Help; **Help is always last**).
- Standard File items: New, Open..., Save, Save as..., Print..., Exit.
- An **ellipsis (`...`)** after a menu item means it opens a dialog that needs more input before the action runs.
- Unavailable choices are shown **grayed out** (disabled), not removed, so the menu layout stays the same.
- Every menu item has a **mnemonic**, an underlined letter that selects it from the keyboard.

**Keyboard model**

| Key | CUA Function |
|-----|--------------|
| F1 | Help (context-sensitive) |
| F10 / Alt | Move focus to / from the menu bar |
| Alt + mnemonic | Open a menu directly |
| Enter | Activate the default push button |
| Esc | Cancel / back out one level |
| Tab / Shift+Tab | Move between fields and controls |
| Arrow keys | Move within a group or list |
| Shift+Del | Cut |
| Ctrl+Ins | Copy |
| Shift+Ins | Paste |
| Del | Clear / delete selection |
| F3 | Exit (text-mode applications) |
| F12 | Cancel (text-mode applications) |

> The **Shift+Del / Ctrl+Ins / Shift+Ins** clipboard keys are the original CUA bindings. **Ctrl+X / Ctrl+C / Ctrl+V** came from Apple's Macintosh specification and were later added alongside them in Windows. Most Windows applications still accept both sets.

**Dialogs and controls**

- Standard push buttons: **OK, Cancel, Help**, placed consistently in every dialog.
- Consistent controls: entry fields, radio buttons (mutually exclusive), check boxes (independent), list boxes, and combination boxes.
- Selection follows an **object–action** model: the user selects an object first, then picks an action from the menu.

**Feedback and messages**

- Messages are classed as **Information**, **Warning**, or **Action/Error**, each with its own icon.
- Every message tells the user what happened and what to do next.

### 3.5 Influence and Legacy

- **Microsoft Windows 3.x** and **OS/2 Presentation Manager** were both designed to follow CUA (Microsoft co-developed OS/2 with IBM).
- The **OSF/Motif Style Guide**, a specification from the Open Software Foundation (an industry consortium, not a standards body), was based on CUA. It carried CUA conventions into X11/Linux desktops.
- Text-mode frameworks such as **Borland Turbo Vision** and the **MS-DOS Editor** used the CUA text-subset model.
- Many CUA concepts, including action bars, pull-downs, mnemonics, and grayed-out choices, later appear in the formal standard **ISO 9241-14**. This is the normal path: ideas proven in a company spec are later formalized by a standards body.
- Modern toolkits (**Qt/PyQt**, **GTK**, **WinForms/WPF**) still follow CUA conventions by default: `&File` mnemonics, `...` on dialog-opening items, File-first/Help-last menu order, F1 for help.

## 4. Current Platform Specifications

These are the working references for desktop UI today. All of them are **specifications**, maintained by the company or project that owns the platform, not by a standards body:

| Specification | Owner | Platform |
|---------------|-------|----------|
| Windows App Design guidelines (formerly *Windows User Experience Interaction Guidelines*) | Microsoft | Windows |
| Human Interface Guidelines (HIG) | Apple | macOS, iOS |
| GNOME Human Interface Guidelines | GNOME Project | GNOME / GTK desktops |
| KDE Human Interface Guidelines | KDE | KDE Plasma / Qt desktops |

---

# Part C — Comparison

## 5. How the Standards and Specifications Relate

| Document | Kind | Issued by | Scope | Status |
|----------|------|-----------|-------|--------|
| ISO 9241-14 (1997) | **Standard** | ISO | Menu dialogues | Formal reference for menu design |
| ISO 9241-161 | **Standard** | ISO | Visual UI elements (menus, dialogs, controls) | Current part of the restructured 9241 series |
| ISO/IEC 11581 | **Standard** | ISO/IEC | UI icons and symbols | Current |
| ANSI/HFES 200 | **Standard** | ANSI (developed by HFES) | U.S. adoption of ISO 9241 software ergonomics | Current |
| IBM CUA (1987–1992) | **Specification** | IBM | Full interaction model: menus, keyboard, dialogs, messages | Historical; conventions live on in modern toolkits |
| OSF/Motif Style Guide | **Specification** | Open Software Foundation | X11/Unix desktop UI, based on CUA | Historical |
| Windows, Apple, GNOME, KDE guidelines | **Specification** | Microsoft, Apple, GNOME, KDE | Their own platforms | Current |

**In practice:** IBM's CUA specification came first and set the conventions. ISO 9241 later formalized many of them as an international standard. Today's working references are the platform specifications from Microsoft, Apple, GNOME, and KDE, and all of them still reflect CUA ideas. When this document says an application "follows the standard," it means ISO 9241. When an application follows Windows or CUA conventions, it is following a **specification**.
