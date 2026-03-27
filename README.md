<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Zoology Community Verifier</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&family=Orbitron:wght@500;700&display=swap" rel="stylesheet">

<style>
body {
    margin: 0;
    font-family: 'Poppins', sans-serif;
    background: radial-gradient(circle at top, #1a1a2e, #0f3460, #000);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.container {
    width: 95%;
    max-width: 420px;
    padding: 25px;
    border-radius: 25px;
    background: rgba(255,255,255,0.05);
    backdrop-filter: blur(25px);
    box-shadow: 0 0 60px rgba(0,0,0,0.8);
    text-align: center;
}

input, select {
    width: 100%;
    padding: 14px;
    border-radius: 12px;
    border: none;
    margin-bottom: 10px;
    background: rgba(255,255,255,0.08);
    color: white;
}

button {
    width: 100%;
    padding: 14px;
    border-radius: 12px;
    border: none;
    background: linear-gradient(45deg,#00f2ff,#0072ff);
    color: white;
    font-weight: 600;
    margin-top: 8px;
    cursor: pointer;
}

.result {
    margin-top: 20px;
    font-size: 18px;
    font-weight: bold;
    padding: 12px;
    border-radius: 12px;
    background: rgba(255,255,255,0.05);
}

.history {
    margin-top: 15px;
    font-size: 12px;
    text-align:left;
}

.admin { display:none; margin-top:20px; }

/* LOGIN POPUP */
.login-box {
    position: fixed;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    display: none;
    justify-content: center;
    align-items: center;
}

.login-card {
    background: rgba(255,255,255,0.08);
    padding: 25px;
    border-radius: 15px;
    backdrop-filter: blur(20px);
    width: 280px;
    text-align: center;
}

/* NAME */
.footer {
    margin-top: 25px;
}

.dev-name {
    font-family: 'Orbitron', sans-serif;
    font-size: 22px;
    color: #ff2e2e;
    text-shadow: 0 0 10px red, 0 0 20px red;
}
</style>
</head>

<body>

<div class="container">

<h2>Zoology Community Verifier</h2>

<input id="numberInput" placeholder="Enter number">
<button onclick="verify()">Verify</button>

<div class="result" id="result"></div>

<button onclick="openWhatsApp()">Open WhatsApp</button>
<button onclick="showLogin()">Admin Access</button>

<div class="history" id="history"></div>

<!-- ADMIN PANEL -->
<div class="admin" id="adminPanel">
<h3>Admin Panel</h3>

<input id="newNumber" placeholder="Enter number to add">
<button onclick="addNumber()">Add Number</button>

<select id="numberList"></select>
<button onclick="removeSelected()">Remove Selected Number</button>

<input id="removeInput" placeholder="Enter number to remove">
<button onclick="removeByInput()">Remove Entered Number</button>

</div>

<div class="footer">
<div class="dev-name"> Developed By Abdullah Rajpar</div>
</div>

</div>

<!-- LOGIN -->
<div id="loginBox" class="login-box">
<div class="login-card">
<h3>Admin Login</h3>
<input type="password" id="adminPass" placeholder="Enter Password">
<button onclick="checkPassword()">Login</button>
</div>
</div>

<script>

// DATABASE
let approvedNumbers = [
"+923368626653","+923152531352","+923162114199","+923103519160","+923352170488",
"+923168902863","+923170290916","+923052184466","+923042402563","+923003995213",
"+923352246658","+923141077005","+923360351427","+923422300214","+923708411203",
"+923273622151","+923082612221","+923152754383","+923368866488","+923022192144",
"+923302352422","+923311355812","+923432123031","+923408652762","+923370751040",
"+923112887781","+923470666022","+923312495332","+923156460317","+923303693929",
"+923063754852","+923282044186","+923372530392","+923166419439","+923315449526",
"+923133795630","+923092501994","+923181199860","+923352398784","+923194853048",
"+923332445425","+923182750813","+923366398842","+923192030585","+923342742455",
"+923443918233","+923072521586","+923255928232","+923152971094","+923152429460",
"+923112483520","+923193540393","+923282172912","+923089249900","+923363362137",
"+923131237497","+923173639221","+923700292189","+923253034366","+923710105122",
"+923373255763","+923422069256","+923122017433","+923152730728","+923453377671",
"+923432907978","+923013312041","+923193767108","+923308672784","+923242798676",
"+923196348530","+923162825352","+923032482919","+923458082647","+923120305493",
"+923022642071","+923142318176","+923118057850","+923493457153","+923706207009",
"+923492967262","+923123358658","+923052846714","+923092998299","+923101167833",
"+923423019477","+923314446927","+923161024644","+923317584451","+923052618208",
"+923103072513","+923102933024","+923122808233","+923278452604","+923338595872",
"+923422743917","+923282937448","+923415554070","+923325431523","+923433240485",
"+923150981348","+923102782397","+923713198487","+923008912464","+923332963251",
"+923141197599","+923121112475","+923191486661","+923184740963","+923482332922",
"+923163794241","+923330377015","+923122688092","+923233465493","+923030880134",
"+923118214401","+923363895340","+923007050793","+923482555306","+923023499077",
"+923093721215","+923282839655","+923299270530","+923431165570","+923103023717",
"+923103154138","+923111091571","+923292573446","+923310200435","+923021399533",
"+923116483624","+923133712038","+923158705844","+923196882552","+923432062081",
"+923195149454"
];

// FORMAT
function format(num){
    num = num.replace(/\D/g,'');
    if(num.startsWith("92")) return "+92"+num.slice(2);
    if(num.startsWith("03")) return "+92"+num.slice(1);
    if(num.startsWith("3")) return "+92"+num;
    return "+92"+num;
}

// VERIFY
function verify(){
    let num = format(document.getElementById("numberInput").value);
    let result = document.getElementById("result");

    if(approvedNumbers.includes(num)){
        result.innerHTML="✅ This number belongs to the Zoology Community";
        result.style.color="#00ff9d";
    } else {
        result.innerHTML="❌ This number is NOT part of the Zoology Community";
        result.style.color="#ff4d4d";
    }

    saveHistory(num);
}

// HISTORY
function saveHistory(num){
    let history = JSON.parse(localStorage.getItem("history"))||[];
    history.unshift(num);
    history=history.slice(0,5);
    localStorage.setItem("history",JSON.stringify(history));

    document.getElementById("history").innerHTML =
    "<b>Recent:</b><br>"+history.join("<br>");
}

// WHATSAPP
function openWhatsApp(){
    let num = format(document.getElementById("numberInput").value);
    window.open("https://wa.me/"+num.replace("+",""));
}

// LOGIN
function showLogin(){
    document.getElementById("loginBox").style.display="flex";
}

function checkPassword(){
    let pass=document.getElementById("adminPass").value;
    if(pass==="rajpar456$#@"){
        document.getElementById("loginBox").style.display="none";
        document.getElementById("adminPanel").style.display="block";
        loadNumbers();
    } else {
        alert("Wrong Password ❌");
    }
}

// LOAD
function loadNumbers(){
    let select=document.getElementById("numberList");
    select.innerHTML="";
    approvedNumbers.forEach(num=>{
        let opt=document.createElement("option");
        opt.value=num;
        opt.textContent=num;
        select.appendChild(opt);
    });
}

// REMOVE SELECT
function removeSelected(){
    let selected=document.getElementById("numberList").value;
    approvedNumbers=approvedNumbers.filter(n=>n!==selected);
    alert("Removed ✅");
    loadNumbers();
}

// REMOVE INPUT
function removeByInput(){
    let input=document.getElementById("removeInput").value;
    let num=format(input);

    if(approvedNumbers.includes(num)){
        approvedNumbers=approvedNumbers.filter(n=>n!==num);
        alert("Removed ✅");
        loadNumbers();
    } else {
        alert("Not Found ❌");
    }
}

// ADD
function addNumber(){
    let num=format(document.getElementById("newNumber").value);
    approvedNumbers.push(num);
    alert("Added ✅");
    loadNumbers();
}

</script>

</body>
</html>
