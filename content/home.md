---
cssclass: dashboard
cssclass: dashboard  
---
# Home
- Personal
	- `="[[" + dateformat(date(today), "yyyy-MM-dd") + "|Today ]]"`
	- [[notepad]]
	- [q3_2024](q3_2024.md)
	- [RHCSA Study Tracker](RHCSA%20Study%20Tracker.md)
	- [[Macro Tracker]]

- Content
	- [Linux](linux/_index.md)
	- [Networking](networking/_index.md)
	- [Tools](tools/_index.md)
	- [Python](python/_index.md)
	- [Writing](writing/_index.md)
	- [Book Notes](images/booknotes/_index.md)

- Work
	- [notepad](work/notepad.md)
	- [[todo]]
	- [[malcom-serenity]]
	- [[sharadupdates]]
	- [[oVirt to Proxmox]]


# Trackers
- ```tracker
searchType: frontmatter
searchTarget: weight
startDate: 2024-06-01
endDate: 2024-09-30
folder: personal/Journal/Daily
fixedScale: 0.75
line:
	lineColor: blue
	title: Weight Tracker
	yAxisLabel: Weight
	yAxisUnit: lbs
	xAxisLabel: Date
	
- ```tracker
searchType: frontmatter
searchTarget: caffeine
folder: personal/Journal/Daily
fixedScale: 0.75
startDate: 2024-09-01
endDate: 2024-09-30
line:
	lineColor: blue
	title: Caffeine Tracker
	yAxisLabel: caffeine
	yAxisUnit: mgs
	xAxisLabel: Date

- ```tracker
searchType: frontmatter
searchTarget: noalcohol
folder: personal/Journal/Daily
fixedScale: 1
datasetName: NoAlcohol
fixedScale: .8
month:
	startWeekOn: 'Mon'
	color: orange
summary:
	template: "Current Streak: {{currentStreak()}} day(s)"

- ```tracker
searchType: frontmatter
searchTarget: noweed
folder: personal/Journal/Daily
fixedScale: 1
datasetName: noweed
fixedScale: .8
month:
	startWeekOn: 'Mon'
	color: green
summary:
	template: "Current Streak: {{currentStreak()}} day(s)"

- ```tracker
searchType: frontmatter
searchTarget: nofap
folder: personal/Journal/Daily
fixedScale: 1
datasetName: NoFap
fixedScale: .8
month:
	startWeekOn: 'Mon'
	color: steelblue
summary:
	template: "Current Streak: {{currentStreak()}} day(s)"

- ```tracker
searchType: frontmatter
searchTarget: sex
folder: personal/Journal/Daily
fixedScale: 1
datasetName: sex
fixedScale: .8
month:
	startWeekOn: 'Mon'
	color: red
summary:
	template: "Current Streak: {{currentStreak()}} day(s)"






