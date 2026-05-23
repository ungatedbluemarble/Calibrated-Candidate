# Contributing to Interview Ready

Thank you for being here. Every contribution to this project serves one purpose: making career preparation accessible to people who do not have access to expensive coaches, insider networks, or professional connections. Keep that in mind as you contribute.

---

## What We Are Looking For

Interview Ready is built on Claude skills. The core skills are functional. What the community can add is depth, coverage, and reach that the initial build could not cover alone.

The highest-value contributions right now are:

**Reference files and assets** for existing skills. Each skill has a `references/` and `assets/` folder with a README explaining exactly what belongs there. These are the easiest and most impactful place to start. Examples: industry-specific question banks for Skill 05, intake extensions for specific candidate populations in Skill 01, JD evaluation examples for Skill 03.

**Bug reports and quality issues.** If a skill produces output that is wrong, misleading, or harmful, that matters more than any feature addition. Use the bug report issue template.

**Skill extensions for underserved populations.** Career changers, veterans, candidates with employment gaps, international job seekers, and candidates with disabilities are all underserved by the current package. Extensions that serve these populations are a priority.

**Platform ports.** A Gemini Gem port of any or all skills is on the roadmap. If you have Gemini development experience, that is a high-value contribution.

---

## What We Are Not Looking For

- Features that significantly increase token consumption without proportional value to the job seeker
- Contributions that make the package dependent on paid third-party services
- Commercial extensions or integrations (these violate the CC BY-NC 4.0 license)
- Generic improvements that are not grounded in a specific user problem

---

## How to Contribute

### Step 1: Open an issue first

Before writing any code or creating any files, open an issue using the appropriate template:

- **Bug report** for something that is not working correctly
- **Feature request** for a new capability or improvement
- **Contribution** for a reference file, asset, or extension you want to add

This gives the maintainer a chance to confirm the contribution is aligned before you invest time building it.

### Step 2: Fork the repo

Fork `ungatedbluemarble/interview-ready` to your own GitHub account. Make your changes in your fork.

### Step 3: Follow the file standards

**No em dashes anywhere.** This is a hard rule across the entire package. Em dashes render as corrupted characters in some environments. Use commas, colons, or restructure the sentence.

**No real names.** All reference files and examples must use John Doe (male) or Jane Doe (female) as placeholder names. No plausible real names.

**No personal data.** Reference files must not contain real user profiles, real job descriptions from identifiable companies, or any personally identifiable information.

**Skill schema awareness.** Any file that references the user profile must use the field names defined in `references/user-profile-schema.md` in the relevant skill folder. Do not invent new fields without opening an issue to discuss the schema change first.

**License compliance.** All contributions must be your original work and are released under CC BY-NC 4.0 on submission. You must confirm this in the contribution issue template before submitting a PR.

### Step 4: Submit a pull request

Use the PR template provided in this repo. Fill in every section. Incomplete PRs will be returned for more information before review begins.

---

## Review Process

The maintainer reviews all PRs. Review time varies. Be patient.

PRs that will be reviewed faster:

- Reference to an open issue in the PR description
- Clear explanation of what changed and why
- Evidence of testing (even informal)
- Clean files with no em dashes, no real names, no personal data

PRs that will be closed without review:

- No corresponding issue
- Changes to core SKILL.md files without prior discussion
- Violations of the CC BY-NC 4.0 license
- Content that could harm job seekers

---

## Questions

Open a discussion in the Discussions tab. Someone will respond.
