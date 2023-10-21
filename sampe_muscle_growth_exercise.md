---
name: backsquat
bodyCategory:
  - lower body
  - core
  - total body
bodyFocusArea:
  - quads
  - abdominals
exerciseType:
  - Resistance
  - Skill
exerciseSubType: SpeedAndExplosive
url: 
input: 
pushPull: push
doableAtHome: true
equipment:
  - barbell
  - weights
preMobility:
  - Olympic Wall Squat
  - Classic calf stretch mobilization
  - Banded super frog
postMobility: []
defaultGoal: reps
tags:
  - gpp-protocol
benchmark:
  - 2023-09-18/1RM/90/estimated
  - 2022-08-12/2RM/75/fictional
  - 2021-08-22/5RM/50/fictional
assistance:
  - Lunges
---
# Benchmarks
## Calculations
### 5/3/1 formula
### Find out training RM based on estimated RM 
```
(weight * reps * 0.0333 + weight) * 0.9
```
```dataviewjs
dv.header(3, "Auto calculated 5/3/1 Training RM calculated from last 1RM")

let pg = dv.current();
let benchmark_list = pg.benchmark

// Hacky workaround to sort by date
const sorted = benchmark_list.toSorted()
const lastRM = sorted.last();

// Build the benchmark hash for easier access
var h = {};
h['date'] = lastRM.split("/", 4)[0];
h['rm'] = lastRM.split("/", 4)[1];
h['weight'] = lastRM.split("/", 4)[2];
h['note'] = lastRM.split("/", 4)[3];

const weight = parseInt(h['weight']);

// Extracts the number from xRM
const reps = parseInt(h['rm'].substring(0,1));

let trainingRM = Math.round((weight * reps * 0.0333 + weight) * 0.9);

dv.paragraph("Calculated Training 1RM: " + trainingRM + "kg");
dv.paragraph("(Estimation based on " + h['rm'] + " / " + h['weight'] + "kg, from " + h['date'] + ")")
```
```dataviewjs
let pg = dv.current();
let benchmark_list = pg.benchmark
// Hacky workaround to sort by date
const sorted = benchmark_list.toSorted().reverse()

// Build the benchmark array

const benchmarkArray = [];

for (let i=0; i < sorted.length; i++){
	let row = [];
	// Date
	row.push(sorted[i].split("/", 1));
	// RM
	row.push(sorted[i].split("/", 2)[1]);
	// weight
	row.push(sorted[i].split("/", 3)[2]);
	// note
	row.push(sorted[i].split("/", 4)[3]);
	benchmarkArray.push(row);
}

dv.header(1, "All benchmarks(sorted)")
dv.table (
	// Header
	["Date", "1RM/3RM/5RM", "Weight in kg", "Note" ],
	// Data
	benchmarkArray
	.sort(k => k.date, 'desc')
)

```


