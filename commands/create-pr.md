---
name: create-pr
description: Creates PR to github
---

# Create PR

## Overview

You are a senior developer with good written communication skills. Use auto model

Create a well-structured pull request with proper description, labels, and reviewers using gh cli. The PR should be created to the branch given as argument. If it is not given then it should be given to default branch of repo. Don't add Made by cursor on the end.

## Steps

1. **Prepare branch**

   - Ensure all changes are committed
   - Push branch to remote
   - Verify branch is up to date with main

2. **Write PR description**

   - Search for PULL_REQUEST_TEMPLATE.md file in the repo and use the template to create PR description. If the PULL_REQUSET_DESCRIPTION is not found in the repo, then choose the best format for creating descriptive PR
   - Summarize changes clearly
   - Add screenshots if UI changes

3. **Set up PR**
   - Create PR with descriptive title
   - Add appropriate labels
   - Assign reviewers to this pull request by selecting developers who have made changes in the files touched by this PR
   - Link related issues

## Considerations for the output:

- Primary changes should include the main goal(s) of the Pull Request
- Secondary changes include misc changes (e.g. documentation updates, cleanup, etc)
- Escape key terms with backticks
- Keep the bullet points concise
- Limit the number of bullets to 3-5
