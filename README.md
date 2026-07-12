# Evening Rain Project Manager

Evening Rain is a personal blog of Muhammad Mustafa Monowar, featuring literary work, microblogs, blogs, media, and resources.

## Workspace Structure

```
evening-rain/
├── eveningrain-projectmanager/    # Project management, design, documentation
├── eveningrain/                   # Site development (HTML, CSS, dependencies)
└── eveningrain-contents/          # Content repository (Markdown files)
```

### Component Overview

| Component | Purpose |
|-----------|---------|
| **Project Manager** | Project planning, design system, documentation, scripting |
| **Site** | Website source code and assets |
| **Contents** | Blog posts, metadata, templates |

## Workflow

```
Project Manager → Site Development
Content Repository → Site Development
Site Development → GitHub (Serve)
```

## Project Manager Structure

```
eveningrain-projectmanager/
├── README.md                          # This file
├── DESIGN.md                          # Design system and color palette
├── AGENT.md                           # Agent instructions
└── project-manager/
    ├── tasks.md                       # Project tasks tracker
    ├── features.md                    # Feature documentation
    ├── issues.md                      # Issue tracking
    ├── logs.md                        # Activity logs
    ├── manual.md                      # Project manual
    ├── templates.md                   # Templates for features, issues, logs, tasks
    ├── refactoring-architecture.md    # Code architecture decisions
    └── design-system.md               # Design tokens and style sync
```

## Design System

**Fonts**: Source Serif 4 (headings), Source Sans 3 (body)

**Color Palette**:
| Name | Hex |
|------|-----|
| Black | `#090909ff` |
| Carbon Black | `#1a1f21ff` |
| Jet Black | `#272c2eff` |
| Iron Grey | `#404547ff` |
| Charcoal | `#585d5fff` |
| Grey | `#717677ff` |
| Grey Olive | `#909595ff` |
| Cool Steel | `#9ca1a1ff` |
| Platinum | `#eaefefff` |

## Task Status Markers

- `[ ]` Open
- `[/]` In-progress
- `[x]` Done
- `[!]` Bug
- `[-]` Abandoned
