<div align="center">

# 🍎 Panduan Setup Claude Code CLI untuk Pengembangan iOS

**Workflow berbasis PRD, extended thinking (`ultrathink`), dan Plan Mode untuk Swift / SwiftUI.**

[![Claude Code](https://img.shields.io/badge/Claude_Code-2.0.x-D97757?logo=anthropic&logoColor=white)](https://code.claude.com/docs)
[![Swift](https://img.shields.io/badge/Swift-6.0-F05138?logo=swift&logoColor=white)](https://www.swift.org)
[![iOS](https://img.shields.io/badge/iOS-17%2B-000000?logo=apple&logoColor=white)](https://developer.apple.com/ios/)
[![GitHub](https://img.shields.io/badge/GitHub-%40alvinzanuaputra-181717?logo=github&logoColor=white)](https://github.com/alvinzanuaputra)
[![Website](https://img.shields.io/badge/Website-alvinzanua.site-4F46E5?logo=googlechrome&logoColor=white)](https://www.alvinzanua.site)

**[Alvin Zanua Putra](https://github.com/alvinzanuaputra)** ([@alvinzanuaputra](https://github.com/alvinzanuaputra)) · Web Developer · [www.alvinzanua.site](https://www.alvinzanua.site)

🌐 **Bahasa:** [English](README-EN.md) · **Bahasa Indonesia**

</div>

---

## Tentang Panduan Ini

Panduan praktis yang mudah di-copy-paste untuk menyiapkan **Claude Code CLI** pada pengembangan iOS native. Isinya mencakup semuanya, mulai dari instalasi sampai workflow berbasis spesifikasi (spec-driven), sehingga kamu bisa berpindah dari repo kosong ke setup yang produktif dengan bantuan AI dengan cepat.

**Yang akan kamu siapkan:**

- 🧠 Konteks proyek `CLAUDE.md` untuk Swift 6 / SwiftUI / MVVM
- 📄 Workflow PRD → Spec → Tasks yang digerakkan oleh custom slash command
- 🔌 XcodeBuildMCP untuk build, test, dan menjalankan app di simulator
- 🤖 Subagent, Agent Skills, hooks, output style, dan plugin
- 🛡️ Level izin bertingkat (sandbox mode) untuk pengembangan yang aman

**Untuk siapa panduan ini?**

- Developer iOS (Swift / SwiftUI) yang menginginkan workflow terstruktur dan berulang dengan Claude Code
- Tim yang ingin konfigurasi `.claude/` bersama yang di-version-control
- Developer dengan stack lain — strukturnya bisa diadopsi; cukup ganti bagian khusus Swift dan XcodeBuildMCP

## Prasyarat

| Kebutuhan | Catatan |
|-------------|-------|
| **macOS** | Wajib untuk Xcode dan iOS Simulator |
| **Xcode 16+** | Diperlukan untuk Swift 6 |
| **Node.js 18+** | Hanya untuk MCP server berbasis `npx` seperti XcodeBuildMCP |
| **Akun Claude atau API key** | Langganan Claude (OAuth) atau `ANTHROPIC_API_KEY` |
| **Git** | Untuk konfigurasi proyek/tim yang dibagikan |
| *(Opsional)* **SwiftLint / swift-format** | Dipakai oleh hooks di [Bagian 15](#15-hooks-untuk-pengembangan-swift) |

## Mulai Cepat

- [ ] Install Claude Code CLI (cara native) → [Bagian 1](#1-instalasi)
- [ ] Autentikasi dengan akun Claude → [Bagian 1](#1-instalasi)
- [ ] Install XcodeBuildMCP → [Bagian 7](#7-integrasi-xcodebuildmcp)
- [ ] Buat struktur direktori `.claude/` proyek → [Bagian 16](#16-struktur-proyek-lengkap)
- [ ] Buat `CLAUDE.md` di root dengan konteks proyek → [Bagian 3](#3-setup-claudemd-untuk-proyek-ios)
- [ ] Buat `.mcp.json` dengan konfigurasi XcodeBuildMCP → [Bagian 7](#7-integrasi-xcodebuildmcp)
- [ ] Buat slash command esensial → [Bagian 8](#8-custom-slash-command-untuk-ios)
- [ ] Siapkan hooks untuk linting Swift → [Bagian 15](#15-hooks-untuk-pengembangan-swift)
- [ ] Buat struktur `docs/` untuk workflow PRD → [Bagian 4](#4-workflow-pengembangan-berbasis-prd)
- [ ] Tulis PRD awal → [Bagian 4](#4-workflow-pengembangan-berbasis-prd)
- [ ] Atur izin di `settings.json` → [Bagian 14](#14-settings--permissions)
- [ ] Tes perintah build: `/build`

---

## Daftar Isi

1. [Instalasi](#1-instalasi)
2. [Hierarki Konfigurasi](#2-hierarki-konfigurasi)
3. [Setup CLAUDE.md untuk Proyek iOS](#3-setup-claudemd-untuk-proyek-ios)
4. [Workflow Pengembangan Berbasis PRD](#4-workflow-pengembangan-berbasis-prd)
5. [Extended Thinking & Ultrathink](#5-extended-thinking--ultrathink)
6. [Konfigurasi Plan Mode](#6-konfigurasi-plan-mode)
7. [Integrasi XcodeBuildMCP](#7-integrasi-xcodebuildmcp)
8. [Custom Slash Command untuk iOS](#8-custom-slash-command-untuk-ios)
9. [Agent Skills](#9-agent-skills)
10. [Konfigurasi Subagent](#10-konfigurasi-subagent)
11. [Output Style](#11-output-style)
12. [Sistem Plugin](#12-sistem-plugin)
13. [Sandbox Mode & Pengembangan Aman](#13-sandbox-mode--pengembangan-aman)
14. [Settings & Permissions](#14-settings--permissions)
15. [Hooks untuk Pengembangan Swift](#15-hooks-untuk-pengembangan-swift)
16. [Struktur Proyek Lengkap](#16-struktur-proyek-lengkap)
17. [Best Practice & Tips](#17-best-practice--tips)

**Lainnya:** [Referensi](#referensi) · [Tentang Penulis](#tentang-penulis) · [Kontribusi](#kontribusi)

---

## 1. Instalasi

### Instalasi Native (Direkomendasikan)

```bash
# macOS/Linux - Native installer (no Node.js required)
curl -fsSL https://claude.ai/install.sh | bash

# Or install latest version
curl -fsSL https://claude.ai/install.sh | bash -s latest
```

### Instalasi via NPM (Alternatif)

```bash
# Global npm install (do NOT use sudo)
npm install -g @anthropic-ai/claude-code

# Migrate existing npm install to native
claude install
```

### Autentikasi

```bash
# Start Claude Code and authenticate via OAuth
claude

# Or set API key environment variable
export ANTHROPIC_API_KEY="your-key-here"
```

### Pemilihan Model

```bash
# Use a model alias at startup (always resolves to the latest version)
claude --model opus
claude --model sonnet
claude --model haiku

# Or set a default model
export ANTHROPIC_MODEL="sonnet"
```

> **Tip**: Alias (`opus`, `sonnet`, `haiku`) selalu menunjuk ke model terbaru di tier-nya.
> Jika butuh perilaku yang konsisten (misalnya di CI), pin ID model lengkap — lihat
> [models overview](https://docs.claude.com/en/docs/about-claude/models/overview) untuk ID terbaru.

---

## 2. Hierarki Konfigurasi

Claude Code memakai sistem konfigurasi berlapis, di mana tiap level dapat menimpa level di bawahnya:

```
Priority (Highest to Lowest):
├── 1. Managed / enterprise policy settings (if set by your organization)
├── 2. Command-line flags (--model, --permission-mode)
├── 3. .claude/settings.local.json (local - personal, gitignored)
├── 4. .claude/settings.json (project - shared with team)
└── 5. ~/.claude/settings.json (user - global)
```

> **Catatan**: Khusus untuk model, urutannya: `/model` dalam sesi → flag `--model` →
> env var `ANTHROPIC_MODEL` → `model` di file settings.

### Scope Konfigurasi

| Scope | Deskripsi | Lokasi |
|-------|-------------|---------|
| **local** | Hanya untuk kamu di proyek saat ini (default untuk MCP) | `.claude/settings.local.json` |
| **project** | Dibagikan ke tim lewat git | `.claude/settings.json`, `.mcp.json` |
| **user** | Tersedia di semua proyekmu | `~/.claude/settings.json` |

### File Konfigurasi Utama

| File | Scope | Status Git | Fungsi |
|------|-------|------------|---------|
| `~/.claude/settings.json` | User | N/A | Preferensi global |
| `~/.claude/CLAUDE.md` | User | N/A | Instruksi global |
| `~/.claude/skills/` | User | N/A | Agent Skills pribadi |
| `~/.claude/commands/` | User | N/A | Slash command pribadi |
| `.claude/settings.json` | Project | Di-commit | Settings tim |
| `.claude/settings.local.json` | Local | Di-gitignore | Override pribadi |
| `.mcp.json` | Project | Di-commit | MCP server bersama |
| `CLAUDE.md` | Project | Di-commit | Konteks utama proyek |
| `.claude/skills/` | Project | Di-commit | Agent Skills proyek |
| `.claude/commands/` | Project | Di-commit | Slash command proyek |
| `.claude/agents/` | Project | Di-commit | Subagent proyek |

---

## 3. Setup CLAUDE.md untuk Proyek iOS

File `CLAUDE.md` adalah penyedia konteks utama. Claude memuatnya otomatis saat sesi dimulai.

### Template CLAUDE.md Root untuk iOS

Buat `CLAUDE.md` di root proyekmu:

````markdown
# Project: [Your App Name]

## Quick Reference
- **Platform**: iOS 17+ / macOS 14+
- **Language**: Swift 6.0
- **UI Framework**: SwiftUI
- **Architecture**: MVVM with @Observable
- **Minimum Deployment**: iOS 17.0
- **Package Manager**: Swift Package Manager

## XcodeBuildMCP Integration
**IMPORTANT**: This project uses XcodeBuildMCP for all Xcode operations.
- Build: `mcp__xcodebuildmcp__build_sim_name_proj`
- Test: `mcp__xcodebuildmcp__test_sim_name_proj`
- Clean: `mcp__xcodebuildmcp__clean`

## Project Structure
```
MyApp/
├── App/                    # App entry point, App delegate
├── Features/               # Feature modules
│   ├── [FeatureName]/
│   │   ├── Views/          # SwiftUI views
│   │   ├── ViewModels/     # @Observable classes
│   │   └── Models/         # Data models
├── Core/                   # Shared utilities
│   ├── Extensions/
│   ├── Services/
│   └── Networking/
├── Resources/              # Assets, Localizations
└── Tests/
```

## Coding Standards

### Swift Style
- Use Swift 6 strict concurrency
- Prefer `@Observable` over `ObservableObject`
- Use `async/await` for all async operations
- Follow Apple's Swift API Design Guidelines
- Use `guard` for early exits
- Prefer value types (structs) over reference types (classes)

### SwiftUI Patterns
- Extract views when they exceed 100 lines
- Use `@State` for local view state only
- Use `@Environment` for dependency injection
- Prefer `NavigationStack` over deprecated `NavigationView`
- Use `@Bindable` for bindings to @Observable objects

### Navigation Pattern
```swift
// Use NavigationStack with type-safe routing
enum Route: Hashable {
    case detail(Item)
    case settings
}

NavigationStack(path: $router.path) {
    ContentView()
        .navigationDestination(for: Route.self) { route in
            // Handle routing
        }
}
```

### Error Handling
```swift
// Always use typed errors
enum AppError: LocalizedError {
    case networkError(underlying: Error)
    case validationError(message: String)
    
    var errorDescription: String? {
        switch self {
        case .networkError(let error): return error.localizedDescription
        case .validationError(let msg): return msg
        }
    }
}
```

## Testing Requirements
- Unit tests for all ViewModels
- UI tests for critical user flows
- Use Swift Testing framework (`@Test`, `#expect`)
- Minimum 80% code coverage for business logic

## DO NOT
- Write UITests during scaffolding phase
- Use deprecated APIs (UIKit when SwiftUI suffices)
- Create massive monolithic views
- Use force unwrapping (`!`) without justification
- Ignore Swift 6 concurrency warnings

## Planning Workflow
When starting new features:
1. Read the PRD from `docs/PRD.md`
2. Create feature spec in `docs/specs/[feature-name].md`
3. Use `ultrathink` for architectural decisions
4. Use Plan Mode (`Shift+Tab`) for implementation strategy
5. Implement incrementally with tests

## Memory Imports
@docs/PRD.md
@docs/ARCHITECTURE.md
@docs/ROADMAP.md
````

### CLAUDE.md Bertingkat untuk Direktori Fitur

Buat `.claude/CLAUDE.md` atau `Features/[FeatureName]/CLAUDE.md`:

````markdown
# [Feature Name] Module

## Purpose
[Description of what this feature does]

## Architecture
- Uses MVVM with @Observable ViewModels
- Parent stores create child stores for modal presentations

## Navigation Pattern
### Sheet-Based Navigation
**Pattern**: Parent stores create optional child stores for modal presentations
**Rules**:
1. Parent ViewModel (an `@Observable` class) holds `var childViewModel: ChildViewModel?`
2. View observes and presents sheet when non-nil
3. Dismissal sets childViewModel to nil

### Example
```swift
@Observable
final class ParentViewModel {
    var detailViewModel: DetailViewModel?
    
    func showDetail(for item: Item) {
        detailViewModel = DetailViewModel(item: item)
    }
}
```

## Testing
Run tests: `mcp__xcodebuildmcp__swift_package_test`
````

---

## 4. Workflow Pengembangan Berbasis PRD

### Struktur Direktori untuk Workflow PRD

```
docs/
├── PRD.md                      # Main Product Requirements Document
├── ARCHITECTURE.md             # System architecture decisions
├── ROADMAP.md                  # Development roadmap & priorities
├── specs/                      # Feature specifications
│   ├── 000-project-setup.md
│   ├── 001-authentication.md
│   ├── 002-dashboard.md
│   └── template.md
└── tasks/                      # Task breakdowns
    ├── 000-sample.md
    └── [feature]-tasks.md
```

### Template PRD (`docs/PRD.md`)

```markdown
# Product Requirements Document: [App Name]

## Executive Summary
[Brief description of the product and its primary value proposition]

## Problem Statement
[What problem does this solve? Who experiences this problem?]

## Target Users
- **Primary**: [Description]
- **Secondary**: [Description]

## Success Metrics
| Metric | Target | Measurement |
|--------|--------|-------------|
| User Retention | 40% D7 | Analytics |
| App Rating | 4.5+ | App Store |
| Crash-Free Rate | 99.5% | Crashlytics |

## Core Features

### Feature 1: [Name]
**Priority**: P0 (Must Have)
**Description**: [Detailed description]
**User Stories**:
- As a [user type], I want [action] so that [benefit]

**Acceptance Criteria**:
- [ ] Criterion 1
- [ ] Criterion 2

**Technical Requirements**:
- iOS 17+ required
- Offline support needed
- Data persistence via SwiftData

### Feature 2: [Name]
[Continue pattern...]

## Non-Functional Requirements
- **Performance**: App launch < 2s, smooth 60fps scrolling
- **Accessibility**: WCAG 2.1 AA compliance
- **Localization**: English (primary), [other languages]
- **Security**: Keychain for credentials, certificate pinning

## Out of Scope (v1.0)
- [Feature explicitly not included]

## Technical Constraints
- Swift 6.0+ with strict concurrency
- SwiftUI-only (no UIKit unless necessary)
- SwiftData for persistence
- Minimum iOS 17.0

## Timeline
| Phase | Duration | Deliverables |
|-------|----------|--------------|
| Design | 2 weeks | Figma mockups |
| Development | 8 weeks | MVP features |
| Testing | 2 weeks | QA sign-off |
| Launch | 1 week | App Store submission |
```

### Template Feature Spec (`docs/specs/template.md`)

````markdown
# Feature Specification: [Feature Name]

**Status**: Draft | In Review | Approved | In Progress | Complete
**Priority**: P0 | P1 | P2
**PRD Reference**: Section [X]
**Author**: [Name]
**Last Updated**: [Date]

## Overview
[Brief description of the feature]

## User Stories
1. As a [user], I want [action] so that [benefit]
2. ...

## Acceptance Criteria
- [ ] AC1: [Specific, testable criterion]
- [ ] AC2: ...

## Technical Design

### Architecture
[How this feature fits into the overall architecture]

### Data Models
```swift
struct FeatureModel: Codable, Identifiable {
    let id: UUID
    // ...
}
```

### API Endpoints (if applicable)
```
GET /api/v1/feature
POST /api/v1/feature
```

### Dependencies
- [ ] Core networking module
- [ ] SwiftData setup

## UI/UX Design
- Figma Link: [URL]
- Key screens: [List]

## Edge Cases
1. [Edge case and how to handle]

## Testing Plan
- Unit tests for ViewModel logic
- UI tests for critical flows
- Performance tests for data loading

## Rollout Plan
- [ ] Feature flag: `feature_[name]_enabled`
- [ ] A/B test configuration

## Open Questions
- [ ] Question 1?
````

### Template File Task (`docs/tasks/feature-tasks.md`)

```markdown
# Tasks: [Feature Name]

**Feature Spec**: `docs/specs/[feature].md`
**Status**: Not Started | In Progress | Complete

## Progress Summary
- Total Steps: X
- Completed: Y
- Current: Step Z

## Steps

### Step 1: [Task Name]
- [ ] Subtask 1
- [ ] Subtask 2
**Notes**: [Implementation notes]

### Step 2: [Task Name]
- [ ] Subtask 1
**Notes**: 

## Changes Log
| Date | Step | Changes |
|------|------|---------|
| YYYY-MM-DD | 1 | Initial implementation |
```

### Slash Command untuk Workflow PRD

Buat `.claude/commands/` dengan file-file berikut:

**`.claude/commands/create-prd.md`**
```markdown
---
description: Create a new PRD from requirements discussion
allowed-tools: Read, Write, Edit
---

# Create Product Requirements Document

Based on our discussion, create a comprehensive PRD in `docs/PRD.md`.

Follow this structure:
1. Executive Summary
2. Problem Statement
3. Target Users
4. Success Metrics
5. Core Features with user stories and acceptance criteria
6. Non-Functional Requirements
7. Technical Constraints
8. Timeline

Ask clarifying questions before writing. Use ultrathink for comprehensive planning.
```

**`.claude/commands/generate-spec.md`**
```markdown
---
description: Generate feature specification from PRD
argument-hint: <feature-name>
allowed-tools: Read, Write
---

# Generate Feature Specification

Read the PRD at `docs/PRD.md` and create a detailed specification for: $ARGUMENTS

1. Extract relevant user stories and requirements
2. Define acceptance criteria
3. Design technical architecture
4. Create the spec file at `docs/specs/$ARGUMENTS.md`

ultrathink about the technical design before writing.
```

**`.claude/commands/generate-tasks.md`**
```markdown
---
description: Break down feature spec into implementable tasks
argument-hint: <feature-name>
allowed-tools: Read, Write
---

# Generate Task Breakdown

Read the specification at `docs/specs/$ARGUMENTS.md` and create a detailed task breakdown.

1. Identify all implementation steps
2. Order by dependencies
3. Estimate complexity
4. Create `docs/tasks/$ARGUMENTS-tasks.md`

Each task should be completable in a single session.
```

---

## 5. Extended Thinking & Ultrathink

Extended thinking **nonaktif secara default** di Claude Code. Kamu bisa mengaktifkannya sesuai kebutuhan.

### Mengaktifkan Extended Thinking

| Metode | Deskripsi |
|--------|-------------|
| **Tombol Tab** | Tekan `Tab` untuk menyalakan/mematikan Thinking selama sesi |
| **Pemicu di prompt** | Gunakan frasa seperti "think", "think hard", "ultrathink" |
| **Environment variable** | Set `MAX_THINKING_TOKENS` agar aktif permanen |

### Hierarki Budget Thinking

Claude Code memetakan kata kunci ke budget token thinking:

| Kata Kunci | Budget | Cocok Untuk |
|---------|--------|----------|
| `think` | ~4K token | Perencanaan sederhana, keputusan cepat |
| `think hard` / `megathink` / `think more` / `think a lot` | ~10K token | Kompleksitas menengah |
| `think harder` / `think longer` | ~16K token | Analisis kompleks |
| `ultrathink` | ~32K token (maks) | Arsitektur, debugging kritis |

### Contoh Penggunaan

```
# Simple planning
"Think about how to structure this view"

# Medium complexity  
"Think hard about the navigation architecture for this feature"

# Complex architectural decisions
"Ultrathink about how to design the offline sync system for this app"
```

### Melihat Proses Thinking

- Tekan `Ctrl+O` untuk beralih ke verbose mode dan melihat proses berpikir Claude
- Thinking muncul sebagai *teks miring abu-abu* di atas respons

### Kapan Memakai Ultrathink

✅ **Pakai Ultrathink untuk:**
- Keputusan arsitektur sistem
- Debugging kompleks (mis. race condition, memory leak)
- Perencanaan refactoring skala besar
- Strategi migrasi (UIKit → SwiftUI, CoreData → SwiftData)
- Analisis optimasi performa
- Eksplorasi codebase yang belum dikenal

❌ **Hindari Ultrathink untuk:**
- Perubahan kode sederhana
- Task yang spesifikasi dan langkahnya sudah jelas
- Iterasi prototyping cepat
- Task yang kecepatannya lebih penting daripada kesempurnaan

### Dikombinasikan dengan Plan Mode

```bash
# Start session in Plan Mode with ultrathink
claude --permission-mode plan

# Then in session:
"Ultrathink about how to implement the authentication flow. 
Read the PRD first, then create a comprehensive plan."
```

### Perkiraan Biaya

| Mode | Perkiraan Biaya per Task |
|------|----------------------|
| Standar | ~$0.02-0.05 |
| think | ~$0.06 |
| think hard | ~$0.15 |
| ultrathink | ~$0.30-0.50 |

Gunakan perintah `/cost` untuk memantau penggunaan token selama sesi.

---

## 6. Konfigurasi Plan Mode

### Apa itu Plan Mode?

Plan Mode menginstruksikan Claude untuk menganalisis codebase dengan **operasi read-only** - cocok untuk:
- Perencanaan implementasi multi-langkah
- Eksplorasi kode sebelum melakukan perubahan
- Pengembangan interaktif dengan penyempurnaan arahan secara iteratif

### Mengaktifkan Plan Mode

```bash
# Start new session in Plan Mode
claude --permission-mode plan

# Or use -p flag for headless Plan Mode query
claude --permission-mode plan -p "Analyze the authentication system and suggest improvements"
```

### Jadikan Default

```json
// .claude/settings.json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

### Selama Sesi

- Tekan `Shift+Tab` untuk berpindah mode: Normal → Auto-Accept → Plan Mode
- Indikator Plan Mode: `⏸ plan mode on`
- Indikator Auto-Accept: `⏵⏵ accept edits on`
- Plan Mode membatasi Claude hanya pada operasi read-only

### Strategi Opus Plan Mode

Gunakan Opus untuk perencanaan, Sonnet untuk eksekusi:

```bash
# Plan with Opus
claude --model opus --permission-mode plan

# Execute with Sonnet (more cost-effective)
claude --model sonnet
```

### Slash Command untuk Plan Mode

**`.claude/commands/plan-feature.md`**
```markdown
---
description: Create implementation plan for a feature
argument-hint: <feature-name>
allowed-tools: Read, Grep, Glob
model: opus
---

# Plan Feature Implementation

ultrathink and create a comprehensive implementation plan for: $ARGUMENTS

## Steps
1. Read the PRD and feature spec
2. Analyze the existing codebase structure
3. Identify dependencies and integration points
4. Create step-by-step implementation plan
5. Identify potential challenges and solutions
6. Estimate effort for each step

Output the plan to `docs/tasks/$ARGUMENTS-plan.md`

DO NOT write any code. This is planning only.
```

---

## 7. Integrasi XcodeBuildMCP

XcodeBuildMCP adalah MCP server esensial untuk pengembangan iOS dengan Claude Code.

### Tipe Transport MCP

| Tipe | Deskripsi | Kasus Penggunaan |
|------|-------------|----------|
| **HTTP** | Direkomendasikan untuk server remote | Layanan cloud (Notion, GitHub) |
| **SSE** | Server-Sent Events (deprecated) | Server remote lama |
| **stdio** | Proses lokal | XcodeBuildMCP, tool lokal |

### Instalasi

```bash
# Add XcodeBuildMCP (stdio transport)
claude mcp add --transport stdio XcodeBuildMCP -- npx -y xcodebuildmcp@latest

# With environment variables
claude mcp add --transport stdio XcodeBuildMCP \
  --env INCREMENTAL_BUILDS_ENABLED=true \
  --env XCODEBUILDMCP_DYNAMIC_TOOLS=true \
  -- npx -y xcodebuildmcp@latest

# Specify scope
claude mcp add --transport stdio XcodeBuildMCP --scope project -- npx -y xcodebuildmcp@latest
```

> **Catatan**: `--` memisahkan flag Claude dari perintah server. Semua yang ada setelah `--` adalah perintah untuk menjalankan MCP server.

### Scope MCP

| Scope | Deskripsi | Lokasi |
|-------|-------------|---------|
| **local** (default) | Hanya kamu, proyek saat ini | `~/.claude.json` di bawah path proyek |
| **project** | Dibagikan ke tim via git | `.mcp.json` |
| **user** | Kamu, semua proyek | `~/.claude.json` |

### Konfigurasi Manual (`.mcp.json`)

Buat `.mcp.json` di root proyek untuk MCP server yang dibagikan ke tim:

```json
{
  "mcpServers": {
    "XcodeBuildMCP": {
      "command": "npx",
      "args": ["-y", "xcodebuildmcp@latest"],
      "env": {
        "INCREMENTAL_BUILDS_ENABLED": "true",
        "XCODEBUILDMCP_SENTRY_DISABLED": "true",
        "XCODEBUILDMCP_DYNAMIC_TOOLS": "true",
        "XCODEBUILDMCP_ENABLED_WORKFLOWS": "simulator,device,project-discovery,swift-package"
      }
    }
  }
}
```

### Ekspansi Environment Variable

`.mcp.json` mendukung ekspansi environment variable:

```json
{
  "mcpServers": {
    "custom-server": {
      "command": "${HOME}/tools/server",
      "env": {
        "API_KEY": "${MY_API_KEY}",
        "BASE_URL": "${API_URL:-https://default.example.com}"
      }
    }
  }
}
```

Sintaks:
- `${VAR}` - Diganti dengan nilai variabel
- `${VAR:-default}` - Memakai nilai default jika VAR tidak diset

### Mengelola MCP Server

```bash
# List all configured servers
claude mcp list

# Get details for a specific server
claude mcp get XcodeBuildMCP

# Remove a server
claude mcp remove XcodeBuildMCP

# Check server status (in session)
/mcp

# Authenticate with OAuth-enabled servers
/mcp  # Then select "Authenticate"
```

### Batas Output MCP

```bash
# Set higher limit for large outputs (default: 25,000 tokens)
export MAX_MCP_OUTPUT_TOKENS=50000
claude

# Set MCP server startup timeout (default: 60 seconds)
MCP_TIMEOUT=10000 claude
```

### MCP Resources dengan @ Mention

Referensikan MCP resource dengan format `@server:protocol://resource/path`:

```
> Analyze @github:issue://123 and suggest a fix
> Review the documentation at @docs:file://api/authentication
```

### Tool XcodeBuildMCP yang Tersedia

| Tool | Deskripsi |
|------|-------------|
| `mcp__xcodebuildmcp__discover_projects` | Cari project/workspace Xcode |
| `mcp__xcodebuildmcp__list_schemes` | Daftar build scheme yang tersedia |
| `mcp__xcodebuildmcp__build_sim_name_proj` | Build untuk iOS simulator |
| `mcp__xcodebuildmcp__build_device_proj` | Build untuk perangkat fisik |
| `mcp__xcodebuildmcp__test_sim_name_proj` | Jalankan test di simulator |
| `mcp__xcodebuildmcp__clean` | Bersihkan hasil build |
| `mcp__xcodebuildmcp__list_simulators` | Daftar simulator yang tersedia |
| `mcp__xcodebuildmcp__boot_simulator` | Nyalakan (boot) simulator |
| `mcp__xcodebuildmcp__install_app` | Install app di simulator/perangkat |
| `mcp__xcodebuildmcp__launch_app` | Jalankan app yang sudah terinstall |
| `mcp__xcodebuildmcp__capture_logs` | Tangkap log runtime |
| `mcp__xcodebuildmcp__screenshot` | Ambil screenshot simulator |
| `mcp__xcodebuildmcp__swift_package_build` | Build Swift package |
| `mcp__xcodebuildmcp__swift_package_test` | Jalankan test Swift package |
| `mcp__xcodebuildmcp__create_project` | Buat scaffold proyek iOS/macOS baru |

### Pemakaian di CLAUDE.md

```markdown
## Build Commands
- **Build**: Use `mcp__xcodebuildmcp__build_sim_name_proj` for simulator builds
- **Test**: Use `mcp__xcodebuildmcp__test_sim_name_proj` for running tests
- **Clean**: Use `mcp__xcodebuildmcp__clean` before major rebuilds
- **Logs**: Use `mcp__xcodebuildmcp__capture_logs` to debug runtime issues
```

---

## 8. Custom Slash Command untuk iOS

### Slash Command Proyek (`.claude/commands/`)

**build.md**
```markdown
---
description: Build the iOS project intelligently
allowed-tools: mcp__xcodebuildmcp__*
---

# Build Project

Detect the project type and build appropriately:
1. Check for .xcworkspace or .xcodeproj
2. Use XcodeBuildMCP to build for iOS Simulator
3. Report any build errors with suggested fixes
```

**test.md**
```markdown
---
description: Run all relevant tests
allowed-tools: mcp__xcodebuildmcp__*, Bash(swift *)
---

# Run Tests

Based on current context:
- For main app: `mcp__xcodebuildmcp__test_sim_name_proj`
- For Swift packages: `mcp__xcodebuildmcp__swift_package_test`
- Report test results and any failures
```

**run-app.md**
```markdown
---
description: Build and launch app on simulator
allowed-tools: mcp__xcodebuildmcp__*
---

# Build and Run

1. List available simulators
2. Build the app for the default simulator (iPhone 15)
3. Boot the simulator if needed
4. Install and launch the app
5. Start capturing logs
```

**create-view.md**
```markdown
---
description: Create a new SwiftUI view with ViewModel
argument-hint: <ViewName>
allowed-tools: Read, Write
---

# Create SwiftUI View: $ARGUMENTS

Create a new SwiftUI view following project patterns:

1. Read existing views for style reference
2. Create `$ARGUMENTS.swift` in appropriate Features directory
3. Create `${ARGUMENTS}ViewModel.swift` as @Observable class
4. Add preview provider
5. Follow project's navigation and styling patterns
```

**refactor-view.md**
```markdown
---
description: Extract and refactor a SwiftUI view
argument-hint: <source-file>
allowed-tools: Read, Write, Edit
---

# Refactor View: $ARGUMENTS

Analyze the view and:
1. Identify extractable subviews (>50 lines or reusable)
2. Extract to separate files
3. Create appropriate ViewModels if needed
4. Ensure proper data flow with bindings
5. Add documentation comments
```

**fix-build.md**
```markdown
---
description: Diagnose and fix build errors
allowed-tools: mcp__xcodebuildmcp__*, Read, Write, Edit, Bash(swift *)
---

# Fix Build Errors

ultrathink about the build errors:

1. Run a clean build to get fresh errors
2. Analyze each error
3. Propose fixes
4. Implement fixes one at a time
5. Verify build succeeds
```

**implement-feature.md**
```markdown
---
description: Implement a feature from spec
argument-hint: <feature-name>
allowed-tools: Read, Write, Edit, mcp__xcodebuildmcp__*
---

# Implement Feature: $ARGUMENTS

Follow the PRD workflow:

1. Read `docs/specs/$ARGUMENTS.md` for requirements
2. Read `docs/tasks/$ARGUMENTS-tasks.md` for task breakdown
3. Implement the current uncompleted task
4. Write tests for the implementation
5. Update task progress in the tasks file
6. Build and test

Stop after completing each task and wait for approval.
```

### Command Pribadi (`~/.claude/commands/`)

**swift-style.md**
```markdown
---
description: Check Swift style and conventions
allowed-tools: Bash(swiftlint *), Bash(swift-format *)
---

# Swift Style Check

Run linting and formatting checks:
1. Run SwiftLint if configured
2. Run swift-format check
3. Report violations with suggested fixes
```

---

## 9. Agent Skills

Agent Skills adalah kemampuan yang **dipanggil oleh model** - Claude memutuskan sendiri kapan memakainya berdasarkan permintaanmu dan deskripsi Skill. Berbeda dengan slash command (dipanggil oleh user), Skills aktif otomatis saat relevan.

### Membuat Agent Skills

Skills disimpan sebagai direktori yang berisi file `SKILL.md`:

| Lokasi | Scope | Kasus Penggunaan |
|----------|-------|----------|
| `~/.claude/skills/skill-name/` | Pribadi | Workflow pribadimu |
| `.claude/skills/skill-name/` | Project | Kemampuan yang dibagikan ke tim |

### Struktur Skill

**`.claude/skills/ios-testing/SKILL.md`**
```markdown
---
name: ios-testing
description: iOS testing expert for unit tests, UI tests, and test-driven development. Use when working with XCTest, testing SwiftUI views, or implementing test strategies.
allowed-tools: Read, Grep, Glob, mcp__xcodebuildmcp__test_*
---

# iOS Testing Expert

## Instructions
1. Analyze existing test coverage
2. Identify untested code paths
3. Generate comprehensive test cases
4. Use XCTest and Swift Testing frameworks

## Best Practices
- Test behavior, not implementation
- Use dependency injection for testability
- Mock external dependencies
- Target 80%+ code coverage for critical paths
```

### Skill dengan Tool Terbatas

Gunakan `allowed-tools` untuk membatasi kemampuan Claude saat Skill aktif:

**`.claude/skills/code-analyzer/SKILL.md`**
```markdown
---
name: code-analyzer
description: Read-only code analysis for architecture review and code quality assessment. Use when reviewing PRs or analyzing codebase structure.
allowed-tools: Read, Grep, Glob
---

# Code Analyzer (Read-Only)

## Review Checklist
1. Code organization and structure
2. Error handling patterns
3. Performance considerations
4. Security concerns
5. Test coverage gaps

Provide detailed feedback without modifying files.
```

### Skill Multi-File

Skill dapat menyertakan file pendukung:

```
.claude/skills/swiftui-components/
├── SKILL.md
├── PATTERNS.md
├── REFERENCE.md
└── templates/
    ├── view-template.swift
    └── viewmodel-template.swift
```

**SKILL.md**
```markdown
---
name: swiftui-components
description: SwiftUI component expert for building reusable views, custom modifiers, and view compositions. Use when creating new SwiftUI views or refactoring UI code.
---

# SwiftUI Components

## Quick Start
For standard patterns, see [PATTERNS.md](PATTERNS.md).
For API reference, see [REFERENCE.md](REFERENCE.md).

## Instructions
1. Analyze the required component functionality
2. Check existing components for reuse
3. Apply templates from templates/ directory
4. Follow project styling conventions
```

### Mengelola Skills

```bash
# View available Skills
> "What Skills are available?"

# Skills activate automatically based on context
> "Help me write tests for the authentication module"
# → iOS testing Skill activates automatically

# Explicitly check Skill files
ls ~/.claude/skills/
ls .claude/skills/
```

### Best Practice Skill

| Praktik | Contoh |
|----------|---------|
| **Scope yang fokus** | "Mengisi formulir PDF" bukan "Pemrosesan dokumen" |
| **Pemicu yang jelas** | Sertakan kapan skill dipakai di deskripsi |
| **Keyword spesifik** | "Use when working with XCTest, testing SwiftUI views" |

---

## 10. Konfigurasi Subagent

Subagent adalah asisten AI khusus yang dipanggil untuk menangani jenis task tertentu.

### Membuat Subagent Khusus iOS

Subagent didefinisikan di `.claude/agents/` atau `~/.claude/agents/`

**`.claude/agents/ios-architect.md`**
```markdown
---
name: ios-architect
description: iOS architecture expert for system design and patterns
model: opus
tools: Read, Grep, Glob
---

You are an expert iOS architect specializing in Swift and SwiftUI.

Your expertise includes:
- MVVM, MVC, VIPER, Clean Architecture
- Swift Concurrency (actors, async/await, Sendable)
- SwiftUI navigation patterns
- Dependency injection
- SwiftData and Core Data
- Modular app architecture

When consulted:
1. Analyze the existing codebase structure
2. Consider scalability and maintainability
3. Propose patterns that fit the team's expertise
4. Provide concrete Swift code examples
5. Consider testing implications

Focus on practical, production-ready advice.
```

**`.claude/agents/swift-reviewer.md`**
```markdown
---
name: swift-reviewer
description: Code reviewer for Swift/SwiftUI code quality
model: sonnet
tools: Read, Grep
---

You are an expert Swift code reviewer.

Review code for:
- Swift 6 concurrency safety
- Memory management (retain cycles, weak references)
- SwiftUI best practices
- API design guidelines compliance
- Performance considerations
- Test coverage
- Documentation quality

Provide specific, actionable feedback with code examples.
```

**`.claude/agents/swiftui-specialist.md`**
```markdown
---
name: swiftui-specialist
description: SwiftUI expert for complex UI implementation
tools: Read, Write, Edit
---

You are a SwiftUI specialist with deep knowledge of:
- Custom layouts and GeometryReader
- Animations and transitions
- Custom view modifiers
- Preference keys
- Environment and EnvironmentObject
- NavigationStack and navigation patterns
- Performance optimization
- Accessibility

When building UI:
1. Start with the simplest approach
2. Extract reusable components
3. Use proper state management
4. Ensure accessibility
5. Consider all device sizes
```

### Menggunakan Subagent

```
# Explicit invocation
"Use the ios-architect to design the data layer"

# Automatic invocation (based on description matching)
"Review this authentication code for security issues"
# Claude may automatically invoke swift-reviewer
```

### Subagent untuk Riset

**`.claude/agents/ios-researcher.md`**
```markdown
---
name: ios-researcher  
description: Research iOS APIs and best practices
tools: WebSearch, WebFetch, Read
---

You are an iOS research specialist.

When researching:
1. Search Apple Developer documentation
2. Find relevant WWDC sessions
3. Check Swift Evolution proposals if relevant
4. Look for community best practices
5. Summarize findings with code examples

Always cite sources and note iOS version requirements.
```

---

## 11. Output Style

Output Style menyesuaikan Claude Code untuk berbagai kebutuhan dengan memodifikasi system prompt-nya.

### Output Style Bawaan

| Style | Deskripsi |
|-------|-------------|
| **Default** | Fokus rekayasa perangkat lunak standar |
| **Explanatory** | Memberikan "Insight" edukatif saat coding |
| **Learning** | Belajar sambil praktik secara kolaboratif dengan penanda `TODO(human)` |

### Mengganti Output Style

```bash
# Open style menu
/output-style

# Or switch directly
/output-style explanatory
/output-style learning
```

### Membuat Output Style Kustom

Buat file di `.claude/output-styles/` atau `~/.claude/output-styles/`:

**`.claude/output-styles/ios-mentor.md`**
```markdown
---
name: iOS Mentor
description: Teaching-focused style for iOS development with explanations and learning opportunities
keep-coding-instructions: true
---

# iOS Mentor Style

You are an iOS development mentor helping developers learn Swift and SwiftUI.

## Behaviors
1. Explain **why** before showing **how**
2. Reference Apple documentation and WWDC sessions
3. Point out common pitfalls and Swift idioms
4. Suggest further learning resources
5. Add `// 💡 Learn: ...` comments explaining key concepts

## Teaching Approach
- Start with the concept, then show implementation
- Compare approaches when multiple valid options exist
- Highlight Swift 6 and iOS 18 modern patterns
```

### Output Style vs Fitur Lain

| Fitur | Fungsi | Pemanggilan |
|---------|---------|------------|
| **Output Style** | Mengubah perilaku system prompt | Otomatis (berlaku satu sesi penuh) |
| **Slash Commands** | Prompt tersimpan | User mengetik `/command` |
| **Agent Skills** | Kemampuan khusus | Otomatis (model yang memutuskan) |
| **Subagents** | Delegasi task | Otomatis atau eksplisit |

---

## 12. Sistem Plugin

Plugin memperluas Claude Code dengan fungsionalitas kustom - commands, agents, Skills, hooks, dan MCP server dalam satu paket.

### Menginstall Plugin

```bash
# Open plugin manager
/plugin

# Install from official marketplace (auto-available)
# Go to Discover tab, select plugin, choose scope

# Add custom marketplace
claude plugin marketplace add owner/repo

# Install directly
claude plugin install plugin-name@marketplace-name
```

### Scope Plugin

| Scope | Deskripsi |
|-------|-------------|
| **user** | Tersedia di semua proyekmu |
| **local** | Hanya kamu, proyek saat ini |
| **project** | Dibagikan ke tim via git |

### Marketplace Resmi

Marketplace resmi Anthropic mencakup:
- Plugin **Code Intelligence** (LSP untuk Swift, Python, dll.)
- Plugin **Workflow** untuk task umum
- Plugin **Integration** untuk layanan eksternal

### Membuat Plugin

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   └── my-command.md
├── agents/
│   └── my-agent.md
├── skills/
│   └── my-skill/
│       └── SKILL.md
├── hooks/
│   └── hooks.json
└── .mcp.json
```

**`.claude-plugin/plugin.json`**
```json
{
  "name": "ios-toolkit",
  "version": "1.0.0",
  "description": "iOS development toolkit with commands, agents, and MCP integration",
  "author": "Your Name"
}
```

### MCP Server yang Disediakan Plugin

Plugin dapat menyertakan MCP server di `.mcp.json`:

```json
{
  "mcpServers": {
    "plugin-server": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/my-server",
      "args": ["--config", "${CLAUDE_PLUGIN_ROOT}/config.json"],
      "env": {
        "DATA_PATH": "${CLAUDE_PLUGIN_ROOT}/data"
      }
    }
  }
}
```

Environment variable:
- `${CLAUDE_PLUGIN_ROOT}` - Path direktori plugin
- `${CLAUDE_PROJECT_DIR}` - Direktori root proyek

### Menguji Plugin Saat Pengembangan

```bash
# Load plugin from local directory
claude --plugin-dir ./my-plugin

# Load multiple plugins
claude --plugin-dir ./plugin-one --plugin-dir ./plugin-two
```

---

## 13. Sandbox Mode & Pengembangan Aman

Claude Code dapat dijalankan dalam mode "sandbox" terbatas yang mencegah operasi destruktif, namun tetap mengizinkan build Xcode dan pengujian di simulator. Cocok untuk:
- Belajar Claude Code dengan aman
- Meninjau perubahan hasil AI sebelum diterapkan
- Lingkungan tim yang membutuhkan pengawasan

### Mode Izin

| Mode | Deskripsi | Kasus Penggunaan |
|------|-------------|----------|
| **Plan Mode** | Read-only, tanpa modifikasi file | Perencanaan arsitektur, code review |
| **Normal Mode** | Meminta izin untuk setiap aksi | Default, keamanan seimbang |
| **Auto-Accept Mode** | Dieksekusi tanpa konfirmasi | Task tepercaya dan repetitif |

### Konfigurasi Sandbox

Buat `.claude/settings.json` yang restriktif: mengizinkan build tetapi membatasi penulisan file:

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "mcp__xcodebuildmcp__discover_projects",
      "mcp__xcodebuildmcp__list_schemes",
      "mcp__xcodebuildmcp__list_simulators",
      "mcp__xcodebuildmcp__build_sim_name_proj",
      "mcp__xcodebuildmcp__build_sim_name_ws",
      "mcp__xcodebuildmcp__test_sim_name_proj",
      "mcp__xcodebuildmcp__test_sim_name_ws",
      "mcp__xcodebuildmcp__clean",
      "mcp__xcodebuildmcp__boot_simulator",
      "mcp__xcodebuildmcp__install_app",
      "mcp__xcodebuildmcp__launch_app",
      "mcp__xcodebuildmcp__capture_logs",
      "mcp__xcodebuildmcp__screenshot",
      "mcp__xcodebuildmcp__swift_package_build",
      "mcp__xcodebuildmcp__swift_package_test",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Bash(swift build *)",
      "Bash(swift test *)",
      "Bash(swiftlint *)",
      "Bash(xcrun simctl *)"
    ],
    "deny": [
      "Write",
      "Edit",
      "Bash(rm *)",
      "Bash(git push *)",
      "Bash(git commit *)",
      "Bash(git checkout *)",
      "Bash(git reset *)"
    ]
  }
}
```

### Workflow Sandbox

#### 1. Mulai di Plan Mode
```bash
# Start session in Plan Mode (read-only)
claude --permission-mode plan
```

#### 2. Biarkan Claude Menganalisis dan Merencanakan
```
"Ultrathink about how to implement the login feature. 
Read the codebase, understand the architecture, and create a detailed plan.
DO NOT write any code yet."
```

#### 3. Tinjau Rencananya
Claude akan menganalisis codebase-mu dan mengusulkan perubahan tanpa memodifikasi apa pun.

#### 4. Pindah ke Normal Mode untuk Eksekusi Terkontrol
```bash
# In session, press Shift+Tab twice to cycle to Normal Mode
# Or start a new session without --permission-mode plan
claude
```

#### 5. Terapkan Perubahan dengan Persetujuan
Di Normal Mode, Claude akan meminta izin sebelum setiap modifikasi file.

### Level Izin Bertingkat

#### Level 1: Sandbox Penuh (Hanya Baca + Build)
```json
{
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "mcp__xcodebuildmcp__build_*",
      "mcp__xcodebuildmcp__test_*",
      "mcp__xcodebuildmcp__list_*",
      "mcp__xcodebuildmcp__boot_simulator",
      "mcp__xcodebuildmcp__capture_logs"
    ],
    "deny": ["Write", "Edit"]
  }
}
```
- ✅ Membaca semua file
- ✅ Build proyek
- ✅ Menjalankan test
- ✅ Boot simulator dan menangkap log
- ❌ Tidak bisa membuat atau mengubah file

#### Level 2: Sandbox + Menulis Dokumentasi
```json
{
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "Write(docs/*)",
      "Edit(docs/*)",
      "mcp__xcodebuildmcp__*"
    ],
    "deny": [
      "Write(*.swift)",
      "Edit(*.swift)",
      "Write(*.json)",
      "Write(*.plist)"
    ]
  }
}
```
- ✅ Semua kemampuan Level 1
- ✅ Bisa menulis PRD, spec, dan dokumentasi
- ❌ Tidak bisa mengubah kode Swift atau file konfigurasi

#### Level 3: Sandbox + File Test
```json
{
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "Write(docs/*)",
      "Write(*Tests/*.swift)",
      "Write(*Tests/**/*.swift)",
      "Edit(*Tests/*.swift)",
      "Edit(*Tests/**/*.swift)",
      "mcp__xcodebuildmcp__*"
    ],
    "deny": [
      "Write(*/App/*)",
      "Write(*/Features/*)",
      "Write(*/Core/*)"
    ]
  }
}
```
- ✅ Semua kemampuan Level 2
- ✅ Bisa menulis dan mengedit file test
- ❌ Tidak bisa mengubah kode produksi

#### Level 4: Pengembangan Penuh (Normal)
```json
{
  "permissions": {
    "allow": [
      "Read", "Write", "Edit",
      "mcp__xcodebuildmcp__*",
      "Bash(git *)",
      "Bash(swift *)"
    ],
    "deny": [
      "Write(.env*)",
      "Write(**/Secrets.swift)",
      "Bash(rm -rf *)"
    ]
  }
}
```
- ✅ Kemampuan pengembangan penuh
- ❌ Tidak bisa menyentuh secrets atau menjalankan perintah destruktif

### Slash Command Sandbox

Buat command khusus sandbox:

**`.claude/commands/sandbox-review.md`**
```markdown
---
description: Review code changes in sandbox mode (read-only)
allowed-tools: Read, Grep, Glob, mcp__xcodebuildmcp__build_*, mcp__xcodebuildmcp__test_*
---

# Sandbox Code Review

Analyze the codebase in read-only mode:

1. Read and understand the current implementation
2. Build the project to verify it compiles
3. Run tests to check current state
4. Identify issues and improvements
5. Output findings as a report

DO NOT modify any files. This is analysis only.
```

**`.claude/commands/sandbox-build.md`**
```markdown
---
description: Build and test without modifying code
allowed-tools: mcp__xcodebuildmcp__*, Read
---

# Sandbox Build & Test

Execute build and test cycle:

1. Clean build products: `mcp__xcodebuildmcp__clean`
2. Build for simulator: `mcp__xcodebuildmcp__build_sim_name_proj`
3. Run all tests: `mcp__xcodebuildmcp__test_sim_name_proj`
4. Report results

No file modifications allowed.
```

**`.claude/commands/sandbox-run.md`**
```markdown
---
description: Build and run app on simulator (sandbox)
allowed-tools: mcp__xcodebuildmcp__*
---

# Sandbox Run App

Build and launch without code changes:

1. List simulators: `mcp__xcodebuildmcp__list_simulators`
2. Boot iPhone 15: `mcp__xcodebuildmcp__boot_simulator`
3. Build app: `mcp__xcodebuildmcp__build_sim_name_proj`
4. Install: `mcp__xcodebuildmcp__install_app`
5. Launch: `mcp__xcodebuildmcp__launch_app`
6. Capture logs: `mcp__xcodebuildmcp__capture_logs`

Report any build errors or runtime issues.
```

### Memakai Override Local Settings

Untuk preferensi sandbox pribadi tanpa memengaruhi settings tim:

**`.claude/settings.local.json`** (di-gitignore)
```json
{
  "permissions": {
    "deny": ["Write", "Edit"]
  }
}
```

Ini menimpa settings tim sehingga mode sandbox berlaku di lingkungan lokalmu.

### Contoh Workflow Sesi Sandbox

```bash
# Terminal 1: Start Claude Code in sandbox
cd /path/to/ios-project
claude --permission-mode plan

# In Claude Code session:
> /sandbox-review
# Claude analyzes codebase, builds, tests, reports findings

> "What improvements would you suggest for the networking layer? ultrathink"
# Claude provides detailed analysis without modifying code

> "Create a detailed implementation plan for those improvements"
# Claude outputs plan to docs/

# When ready to implement, start new session:
# Terminal 2: Normal mode for implementation
claude
> "Implement step 1 from the networking improvement plan"
# Claude asks permission before each change
```

### Integrasi Xcode di Sandbox Mode

Bahkan di sandbox penuh, kamu tetap bisa:

| Aksi | Perintah | Diizinkan |
|--------|---------|---------|
| Build proyek | `mcp__xcodebuildmcp__build_sim_name_proj` | ✅ |
| Jalankan test | `mcp__xcodebuildmcp__test_sim_name_proj` | ✅ |
| Boot simulator | `mcp__xcodebuildmcp__boot_simulator` | ✅ |
| Install app | `mcp__xcodebuildmcp__install_app` | ✅ |
| Jalankan app | `mcp__xcodebuildmcp__launch_app` | ✅ |
| Ambil screenshot | `mcp__xcodebuildmcp__screenshot` | ✅ |
| Tangkap log | `mcp__xcodebuildmcp__capture_logs` | ✅ |
| Buat proyek | `mcp__xcodebuildmcp__create_project` | ❌ (jika Write ditolak) |

---

## 14. Settings & Permissions

### Settings Proyek (`.claude/settings.json`)

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": [
      "mcp__xcodebuildmcp__*",
      "Read",
      "Write",
      "Edit",
      "Bash(git *)",
      "Bash(swift *)",
      "Bash(swiftlint *)",
      "Bash(swift-format *)",
      "Bash(xcodegen *)",
      "Bash(pod *)",
      "Bash(mint *)",
      "WebFetch"
    ],
    "deny": [
      "Read(.env*)",
      "Read(**/Secrets.swift)",
      "Write(.env*)",
      "Bash(rm -rf *)"
    ]
  },
  "env": {
    "PROJECT_NAME": "MyApp",
    "DEFAULT_SIMULATOR": "iPhone 15",
    "SWIFT_VERSION": "6.0",
    "IOS_DEPLOYMENT_TARGET": "17.0"
  }
}
```

### Settings Lokal (`.claude/settings.local.json`)

```json
{
  "model": "opus",
  "env": {
    "DEVELOPMENT_TEAM": "YOUR_TEAM_ID",
    "CODE_SIGN_IDENTITY": "Apple Development"
  }
}
```

### Settings User Global (`~/.claude/settings.json`)

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Read",
      "Bash(swift *)"
    ]
  },
  "theme": "dark",
  "preferredEditor": "xcode"
}
```

---

## 15. Hooks untuk Pengembangan Swift

Hooks adalah perintah shell buatan user yang dijalankan pada berbagai titik dalam lifecycle Claude Code. Hooks memberi **kontrol deterministik** - aksi tertentu selalu terjadi, tidak bergantung pada LLM.

### Event Hook

| Event | Pemicu | Kasus Penggunaan |
|-------|---------|----------|
| **PreToolUse** | Sebelum pemanggilan tool (bisa memblokir) | Validasi perintah, lindungi file |
| **PostToolUse** | Setelah pemanggilan tool selesai | Format kode, jalankan linter |
| **PermissionRequest** | Saat dialog izin ditampilkan | Auto-approve/deny |
| **UserPromptSubmit** | Saat user mengirim prompt | Pra-proses input |
| **Notification** | Saat Claude mengirim notifikasi | Alert kustom |
| **Stop** | Saat Claude selesai merespons | Ringkasan, cleanup |
| **SubagentStop** | Saat task subagent selesai | Post-processing |
| **PreCompact** | Sebelum operasi compact | Simpan konteks |
| **SessionStart** | Saat sesi dimulai/dilanjutkan | Setup environment |
| **SessionEnd** | Saat sesi berakhir | Cleanup, logging |

### Konfigurasi

Tambahkan ke `.claude/settings.json` atau gunakan perintah `/hooks`:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/session-start.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | { read f; [[ \"$f\" == *.swift ]] && swiftlint lint --path \"$f\" --quiet; }"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "python3 -c \"import json,sys; d=json.load(sys.stdin); p=d.get('tool_input',{}).get('file_path',''); sys.exit(2 if '.env' in p or 'Secrets.swift' in p else 0)\""
          }
        ]
      }
    ]
  }
}
```

### Pola Matcher

| Pola | Cocok Dengan |
|---------|---------|
| `Write` | Tepat tool "Write" |
| `Write\|Edit` | Tool Write ATAU Edit |
| `mcp__xcodebuildmcp__*` | Semua tool XcodeBuildMCP |
| `*` atau `""` | Semua tool |

### Return Code Hook

Untuk hook **PreToolUse**:
- Exit `0`: Lanjutkan pemanggilan tool
- Exit `2`: Blokir pemanggilan tool dan beri feedback ke Claude

Untuk hook **PermissionRequest**, keluarkan JSON:
```json
{
  "decision": "approve",
  "reason": "Documentation file auto-approved",
  "suppressOutput": true
}
```

### Environment Variable

| Variabel | Deskripsi |
|----------|-------------|
| `$CLAUDE_PROJECT_DIR` | Direktori root proyek |
| `${CLAUDE_PLUGIN_ROOT}` | Direktori plugin (untuk hook dari plugin) |

### Script Hook

**`.claude/hooks/session-start.sh`**
```bash
#!/bin/bash

PROJECT_NAME=$(basename "$(pwd)")
SWIFT_VERSION=$(swift --version 2>/dev/null | head -1 | grep -oE '[0-9]+\.[0-9]+' | head -1)
XCODE_VERSION=$(xcodebuild -version 2>/dev/null | head -1)

echo "🚀 Starting $PROJECT_NAME session" >&2
echo "📱 Swift $SWIFT_VERSION | $XCODE_VERSION" >&2

# Check for common issues
if ! command -v swiftlint &> /dev/null; then
    echo "⚠️  SwiftLint not installed" >&2
fi

# Check if simulator is booted
if ! xcrun simctl list devices booted | grep -q "Booted"; then
    echo "💡 No simulator running. Use /run-app to boot one." >&2
fi
```

**`.claude/hooks/post-swift-edit.sh`**
```bash
#!/bin/bash

# Read file path from stdin JSON
FILE=$(jq -r '.tool_input.file_path // empty')

if [[ "$FILE" != *.swift ]]; then
    exit 0
fi

# Run SwiftLint on the edited file
if command -v swiftlint &> /dev/null && [ -f ".swiftlint.yml" ]; then
    swiftlint lint --path "$FILE" --quiet 2>&1 | head -5 >&2
fi

# Run swift-format check
if command -v swift-format &> /dev/null; then
    swift-format lint "$FILE" 2>&1 | head -3 >&2
fi
```

**`.claude/hooks/file-protection.sh`** (PreToolUse)
```bash
#!/bin/bash

# Block edits to sensitive files
FILE=$(jq -r '.tool_input.file_path // empty' < /dev/stdin)

PROTECTED_PATTERNS=(
    ".env"
    "Secrets.swift"
    "GoogleService-Info.plist"
    ".git/"
    "Podfile.lock"
)

for pattern in "${PROTECTED_PATTERNS[@]}"; do
    if [[ "$FILE" == *"$pattern"* ]]; then
        echo "🛡️ Protected file: $FILE" >&2
        exit 2  # Block the operation
    fi
done

exit 0  # Allow the operation
```

### Setup lewat Perintah `/hooks`

```bash
# Interactive hook configuration
/hooks

# Select event type (e.g., PostToolUse)
# Add matcher (e.g., "Write|Edit")
# Add hook command
# Choose storage location (User/Project)
```

Buat hooks bisa dieksekusi (executable):
```bash
chmod +x .claude/hooks/*.sh
```

---

## 16. Struktur Proyek Lengkap

```
MyiOSApp/
├── .claude/
│   ├── commands/                    # Project slash commands
│   │   ├── build.md
│   │   ├── test.md
│   │   ├── run-app.md
│   │   ├── create-view.md
│   │   ├── implement-feature.md
│   │   ├── create-prd.md
│   │   ├── generate-spec.md
│   │   ├── sandbox-build.md
│   │   └── sandbox-review.md
│   ├── agents/                      # Project subagents
│   │   ├── ios-architect.md
│   │   ├── swift-reviewer.md
│   │   └── swiftui-specialist.md
│   ├── skills/                      # Agent Skills
│   │   ├── ios-testing/
│   │   │   └── SKILL.md
│   │   └── swiftui-components/
│   │       ├── SKILL.md
│   │       └── templates/
│   ├── output-styles/               # Custom output styles
│   │   └── ios-mentor.md
│   ├── hooks/                       # Automation hooks
│   │   ├── session-start.sh
│   │   ├── post-swift-edit.sh
│   │   └── file-protection.sh
│   ├── settings.json               # Team settings (committed)
│   ├── settings.local.json         # Personal settings (gitignored)
│   └── settings.sandbox.json       # Sandbox mode template
├── .mcp.json                        # MCP server configuration
├── CLAUDE.md                        # Main project context
├── docs/
│   ├── PRD.md                      # Product Requirements Document
│   ├── ARCHITECTURE.md             # Architecture decisions
│   ├── ROADMAP.md                  # Development roadmap
│   ├── specs/                      # Feature specifications
│   │   ├── template.md
│   │   ├── 001-authentication.md
│   │   └── 002-dashboard.md
│   └── tasks/                      # Task breakdowns
│       └── authentication-tasks.md
├── MyApp/
│   ├── App/
│   │   └── MyAppApp.swift
│   ├── Features/
│   │   ├── Authentication/
│   │   │   ├── CLAUDE.md           # Feature-specific context
│   │   │   ├── Views/
│   │   │   └── ViewModels/
│   │   └── Dashboard/
│   ├── Core/
│   │   ├── Extensions/
│   │   ├── Services/
│   │   └── Networking/
│   └── Resources/
├── MyAppTests/
├── MyAppUITests/
├── Package.swift                    # If using SPM
├── .swiftlint.yml                  # SwiftLint configuration
├── .swift-format                    # Swift-format configuration
└── .gitignore
```

### Tambahan `.gitignore` yang Direkomendasikan

```gitignore
# Claude Code
.claude/settings.local.json
.claude/*.log

# Keep these committed
!.claude/commands/
!.claude/agents/
!.claude/hooks/
!.claude/settings.json
```

---

## 17. Best Practice & Tips

### Best Practice Workflow

1. **Mulai dengan Perencanaan**
   - Gunakan Plan Mode untuk fitur baru
   - Tulis spec sebelum kode
   - Gunakan `ultrathink` untuk keputusan arsitektur

2. **Pengembangan Inkremental**
   - Selesaikan satu task dalam satu waktu
   - Test setelah setiap perubahan
   - Commit secara rutin

3. **Manajemen Konteks**
   - Gunakan `/compact` saat konteks membesar
   - Gunakan `/clear` di antara task yang tidak berkaitan
   - Jaga CLAUDE.md tetap ringkas tapi lengkap

4. **Penggunaan Subagent**
   - Gunakan subagent `ios-architect` di awal untuk keputusan desain
   - Gunakan subagent `swift-reviewer` sebelum membuat PR
   - Jalankan subagent riset untuk API yang belum dikenal

### Pola Prompt untuk Pengembangan iOS

```
# Starting a new feature
"Read the PRD and spec for [feature]. Ultrathink about the implementation 
approach, then create a task breakdown in docs/tasks/"

# Code implementation
"Implement step 2 from docs/tasks/[feature]-tasks.md. Write tests first, 
then implementation. Build and run tests when complete."

# Debugging
"The app crashes when [action]. Use ultrathink to analyze the issue. 
Capture logs with XcodeBuildMCP and identify the root cause."

# Refactoring
"This view is too large. Think hard about how to extract it into 
smaller components while maintaining the same behavior."

# Architecture review
"Use the ios-architect to review the current data layer design 
and suggest improvements for offline support."
```

### Shortcut Keyboard

| Shortcut | Aksi |
|----------|--------|
| `Tab` | Menyalakan/mematikan extended thinking |
| `Shift+Tab` | Berpindah mode izin (Normal → Auto-Accept → Plan) |
| `Ctrl+C` | Membatalkan operasi yang sedang berjalan |
| `Ctrl+O` | Beralih ke verbose mode (melihat proses thinking) |
| `/` | Membuka menu slash command |
| `@` | Mereferensikan file, direktori, atau MCP resource |

### Referensi Perintah Umum

| Perintah | Deskripsi |
|---------|-------------|
| `/help` | Menampilkan semua perintah yang tersedia |
| `/clear` | Membersihkan konteks percakapan |
| `/compact` | Meringkas dan memadatkan konteks |
| `/cost` | Menampilkan penggunaan token dan biaya |
| `/model` | Mengganti model di tengah sesi |
| `/config` | Membuka antarmuka konfigurasi |
| `/agents` | Mengelola subagent |
| `/hooks` | Mengatur hooks |
| `/mcp` | Mengelola MCP server |
| `/plugin` | Plugin manager (cari, install, kelola) |
| `/output-style` | Mengganti output style |
| `/status` | Menampilkan status sesi dan imports |

### Melanjutkan Percakapan

```bash
# Continue most recent conversation
claude --continue

# Show conversation picker
claude --resume

# Continue with a specific prompt
claude --continue --print "Continue with my task"
```

### Referensi dengan @

```bash
# Reference a file
> Explain the logic in @src/Features/Auth/AuthViewModel.swift

# Reference a directory
> What's the structure of @MyApp/Features?

# Reference with line ranges (select in editor + Alt+K)
> Review @src/login.swift:45-67

# Reference MCP resources
> Show me @github:issue://123
```

### Tips Performa

1. **Gunakan Incremental Build**
   - Aktifkan `INCREMENTAL_BUILDS_ENABLED=true` di XcodeBuildMCP
   - Siklus iterasi jauh lebih cepat

2. **Minimalkan Konteks**
   - Jangan memuat file yang tidak perlu
   - Gunakan import yang spesifik di CLAUDE.md
   - Lakukan compact secara rutin pada sesi panjang

3. **Pilih Model yang Tepat**
   - Opus untuk perencanaan dan arsitektur
   - Sonnet untuk sebagian besar pekerjaan implementasi
   - Haiku untuk task sederhana yang berulang

4. **Gabungkan Perubahan yang Berkaitan**
   - Kelompokkan perubahan file yang berkaitan
   - Mengurangi siklus build-test

### Troubleshooting

```bash
# Check Claude Code health
claude doctor

# Debug MCP connections
claude --mcp-debug

# View session logs
tail -f ~/.claude/logs/session.log

# Reset configuration
# ⚠️ Destructive: deletes ALL user settings, skills, commands and history.
# Back it up first:
cp -r ~/.claude ~/.claude.backup
rm -rf ~/.claude && claude
```


---

## Referensi

- [Claude Code Official Documentation](https://code.claude.com/docs)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [XcodeBuildMCP GitHub](https://github.com/cameroncooke/XcodeBuildMCP)
- [Apple Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/)
- [Swift Evolution Proposals](https://github.com/apple/swift-evolution)

---

## Tentang Penulis

<table>
  <tr>
    <td>
      <h3>Alvin Zanua Putra</h3>
      <p><b>Web Developer</b> · Mahasiswa Teknik Informatika · Freelance software developer</p>
      <p>Tertarik pada software engineering, web development, game development, dan AI engineering.</p>
      <p>
        🐙 GitHub: <a href="https://github.com/alvinzanuaputra">@alvinzanuaputra</a><br>
        🌐 Website: <a href="https://www.alvinzanua.site">www.alvinzanua.site</a>
      </p>
    </td>
  </tr>
</table>

Terbuka untuk proyek freelance — silakan hubungi lewat website atau GitHub saya.

---

## Kontribusi

Menemukan perintah yang usang, typo, atau punya tips untuk dibagikan? Kontribusi sangat diterima:

1. Fork repository ini
2. Buat branch: `git checkout -b improve-docs`
3. Commit perubahanmu lalu buka Pull Request — atau cukup buka issue di repository ini

Jika panduan ini membantu, pertimbangkan memberi ⭐ pada repo dan membagikannya ke developer iOS lainnya.

---

<div align="center">

*Terakhir diperbarui: November 2026 · Ditulis untuk Claude Code 2.0.x*

Dibuat dengan ☕ oleh [@alvinzanuaputra](https://github.com/alvinzanuaputra) · [www.alvinzanua.site](https://www.alvinzanua.site)

</div>
