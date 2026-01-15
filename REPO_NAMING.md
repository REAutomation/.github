# Repository Naming Convention

This document defines the naming standards for all repositories in the REAutomation organization.

## General Format

```
[category]-[name]-[stack]
```

All names must be:
- **lowercase**
- **English**
- Using **hyphens** as separators (no underscores)

---

## 1. Global Repositories (Project-Independent)

### Libraries
```
lib-[name]-[stack]
```

Examples:
- `lib-base-codesys` - Base library for CODESYS
- `lib-motion-codesys` - Motion control library
- `lib-comms-codesys` - Communication protocols
- `lib-cbml-extensions-codesys` - CBML extensions

### Templates
```
template-[name]-[stack]
```

Examples:
- `template-machine-codesys` - Standard machine template
- `template-hmi-web` - Web HMI starter template
- `template-project-tia` - TIA Portal template

### Internal Tools & Documentation
```
internal-[name]
```

Examples:
- `internal-tools` - Helper scripts and utilities
- `internal-docs` - Company documentation
- `internal-snippets` - Code snippets collection

---

## 2. Customer Projects

### Format
```
[customer]-[projectnr]-[part]-[stack]
```

| Segment | Description | Example |
|---------|-------------|---------|
| `customer` | Customer short name | `mahle`, `blumer`, `siemens` |
| `projectnr` | Project number (YYNNN) | `24001`, `25012` |
| `part` | Machine part / module | `main`, `feeder`, `gantry` |
| `stack` | Technology stack | `codesys`, `tia`, `hmi-web` |

### Examples

```
mahle-24001-main-codesys       # Main PLC
mahle-24001-feeder-codesys     # Feeder unit
mahle-24001-handling-codesys   # Handling system
mahle-24001-safety-tia         # Safety PLC (TIA)
mahle-24001-hmi-web            # Web HMI (no part needed)
mahle-24001-backend            # Backend server

blumer-24002-gantry-codesys
blumer-24002-hmi-web
```

### When Part is Not Needed

For single-instance components, omit the part:
```
mahle-24001-hmi-web      # Only one HMI per project
mahle-24001-backend      # Only one backend per project
mahle-24001-docs         # Project documentation
```

---

## 3. Technology Stack Suffixes

| Suffix | Technology | Description |
|--------|------------|-------------|
| `-codesys` | CODESYS V3.5 | PLC programming |
| `-tia` | TIA Portal | Siemens PLC |
| `-hmi-web` | React/Web | Web-based visualization |
| `-hmi-codesys` | CODESYS Visu | CODESYS visualization |
| `-backend` | Node.js | Server / API |
| `-docs` | Markdown | Documentation |
| `-config` | Various | Configuration files |

---

## 4. Topics (Tags)

Add these topics to repositories for better filtering:

### By Customer
`customer-mahle`, `customer-blumer`, `customer-siemens`

### By Technology
`codesys`, `tia`, `react`, `nodejs`, `plc`

### By Type
`library`, `template`, `project`, `documentation`

### By Year
`2024`, `2025`

---

## 5. Examples Overview

| Repository | Type | Customer | Project | Part | Stack |
|------------|------|----------|---------|------|-------|
| `lib-motion-codesys` | Library | - | - | - | CODESYS |
| `template-machine-codesys` | Template | - | - | - | CODESYS |
| `internal-tools` | Internal | - | - | - | - |
| `mahle-24001-main-codesys` | Project | Mahle | 24001 | Main | CODESYS |
| `mahle-24001-hmi-web` | Project | Mahle | 24001 | - | Web HMI |
| `blumer-24002-gantry-codesys` | Project | Blumer | 24002 | Gantry | CODESYS |

---

## 6. Creating a New Repository

1. Determine the category (lib / template / internal / customer project)
2. Follow the naming pattern above
3. Add appropriate topics
4. Use a template if available
5. Set visibility to **Private** (unless open source library)
