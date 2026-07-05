# Installation - audit-seo (ChatGPT / Codex skill)

This is the **Codex skill** packaging of `audit-seo`. It is a pure-instruction skill: Markdown doctrine files plus an `agents/openai.yaml` interface descriptor. **No Python, no dependencies, no network calls.**

## Prerequisites

- OpenAI Codex CLI installed and working.
- A terminal with `unzip` (macOS, Linux) or PowerShell with `Expand-Archive` (Windows).

## Installation

Codex discovers skills under its skills directory. Place the `audit-seo/` folder there so that `SKILL.md` sits at `<codex-skills-dir>/audit-seo/SKILL.md`.

### macOS / Linux

```bash
# 1. Create the Codex skills directory if it doesn't exist
mkdir -p ~/.codex/skills

# 2. Unzip the skill into that directory
unzip audit-seo.zip -d ~/.codex/skills/

# 3. Verify the structure
ls ~/.codex/skills/audit-seo/
# Expected: SKILL.md  README.md  INSTALL.md  agents/  checklists/  references/  templates/
```

### Windows (PowerShell)

```powershell
# 1. Create the Codex skills directory if it doesn't exist
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\skills"

# 2. Unzip the skill into that directory
Expand-Archive -Path .\audit-seo.zip -DestinationPath "$env:USERPROFILE\.codex\skills\"

# 3. Verify the structure
Get-ChildItem "$env:USERPROFILE\.codex\skills\audit-seo\"
# Expected: SKILL.md  README.md  INSTALL.md  agents  checklists  references  templates
```

> If your Codex installation reads skills from a different path, place the `audit-seo/` folder wherever your Codex config expects skills, keeping the folder structure intact.

## The interface descriptor

`agents/openai.yaml` declares how the skill surfaces in Codex:

```yaml
interface:
  display_name: "SEO Audit"
  short_description: "Audit web SEO from code"
  default_prompt: "Use $audit-seo to audit this web project for technical, metadata, content, and AI-search SEO issues."

policy:
  allow_implicit_invocation: true
```

`allow_implicit_invocation: true` lets Codex activate the skill automatically when a request matches its `description` (SEO audit / technical SEO review / metadata / robots / sitemap / structured data / Core Web Vitals / etc.).

## Verification

In a Codex session, ask:

> "Audit the SEO of this project."

The skill should activate, detect the stack, run through the checklists, and produce a structured report following `templates/report.md`. If implicit activation misses, invoke it explicitly:

> "Use the audit-seo skill to review this repo."

Or run the slash form if your Codex setup exposes it:

> "/audit-seo focus=metadata"

## Offline by design

Static analysis (stack detection, file inventory, checklist review, report generation) runs entirely from the project source with no network access. The **only** time the skill touches the network is the optional crawl step - and only against a URL you provide that is `localhost` or a domain you own (it asks for confirmation otherwise). If no URL is given, or no network is available, it limits itself to static analysis and says so in the report's "Limites de l'audit" section.

## Updating the skill

Replace the directory with the newer version:

```bash
# macOS / Linux
rm -rf ~/.codex/skills/audit-seo
unzip audit-seo.zip -d ~/.codex/skills/
```

## Uninstalling

```bash
# macOS / Linux
rm -rf ~/.codex/skills/audit-seo

# Windows (PowerShell)
Remove-Item -Recurse -Force "$env:USERPROFILE\.codex\skills\audit-seo"
```

## Troubleshooting

**Skill doesn't activate when expected.**
Activation depends on the `description` in `SKILL.md`'s front-matter and on `allow_implicit_invocation: true` in `agents/openai.yaml`. If your phrasing doesn't match, force it: "Use the audit-seo skill to...".

**The skill wants to crawl a URL.**
By design it only fetches a URL you explicitly pass, and only if it's `localhost` or a domain you own. It will ask for confirmation before crawling anything else. To stay fully offline, just don't pass a URL - the static audit runs on source alone.

**The report says many things "could not be verified."**
That's expected when no live URL, build, or analytics access is available. Static analysis flags anti-patterns in code; measured values (real status codes, Core Web Vitals, index coverage) require a crawl or external tooling, which the skill honestly lists as out of scope for that run.
