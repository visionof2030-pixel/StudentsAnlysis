<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>تقرير تحليل نتائج اختبار مادة الرياضيات</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>

<style>
@page{size:A4;margin:0}
body{
    font-family:'Segoe UI', Tahoma, Arial, sans-serif;
    background:#f5f7fa;
    margin:0;
    padding:12px;
    -webkit-text-size-adjust:100%;
    touch-action:manipulation
}
.actions{
    text-align:center;
    margin-bottom:12px;
    background:#fff;
    padding:15px;
    border-radius:10px;
    box-shadow:0 4px 12px rgba(0,0,0,0.1)
}
button, .btn{
    background:#25d366;
    color:#fff;
    border:none;
    padding:12px 28px;
    font-size:16px;
    border-radius:8px;
    cursor:pointer;
    font-weight:bold;
    margin:5px;
    transition:all 0.3s ease;
    display:inline-flex;
    align-items:center;
    justify-content:center;
    gap:8px
}
button:hover, .btn:hover{
    background:#1da851;
    transform:translateY(-2px);
    box-shadow:0 4px 12px rgba(37,211,102,0.3)
}
.btn-secondary{
    background:#1a5c9e
}
.btn-secondary:hover{
    background:#154a80;
    box-shadow:0 4px 12px rgba(26,92,158,0.3)
}
.btn-excel{
    background:#4caf50
}
.btn-excel:hover{
    background:#388e3c;
    box-shadow:0 4px 12px rgba(76,175,80,0.3)
}
.btn-danger{
    background:#f44336
}
.btn-danger:hover{
    background:#d32f2f;
    box-shadow:0 4px 12px rgba(244,67,54,0.3)
}
.input-group{
    margin:15px 0;
    display:flex;
    flex-wrap:wrap;
    gap:10px;
    justify-content:center
}
input[type="text"], select{
    padding:12px 15px;
    border:2px solid #ddd;
    border-radius:8px;
    font-size:15px;
    min-width:250px;
    transition:all 0.3s ease
}
input[type="text"]:focus, select:focus{
    outline:none;
    border-color:#25d366;
    box-shadow:0 0 0 3px rgba(37,211,102,0.1)
}
.file-upload{
    position:relative;
    display:inline-block
}
.file-upload input[type="file"]{
    position:absolute;
    left:0;
    top:0;
    opacity:0;
    width:100%;
    height:100%;
    cursor:pointer
}
.file-label{
    display:inline-flex;
    align-items:center;
    gap:8px;
    padding:12px 28px;
    background:#2196f3;
    color:white;
    border-radius:8px;
    cursor:pointer;
    font-weight:bold;
    font-size:16px;
    transition:all 0.3s ease
}
.file-label:hover{
    background:#1976d2;
    transform:translateY(-2px)
}
.alert{
    display:none;
    padding:12px;
    margin:10px auto;
    border-radius:8px;
    text-align:center;
    max-width:500px;
    font-weight:bold
}
.alert.success{
    background:#d4edda;
    color:#155724;
    border:1px solid #c3e6cb
}
.alert.error{
    background:#f8d7da;
    color:#721c24;
    border:1px solid #f5c6cb
}
.alert.warning{
    background:#fff3cd;
    color:#856404;
    border:1px solid #ffeaa7
}
.loading{
    display:none;
    position:fixed;
    top:0;
    left:0;
    right:0;
    bottom:0;
    background:rgba(255,255,255,0.9);
    z-index:1000;
    flex-direction:column;
    justify-content:center;
    align-items:center
}
.spinner{
    width:50px;
    height:50px;
    border:4px solid #f3f3f3;
    border-top:4px solid #25d366;
    border-radius:50%;
    animation:spin 1s linear infinite
}
@keyframes spin{
    0%{transform:rotate(0deg)}
    100%{transform:rotate(360deg)}
}

/* تصميم التقرير الرئيسي */
.page{
    background:#fff;
    width:210mm;
    min-height:297mm;
    margin:auto;
    padding:14mm;
    box-sizing:border-box;
    box-shadow:0 8px 30px rgba(0,0,0,0.15)
}
.header{
    display:flex;
    justify-content:space-between;
    font-size:13px;
    margin-bottom:10px;
    padding-bottom:15px;
    border-bottom:2px solid #1a5c9e
}
.title{
    text-align:center;
    font-size:22px;
    font-weight:bold;
    margin:14px 0;
    padding:12px;
    background:#1a5c9e;
    color:#fff;
    border-radius:8px;
    position:relative;
    overflow:hidden
}
.title::before{
    content:"";
    position:absolute;
    top:0;
    left:0;
    right:0;
    height:3px;
    background:linear-gradient(90deg,#25d366,#4caf50,#2196f3)
}
.info{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:10px;
    margin-bottom:12px
}
.box{
    border:1px solid #ddd;
    border-radius:8px;
    padding:12px;
    text-align:center;
    font-size:15px;
    background:#f8fafc;
    transition:all 0.3s ease
}
.box:hover{
    transform:translateY(-3px);
    box-shadow:0 4px 12px rgba(0,0,0,0.1);
    border-color:#1a5c9e
}
.box span{
    display:block;
    font-weight:bold;
    margin-top:4px;
    font-size:16px;
    color:#1a5c9e
}
.summary{
    border:1px solid #ddd;
    border-radius:8px;
    padding:15px;
    font-size:15px;
    margin-bottom:14px;
    background:#f8fafc;
    line-height:1.8
}
.summary strong{
    color:#1a5c9e;
    display:inline-block;
    margin-left:5px
}
.section-title{
    font-size:18px;
    font-weight:bold;
    margin:14px 0 8px;
    color:#1a5c9e;
    padding-right:10px;
    border-right:4px solid #25d366
}
.table{
    border:1px solid #ddd;
    border-radius:8px;
    overflow:hidden;
    margin-bottom:12px
}
.row{
    display:grid;
    grid-template-columns:1fr 1fr 1fr;
    border-bottom:1px solid #eee;
    transition:background 0.3s
}
.row:hover{
    background:#f8fafc
}
.row.header-row{
    background:#1a5c9e;
    color:#fff;
    font-weight:bold
}
.cell{
    padding:10px;
    text-align:center;
    font-size:15px;
    display:flex;
    align-items:center;
    justify-content:center
}
.level{
    color:#fff;
    font-weight:bold;
    padding:5px 12px;
    border-radius:6px;
    font-size:14px;
    min-width:80px
}
.excellent{background:#4caf50}
.verygood{background:#009688}
.good{background:#2196f3}
.pass{background:#ff9800}
.weak{background:#f44336}
.stats{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:10px;
    margin-bottom:14px
}
.stat{
    border:1px solid #ddd;
    border-radius:8px;
    padding:12px;
    text-align:center;
    font-size:15px;
    background:white
}
.stat strong{
    display:block;
    font-size:20px;
    margin-top:4px;
    color:#1a5c9e
}
.students-section{
    margin:20px 0
}
.students-table{
    width:100%;
    border-collapse:collapse;
    margin:10px 0
}
.students-table th{
    background:#1a5c9e;
    color:white;
    padding:12px;
    font-weight:bold;
    border:none
}
.students-table td{
    padding:10px;
    border:1px solid #ddd;
    text-align:center
}
.students-table tr:nth-child(even){
    background:#f8fafc
}
.compare{
    border:1px solid #ddd;
    border-radius:8px;
    padding:15px;
    margin-bottom:14px;
    font-size:15px;
    background:#f8fafc
}
.compare p{
    margin:4px 0
}
.charts{
    display:grid;
    grid-template-columns:1fr 1fr 1fr;
    gap:12px;
    margin-bottom:14px
}
.chart-box{
    border:1px solid #ddd;
    border-radius:8px;
    padding:15px;
    height:220px;
    background:white
}
.chart-box canvas{
    width:100%!important;
    height:190px!important
}
.footer{
    display:flex;
    justify-content:space-between;
    font-size:14px;
    margin-top:14px;
    padding-top:15px;
    border-top:2px solid #eee
}
.footer div{
    text-align:center;
    flex:1
}
.footer strong{
    display:block;
    margin-bottom:5px;
    color:#1a5c9e
}
.signature-line{
    width:120px;
    height:1px;
    background:#333;
    margin:8px auto
}
.students-navigation{
    display:flex;
    justify-content:center;
    align-items:center;
    gap:15px;
    margin:15px 0;
    padding:10px;
    background:#f8fafc;
    border-radius:8px
}
.nav-btn{
    background:#1a5c9e;
    color:white;
    border:none;
    width:40px;
    height:40px;
    border-radius:8px;
    font-size:18px;
    cursor:pointer;
    display:flex;
    align-items:center;
    justify-content:center;
    transition:all 0.3s ease
}
.nav-btn:disabled{
    background:#ccc;
    cursor:not-allowed
}
.nav-btn:hover:not(:disabled){
    background:#154a80;
    transform:translateY(-2px)
}
.page-info{
    font-weight:bold;
    color:#1a5c9e
}
.search-filter{
    display:flex;
    gap:10px;
    margin:15px 0;
    flex-wrap:wrap
}
.search-box{
    flex:1;
    min-width:200px;
    position:relative
}
.search-box input{
    width:100%;
    padding:10px 35px 10px 15px;
    border:2px solid #ddd;
    border-radius:8px
}
.search-icon{
    position:absolute;
    left:12px;
    top:50%;
    transform:translateY(-50%);
    color:#666
}
.filter-select{
    min-width:150px
}

@media print{
    .actions,.alert,.loading,.search-filter,.students-navigation{
        display:none!important
    }
    body{
        padding:0;
        background:#fff
    }
    .page{
        box-shadow:none;
        margin:0;
        width:100%
    }
}
@media (max-width:1200px){
    .page{
        width:100%;
        margin:10px auto
    }
    .charts{
        grid-template-columns:repeat(2,1fr)
    }
}
@media (max-width:768px){
    .page{
        padding:10mm;
        width:100%
    }
    .header{
        flex-direction:column;
        text-align:center;
        gap:10px
    }
    .info,.stats{
        grid-template-columns:repeat(2,1fr)
    }
    .charts{
        grid-template-columns:1fr
    }
    .chart-box{
        height:200px
    }
    .footer{
        flex-direction:column;
        gap:15px;
        text-align:center
    }
    .btn,button{
        width:100%;
        margin:5px 0
    }
    .input-group{
        flex-direction:column
    }
    input[type="text"],select{
        width:100%
    }
}
@media (max-width:480px){
    .info,.stats{
        grid-template-columns:1fr
    }
    .row{
        grid-template-columns:1fr;
        text-align:center
    }
    .cell{
        justify-content:center
    }
    .title{
        font-size:18px
    }
}
</style>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>

<body>

<div class="actions">
    <h2 style="color:#1a5c9e;margin-bottom:15px">
        <i class="fas fa-chart-line"></i> نظام تحليل نتائج الاختبارات
    </h2>
    
    <div class="input-group">
        <input type="text" id="subjectName" placeholder="اسم المادة (مثل: الرياضيات)">
        <input type="text" id="className" placeholder="الصف (مثل: ثاني متوسط)">
        <input type="text" id="termName" placeholder="الفصل (مثل: الأول 1447هـ)">
    </div>
    
    <div class="input-group">
        <input type="number" id="totalScore" placeholder="الدرجة الكاملة للاختبار">
        <input type="text" id="examDate" placeholder="تاريخ الاختبار">
    </div>
    
    <div class="input-group">
        <div class="search-box">
            <i class="fas fa-search search-icon"></i>
            <input type="text" id="searchInput" placeholder="أدخل أسماء الطلاب ودرجاتهم">
        </div>
        <div class="file-upload">
            <input type="file" id="textFile" accept=".txt,.csv" onchange="handleTextFile(this.files[0])">
            <label class="file-label">
                <i class="fas fa-file-upload"></i> رفع ملف نصي
            </label>
        </div>
    </div>
    
    <div class="alert" id="alertMessage"></div>
    
    <div>
        <button onclick="generateReport()">
            <i class="fas fa-chart-bar"></i> إنشاء التقرير
        </button>
        <button class="btn-secondary" onclick="clearForm()">
            <i class="fas fa-redo"></i> مسح البيانات
        </button>
        <button class="btn-excel" onclick="exportToExcel()">
            <i class="fas fa-file-excel"></i> تصدير Excel
        </button>
        <button onclick="exportAndShare()">
            <i class="fab fa-whatsapp"></i> مشاركة PDF
        </button>
    </div>
</div>

<div class="loading" id="loading">
    <div class="spinner"></div>
    <p style="margin-top:15px;font-weight:bold;color:#1a5c9e">جاري إنشاء التقرير...</p>
</div>

<div class="page" id="report">

<div class="header">
<div>وزارة التعليم<br>الإدارة العامة للتعليم</div>
<div style="text-align:center;font-weight:bold">مدرسة النخبة النموذجية</div>
<div>الاختبار النهائي</div>
</div>

<div class="title">تقرير تحليل نتائج اختبار مادة <span id="reportSubject">الرياضيات</span></div>

<div class="info">
<div class="box">الصف<span id="reportClass">ثاني متوسط</span></div>
<div class="box">الفصل<span id="reportTerm">الأول 1447هـ</span></div>
<div class="box">درجة الاختبار<span id="reportTotalScore">40</span></div>
</div>

<div class="summary">
<strong>مستوى الأداء العام:</strong> <span id="overallLevel">جيد جدًا</span><br>
<strong>نسبة الطلاب دون مستوى الإتقان:</strong> <span id="belowMastery">25%</span><br>
<strong>قراءة تحليلية مختصرة:</strong>
<span id="analysisText">تعكس النتائج استقرارًا جيدًا في مستوى التحصيل مع وجود فئة محدودة تحتاج دعمًا إضافيًا.</span>
</div>

<div class="section-title">توزيع الطلاب حسب المستوى</div>

<div class="table">
<div class="row header-row">
    <div class="cell">عدد الطلاب</div>
    <div class="cell">النطاق</div>
    <div class="cell">المستوى</div>
</div>
<div class="row">
    <div class="cell" id="excellentCount">7</div>
    <div class="cell" id="excellentRange">36 - 40</div>
    <div class="cell"><span class="level excellent">ممتاز</span></div>
</div>
<div class="row">
    <div class="cell" id="verygoodCount">5</div>
    <div class="cell" id="verygoodRange">32 - 35.99</div>
    <div class="cell"><span class="level verygood">جيد جدًا</span></div>
</div>
<div class="row">
    <div class="cell" id="goodCount">3</div>
    <div class="cell" id="goodRange">28 - 31.99</div>
    <div class="cell"><span class="level good">جيد</span></div>
</div>
<div class="row">
    <div class="cell" id="passCount">2</div>
    <div class="cell" id="passRange">20 - 27.99</div>
    <div class="cell"><span class="level pass">مقبول</span></div>
</div>
<div class="row">
    <div class="cell" id="weakCount">3</div>
    <div class="cell" id="weakRange">0 - 19.99</div>
    <div class="cell"><span class="level weak">ضعيف</span></div>
</div>
</div>

<div class="stats">
<div class="stat">عدد الطلاب<strong id="totalStudents">20</strong></div>
<div class="stat">المتوسط<strong id="averageScore">30.7</strong></div>
<div class="stat">نسبة النجاح<strong id="successRate">85%</strong></div>
</div>

<div class="section-title">قائمة الطلاب والدرجات</div>

<div class="search-filter">
    <div class="search-box">
        <i class="fas fa-search search-icon"></i>
        <input type="text" id="studentSearch" placeholder="بحث عن طالب..." oninput="filterStudents()">
    </div>
    <select class="filter-select" id="gradeFilter" onchange="filterStudents()">
        <option value="">جميع التقديرات</option>
        <option value="ممتاز">ممتاز</option>
        <option value="جيد جدًا">جيد جدًا</option>
        <option value="جيد">جيد</option>
        <option value="مقبول">مقبول</option>
        <option value="ضعيف">ضعيف</option>
    </select>
</div>

<div class="students-section">
    <table class="students-table" id="studentsTable">
        <thead>
            <tr>
                <th>#</th>
                <th>اسم الطالب</th>
                <th>الدرجة</th>
                <th>النسبة</th>
                <th>التقدير</th>
            </tr>
        </thead>
        <tbody id="studentsTableBody">
            <!-- سيتم ملؤها جينياً -->
        </tbody>
    </table>
</div>

<div class="students-navigation">
    <button class="nav-btn" onclick="prevPage()" id="prevBtn">
        <i class="fas fa-chevron-right"></i>
    </button>
    <span class="page-info" id="pageInfo">الصفحة 1 من 1</span>
    <button class="nav-btn" onclick="nextPage()" id="nextBtn">
        <i class="fas fa-chevron-left"></i>
    </button>
</div>

<div class="section-title">مقارنة الصفوف (حتى 10 فصول – مرتبة حسب المتوسط)</div>

<div class="compare">
<p>يعرض المخطط متوسط الدرجات ونسبة دون مستوى الإتقان مرتبة من الأعلى إلى الأدنى.</p>
</div>

<div class="charts">
<div class="chart-box"><canvas id="avgChart"></canvas></div>
<div class="chart-box"><canvas id="lowChart"></canvas></div>
<div class="chart-box"><canvas id="pieChart"></canvas></div>
</div>

<div class="footer">
<div>
    <strong>مدير المدرسة</strong>
    <div class="signature-line"></div>
    د. عبدالله محمد الشمراني
</div>
<div>
    <strong>رئيس القسم</strong>
    <div class="signature-line"></div>
    أ. خالد إبراهيم العتيبي
</div>
<div>
    <strong>معلم المادة</strong>
    <div class="signature-line"></div>
    أ. سعيد علي الغامدي
</div>
</div>

</div>

<script>
// البيانات العالمية
let studentsData = [];
let filteredStudents = [];
let currentPage = 1;
const studentsPerPage = 10;
let avgChart, lowChart, pieChart;

// بيانات الصفوف للمقارنة
const classes = ["2/أ", "2/ب", "2/ج", "2/د", "2/هـ", "2/و", "2/ز", "2/ح", "2/ط", "2/ي"];
const averages = [33.5, 32.1, 31.8, 30.9, 30.2, 29.7, 29.3, 28.9, 28.4, 27.8];
const belowMastery = [12, 18, 20, 22, 25, 27, 30, 32, 35, 38];

// معالجة الملف النصي
function handleTextFile(file) {
    if (!file) return;
    
    showLoading(true);
    
    const reader = new FileReader();
    reader.onload = function(e) {
        try {
            const text = e.target.result;
            parseStudentData(text);
            showAlert('تم تحميل بيانات الطلاب بنجاح', 'success');
        } catch (error) {
            console.error('خطأ في قراءة الملف:', error);
            showAlert('خطأ في قراءة الملف. تأكد من صحة البيانات', 'error');
        } finally {
            showLoading(false);
        }
    };
    
    reader.onerror = function() {
        showAlert('خطأ في قراءة الملف', 'error');
        showLoading(false);
    };
    
    reader.readAsText(file, 'UTF-8');
}

// تحليل بيانات الطلاب من النص
function parseStudentData(text) {
    studentsData = [];
    const lines = text.split('\n');
    
    lines.forEach(line => {
        line = line.trim();
        if (!line) return;
        
        // محاولة التعرّف على تنسيقات مختلفة
        let name = '', score = 0;
        
        // تنسيق: اسم الطالب، الدرجة
        if (line.includes(',')) {
            const parts = line.split(',');
            name = parts[0].trim();
            score = parseFloat(parts[1].trim());
        }
        // تنسيق: اسم الطالب - الدرجة
        else if (line.includes('-')) {
            const parts = line.split('-');
            name = parts[0].trim();
            score = parseFloat(parts[1].trim());
        }
        // تنسيق: اسم الطالب الدرجة
        else {
            const parts = line.split(/\s+/);
            if (parts.length >= 2) {
                score = parseFloat(parts[parts.length - 1]);
                name = parts.slice(0, parts.length - 1).join(' ');
            }
        }
        
        if (name && !isNaN(score)) {
            const totalScore = parseInt(document.getElementById('totalScore').value) || 40;
            const percentage = ((score / totalScore) * 100).toFixed(1);
            const grade = calculateGrade(percentage);
            
            studentsData.push({
                name: name,
                score: score,
                percentage: percentage,
                grade: grade
            });
        }
    });
    
    if (studentsData.length > 0) {
        showAlert(`تم تحليل ${studentsData.length} طالباً`, 'success');
        displayStudents();
    } else {
        // استخدام بيانات تجريبية
        generateSampleData();
    }
}

// حساب التقدير بناءً على النسبة
function calculateGrade(percentage) {
    if (percentage >= 90) return "ممتاز";
    if (percentage >= 80) return "جيد جدًا";
    if (percentage >= 70) return "جيد";
    if (percentage >= 60) return "مقبول";
    return "ضعيف";
}

// توليد بيانات تجريبية
function generateSampleData() {
    const sampleNames = [
        "أحمد محمد العتيبي", "محمد عبدالله الشمراني", "سعيد علي الغامدي",
        "خالد إبراهيم الرشيد", "عبدالرحمن سعد الحربي", "فهد ناصر المطيري",
        "تركي سليمان القحطاني", "يوسف حمد السليمان", "عمر صالح العصيمي",
        "بدر عبدالعزيز الشهري", "سلطان راشد الدوسري", "نواف مطلق العجمي",
        "وليد فيصل الزهراني", "ماجد حامد القصير", "هاني عبدالمحسن البلوي",
        "ناصر علي القحطاني", "راشد سعد المطيري", "سلمان عبدالله الشهري",
        "فيصل محمد العصيمي", "طارق خالد الرشيد"
    ];
    
    const totalScore = parseInt(document.getElementById('totalScore').value) || 40;
    
    studentsData = sampleNames.map((name, index) => {
        // توزيع عشوائي للدرجات مع تنوع
        let score;
        if (index < 7) score = Math.floor(Math.random() * 5) + 36; // ممتاز
        else if (index < 12) score = Math.floor(Math.random() * 4) + 32; // جيد جدًا
        else if (index < 15) score = Math.floor(Math.random() * 4) + 28; // جيد
        else if (index < 17) score = Math.floor(Math.random() * 8) + 20; // مقبول
        else score = Math.floor(Math.random() * 20); // ضعيف
        
        const percentage = ((score / totalScore) * 100).toFixed(1);
        const grade = calculateGrade(percentage);
        
        return {
            name: name,
            score: score,
            percentage: percentage,
            grade: grade
        };
    });
}

// إنشاء التقرير
function generateReport() {
    // الحصول على البيانات من الحقول
    const subject = document.getElementById('subjectName').value || 'الرياضيات';
    const className = document.getElementById('className').value || 'ثاني متوسط';
    const term = document.getElementById('termName').value || 'الأول 1447هـ';
    const totalScore = document.getElementById('totalScore').value || '40';
    const examDate = document.getElementById('examDate').value || '';
    
    // إذا لم تكن هناك بيانات طلاب، إنشاء بيانات تجريبية
    if (studentsData.length === 0) {
        generateSampleData();
    }
    
    // تحديث معلومات التقرير
    document.getElementById('reportSubject').textContent = subject;
    document.getElementById('reportClass').textContent = className;
    document.getElementById('reportTerm').textContent = term;
    document.getElementById('reportTotalScore').textContent = totalScore;
    
    // حساب الإحصائيات
    calculateStatistics();
    
    // عرض الطلاب
    displayStudents();
    
    // إنشاء المخططات
    createCharts();
    
    // إضافة نص تحليلي بناءً على النتائج
    generateAnalysisText();
    
    showAlert('تم إنشاء التقرير بنجاح', 'success');
    
    // التمرير إلى التقرير
    document.getElementById('report').scrollIntoView({ behavior: 'smooth' });
}

// حساب الإحصائيات
function calculateStatistics() {
    const totalScore = parseInt(document.getElementById('totalScore').value) || 40;
    
    let excellentCount = 0, verygoodCount = 0, goodCount = 0, passCount = 0, weakCount = 0;
    let total = 0, sum = 0, maxScore = 0, minScore = totalScore;
    let passedCount = 0;
    
    studentsData.forEach(student => {
        total++;
        sum += student.score;
        maxScore = Math.max(maxScore, student.score);
        minScore = Math.min(minScore, student.score);
        
        if (student.score >= (totalScore * 0.6)) passedCount++;
        
        switch(student.grade) {
            case 'ممتاز': excellentCount++; break;
            case 'جيد جدًا': verygoodCount++; break;
            case 'جيد': goodCount++; break;
            case 'مقبول': passCount++; break;
            case 'ضعيف': weakCount++; break;
        }
    });
    
    const average = (sum / total).toFixed(1);
    const successRate = ((passedCount / total) * 100).toFixed(1);
    const belowMasteryRate = ((weakCount + passCount) / total * 100).toFixed(1);
    
    // تحديث الإحصائيات
    document.getElementById('totalStudents').textContent = total;
    document.getElementById('averageScore').textContent = average;
    document.getElementById('successRate').textContent = successRate + '%';
    document.getElementById('belowMastery').textContent = belowMasteryRate + '%';
    
    // تحديث توزيع التقديرات
    document.getElementById('excellentCount').textContent = excellentCount;
    document.getElementById('verygoodCount').textContent = verygoodCount;
    document.getElementById('goodCount').textContent = goodCount;
    document.getElementById('passCount').textContent = passCount;
    document.getElementById('weakCount').textContent = weakCount;
    
    // تحديد مستوى الأداء العام
    let overallLevel = '';
    if (parseFloat(successRate) >= 90) overallLevel = 'ممتاز';
    else if (parseFloat(successRate) >= 80) overallLevel = 'جيد جدًا';
    else if (parseFloat(successRate) >= 70) overallLevel = 'جيد';
    else if (parseFloat(successRate) >= 60) overallLevel = 'مقبول';
    else overallLevel = 'ضعيف';
    
    document.getElementById('overallLevel').textContent = overallLevel;
}

// إنشاء نص تحليلي
function generateAnalysisText() {
    const totalStudents = studentsData.length;
    const successRate = parseFloat(document.getElementById('successRate').textContent);
    const averageScore = parseFloat(document.getElementById('averageScore').textContent);
    const totalScore = parseInt(document.getElementById('reportTotalScore').textContent);
    
    let analysis = '';
    
    if (successRate >= 90) {
        analysis = `أظهرت النتائج تميزاً واضحاً في مستوى التحصيل حيث بلغت نسبة النجاح ${successRate}%. يعكس هذا المستوى تفوق الطلاب وإتقانهم للمهارات المطلوبة.`;
    } else if (successRate >= 80) {
        analysis = `تشير النتائج إلى مستوى جيد جداً من التحصيل بنسبة نجاح ${successRate}% ومتوسط ${averageScore}/${totalScore}. هناك مجال للتحسين لتحقيق التميز الكامل.`;
    } else if (successRate >= 70) {
        analysis = `النسبة ${successRate}% تدل على مستوى جيد مع وجود فئة تحتاج إلى دعم إضافي. يوصى بتقديم تدريبات مكثفة للطلاب ذوي الدرجات المنخفضة.`;
    } else if (successRate >= 60) {
        analysis = `النسبة ${successRate}% تشير إلى حاجة لمراجعة استراتيجيات التدريس. يوصى بإعادة تقييم المنهج وتقديم برامج دعم مكثفة.`;
    } else {
        analysis = `النسبة ${successRate}% تعكس ضعفاً في التحصيل يحتاج إلى تدخل عاجل. يوصى بإعادة تدريس المهارات الأساسية وتقديم برامج علاجية.`;
    }
    
    document.getElementById('analysisText').textContent = analysis;
}

// عرض قائمة الطلاب
function displayStudents() {
    const searchTerm = document.getElementById('studentSearch').value.toLowerCase();
    const gradeFilter = document.getElementById('gradeFilter').value;
    
    // تصفية الطلاب
    filteredStudents = studentsData.filter(student => {
        const matchesSearch = student.name.toLowerCase().includes(searchTerm);
        const matchesGrade = !gradeFilter || student.grade === gradeFilter;
        return matchesSearch && matchesGrade;
    });
    
    // حساب عدد الصفحات
    const totalPages = Math.ceil(filteredStudents.length / studentsPerPage);
    
    // تحديث أزرار التصفح
    document.getElementById('prevBtn').disabled = currentPage <= 1;
    document.getElementById('nextBtn').disabled = currentPage >= totalPages;
    document.getElementById('pageInfo').textContent = `الصفحة ${currentPage} من ${totalPages}`;
    
    // حساب بداية ونهاية الصفحة الحالية
    const startIndex = (currentPage - 1) * studentsPerPage;
    const endIndex = Math.min(startIndex + studentsPerPage, filteredStudents.length);
    const pageStudents = filteredStudents.slice(startIndex, endIndex);
    
    // إنشاء جدول الطلاب
    const tableBody = document.getElementById('studentsTableBody');
    tableBody.innerHTML = '';
    
    if (pageStudents.length === 0) {
        tableBody.innerHTML = `
            <tr>
                <td colspan="5" style="text-align: center; padding: 20px; color: #666;">
                    <i class="fas fa-users" style="font-size: 30px; margin-bottom: 10px; display: block; opacity: 0.5;"></i>
                    لا توجد بيانات للعرض
                </td>
            </tr>
        `;
        return;
    }
    
    pageStudents.forEach((student, index) => {
        const rowNumber = startIndex + index + 1;
        const gradeClass = getGradeClass(student.grade);
        
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${rowNumber}</td>
            <td style="text-align: right; font-weight: bold;">${student.name}</td>
            <td><strong>${student.score}</strong></td>
            <td>${student.percentage}%</td>
            <td><span class="${gradeClass}" style="padding: 5px 10px; border-radius: 4px; color: white; font-weight: bold;">${student.grade}</span></td>
        `;
        tableBody.appendChild(row);
    });
}

// الحصول على كلاس CSS للتقدير
function getGradeClass(grade) {
    switch(grade) {
        case 'ممتاز': return 'excellent';
        case 'جيد جدًا': return 'verygood';
        case 'جيد': return 'good';
        case 'مقبول': return 'pass';
        case 'ضعيف': return 'weak';
        default: return '';
    }
}

// تصفية الطلاب
function filterStudents() {
    currentPage = 1;
    displayStudents();
}

// الصفحة التالية
function nextPage() {
    const totalPages = Math.ceil(filteredStudents.length / studentsPerPage);
    if (currentPage < totalPages) {
        currentPage++;
        displayStudents();
        document.getElementById('studentsTable').scrollIntoView({ behavior: 'smooth' });
    }
}

// الصفحة السابقة
function prevPage() {
    if (currentPage > 1) {
        currentPage--;
        displayStudents();
        document.getElementById('studentsTable').scrollIntoView({ behavior: 'smooth' });
    }
}

// إنشاء المخططات
function createCharts() {
    // تدمير المخططات القديمة إن وجدت
    if (avgChart) avgChart.destroy();
    if (lowChart) lowChart.destroy();
    if (pieChart) pieChart.destroy();
    
    // مخطط متوسطات الصفوف
    const avgCtx = document.getElementById('avgChart').getContext('2d');
    avgChart = new Chart(avgCtx, {
        type: 'bar',
        data: {
            labels: classes,
            datasets: [{
                data: averages,
                backgroundColor: '#1a5c9e',
                barThickness: 18
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false },
                tooltip: {
                    callbacks: {
                        label: (ctx) => `المتوسط: ${ctx.raw} درجة`
                    }
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 40,
                    ticks: { stepSize: 5, font: { size: 13 } }
                },
                x: { ticks: { font: { size: 13 } } }
            }
        }
    });
    
    // مخطط نسبة دون الإتقان
    const lowCtx = document.getElementById('lowChart').getContext('2d');
    lowChart = new Chart(lowCtx, {
        type: 'bar',
        data: {
            labels: classes,
            datasets: [{
                data: belowMastery,
                backgroundColor: '#f44336',
                barThickness: 18
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false },
                tooltip: {
                    callbacks: {
                        label: (ctx) => `دون الإتقان: ${ctx.raw}%`
                    }
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 100,
                    ticks: {
                        stepSize: 10,
                        font: { size: 13 },
                        callback: v => v + '%'
                    }
                },
                x: { ticks: { font: { size: 13 } } }
            }
        }
    });
    
    // مخطط دائري للتقديرات
    const excellentCount = parseInt(document.getElementById('excellentCount').textContent) || 0;
    const verygoodCount = parseInt(document.getElementById('verygoodCount').textContent) || 0;
    const goodCount = parseInt(document.getElementById('goodCount').textContent) || 0;
    const passCount = parseInt(document.getElementById('passCount').textContent) || 0;
    const weakCount = parseInt(document.getElementById('weakCount').textContent) || 0;
    const total = excellentCount + verygoodCount + goodCount + passCount + weakCount;
    
    const pieCtx = document.getElementById('pieChart').getContext('2d');
    pieChart = new Chart(pieCtx, {
        type: 'pie',
        data: {
            labels: ["ممتاز", "جيد جدًا", "جيد", "مقبول", "ضعيف"],
            datasets: [{
                data: [
                    (excellentCount / total * 100).toFixed(1),
                    (verygoodCount / total * 100).toFixed(1),
                    (goodCount / total * 100).toFixed(1),
                    (passCount / total * 100).toFixed(1),
                    (weakCount / total * 100).toFixed(1)
                ],
                backgroundColor: ["#4caf50", "#009688", "#2196f3", "#ff9800", "#f44336"]
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: { font: { size: 13 } }
                },
                tooltip: {
                    callbacks: {
                        label: (ctx) => `${ctx.label}: ${ctx.raw}%`
                    }
                }
            }
        }
    });
}

// تصدير إلى Excel
function exportToExcel() {
    if (studentsData.length === 0) {
        showAlert('لا توجد بيانات للتصدير', 'warning');
        return;
    }
    
    showLoading(true);
    
    try {
        // تحضير البيانات
        const excelData = [
            ['#', 'اسم الطالب', 'الدرجة', 'النسبة المئوية', 'التقدير']
        ];
        
        studentsData.forEach((student, index) => {
            excelData.push([
                index + 1,
                student.name,
                student.score,
                student.percentage + '%',
                student.grade
            ]);
        });
        
        // إضافة إحصائيات
        excelData.push([]);
        excelData.push(['الإحصائيات', '', '', '', '']);
        excelData.push(['عدد الطلاب:', document.getElementById('totalStudents').textContent]);
        excelData.push(['المتوسط:', document.getElementById('averageScore').textContent]);
        excelData.push(['نسبة النجاح:', document.getElementById('successRate').textContent]);
        excelData.push(['مستوى الأداء العام:', document.getElementById('overallLevel').textContent]);
        
        // إنشاء ورقة عمل
        const ws = XLSX.utils.aoa_to_sheet(excelData);
        
        // تنسيق الأعمدة
        const wscols = [
            {wch: 5},
            {wch: 30},
            {wch: 10},
            {wch: 15},
            {wch: 12}
        ];
        ws['!cols'] = wscols;
        
        // إنشاء مصنف
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, 'نتائج الطلاب');
        
        // حفظ الملف
        const subject = document.getElementById('subjectName').value || 'الرياضيات';
        const fileName = `نتائج_${subject}_${new Date().toISOString().slice(0,10)}.xlsx`;
        XLSX.writeFile(wb, fileName);
        
        showAlert('تم تصدير البيانات إلى Excel بنجاح!', 'success');
        
    } catch (error) {
        console.error('خطأ في تصدير Excel:', error);
        showAlert('حدث خطأ أثناء تصدير Excel', 'error');
    } finally {
        showLoading(false);
    }
}

// مشاركة عبر واتساب
async function exportAndShare() {
    showLoading(true);
    
    try {
        const element = document.getElementById("report");
        const canvas = await html2canvas(element, {
            scale: 3,
            backgroundColor: "#fff",
            useCORS: true,
            logging: false
        });
        
        const imgData = canvas.toDataURL("image/jpeg", 1.0);
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF("p", "mm", "a4");
        
        pdf.addImage(imgData, "JPEG", 0, 0, 210, 297);
        
        // إضافة معلومات إضافية
        const subject = document.getElementById('subjectName').value || 'الرياضيات';
        const fileName = `تقرير_نتائج_${subject}.pdf`;
        
        if (navigator.share && navigator.canShare) {
            const pdfBlob = pdf.output('blob');
            const pdfFile = new File([pdfBlob], fileName, { type: "application/pdf" });
            
            try {
                await navigator.share({
                    files: [pdfFile],
                    title: `تقرير نتائج ${subject}`,
                    text: `تقرير تحليل نتائج مادة ${subject}`
                });
                showAlert('تم مشاركة التقرير عبر واتساب بنجاح!', 'success');
            } catch (shareError) {
                // إذا فشلت المشاركة، حفظ الملف
                pdf.save(fileName);
                showAlert('تم حفظ التقرير كملف PDF!', 'success');
            }
        } else {
            pdf.save(fileName);
            showAlert('تم حفظ التقرير كملف PDF!', 'success');
        }
        
    } catch (error) {
        console.error('خطأ في إنشاء PDF:', error);
        showAlert('حدث خطأ أثناء إنشاء التقرير', 'error');
    } finally {
        showLoading(false);
    }
}

// مسح البيانات
function clearForm() {
    if (confirm('هل أنت متأكد من رغبتك في مسح جميع البيانات؟')) {
        document.getElementById('subjectName').value = '';
        document.getElementById('className').value = '';
        document.getElementById('termName').value = '';
        document.getElementById('totalScore').value = '';
        document.getElementById('examDate').value = '';
        document.getElementById('searchInput').value = '';
        document.getElementById('studentSearch').value = '';
        document.getElementById('gradeFilter').value = '';
        
        studentsData = [];
        filteredStudents = [];
        currentPage = 1;
        
        // إعادة تعيين التقرير إلى البيانات الافتراضية
        document.getElementById('reportSubject').textContent = 'الرياضيات';
        document.getElementById('reportClass').textContent = 'ثاني متوسط';
        document.getElementById('reportTerm').textContent = 'الأول 1447هـ';
        document.getElementById('reportTotalScore').textContent = '40';
        document.getElementById('totalStudents').textContent = '20';
        document.getElementById('averageScore').textContent = '30.7';
        document.getElementById('successRate').textContent = '85%';
        document.getElementById('belowMastery').textContent = '25%';
        document.getElementById('overallLevel').textContent = 'جيد جدًا';
        document.getElementById('analysisText').textContent = 'تعكس النتائج استقرارًا جيدًا في مستوى التحصيل مع وجود فئة محدودة تحتاج دعمًا إضافيًا.';
        
        document.getElementById('excellentCount').textContent = '7';
        document.getElementById('verygoodCount').textContent = '5';
        document.getElementById('goodCount').textContent = '3';
        document.getElementById('passCount').textContent = '2';
        document.getElementById('weakCount').textContent = '3';
        
        // إعادة إنشاء المخططات
        createCharts();
        
        // مسح جدول الطلاب
        document.getElementById('studentsTableBody').innerHTML = '';
        document.getElementById('pageInfo').textContent = 'الصفحة 1 من 1';
        document.getElementById('prevBtn').disabled = true;
        document.getElementById('nextBtn').disabled = true;
        
        showAlert('تم مسح جميع البيانات بنجاح', 'success');
    }
}

// إدارة التحميل
function showLoading(show) {
    document.getElementById('loading').style.display = show ? 'flex' : 'none';
}

// عرض رسائل التنبيه
function showAlert(message, type) {
    const alertDiv = document.getElementById('alertMessage');
    alertDiv.textContent = message;
    alertDiv.className = `alert ${type}`;
    alertDiv.style.display = 'block';
    
    setTimeout(() => {
        alertDiv.style.display = 'none';
    }, 4000);
}

// تهيئة الصفحة
window.onload = function() {
    // إنشاء المخططات الافتراضية
    createCharts();
    
    // تحسين الإدخال على الهواتف
    if ('ontouchstart' in window) {
        document.body.classList.add('touch-device');
        
        // زيادة حجم الأزرار على الهواتف
        const buttons = document.querySelectorAll('button, .btn');
        buttons.forEach(btn => {
            btn.style.minHeight = '44px';
        });
    }
};

// دعم إدخال بيانات الطلاب بشكل يدوي
document.getElementById('searchInput').addEventListener('keypress', function(e) {
    if (e.key === 'Enter') {
        const input = this.value.trim();
        if (input) {
            const lines = input.split('\n').filter(line => line.trim());
            const tempText = lines.join('\n');
            parseStudentData(tempText);
            this.value = '';
        }
    }
});
</script>
</body>
</html>