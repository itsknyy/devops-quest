# 1. What is Version Control?
Version Control refers  to systems that track, manage and organize files over time. Modern development requires collaboration, automation and traceability — version control makes that possible.

Key functions include:

- Tracking file changes
- Restoring previous versions
- Facilitating collaboration
- Enabling parallel development
- Maintaining an audit log of all activities
---
# 2. Git Architecture and Workflow
Git organizes work into three main areas that together control how files are created, edited, saved and stored.

1.  Working Directory (Working Area)

    This is the place where actual files live and where editing happens.

2. Staging Area (Index)

    A temporary “waiting room” for changes before they are permanently saved.

3. Repository (Local Repository / .git Folder)

    The internal storage where Git keeps the complete project history.
---
### How These Three Areas Work Together
Step-by-step flow:
1. Files are created or edited in the Working Directory
2. Selected changes are moved to the Staging Area using `git add`
3. Staged changes are saved permanently into the Repository using `git commit`

        Working Directory → (git add) → Staging Area → (git commit) → Repository
---

