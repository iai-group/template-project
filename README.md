# IAI Project Template

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
![Coverage Badge](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/vinaysetty/1cc32e6b43d911995bf07adb1cce4e89/raw/coverage.template-project.main.json)

This repository serves as a template for software projects.

# Testing and GitHub actions

Using `pre-commit` hooks, `flake8`, `black`, `mypy`, `docformatter`, and `pytest` are locally run on every commit. For more details on how to use `pre-commit` hooks see [here](https://github.com/iai-group/guidelines/tree/main/python#install-pre-commit-hooks).

Similarly, Github actions are used to run `flake8`, `black` and `pytest` on every push and pull request. The `pytest` results are sent to [CodeCov](https://about.codecov.io/) using their API for to get test coverage analysis. Details on Github actions are [here](https://github.com/iai-group/guidelines/blob/main/github/Actions.md).


# Setting up CI pipeline

This document provides step-by-step instructions for setting up a Continuous Integration (CI) pipeline, including configuring GitHub Actions and integrating a dynamic Gist badge.

---

## There are three CI files in this template repository.

1. `python-package-conda.yaml` this file contains the GitHub action workflow which runs every time someone pushes code to the remote branch.
2. `ci.yaml` this file contains the GitHub action workflow which runs when a pull request is created or updated. With dynamic coverage badge comparing the coverage to the main branch.
3. `merge.yaml` this file contains the GitHub action workflow which runs when a branch is merged to `main`.
## How to Set Up `GIST_SECRET` for GitHub Actions

### Step 1: Generate a Personal Access Token (PAT)
1. **Sign In to GitHub:**
   - Log in to your [GitHub account](https://github.com/).

2. **Navigate to Developer Settings:**
   - Click on your profile picture in the top-right corner.
   - Select **Settings** > **Developer settings** > **Personal access tokens** > **Tokens (classic)**.

3. **Generate a New Token:**
   - Click **Generate new token** (classic).
   - **Scopes to Select:**
     - Check the **`gist`** scope to allow access to your gists.

4. **Set Expiration (Optional):**
   - Choose a validity period for the token, or select **No expiration** for an indefinite token.

5. **Generate the Token:**
   - Click **Generate token**.
   - Copy the token displayed on the screen. (You won’t be able to see it again, so save it securely.)

---

### Step 2: Add `GIST_SECRET` to Your Repository
1. **Open Repository Settings:**
   - Navigate to the repository where you want to use the Gist.

2. **Go to Secrets and Variables:**
   - In the repository, click **Settings**.
   - Select **Secrets and variables** > **Actions**.

3. **Add a New Secret:**
   - Click **New repository secret**.
   - Set the **Name** as `GIST_SECRET`.
   - Paste the Personal Access Token (PAT) into the **Value** field.

4. **Save the Secret:**
   - Click **Add secret** to save.

---

### Step 3: Use `GIST_SECRET` in Your Workflow
In your GitHub Actions workflow, reference the `GIST_SECRET` as follows:

```yaml
- name: Create Gist Badge
  uses: schneegans/dynamic-badges-action@v1.7.0
  with:
    auth: ${{ secrets.GIST_SECRET }}
    gistID: your_gist_id_here
    filename: badge.json
    label: coverage
    message: 85%
    color: green

---

### Step 4: Create a GitHub Gist
1. **Sign In:** Log in to your [GitHub account](https://github.com/).
2. **Navigate to Gists:** Click on your profile picture in the top-right corner and select "Your gists" from the dropdown menu.
3. **Create a New Gist:** Click the "New gist" button.
4. **Add Content:**
   - **Description:** Provide a brief description of your gist.
   - **Filename:** Enter a filename, `coverage.${{ env.REPO_NAME }}.main.json`
   - **Content:** Paste the sample coverage data `{"schemaVersion":1,"label":"coverage","message":"36%","color":"red"}`
5. **Set Visibility:**
   - **Public Gist:** Visible to everyone and searchable.
   - **Secret Gist:** Not listed publicly but accessible via URL.
6. **Create Gist:** Click "Create public gist" or "Create secret gist" based on your preference.

### Step 5: Retrieve the Gist ID
After creating the gist, you'll be redirected to its page. The URL will look like this:
`https://gist.github.com/your_username/gist_id`

The `gist_id` is the unique identifier for your gist.

### Step 6: Integrate the Gist into Your Project
#### Markdown Embed
To embed the gist in your project's README or other documentation, use the following syntax:

```markdown
[![Gist Badge](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/your_username/gist_id/raw/badge.json)](https://gist.github.com/your_username/gist_id)

### Step 7: Update the CI yaml files 
* Replace the gist id in the CI files under `ci.yaml`, `merge.yaml` etc.