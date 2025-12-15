<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>نظام تحليل نتائج الطلاب</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
:root{
  --primary:#0b3c5d;
  --secondary:#1d6fa5;
  --bg:#f4f7fb;
  --good:#2e7d32;
  --mid:#f9a825;
  --bad:#c62828;
}
body{
  margin:0;
  font-family:"Segoe UI",Tahoma;
  background:var(--bg);
}
header{
  background:linear-gradient(to left,var(--primary),var(--secondary));
  color:#fff;
  padding:20px;
  text-align:center;
}
nav{
  display:flex;
  background:#fff;
  border-bottom:3px solid var(--primary);
}
nav button{
  flex:1;
  border:none;
  padding:12px;
  background:none;
  font-weight:bold;
  cursor:pointer;
  color:var(--primary);
}
nav button:hover{background:#e3eef7}
section{display:none;padding:15px}
section.active{display:block}
.card{
  background:#fff;
  border-radius:10px;
  padding:15px;
  margin-bottom:15px;
  box-shadow:0 2px 6px rgba(0,0,0,.08);
}
h3{
  margin-top:0;
  color:var(--primary);
  border-right:4px solid var(--primary);
  padding-right:8px;
}
textarea,input{
  width:100%;
  padding:7px;
  margin:4px 0;
}
table{
  width:100%;
  border-collapse:collapse;
  font-size:14px;
}
th,td{
  border:1px solid #ccc;
  padding:6px;
  text-align:center;
}
th{background:#e8f1f8}
.badge{
  color:#fff;
  padding:4px 7px;
  border-radius:4px;
  font-size:12px;
}
.good{background:var(--good)}
.mid{background:var(--mid)}
.bad{background:var(--bad)}
button.main{
  background:var(--primary);
  color:#fff;
  border:none;
  padding:10px 15px;
  border-radius:6px;
  cursor:pointer;
}
footer{
  text-align:center;
  font-size:12px;
  padding:10px;
  color:#555;
}
canvas{
  max-width:100%;
}
</style>
</head>

<body>

<header>
  <h1>نظام تحليل نتائج الطلاب</h1>
  <p>تحليل إحصائي وتربوي مدعوم برسوم بيانية متقدمة</p>
</header>

<nav>
  <button onclick="show('input')">إدخال البيانات</button>
  <button onclick="show('analysis')">التحليل</button>
  <button onclick="show('charts')">الرسوم البيانية</button>
</nav>


tion render(){
<!-- إدخال -->
<section id="input" class="active">
  <div class="card">
    <h3>إدخال أسماء الطلاب</h3>
    <textarea id="names" rows="4" placeholder="كل اسم في سطر"></textarea>
    <button class="main" onclick="addStudents()">إضافة</button>
  </div>

  <div class="card">
    <h3>درجات الطلاب</h3>
    <table>
      <thead>
        <tr>
          <th>الطالب</th>
          <th>المهام (40)</th>
          <th>الحضور (20)</th>
          <th>المستمر (60)</th>
          <th>النهائي (40)</th>
          <th>المجموع</th>
          <th>المستوى</th>
        </tr>
      </thead>
      <tbody id="table"></tbody>
    </table>
  </div>
</section>

<!-- التحليل -->
<section id="analysis">
  <div class="card">
    <h3>التحليل الإحصائي</h3>
    <p>المتوسط: <b id="avg"></b></p>
    <p>الانحراف المعياري: <b id="std"></b></p>
    <p>أعلى درجة: <b id="max"></b></p>
    <p>أقل درجة: <b id="min"></b></p>
  </div>
</section>

<!-- الرسوم -->
<section id="charts">
  <div class="card">
    <h3>تحليل بصري لمكونات الدرجة</h3>
    <canvas id="barChart"></canvas>
  </div>

  <div class="card">
    <h3>توزيع مستويات الأداء</h3>
    <canvas id="pieChart"></canvas>
  </div>

  <div class="card">
    <h3>تفسير تربوي للرسم البياني</h3>
    <ul>
      <li>يعكس الرسم الأول مستوى <b>التطبيق والتحليل</b> من خلال المهام الأدائية.</li>
      <li>يعكس الحضور مهارات <b>التذكر والفهم</b>.</li>
      <li>يوضح الرسم الدائري نسبة الطلاب المتفوقين والمتعثرين لدعم القرار.</li>
    </ul>
  </div>
</section>

<footer>
  مشروع تربوي تحليلي – قابل للتطبيق في مدارس المملكة العربية السعودية
</footer>

<script>
let students=[];
let barChart,pieChart;

function show(id){
  document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
  document.getElementById(id).classList.add("active");
}

function addStudents(){
  names.value.split("\n").forEach(n=>{
    if(n.trim()) students.push({name:n.trim(),tasks:0,att:0,final:0});
  });
  render();
}

function level(t){
  if(t>=90) return ["متفوق","good"];
  if(t>=75) return ["جيد جدًا","good"];
  if(t>=60) return ["جيد","mid"];
  return ["متعثر","bad"];
}

fun
c
  table.innerHTML="";
  students.forEach((s,i)=>{
    const cont=s.tasks+s.att;
    const total=cont+s.final;
    const [txt,cls]=level(total);

    table.innerHTML+=`
    <tr>
      <td>${s.name}</td>
      <td><input type="number" max="40" onchange="students[${i}].tasks=+this.value;render()"></td>
      <td><input type="number" max="20" onchange="students[${i}].att=+this.value;render()"></td>
      <td>${cont}</td>
      <td><input type="number" max="40" onchange="students[${i}].final=+this.value;render()"></td>
      <td>${total}</td>
      <td><span class="badge ${cls}">${txt}</span></td>
    </tr>`;
  });
  analyze();
  drawCharts();
}

function analyze(){
  if(!students.length) return;
  const totals=students.map(s=>s.tasks+s.att+s.final);
  const avgV=totals.reduce((a,b)=>a+b,0)/totals.length;
  const stdV=Math.sqrt(totals.reduce((a,b)=>a+Math.pow(b-avgV,2),0)/totals.length);

  avg.textContent=avgV.toFixed(2);
  std.textContent=stdV.toFixed(2);
  max.textContent=Math.max(...totals);
  min.textContent=Math.min(...totals);
}

function drawCharts(){
  const labels=students.map(s=>s.name);
  const tasks=students.map(s=>s.tasks);
  const att=students.map(s=>s.att);
  const fin=students.map(s=>s.final);

  if(barChart) barChart.destroy();
  barChart=new Chart(barChartCanvas,{
    type:'bar',
    data:{
      labels,
      datasets:[
        {label:'المهام الأدائية',data:tasks,backgroundColor:'#1976d2'},
        {label:'الحضور',data:att,backgroundColor:'#4caf50'},
        {label:'الاختبار النهائي',data:fin,backgroundColor:'#f57c00'}
      ]
    },
    options:{responsive:true}
  });

  const levels={متفوق:0,"جيد جدًا":0,جيد:0,متعثر:0};
  students.forEach(s=>levels[level(s.tasks+s.att+s.final)[0]]++);

  if(pieChart) pieChart.destroy();
  pieChart=new Chart(pieChartCanvas,{
    type:'pie',
    data:{
      labels:Object.keys(levels),
      datasets:[{data:Object.values(levels),
        backgroundColor:['#2e7d32','#4caf50','#f9a825','#c62828']}]
    }
  });
}

const barChartCanvas=document.getElementById("barChart").getContext("2d");
const pieChartCanvas=document.getElementById("pieChart").getContext("2d");
</script>

</body>
</html>
