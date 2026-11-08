# Bibliographies and Zotero

Typst has bibliographies built in: you keep your sources in a file in the project, cite them in
the text, and Typst formats the citations and the list of references in the style you choose.
Weave adds completion of citation keys and picks up changes to the file as soon as they happen,
so a reference manager such as Zotero can keep the file up to date for you.

## A bibliography file

Typst reads two formats; a project may use either, or both:

- **BibLaTeX / BibTeX** (`.bib`), the format most reference managers export:

  ```bib
  @book{knuth1984,
    author    = {Donald E. Knuth},
    title     = {The TeXbook},
    publisher = {Addison-Wesley},
    year      = {1984},
  }
  ```

- **Hayagriva** (`.yml` or `.yaml`), Typst's own format, easy to write by hand
  ([its documentation](https://github.com/typst/hayagriva/blob/main/docs/file-format.md)):

  ```yaml
  lamport1994:
    type: book
    title: "LaTeX: A Document Preparation System"
    author: Lamport, Leslie
    date: 1994
    publisher: Addison-Wesley
  ```

Put the file in the project folder (or a folder inside it), next to your `.typ` files. Typst only
reads files inside the project, so a file elsewhere on your computer can't be used.

## Citing

Add the bibliography where the list of references should appear, usually at the end:

```typ
#bibliography("refs.bib")
// Both formats at once:
#bibliography(("refs.bib", "refs.yml"))
```

Then cite with `@` and the entry's key:

```typ
Typesetting is an old craft @knuth1984.
As shown in @knuth1984[p.~7], …              // with a page
#cite(<lamport1994>, form: "prose") argues …  // "Lamport (1994) argues …"
```

- **Completion:** type `@` and Weave offers the keys of your bibliography and the labels of your
  document (Ctrl+Space shows them again). Keys appear after the first compile with the
  bibliography, and new entries after the file changes.
- **Only cited works are listed.** To list every entry of the file, use
  `#bibliography("refs.bib", full: true)`.
- **Keys with unusual characters:** `@` works for keys of letters, digits, `-`, `_`, `:` and `.`.
  For others, e.g. a `/`, write `#cite(label("DBLP:books/lib/Knuth86a"))`.
- Citation keys and the list of references are not spell-checked or counted as words.

## Styles

The `style` decides how citations and references look. Typst has many styles built in, for
example:

| Field | Style |
|---|---|
| Engineering, IT | `"ieee"` |
| Psychology, life sciences | `"apa"` |
| Social sciences | `"chicago-author-date"` |
| Humanities | `"mla"`, `"chicago-notes"`, `"harvard-cite-them-right"` |
| Physics | `"american-physics-society"` |

```typ
#bibliography("refs.bib", style: "apa")
```

A journal or university may ask for its own style: download its `.csl` file (e.g. from the
[Zotero Style Repository](https://www.zotero.org/styles)), put it in the project, and name it:

```typ
#bibliography("refs.bib", style: "styles/my-journal.csl")
```

The language of the document (`#set text(lang: "de")`) sets words such as "and" or "p." in the
citations.

## Zotero: a bibliography that keeps itself up to date

With the [Better BibTeX](https://retorque.re/zotero-better-bibtex/) plugin for Zotero, Zotero
writes your library (or a collection) to a `.bib` file in the project whenever it changes. Weave
notices the new file at once and compiles again; the new keys are offered when you type `@`.

1. **Install Better BibTeX** in Zotero (see its website). Each item now has a stable citation
   key, shown in Zotero's item pane, e.g. `knuthTeXbook1984`.
2. **Export once, kept up to date:** in Zotero, right-click the collection for this document (or
   your library) and choose **Export Collection…** (**Export Library…**). Choose the format
   **Better BibLaTeX**, tick **Keep updated**, and save the file into your project folder, e.g. as
   `refs.bib`.
3. **Use it in the document:** `#bibliography("refs.bib")`, and cite with `@` as above.

From now on, adding or changing an item in that Zotero collection rewrites `refs.bib`. Better
BibTeX's settings (**Automatic exports**) list your kept exports and say when they run: **on
change** (right away), **on idle** (when Zotero has been unused for a few seconds) or **paused**.

Good to know:

- **Don't edit the exported file by hand:** Zotero overwrites it with the next change. Fix entries
  in Zotero instead. If the file is open in Weave with unsaved edits when Zotero rewrites it,
  Weave asks which version to keep.
- **One collection per document** keeps the file small and the export quick.
- **With Git**, commit the `.bib` file with the project, so the document compiles anywhere, also
  without Zotero.
- **Offline:** everything above works without a network; only installing Zotero plugins or
  downloading styles needs one.

## When something goes wrong

| What you see | What to do |
|---|---|
| ``label `<…>` does not exist in the document`` after `@…` (or ``citation key `…` is not present in the bibliography`` from `#cite`) | The key is in no bibliography file (nor a label of the document). Check the spelling (completion helps), and that Zotero's export ran and contains the item. |
| `failed to parse BibLaTeX (…)`, e.g. `(expected comma)`, at a place in the file | The `.bib` file has an error at that line. For Zotero exports, fix the item in Zotero; Better BibTeX writes valid files. |
| `file not found (searched at …)` for the bibliography or a `.csl` file | The path is relative to the `.typ` file that calls `bibliography`; the file must be inside the project folder. |
| A cited work is missing from the list | The list shows only cited works, and each citation belongs to one bibliography; use `full: true` to list all entries. |
| Completion doesn't offer a new key | Wait for the compile after the file changed (see the status bar); the key must be in a file the bibliography reads. |
