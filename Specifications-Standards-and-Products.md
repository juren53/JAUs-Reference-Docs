# Specifications, Standards, and Products

Definitions and comparisons for three terms that are often confused: **specification**, **standard**, and **product**. These definitions are used throughout JAUs-Reference-Docs. For an applied example, see [Application-UI-Standards-and-Specs.md](Application-UI-Standards-and-Specs.md).

## Contents

1. Core Definitions & Comparisons
2. Common Misuse of the Term "Standard"
3. Key Articles and Engineering Frameworks
4. Practical Example: The Pull-Down Menu Evolution

---

## 1. Core Definitions & Comparisons

| Category | Specification | Standard | Product |
|----------|---------------|----------|---------|
| **Primary Goal** | Defines how a specific system, component, or interface must be constructed or perform. | Establishes broad consensus, uniformity, and interoperability across an industry. | Implements or realizes a specification or standard as a usable software or hardware deliverable. |
| **Authority / Governance** | Created by an individual company, engineering team, project, vendor, consortium, or working group. | Published only by a recognized standards body (ISO, IEC, IEEE, ANSI, etc.) through a formal consensus process. | Produced by a manufacturer, software developer, or vendor for end-user adoption. |
| **Flexibility** | High; tied to a specific project, revision cycle, or product release. The owner can change it at will. | Low to moderate; changes require a formal multi-stakeholder consensus process. | Fixed for any one release; changes arrive as new builds or versions. |
| **Examples** | IBM CUA, OpenAPI Specification, Microsoft Windows App Design guidelines | ISO 9241, IEEE 802.11, ANSI/HFES 200 | Microsoft Word, Linux OS, Dwarf Mini Smart Telescope |

**Short version:**

- A **standard** says what *an industry* agrees on.
- A **specification** says what *one organization* has decided.
- A **product** is *the thing that gets built*, which may follow one or more standards and specifications.

## 2. Common Misuse of the Term "Standard"

Products and company specifications are often wrongly called standards. Some typical examples:

| Commonly heard | What it actually is |
|----------------|---------------------|
| "The CUA standard" | IBM's **specification** |
| "Word is the standard for documents" | Microsoft Word is a **product**. Its file format (.docx / Office Open XML) began as a Microsoft **specification** and was later published as **standards** (ECMA-376, ISO/IEC 29500). |
| "Windows is the industry standard" | Windows is a **product**. Microsoft's UI guidelines for it are a **specification**. |
| "A de facto standard" | A widely adopted **specification** or **product**. Wide use does not make something a standard. |

**Why it matters:** A standard is controlled by a neutral body through an open process. A specification or product is controlled by its owner, who can change it, license it, or drop it at any time. Calling a product or spec a "standard" hides that dependency.

**Gray areas:**

- **Consortia** such as W3C, OASIS, and the OpenAPI Initiative use formal, multi-member processes and are often counted as standards organizations. Under the strict definition used here, their documents are **specifications** unless they are also adopted by a formal standards body. For example, W3C's HTML specifications are specifications in these docs.
- **Path to a standard:** A specification can *become* a standard when a standards body adopts it. Office Open XML (Microsoft → ECMA-376 → ISO/IEC 29500) and PDF (Adobe → ISO 32000) are examples. After adoption, the standards-body version is the standard; the original company document is still a specification.

## 3. Key Articles and Engineering Frameworks

Systems engineering and software engineering literature often analyzes these distinctions.

### 3.1 The Systems Engineering Framework: Requirements vs. Specifications vs. Standards

In systems engineering literature (e.g., the Defense Acquisition University's *Systems Engineering Fundamentals*), the distinction centers on how stakeholder intent becomes a design:

- **Requirements** state *what* stakeholders need. Example: "The user must be able to open a file via a menu."
- **Specifications** define *how* the system will technically meet those requirements. Example: "The application menu bar shall implement a pull-down menu under Alt+F containing an 'Open...' item."
- **Standards** provide the baseline rules and constraints that keep specifications consistent across an industry.

### 3.2 ISO/IEC Directives & Technical Committee Guidelines

The ISO/IEC Directives define several deliverable types. Some of them are formally called *specifications* even though ISO publishes them, because they have not reached full consensus:

- **International Standard (IS):** Has full international consensus among member bodies. This is the only deliverable that is a full standard.
- **Technical Specification (TS):** Published when a technology is still developing, or when full consensus for a standard can't yet be reached. It may later become an International Standard.
- **Publicly Available Specification (PAS):** Published quickly to meet an immediate market need, with consensus only within the working group.
- **Technical Report (TR):** Informative material, such as data or surveys, that doesn't specify requirements.

> This means the standards bodies themselves draw the same line used in these docs: a document is not a standard until it achieves formal consensus.

### 3.3 Software Engineering Institute (SEI / Carnegie Mellon): *Software Specification: A Framework*

This SEI curriculum module explores specifications as intermediate artifacts. A specification sits between abstract standards and requirements on one side and concrete software products on the other. It works as a formal "blueprint" that can be verified, and checked for compliance, before or during product development.

## 4. Practical Example: The Pull-Down Menu Evolution

Applying these definitions to the desktop pull-down menu shows how the three levels fit together:

| Level | Example | Role |
|-------|---------|------|
| **Specification** | IBM **Common User Access (CUA)** (1987) | IBM's own technical blueprint for how its applications' menus, keyboard, and dialogs should work |
| **Standard** | **ISO 9241-14** (1997) | Formalized menu dialogues as an official international standard |
| **Product** | **Microsoft Word**, **GIMP**, **Lotus 1-2-3** | Actual software applications whose menus implement those conventions in code |

The order matters: the specification came first (1987), the standard formalized many of the same ideas a decade later (1997), and products implemented the conventions throughout. Products followed CUA long before ISO 9241-14 existed, so for that period they were following a **specification**, not a standard.

See [Application-UI-Standards-and-Specs.md](Application-UI-Standards-and-Specs.md) for the full details of CUA and ISO 9241.
