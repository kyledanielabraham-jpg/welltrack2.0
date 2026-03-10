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
background:linear-gradient(135deg, #ff9ec4 0%, #ffc1d6 100%);
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
border-radius:24px;
box-shadow:0 15px 35px rgba(0,0,0,0.15);
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
margin-bottom:25px;
}

.header img{
width:60px;
height:60px;
border-radius:12px;
}

.school-title h1{
margin:0;
color:hotpink;
letter-spacing: -1px;
}

/* --- SIDE-BY-SIDE INPUT LAYOUT --- */
.bmi-inputs-row {
    display: flex;
    gap: 15px;
    max-width: 600px;
    margin: 20px auto;
}

.modern-input-group {
    flex: 1; /* Makes both Height and Weight take equal width */
    display: flex;
    align-items: center;
    background: #fff5fa;
    border: 2px solid black;
    border-radius: 12px;
    transition: all 0.3s ease;
    overflow: hidden;
    height: 55px;
}

.modern-input-group:focus-within {
    border-color: hotpink;
    box-shadow: 0 0 12px rgba(255, 105, 180, 0.25);
    background: white;
}

.input-area {
    flex: 1;
    display: flex;
    height: 100%;
}

.modern-input-group input {
    flex: 1;
    border: none !important;
    background: transparent !important;
    padding: 0 15px;
    font-family: inherit;
    font-size: 16px;
    margin: 0 !important;
    outline: none !important;
    height: 100%;
    min-width: 0;
}

.unit-switch {
    background: #ffd6eb;
    border: none;
    border-left: 2px solid black;
    padding: 0 5px;
    height: 100%;
    width: 75px; /* Compact width for side-by-side look */
    font-family: inherit;
    font-weight: 600;
    cursor: pointer;
    color: #333;
    text-align: center;
    outline: none;
}

/* Imperial ft/in split inside the small box */
#ftInput {
    display: none;
    width: 100%;
}

#ftInput input {
    text-align: center;
    padding: 0 !important;
}

#ftInput input:first-child {
    border-right: 1px solid rgba(0,0,0,0.1) !important;
}

/* Responsive: Stack them on mobile */
@media (max-width: 600px) {
    .bmi-inputs-row {
        flex-direction: column;
    }
}

/* Standard inputs for other pages */
input:not(.modern-input-group input){
width:100%;
max-width:400px;
padding:14px;
margin:10px auto;
border-radius:12px;
border:2px solid black;
background:#fff5fa;
display:block;
box-sizing:border-box;
}

button{
padding:12px 24px;
border-radius:12px;
background:hotpink;
color:white;
border:none;
margin:8px;
cursor:pointer;
font-weight:600;
transition: 0.2s;
}

button:hover{
background:#ff5ca1;
transform: scale(1.02);
}

table{ width:100%; border-collapse:collapse; margin-top:20px; }
th{ background:hotpink; color:white; padding:12px; }
td{ padding:12px; border:1px solid #eee; font-size:14px; }

.Underweight{color:#2196f3;font-weight:bold;}
.Normal{color:#4caf50;font-weight:bold;}
.Overweight{color:#ff9800;font-weight:bold;}
.Obese{color:#f44336;font-weight:bold;}
</style>
</head>

<body>

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

<div id="loginPage" class="card" style="display:none;">
    <h2>Login</h2>
    <input id="loginUser" placeholder="Surname-Section">
    <input type="password" id="loginPass" placeholder="Password">
    <button onclick="login()">Login</button>
    <p onclick="showPage('signupPage')" style="cursor:pointer;color:blue">Need an account? Sign Up</p>
    <button onclick="showPage('frontPage')" style="background:#555">Back</button>
</div>

<div id="signupPage" class="card" style="display:none;">
    <h2>Sign Up</h2>
    <input id="signupUser" placeholder="Surname-Section">
    <input type="password" id="signupPass" placeholder="Password">
    <button onclick="signup()">Sign Up</button>
    <button onclick="showPage('loginPage')">Back</button>
</div>

<div id="bmiPage" class="card" style="display:none;">
    <h1>WellTrack BMI</h1>
    <p>Logged in: <b id="displayUser"></b></p>

    <div class="bmi-inputs-row">
        <div class="modern-input-group">
            <div class="input-area">
                <div id="cmInput" style="width:100%; height:100%;">
                    <input type="number" id="height" placeholder="Height">
                </div>
                <div id="ftInput" style="height:100%;">
                    <input type="number" id="hFt" placeholder="Ft">
                    <input type="number" id="hIn" placeholder="In">
                </div>
            </div>
            <select id="hUnit" class="unit-switch" onchange="toggleInputs()">
                <option value="cm">cm</option>
                <option value="ft">ft/in</option>
            </select>
        </div>

        <div class="modern-input-group">
            <div class="input-area">
                <input type="number" id="weight" placeholder="Weight">
            </div>
            <select id="wUnit" class="unit-switch">
                <option value="kg">kg</option>
                <option value="lbs">lbs</option>
            </select>
        </div>
    </div>

    <button onclick="calculateBMI()" style="width:100%; max-width:600px;">Calculate & Save</button>
    <h3 id="result"></h3>

    <hr style="border:0; border-top:1px solid #eee; margin:25px 0;">
    <button onclick="showAnalytics()" style="background:#00bcd4">Progress Chart</button>
    <button onclick="viewDashboard(false,true)" style="background:#ba68c8">My Records</button>
    <button id="adminBtn" style="display:none;background:#5e00ff" onclick="viewDashboard(true,false)">Admin Panel</button>
    <button onclick="logout()" style="background:#555">Logout</button>
</div>

<div id="dashboardPage" class="card" style="display:none;">
    <h2 id="dashTitle">Records</h2>
    <input id="searchBox" placeholder="Search..." onkeyup="searchRecords()">
    <table>
        <thead>
            <tr><th>Date</th><th>Surname</th><th>Section</th><th>BMI</th><th>Category</th><th id="actionCol" style="display:none">Action</th></tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>
    <br>
    <button onclick="showStats()" style="background:#00bcd4">Show Average</button>
    <p id="statsResult"></p>
    <button onclick="currentUser ? showPage('bmiPage') : showPage('frontPage')" style="background:#555">Back</button>
</div>

<div id="analyticsPage" class="card" style="display:none;">
    <h2>BMI History</h2>
    <canvas id="bmiChart"></canvas>
    <button onclick="showPage('bmiPage')" style="margin-top:20px;">Back</button>
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

function toggleInputs() {
    let hUnit = document.getElementById("hUnit").value;
    document.getElementById("cmInput").style.display = (hUnit === "cm") ? "block" : "none";
    document.getElementById("ftInput").style.display = (hUnit === "ft") ? "flex" : "none";
}

function signup(){
    let u=signupUser.value.trim(); let p=signupPass.value.trim();
    if(!u.includes("-")){alert("Format: Surname-Section");return}
    users[u]=p; localStorage.setItem("users",JSON.stringify(users));
    alert("Account created!"); showPage("loginPage")
}

function login(){
    let u=loginUser.value.trim(); let p=loginPass.value.trim();
    if(users[u]===p){
        currentUser=u; displayUser.innerText=u;
        adminBtn.style.display=(u==="admin")?"inline-block":"none";
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
    let hCm, wKg;
    let hUnit = document.getElementById("hUnit").value;
    let wUnit = document.getElementById("wUnit").value;
    let weightVal = parseFloat(weight.value);

    if(hUnit === "cm") {
        hCm = parseFloat(height.value);
    } else {
        let ft = parseFloat(document.getElementById("hFt").value) || 0;
        let inch = parseFloat(document.getElementById("hIn").value) || 0;
        hCm = (ft * 30.48) + (inch * 2.54);
    }

    wKg = (wUnit === "lbs") ? (weightVal / 2.20462) : weightVal;

    if(!hCm || !wKg){alert("Enter height and weight");return}

    let bmi=(wKg/((hCm/100)**2)).toFixed(2)
    let cat=getCategory(bmi)
    let p=currentUser.split("-")

    bmiRecords.push({
        owner:currentUser, date:new Date().toLocaleDateString(),
        surname:p[0]||"User", section:p[1]||"N/A",
        height:hCm.toFixed(1), weight:wKg.toFixed(1),
        bmi:bmi, category:cat
    })

    localStorage.setItem("bmiRecords",JSON.stringify(bmiRecords))
    result.innerHTML=`Result: <span class="${cat}">${bmi} (${cat})</span>`;
}

function viewDashboard(admin,userOnly,publicView=false){
    if(publicView)currentUser=""
    showPage("dashboardPage")
    actionCol.style.display=(admin||userOnly)?"table-cell":"none"
    tableBody.innerHTML=""
    bmiRecords.forEach((r,i)=>{
        if(userOnly&&r.owner!==currentUser)return
        tableBody.innerHTML+=`<tr>
            <td>${r.date}</td><td>${r.surname}</td><td>${r.section}</td>
            <td>${r.bmi}</td><td class="${r.category}">${r.category}</td>
            ${(admin||(userOnly&&r.owner===currentUser))?
            `<td><button onclick="deleteRec(${i},${userOnly})" style="background:red; padding:5px;">X</button></td>`:""}
        </tr>`
    })
}

function deleteRec(i,userView){
    if(confirm("Delete?")){
        bmiRecords.splice(i,1)
        localStorage.setItem("bmiRecords",JSON.stringify(bmiRecords))
        viewDashboard(currentUser==="admin",userView)
    }
}

function searchRecords(){
    let val=searchBox.value.toLowerCase()
    document.querySelectorAll("#tableBody tr").forEach(row=>{
        row.style.display=(row.innerText.toLowerCase().includes(val))?"":"none"
    })
}

function showStats(){
    if(bmiRecords.length===0)return
    let total=0; bmiRecords.forEach(r=>total+=parseFloat(r.bmi));
    statsResult.innerText="Average BMI: "+(total/bmiRecords.length).toFixed(2);
}

function showAnalytics(){
    showPage("analyticsPage")
    let data=bmiRecords.filter(r=>r.owner===currentUser)
    let ctx=document.getElementById("bmiChart")
    if(myChart)myChart.destroy()
    myChart=new Chart(ctx,{
        type:"line",
        data:{
            labels:data.map(r=>r.date),
            datasets:[{label:"BMI Trend", data:data.map(r=>r.bmi), borderColor:"hotpink", tension:0.3}]
        }
    })
}

function logout(){currentUser=""; showPage("frontPage");}
</script>

</body>
</html>
