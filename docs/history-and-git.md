# History and Git

Weave keeps two kinds of history for a project:

- **Snapshots** (the **History** tab): copies of the whole project that Weave takes on its own as
  you work, and whenever you ask. They need no setup, work offline and stay on your computer.
- **Git** (the **Git** tab): commits you make yourself, which you can push to GitHub, GitLab,
  Bitbucket, Codeberg or any other Git server, to share the project or keep it elsewhere.

You can use either, or both: snapshots catch the small steps between commits.

## Snapshots

### When they are taken

- **Automatically**, while automatic snapshots are on: before your first edit after opening the
  project, after a save at most every 5 minutes, and when the project closes, each time only if
  something changed.
- **Before Weave changes many files at once:** before a restore ("Before restoring"), before
  **Replace** in the search panel changes files on disk ("Before replacing"), and before a Git pull
  ("Before pull"). These are taken even with automatic snapshots off.
- **When you ask:** **Take snapshot…** in the History tab, with a name if you like ("Sent to my
  supervisor"). Named snapshots are shown in bold.

A snapshot holds every file of the project up to 20 MB, with the text you haven't saved yet.
It leaves out larger files, the exported PDF and hidden files and folders (names starting with a
dot, such as `.git`). Files that don't change between snapshots are stored once, so a hundred
snapshots of a thesis with figures take little more room than one.

### How long they are kept

Automatic snapshots: all of the last 24 hours, then the last one of each day for 30 days, then the
last one of each week. Snapshots you took, and the "Before …" ones, stay until you delete them.

To turn automatic snapshots off for a project, uncheck **Automatic snapshots** in the History
tab's ⋮ menu, or in Settings → **This project**. This writes `[history] automatic = false` into
`.weave/project.toml`.

### Looking back

The History tab lists the snapshots by day, newest first: "14:30 · Automatic · 3 files changed"
(the files that changed since the snapshot before). Click one to see the files that differ
between it and now:

- **A** the file is new since then, **M** it changed, **D** it was deleted.
- Click a file to open its **diff**: a read-only tab ("main.typ — 14:30 ↔ now") with the lines
  removed since then in red and the lines added in green, the changed words highlighted, and the
  places of the changes marked next to the scrollbar. Images and other files that aren't text,
  and files over 5 MB, show their sizes instead. Cmd/Ctrl+W closes the diff.

### Restoring

- **One file:** the file's ⋯ menu in the list, or **Restore this file** in its diff. Restoring a
  file that is new since then deletes it.
- **The whole project:** the snapshot's ⋯ menu → **Restore project…**. Weave says how many files
  it will put back and how many files added since it will delete, and asks first. Files larger
  than 20 MB are left as they are.

Before a restore, Weave takes a "Before restoring" snapshot of the project as it is, and the
note after the restore offers **Undo**, which restores that one. Open tabs show the restored text
at once (their unsaved edits are replaced; they are in the "Before restoring" snapshot).

### Where snapshots are kept

In the project folder, in `.weave/history/`. They move, copy and back up with the project. Weave
also writes `.weave/.gitignore`, so Git ignores the snapshots: they never end up in a commit.
Commit `.weave/.gitignore` itself (it shows up as a new file in the Git tab after the first
snapshot), so other copies of the repository ignore them too.

To free the room snapshots take, delete the ones you don't need, or delete `.weave/history/`
while the project is closed (all its snapshots go).

## Git

### A repository

Weave works with a Git repository whose top folder is the project folder (the one with `.git`
in it). If the project isn't one yet, the Git tab offers **Create repository** (its branch is
called `main`). A project inside a bigger repository (for example `papers/2026/` in a
repository of all your papers) is shown as such, without Git features; open the repository's
top folder as the project instead, or keep the paper in a repository of its own.

Weave doesn't clone repositories: clone with another Git tool (GitHub Desktop, the `git`
program), then open the folder in Weave.

### Changes, staging and committing

The **Git** tab (Ctrl+Shift+G, on every platform) shows the branch and its remote, and below it
the changes since the last commit:

| Letter | Meaning |
|---|---|
| **M** | modified |
| **A** | added (new and staged) |
| **U** | untracked: new, not yet in Git |
| **D** | deleted |
| **R** | renamed |
| **C** | conflicted: a merge left it for you to resolve (in another Git tool) |

The file tree shows the same letters next to the files, and a dot on folders with changes.

1. **Stage** what should go into the commit: **+** next to a file, or **Stage all**. **−**
   unstages it again. Staging saves the file first. Click a file to see its diff (against the
   last commit, "HEAD", or against what is staged).
2. **Write a message** and press **Commit** (or Cmd/Ctrl+Enter in the message).

Weave never commits on its own. The status follows your saves and changes made by other programs,
and is read again when you come back to Weave.

**Who commits:** the name and email in the repository's Git settings, else your global Git
settings (on Windows and Linux; the sandboxed macOS app can't read them). If neither has them,
the commit box asks once; Weave keeps them in its own settings (not in the repository) and uses
them for every repository that doesn't name you. Change them in the Git tab's ⋮ menu → **Name and
email for commits…**, or in Settings → **Git**.

### Remotes: HTTPS only

Weave pulls and pushes over **HTTPS** (`https://github.com/you/thesis.git`). Remotes that use
**SSH** (`git@github.com:you/thesis.git`) need the `ssh` program, which Weave doesn't run: the Git
tab explains this, and for GitHub, GitLab, Bitbucket and Codeberg offers **Use the HTTPS
address…**. Changing the remote to HTTPS affects other Git tools too; they keep working with it.

To set or change the remote: the Git tab's ⋮ menu → **Set remote URL…** (the repository's
`origin`). Create the repository on the server first (empty, without a README, is simplest).

### Pull, push and fetch

The buttons next to the remote URL:

- **Fetch** gets the remote's new commits without changing your files; the branch line then
  shows how many there are (`main → origin/main ↑2 ↓1`: 2 to push, 1 to pull).
- **Pull** saves your edits, takes a "Before pull" snapshot, and brings in the remote's commits:
  your branch moves forward, or, if both sides have new commits, a merge commit joins them.
  If the merge would have conflicts, or would change a file with uncommitted changes, nothing
  changes and the note says why.
- **Push** sends your commits. A branch that has never been pushed goes to `origin` under its own
  name after Weave asks. Weave never forces a push: if the remote has commits you don't have,
  it says "pull first".

They run only when you click them, show their progress in the status bar ("Pushing… 1.2 MB")
with a button to cancel, and end with a note. Proxies set in the environment (`HTTPS_PROXY`)
are used.

### Signing in: tokens

Most Git servers don't accept your account password over HTTPS any more; they want a **token**
(a long password made for one purpose, which you can revoke). When the server asks, Weave shows
**Sign in to …**: enter your username and the token. With **Remember** (on by default), Weave
keeps them in the system's password store (the login keychain on macOS, the Credential Manager
on Windows, the Secret Service, such as GNOME Keyring, on Linux) and uses them next time. To forget
them: the Git tab's ⋮ menu → **Forget the sign-in for …**, or Settings → **Git** → **Sign-ins**.
On macOS they appear in Keychain Access as `dev.ozdemirsoftware.weave.git`, with the server as
the account. Where no password store runs (a Linux system without the Secret Service), Weave
asks for the sign-in again each time it starts.

**GitHub** ([GitHub's guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)):
your profile picture → **Settings** → **Developer settings** → **Personal access tokens**:

- **Fine-grained tokens** → **Generate new token**: choose the repository under **Repository
  access**, and give **Contents** the access **Read and write**; or
- **Tokens (classic)** → **Generate new token (classic)** with the scope **repo**.

Sign in with your GitHub username and the token.

**GitLab** ([GitLab's guide](https://docs.gitlab.com/user/profile/personal_access_tokens/)):
your avatar → **Edit profile** → **Access** → **Personal access tokens** → **Generate token** →
**Legacy token**, with the scope **write_repository** (pull and push; **read_repository** is
enough to pull only).
Sign in with your GitLab username and the token.

**Bitbucket** ([Atlassian's guide](https://support.atlassian.com/bitbucket-cloud/docs/using-api-tokens/)):
Bitbucket's app passwords no longer work; use an **API token**. Your profile → **Account
settings** → **Security** → **Create and manage API tokens** → **Create API token with scopes**,
choose **Bitbucket** as the app and the scopes **read:repository:bitbucket** and
**write:repository:bitbucket**. Sign in with your **Bitbucket username** (as shown in your
Bitbucket settings; it is case sensitive, and not your email address) and the token.

**Codeberg** ([Codeberg's guide](https://docs.codeberg.org/advanced/access-token/)):
your profile picture → **Settings** → **Applications** → generate a token with read and write
access to repositories. Sign in with your Codeberg username and the token (Codeberg still
takes your password if you don't use two-factor authentication, but a token is safer).

Other servers: use what their `git` instructions say for HTTPS. Weave sends the username and
token only over HTTPS (plain HTTP only to your own computer).

### What Weave doesn't do

Weave covers the everyday steps for a document. For the rest, use another Git tool on the same
folder; Weave sees its changes.

- **SSH remotes**, cloning, branches (creating, switching, merging), tags, stashes, rebasing,
  and resolving merge conflicts.
- **Git hooks** don't run, and **commits aren't signed**: if the repository asks for signed
  commits, the Git tab says so; commit with another tool if the server requires signatures.
- **Git LFS:** files kept with LFS are seen as the small pointer files Git stores for them; Weave
  doesn't download or upload their content (the Git tab warns about it).
- **Filters, credential helpers and Git's `http.*` settings** aren't used. Remembered sign-ins
  are Weave's own (see above).
- **Repositories in special formats** (a split or sparse index) can be read but not changed;
  the note says so.

## When something goes wrong

| What you see | What to do |
|---|---|
| "Pull failed: Commit or undo your changes first; a pull would change …" | Files with uncommitted changes would be overwritten. Commit (or restore) them, then pull again. |
| "Pull failed: Pulling would cause conflicts in …; resolve them with another Git tool." | Both you and the remote changed the same lines. Nothing was changed. Pull and merge with another Git tool, or undo your side. |
| "Push failed: The remote has commits you don't have; pull first, then push." | Someone (or you, elsewhere) pushed first. Pull, then push. |
| "Push failed: The server refused the push: …" | The server's reason follows, e.g. a protected branch. Check the repository's settings on the server. |
| **Sign in to …** comes back with "… didn’t accept the username and password or token" | Check the username (Bitbucket: your username, not your email), that the token isn't expired, and that it allows writing to the repository. |
| "Weave pulls and pushes over HTTPS only …" | The remote uses SSH: **Use the HTTPS address…**, or **Set remote URL…**. |
| "Push failed: Commit something first: the branch has no commits" | A new repository: stage and commit first. |
| "Pull failed: Git needs a name and an email for commits. …" | A merge commit needs them: the ⋮ menu → **Name and email for commits…**. |
| "Fetch failed: …" with a network error, or nothing happens for a long time | Check the connection and the URL. Cancel in the status bar stops the operation as soon as data moves; a server that sends nothing is given up after 30 s (connecting) or 2 minutes (waiting for an answer). |
| "Snapshots could not be taken: …" in the status bar | Usually a full disk or a read-only folder. Your project is unaffected; free space, or turn automatic snapshots off. |
| "1 snapshot can’t be read. It is kept as it is." in the History tab | A snapshot's files were damaged (for example by a copy that stopped half-way). The other snapshots are fine. |
