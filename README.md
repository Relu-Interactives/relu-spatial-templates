# Relu Spatial Templates

Hosting repository for **Relu Spatial project template archives**.

Template zips are stored as **GitHub Release assets** on this repo — that's why the
file tree is (intentionally) empty. Browse the
[**Releases**](../../releases) page to see every published template archive.

## How it works

```
Editor / Admin dashboard                Relu Spatial API                  This repo
────────────────────────                ────────────────                  ─────────
"Convert Project to Template"  ──zip──▶  POST /project-templates/  ──▶   Release  template-<uuid>
                                         upload-archive                   └─ asset: <name>.zip
                                                                              │
Desktop editor "Use Template"  ◀──────────── plain HTTPS download ◀──────────┘
```

1. A creator/admin converts a project to a template in the desktop editor (or
   uploads one via the admin dashboard). The Relu Spatial backend packs the
   upload into a **release** on this repo, tagged `template-<uuid>`, with the
   project zip attached as the release asset.
2. The template record in the Relu Spatial database points at the asset's
   direct download URL (`…/releases/download/<tag>/<file>.zip`).
3. When a user clicks **Use Template** in the desktop editor, the zip is
   downloaded over plain HTTPS and unpacked into a new local project —
   **no git installation required** on the user's machine.

## What's inside a template archive

Each zip is a complete Relu Spatial project:

- `.editor/project.json` — project metadata, **sanitized**: the
  `server_project_code` that links a project to its published server instance
  is stripped at pack time (and again at use time), so template users always
  start a brand-new project and can never publish over the original author's
  live project.
- `.editor/ecs-metadata.json` — the serialized ECS world/scene.
- `assets/` — models, images and other project assets.
- The ECS runtime scaffold (`package.json`, `index.html`, `main.ts`, …).

Excluded from every archive: `node_modules/`, `dist/`, `.git/` and stale build
zips.

## Managing templates

- **Publish / draft / edit / delete** templates from the
  [Relu Spatial admin dashboard](https://admin.reluspatial.com) → Templates.
- Releases here are **managed by the backend** — deleting a template in the
  dashboard deletes its release automatically. Please don't delete releases by
  hand; a release that's still referenced by a live template will break its
  "Use Template" download.
- Hand-curated templates are also possible: any publicly downloadable zip URL
  (including a GitHub repo's `archive/refs/heads/main.zip`) can be used as a
  template's `template_url` in the dashboard.

## Why GitHub Releases?

Free hosting with direct download URLs and up to 2 GB per asset — template
archives are often binary-heavy (3D models, textures), which makes them a poor
fit for a git file tree but a perfect fit for release assets.

---

© Relu Interactives — template archives are publicly downloadable; only
publish content you're happy to share.
