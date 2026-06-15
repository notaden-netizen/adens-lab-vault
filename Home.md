# 🧬 Aden's Lab Notebook

**Date:** `=dateformat(now(), "EEEE, MMMM d, yyyy")`  
**Time:** `=dateformat(now(), "h:mm a")`

---

## 📊 Progress Tracker

```dataviewjs
const tasks = dv.pages()
  .filter(p => p.file.tasks && p.file.tasks.length > 0);

let completed = 0;
let total = 0;

for (const task of dv.pages().filter(p => p.file.tasks)) {
  for (const t of task.file.tasks || []) {
    total++;
    if (t.completed) completed++;
  }
}

const percentage = total > 0 ? Math.round((completed / total) * 100) : 0;

const svg = `
<svg viewBox="0 0 200 200" style="width: 200px; height: 200px; transform: rotate(-90deg); margin: 20px auto; display: block;">
  <defs>
    <linearGradient id="circleGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#5B21B6;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#7C3AED;stop-opacity:1" />
    </linearGradient>
  </defs>
  <circle cx="100" cy="100" r="90" fill="none" stroke="#2D1B4E" stroke-width="8"/>
  <circle cx="100" cy="100" r="90" fill="none" stroke="url(#circleGrad)" stroke-width="8" 
    stroke-dasharray="${(percentage / 100) * 565.48} 565.48"
    style="transition: stroke-dasharray 0.5s ease; filter: drop-shadow(0 0 8px rgba(167, 139, 250, 0.6));"/>
  <text x="100" y="95" text-anchor="middle" dy="0.3em" 
    style="font-size: 42px; font-weight: bold; fill: #A78BFA; font-family: 'Times New Roman', serif;">
    ${percentage}%
  </text>
  <text x="100" y="135" text-anchor="middle" 
    style="font-size: 14px; fill: #C4B5FD; font-family: 'Times New Roman', serif;">
    ${completed}/${total} tasks
  </text>
</svg>
`;

dv.el('div', svg);
```

---

## ⚡ Quick Actions

```dataviewjs
const actions = [
  { icon: "🧪", label: "Create Experiment", link: "02-EXPERIMENTS/Experiment-Template" },
  { icon: "🧬", label: "Add Plasmid", link: "03-MOLECULAR-DATABASE/Plasmid-Template" },
  { icon: "📌", label: "Add Primer", link: "03-MOLECULAR-DATABASE/Primer-Template" },
  { icon: "📄", label: "Add Source", link: "04-LITERATURE-HUB/Source-Template" },
  { icon: "📋", label: "Add Protocol", link: "05-PROTOCOLS/Protocol-Template" },
  { icon: "📊", label: "New Analysis", link: "06-ANALYSIS/Analysis-Template" }
];

let html = '<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0;">';

for (const action of actions) {
  html += `
    <div style="
      background: linear-gradient(135deg, #5B21B6 0%, #7C3AED 100%);
      border: 2px solid #A78BFA;
      border-radius: 8px;
      padding: 20px;
      text-align: center;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 4px 6px rgba(123, 31, 162, 0.3);
    "
    onmouseover="this.style.boxShadow='0 8px 12px rgba(167, 139, 250, 0.5)'; this.style.transform='translateY(-2px)';"
    onmouseout="this.style.boxShadow='0 4px 6px rgba(123, 31, 162, 0.3)'; this.style.transform='translateY(0)';"
    >
      <div style="font-size: 32px; margin-bottom: 10px;">${action.icon}</div>
      <a href="#" onclick="app.workspace.openLinkText('${action.link}', '', false); return false;" style="
        color: #FFFFFF;
        text-decoration: none;
        font-family: 'Times New Roman', serif;
        font-weight: bold;
        font-size: 14px;
      ">${action.label}</a>
    </div>
  `;
}

html += '</div>';
dv.el('div', html);
```

---

## 📊 Dashboard Statistics

| Metric | Count |
|--------|-------|
| 🧪 Active Experiments | `=length(filter(dv.pages('"02-EXPERIMENTS"'), p => p.status = "active"))` |
| 🧬 Total Plasmids | `=length(filter(dv.pages('"03-MOLECULAR-DATABASE"'), p => p.type = "plasmid"))` |
| 📌 Total Primers | `=length(filter(dv.pages('"03-MOLECULAR-DATABASE"'), p => p.type = "primer"))` |
| 📚 Literature Sources | `=length(dv.pages('"04-LITERATURE-HUB"'))` |

---

## 🧪 Active Experiments

```dataviewjs
const experiments = dv.pages('"02-EXPERIMENTS"')
  .filter(p => p.status === "active" || !p.status)
  .sort(p => p.date, "desc")
  .limit(5);

if (experiments.length === 0) {
  dv.paragraph("📭 No active experiments. [Create one →](02-EXPERIMENTS/Experiment-Template)");
} else {
  dv.table(["Experiment", "Date Started", "Status"], 
    experiments.map(e => [
      `[[${e.file.name}]]`,
      e.date ? dv.date.luxon.DateTime.fromISO(e.date).toLocaleString() : "N/A",
      `🟢 ${e.status || "Active"}`
    ])
  );
}
```

---

## 🧬 Molecular Database

### Recent Plasmids
```dataviewjs
const plasmids = dv.pages('"03-MOLECULAR-DATABASE"')
  .filter(p => p.type === "plasmid")
  .sort(p => p.date, "desc")
  .limit(4);

if (plasmids.length === 0) {
  dv.paragraph("No plasmids yet. [Add one →](03-MOLECULAR-DATABASE/Plasmid-Template)");
} else {
  dv.table(["Name", "Size (bp)", "Markers"], 
    plasmids.map(p => [
      `[[${p.file.name}]]`,
      p.size || "—",
      (p.markers || []).join(", ") || "—"
    ])
  );
}
```

### Recent Primers
```dataviewjs
const primers = dv.pages('"03-MOLECULAR-DATABASE"')
  .filter(p => p.type === "primer")
  .sort(p => p.date, "desc")
  .limit(4);

if (primers.length === 0) {
  dv.paragraph("No primers yet. [Add one →](03-MOLECULAR-DATABASE/Primer-Template)");
} else {
  dv.table(["Name", "Sequence (5' → 3')", "Tm (°C)"], 
    primers.map(p => [
      `[[${p.file.name}]]`,
      `\`${(p.sequence || "").substring(0, 25)}${(p.sequence || "").length > 25 ? "..." : ""}\``,
      p.tm || "—"
    ])
  );
}
```

---

## 📚 Literature Hub

```dataviewjs
const sources = dv.pages('"04-LITERATURE-HUB"')
  .sort(p => p.date_added, "desc")
  .limit(5);

if (sources.length === 0) {
  dv.paragraph("📭 No sources yet. [Add one →](04-LITERATURE-HUB/Source-Template)");
} else {
  dv.table(["Title", "Authors", "Year", "Status"], 
    sources.map(s => [
      `[[${s.file.name}]]`,
      (s.authors || []).slice(0, 2).join(", ") + (s.authors && s.authors.length > 2 ? "..." : ""),
      s.year || "—",
      s.read ? "✅ Read" : "📖 To Read"
    ])
  );
}
```

**💡 Integration Tips:**
- Use **Zotero** with the Obsidian Zotero plugin for seamless citation management
- Export bibliography as BibTeX for LaTeX projects
- Tag sources with `#review`, `#cited`, `#important` for easy filtering

---

## 📋 Templates Reference

| Template | Purpose | Link |
|----------|---------|------|
| 🧪 Experiment | Track experimental design & results | [[Experiment-Template]] |
| 🧬 Plasmid | Document plasmid information | [[Plasmid-Template]] |
| 📌 Primer | Store primer sequences & properties | [[Primer-Template]] |
| 📄 Source/Paper | Manage literature citations | [[Source-Template]] |
| 📋 Protocol | Document lab procedures | [[Protocol-Template]] |
| 🔬 Cloning Workflow | Step-by-step cloning guide | [[Cloning-Workflow]] |
| 📊 Analysis | Document data analysis | [[Analysis-Template]] |

---

## ⌨️ Custom Keyboard Shortcuts

### Setup Instructions:
1. Go to **Settings → Hotkeys**
2. Search for each action and assign these shortcuts:

| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Create Experiment | `Cmd + Opt + E` | `Ctrl + Alt + E` |
| Create Lab Entry | `Cmd + Opt + L` | `Ctrl + Alt + L` |
| Add Plasmid | `Cmd + Opt + P` | `Ctrl + Alt + P` |
| Add Primer | `Cmd + Opt + R` | `Ctrl + Alt + R` |
| Add Source | `Cmd + Opt + S` | `Ctrl + Alt + S` |
| Toggle Preview | `Cmd + E` | `Ctrl + E` |

---

## 🔌 Essential Plugins

### Core Plugins (Must Have):
- ✅ **Dataview** - Dynamic queries & displays
- ✅ **Dataviewjs** - JavaScript in Dataview blocks
- ✅ **Templater** - Advanced template engine
- ✅ **Calendar** - Visual date navigation
- ✅ **Periodic Notes** - Daily/weekly notes
- ✅ **Quick Add** - Quick capture prompts

### Scientific Plugins (Recommended):
- ✅ **Obsidian Zotero** - Citation management
- ✅ **Excalidraw** - Draw diagrams & structures
- ✅ **Mermaid** - Create flowcharts
- ✅ **LaTeX** - Mathematical equations
- ✅ **Natural Language Dates** - Parse date formats

### Productivity Plugins:
- ✅ **Tag Wrangler** - Manage tags
- ✅ **Obsidian Git** - Version control
- ✅ **Advanced Tables** - Enhanced markdown tables
- ✅ **Workspaces** - Save workspace layouts

---

## 📊 Vault Analytics

```dataviewjs
const allPages = dv.pages();
const totalNotes = allPages.length;
const experiments = dv.pages('"02-EXPERIMENTS"').length;
const molecularData = dv.pages('"03-MOLECULAR-DATABASE"').length;
const literature = dv.pages('"04-LITERATURE-HUB"').length;
const protocols = dv.pages('"05-PROTOCOLS"').length;

dv.paragraph(`
**📈 Vault Overview**

- **Total Notes:** ${totalNotes}
- **Experiments:** ${experiments}
- **Molecular Data:** ${molecularData}
- **Literature Sources:** ${literature}
- **Protocols:** ${protocols}
- **Last Updated:** ${dateformat(now(), "MMMM d, yyyy 'at' h:mm a")}
`);
```

---

## 🔄 Recently Modified Files

```dataviewjs
const recent = dv.pages()
  .sort(p => p.file.mtime, "desc")
  .limit(8);

dv.table(["File", "Modified Date"], 
  recent.map(f => [
    `[[${f.file.name}]]`,
    f.file.mtime ? dv.date.luxon.DateTime.fromJSDate(f.file.mtime).toLocaleString() : "N/A"
  ])
);
```

---

**🔐 Keep Your Lab Notes Organized & Secure**  
*Last Updated: `=dateformat(now(), "MMMM d, yyyy 'at' h:mm a")`*
