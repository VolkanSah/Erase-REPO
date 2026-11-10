# Erase REPO: Clean Slate Workflow

###### My first pure Vibecoding project is crafted on shit, but is not shit... or isn't it?

*Sometimes, just sometimes...*

### Scenario

* **The Friction:** You can't log in with SSH, you don't want to delete the whole repository, and web-based workflows suck. The absolute worst is sitting in the "ceramic department", being **abnormally lazy**, and wanting to upload fresh code without dealing with CLI overhead.
* **The Auth Nightmare:** Crafting PGP keys, debugging SSL/SSH keys, auditing accounts, and messing around with `GITHUB_TOKEN` permissions in a locked environment? WTF!
* **The AI Battle:** Gemini kept trying to be overly smart with dangerous `rm -rf` scripts and complex token setups that almost blew up the environment. I literally had to kick its ass 3 times until it finally came up with the `git read-tree --empty` solution.
* **The Pure Vibe Solution:** A single-button workflow triggered straight from your phone while on the toilet. Zero local Git commands, zero SSH keys, pure automation.
* **The Result:** Completely wipes your repository branches clean in seconds while strictly keeping your whitelisted core files intact!

```yaml
name: Clean Slate (Custom Keep List)

on:
  workflow_dispatch:

permissions:
  contents: write

jobs:
  wipe-repo:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Rebuild Clean Branch with Whitelist
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          
          # 1. Create an orphan branch with no history
          git checkout --orphan temp-clean-slate
          
          # 2. Empty the Git index (files on disk stay untouched)
          git read-tree --empty
          
          # =================================================================
          # WHITELIST: Define what should be preserved.
          # Comment out unused lines with '#'!
          # =================================================================
          
          # Core / Infrastructure
          [ -e .github ] && git add .github/
          [ -e README.md ] && git add README.md
          [ -e .gitignore ] && git add .gitignore
          [ -e .gitattributes ] && git add .gitattributes
          
          # Documentation & Legal
          [ -e LICENSE ] && git add LICENSE
          [ -e SECURITY.md ] && git add SECURITY.md
          [ -e STATUS.md ] && git add STATUS.md
          [ -e CONTRIBUTING.md ] && git add CONTRIBUTING.md
          [ -e CODE_OF_CONDUCT.md ] && git add CODE_OF_CONDUCT.md
          [ -e CITATION.cff ] && git add CITATION.cff
          
          # Custom Additions 
          # [ -e .pyfun ] && git add .pyfun
          # =================================================================
          
          # 3. Create a clean commit
          git commit -m "style: reset repository keeping whitelisted core metadata"
          
          # 4. Replace the local main branch with the clean branch
          git branch -D main
          git branch -m main

      - name: Force Push to Main
        run: |
          git push origin main --force

```

---

### Credits

*Credits to annoying Gemini (who wanted to `rm` everything and overcomplicate tokens) and the shit-lazy Volkan Sah on the throne!* 🥔⚡️🍺
#FUCKBORDERS! 
