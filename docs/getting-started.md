# Getting started

Weave is an editor for [Typst](https://typst.app/docs) documents: you write on the left, and the
preview on the right shows the pages as you type. This guide shows the way around; Typst's own
[tutorial](https://typst.app/docs/tutorial/) teaches the language. Keys are written for both
systems: **Cmd** on macOS, **Ctrl** on Windows and Linux (Cmd/Ctrl+S).

## A project

A Weave project is a folder with your `.typ` files, images, bibliography and anything else the
document uses. The welcome screen offers:

- **New project…**: a name (it becomes the folder's name), **Choose…** where to create it, and a
  template:
  - **Empty**: a single file with a title.
  - **Article**: title, author, abstract, numbered sections and a bibliography.
  - **Report**: a title page, a table of contents and one file per chapter.
- **Open folder…** (Cmd/Ctrl+O): any folder with Typst files, for example one made with the Typst
  web app or the command line. Weave changes nothing in it until you edit.
- **Recent**: the projects you had open; **Remove from the list** forgets one (the folder stays).

The **main file** is the one Typst compiles; the others are included or imported from it.
Weave picks `main.typ`, or else the `.typ` file nearest the top. To choose another, right-click
it in the file tree → **Set as main file**.

## The window

- **The sidebar** (Cmd/Ctrl+B shows or hides it), with tabs: **Files** (the project's files and
  folders; right-click for new files, renaming, deleting), **Outline** (the headings; click one
  to go there), **Search** (the whole project, with replace), **Packages**, **Fonts**, **History**
  and **Git**.
- **The editor**, one tab per open file. Cmd/Ctrl+W closes a tab, Ctrl+Tab shows the next one.
- **The preview** (Cmd/Ctrl+Alt+B; Option on macOS): the pages, with zoom, **Fit width** and
  **Follow the cursor**. A click on text in the preview shows that place in the editor
  (Settings → Editor can limit that to Cmd/Ctrl+click).
- **Problems** (Cmd/Ctrl+Shift+M): errors and warnings, with hints; click one to go to its place.
  While the document has errors, the preview keeps the last version that compiled.
- **The status bar**: "Compiled in 87 ms · 12 pages", the numbers of errors and warnings, the
  word count, the spelling language, **Autosave**, the cursor's line and column, and Typst's
  version.

Drag the dividers to resize the panes; Weave remembers the layout for each project. F6 moves
the keyboard focus from pane to pane.

## Writing

- **Completion:** Weave offers functions, parameters, labels, citation keys and file paths as
  you type; Ctrl+Space (on macOS too) shows them. Up and Down, then Enter or Tab, choose one.
- **Help in place:** hover over a function to see what it does; F12 goes to where something is
  defined, also in other files and in packages.
- **Find and replace:** Cmd/Ctrl+F finds in the open file; replace with Cmd+Option+F on macOS,
  Ctrl+H elsewhere. Cmd/Ctrl+Shift+F searches the whole project, and can replace in every file
  at once (Weave takes a snapshot first, so you can undo it in the **History** tab).
- **More:** Cmd/Ctrl+/ comments lines in or out; Alt+Z (Option+Z on macOS) wraps long lines.
- **Spelling:** misspelled words in your text (never in code or math) are underlined.
  Right-click one for suggestions, or **Add "…" to dictionary**. Click the language in the status
  bar to choose others, or add a Hunspell dictionary. English (US) is built in.
- **Words:** the status bar counts the words of the document, and of the selection.

## Saving

**Autosave** is on: Weave saves about a second after you stop typing. Click **Autosave: on** in
the status bar to turn it off; then Cmd/Ctrl+S saves. Either way, Weave keeps a copy of unsaved
text in its data folder, so after a crash or a power cut it offers **Restore unsaved changes?**
the next time you open the project.

When another program changes a file (Zotero updating a bibliography, a Git tool, a sync
service), Weave reads it again. If you have unsaved edits in that file, it asks which version to
keep.

## Packages and fonts

Typst packages come from [Typst Universe](https://typst.app/universe):

```typ
#import "@preview/showybox:2.0.4": showybox
```

Popular packages come with Weave and work offline from the start. Others are downloaded the
first time a document imports them, and then work offline too. The **Packages** tab shows what
is on your computer, and can fetch the list of all packages to browse.

Fonts: Weave uses the fonts installed on your computer and the ones built into Typst. The
**Fonts** tab lists them and says where each comes from. **Import fonts…** adds fonts for Weave
alone; a project fonts folder (also in the Fonts tab) keeps fonts with the project, so it
compiles the same everywhere.

## Exporting a PDF

Cmd/Ctrl+E, or the PDF button in the toolbar at the top, writes the PDF into the project (for
`main.typ`, `main.pdf`), and the note says where. The button next to it, **Export options…**,
chooses the file (also outside the project), the standard (for example PDF/A-2b for archiving,
or PDF/UA-1, accessible) and the pages. A document with errors isn't exported: the note says how
many there are.

## Settings and commands

- **Settings** (Cmd/Ctrl+, or the gear): **Appearance** (light or dark, and a larger interface
  text), **Editor** (font, size, wrapping, autosave, what a click in the preview does), **This
  project** (spelling languages, automatic snapshots, PDF export), **Git**, **Storage**
  (downloaded packages), **Keyboard shortcuts** and **About** (version and licenses).
- **The command palette** (Cmd/Ctrl+Shift+P) lists every command with its keys: type a few
  letters and press Enter.

## Next

- [Bibliographies and Zotero](bibliography.md): citations, styles, and a bibliography that keeps
  itself up to date.
- [History and Git](history-and-git.md): snapshots to go back to, and Git to share a project.
- [Installing Weave](install.md): updating, uninstalling, and where Weave keeps its data.
- Typst's [documentation](https://typst.app/docs) and its [forum](https://forum.typst.app).
