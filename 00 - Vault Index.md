# Aquamarine Vault

> [!info] Live vault navigator
> This page discovers notes and folders automatically. Nothing here needs to be maintained by hand.

## Writing activity

```dataviewjs
const currentPath = dv.current().file.path;
const toDateKey = date => [
    date.getFullYear(),
    String(date.getMonth() + 1).padStart(2, "0"),
    String(date.getDate()).padStart(2, "0")
].join("-");

const today = new Date();
today.setHours(12, 0, 0, 0);

const activityStart = new Date(today);
activityStart.setDate(activityStart.getDate() - 364);

const gridStart = new Date(activityStart);
gridStart.setDate(gridStart.getDate() - gridStart.getDay());

const gridEnd = new Date(today);
gridEnd.setDate(gridEnd.getDate() + (6 - gridEnd.getDay()));

const startKey = toDateKey(gridStart);
const endKey = toDateKey(gridEnd);
const stats = new Map();

const pages = dv.pages()
    .array()
    .filter(page => page.file.path !== currentPath)
    .map(page => {
        const filenameDate = page.file.name.match(/^(\d{4}-\d{2}-\d{2})/);
        const day = filenameDate
            ? filenameDate[1]
            : page.file.day?.toFormat("yyyy-LL-dd")
              ?? page.file.ctime.toFormat("yyyy-LL-dd");
        return { page, day };
    })
    .filter(entry => entry.day >= startKey && entry.day <= endKey);

await Promise.all(pages.map(async ({ page, day }) => {
    const file = app.vault.getAbstractFileByPath(page.file.path);
    if (!file || file.extension !== "md") return;

    const source = await app.vault.cachedRead(file);
    const prose = source
        .replace(/^---\r?\n[\s\S]*?\r?\n---\r?\n?/, " ")
        .replace(/\x60{3}[\s\S]*?\x60{3}/g, " ")
        .replace(/`[^`]*`/g, " ")
        .replace(/!\[\[[^\]]+\]\]/g, " ")
        .replace(/!\[[^\]]*\]\([^)]+\)/g, " ")
        .replace(/\[([^\]]+)\]\([^)]+\)/g, "$1")
        .replace(/\[\[([^\]|]+)\|([^\]]+)\]\]/g, "$2")
        .replace(/\[\[([^\]]+)\]\]/g, "$1")
        .replace(/https?:\/\/\S+/g, " ");
    const words = prose.match(/[\p{L}\p{N}]+(?:['’][\p{L}\p{N}]+)*/gu)?.length ?? 0;

    const entry = stats.get(day) ?? { words: 0, notes: [] };
    entry.words += words;
    entry.notes.push({ name: page.file.name, path: page.file.path });
    stats.set(day, entry);
}));

const dates = [];
for (let date = new Date(gridStart); date <= gridEnd; date.setDate(date.getDate() + 1)) {
    dates.push(new Date(date));
}

const wordsByDay = dates.map(date => stats.get(toDateKey(date))?.words ?? 0);
const maxWords = Math.max(1, ...wordsByDay);
const inActivityWindow = date => date >= activityStart && date <= today;
const activeDays = wordsByDay.filter((words, index) =>
    inActivityWindow(dates[index]) && words > 0
).length;
const totalWords = wordsByDay.reduce((sum, words, index) =>
    sum + (inActivityWindow(dates[index]) ? words : 0), 0
);

let run = 0;
let longestStreak = 0;
for (let index = 0; index < dates.length && dates[index] <= today; index++) {
    if (dates[index] < activityStart) continue;
    if (wordsByDay[index] > 0) {
        run += 1;
        longestStreak = Math.max(longestStreak, run);
    } else {
        run = 0;
    }
}

const heatmap = dv.container.createDiv({ cls: "vault-heatmap" });
const metrics = heatmap.createDiv({ cls: "vault-heatmap-metrics" });

const addMetric = (value, label) => {
    const metric = metrics.createDiv({ cls: "vault-heatmap-metric" });
    metric.createDiv({ text: value, cls: "vault-heatmap-metric-value" });
    metric.createDiv({ text: label, cls: "vault-heatmap-metric-label" });
};

addMetric(totalWords.toLocaleString(), "words in the last year");
addMetric(activeDays.toLocaleString(), "active writing days");
addMetric(`${longestStreak} day${longestStreak === 1 ? "" : "s"}`, "longest streak");

const scroller = heatmap.createDiv({ cls: "vault-heatmap-scroller" });
const calendar = scroller.createDiv({ cls: "vault-heatmap-calendar" });
const weekCount = Math.ceil(dates.length / 7);
calendar.style.setProperty("--vault-heatmap-weeks", weekCount);

for (const [weekday, label] of [[1, "Mon"], [3, "Wed"], [5, "Fri"]]) {
    const dayLabel = calendar.createDiv({ text: label, cls: "vault-heatmap-day-label" });
    dayLabel.style.gridColumn = "1";
    dayLabel.style.gridRow = String(weekday + 2);
}

const seenMonths = new Set();
dates.forEach((date, index) => {
    const monthKey = `${date.getFullYear()}-${date.getMonth()}`;
    if (seenMonths.has(monthKey)) return;
    seenMonths.add(monthKey);

    const label = calendar.createDiv({
        text: date.toLocaleDateString(undefined, { month: "short" }),
        cls: "vault-heatmap-month-label"
    });
    label.style.gridColumn = `${Math.floor(index / 7) + 2} / span 4`;
    label.style.gridRow = "1";
});

dates.forEach((date, index) => {
    const key = toDateKey(date);
    const entry = stats.get(key);
    const words = entry?.words ?? 0;
    const level = words === 0
        ? 0
        : Math.max(1, Math.min(4,
            Math.ceil((Math.log1p(words) / Math.log1p(maxWords)) * 4)
        ));
    const noteText = entry
        ? ` across ${entry.notes.length} note${entry.notes.length === 1 ? "" : "s"}`
        : "";
    const label = `${date.toLocaleDateString(undefined, {
        year: "numeric",
        month: "short",
        day: "numeric"
    })}: ${words.toLocaleString()} words${noteText}`;

    const cell = calendar.createEl("span", {
        cls: `vault-heatmap-cell vault-heatmap-level-${level}`
    });
    cell.style.gridColumn = String(Math.floor(index / 7) + 2);
    cell.style.gridRow = String(date.getDay() + 2);
    cell.setAttr("title", label);
    cell.setAttr("aria-label", label);
});

const legend = heatmap.createDiv({ cls: "vault-heatmap-legend" });
legend.createSpan({ text: "Less" });
for (let level = 0; level <= 4; level++) {
    legend.createSpan({ cls: `vault-heatmap-cell vault-heatmap-level-${level}` });
}
legend.createSpan({ text: "More" });

heatmap.createDiv({
    text: "Current word totals are grouped by the date at the start of a note’s filename; undated notes use their creation date. Hover over a square for details.",
    cls: "vault-heatmap-caption"
});
```

## Recently updated

```dataviewjs
const currentPath = dv.current().file.path;
const recent = dv.pages()
    .array()
    .filter(page => page.file.path !== currentPath)
    .sort((a, b) => b.file.mtime.toMillis() - a.file.mtime.toMillis())
    .slice(0, 12);

const grid = dv.container.createDiv({ cls: "vault-recent-grid" });

for (const page of recent) {
    const card = grid.createDiv({ cls: "vault-recent-card" });
    const target = page.file.path.replace(/\.md$/i, "");
    const link = card.createEl("a", {
        text: page.file.name,
        cls: "internal-link vault-recent-title"
    });
    link.setAttr("data-href", target);
    link.setAttr("href", target);

    card.createDiv({
        text: page.file.folder || "Vault root",
        cls: "vault-recent-folder"
    });
    card.createDiv({
        text: page.file.mtime.toFormat("yyyy-LL-dd · HH:mm"),
        cls: "vault-recent-time"
    });
}
```

## Browse folders

```dataviewjs
const currentPath = dv.current().file.path;
const collator = new Intl.Collator(undefined, {
    numeric: true,
    sensitivity: "base"
});

const pages = dv.pages()
    .array()
    .filter(page => page.file.path !== currentPath)
    .sort((a, b) => collator.compare(a.file.name, b.file.name));

const newNode = () => ({ folders: new Map(), notes: [] });
const root = newNode();

for (const page of pages) {
    const parts = page.file.folder
        ? page.file.folder.split("/").filter(Boolean)
        : [];
    let node = root;

    for (const part of parts) {
        if (!node.folders.has(part)) node.folders.set(part, newNode());
        node = node.folders.get(part);
    }

    node.notes.push({
        name: page.file.name,
        path: page.file.path
    });
}

const countNotes = node => {
    let total = node.notes.length;
    for (const child of node.folders.values()) total += countNotes(child);
    return total;
};

const countFolders = node => {
    let total = node.folders.size;
    for (const child of node.folders.values()) total += countFolders(child);
    return total;
};

const shell = dv.container.createDiv({ cls: "vault-index-shell" });
const toolbar = shell.createDiv({ cls: "vault-index-toolbar" });
const search = toolbar.createEl("input", {
    cls: "vault-index-search",
    attr: {
        type: "search",
        placeholder: "Filter folders or notes…",
        "aria-label": "Filter folders or notes"
    }
});
const expandButton = toolbar.createEl("button", {
    text: "Expand all",
    cls: "vault-index-button"
});
const collapseButton = toolbar.createEl("button", {
    text: "Collapse all",
    cls: "vault-index-button"
});

shell.createDiv({
    text: `${pages.length} notes in ${countFolders(root)} folders`,
    cls: "vault-index-status"
});

const tree = shell.createDiv({ cls: "vault-index-tree" });

const createNoteLink = (note, parent) => {
    const item = parent.createEl("li", { cls: "vault-index-note" });
    item.createSpan({ text: "◇", cls: "vault-index-note-icon" });
    const target = note.path.replace(/\.md$/i, "");
    const link = item.createEl("a", {
        text: note.name,
        cls: "internal-link"
    });
    link.setAttr("data-href", target);
    link.setAttr("href", target);
};

const renderFolder = (name, node, parent, depth = 0) => {
    const details = parent.createEl("details", {
        cls: `vault-index-folder vault-index-depth-${Math.min(depth, 4)}`
    });
    const summary = details.createEl("summary");
    summary.createSpan({ text: "▸", cls: "vault-index-chevron" });
    summary.createSpan({ text: "📁", cls: "vault-index-folder-icon" });
    summary.createSpan({ text: name, cls: "vault-index-folder-name" });
    summary.createSpan({
        text: String(countNotes(node)),
        cls: "vault-index-count"
    });

    if (node.notes.length > 0) {
        const list = details.createEl("ul", { cls: "vault-index-notes" });
        for (const note of node.notes) createNoteLink(note, list);
    }

    const folders = [...node.folders.entries()]
        .sort(([a], [b]) => collator.compare(a, b));
    for (const [childName, childNode] of folders) {
        renderFolder(childName, childNode, details, depth + 1);
    }
};

if (root.notes.length > 0) {
    renderFolder("Vault root", { folders: new Map(), notes: root.notes }, tree);
}

for (const [name, node] of [...root.folders.entries()]
    .sort(([a], [b]) => collator.compare(a, b))) {
    renderFolder(name, node, tree);
}

const allFolders = () => [...tree.querySelectorAll("details.vault-index-folder")];

expandButton.addEventListener("click", () => {
    for (const folder of allFolders()) folder.open = true;
});

collapseButton.addEventListener("click", () => {
    for (const folder of allFolders()) folder.open = false;
});

search.addEventListener("input", () => {
    const query = search.value.trim().toLocaleLowerCase();
    const folders = allFolders();

    for (const folder of folders) folder.style.display = "";

    if (!query) {
        for (const folder of folders) folder.open = false;
        return;
    }

    for (const folder of [...folders].reverse()) {
        const matches = folder.textContent.toLocaleLowerCase().includes(query);
        folder.style.display = matches ? "" : "none";
        if (matches) folder.open = true;
    }
});
```
