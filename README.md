# Git Externals

A lightweight tool to manage external Git repositories in your projects.

## Overview

Git Externals allows you to define, clone, and update Git repositories as externals (subfolders) in your project. It's a simpler alternative to Git submodules with an easy-to-use configuration format and automated update capabilities.

## Features

- **Simple Configuration**: Define externals in a `.gitexternals` file using a straightforward format
- **Efficient Cloning**: Uses shallow clones with blob filtering for faster downloads
- **Automatic Updates**: Updates all externals with a single command
- **Branch Support**: Specify which branch to clone for each external
- **Post-Clone Scripts**: Run custom scripts after cloning/updating
- **Git LFS Support**: Handle repositories with large binary files like images
- **Self-Update**: The script automatically checks for updates on every run
- **Git Hooks Integration**: Automatically update externals after git pull/merge operations
- **Clean Working Directory**: Git repositories are stored in `.git/externals` and only files are copied to the target paths

## Installation

1. Download [the script](init):

2. Make it executable:

```bash
chmod +x init
```

3. Optionally, move it to a location in your PATH:

```bash
sudo mv init /usr/local/bin/git-externals
```

### Dependencies

The script requires the following tools to be installed:

- `rsync` - For efficient file copying
- `git-lfs` - Only required for repositories with LFS support enabled

On most Linux distributions and macOS, rsync is already installed. Git LFS can be installed following [the official instructions](https://git-lfs.github.com/).

## Usage

### Basic Usage

Run the script without arguments to clone/update all externals:

```bash
./init
```

### Update the Script

Update the script to the latest version:

```bash
./init update
```

### Get Help

Show available commands:

```bash
./init help
```

### Install Git Hook

To automatically update externals whenever you pull or merge changes, install the post-merge hook:

```bash
./init install-hook
```

This will create a Git hook that runs the script after every `git pull` or `git merge` operation, ensuring your externals are always up to date.

## Configuration

### .gitexternals File

Create a `.gitexternals` file in the root of your repository with the following format:

```
[external "name"]
path = path/to/clone/repository
url = https://github.com/username/repo.git
branch = main
script = optional_script_to_run.sh
lfs = false
```

### Multiple Externals Example

```
[external "core-library"]
path = lib/core
url = https://github.com/company/core-lib.git
branch = stable

[external "ui-components"]
path = src/ui
url = https://github.com/company/ui-components.git
branch = develop
script = scripts/install-deps.sh

[external "art-assets"]
path = assets/art
url = https://github.com/company/art-assets.git
branch = main
lfs = true
```

### Git LFS Support

For repositories that use Git LFS (Large File Storage) to manage large binary files like images, 3D models, or audio files, enable the LFS support by adding `lfs = true` to the external configuration:

```
[external "assets-repo"]
path = Assets/Graphics
url = https://github.com/company/game-assets.git
branch = main
lfs = true
```

When LFS is enabled for an external:

1. The script will check if git-lfs is installed
2. Initialize LFS in the repository
3. Pull LFS objects to ensure large binary files are properly downloaded
4. Show warnings if git-lfs is not installed

This ensures that repositories with large binary assets are correctly handled.

## Self-Update Configuration

The script automatically checks for updates every time it runs, ensuring you always have the latest version without having to manually run the update command. You can also manually trigger an update with:

```bash
./init update
```

## How It Works

1. The script reads the `.gitexternals` file
2. For each external:
   - The actual Git repository is stored in `.git/externals/[name]`
   - If LFS is enabled, LFS objects are properly initialized and pulled
   - Files are copied from the cache to the target path (excluding the `.git` directory)
   - A `.gitexternal` file is created in the target directory to track the source
   - If a script is specified, it runs the script after cloning/updating
3. For self-update, it:
   - Automatically checks for updates on every run
   - Downloads the latest version from the default URL
   - Compares versions and updates if a newer version is available
4. When the post-merge hook is installed:
   - The script automatically runs after every git pull/merge
   - Keeps all externals in sync with minimal manual intervention

## Benefits of This Approach

1. **Clean Working Directory**: No `.git` directories in your project structure
2. **Space Efficiency**: Only one copy of each external repository
3. **Clear Indication**: The `.gitexternal` file makes it clear that files shouldn't be modified directly
4. **Easier Updates**: Updates are cleaner as you're not dealing with potential merge conflicts
5. **Read-Only Externals**: Prevents accidental commits to external repositories
6. **Always Up-to-Date**: Automatic update checks ensure you're always using the latest version
7. **Large Binary Support**: Properly handles repositories with Git LFS content

## Versioning

The script follows a `YYYY.M.PATCH` versioning scheme (e.g., `2025.3.3`).

## License

This project is available under the MIT License.

---

_Note: Git Externals is designed to be a simpler alternative to Git submodules. It's particularly useful for projects that need to include external repositories without the complexity of submodules._
