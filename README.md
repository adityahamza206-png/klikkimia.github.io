<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Klik Kimia 🖱️🧪</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&family=Roboto&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body { margin:0; font-family:'Roboto'; background:#f0f4f8; }
header { background:white; padding:15px; text-align:center; font-family:Poppins; font-size:24px; display:flex; justify-content:center; align-items:center; gap:10px; }
.container { padding:20px; }
.grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(180px,1fr)); gap:15px; }
.card { padding:25px; border-radius:15px; color:white; cursor:pointer; text-align:center; transition:0.3s; }
.card:hover{ transform:scale(1.05); box-shadow:0 5px 15px rgba(0,0,0,0.2);}
.card i { font-size:36px; margin-bottom:10px; display:block; }
.blue{background:linear-gradient(135deg,#3b82f6,#1e3a8a);}
.green{background:linear-gradient(135deg,#10b981,#065f46);}
.purple{background:linear-gradient(135deg,#8b5cf6,#4c1d95);}
.orange{background:linear-gradient(135deg,#f97316,#7c2d12);}
.hidden{display:none;}
.box { padding:15px; border-radius:12px; margin-top:15px; color:white; cursor:pointer; transition:0.3s;}
.box:hover{opacity:0.95;}
.b1{background:#3b82f6;}
.b2{background:#10b981;}
.b3{background:#f59e0b;}
.b4{background:#8b5cf6;}
.b5{background:#6366f1;}
.box-content{display:none; margin-top:10px; color:#fff;}
.option{display:block;margin:5px 0;}
button{margin-top:10px;padding:10px;border:none;border-radius:8px;background:#1d4ed8;color:white;cursor:pointer;}
button:hover{background:#2563eb;}
footer{text-align:center;margin-top:30px;padding:15px;}
/* pH Graph */
#phGraph{width:100%;height:200px;background:#e2e8f0;border-radius:12px;margin-top:10px;position:relative;}
.ph-bar{position:absolute;bottom:0;height:100%;width:5px;background:#f97316;transition:height 0.3s;}
</style>
</head>
<body>

<header><i class="fa-solid fa-hand-pointer"></i> Klik Kimia</header>

<div class="container">

<div id="dashboard">
<h2>Dashboard</h2>
<div class="grid">
<div class="card blue" onclick="openMateri('hidrolisis')"><i class="fa-solid fa-droplet"></i>Hidrolisis</div>
<div class="card green" onclick="openMateri('buffer')"><i class="fa-solid fa-scale-balanced"></i>Buffer</div>
<div class="card purple" onclick="openMateri('titrasi')"><i class="fa-solid fa-vial"></i>Titrasi</div>
<div class="card orange" onclick="openMateri('koloid')"><i class="fa-solid fa-microscope"></i>Koloid</div>
</div>
</div>

<div id="materi" class="hidden">
<button onclick="back()">⬅ Kembali</button>
<h2 id="judul"></h2>
<div id="materiContent"></div>

<div class="box b5">
<h3>Simulasi pH Interaktif</h3>
<input type="range" id="phSlider" min=0 max=14 step=0.1 value=7 oninput="updatePH()">
<p>pH: <span id="phValue">7</span></p>
<div id="phGraph"></div>
</div>

<div class="box b5">
<h3>Contoh Soal</h3>
<form id="quizForm"></form>
<button onclick="cekJawaban()">Cek Jawaban</button>
<div id="pembahasan"></div>
</div>
</div>

</div>

<footer>Dibuat oleh <b>Aditya Rahman</b> - Kelas 11-1</footer>

<script>
// Global soal
let soalData=[];

// Materi database
const materiDB = {
"hidrolisis": {
judul:"HIDROLISIS",
sub:[
{title:"Pengertian", color:"b1", icon:"fa-circle-info", content:"Reaksi ion garam dengan air yang menghasilkan H+ atau OH- sehingga mempengaruhi pH larutan."},
{title:"Jenis", color:"b2", icon:"fa-list", content:"Hidrolisis sebagian & hidrolisis total."},
{title:"Rumus Penting", color:"b3", icon:"fa-flask", content:"Ka x Kb = Kw"},
{title:"Konsep Inti", color:"b4", icon:"fa-lightbulb", content:"Ion bereaksi dengan air → terbentuk H+ atau OH- → pH berubah"},
{title:"Contoh & Fakta", color:"b1", icon:"fa-atom", content:"NH4Cl → larutan asam; NaCl → netral. Banyak garam dalam industri makanan."}
],
soal:[
{q:"Larutan NH4Cl 0,1 M memiliki sifat...", o:["Basa kuat","Netral","Asam","Basa lemah"], a:2, p:"NH4+ terhidrolisis menghasilkan H+ sehingga larutan bersifat asam."},
{q:"Garam yang mengalami hidrolisis total adalah...", o:["NaCl","NH4Cl","CH3COONa","NH4CN"], a:3, p:"NH4+ dan CN- sama-sama terhidrolisis sepenuhnya."}
]
},
"buffer": {
judul:"LARUTAN BUFFER",
sub:[
{title:"Pengertian", color:"b1", icon:"fa-circle-info", content:"Larutan yang dapat mempertahankan pH saat ditambah asam atau basa."},
{title:"Jenis", color:"b2", icon:"fa-list", content:"Buffer asam & buffer basa."},
{title:"Rumus Penting", color:"b3", icon:"fa-flask", content:"pH = pKa + log([A-]/[HA])"},
{title:"Konsep Inti", color:"b4", icon:"fa-lightbulb", content:"Buffer menahan perubahan pH menggunakan pasangan asam lemah & basa konjugasinya."},
{title:"Contoh & Fakta", color:"b1", icon:"fa-atom", content:"Darah manusia, larutan farmasi, buffer industri."}
],
soal:[
{q:"Jika [A-]=[HA], maka pH larutan buffer adalah...", o:["pKa","2pKa","pKa/2","pH=7"], a:0, p:"Perbandingan 1:1 → log=0 → pH = pKa"},
{q:"Buffer asam ideal terdiri dari...", o:["Asam kuat","Basa kuat","Asam lemah + garam","Garam saja"], a:2, p:"Buffer asam dibentuk dari asam lemah + garamnya."}
]
},
"titrasi": {
judul:"TITRASI",
sub:[
{title:"Pengertian", color:"b1", icon:"fa-circle-info", content:"Metode untuk menentukan konsentrasi larutan dengan menambahkan larutan standar hingga titik ekuivalen."},
{title:"Jenis", color:"b2", icon:"fa-list", content:"Titrasi asam-basa, titrasi redoks."},
{title:"Rumus Penting", color:"b3", icon:"fa-flask", content:"M1V1 = M2V2"},
{title:"Konsep Inti", color:"b4", icon:"fa-lightbulb", content:"Titik ekuivalen dicapai saat mol asam = mol basa."},
{title:"Contoh & Fakta", color:"b1", icon:"fa-atom", content:"Menentukan kadar cuka, uji kualitas air, laboratorium kimia."}
],
soal:[
{q:"Volume 25 mL HCl 0,1 M dititrasi NaOH 0,1 M. Volume NaOH saat ekuivalen?", o:["10","25","50","100"], a:1, p:"M1V1=M2V2 → 0,1×25=0,1×V → V=25 mL."},
{q:"Pada titik ekuivalen asam kuat-basa kuat, pH = ...", o:["<7","=7",">7","Tidak tentu"], a:1, p:"Asam kuat + basa kuat → netral"}
]
},
"koloid": {
judul:"KOLOID",
sub:[
{title:"Pengertian", color:"b1", icon:"fa-circle-info", content:"Campuran partikel halus antara larutan dan suspensi."},
{title:"Jenis", color:"b2", icon:"fa-list", content:"Sol, emulsi, aerosol."},
{title:"Rumus Penting", color:"b3", icon:"fa-flask", content:"Tidak spesifik, lebih ke efek fisika."},
{title:"Konsep Inti", color:"b4", icon:"fa-lightbulb", content:"Efek Tyndall & gerak Brown"},
{title:"Contoh & Fakta", color:"b1", icon:"fa-atom", content:"Susu, kabut, gelatin, tinta koloid."}
],
soal:[
{q:"Efek Tyndall adalah...", o:["Reaksi kimia","Hamburan cahaya","Perubahan warna","Pengendapan"], a:1, p:"Cahaya dihamburkan oleh partikel koloid."},
{q:"Gerak Brown terjadi karena...", o:["Gravitasi","Tumbukan molekul","Suhu rendah","Tekanan"], a:1, p:"Partikel bergerak akibat tumbukan molekul medium."}
]
}
};

function openMateri(topik){
let data=materiDB[topik];
document.getElementById("dashboard").classList.add("hidden");
document.getElementById("materi").classList.remove("hidden");
document.getElementById("judul").innerText=data.judul;

let html="";
data.sub.forEach(s=>{
html+=`<div class="box ${s.color}" onclick="toggleBox(this)"><b>${s.title} <i class="fa-solid ${s.icon}"></i></b>
<div class="box-content">${s.content}</div></div>`;
});

document.getElementById("materiContent").innerHTML=html;
soalData=data.soal;
loadQuiz();
initPHGraph();
}

function toggleBox(el){
let content=el.querySelector(".box-content");
content.style.display=(content.style.display==="block")?"none":"block";
}

function loadQuiz(){
let form=document.getElementById("quizForm");
form.innerHTML="";
document.getElementById("pembahasan").innerHTML="";
soalData.forEach((s,i)=>{
let html=`<p>${i+1}. ${s.q}</p>`;
s.o.forEach((opt,j)=>{
html+=`<label class="option"><input type="radio" name="q${i}" value="${j}"> ${opt}</label>`;
});
form.innerHTML+=html;
});
}

function cekJawaban(){
let hasil="";
soalData.forEach((s,i)=>{
let j=document.querySelector(`input[name=q${i}]:checked`);
let benar=s.o[s.a];
if(j && parseInt(j.value)===s.a){hasil+=`<p class="correct">${i+1}. Benar ✔<br>${s.p}</p>`;}
else{hasil+=`<p class="wrong">${i+1}. Salah ✖<br>Jawaban: ${benar}<br>${s.p}</p>`;}
});
document.getElementById("pembahasan").innerHTML=hasil;
}

function back(){
document.getElementById("dashboard").classList.remove("hidden");
document.getElementById("materi").classList.add("hidden");
}

function initPHGraph(){
let graph=document.getElementById("phGraph");
graph.innerHTML="";
for(let i=0;i<15;i++){
let bar=document.createElement("div");
bar.classList.add("ph-bar");
bar.style.left=(i*7)+"%";
bar.style.height="50%";
graph.appendChild(bar);
}
updatePH();
}

function updatePH(){
let ph=document.getElementById("phSlider").value;
document.getElementById("phValue").innerText=ph;
let bars=document.querySelectorAll(".ph-bar");
bars.forEach((b,i)=>{
b.style.height=(ph/14*100)+"%";
});
}
</script>
</body>
</html>
