---
created: 2026-05-08 01:32
updated:
tags:
  - home
  - dashboard
status: active
---

# 🏠 Home Dashboard

> Mission: Build world-class backend systems. Document. Connect. Revise. Ship.

---

```dataview
TASK
FROM "01_System_Design" 
OR "02_Backend_Engineering"
OR "03_Spring_Boot"
OR "04_Databases"
OR "05_Kafka"
OR "06_AWS"
OR "07_Kubernetes"
WHERE !completed
SORT contains(text, "#todo/high") DESC,
     contains(text, "#todo/medium") DESC,
     contains(text, "#todo/low") DESC,
     file.mtime DESC
```
## ⚡ Command Center

| Area | Open |
|---|---|
| 🧭 System Design | [[01_System_Design]] |
| 🧱 Backend Engineering | [[02_Backend_Engineering]] |
| 🍃 Spring Boot | [[03_Spring_Boot]] |
| 🗄️ Databases | [[04_Databases]] |
| 📨 Kafka | [[05_Kafka]] |
| ☁️ AWS | [[06_AWS]] |
| 🚢 Kubernetes | [[07_Kubernetes]] |
| 🎯 Interview Q&A | [[08_Interview_QA]] |
| 🗺️ Diagrams | [[09_Diagrams]] |
| 📚 Print Books | [[10_Print_Books]] |
| 📅 Daily Logs | [[11_Daily_Logs]] |

---

## 📊 Vault Snapshot

```dataviewjs
const pages = dv.pages().where(p => !p.file.path.startsWith("90_Templates") && !p.file.path.startsWith("99_Attachments"));
const totalNotes = pages.length;
const activeNotes = pages.where(p => p.status == "active").length;
const reviewNotes = pages.where(p => p["next-review"]).length;
const tasks = pages.file.tasks;
const openTasks = tasks.where(t => !t.completed).length;

dv.table(
  ["Metric", "Value"],
  [
    ["Total Notes", totalNotes],
    ["Active Notes", activeNotes],
    ["Notes With Review Date", reviewNotes],
    ["Open Tasks", openTasks]
  ]
);
```

---

## 🔥 Recently Worked On — Top 5

```dataview
TABLE WITHOUT ID
  file.link AS "File",
  regexreplace(file.folder, "^.*?/","") AS "Folder",
  file.mtime AS "Last Edited",
  choice(
    date(today) = date(file.mtime),
    "Today",
    (date(today) - date(file.mtime)).days + " days ago"
  ) AS "Age"
FROM ""
WHERE !startswith(file.path, "90_Templates")
  AND !startswith(file.path, "99_Attachments")
SORT file.mtime DESC
LIMIT 5
```

---

## ✅ Active To-Dos

```tasks
not done
sort by priority
sort by due
limit 15
```

---

## 🎯 Today’s Focus

- [ ] 
- [ ] 
- [ ] 

> Rule: One deep concept. One implementation step. One revision.

---

## 🧠 Knowledge Areas — Notes Count

```dataviewjs
const folders = [
  "01_System_Design",
  "02_Backend_Engineering",
  "03_Spring_Boot",
  "04_Databases",
  "05_Kafka",
  "06_AWS",
  "07_Kubernetes",
  "08_Interview_QA",
  "09_Diagrams",
  "10_Print_Books",
  "11_Daily_Logs"
];

const rows = folders.map(folder => {
  const count = dv.pages(`"${folder}"`).length;
  const bar = "█".repeat(Math.min(count, 20));
  return [folder, count, bar];
});

dv.table(["Folder", "Notes", "Progress"], rows);
```

---

## 🏷️ Top Tags

```dataviewjs
const tagMap = new Map();

for (const page of dv.pages()) {
  if (!page.file.tags) continue;
  for (const tag of page.file.tags) {
    tagMap.set(tag, (tagMap.get(tag) ?? 0) + 1);
  }
}

const rows = [...tagMap.entries()]
  .sort((a, b) => b[1] - a[1])
  .slice(0, 10)
  .map(([tag, count]) => [tag, count]);

dv.table(["Tag", "Count"], rows);
```

---

## 🗓️ Recently Created — Last 7 Days

```dataview
TABLE WITHOUT ID
  file.link AS "File",
  file.folder AS "Folder",
  file.ctime AS "Created",
  choice(
    date(today) = date(file.ctime),
    "Today",
    (date(today) - date(file.ctime)).days + " days ago"
  ) AS "Age"
FROM ""
WHERE file.ctime >= date(today) - dur(7 days)
  AND !startswith(file.path, "90_Templates")
  AND !startswith(file.path, "99_Attachments")
SORT file.ctime DESC
LIMIT 10
```

---

## 🧩 System Design Revision Map

| Topic | Link | Status |
|---|---|---|
| Load Balancing | [[Load Balancing]] | ⬜ |
| Indexing | [[Indexing]] | ⬜ |
| Caching | [[Caching]] | ⬜ |
| Distributed Transactions | [[Distributed Transactions]] | ⬜ |
| Kafka | [[Kafka]] | ⬜ |
| Rate Limiting | [[Rate Limiting]] | ⬜ |
| API Gateway | [[API Gateway]] | ⬜ |
| Sharding | [[Sharding]] | ⬜ |
| Replication | [[Replication]] | ⬜ |
| Consistency Models | [[Consistency Models]] | ⬜ |

---

## 📚 Print / Revision Books

```dataview
TABLE WITHOUT ID
  file.link AS "Book",
  book-topic AS "Topic",
  status AS "Status",
  file.mtime AS "Updated"
FROM "10_Print_Books"
SORT file.mtime DESC
```

---

## 🔁 Upcoming Reviews

```dataview
TABLE WITHOUT ID
  file.link AS "File",
  next-review AS "Next Review",
  review-priority AS "Priority",
  last-reviewed AS "Last Reviewed"
FROM ""
WHERE next-review
SORT next-review ASC
LIMIT 10
```

---

## 🧭 Learning Pipeline

```dataview
TABLE WITHOUT ID
  file.link AS "Topic",
  status AS "Status",
  priority AS "Priority",
  difficulty AS "Difficulty"
FROM ""
WHERE status
  AND !startswith(file.path, "90_Templates")
  AND !startswith(file.path, "99_Attachments")
SORT priority DESC, file.mtime DESC
LIMIT 20
```

---

## 🧰 Quick Create

| Need | Create In |
|---|---|
| New system design topic | `01_System_Design` |
| New backend concept | `02_Backend_Engineering` |
| New Spring Boot note | `03_Spring_Boot` |
| New database topic | `04_Databases` |
| New Kafka topic | `05_Kafka` |
| New AWS service note | `06_AWS` |
| New Kubernetes note | `07_Kubernetes` |
| New interview Q&A | `08_Interview_QA` |
| New case study | `09_Diagrams` |
| New printable book | `10_Print_Books` |
| New daily log | `11_Daily_Logs` |

---

## 🩺 Vault Health

```dataviewjs
const all = dv.pages();
const orphan = all.where(p => p.file.outlinks.length === 0 && !p.file.path.startsWith("90_Templates")).length;
const templates = dv.pages('"90_Templates"').length;
const attachments = dv.pages('"99_Attachments"').length;

dv.table(
  ["Check", "Value"],
  [
    ["Orphan Notes", orphan],
    ["Templates", templates],
    ["Attachment Notes", attachments],
    ["Total Files", all.length]
  ]
);
```

---

## 🧠 Reflection

### What am I currently trying to understand deeply?

### What keeps confusing me?

### What should I revise next?

---

> “The best engineers are not just good at building systems; they are good at understanding systems deeply.”
