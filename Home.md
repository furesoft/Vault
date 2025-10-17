---
editor-width: 100
banner: "![[night-sky.jpg]]"
cssclasses:
  - hide-properties
created: 2025-10-07T18:32
updated: 2025-10-15T18:50
---

```dataviewjs
// === CONFIG ===
const dailyFolder = "00 - Daily/";
const weeklyFolder = "01 - Weekly/";
const dateFormat = "yyyy-MM-dd";
const weekFormat = "kkkk-'W'WW";

// === BUILD PATHS ===
const today = dv.date("today");
const dailyPath = `${dailyFolder}${today.toFormat(dateFormat)}`;
const weeklyPath = `${weeklyFolder}${today.toFormat(weekFormat)}`;

// === BUTTONS ===
let content = "";
content += `<a class="internal-link elegant-btn ready" href="${dailyPath}">📅 Today</a>`;
content += `<a class="internal-link elegant-btn ready" href="${weeklyPath}">🗓️ This Week</a>`;

// === OUTPUT ===
dv.el("div", `<div class="breadcrumbs-wrapper">${content}</div>`);
```

```search-bar
show recent files
```

```dataviewjs
// Create container
let container = dv.el("div", "", {cls: "tag-cloud-container"});

// Get all pages
let pages = dv.pages();

// Flatten all tags across all pages
let allTags = [];
for (let page of pages) {
    if (page.file.tags) {
        allTags.push(...page.file.tags);
    }
}

// Count occurrences of each tag
let tagCounts = allTags.reduce((acc, tag) => {
    acc[tag] = (acc[tag] || 0) + 1;
    return acc;
}, {});

// 🔹 Sort tags alphabetically
let sortedTags = Object.keys(tagCounts).sort((a, b) => a.localeCompare(b));

// Render each tag as clickable link
for (let tag of sortedTags) {
    let count = tagCounts[tag];
    let link = dv.el(
        "a",
        `${tag} (${count})`,
        {
            href: `obsidian://search?query=${encodeURIComponent(tag)}`,
            cls: "tag-chip"
        }
    );
    link.style.margin = "2px"; // add spacing
    container.appendChild(link);
}

```
