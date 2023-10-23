<%* 
// Get current training plan && get the first ID ONLY
// Let's assume you track the current training plan as a link on a page called "🍏 Health" with the metadata tag "trainingplan"
let healthpage = DataviewAPI.page("🍏 Health");
let work = DataviewAPI.page(healthpage.trainingplan.path).work[0].split("/");

// Sample:
// 0/bench press/5x9(40%)/5x11(50%)/5x13(60%)/5x14(65%)/5x17(75%)/5x19(85%)

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

// This only works if it's the 5/3/1 plan and there are exactly 3 warmup sets and 3 work sets
for (let i=2; i < work.length; i++){
	// Since we start with 2 as the index but want 2 we just substract 1 from each iteration
	var setXreps = "set_" + (i-1) + "_reps"
	var setXweight = "set_" + (i-1) + "_weight"
	var setXpercentage = "set_" + (i-1) + "_percentage"
	todayswork[setXreps] = parseExercise(work[i])['reps'];
	todayswork[setXweight] = parseExercise(work[i])['weight'];
	todayswork[setXpercentage] = parseExercise(work[i])['percentage'];
};



-%>

## Todays work from the current plan: [[<% healthpage.trainingplan.path %>|<% healthpage.trainingplan.fileName() %>]]
trainingPlan::  [[<% healthpage.trainingplan.fileName() %>]]
cycleID:: <% todayswork['id'] %>
### Exercise:: <% todayswork['exercise'] %>
### Warm Up
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_1_reps'] %>x<% todayswork['set_1_weight'] %> kg (<% todayswork['set_1_percentage'] %>)
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_2_reps'] %>x<% todayswork['set_2_weight'] %> kg (<% todayswork['set_2_percentage'] %>)
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_3_reps'] %>x<% todayswork['set_3_weight'] %> kg (<% todayswork['set_3_percentage'] %>)
### Set 1 
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_4_reps'] %>x<% todayswork['set_4_weight'] %> kg (<% todayswork['set_4_percentage']%>)
### Set 2 
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_5_reps'] %>x<% todayswork['set_5_weight'] %> kg (<% todayswork['set_5_percentage']%>)
### Set 3
- [ ] <% todayswork['exercise'] %>:: <% todayswork['set_6_reps'] %>x<% todayswork['set_6_weight'] %> kg (<% todayswork['set_6_percentage']%>) <% todayswork['exercise'] %>ExtraReps::
