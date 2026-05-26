# assets/: Skill 01: Interviewer

This folder is reserved for assets that support the onboarding and profile-building experience.

---

## What Belongs Here

**Profile export templates**
If you want the exported `calibrated_candidate_profile.json` to render as something human-readable: a formatted PDF summary, a one-page profile card, or a structured Word document the user can review and correct: place the template file here and update the SKILL.md to reference it during export.

**Onboarding flow diagrams**
A visual map of the intake sequence can help contributors understand how the skill routes between topic areas and when it hands off to other skills. Place any flow diagrams here in SVG or PNG format.

**Localization files**
If you extend this skill to support languages other than English, place localized versions of the onboarding prompts and confirmation language here. Name them clearly: `onboarding_es.md`, `onboarding_fr.md`, and so on. Update SKILL.md to detect the user's language and load the appropriate file.

**Custom intake question sets**
Some deployments may want to add domain-specific intake questions: for example, questions specific to healthcare candidates, veterans transitioning to civilian roles, or candidates re-entering the workforce after a gap. Place those question set files here and reference them conditionally from SKILL.md based on the user's stated situation.

---

## What Does Not Belong Here

- Executable scripts: those go in a `scripts/` folder if you add one
- Reference documents used to guide Claude's writing: those go in `references/`
- Files larger than a few hundred KB: this package is designed to be lightweight and cloneable quickly

---

## Contributing

If you add an asset that meaningfully extends this skill for a broad audience, open a pull request with the file and a note in the PR description explaining what it does and when Claude should load it.
