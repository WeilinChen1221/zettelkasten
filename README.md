# Zettelkasten Notes

This repository is used to manage and synchronize Markdown notes with Git and GitHub.

It supports two types of notes:

* Public or non-sensitive notes stored in plain text.
* Private notes encrypted with [`git-crypt`](https://github.com/AGWA/git-crypt) before being pushed to GitHub.

## Repository Structure

```text
zettelkasten/
  vault/                  # Encrypted private notes
  .gitattributes          # git-crypt encryption rules
  .gitignore              # Files that should never be committed
  README.md               # Plain-text notes
```

## Setup

Install `git-crypt`.

On macOS:

```bash
brew install git-crypt
```

On Linux, use your package manager if available:

```bash
sudo apt install git-crypt
```

Then initialize the repository:

```bash
git init
git branch -M main
git-crypt init
```

Create the encryption rules in `.gitattributes`:

```gitattributes
vault/** filter=git-crypt diff=git-crypt

.gitattributes !filter !diff
.gitignore !filter !diff
```

Create a `.gitignore` file:

```gitignore
.DS_Store
Thumbs.db

*.gitcrypt.key
git-crypt-key*
```

Add and commit the initial files:

```bash
git add .
git commit -m "init notes"
```

## Export the Encryption Key

```bash
git-crypt export-key ~/secure-keys/notes.gitcrypt.key
```

This key is required to unlock the encrypted files on another machine.

## Clone and Unlock on a New Machine

Install `git-crypt` first.

Then clone the repository:

```bash
git clone git@github.com:YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Unlock the encrypted files using the exported key:

```bash
git-crypt unlock ~/secure-keys/notes.gitcrypt.key
```

After unlocking, encrypted notes will appear as normal readable files locally.

## Daily Workflow

Use Git normally:

```bash
git pull
```

Edit notes as needed.

Then commit and push:

```bash
git status
git add .
git commit -m "update notes"
git push
```

## Check Encryption Status

To verify which files are encrypted:

```bash
git-crypt status
```
