# Preparation

Clone this repo, checkout the `master` branch as well as the `gh-pages` branch via:

`
git fetch
git checkout gh-pages
git checkout master
`

The `gh-pages` branch is meant to be used via `git worktree` (available since v2.5.0),
so before starting on the page you should execute:

`git worktree add dist gh-pages`

This will set-up the `dist` folder in such a way that its contents are versioned under the `gh-pages` branch.

If git is complaining about the `dist` directory already existing, delete it and try again.

Next, you'll need to set-up the required resources in order to generate HTML from markdown.
We only include the instructions for Ubuntu here.

1. Install the prerequisites:

  * `sudo apt-get install ruby ruby-dev make gcc nodejs zlib1g-dev`
  * `sudo gem install jekyll --no-rdoc --no-ri`
  * `sudo gem install bundler --no-rdoc --no-ri`

2. Install additional dependencies:

  * `bundle install`
  
Now you will be able to execute `npm run gen-md`.  

# How to build

- Execute `npm install`, if you haven't done so yet
- Generate Markdown: `npm run gen-md`
- Build: `npm run build`

# How to deploy
- Build the website via `npm run build`
- Commit and push your changes on the `master` branch:
  ```
  git commit -m "<Messages describing your changes>"
  git push 
  ```
- Run `npm run publish`, which will commit all newly added and modified files within the `dist` folder with a generated commit message.
