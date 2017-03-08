# Prep

Checkout the `master` branch as well as the `gh-pages` branch. The `gh-pages` branch is meant to be used 
via `git worktree` (available since v2.5.0), so before starting on the page you should execute:

`git worktree add dist gh-pages`

This will set-up the `dist` folder in such a way that its contents are versioned under the `gh-pages` branch.

# How to build 

- Execute `npm install`
- Generate Markdown: `npm run gen-md`
- Build: `npm run build`

# How to deploy
- Build the website via `npm run build`
- Commit and push your changes on the `master` branch:
  ```
  git commit -m "<Messages describing your changes>"
  git push 
  ```
- Run `npm run publish`, which will commit all newly added and modified files within the `dist` folder with the 
  most recent commit message of the `website` branch