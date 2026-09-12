# Git

## What I Learned

Git is a distributed version control system used to track changes in files, collaborate on projects, and maintain the history of a codebase.

I focused on practical Git usage relevant to DevOps work, including local repositories, remote repositories, GitHub workflows, branching, commits, and troubleshooting.

## Topics Covered

- Git fundamentals
- Version control concepts
- Git repository
- Working tree and staging area
- Git initialization
- `git init`
- Checking repository status
- `git status`
- Adding files to the staging area
- `git add`
- Creating commits
- `git commit`
- Viewing commit history
- `git log`
- Git configuration
- `git config`
- Git user name and email configuration
- Git branches
- Creating and switching branches
- Merging branches
- GitHub
- Remote repositories
- `git remote`
- Connecting a local repository to GitHub
- `git clone`
- `git push`
- `git pull`
- Tracking remote branches
- GitHub authentication using a Personal Access Token
- `.gitignore`
- Git tags and releases
- Pull requests and code review
- Merge conflicts
- Resolving merge conflicts
- Git stash
- Git revert
- Git restore
- Git reset
- Git history recovery
- Interactive rebase
- Git cherry-pick

## Hands-On Practice

- Created and initialized Git repositories
- Configured Git user information
- Created commits and maintained commit history
- Checked repository status before committing
- Added files to the staging area
- Worked with branches and merging
- Connected local repositories to GitHub
- Cloned GitHub repositories
- Pushed local changes to GitHub
- Pulled changes from remote repositories
- Practiced GitHub authentication using a Personal Access Token
- Worked with `.gitignore`
- Practiced tags and releases
- Practiced pull requests and code review concepts
- Practiced resolving merge conflicts
- Practiced `stash`, `revert`, `restore`, and `reset`
- Practiced recovering Git history
- Practiced interactive rebase
- Used Git for documenting DevOps learning

## Key Concepts

### Git Repository

A Git repository stores the project files along with the complete version history tracked by Git.

### Working Tree

The working tree contains the files currently being edited in the local repository.

### Staging Area

The staging area contains changes selected for the next commit.

### Commit

A commit is a recorded snapshot of staged changes with a message describing the change.

### Branch

A branch is an independent line of development within a Git repository.

Branches allow developers to work on changes without directly modifying the main branch.

### Remote Repository

A remote repository is a Git repository hosted separately from the local repository.

GitHub is commonly used to host remote Git repositories.

### Git Push and Pull

`git push` sends local commits to a remote repository.

`git pull` retrieves changes from a remote repository and integrates them into the local branch.

### Merge Conflict

A merge conflict occurs when Git cannot automatically combine changes from different branches.

The conflicting files must be reviewed and the conflicts resolved before completing the merge.

### GitHub

GitHub provides remote repository hosting and collaboration features such as pull requests, code review, issues, and releases.

## DevOps Relevance

Git is a core tool in DevOps because it provides version control for:

- Application source code
- Infrastructure code
- Configuration files
- Shell scripts
- CI/CD pipelines
- Terraform configurations
- Kubernetes manifests
- Deployment files

Git also integrates with CI/CD tools to automatically build, test, and deploy changes.

A common DevOps workflow is:

Developer → Git → GitHub → CI/CD Pipeline → Build/Test → Deployment


