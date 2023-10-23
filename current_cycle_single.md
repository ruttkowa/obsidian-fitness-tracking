<%* 
console.log("Started the templater for the current 5/3/1 cycle, single version.")
// Get last training page with the current cycle id
let today = moment().format('YYYY-MM-DD');
let lastentry = DataviewAPI.pages("#physicalpractice").where(p => p.date < moment(today)).sort(k => k.date, 'desc').limit(1);

console.log("Fetched last entry from the tag physicalpractice: " + lastentry);
// Set ID 
let id = parseInt(lastentry.values[0].cycleID) + 1

console.log("New ID for this work is: " + id)
console.log("Querying current training plan and exercise")
// Get current training plan 
let healthpage = DataviewAPI.page("🍏 Health");
let work = DataviewAPI.page(healthpage.trainingplan.path).work[id].split("/");
let exercise = DataviewAPI.page(work[1]);
// Sample:
// 0/bench press/5x9(40%)/5x11(50%)/5x13(60%)/5x14(65%)/5x17(75%)/5x19(85%)
console.log("Current training plan can be found at: " + healthpage.trainingplan.path);
console.log("The exercise is: " + work[1]);

// Basic setup which is always the same
let todayswork = {};
todayswork['id'] = work[0];
todayswork['next'] = parseInt(work[0])+ 1;
todayswork['exercise'] = work[1];


// Split the individual sets to get reps, weight and percentage
function parseExercise(string) {
	// Example, starting with 5x9(40%)
	// Reps ['5', '9(40%)']
	h = {}
	h['reps'] = parseInt(string.split("x")[0]);
	// Weight ['9', '40%)']
	h['weight'] = parseInt(string.split("x")[1].split("(")[0]);
	// Percentage 
	h['percentage'] = string.split("x")[1].split("(")[1].slice(0, -1);
	return h;
};

console.log("Parsing exercises and building todays work");
// This only works if it's the 5/3/1 plan and there are exactly 3 warmup sets and 3 work sets
// We need to start with 2 as the index because the first position in the array is the ID and the second is the exercise name

for (let i=2; i < work.length; i++){
	// Since we start with 2 as the index but want 2 we just substract 1 from each iteration
	var setXreps = "set_" + (i-1) + "_reps"
	var setXweight = "set_" + (i-1) + "_weight"
	var setXpercentage = "set_" + (i-1) + "_percentage"
	todayswork[setXreps] = parseExercise(work[i])['reps'];
	todayswork[setXweight] = parseExercise(work[i])['weight'];
	todayswork[setXpercentage] = parseExercise(work[i])['percentage'];
};
console.log("Parsing finished. Todays work is:");
console.log(todayswork);
console.log("------------");
console.log("Starting output");
console.log("------------");

-%>

## Todays work from the current plan: [[<% healthpage.trainingplan.path %>|<% healthpage.trainingplan.fileName() %>]]
trainingPlan::  [[<% healthpage.trainingplan.fileName() %>]]
cycleID:: <% todayswork['id'] %>
### Exercise:: <% todayswork['exercise'] %>
### Mobility
*A selection of possible mobilizations do to before the main exercise.*
<%*  
// exercise mobility loop
for (let i=0; i < exercise.preMobility.length; i++){
-%>
- <% exercise.preMobility[i] %>
<%*  
// end work for loop
}
-%>
### Warm Up
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_1_reps'] %>x<% todayswork['set_1_weight'] %> kg (<% todayswork['set_1_percentage'] %>)
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_2_reps'] %>x<% todayswork['set_2_weight'] %> kg (<% todayswork['set_2_percentage'] %>)
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_3_reps'] %>x<% todayswork['set_3_weight'] %> kg (<% todayswork['set_3_percentage'] %>)
### Set 1 
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_4_reps'] %>x<% todayswork['set_4_weight'] %> kg (<% todayswork['set_4_percentage']%>)
### Set 2 
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_5_reps'] %>x<% todayswork['set_5_weight'] %> kg (<% todayswork['set_5_percentage']%>)
### Set 3
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_6_reps'] %>x<% todayswork['set_6_weight'] %> kg (<% todayswork['set_6_percentage']%>), <% todayswork['exercise'] %>ExtraReps::

### Assistance

Possible assistance work:
<%*  
// exercise mobility loop
for (let i=0; i < exercise.assistance.length; i++){
-%>
- <% exercise.assistance[i] %>
<%*  
// end work for loop
}
-%>
