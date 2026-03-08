<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>WellTrack BMI - SanJoseLitexSeniorHighSchool</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
font-family:'Poppins',sans-serif;
background:linear-gradient(to right,#ff9ec4,#ff9ec4);
display:flex;
justify-content:center;
align-items:center;
min-height:100vh;
margin:0;
padding:20px;
}

.card{
background:white;
padding:35px;
border-radius:20px;
box-shadow:0 10px 25px rgba(0,0,0,0.2);
width:95%;
max-width:950px;
text-align:center;
border:2px solid black;
max-height:90vh;
overflow:auto;
}

.header{
display:flex;
align-items:center;
justify-content:center;
gap:15px;
margin-bottom:20px;
}

.header img{
width:60px;
height:60px;
border-radius:10px;
}

.school-title h1{
margin:0;
color:hotpink;
}

.school-title p{
margin:0;
font-size:14px;
color:#555;
}

input{
width:100%;
max-width:400px;
padding:12px;
margin:10px auto;
border-radius:10px;
border:2px solid black;
background:#ffe6f5;
display:block;
box-sizing:border-box;
}

.password-container{
position:relative;
width:100%;
max-width:400px;
margin:auto;
}

.password-container input{
width:100%;
padding-right:40px;
}

.toggle-eye{
position:absolute;
right:12px;
top:50%;
transform:translateY(-50%);
cursor:pointer;
}

button{
padding:10px 20px;
border-radius:10px;
background:hotpink;
color:white;
border:none;
margin:5px;
cursor:pointer;
font-weight:600;
}

button:hover{
background:#ff5ca1;
}

table{
width:100%;
border-collapse:collapse;
margin-top:15px;
background:white;
}

th{
background:hotpink;
color:white;
padding:10px;
}

td{
padding:10px;
border:1px solid #ddd;
font-size:13px;
}

.chart-box{
max-width:700px;
margin:auto;
}

.Underweight{color:#2196f3;font-weight:bold;}
.Normal{color:#4caf50;font-weight:bold;}
.Overweight{color:#ff9800;font-weight:bold;}
.Obese{color:#f44336;font-weight:bold;}

</style>
</head>

<body>

<!-- FRONT PAGE -->

<div id="frontPage" class="card">

<div class="header">

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQvNcutHVwovKwxA6Ul2UJhbSLMbI0ElMvy02UE_sbCQky9hnmnl3c7fFE&s=10">

<div class="school-title">
<h1>WellTrack BMI</h1>
<p>SanJoseLitexSeniorHighSchool</p>
</div>

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIIgIbE3prBazJq_qX0sFnyRAEi7oWAZyfBp7ujJBIRw&s=10">

</div>

<button onclick="showPage('loginPage')">Login / Sign Up</button>
<button onclick="viewDashboard(false,false,true)" style="background:#555">Public Dashboard</button>

</div>

<!-- LOGIN -->

<div id="loginPage" class="card" style="display:none;">

<h2>Login</h2>

<input id="loginUser" placeholder="Surname-Section">

<div class="password-container">
<input type="password" id="loginPass" placeholder="Password">
<span class="toggle-eye" onclick="togglePass('loginPass')">👁</span>
</div>

<button onclick="login()">Login</button>

<p onclick="showPage('signupPage')" style="cursor:pointer;color:blue">Create Account</p>

<button onclick="showPage('frontPage')" style="background:#555">Back</button>

</div>

<!-- SIGNUP -->

<div id="signupPage" class="card" style="display:none;">

<h2>Sign Up</h2>

<input id="signupUser" placeholder="Surname-Section">

<div class="password-container">
<input type="password" id="signupPass" placeholder="Password">
<span class="toggle-eye" onclick="togglePass('signupPass')">👁</span>
</div>

<button onclick="signup()">Sign Up</button>

<button onclick="showPage('loginPage')">Back</button>

</div>

<!-- BMI PAGE -->

<div id="bmiPage" class="card" style="display:none;">

<h1>WellTrack BMI</h1>

<p>Logged in: <b id="displayUser"></b></p>

<input type="number" id="height" placeholder="Height (cm)">
<input type="number" id="weight" placeholder="Weight (kg)">

<button onclick="calculateBMI()">Calculate & Save</button>

<h3 id="result"></h3>

<hr>

<button onclick="showAnalytics()" style="background:#00bcd4">My Progress Chart</button>
<button onclick="viewDashboard(false,true)" style="background:#ba68c8">Manage My Records</button>
<button onclick="viewDashboard(false,false,true)" style="background:#ff9ec4">Public Dashboard</button>

<button id="adminBtn" style="display:none;background:#5e00ff" onclick="viewDashboard(true,false)">Admin Panel</button>

<button onclick="logout()" style="background:#555">Logout</button>

</div>

<!-- DASHBOARD -->

<div id="dashboardPage" class="card" style="display:none;">

<h2 id="dashTitle">BMI Records</h2>

<p id="studentCount"></p>

<input id="searchBox" placeholder="Search surname or section..." onkeyup="searchRecords()">

<table>

<thead>
<tr>
<th>Date</th>
<th>Surname</th>
<th>Section</th>
<th>BMI</th>
<th>Category</th>
<th id="actionCol" style="display:none">Actions</th>
</tr>
</thead>

<tbody id="tableBody"></tbody>

</table>

<br>

<button onclick="showStats()" style="background:#00bcd4">Class Statistics</button>

<p id="statsResult"></p>

<button onclick="exportCSV()" style="background:#4caf50">Export to Excel</button>

<button id="retrieveBtn" style="display:none;background:green" onclick="showDeleted()">Retrieve Records</button>

<br>

<button onclick="currentUser ? showPage('bmiPage') : showPage('frontPage')">Back</button>

</div>

<!-- ANALYTICS -->

<div id="analyticsPage" class="card" style="display:none;">

<h2>Your BMI Trend</h2>

<div class="chart-box">
<canvas id="bmiChart"></canvas>
</div>

<button onclick="showPage('bmiPage')">Back</button>

</div>

<script>

let users=JSON.parse(localStorage.getItem("users"))||{"admin":"admin123"}
let bmiRecords=JSON.parse(localStorage.getItem("bmiRecords"))||[]
let deletedRecords=JSON.parse(localStorage.getItem("deletedRecords"))||[]

let currentUser=""
let myChart=null

function showPage(id){
document.querySelectorAll(".card").forEach(c=>c.style.display="none")
document.getElementById(id).style.display="block"
}

function togglePass(id){
let x=document.getElementById(id)
x.type=x.type==="password"?"text":"password"
}

function signup(){
let u=signupUser.value.trim()
let p=signupPass.value.trim()
if(!u.includes("-")){alert("Use format: Surname-Section");return}
users[u]=p
localStorage.setItem("users",JSON.stringify(users))
alert("Account created!")
showPage("loginPage")
}

function login(){
let u=loginUser.value.trim()
let p=loginPass.value.trim()
if(users[u]===p){
currentUser=u
displayUser.innerText=u
adminBtn.style.display=(u==="admin")?"inline-block":"none"
showPage("bmiPage")
}else alert("Invalid login")
}

function getCategory(b){
if(b<18.5)return"Underweight"
if(b<24.9)return"Normal"
if(b<29.9)return"Overweight"
return"Obese"
}

function calculateBMI(){

let h=parseFloat(height.value)
let w=parseFloat(weight.value)

if(!h||!w){alert("Enter values");return}

let bmi=(w/((h/100)**2)).toFixed(2)
let cat=getCategory(bmi)

let p=currentUser.split("-")

bmiRecords.push({
owner:currentUser,
date:new Date().toLocaleDateString(),
surname:p[0],
section:p[1],
height:h,
weight:w,
bmi:bmi,
category:cat
})

localStorage.setItem("bmiRecords",JSON.stringify(bmiRecords))

result.innerText=`BMI: ${bmi} (${cat}) Saved`

}

function viewDashboard(admin,userOnly,publicView=false){

if(publicView)currentUser=""

showPage("dashboardPage")

dashTitle.innerText=publicView?"Public Dashboard":userOnly?"My Records":"Records Dashboard"

actionCol.style.display=(admin||userOnly)?"table-cell":"none"

retrieveBtn.style.display=(currentUser==="admin")?"inline-block":"none"

studentCount.innerText="Total Records: "+bmiRecords.length

tableBody.innerHTML=""

bmiRecords.forEach((r,i)=>{

if(userOnly&&r.owner!==currentUser)return

tableBody.innerHTML+=`<tr>

<td>${r.date}</td>
<td>${r.surname}</td>
<td>${r.section}</td>
<td>${r.bmi}</td>
<td class="${r.category}">${r.category}</td>

${(admin||(userOnly&&r.owner===currentUser))?

`<td>
<button onclick="updateRec(${i},${userOnly})" style="background:orange">Edit</button>
<button onclick="deleteRec(${i},${userOnly})" style="background:red">X</button>
</td>`:""}

</tr>`

})

}

function updateRec(i,userView){

let r=bmiRecords[i]

let sec=prompt("Section",r.section)
let h=prompt("Height",r.height)
let w=prompt("Weight",r.weight)

if(sec&&h&&w){

let bmi=(w/((h/100)**2)).toFixed(2)

bmiRecords[i]={...r,section:sec,height:h,weight:w,bmi:bmi,category:getCategory(bmi)}

localStorage.setItem("bmiRecords",JSON.stringify(bmiRecords))

viewDashboard(currentUser==="admin",userView)

}

}

function deleteRec(i,userView){

if(confirm("Delete record?")){

deletedRecords.push(bmiRecords[i])

bmiRecords.splice(i,1)

localStorage.setItem("bmiRecords",JSON.stringify(bmiRecords))
localStorage.setItem("deletedRecords",JSON.stringify(deletedRecords))

viewDashboard(currentUser==="admin",userView)

}

}

function showDeleted(){

tableBody.innerHTML=""
dashTitle.innerText="Deleted Records"

deletedRecords.forEach((r,i)=>{

tableBody.innerHTML+=`<tr>

<td>${r.date}</td>
<td>${r.surname}</td>
<td>${r.section}</td>
<td>${r.bmi}</td>
<td>${r.category}</td>

<td>
<button onclick="restoreRec(${i})" style="background:green">Retrieve</button>
</td>

</tr>`

})

}

function restoreRec(i){

bmiRecords.push(deletedRecords[i])

deletedRecords.splice(i,1)

localStorage.setItem("bmiRecords",JSON.stringify(bmiRecords))
localStorage.setItem("deletedRecords",JSON.stringify(deletedRecords))

showDeleted()

}

function searchRecords(){

let val=searchBox.value.toLowerCase()

document.querySelectorAll("#tableBody tr").forEach(row=>{

let s=row.children[1].innerText.toLowerCase()
let sec=row.children[2].innerText.toLowerCase()

row.style.display=(s.includes(val)||sec.includes(val))?"":"none"

})

}

function showStats(){

if(bmiRecords.length===0){
statsResult.innerText="No data"
return
}

let total=0

bmiRecords.forEach(r=>total+=parseFloat(r.bmi))

let avg=(total/bmiRecords.length).toFixed(2)

statsResult.innerText="Total Students: "+bmiRecords.length+" | Average BMI: "+avg

}

function exportCSV(){

let csv="Date,Surname,Section,BMI,Category\n"

bmiRecords.forEach(r=>{
csv+=`${r.date},${r.surname},${r.section},${r.bmi},${r.category}\n`
})

let blob=new Blob([csv],{type:"text/csv"})

let url=URL.createObjectURL(blob)

let a=document.createElement("a")

a.href=url
a.download="BMI_Records.csv"

a.click()

}

function showAnalytics(){

showPage("analyticsPage")

let data=bmiRecords.filter(r=>r.owner===currentUser)

let labels=data.map(r=>r.date)
let bmis=data.map(r=>r.bmi)

let ctx=document.getElementById("bmiChart")

if(myChart)myChart.destroy()

myChart=new Chart(ctx,{
type:"line",
data:{
labels:labels,
datasets:[{
label:"My BMI Trend",
data:bmis,
borderColor:"hotpink",
backgroundColor:"rgba(255,105,180,0.2)",
fill:true
}]
}
})

}

function logout(){

currentUser=""
showPage("frontPage")

}

</script>

</body>
</html>