# Aquamarine Vault Index

> [!info] Live table of contents
> This page is generated from the vault by Dataview. New notes and folders appear automatically; there is nothing to update manually. Click any note title to open it, or use `Ctrl+F` to search this page.

## Recently updated

```dataview
TABLE WITHOUT ID
    file.link AS "Note",
    choice(file.folder = "", "Vault root", file.folder) AS "Folder",
    dateformat(file.mtime, "yyyy-MM-dd HH:mm") AS "Last updated"
FROM ""
WHERE file.path != this.file.path
SORT file.mtime DESC
LIMIT 15
```

## All notes by folder

```dataview
TABLE WITHOUT ID
    choice(Folder = "", "Vault root", Folder) AS "Folder",
    length(rows) AS "Notes",
    rows.file.link AS "Contents"
FROM ""
WHERE file.path != this.file.path
GROUP BY file.folder AS Folder
SORT Folder ASC
```
