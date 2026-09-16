# How to publish this as your GitHub repository

Delete this file before your final commit — it is setup guidance, not a deliverable.

## 1. Create the repository on GitHub

Go to https://github.com/new

- **Repository name:** `cybersecurity-task1-security-assessment`
- **Description:** `Foundational cybersecurity assessment of a small organization — assets, threats, CIA triad, network architecture, protocols and endpoint hardening`
- **Visibility:** Public (so your reviewer can open the link without a login)
- **Do NOT** tick "Add a README" — this repo already has one

## 2. Push from your machine

Open a terminal in the unzipped folder and run:

```bash
git init
git add .
git commit -m "Task 1: foundational cybersecurity assessment"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/cybersecurity-task1-security-assessment.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username.

## 3. Alternative — no terminal required

On the new empty repository page, click **"uploading an existing file"**, then drag the
`README.md`, `LICENSE`, `.gitignore`, `docs/` folder and `diagrams/` folder into the browser.
GitHub preserves the folder structure. Add a commit message and click **Commit changes**.

## 4. Verify before you submit

- [ ] `README.md` renders on the repository landing page
- [ ] The two Mermaid diagrams in `docs/04-network-architecture.md` render as pictures, not code blocks
- [ ] `diagrams/network-diagram.svg` opens and displays correctly
- [ ] Every link in the README's "Deliverables map" table works
- [ ] Repository is **Public** — open the URL in a private browser window to confirm
- [ ] You have deleted this `PUSH-TO-GITHUB.md` file

## 5. Optional polish

- Add repository topics: `cybersecurity`, `security-assessment`, `risk-assessment`,
  `network-security`, `nist-csf`, `cis-controls`
- Add a short "About" description in the repository sidebar

## 6. Submit

Paste the repository URL into the LMS. Nothing else is required —
no PDF, Word file or presentation.

```
https://github.com/YOUR-USERNAME/cybersecurity-task1-security-assessment
```
