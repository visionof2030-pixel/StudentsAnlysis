<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>تقرير تحليل نتائج اختبار مادة الرياضيات</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- المكتبات الخارجية -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>

<style>
@page {
    size: A4;
    margin: 0;
}

* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: 'Tahoma', 'Arial', sans-serif;
    background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
    margin: 0;
    padding: 12px;
    min-height: 100vh;
}

.actions-container {
    text-align: center;
    margin-bottom: 20px;
    padding: 15px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.actions-title {
    font-size: 18px;
    font-weight: bold;
    color: #1a5c9e;
    margin-bottom: 15px;
    padding-bottom: 10px;
    border-bottom: 2px solid #25d366;
}

.input-group {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 12px;
    margin-bottom: 15px;
}

.input-field {
    flex: 1;
    min-width: 250px;
    max-width: 400px;
    position: relative;
}

.input-field input {
    width: 100%;
    padding: 12px 40px 12px 15px;
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    font-size: 14px;
    transition: all 0.3s ease;
    background: #f8f9fa;
}

.input-field input:focus {
    outline: none;
    border-color: #25d366;
    box-shadow: 0 0 0 3px rgba(37, 211, 102, 0.1);
    background: white;
}

.buttons-group {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
}

.btn {
    background: #25d366;
    color: white;
    border: none;
    padding: 12px 24px;
    font-size: 15px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    margin: 4px;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    min-width: 200px;
}

.btn:hover {
    background: #1da851;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(37, 211, 102, 0.3);
}

.btn:active {
    transform: translateY(0);
}

.btn-secondary {
    background: #1a5c9e;
}

.btn-secondary:hover {
    background: #154a80;
    box-shadow: 0 4px 12px rgba(26, 92, 158, 0.3);
}

.btn-danger {
    background: #f44336;
}

.btn-danger:hover {
    background: #d32f2f;
    box-shadow: 0 4px 12px rgba(244, 67, 54, 0.3);
}

.btn i {
    font-size: 16px;
}

.file-input-container {
    position: relative;
    display: inline-block;
}

.file-input-container input[type="file"] {
    position: absolute;
    left: 0;
    top: 0;
    opacity: 0;
    width: 100%;
    height: 100%;
    cursor: pointer;
}

.file-label {
    display: inline-block;
    padding: 12px 24px;
    background: #2196f3;
    color: white;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s ease;
}

.file-label:hover {
    background: #1976d2;
    transform: translateY(-2px);
}

.loading {
    display: none;
    text-align: center;
    padding: 20px;
}

.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #25d366;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto 10px;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.page {
    background: white;
    width: 210mm;
    min-height: 297mm;
    margin: 20px auto;
    padding: 20mm;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
    border-radius: 4px;
    position: relative;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
    margin-bottom: 25px;
    padding-bottom: 20px;
    border-bottom: 3px solid #1a5c9e;
    background: linear-gradient(to right, #f8fafc, #ffffff, #f8fafc);
    padding: 15px;
    border-radius: 10px;
}

.header div {
    flex: 1;
    text-align: center;
    padding: 0 10px;
}

.header div:first-child {
    text-align: right;
}

.header div:last-child {
    text-align: left;
}

.logo {
    font-size: 20px;
    font-weight: bold;
    color: #1a5c9e;
    padding: 10px 20px;
    border: 2px solid #1a5c9e;
    border-radius: 10px;
    background: linear-gradient(135deg, #f0f7ff 0%, #e3eeff 100%);
    box-shadow: 0 4px 8px rgba(26, 92, 158, 0.1);
}

.title {
    text-align: center;
    font-size: 26px;
    font-weight: bold;
    margin: 30px 0;
    padding: 25px;
    background: linear-gradient(135deg, #1a5c9e 0%, #2a6cb0 100%);
    color: white;
    border-radius: 15px;
    box-shadow: 0 6px 15px rgba(26, 92, 158, 0.25);
    position: relative;
    overflow: hidden;
}

.title::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 5px;
    background: linear-gradient(90deg, #25d366, #4caf50, #2196f3, #1a5c9e);
}

.info-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 15px;
    margin-bottom: 30px;
}

.info-box {
    border: 2px solid #e1e8f0;
    border-radius: 12px;
    padding: 18px;
    text-align: center;
    font-size: 14px;
    background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
}

.info-box:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    border-color: #1a5c9e;
}

.info-box::after {
    content: "";
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 4px;
    background: linear-gradient(90deg, #25d366, #1a5c9e);
}

.info-box span {
    display: block;
    font-weight: bold;
    margin-top: 10px;
    font-size: 18px;
    color: #1a5c9e;
}

.analysis-section {
    border: 2px solid #e1e8f0;
    border-radius: 15px;
    padding: 25px;
    font-size: 15px;
    margin-bottom: 30px;
    background: linear-gradient(135deg, #f8fafc 0%, #ffffff 100%);
    line-height: 1.8;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}

.analysis-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 15px;
    border-bottom: 2px solid #e0e0e0;
}

.section-title {
    font-size: 20px;
    font-weight: bold;
    color: #1a5c9e;
    margin: 25px 0 20px;
    padding-right: 15px;
    border-right: 5px solid #25d366;
    background: linear-gradient(to right, #f0f7ff, transparent);
    padding: 12px 20px;
    border-radius: 0 10px 10px 0;
}

.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 20px;
    margin-bottom: 30px;
}

.stat-box {
    border: 2px solid #e1e8f0;
    border-radius: 12px;
    padding: 25px;
    text-align: center;
    font-size: 15px;
    background: white;
    transition: all 0.3s ease;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.stat-box:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.12);
}

.stat-box strong {
    display: block;
    font-size: 32px;
    margin-top: 12px;
    color: #1a5c9e;
    font-weight: bold;
    text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
}

.stat-box.success {
    border-color: #4caf50;
    background: linear-gradient(135deg, #f1f8e9 0%, #e8f5e9 100%);
}

.stat-box.warning {
    border-color: #ff9800;
    background: linear-gradient(135deg, #fff3e0 0%, #fff8e1 100%);
}

.stat-box.info {
    border-color: #2196f3;
    background: linear-gradient(135deg, #e3f2fd 0%, #e1f5fe 100%);
}

.charts-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 25px;
    margin-bottom: 35px;
}

.chart-box {
    border: 2px solid #e1e8f0;
    border-radius: 15px;
    padding: 25px;
    background: white;
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.08);
    transition: all 0.3s ease;
    height: 320px;
    display: flex;
    flex-direction: column;
}

.chart-box:hover {
    transform: translateY(-7px);
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.15);
}

.chart-title {
    text-align: center;
    font-weight: bold;
    margin-bottom: 20px;
    color: #1a5c9e;
    font-size: 18px;
    padding-bottom: 10px;
    border-bottom: 2px solid #f0f0f0;
}

.chart-container {
    flex: 1;
    position: relative;
    width: 100%;
    height: 100%;
}

.chart-container canvas {
    width: 100% !important;
    height: 100% !important;
}

.footer {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    font-size: 15px;
    margin-top: 50px;
    padding-top: 25px;
    border-top: 3px solid #e0e0e0;
    background: linear-gradient(to right, #f8fafc, #ffffff, #f8fafc);
    padding: 25px;
    border-radius: 10px;
}

.footer div {
    text-align: center;
    flex: 1;
    padding: 0 20px;
}

.footer strong {
    display: block;
    margin-bottom: 8px;
    color: #1a5c9e;
    font-size: 16px;
    font-weight: bold;
}

.signature-line {
    width: 180px;
    height: 2px;
    background: #333;
    margin: 15px auto;
    position: relative;
}

.signature-line::after {
    content: "التوقيع";
    position: absolute;
    top: -25px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 12px;
    color: #666;
}

.alert {
    display: none;
    padding: 15px;
    margin: 15px auto;
    border-radius: 8px;
    text-align: center;
    max-width: 500px;
    font-weight: bold;
}

.alert.success {
    background: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
}

.alert.error {
    background: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
}

.alert.warning {
    background: #fff3cd;
    color: #856404;
    border: 1px solid #ffeaa7;
}

.watermark {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) rotate(-45deg);
    font-size: 90px;
    color: rgba(26, 92, 158, 0.03);
    font-weight: bold;
    pointer-events: none;
    z-index: 1;
    font-family: 'Arial', sans-serif;
}

.report-date {
    text-align: left;
    font-size: 13px;
    color: #666;
    margin-top: 10px;
    font-style: italic;
}

.chart-legend {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 15px;
    font-size: 12px;
}

.legend-item {
    display: flex;
    align-items: center;
    gap: 5px;
}

.legend-color {
    width: 12px;
    height: 12px;
    border-radius: 50%;
}

@media print {
    .actions-container, .alert, .loading {
        display: none !important;
    }
    
    body {
        padding: 0;
        background: white;
    }
    
    .page {
        box-shadow: none;
        margin: 0;
        padding: 15mm;
        width: 100%;
        height: auto;
    }
    
    .charts-container {
        page-break-inside: avoid;
    }
    
    .chart-box {
        page-break-inside: avoid;
    }
}

@media (max-width: 1200px) {
    .page {
        width: 100%;
        margin: 10px auto;
        padding: 15mm;
    }
    
    .charts-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 768px) {
    .header {
        flex-direction: column;
        gap: 15px;
    }
    
    .header div {
        text-align: center !important;
    }
    
    .charts-container {
        grid-template-columns: 1fr;
        gap: 20px;
    }
    
    .chart-box {
        height: 280px;
    }
    
    .footer {
        flex-direction: column;
        gap: 25px;
        text-align: center;
    }
    
    .input-field {
        min-width: 100%;
    }
    
    .btn {
        min-width: 100%;
    }
    
    .info-grid, .stats-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 480px) {
    .info-grid, .stats-grid {
        grid-template-columns: 1fr;
    }
    
    .chart-box {
        height: 250px;
        padding: 15px;
    }
    
    .title {
        font-size: 22px;
        padding: 20px;
    }
    
    .section-title {
        font-size: 18px;
    }
}
</style>

<!-- أيقونات Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>

<body>

<div class="actions-container">
    <div class="actions-title">
        <i class="fas fa-chart-line"></i> نظام تحليل نتائج الاختبارات
    </div>
    
    <div class="input-group">
        <div class="input-field">
            <i class="fas fa-key" style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); color: #666;"></i>
            <input type="text" id="apiKey" placeholder="أدخل مفتاح Gemini API للحصول على تحليل ذكي">
        </div>
        
        <div class="file-input-container">
            <input type="file" id="pdfFile" accept="application/pdf" onchange="handlePDF(this.files[0])">
            <label for="pdfFile" class="file-label">
                <i class="fas fa-file-pdf"></i> اختر ملف النتائج (PDF)
            </label>
        </div>
    </div>
    
    <div class="alert" id="alertMessage"></div>
    
    <div class="buttons-group">
        <button class="btn" onclick="exportAndShare()">
            <i class="fab fa-whatsapp"></i> إرسال عبر واتساب (PDF)
        </button>
        <button class="btn btn-secondary" onclick="printReport()">
            <i class="fas fa-print"></i> طباعة التقرير
        </button>
        <button class="btn btn-secondary" onclick="saveAsImage()">
            <i class="fas fa-image"></i> حفظ كصورة
        </button>
        <button class="btn btn-danger" onclick="resetForm()">
            <i class="fas fa-redo"></i> مسح البيانات
        </button>
    </div>
</div>

<div class="loading" id="loading">
    <div class="spinner"></div>
    <p>جاري تحليل البيانات وعمل التقرير...</p>
</div>

<div class="page" id="report">
    <div class="watermark">تقرير رسمي</div>
    
    <div class="header">
        <div>
            <strong>وزارة التعليم</strong><br>
            الإدارة العامة للتعليم<br>
            منطقة الرياض التعليمية
        </div>
        <div class="logo">
            <i class="fas fa-school"></i> مدرسة النخبة النموذجية
        </div>
        <div>
            <strong>الاختبار النهائي</strong><br>
            الفصل الدراسي الثاني<br>
            1446 هـ / 2024 م
        </div>
    </div>

    <div class="title">
        <i class="fas fa-chart-bar"></i> تقرير تحليل نتائج الاختبار
        <div class="report-date" id="currentDate"></div>
    </div>

    <div class="info-grid">
        <div class="info-box">
            <i class="fas fa-book" style="color: #1a5c9e; margin-bottom: 8px; font-size: 20px;"></i>
            المادة
            <span id="subject">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-graduation-cap" style="color: #1a5c9e; margin-bottom: 8px; font-size: 20px;"></i>
            الصف
            <span id="grade">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-calendar-alt" style="color: #1a5c9e; margin-bottom: 8px; font-size: 20px;"></i>
            الفصل
            <span id="term">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-star" style="color: #1a5c9e; margin-bottom: 8px; font-size: 20px;"></i>
            الدرجة الكاملة
            <span id="maxScore">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-clock" style="color: #1a5c9e; margin-bottom: 8px; font-size: 20px;"></i>
            تاريخ الاختبار
            <span id="examDate">-</span>
        </div>
    </div>

    <div class="section-title">
        <i class="fas fa-analysis"></i> التحليل العام للنتائج
    </div>
    
    <div class="analysis-section">
        <div class="analysis-header">
            <strong style="font-size: 16px;">ملخص التحليل الإحصائي</strong>
            <span class="badge" id="analysisStatus" style="background: #e0e0e0; padding: 6px 12px; border-radius: 20px; font-size: 13px;">
                <i class="fas fa-clock"></i> قيد الانتظار
            </span>
        </div>
        <div id="analysisText" style="text-align: justify; line-height: 1.9; font-size: 15px;">
            سيظهر هنا التحليل الذكي للنتائج بعد رفع ملف PDF وإدخال مفتاح API. سيتضمن التقرير تحليلًا شاملاً للأداء ومقارنات مع النتائج السابقة وتوصيات للتحسين.
        </div>
    </div>

    <div class="section-title">
        <i class="fas fa-chart-pie"></i> الإحصائيات الرئيسية
    </div>

    <div class="stats-grid">
        <div class="stat-box info">
            <i class="fas fa-users" style="font-size: 28px; color: #2196f3; margin-bottom: 12px;"></i>
            عدد الطلاب
            <strong id="studentsCount">-</strong>
        </div>
        <div class="stat-box success">
            <i class="fas fa-calculator" style="font-size: 28px; color: #4caf50; margin-bottom: 12px;"></i>
            المتوسط
            <strong id="averageScore">-</strong>
        </div>
        <div class="stat-box">
            <i class="fas fa-trophy" style="font-size: 28px; color: #ff9800; margin-bottom: 12px;"></i>
            أعلى درجة
            <strong id="maxStudentScore">-</strong>
        </div>
        <div class="stat-box warning">
            <i class="fas fa-exclamation-triangle" style="font-size: 28px; color: #ff9800; margin-bottom: 12px;"></i>
            أدنى درجة
            <strong id="minStudentScore">-</strong>
        </div>
        <div class="stat-box success">
            <i class="fas fa-percentage" style="font-size: 28px; color: #4caf50; margin-bottom: 12px;"></i>
            نسبة النجاح
            <strong id="successRate">-</strong>
        </div>
    </div>

    <div class="section-title">
        <i class="fas fa-chart-area"></i> الرسوم البيانية الإحصائية
    </div>

    <div class="charts-container">
        <div class="chart-box">
            <div class="chart-title">
                <i class="fas fa-chart-bar"></i> توزيع الدرجات
            </div>
            <div class="chart-container">
                <canvas id="distributionChart"></canvas>
            </div>
            <div class="chart-legend" id="distributionLegend"></div>
        </div>
        
        <div class="chart-box">
            <div class="chart-title">
                <i class="fas fa-chart-pie"></i> نسبة النجاح والرسوب
            </div>
            <div class="chart-container">
                <canvas id="successChart"></canvas>
            </div>
            <div class="chart-legend" id="successLegend"></div>
        </div>
        
        <div class="chart-box">
            <div class="chart-title">
                <i class="fas fa-chart-line"></i> توزيع التقديرات
            </div>
            <div class="chart-container">
                <canvas id="gradesChart"></canvas>
            </div>
            <div class="chart-legend" id="gradesLegend"></div>
        </div>
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
// تهيئة متغيرات المخططات
let distributionChart, successChart, gradesChart;

// تحديث التاريخ الحالي
function updateCurrentDate() {
    const now = new Date();
    const options = { 
        year: 'numeric', 
        month: 'long', 
        day: 'numeric',
        weekday: 'long'
    };
    const hijriDate = now.toLocaleDateString('ar-SA', options);
    document.getElementById('currentDate').textContent = `تاريخ التقرير: ${hijriDate}`;
}

// إنشاء مخططات محسنة
function initEnhancedCharts(data) {
    // تدمير المخططات القديمة إن وجدت
    if (distributionChart) distributionChart.destroy();
    if (successChart) successChart.destroy();
    if (gradesChart) gradesChart.destroy();
    
    const maxScore = parseInt(data.max_score) || 100;
    const averageScore = data.stats.average || 0;
    const successRate = data.stats.success || 0;
    const gradeLevels = data.levels || [0, 0, 0, 0, 0];
    const gradeLabels = ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'];
    const gradeColors = ['#4caf50', '#009688', '#2196f3', '#ff9800', '#f44336'];
    const gradeBorderColors = ['#388e3c', '#00796b', '#1976d2', '#f57c00', '#d32f2f'];
    
    // 1. مخطط توزيع الدرجات (شريطي أفقي)
    const distributionCtx = document.getElementById('distributionChart').getContext('2d');
    distributionChart = new Chart(distributionCtx, {
        type: 'bar',
        data: {
            labels: ['المتوسط', 'الأعلى', 'الأدنى'],
            datasets: [{
                label: 'الدرجات',
                data: [averageScore, data.stats.max || 0, data.stats.min || 0],
                backgroundColor: [
                    'rgba(37, 211, 102, 0.8)',
                    'rgba(255, 152, 0, 0.8)',
                    'rgba(244, 67, 54, 0.8)'
                ],
                borderColor: [
                    '#25d366',
                    '#ff9800',
                    '#f44336'
                ],
                borderWidth: 2,
                borderRadius: 8,
                barPercentage: 0.6
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            indexAxis: 'y',
            scales: {
                x: {
                    beginAtZero: true,
                    max: maxScore,
                    grid: {
                        color: 'rgba(0,0,0,0.05)',
                        drawBorder: false
                    },
                    ticks: {
                        font: {
                            size: 12,
                            family: 'Tahoma'
                        },
                        color: '#555'
                    },
                    title: {
                        display: true,
                        text: 'الدرجة',
                        font: {
                            size: 13,
                            weight: 'bold'
                        },
                        color: '#1a5c9e'
                    }
                },
                y: {
                    grid: {
                        display: false
                    },
                    ticks: {
                        font: {
                            size: 14,
                            weight: 'bold',
                            family: 'Tahoma'
                        },
                        color: '#333'
                    }
                }
            },
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    backgroundColor: 'rgba(0,0,0,0.7)',
                    titleFont: {
                        size: 13,
                        family: 'Tahoma'
                    },
                    bodyFont: {
                        size: 13,
                        family: 'Tahoma'
                    },
                    padding: 10,
                    cornerRadius: 8,
                    callbacks: {
                        label: function(context) {
                            return `${context.label}: ${context.raw} من ${maxScore}`;
                        }
                    }
                }
            }
        }
    });
    
    // تحديث وسيلة الإيضاح
    document.getElementById('distributionLegend').innerHTML = `
        <div class="legend-item">
            <div class="legend-color" style="background-color: #25d366;"></div>
            <span>المتوسط</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: #ff9800;"></div>
            <span>الأعلى</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: #f44336;"></div>
            <span>الأدنى</span>
        </div>
    `;
    
    // 2. مخطط النجاح والرسوب (دائري مجوف)
    const successCtx = document.getElementById('successChart').getContext('2d');
    successChart = new Chart(successCtx, {
        type: 'doughnut',
        data: {
            labels: ['نسبة النجاح', 'نسبة الرسوب'],
            datasets: [{
                data: [successRate, 100 - successRate],
                backgroundColor: [
                    'rgba(76, 175, 80, 0.9)',
                    'rgba(244, 67, 54, 0.9)'
                ],
                borderColor: [
                    '#4caf50',
                    '#f44336'
                ],
                borderWidth: 3,
                hoverOffset: 15,
                hoverBorderWidth: 4
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '70%',
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: {
                        font: {
                            size: 13,
                            family: 'Tahoma',
                            weight: 'bold'
                        },
                        padding: 20,
                        usePointStyle: true,
                        pointStyle: 'circle'
                    }
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return `${context.label}: ${context.raw.toFixed(1)}%`;
                        }
                    },
                    backgroundColor: 'rgba(0,0,0,0.7)',
                    titleFont: {
                        size: 13,
                        family: 'Tahoma'
                    },
                    bodyFont: {
                        size: 14,
                        family: 'Tahoma',
                        weight: 'bold'
                    }
                }
            },
            animation: {
                animateScale: true,
                animateRotate: true,
                duration: 1000
            }
        }
    });
    
    // تحديث وسيلة الإيضاح
    document.getElementById('successLegend').innerHTML = `
        <div class="legend-item">
            <div class="legend-color" style="background-color: #4caf50;"></div>
            <span>النجاح: ${successRate.toFixed(1)}%</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: #f44336;"></div>
            <span>الرسوب: ${(100 - successRate).toFixed(1)}%</span>
        </div>
    `;
    
    // 3. مخطط توزيع التقديرات (خطي مع نقاط)
    const gradesCtx = document.getElementById('gradesChart').getContext('2d');
    gradesChart = new Chart(gradesCtx, {
        type: 'line',
        data: {
            labels: gradeLabels,
            datasets: [{
                label: 'عدد الطلاب',
                data: gradeLevels,
                backgroundColor: 'rgba(26, 92, 158, 0.1)',
                borderColor: '#1a5c9e',
                borderWidth: 3,
                pointBackgroundColor: gradeColors,
                pointBorderColor: gradeBorderColors,
                pointBorderWidth: 2,
                pointRadius: 8,
                pointHoverRadius: 10,
                fill: true,
                tension: 0.3
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                y: {
                    beginAtZero: true,
                    grid: {
                        color: 'rgba(0,0,0,0.05)',
                        drawBorder: false
                    },
                    ticks: {
                        font: {
                            size: 12,
                            family: 'Tahoma'
                        },
                        color: '#555'
                    },
                    title: {
                        display: true,
                        text: 'عدد الطلاب',
                        font: {
                            size: 13,
                            weight: 'bold'
                        },
                        color: '#1a5c9e'
                    }
                },
                x: {
                    grid: {
                        display: false
                    },
                    ticks: {
                        font: {
                            size: 13,
                            weight: 'bold',
                            family: 'Tahoma'
                        },
                        color: '#333'
                    }
                }
            },
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    backgroundColor: 'rgba(0,0,0,0.7)',
                    titleFont: {
                        size: 13,
                        family: 'Tahoma'
                    },
                    bodyFont: {
                        size: 13,
                        family: 'Tahoma'
                    },
                    padding: 10,
                    cornerRadius: 8,
                    callbacks: {
                        label: function(context) {
                            const total = gradeLevels.reduce((a, b) => a + b, 0);
                            const percentage = total > 0 ? ((context.raw / total) * 100).toFixed(1) : 0;
                            return `${context.label}: ${context.raw} طالب (${percentage}%)`;
                        }
                    }
                }
            },
            animation: {
                duration: 1000,
                easing: 'easeInOutQuart'
            }
        }
    });
    
    // تحديث وسيلة الإيضاح
    let gradesLegendHTML = '';
    gradeLabels.forEach((label, index) => {
        const count = gradeLevels[index] || 0;
        const total = gradeLevels.reduce((a, b) => a + b, 0);
        const percentage = total > 0 ? ((count / total) * 100).toFixed(1) : 0;
        
        gradesLegendHTML += `
            <div class="legend-item">
                <div class="legend-color" style="background-color: ${gradeColors[index]};"></div>
                <span>${label}: ${count} (${percentage}%)</span>
            </div>
        `;
    });
    document.getElementById('gradesLegend').innerHTML = gradesLegendHTML;
}

// معالجة ملف PDF
async function handlePDF(file) {
    if (!file) {
        showAlert('الرجاء اختيار ملف PDF أولاً', 'error');
        return;
    }
    
    // التحقق من حجم الملف
    if (file.size > 10 * 1024 * 1024) { // 10MB
        showAlert('حجم الملف كبير جداً. الرجاء اختيار ملف أصغر من 10MB', 'error');
        return;
    }
    
    document.getElementById('loading').style.display = 'block';
    showAlert('جاري قراءة ملف PDF وتحليل البيانات...', 'warning');
    
    try {
        const reader = new FileReader();
        
        reader.onload = async function(event) {
            try {
                const typedarray = new Uint8Array(event.target.result);
                const pdf = await pdfjsLib.getDocument({data: typedarray}).promise;
                let text = '';
                
                // استخراج النص من أول 5 صفحات (لتحسين السرعة)
                const pageLimit = Math.min(pdf.numPages, 5);
                for (let i = 1; i <= pageLimit; i++) {
                    const page = await pdf.getPage(i);
                    const content = await page.getTextContent();
                    content.items.forEach(item => {
                        text += item.str + ' ';
                    });
                }
                
                await analyzeWithGemini(text);
                
            } catch (error) {
                console.error('خطأ في قراءة ملف PDF:', error);
                showAlert('خطأ في قراءة ملف PDF. تأكد من صحة الملف.', 'error');
                document.getElementById('loading').style.display = 'none';
            }
        };
        
        reader.onerror = function() {
            showAlert('خطأ في قراءة الملف. حاول مرة أخرى.', 'error');
            document.getElementById('loading').style.display = 'none';
        };
        
        reader.readAsArrayBuffer(file);
        
    } catch (error) {
        console.error('خطأ في معالجة الملف:', error);
        showAlert('حدث خطأ غير متوقع في معالجة الملف.', 'error');
        document.getElementById('loading').style.display = 'none';
    }
}

// تحليل النص باستخدام Gemini API
async function analyzeWithGemini(text) {
    const apiKey = document.getElementById('apiKey').value.trim();
    
    if (!apiKey) {
        showAlert('الرجاء إدخال مفتاح Gemini API للحصول على تحليل ذكي', 'warning');
        useSampleData();
        document.getElementById('loading').style.display = 'none';
        return;
    }
    
    try {
        const prompt = `أنت محلل نتائج طلاب محترف. قم بتحليل ملف النتائج هذا واستخرج المعلومات التالية:

1. معلومات عامة:
- اسم المادة (مثل: الرياضيات، العلوم، اللغة العربية)
- الصف الدراسي (مثل: الثالث الثانوي، الثاني المتوسط)
- الفصل الدراسي (مثل: الأول، الثاني)
- درجة الاختبار الكاملة (رقم)
- تاريخ الاختبار (بالهجري أو الميلادي)

2. التقديرات (عدد الطلاب في كل فئة):
- ممتاز (90-100%)
- جيد جداً (80-89%)
- جيد (70-79%)
- مقبول (60-69%)
- ضعيف (أقل من 60%)

3. إحصائيات:
- عدد الطلاب
- متوسط الدرجات
- أعلى درجة
- أدنى درجة
- نسبة النجاح (النسبة المئوية للطلاب الناجحين)

4. تحليل نصي موجز (3-4 جمل) يلخص أداء الطلاب ويقدم توصيات.

أرجع النتائج بصيغة JSON منظمة بالشكل التالي:
{
  "subject": "اسم المادة",
  "grade": "الصف",
  "term": "الفصل",
  "max_score": "الدرجة الكاملة",
  "exam_date": "تاريخ الاختبار",
  "analysis": "تحليل نصي",
  "levels": [عدد_ممتاز, عدد_جيد_جدا, عدد_جيد, عدد_مقبول, عدد_ضعيف],
  "stats": {
    "count": عدد_الطلاب,
    "average": المتوسط,
    "max": أعلى_درجة,
    "min": أدنى_درجة,
    "success": نسبة_النجاح
  }
}

البيانات:
${text.substring(0, 3000)}`;
        
        const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-lite:generateContent?key=${apiKey}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                contents: [{
                    parts: [{
                        text: prompt
                    }]
                }]
            })
        });
        
        if (!response.ok) {
            throw new Error(`خطأ في الاتصال: ${response.status}`);
        }
        
        const data = await response.json();
        
        if (!data.candidates || !data.candidates[0].content.parts[0].text) {
            throw new Error('استجابة غير صالحة من API');
        }
        
        const responseText = data.candidates[0].content.parts[0].text;
        const jsonMatch = responseText.match(/\{[\s\S]*\}/);
        
        if (!jsonMatch) {
            throw new Error('لم يتم العثور على JSON في الاستجابة');
        }
        
        const jsonData = JSON.parse(jsonMatch[0]);
        renderData(jsonData);
        showAlert('تم تحليل البيانات بنجاح!', 'success');
        
    } catch (error) {
        console.error('خطأ في تحليل البيانات:', error);
        showAlert(`خطأ في التحليل: ${error.message}. يتم استخدام بيانات تجريبية.`, 'error');
        useSampleData();
    } finally {
        document.getElementById('loading').style.display = 'none';
    }
}

// استخدام بيانات تجريبية
function useSampleData() {
    const sampleData = {
        subject: "الرياضيات",
        grade: "الثاني الثانوي",
        term: "الثاني",
        max_score: "100",
        exam_date: "15/06/1446 هـ",
        analysis: "أظهرت نتائج الاختبار تحسناً ملحوظاً في مستوى الطلاب مقارنة بالاختبار السابق. لوحظ تركيز الضعف في الأسئلة المتعلقة بالتفاضل والتكامل، بينما أبدى الطلاب تفوقاً في الأسئلة الجبرية. نوصي بتكثيف التدريبات العملية في مواضيع التفاضل وحل المسائل التطبيقية لتحسين النتائج في الاختبارات القادمة.",
        levels: [8, 12, 15, 8, 5],
        stats: {
            count: 48,
            average: 76.5,
            max: 98,
            min: 45,
            success: 85.4
        }
    };
    
    renderData(sampleData);
}

// عرض البيانات في التقرير
function renderData(data) {
    // تحديث المعلومات العامة
    document.getElementById('subject').innerText = data.subject;
    document.getElementById('grade').innerText = data.grade;
    document.getElementById('term').innerText = data.term;
    document.getElementById('maxScore').innerText = data.max_score;
    document.getElementById('examDate').innerText = data.exam_date;
    
    // تحديث التحليل النصي
    document.getElementById('analysisText').innerHTML = data.analysis.replace(/\n/g, '<br>');
    document.getElementById('analysisStatus').innerHTML = '<i class="fas fa-check-circle"></i> مكتمل';
    document.getElementById('analysisStatus').style.background = 'linear-gradient(135deg, #d4edda, #c3e6cb)';
    document.getElementById('analysisStatus').style.color = '#155724';
    
    // تحديث الإحصائيات
    document.getElementById('studentsCount').innerText = data.stats.count;
    document.getElementById('averageScore').innerText = data.stats.average.toFixed(1);
    document.getElementById('maxStudentScore').innerText = data.stats.max;
    document.getElementById('minStudentScore').innerText = data.stats.min;
    document.getElementById('successRate').innerText = data.stats.success.toFixed(1) + '%';
    
    // إنشاء المخططات المحسنة
    initEnhancedCharts(data);
}

// تصدير ومشاركة التقرير مع تحسين جودة PDF
async function exportAndShare() {
    try {
        document.getElementById('loading').style.display = 'block';
        showAlert('جاري إنشاء ملف PDF عالي الجودة...', 'warning');
        
        // إخفاء عناصر غير ضرورية أثناء التصدير
        const reportElement = document.getElementById('report');
        const watermark = reportElement.querySelector('.watermark');
        if (watermark) watermark.style.display = 'none';
        
        // استخدام دقة عالية للتصدير
        const canvas = await html2canvas(reportElement, {
            scale: 4, // دقة عالية جداً
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false,
            allowTaint: true,
            removeContainer: true,
            windowWidth: reportElement.scrollWidth,
            windowHeight: reportElement.scrollHeight
        });
        
        // إعادة عرض العلامة المائية
        if (watermark) watermark.style.display = 'block';
        
        const imgData = canvas.toDataURL('image/jpeg', 1.0);
        
        // إنشاء PDF مع هوامش وإعدادات محسنة
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF({
            orientation: 'portrait',
            unit: 'mm',
            format: 'a4',
            compress: true
        });
        
        const pageWidth = pdf.internal.pageSize.getWidth();
        const pageHeight = pdf.internal.pageSize.getHeight();
        
        // إضافة صورة التقرير مع هوامش
        const margin = 10;
        const imgWidth = pageWidth - (margin * 2);
        const imgHeight = (canvas.height * imgWidth) / canvas.width;
        
        // إضافة الصورة الرئيسية
        pdf.addImage(imgData, 'JPEG', margin, margin, imgWidth, imgHeight);
        
        // إضافة معلومات إضافية في رأس الصفحة
        pdf.setFontSize(10);
        pdf.setTextColor(100, 100, 100);
        pdf.text('تقرير تحليل النتائج - مدرسة النخبة النموذجية', pageWidth / 2, 8, { align: 'center' });
        
        // إضافة تذييل الصفحة
        const currentDate = new Date().toLocaleDateString('ar-SA');
        pdf.text(`تاريخ الإصدار: ${currentDate}`, pageWidth / 2, pageHeight - 10, { align: 'center' });
        pdf.text(`الصفحة 1 من 1`, pageWidth - 10, pageHeight - 10, { align: 'right' });
        
        // حفظ PDF مع اسم ملف مناسب
        const fileName = `تقرير_نتائج_${document.getElementById('subject').innerText || 'الاختبار'}_${new Date().toISOString().slice(0,10)}.pdf`;
        const pdfBlob = pdf.output('blob');
        const pdfFile = new File([pdfBlob], fileName, { 
            type: 'application/pdf',
            lastModified: new Date().getTime()
        });
        
        // محاولة المشاركة عبر واتساب
        if (navigator.share && navigator.canShare && navigator.canShare({ files: [pdfFile] })) {
            try {
                await navigator.share({
                    files: [pdfFile],
                    title: 'تقرير تحليل النتائج',
                    text: `تقرير تحليل نتائج ${document.getElementById('subject').innerText || 'الاختبار'} - ${document.getElementById('grade').innerText || ''}`
                });
                showAlert('تم مشاركة التقرير بنجاح عبر واتساب!', 'success');
            } catch (shareError) {
                console.log('مشاركة عبر واتساب فشلت، يتم الحفظ بدلاً من ذلك:', shareError);
                pdf.save(fileName);
                showAlert('تم حفظ التقرير كملف PDF!', 'success');
            }
        } else {
            pdf.save(fileName);
            showAlert('تم حفظ التقرير كملف PDF!', 'success');
        }
        
    } catch (error) {
        console.error('خطأ في التصدير:', error);
        showAlert('حدث خطأ أثناء إنشاء التقرير. حاول مرة أخرى.', 'error');
    } finally {
        document.getElementById('loading').style.display = 'none';
    }
}

// طباعة التقرير
function printReport() {
    // إعداد خاص للطباعة
    const originalStyles = document.querySelector('style').innerHTML;
    const printStyles = `
        @media print {
            .actions-container, .alert, .loading { display: none !important; }
            body { padding: 0; background: white; }
            .page { box-shadow: none; margin: 0; padding: 15mm; width: 100%; }
            .charts-container { page-break-inside: avoid; }
            .chart-box { page-break-inside: avoid; }
            .watermark { opacity: 0.1; }
        }
    `;
    
    document.querySelector('style').innerHTML = originalStyles + printStyles;
    window.print();
    
    // استعادة الأنماط الأصلية بعد الطباعة
    setTimeout(() => {
        document.querySelector('style').innerHTML = originalStyles;
    }, 1000);
}

// حفظ التقرير كصورة
async function saveAsImage() {
    try {
        document.getElementById('loading').style.display = 'block';
        showAlert('جاري حفظ الصورة عالية الجودة...', 'warning');
        
        const canvas = await html2canvas(document.getElementById('report'), {
            scale: 3,
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false
        });
        
        const link = document.createElement('a');
        const fileName = `تقرير_نتائج_${document.getElementById('subject').innerText || 'الاختبار'}_${new Date().toISOString().slice(0,10)}.png`;
        link.download = fileName;
        link.href = canvas.toDataURL('image/png', 1.0);
        link.click();
        
        showAlert('تم حفظ الصورة عالية الجودة بنجاح!', 'success');
        
    } catch (error) {
        console.error('خطأ في حفظ الصورة:', error);
        showAlert('حدث خطأ أثناء حفظ الصورة.', 'error');
    } finally {
        document.getElementById('loading').style.display = 'none';
    }
}

// عرض رسائل التنبيه
function showAlert(message, type) {
    const alertDiv = document.getElementById('alertMessage');
    alertDiv.textContent = message;
    alertDiv.className = `alert ${type}`;
    alertDiv.style.display = 'block';
    
    setTimeout(() => {
        alertDiv.style.display = 'none';
    }, 5000);
}

// إعادة تعيين النموذج
function resetForm() {
    if (confirm('هل أنت متأكد من رغبتك في مسح جميع البيانات؟')) {
        document.getElementById('apiKey').value = '';
        document.getElementById('pdfFile').value = '';
        
        const resetData = {
            subject: "-",
            grade: "-",
            term: "-",
            max_score: "-",
            exam_date: "-",
            analysis: "سيظهر هنا التحليل الذكي للنتائج بعد رفع ملف PDF وإدخال مفتاح API. سيتضمن التقرير تحليلًا شاملاً للأداء ومقارنات مع النتائج السابقة وتوصيات للتحسين.",
            levels: [0, 0, 0, 0, 0],
            stats: {
                count: "-",
                average: "-",
                max: "-",
                min: "-",
                success: 0
            }
        };
        
        renderData(resetData);
        document.getElementById('analysisStatus').innerHTML = '<i class="fas fa-clock"></i> قيد الانتظار';
        document.getElementById('analysisStatus').style.background = '#e0e0e0';
        document.getElementById('analysisStatus').style.color = '#333';
        
        showAlert('تم مسح جميع البيانات بنجاح.', 'success');
    }
}

// تهيئة الصفحة
window.onload = function() {
    updateCurrentDate();
    
    // تهيئة مخططات فارغة
    const initialData = {
        subject: "-",
        grade: "-",
        term: "-",
        max_score: "100",
        exam_date: "-",
        analysis: "",
        levels: [0, 0, 0, 0, 0],
        stats: {
            count: 0,
            average: 0,
            max: 0,
            min: 0,
            success: 0
        }
    };
    
    initEnhancedCharts(initialData);
    
    // إضافة تأثيرات عند التمرير
    window.addEventListener('scroll', function() {
        const scrollTop = window.pageYOffset || document.documentElement.scrollTop;
        const reportElement = document.getElementById('report');
        
        if (scrollTop > 100) {
            reportElement.style.transform = 'translateY(-10px)';
            reportElement.style.boxShadow = '0 15px 40px rgba(0, 0, 0, 0.2)';
        } else {
            reportElement.style.transform = 'translateY(0)';
            reportElement.style.boxShadow = '0 8px 30px rgba(0, 0, 0, 0.15)';
        }
    });
};
</script>
</body>
</html>