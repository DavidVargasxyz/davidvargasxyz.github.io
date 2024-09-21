---
title: Ultimate Obsidian Setup Guide
---
https://www.youtube.com/watch?v=AatZl1Z_n-g&list=PL7oLu8NfQd84_gsyqBVSVgUmCCgcvSZMx&index=22
Settings > appearance > CSS Snippets > folder icon
https://github.com/TfTHacker/DashboardPlusPlus
https://github.com/TfTHacker/DashboardPlusPlus/blob/master/.obsidian/snippets/dashboard.css

```
/* Updated 2022-02-28 */

.dashboard {

padding-left: 25px !important;

padding-right: 25px !important;

padding-top: 20px !important;

}

.dashboard .markdown-preview-section {

max-width: 100%;

}

/* Title at top of the document */

.dashboard .markdown-preview-section .title {

top: 60px;

position: absolute;

font-size: 26pt !important;

font-weight: bolder;

letter-spacing: 8px;

}

.dashboard h1 {

border-bottom-style: dotted !important;

border-width: 1px !important;

padding-bottom: 3px !important;

}

.dashboard div > ul {

list-style: none;

display: flex;

column-gap: 50px;

flex-flow: row wrap;

}

.dashboard div > ul > li {

min-width: 250px;

width: 15%;

}
```

Paste into notepad > save as dashboard.css > add to snippets folder in .obsidian
enable the snippet.

Create a new page called home

add cssclass: dashboard to the [[YAML In Obsidian]] header.

Grab the dashboard++ markdown file from the repo

See video at 3:32 to turn readable line length on for only this page (if needed)

* Each list expands to the right

### Add a Banner

add banners plugin > create page called banners > drag and drop image file into their > open commands on page and type "banner" > select add from local image

can change banner height from the plugin

install homepage plugin
Opens obsidiann in homepage
set homepage > set homepage view to "reading view" > turn on "refresh dataview"
set up homepage hotkey to ctrl+h

## Obsidian todo
* to do list
	* checklist plugin
* importing notes
* FInish dashboard
* Book Library
* finance tracker
* Kanban board?
* learn to use kindle highlights plugin
* look up todoist plugin
* set up spaced repetition studying
* use vim in obsidian?

## Settings
- Made new daily notes go into journal folder
- Made attachments go into attachment folder
- Disable "add dates as link"
- Type "@" to choose today's date.
- edit > turn readable line length off

## Installed
* templater
* cmenu
* editing toolbar
* homepage
* banners
* book search
* tracker
* periodic notes
	* add week to calendar view
* dataview
* rollover daily todos
* Natural language dates
* recent files
* paste url into selection
* better word count
* kindle highlights
* spaced repetition
* plugin update tracker
* note refractor
* table of contents
* minimal theme settings
* emoji toolbar
* Editor syntax highlight
* calendar

### (Must have) Plugins

Settings search
- search settings through settings menu
Commander
- customize spacing on ribbon and every other menu and bar in obsidian and add/remove different commands
- set up macros
Recent Files
	creates tab of recent filesi
Paste URL into selection
	highlight text and paste URL into it
better word count
	shows words and characters
Kindle Highlights
	import and sync kindle highlights
spaced repetition
plugin update tracker
	icon in status bar when there are updates
	customize how much time to wait before a plugin prompts to update
	set to three to avoid bugs from fresh releases
note refractor
	break one note into multiple notes
	split by not headings
	attach selection to existing note
	create new note with preset perameters
	split selection using first line as filename
Table of Contents
janitor
	scan vault based on parameters for cleanup
Minimal theme settings
Emoji Shortcode
Editor syntax highlight

## Pull Text From Images
Use the Obsidian text extractor plugin to pull text from images to the clipboard.

https://github.com/scambier/obsidian-text-extractor

Install and enable. Then right click an image, press "Text Extractor" Then "Extract text to clipboard".

## How to use dataview

Dataview Documentation https://blacksmithgu.github.io/obsidian-dataview/annotation/add-metadata/
Blog post https://dannyhatcher.com/obsidian-dataview-for-beginners/

Install plugin > enable top 3 Js settings > turn on task settings

Three Back ticks with "dataview" next to them.`

```
```dataview
```


add table, list, task, or calendar

- Parameters are case sensitive

### Table

#### From
from "PerfectDarkMode/Books/Reading List"
- Us full filepath to grab file
- can use folder or file
	AND
		from PerfectDarkMode/Books/Reading List and perfectdarkmode
	OR
		add or for either or
		from PerfectDarkMode/Books/Reading List and perfectdarkmode
	"-"
		Add dash to exclude the specified results
			- from PerfectDarkMode/Books/Reading
	Stacking
		from PerfectDarkMode/Books/Reading List and perfectdarkmode or personal

#### Where
where file.name = "Reading List"
- Works in all 3 querys
- Pick specific file without changing the source
- Not equal
	- where file.name!= "Reading List"
	- 

file.size
	- add file size column

sort file.ctime (asc or desc)

Change names of table columns
- append with 
	- as "namehere"
	- table file.ctime as "Date Created"
Remove ID
	table without id file.link, file.ctime as "creation time"

```
\`\`\`dataview
table Evergreen_Notes
```

```
\`\`\`dataview
table file.ctime as "Date Created"
from "PerfectDarkMode/Books"
sort file.ctime desc
```

### List

```
\`\`\`dataview
list file.ctime
```

### Task
file.ctime does not work here
tasks ask for different information than tables or lists

Group tasks
	- group by status

```
\`\`\`dataview
task
group by status
```

[  ] makes a task (with a - before it)
- [x] Example task ✅ 2023-01-10
- [ ] Task 2
- [ ] Task 3


## How to use Templates

Insert date or time

{{date}}
{{time}}


How to use templates https://help.obsidian.md/Plugins/Templates

## Free Obsidian Sync Between Linux and Iphone
Create a cloudant account

https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_cloudant.md
https://1e7c1ce7-9c86-4e46-92c5-ec34ef47777f-bluemix.cloudantnosqldb.appdomain.cloud/