<p align="center">
  <img src="https://user-images.githubusercontent.com/672120/231150250-7a45ff99-b424-4a69-b70a-7d6267f8d89e.jpg" alt="Git Graph preview" />
</p>

# Git Graph extension for Visual Studio Code

[![Visual Studio Marketplace Version](https://img.shields.io/visual-studio-marketplace/v/mhutchie.git-graph)](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
[![Visual Studio Marketplace Downloads](https://img.shields.io/visual-studio-marketplace/d/mhutchie.git-graph)](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
[![License](https://img.shields.io/github/license/mhutchie/vscode-git-graph)](LICENSE)

Git Graph is an open-source Visual Studio Code extension that visualizes Git history as an interactive graph and lets you perform common Git actions from a single view.

**Key benefits**
* Understand branch and merge history at a glance.
* Review commits, diffs, and file changes faster.
* Work efficiently with medium and large repositories using performance-focused options.

![Recording of Git Graph](https://github.com/mhutchie/vscode-git-graph/raw/master/resources/demo.gif)

## Quick Start

### Requirements

* Visual Studio Code 1.38+.
* Git installed and available on your PATH (or configured via `git.path`).

### Install

Install Git Graph from the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph).

### Open Git Graph

Open the Command Palette (`Ctrl/Cmd + Shift + P`) and run **Git Graph: View Git Graph**.

## Features

### Graph & history visualization

* Interactive graph with local/remote branches, tags, and stashes.
* Hover tooltips that show branch/tag/stash inclusion and HEAD ancestry.
* Filter which branches are displayed using the Branches dropdown.

### Commit inspection & comparison

* Commit details with file change lists, diffs, and file open actions.
* Compare two commits and review diffs across the selected range.
* Code review markers for tracking reviewed files.

### Git actions from the UI

* Create, checkout, rename, delete, merge, rebase, reset, fetch, pull, and push branches.
* Add, delete, and push tags (annotated or lightweight).
* Stash actions: apply, pop, drop, and create branch from stash.
* Open files, copy hashes and ref names, and view annotated tag details.

### Integrations & workflow helpers

* Issue linking in commit messages.
* Pull request creation with GitHub, GitLab, and Bitbucket providers (plus custom providers).
* Repository settings widget for remotes and integrations.

## Keyboard Shortcuts

Common shortcuts in the Git Graph view:

* `Ctrl/Cmd + F`: Open the Find widget.
* `Ctrl/Cmd + H`: Center the view on HEAD.
* `Ctrl/Cmd + R`: Refresh the view.
* `Ctrl/Cmd + S`: Jump to the next stash.
* `Ctrl/Cmd + Shift + S`: Jump to the previous stash.

When the Commit Details View is open:

* `Up` / `Down`: Open details for the commit above/below.
* `Ctrl/Cmd + Up` / `Ctrl/Cmd + Down`: Open details for parent/child commit.

## Settings

Git Graph offers a wide range of configuration options for graph style, performance, and UX. Full documentation is available in the [Extension Settings wiki](https://github.com/mhutchie/vscode-git-graph/wiki/Extension-Settings).

**Common settings**

* `git-graph.graph.style`: Rounded or angular graph lines.
* `git-graph.repository.commits.initialLoad`: Number of commits loaded initially.
* `git-graph.repository.showRemoteBranches`: Show/hide remote branches by default.
* `git-graph.repository.fetchAndPrune`: Prune stale refs when fetching.
* `git-graph.commitDetailsView.location`: Inline or docked details view.

**Examples**

```json
{
  "git-graph.repository.commits.initialLoad": 500,
  "git-graph.repository.commits.loadMore": 200,
  "git-graph.repository.fetchAndPrune": true
}
```

```json
{
  "git-graph.graph.style": "angular",
  "git-graph.commitDetailsView.location": "Docked to Bottom"
}
```

This extension also consumes `git.path` to locate a portable Git installation.

## Commands

| Command | Description |
| --- | --- |
| `git-graph.view` | Open Git Graph. |
| `git-graph.fetch` | Open Git Graph and fetch from remotes. |
| `git-graph.addGitRepository` | Add a Git repository to Git Graph. |
| `git-graph.removeGitRepository` | Remove a Git repository from Git Graph. |
| `git-graph.clearAvatarCache` | Clear cached avatars. |
| `git-graph.endAllWorkspaceCodeReviews` | End all code reviews in the workspace. |
| `git-graph.endSpecificWorkspaceCodeReview` | End a specific code review in the workspace. |
| `git-graph.resumeWorkspaceCodeReview` | Resume a specific code review in the workspace. |
| `git-graph.version` | Show Git Graph version information. |

## Performance Tips

* Increase `git-graph.repository.commits.initialLoad` only if needed.
* Enable `git-graph.repository.commits.loadMoreAutomatically` for smoother scrolling.
* Disable `git-graph.repository.showUncommittedChanges` in very large repos if load time is slow.
* Use `git-graph.repository.onlyFollowFirstParent` to simplify history in large merge-heavy repos.

## FAQ / Troubleshooting

**Git Graph shows no repositories**
* Make sure the folder you opened in VS Code is a Git repository.
* Check `git.path` if you are using a portable Git installation.

**The graph is slow on large repositories**
* Reduce the initial commit load and disable uncommitted changes in settings.
* Use branch filters to limit what is shown.

**Issue linking or PR creation is not working**
* Configure the repository settings widget or custom PR provider in settings.

## Contributing

Contributions are welcome. Please read the [contributing guide](CONTRIBUTING.md) and review the [code of conduct](CODE_OF_CONDUCT.md) before opening issues or pull requests.

## Como desarrollador

Guía rápida para contribuir desde el repositorio:

* Instalar dependencias: `npm install`
* Compilar: `npm run compile`
* Lint: `npm run lint`
* Tests: `npm test`

## License

This project is licensed under the terms of the [LICENSE](LICENSE).

## Acknowledgements

Thank you to all of the contributors that help with the development of Git Graph.

Some of the icons used in Git Graph are from the following sources, please support them for their excellent work.

* [GitHub Octicons](https://octicons.github.com/) ([License](https://github.com/primer/octicons/blob/master/LICENSE))
* [Icons8](https://icons8.com/icon/pack/free-icons/ios11) ([License](https://icons8.com/license))
