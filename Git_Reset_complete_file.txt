# Step-by-Step Guide to Git Reset with Examples

## Introduction

The `git reset` command is a powerful tool in Git that allows you to undo changes in your repository. It can modify the commit history and alter the state of your working directory and staging area. This command is often considered destructive because it can discard commits, which can lead to the loss of changes if not used carefully. In this guide, we will explore how to use `git reset` with its three primary modes: `--soft`, `--mixed`, and `--hard`.

## Understanding Git Reset Modes

- **`--soft`**: This mode resets the commit history but keeps changes in the staging area (index). You can recommit these changes if needed.
- **`--mixed`** (default): This mode resets the commit history and the staging area but keeps changes in your working directory. It allows you to modify and recommit them.
- **`--hard`**: This mode resets the commit history, staging area, and working directory. All changes are discarded, making it the most destructive option.

## Step 1: Prepare Your Environment

Before performing a reset, ensure that your local repository is synchronized with the remote repository. This can be done using the `git pull` command.

```bash
git pull
```

This command fetches and merges changes from the remote repository into your local branch.

## Step 2: View the Commit History

To understand which commit you want to reset to, you need to view your repository’s commit history using the `git log` command.

```bash
git log
```

This will display a list of commits, with each commit identified by a SHA-1 hash. Identify the commit you want to reset to and note its hash.

## Step 3: Perform a Mixed Reset (Default)

Let’s say you want to reset your repository to a specific commit, keeping changes in your working directory but removing them from the staging area.

```bash
git reset <commit_hash>
```

Example:

```bash
git reset fd0c8d3
```

After running this command:
- The commit history will be reset to the specified commit.
- Changes in commits after this commit will be kept in your working directory but unstaged.

To verify the changes:

```bash
git status
```

You will see the files in the working directory as unstaged.

## Step 4: Perform a Soft Reset

If you want to keep the changes both in the working directory and the staging area while resetting the commit history, use the `--soft` option.

```bash
git reset --soft <commit_hash>
```

Example:

```bash
git reset --soft fd0c8d3
```

After running this command:
- The commit history will be reset to the specified commit.
- Changes in the working directory remain staged.

You can immediately recommit these changes if needed:

```bash
git commit -m "Recommitted changes after soft reset"
```

## Step 5: Perform a Hard Reset

For a complete reset, where all changes are discarded and your repository is restored to a previous state, use the `--hard` option.

```bash
git reset --hard <commit_hash>
```

Example:

```bash
git reset --hard fd0c8d3
```

After running this command:
- The commit history, staging area, and working directory are all reset to the specified commit.
- All changes after that commit are lost.

To verify the reset:

```bash
git status
```

You should see that there are no changes in your working directory or staging area.

## Step 6: Reset by a Specific Number of Commits

You can also reset by specifying the number of commits to go back.

```bash
git reset HEAD~<number_of_commits>
```

Example:

```bash
git reset HEAD~5
```

This command resets the last five commits and keeps the changes in the working directory (mixed mode by default).

## Step 7: Synchronize with Remote Repository

If you accidentally perform a reset on a public branch and wish to revert the local changes, you can synchronize your local repository with the remote repository.

```bash
git reset --hard <commit_hash>
git pull
```

This will reset your local branch to match the remote branch.

## Conclusion

The `git reset` command is a powerful tool that can help you undo changes and clean up your commit history. However, it is crucial to use it carefully, especially on public branches, to avoid losing important work. By following this step-by-step guide, you should be able to safely and effectively use `git reset` in your Git workflow.


### A step-by-step guide on how to use each of the `git reset` modes (`--soft`, `--mixed`, and `--hard`) with examples from scratch.

---

## **Example Setup**

Let's first create a new Git repository and make some commits to demonstrate the `git reset` command.

### **Step 1: Initialize a New Git Repository**

```bash
mkdir git-reset-demo
cd git-reset-demo
git init
```

### **Step 2: Create an Initial File and Commit**

```bash
echo "Initial content" > file.txt
git add file.txt
git commit -m "Initial commit"
```

### **Step 3: Make Additional Commits**

Let's create a few more commits.

```bash
echo "Change 1" >> file.txt
git add file.txt
git commit -m "Added change 1"

echo "Change 2" >> file.txt
git add file.txt
git commit -m "Added change 2"

echo "Change 3" >> file.txt
git add file.txt
git commit -m "Added change 3"
```

At this point, your commit history looks like this:

```bash
git log --oneline
```

Output:
```
<commit_hash3> Added change 3
<commit_hash2> Added change 2
<commit_hash1> Added change 1
<commit_hash0> Initial commit
```

---

## **Example 1: Git Reset with `--soft`**

### **Scenario:**
You want to undo the last commit but keep the changes staged so you can recommit them.

### **Step 1: Perform a Soft Reset**

```bash
git reset --soft HEAD~1
```

This command will reset the repository to the commit before the last one (`HEAD~1`).

### **Step 2: Check the Status and Log**

```bash
git status
```

Output:
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   file.txt
```

```bash
git log --oneline
```

Output:
```
<commit_hash2> Added change 2
<commit_hash1> Added change 1
<commit_hash0> Initial commit
```

The last commit (`"Added change 3"`) is gone from the history, but the changes are still staged and ready to be recommitted.

### **Step 3: Recommit the Changes**

```bash
git commit -m "Recommitted change 3 after soft reset"
```

---

## **Example 2: Git Reset with `--mixed` (Default)**

### **Scenario:**
You want to undo the last commit and keep the changes in the working directory but unstaged.

### **Step 1: Perform a Mixed Reset**

```bash
git reset HEAD~1
```

This command will reset the repository to the commit before the last one (`HEAD~1`) and unstages the changes.

### **Step 2: Check the Status and Log**

```bash
git status
```

Output:
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   file.txt
```

```bash
git log --oneline
```

Output:
```
<commit_hash2> Added change 2
<commit_hash1> Added change 1
<commit_hash0> Initial commit
```

The last commit (`"Added change 3"`) is gone from the history, and the changes are now in your working directory but not staged.

### **Step 3: Recommit the Changes**

If you want to recommit these changes, you first need to stage them again:

```bash
git add file.txt
git commit -m "Recommitted change 3 after mixed reset"
```

---

## **Example 3: Git Reset with `--hard`**

### **Scenario:**
You want to completely undo the last commit and discard the changes in the working directory.

### **Step 1: Perform a Hard Reset**

```bash
git reset --hard HEAD~1
```

This command will reset the repository to the commit before the last one (`HEAD~1`), and it will also discard any changes made in the last commit.

### **Step 2: Check the Status and Log**

```bash
git status
```

Output:
```
On branch main
nothing to commit, working tree clean
```

```bash
git log --oneline
```

Output:
```
<commit_hash2> Added change 2
<commit_hash1> Added change 1
<commit_hash0> Initial commit
```

The last commit (`"Added change 3"`) is gone from the history, and the changes are completely discarded from your working directory and staging area.

---

## **Example 4: Git Reset by a Specific Number of Commits**

### **Scenario:**
You want to reset the last two commits, keeping changes in the working directory.

### **Step 1: Perform a Mixed Reset by Number of Commits**

```bash
git reset HEAD~2
```

This command will reset the last two commits and keep the changes in the working directory.

### **Step 2: Check the Status and Log**

```bash
git status
```

Output:
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   file.txt
```

```bash
git log --oneline
```

Output:
```
<commit_hash1> Added change 1
<commit_hash0> Initial commit
```

The last two commits (`"Added change 3"` and `"Added change 2"`) are gone, but the changes are present in the working directory.

### **Step 3: Recommit the Changes**

If needed, stage and recommit the changes:

```bash
git add file.txt
git commit -m "Recommitted changes after resetting last two commits"
```

---

## **Conclusion**

The `git reset` command is an essential tool for managing your commit history and modifying your repository's state. Use the `--soft`, `--mixed`, and `--hard` options depending on how you want to handle the changes in your working directory and staging area. Always be cautious, especially when using the `--hard` option, as it can lead to data loss if not used carefully.