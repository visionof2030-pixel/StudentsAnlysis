<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>تقرير تحليل نتائج اختبار مادة الرياضيات</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>

<style>
@page{size:A4;margin:0}
body{font-family:Tahoma,Arial,sans-serif;background:#f5f7fa;margin:0;padding:12px}
.actions{text-align:center;margin-bottom:12px}
button,input[type=file],input[type=text]{
background:#25d366;color:#fff;border:none;padding:12px 20px;
font-size:15px;border-radius:8px;cursor:pointer;font-weight:bold;margin:4px
}
input[type=text]{width:300px}
.page{background:#fff;width:210mm;height:297mm;margin:auto;padding:14mm;box-sizing:border-box}
.header{display:flex;justify-content:space-between;font-size:13px;margin-bottom:10px}
.title{text-align:center;font-size:22px;font-weight:bold;margin:14px 0;padding:10px;background:#1a5c9e;color:#fff;border-radius:8px}
.info{display:grid;grid-template-columns:repeat(5,1fr);gap:8px;margin-bottom:12px}
.box{border:1px solid #ddd;border-radius:8px;padding:8px;text-align:center;font-size:14px}
.box span{display:block;font-weight:bold;margin-top:4px}
.summary{border:1px solid #ddd;border-radius:8px;padding:10px;font-size:14px;margin-bottom:12px;white-space:pre-line}
.section-title{font-size:17px;font-weight:bold;margin:12px 0 6px}
.stats{display:grid;grid-template-columns:repeat(5,1fr);gap:8px;margin-bottom:12px}
.stat{border:1px solid #ddd;border-radius:8px;padding:8px;text-align:center;font-size:14px}
.stat strong{display:block;font-size:16px;margin-top:4px}
.charts{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:12px}
.chart-box{border:1px solid #ddd;border-radius:8px;padding:8px;height:190px}
.chart-box canvas{width:100%!important;height:160px!important}
.footer{display:flex;justify-content:space-between;font-size:13px;margin-top:10px}
@media print{.actions{display:none}body{padding:0;background:#fff}}
</style>
</head>

<body>

<div class="actions">
<input type="text" id="apiKey" placeholder="أدخل مفتاح Gemini API">
<input type="file" accept="application/pdf" onchange="handlePDF(this.files[0])">
<button onclick="exportAndShare()">إرسال التقرير عبر واتساب (PDF)</button>
</div>

<div class="page" id="report">

<div class="header">
<div>وزارة التعليم<br>الإدارة العامة للتعليم</div>
<div style="text-align:center">مدرسة النخبة النموذجية</div>
<div>الاختبار النهائي</div>
</div>

<div class="title">تقرير تحليل نتائج الاختبار</div>

<div class="info">
<div class="box">المادة<span id="subject">-</span></div>
<div class="box">الصف<span id="grade">-</span></div>
<div class="box">الفصل<span id="term">-</span></div>
<div class="box">الدرجة الكاملة<span id="maxScore">-</span></div>
<div class="box">تاريخ الاختبار<span id="examDate">-</span></div>
</div>

<div class="summary" id="analysisText"></div>

<div class="section-title">الإحصائيات</div>

<div class="stats">
<div class="stat">عدد الطلاب<strong id="studentsCount">-</strong></div>
<div class="stat">المتوسط<strong id="averageScore">-</strong></div>
<div class="stat">أعلى درجة<strong id="maxStudentScore">-</strong></div>
<div class="stat">أدنى درجة<strong id="minStudentScore">-</strong></div>
<div class="stat">نسبة النجاح<strong id="successRate">-</strong></div>
</div>

<div class="section-title">الرسوم البيانية</div>

<div class="charts">
<div class="chart-box"><canvas id="avgChart"></canvas></div>
<div class="chart-box"><canvas id="lowChart"></canvas></div>
<div class="chart-box"><canvas id="pieChart"></canvas></div>
</div>

<div class="footer">
<div>مدير المدرسة</div>
<div>رئيس القسم</div>
<div>معلم المادة</div>
</div>

</div>

<script>
pdfjsLib.GlobalWorkerOptions.workerSrc="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

let avgChart,lowChart,pieChart;

function initCharts(gradesDist){
avgChart=new Chart(document.getElementById("avgChart"),{
type:"bar",
data:{labels:["المتوسط"],datasets:[{data:[gradesDist.average],backgroundColor:"#1a5c9e"}]},
options:{responsive:true,maintainAspectRatio:false,scales:{y:{beginAtZero:true}}}
});
lowChart=new Chart(document.getElementById("lowChart"),{
type:"bar",
data:{labels:["نسبة النجاح"],datasets:[{data:[gradesDist.success],backgroundColor:"#4caf50"}]},
options:{responsive:true,maintainAspectRatio:false,scales:{y:{beginAtZero:true,max:100}}}
});
pieChart=new Chart(document.getElementById("pieChart"),{
type:"pie",
data:{
labels:["ممتاز","جيد جدًا","جيد","مقبول","ضعيف"],
datasets:[{data:gradesDist.levels,backgroundColor:["#4caf50","#009688","#2196f3","#ff9800","#f44336"]}]
},
options:{responsive:true,maintainAspectRatio:false}
});
}

async function handlePDF(file){
const reader=new FileReader();
reader.onload=async()=>{
const pdf=await pdfjsLib.getDocument({data:reader.result}).promise;
let text="";
for(let i=1;i<=pdf.numPages;i++){
const page=await pdf.getPage(i);
const content=await page.getTextContent();
content.items.forEach(it=>text+=it.str+" ");
}
analyzeWithGemini(text);
};
reader.readAsArrayBuffer(file);
}

async function analyzeWithGemini(text){
const key=document.getElementById("apiKey").value;
const prompt=`أنت محلل نتائج طلاب محترف. قم بتحليل ملف النتائج هذا واستخرج المعلومات التالية:

1. معلومات عامة:
- اسم المادة
- الصف الدراسي
- الفصل الدراسي
- درجة الاختبار الكاملة
- تاريخ الاختبار

2. التقديرات (ممتاز، جيد جداً، جيد، مقبول، ضعيف)

3. إحصائيات:
- عدد الطلاب
- متوسط الدرجات
- أعلى درجة
- أدنى درجة
- نسبة النجاح

أرجع النتائج بصيغة JSON منظمة.

البيانات:
${text}`;
const res=await fetch(\`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-lite:generateContent?key=\${key}\`,{
method:"POST",
headers:{"Content-Type":"application/json"},
body:JSON.stringify({contents:[{parts:[{text:prompt}]}]})
});
const data=await res.json();
const json=JSON.parse(data.candidates[0].content.parts[0].text);
renderData(json);
}

function renderData(d){
document.getElementById("subject").innerText=d.subject;
document.getElementById("grade").innerText=d.grade;
document.getElementById("term").innerText=d.term;
document.getElementById("maxScore").innerText=d.max_score;
document.getElementById("examDate").innerText=d.exam_date;
document.getElementById("analysisText").innerText="تحليل عام:\n"+d.analysis;
document.getElementById("studentsCount").innerText=d.stats.count;
document.getElementById("averageScore").innerText=d.stats.average;
document.getElementById("maxStudentScore").innerText=d.stats.max;
document.getElementById("minStudentScore").innerText=d.stats.min;
document.getElementById("successRate").innerText=d.stats.success+"%";
initCharts({
average:d.stats.average,
success:d.stats.success,
levels:d.levels
});
}

async function exportAndShare(){
const canvas=await html2canvas(document.getElementById("report"),{scale:3,backgroundColor:"#fff"});
const img=canvas.toDataURL("image/jpeg",1.0);
const {jsPDF}=window.jspdf;
const pdf=new jsPDF("p","mm","a4");
pdf.addImage(img,"JPEG",0,0,210,297);
const blob=pdf.output("blob");
const file=new File([blob],"تقرير_تحليل_النتائج.pdf",{type:"application/pdf"});
if(navigator.canShare&&navigator.canShare({files:[file]})){
await navigator.share({files:[file],title:"تقرير تحليل النتائج"});
}else{
pdf.save("تقرير_تحليل_النتائج.pdf");
}
}
</script>

</body>
</html>