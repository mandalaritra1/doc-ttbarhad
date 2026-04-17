# Agent Instructions For TTbarHad Zensical Docs

This repository is the static Zensical documentation site for the ttbar all-hadronic analysis.

## Site Scope

- Use Zensical only.
- Source markdown lives in `docs/`.
- Generated output lives in `site-zensical/`.
- Build with `.venv-mkdocs-material/bin/zensical build`.
- `mkdocs.yml` and `site/` are deprecated and should not be recreated.
- Do not run `zensical build` for Markdown-only content edits because it can disrupt the current local serve.
- Keep navigation explicit in `zensical.toml` so dated meeting pages do not clutter the sidebar.

## Source Of Truth

- Use `/mnt/8A04C21E04C20CDF/wsLinux/ai-wiki` for durable analysis context.
- Use `/mnt/8A04C21E04C20CDF/wsLinux/research-notes/meetings/ttbarhad` for meeting progress.
- For meeting updates, read only `research-notes/meetings/ttbarhad/**` and, if needed, `research-notes/projects/ttbarhadronic.md`.
- Do not use `research-notes/meetings/zjet/**` for this site.
- Treat `research-notes` and `ai-wiki` as read-only sources unless Aritra explicitly asks to edit them.

## Meeting Import Workflow

1. Find meeting source files under:
   `/mnt/8A04C21E04C20CDF/wsLinux/research-notes/meetings/ttbarhad/YYYY/MM/meeting_YYYY_MM_DD_*.md`
2. Create or update one site page per meeting:
   `docs/meetings/YYYY-MM-DD.md`
3. Keep `docs/meetings/recents.md` concise:
   recent meetings, active action items, and a link to the archive.
4. Keep `docs/meetings/all.md` as the full archive:
   group meetings by year and month using HTML `<details>` blocks.
5. Summarize meeting content for presentation tracking.
   Do not copy long raw notes or personal debugging detail into the site.
6. If source notes contain plots, summarize what the plots were used for.
   Only copy or embed images when Aritra explicitly asks for figures to be included.

## Public-Facing Tone

- Keep pages presentation-ready and collaboration-facing.
- Prefer concise tables, decisions, action items, and status.
- Avoid personal debugging narratives unless they are needed to explain an analysis decision.
