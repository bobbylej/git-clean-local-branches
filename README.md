# 🧹 Branch Broomstick (Clean Local Git Branches Script)

## 🤔 Do You Suffer from Branch Clutter Syndrome? 🧹

Are your local Git branches multiplying like rabbits, leaving you overwhelmed and disorganized? Do you find yourself drowning in a sea of outdated branches, desperately seeking a lifeline? Fear not, for the "Branch Broomstick" is here to rescue you from the depths of branch chaos!

With just a flick of the `-f` switch, this trusty script will sweep away those pesky, forgotten branches that no longer grace the remote. Say goodbye to branch clutter and hello to a cleaner, more serene repository. Dive into the instructions below and let the tidying commence! 🚀✨

## Features

- Deletes local branches that are not present on the remote.
- Optionally force deletes branches using the `-f` or `--force` flag.

## Setup Instructions

Follow these steps to set up the script on your computer:

### 1. Save the Script

1. Open a terminal.
2. Create a directory for your scripts if it doesn't already exist:
   ```bash
   mkdir -p ~/bin
   ```
3. Save the script to a file named `clean-local-branches.sh` in the `~/bin` directory:
   ```bash
   nano ~/bin/clean-local-branches.sh
   ```
4. Copy and paste the following script into the file:

   ```bash
   #!/bin/bash

   # Default delete command
   delete_command="git branch -d"

   # Check for -f or --force flag
   while [[ "$#" -gt 0 ]]; do
       case $1 in
           -f|--force) delete_command="git branch -D"; shift ;;
           *) echo "Unknown parameter passed: $1"; exit 1 ;;
       esac
       shift
   done

   # Fetch the latest state of the remote branches
   git fetch --prune

   # Get a list of local branches
   local_branches=$(git branch --format='%(refname:short)')

   # Get a list of remote branches
   remote_branches=$(git branch -r --format='%(refname:short)' | sed 's/origin\///')

   # Loop through each local branch
   for branch in $local_branches; do
     # Check if the branch exists on the remote
     if ! echo "$remote_branches" | grep -q "^$branch$"; then
       # If not, delete the local branch
       echo "Deleting local branch: $branch"
       $delete_command "$branch"
     fi
   done
   ```

5. Save and exit the editor (in `nano`, press `CTRL + X`, then `Y`, and `Enter`).

### 2. Make the Script Executable

Run the following command to make the script executable:

```bash
chmod +x ~/bin/clean-local-branches.sh
```


### 3. Add the Script to Your PATH

Ensure that the `~/bin` directory is included in your `PATH`. Add the following line to your `~/.bashrc` or `~/.zshrc` file:

```bash
export PATH="$HOME/bin:$PATH"
```

Reload your shell configuration:

- For Bash:
  ```bash
  source ~/.bashrc
  ```

- For Zsh:
  ```bash
  source ~/.zshrc
  ```

### 4. Usage

You can now run the script from any directory:

- **Normal Deletion**: Deletes branches that are fully merged.
  ```bash
  clean-local-branches.sh
  ```

- **Force Deletion**: Forcefully deletes branches, regardless of their merge status.
  ```bash
  clean-local-branches.sh -f
  ```

  Or:

  ```bash
  clean-local-branches.sh --force
  ```

## Notes

- **Safety**: Always ensure that you have pushed any important changes to the remote before running this script, as it will permanently delete local branches that do not exist on the remote.
- **Remote Name**: The script assumes the default remote name is `origin`. Adjust the script if your remote has a different name.

Feel free to share this script with others to help them manage their local Git branches more efficiently!

## ⚠️ Disclaimer

**Note:** The author of this script takes no responsibility for any unpushed changes that may be lost during the branch cleaning process. Please ensure that all important changes are safely pushed to the remote before using this script. Happy cleaning!