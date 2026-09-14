# Create and Upload the GitHub Repository

## Method 1 — GitHub website

1. Extract the downloaded ZIP file.
2. Rename `RENAME_TO_AIML_GROUP_ID` using your official Group ID.
3. Sign in to GitHub and select **New repository**.
4. Enter a repository name such as `AIML-Stock-Market-GROUP-ID`.
5. Select **Private** unless the lecturer requests a public repository.
6. Do not create another README, `.gitignore`, or licence on GitHub.
7. Select **Create repository**.
8. Select **uploading an existing file**.
9. Drag the contents of the extracted group folder into the upload area.
10. Use the commit message `Initialize AIML project repository structure`.
11. Select **Commit changes**.

The `.gitkeep` files allow GitHub to preserve folders that are currently empty.

## Method 2 — Git commands

Open Git Bash or the VS Code terminal inside the renamed group folder:

```bash
git init
git add .
git commit -m "Initialize AIML project repository structure"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

Replace the URL with the URL of the empty repository created on GitHub.

## Recommended later commits

```text
Add raw stock-market dataset
Add Member 1 missing-data preprocessing notebook
Add categorical encoding notebook
Add outlier-handling notebook
Add feature-scaling notebook
Add feature-engineering notebook
Add feature-selection notebook
Integrate group preprocessing pipeline
Add EDA figures and preprocessing logs
Finalize Progress Review I documentation
```

