<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Smart Survey Playground Designer</title>

<script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<style>
body{
font-family:Arial;
background:#0f172a;
color:white;
margin:0;
text-align:center;
}
section{
margin:20px;
padding:20px;
background:#1e293b;
border-radius:12px;
}
button{
padding:10px 20px;
border:none;
border-radius:8px;
background:#22c55e;
color:white;
cursor:pointer;
margin:10px;
}
canvas{background:white;border-radius:10px;}
</style>
</head>

<body>

<h1>Smart Survey Playground Designer</h1>

<!-- ================= UPLOAD ================= -->

<section>
<h2>Upload Grid Leveling Excel</h2>
<input type="file" id="excelFile">
<button onclick="readExcel()">Load Data</button>
<p id="status"></p>
</section>

<!-- ================= TRAVERSE ================= -->

<section>
<h2>Traverse Calculator</h2>

<label>Start Easting:</label>
<input id="startE" value="473704.69"><br>

<label>Start Northing:</label>
<input id="startN" value="2897384.82"><br>

<label>Azimuth AB (deg):</label>
<input id="azimuth" value="28.22"><br>

<button onclick="solveTraverse()">Solve Traverse</button>

<p id="traverseResult"></p>

</section>

<!-- ================= 2D DRAW ================= -->

<section>
<h2>2D Site Plan</h2>
<canvas id="plan2D" width="500" height="400"></canvas>
</section>

<!-- ================= 3D MODEL ================= -->

<section>
<h2>3D Terrain Model</h2>
<div id="threeContainer"></div>
</section>

<script>

let gridData=[];

/* ================= READ EXCEL ================= */

function readExcel(){
const file=document.getElementById('excelFile').files[0];
const reader=new FileReader();

reader.onload=function(e){
const data=new Uint8Array(e.target.result);
const workbook=XLSX.read(data,{type:'array'});
const sheet=workbook.Sheets[workbook.SheetNames[0]];
gridData=XLSX.utils.sheet_to_json(sheet);

document.getElementById("status").innerText=
"Data Loaded Successfully: "+gridData.length+" points";

draw2D();
create3D();
};

reader.readAsArrayBuffer(file);
}

/* ================= TRAVERSE ================= */

function solveTraverse(){

let E=parseFloat(startE.value);
let N=parseFloat(startN.value);
let az=parseFloat(azimuth.value)*Math.PI/180;

let L=50; // demo side length

let points=[];

for(let i=0;i<4;i++){
E+=L*Math.sin(az);
N+=L*Math.cos(az);

points.push({E,N});

az+=Math.PI/2;
}

document.getElementById("traverseResult").innerText=
"Traverse Solved (Demo Square Created)";

draw2D(points);
}

/* ================= 2D DRAW ================= */

function draw2D(points=[]){

const canvas=document.getElementById("plan2D");
const ctx=canvas.getContext("2d");

ctx.clearRect(0,0,500,400);

ctx.fillStyle="green";
ctx.fillRect(150,120,200,120); // football field

ctx.fillStyle="blue";
ctx.fillRect(370,150,60,60); // pool

ctx.fillStyle="orange";
ctx.fillRect(70,150,60,30); // tennis

ctx.fillStyle="black";
ctx.fillText("Football",200,110);
ctx.fillText("Pool",370,140);
ctx.fillText("Tennis",70,140);
}

/* ================= 3D MODEL ================= */

function create3D(){

const container=document.getElementById("threeContainer");
container.innerHTML="";

const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(75,600/400,0.1,1000);

const renderer=new THREE.WebGLRenderer();
renderer.setSize(600,400);
container.appendChild(renderer.domElement);

const geometry=new THREE.PlaneGeometry(10,10,20,20);

const material=new THREE.MeshBasicMaterial({
wireframe:true
});

const plane=new THREE.Mesh(geometry,material);
scene.add(plane);

plane.rotation.x=-Math.PI/2;

camera.position.z=8;
camera.position.y=6;
camera.lookAt(0,0,0);

function animate(){
requestAnimationFrame(animate);
plane.rotation.z+=0.002;
renderer.render(scene,camera);
}

animate();
}

</script>
</body>
</html>
