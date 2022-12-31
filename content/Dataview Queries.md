
Link to any page that mentions a page
type: dataview
```
LIST FROM [[Note Name]]
```
Can also be AND joined to something else.

---

List from a directory given a tag value
type: dataview
```
LIST FROM "Books" WHERE completed = true
```
---

List all recently modified files
type: dataview
```
TABLE file.mtime AS "Last Modified" SORT file.mtime DESC LIMIT 5
```
---

See all the details of files that link to a page you want to see
type: dataview
```
TABLE file WHERE contains(file.outlinks, [[Note Name]])
```
---

Get all of the tasks about a page.
type: dataviewjs
```
dv.taskList(dv.pages().file.tasks
	.where(t => !t.completed)
	.where(t => t.text.includes("[[Note Name]]")))
```
---

Get all of the files that are tagged with a certain tag.
type: dataview
```
LIST WHERE contains(file.tags, "#blog")
```
---

Normal queries in Obsidian
type: query
```
tag:#reference
```
---

Get all the tasks with a tag from pages other than the one you are on
```
dv.taskList(dv.pages().filter(page => page.file.name !== dv.current().file.name).file.tasks
	.where(t => !t.completed)
	.where(t => t.text.includes("#todo/personal")))
```

---
When using GROUP BY, selecting data now has to be prepended by 'rows.'
```
TABLE rows.file.link AS "Note" FROM "Folder" GROUP BY completed AS "Completed"
```

---
To hide an index, use WITHOUT
```
TABLE WITHOUT id rows.file.link AS "Note" FROM "Folder" GROUP BY completed
```

---
Get all posts that start with a string and sort by metadata.
```
TABLE subtitle AS "Subtitle", posted AS "Posted" FROM "Folder" WHERE startswith(file.name, "LS") SORT posted DESC, file.name
```

Official Reference: https://blacksmithgu.github.io/obsidian-dataview
Forum: https://forum.obsidian.md/t/dataview-plugin-snippet-showcase/13673
Publish Reference: https://publish.obsidian.md/hub/04+-+Guides%2C+Workflows%2C+%26+Courses/Guides/An+Introduction+to+Dataview