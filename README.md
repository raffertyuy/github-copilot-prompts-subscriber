# github-copilot-prompts-subscriber
This repo features a github actions workflow `pull-copilot-prompts.yml` which pulls the latest custom prompt files from a [shared/subscribed repo](https://github.com/raffertyuy/github-copilot-prompts).

This action was created through vibe coding. Here is my initial prompt

```markdown
Create a new github actions workflow called `pull-copilot-prompts.yml`.
This workflow copies the *.md files from the publisher git repo's .github folder into the .github folder of this repository. (source=publisher git repo, destination=this repo). The changes are done in a new branch, with a pull request. If there are no new/changed files, no branch/pull request is created.

Trigger: every monday 12am and on workflow_dispatch

Logic:
- clones the source git repo using:
  - GIT URL in actions variable `PUBLISHER_GIT_URL`. This URL is going to be in the form of `http(s)://URL/repo.git`
  - PAT token in actions secret `PAT`
- compare the /.github/**/*.md files from the source repo with the destination repo
  - if there every file are the same/equal, then the GitHub actions workflow is complete.
  - else (if unequal files - diff files or files that don't exist in the destination repo), then
    - create a new branch in the destination repo
    - copy all *.md files found in the `/.github` folder (and subfolders) into the corresponding `/.github` folder of this repo.
    - if the folder doesn't exist, create it
    - if the .md file exists, replace it. 
    - using the same PAT secret
      - git add the changes to this destination repo
      - git commit with a descriptive commit message and push
      - submit a pull request with a descriptive PR message
```