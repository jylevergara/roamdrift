# Creating Git Commits with Specific Dates and Times

## Overview

Git allows you to set custom dates for commits using environment variables or command-line options. There are two types of dates in Git commits:

1. **Author Date**: When the changes were originally made
2. **Commit Date**: When the commit was created/applied

## Methods to Set Custom Dates

### Method 1: Using Environment Variables

#### Set Author Date

```bash
# Set author date for next commit
export GIT_AUTHOR_DATE="2024-01-15 14:30:00"
git commit -m "Your commit message"

# Or in one line
GIT_AUTHOR_DATE="2024-01-15 14:30:00" git commit -m "Your commit message"
```

#### Set Commit Date

```bash
# Set commit date for next commit
export GIT_COMMITTER_DATE="2024-01-15 14:30:00"
git commit -m "Your commit message"

# Or in one line
GIT_COMMITTER_DATE="2024-01-15 14:30:00" git commit -m "Your commit message"
```

#### Set Both Dates

```bash
# Set both author and commit dates
GIT_AUTHOR_DATE="2024-01-15 14:30:00" GIT_COMMITTER_DATE="2024-01-15 14:30:00" git commit -m "Your commit message"
```

### Method 2: Using Git Commit Options

#### Set Author Date with --date

```bash
git commit --date="2024-01-15 14:30:00" -m "Your commit message"
```

Note: The `--date` option only sets the author date, not the commit date.

## Date Formats Supported

Git accepts various date formats:

### ISO 8601 Format (Recommended)

```bash
# Full ISO format
git commit --date="2024-01-15T14:30:00" -m "Message"

# With timezone
git commit --date="2024-01-15T14:30:00-05:00" -m "Message"
```

### Human-Readable Formats

```bash
# Various human-readable formats
git commit --date="Jan 15 2024 2:30 PM" -m "Message"
git commit --date="2024-01-15 14:30" -m "Message"
git commit --date="15 Jan 2024 14:30:00" -m "Message"
```

### Relative Dates

```bash
# Relative to current time
git commit --date="2 days ago" -m "Message"
git commit --date="1 week ago" -m "Message"
git commit --date="3 hours ago" -m "Message"
```

### Unix Timestamps

```bash
# Unix timestamp
git commit --date="1705339800" -m "Message"
```

## Practical Examples

### Example 1: Backdating a Commit

```bash
# Create a commit as if it was made yesterday at 3 PM
git add .
git commit --date="yesterday 15:00" -m "Fix bug in user authentication"
```

### Example 2: Creating Multiple Commits with Sequential Dates

```bash
# First commit
git add file1.txt
GIT_AUTHOR_DATE="2024-01-10 09:00:00" git commit -m "Initial implementation"

# Second commit (next day)
git add file2.txt
GIT_AUTHOR_DATE="2024-01-11 10:30:00" git commit -m "Add validation"

# Third commit
git add file3.txt
GIT_AUTHOR_DATE="2024-01-12 14:15:00" git commit -m "Update documentation"
```

### Example 3: Amending an Existing Commit's Date

```bash
# Change the date of the last commit
git commit --amend --date="2024-01-15 16:45:00"

# Change both author and commit dates of last commit
GIT_AUTHOR_DATE="2024-01-15 16:45:00" GIT_COMMITTER_DATE="2024-01-15 16:45:00" git commit --amend --no-edit
```

## Advanced Scenarios

### Rewriting History with Custom Dates

If you need to change dates for multiple commits, use `git rebase`:

```bash
# Interactive rebase to edit multiple commits
git rebase -i HEAD~3

# In the editor, change 'pick' to 'edit' for commits you want to modify
# Then for each commit:
GIT_AUTHOR_DATE="desired-date" GIT_COMMITTER_DATE="desired-date" git commit --amend --no-edit
git rebase --continue
```

### Using git filter-branch (for extensive history rewriting)

```bash
# WARNING: This rewrites history - use with caution
git filter-branch --env-filter '
if [ $GIT_COMMIT = "commit-hash-to-change" ]; then
    export GIT_AUTHOR_DATE="2024-01-15 14:30:00"
    export GIT_COMMITTER_DATE="2024-01-15 14:30:00"
fi
' HEAD
```

## Timezone Considerations

### Specify Timezone

```bash
# With specific timezone
git commit --date="2024-01-15 14:30:00 -0500" -m "Message"

# UTC timezone
git commit --date="2024-01-15 19:30:00 +0000" -m "Message"
```

### Current System Timezone

If no timezone is specified, Git uses your system's current timezone.

## Verification

### Check Commit Dates

```bash
# View commit log with dates
git log --pretty=format:"%h %ad %cd %s" --date=iso

# Where:
# %h = commit hash
# %ad = author date
# %cd = commit date
# %s = subject/message
```

### View Detailed Commit Information

```bash

```
