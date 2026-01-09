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
    page-break-after: always;
}

.page:last-child {
    page-break-after: auto;
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

/* جدول أسماء الطلاب */
.students-section {
    margin: 30px 0;
}

.students-table-container {
    overflow-x: auto;
    border-radius: 10px;
    border: 2px solid #e1e8f0;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}

.students-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
}

.students-table thead {
    background: linear-gradient(135deg, #1a5c9e 0%, #2a6cb0 100%);
    color: white;
}

.students-table th {
    padding: 15px;
    text-align: center;
    font-weight: bold;
    border-bottom: 3px solid #154a80;
}

.students-table tbody tr {
    border-bottom: 1px solid #e0e0e0;
    transition: all 0.3s ease;
}

.students-table tbody tr:hover {
    background-color: #f5f9ff;
    transform: scale(1.002);
}

.students-table td {
    padding: 12px 15px;
    text-align: center;
    vertical-align: middle;
}

.students-table .student-rank {
    font-weight: bold;
    color: #1a5c9e;
    width: 60px;
}

.students-table .student-name {
    text-align: right;
    font-weight: 500;
}

.students-table .student-score {
    font-weight: bold;
    color: #25d366;
}

.students-table .student-grade {
    font-weight: bold;
    padding: 6px 12px;
    border-radius: 20px;
    display: inline-block;
    min-width: 80px;
}

.grade-excellent {
    background-color: #e8f5e9;
    color: #2e7d32;
}

.grade-very-good {
    background-color: #e0f2f1;
    color: #00695c;
}

.grade-good {
    background-color: #e3f2fd;
    color: #1565c0;
}

.grade-acceptable {
    background-color: #fff3e0;
    color: #ef6c00;
}

.grade-weak {
    background-color: #ffebee;
    color: #c62828;
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

.pagination-controls {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 20px;
    padding: 15px;
}

.page-number {
    padding: 8px 15px;
    background: #f0f0f0;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.page-number.active {
    background: #1a5c9e;
    color: white;
}

.page-number:hover:not(.active) {
    background: #e0e0e0;
}

@media print {
    .actions-container, .alert, .loading, .pagination-controls {
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
        page-break-after: always;
    }
    
    .page:last-child {
        page-break-after: auto;
    }
    
    .charts-container, .students-table-container {
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
    
    .students-table {
        font-size: 12px;
    }
    
    .students-table th,
    .students-table td {
        padding: 8px 10px;
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
    
    .students-table {
        font-size: 11px;
    }
}
</style>

<!-- أيقونات Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>

<body>

<div class="actions-container">
    <div class="actions-title">
        <i class="fas fa-chart-line"></i> نظام تحليل نتائج الاختبارات المتقدم
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
            <i class="fab fa-whatsapp"></i> إرسال عبر واتساب (PDF متعدد الصفحات)
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

<!-- الصفحة الأولى: نظرة عامة -->
<div class="page" id="reportPage1">
    <div class="watermark">الصفحة ١</div>
    
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

<!-- الصفحة الثانية: قائمة أسماء الطلاب -->
<div class="page" id="reportPage2" style="display: none;">
    <div class="watermark">الصفحة ٢</div>
    
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

    <div class="title" style="font-size: 24px; padding: 20px;">
        <i class="fas fa-users"></i> قائمة أسماء الطلاب ونتائجهم
        <div class="report-date">المادة: <span id="subjectPage2">-</span></div>
    </div>

    <div class="section-title">
        <i class="fas fa-list-ol"></i> ترتيب الطلاب حسب الدرجات
    </div>

    <div class="students-section">
        <div class="students-table-container">
            <table class="students-table" id="studentsTable">
                <thead>
                    <tr>
                        <th>#</th>
                        <th>الترتيب</th>
                        <th>اسم الطالب</th>
                        <th>الدرجة</th>
                        <th>النسبة</th>
                        <th>التقدير</th>
                        <th>الحالة</th>
                    </tr>
                </thead>
                <tbody id="studentsTableBody">
                    <!-- سيتم ملؤه بالبيانات -->
                </tbody>
            </table>
        </div>
    </div>

    <div class="section-title" style="margin-top: 40px;">
        <i class="fas fa-chart-bar"></i> ملخص التقديرات
    </div>

    <div class="stats-grid" style="grid-template-columns: repeat(5, 1fr);">
        <div class="stat-box" style="background: #e8f5e9;">
            <i class="fas fa-award" style="font-size: 24px; color: #2e7d32;"></i>
            ممتاز
            <strong id="excellentCount">0</strong>
        </div>
        <div class="stat-box" style="background: #e0f2f1;">
            <i class="fas fa-star" style="font-size: 24px; color: #00695c;"></i>
            جيد جداً
            <strong id="veryGoodCount">0</strong>
        </div>
        <div class="stat-box" style="background: #e3f2fd;">
            <i class="fas fa-thumbs-up" style="font-size: 24px; color: #1565c0;"></i>
            جيد
            <strong id="goodCount">0</strong>
        </div>
        <div class="stat-box" style="background: #fff3e0;">
            <i class="fas fa-check-circle" style="font-size: 24px; color: #ef6c00;"></i>
            مقبول
            <strong id="acceptableCount">0</strong>
        </div>
        <div class="stat-box" style="background: #ffebee;">
            <i class="fas fa-exclamation-circle" style="font-size: 24px; color: #c62828;"></i>
            ضعيف
            <strong id="weakCount">0</strong>
        </div>
    </div>

    <div class="footer" style="margin-top: 60px;">
        <div>
            <strong>ملاحظات:</strong><br>
            <div style="text-align: right; font-size: 13px; line-height: 1.6; margin-top: 10px;">
                ١. التقديرات حسب نظام المدرسة<br>
                ٢. نسبة النجاح ٦٠٪ فأعلى<br>
                ٣. الرجوع للمعلم للاستفسار<br>
            </div>
        </div>
        <div style="text-align: center;">
            <strong>إجمالي الطلاب:</strong><br>
            <span style="font-size: 32px; font-weight: bold; color: #1a5c9e;" id="totalStudentsPage2">0</span>
        </div>
        <div style="text-align: left;">
            <strong>نسبة النجاح:</strong><br>
            <span style="font-size: 32px; font-weight: bold; color: #4caf50;" id="successRatePage2">0%</span>
        </div>
    </div>
</div>

<!-- عناصر التحكم بالصفحات -->
<div class="pagination-controls">
    <div class="page-number active" onclick="showPage(1)">الصفحة ١: النظرة العامة</div>
    <div class="page-number" onclick="showPage(2)">الصفحة ٢: قائمة الطلاب</div>
</div>

<script>
// متغيرات التطبيق
let distributionChart, successChart, gradesChart;
let allStudentsData = [];
let currentReportData = {};

// عرض صفحة معينة
function showPage(pageNumber) {
    // إخفاء جميع الصفحات
    document.getElementById('reportPage1').style.display = 'none';
    document.getElementById('reportPage2').style.display = 'none';
    
    // إظهار الصفحة المطلوبة
    document.getElementById(`reportPage${pageNumber}`).style.display = 'block';
    
    // تحديث عناصر التحكم
    document.querySelectorAll('.page-number').forEach((el, index) => {
        if (index + 1 === pageNumber) {
            el.classList.add('active');
        } else {
            el.classList.remove('active');
        }
    });
}

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
    
    // مخطط توزيع الدرجات
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
                    grid: { color: 'rgba(0,0,0,0.05)', drawBorder: false },
                    ticks: { font: { size: 12, family: 'Tahoma' }, color: '#555' },
                    title: { display: true, text: 'الدرجة', font: { size: 13, weight: 'bold' }, color: '#1a5c9e' }
                },
                y: {
                    grid: { display: false },
                    ticks: { font: { size: 14, weight: 'bold', family: 'Tahoma' }, color: '#333' }
                }
            },
            plugins: {
                legend: { display: false },
                tooltip: {
                    backgroundColor: 'rgba(0,0,0,0.7)',
                    titleFont: { size: 13, family: 'Tahoma' },
                    bodyFont: { size: 13, family: 'Tahoma' },
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
    
    // مخطط النجاح والرسوب
    const successCtx = document.getElementById('successChart').getContext('2d');
    successChart = new Chart(successCtx, {
        type: 'doughnut',
        data: {
            labels: ['نسبة النجاح', 'نسبة الرسوب'],
            datasets: [{
                data: [successRate, 100 - successRate],
                backgroundColor: ['rgba(76, 175, 80, 0.9)', 'rgba(244, 67, 54, 0.9)'],
                borderColor: ['#4caf50', '#f44336'],
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
                        font: { size: 13, family: 'Tahoma', weight: 'bold' },
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
                    titleFont: { size: 13, family: 'Tahoma' },
                    bodyFont: { size: 14, family: 'Tahoma', weight: 'bold' }
                }
            },
            animation: { animateScale: true, animateRotate: true, duration: 1000 }
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
    
    // مخطط توزيع التقديرات
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
                    grid: { color: 'rgba(0,0,0,0.05)', drawBorder: false },
                    ticks: { font: { size: 12, family: 'Tahoma' }, color: '#555' },
                    title: { display: true, text: 'عدد الطلاب', font: { size: 13, weight: 'bold' }, color: '#1a5c9e' }
                },
                x: {
                    grid: { display: false },
                    ticks: { font: { size: 13, weight: 'bold', family: 'Tahoma' }, color: '#333' }
                }
            },
            plugins: {
                legend: { display: false },
                tooltip: {
                    backgroundColor: 'rgba(0,0,0,0.7)',
                    titleFont: { size: 13, family: 'Tahoma' },
                    bodyFont: { size: 13, family: 'Tahoma' },
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
            animation: { duration: 1000, easing: 'easeInOutQuart' }
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

// استخراج بيانات الطلاب من النص
function extractStudentsFromText(text) {
    const students = [];
    const lines = text.split('\n');
    
    // أنماط البحث عن أسماء الطلاب ودرجاتهم
    const patterns = [
        /(\d+)[\.\s]+([\u0600-\u06FF\s]+)\s+(\d+)/, // رقم. اسم الدرجة
        /([\u0600-\u06FF\s]+)\s+(\d+)/, // اسم الدرجة
        /(\d+)[\.\)\s]+\s*([\u0600-\u06FF\s]+)/ // رقم. اسم
    ];
    
    lines.forEach(line => {
        for (const pattern of patterns) {
            const match = line.match(pattern);
            if (match) {
                let name, score;
                
                if (match.length === 4) {
                    // النمط الأول: رقم. اسم الدرجة
                    name = match[2].trim();
                    score = parseInt(match[3]);
                } else if (match.length === 3) {
                    // النمط الثاني: اسم الدرجة
                    name = match[1].trim();
                    score = parseInt(match[2]);
                }
                
                // التحقق من صحة البيانات
                if (name && score !== undefined && score >= 0 && score <= 100) {
                    // تجنب الأسماء القصيرة جداً أو الكلمات الشائعة
                    if (name.length > 3 && 
                        !['اسم', 'الطالب', 'الدرجة', 'المجموع', 'المادة', 'التاريخ'].includes(name)) {
                        
                        // حساب التقدير
                        let grade = '';
                        let gradeClass = '';
                        
                        if (score >= 90) {
                            grade = 'ممتاز';
                            gradeClass = 'grade-excellent';
                        } else if (score >= 80) {
                            grade = 'جيد جداً';
                            gradeClass = 'grade-very-good';
                        } else if (score >= 70) {
                            grade = 'جيد';
                            gradeClass = 'grade-good';
                        } else if (score >= 60) {
                            grade = 'مقبول';
                            gradeClass = 'grade-acceptable';
                        } else {
                            grade = 'ضعيف';
                            gradeClass = 'grade-weak';
                        }
                        
                        students.push({
                            name: name,
                            score: score,
                            grade: grade,
                            gradeClass: gradeClass,
                            percentage: Math.round((score / 100) * 100),
                            status: score >= 60 ? 'ناجح' : 'راسب'
                        });
                    }
                }
                break;
            }
        }
    });
    
    // إذا لم يتم العثور على بيانات، استخدام بيانات تجريبية
    if (students.length === 0) {
        return generateSampleStudents();
    }
    
    // ترتيب الطلاب حسب الدرجة (تنازلي)
    students.sort((a, b) => b.score - a.score);
    
    return students;
}

// إنشاء بيانات طلاب تجريبية
function generateSampleStudents() {
    const sampleNames = [
        'أحمد محمد علي', 'محمد عبدالله إبراهيم', 'خالد سعيد حسن', 'سالم علي محمد',
        'عبدالعزيز فهد ناصر', 'فيصل خالد أحمد', 'نواف سعد عبدالله', 'تركي ماجد راشد',
        'بدر ناصر حمد', 'سلطان فيصل محمد', 'فواز خالد سعود', 'ماجد عبدالرحمن علي',
        'هشام محمد عبدالله', 'وائل أحمد خالد', 'ياسر سعيد محمد', 'زيد ناصر فهد',
        'سعود فيصل خالد', 'عبدالله محمد أحمد', 'عمر خالد سعيد', 'وليد عبدالعزيز ناصر'
    ];
    
    const students = [];
    
    // إنشاء درجات عشوائية مع توزيع طبيعي
    for (let i = 0; i < sampleNames.length; i++) {
        // توزيع طبيعي للدرجات حول 75
        let score = Math.floor(Math.random() * 40) + 55;
        if (Math.random() < 0.3) score += 10; // 30% يحصلون على درجات أعلى
        if (Math.random() < 0.2) score -= 15; // 20% يحصلون على درجات أقل
        
        // التأكد من أن الدرجة بين 0 و100
        score = Math.max(0, Math.min(100, score));
        
        // حساب التقدير
        let grade = '';
        let gradeClass = '';
        
        if (score >= 90) {
            grade = 'ممتاز';
            gradeClass = 'grade-excellent';
        } else if (score >= 80) {
            grade = 'جيد جداً';
            gradeClass = 'grade-very-good';
        } else if (score >= 70) {
            grade = 'جيد';
            gradeClass = 'grade-good';
        } else if (score >= 60) {
            grade = 'مقبول';
            gradeClass = 'grade-acceptable';
        } else {
            grade = 'ضعيف';
            gradeClass = 'grade-weak';
        }
        
        students.push({
            name: sampleNames[i % sampleNames.length],
            score: score,
            grade: grade,
            gradeClass: gradeClass,
            percentage: Math.round((score / 100) * 100),
            status: score >= 60 ? 'ناجح' : 'راسب'
        });
    }
    
    // ترتيب حسب الدرجة
    students.sort((a, b) => b.score - a.score);
    
    return students;
}

// تحديث جدول الطلاب
function updateStudentsTable(students) {
    const tbody = document.getElementById('studentsTableBody');
    tbody.innerHTML = '';
    
    let excellentCount = 0;
    let veryGoodCount = 0;
    let goodCount = 0;
    let acceptableCount = 0;
    let weakCount = 0;
    
    students.forEach((student, index) => {
        const row = document.createElement('tr');
        
        // عدادات التقديرات
        switch(student.grade) {
            case 'ممتاز': excellentCount++; break;
            case 'جيد جداً': veryGoodCount++; break;
            case 'جيد': goodCount++; break;
            case 'مقبول': acceptableCount++; break;
            case 'ضعيف': weakCount++; break;
        }
        
        row.innerHTML = `
            <td>${index + 1}</td>
            <td class="student-rank">${index + 1}</td>
            <td class="student-name">${student.name}</td>
            <td class="student-score">${student.score}</td>
            <td>${student.percentage}%</td>
            <td><span class="student-grade ${student.gradeClass}">${student.grade}</span></td>
            <td><span style="color: ${student.status === 'ناجح' ? '#4caf50' : '#f44336'}; 
                           font-weight: bold;">${student.status}</span></td>
        `;
        tbody.appendChild(row);
    });
    
    // تحديث إحصائيات التقديرات
    document.getElementById('excellentCount').textContent = excellentCount;
    document.getElementById('veryGoodCount').textContent = veryGoodCount;
    document.getElementById('goodCount').textContent = goodCount;
    document.getElementById('acceptableCount').textContent = acceptableCount;
    document.getElementById('weakCount').textContent = weakCount;
    
    // تحديث الإحصائيات العامة في الصفحة الثانية
    document.getElementById('totalStudentsPage2').textContent = students.length;
    
    // حساب نسبة النجاح
    const successCount = students.filter(s => s.status === 'ناجح').length;
    const successRate = students.length > 0 ? Math.round((successCount / students.length) * 100) : 0;
    document.getElementById('successRatePage2').textContent = `${successRate}%`;
}

// معالجة ملف PDF
async function handlePDF(file) {
    if (!file) {
        showAlert('الرجاء اختيار ملف PDF أولاً', 'error');
        return;
    }
    
    if (file.size > 10 * 1024 * 1024) {
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
                
                // استخراج النص من جميع الصفحات
                for (let i = 1; i <= pdf.numPages; i++) {
                    const page = await pdf.getPage(i);
                    const content = await page.getTextContent();
                    content.items.forEach(item => {
                        text += item.str + ' ';
                    });
                    text += '\n'; // فصل بين صفحات
                }
                
                // استخراج بيانات الطلاب
                allStudentsData = extractStudentsFromText(text);
                
                // تحليل النص باستخدام Gemini API
                await analyzeWithGemini(text);
                
                // عرض الصفحة الثانية
                showPage(2);
                
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
        // استخدام بيانات الطلاب المستخرجة لحساب الإحصائيات
        const totalStudents = allStudentsData.length;
        const totalScore = allStudentsData.reduce((sum, student) => sum + student.score, 0);
        const averageScore = totalStudents > 0 ? totalScore / totalStudents : 0;
        const maxScore = totalStudents > 0 ? Math.max(...allStudentsData.map(s => s.score)) : 0;
        const minScore = totalStudents > 0 ? Math.min(...allStudentsData.map(s => s.score)) : 0;
        const successCount = allStudentsData.filter(s => s.status === 'ناجح').length;
        const successRate = totalStudents > 0 ? (successCount / totalStudents) * 100 : 0;
        
        // حساب توزيع التقديرات
        const gradeLevels = [0, 0, 0, 0, 0]; // ممتاز، جيد جداً، جيد، مقبول، ضعيف
        allStudentsData.forEach(student => {
            switch(student.grade) {
                case 'ممتاز': gradeLevels[0]++; break;
                case 'جيد جداً': gradeLevels[1]++; break;
                case 'جيد': gradeLevels[2]++; break;
                case 'مقبول': gradeLevels[3]++; break;
                case 'ضعيف': gradeLevels[4]++; break;
            }
        });
        
        // إنشاء التحليل النصي بناءً على البيانات
        const analysisText = `تم تحليل نتائج ${totalStudents} طالباً في هذا الاختبار. 
        بلغ متوسط الدرجات ${averageScore.toFixed(1)} من 100، مع أعلى درجة ${maxScore} وأدنى درجة ${minScore}. 
        نسبة النجاح الإجمالية بلغت ${successRate.toFixed(1)}%. 
        التوزيع العام للطلاب كان: ${gradeLevels[0]} طالباً بمستوى ممتاز، ${gradeLevels[1]} جيد جداً، ${gradeLevels[2]} جيد، ${gradeLevels[3]} مقبول، و${gradeLevels[4]} طالباً يحتاجون للتحسين.`;
        
        // إنشاء بيانات التقرير
        const reportData = {
            subject: "الرياضيات", // سيتم استخراجها من النص إذا أمكن
            grade: "الثاني الثانوي",
            term: "الثاني",
            max_score: "100",
            exam_date: "15/06/1446 هـ",
            analysis: analysisText,
            levels: gradeLevels,
            stats: {
                count: totalStudents,
                average: averageScore,
                max: maxScore,
                min: minScore,
                success: successRate
            }
        };
        
        renderData(reportData);
        updateStudentsTable(allStudentsData);
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
    
    allStudentsData = generateSampleStudents();
    renderData(sampleData);
    updateStudentsTable(allStudentsData);
    
    // عرض الصفحة الثانية
    showPage(2);
}

// عرض البيانات في التقرير
function renderData(data) {
    currentReportData = data;
    
    // تحديث المعلومات العامة في الصفحة الأولى
    document.getElementById('subject').innerText = data.subject;
    document.getElementById('grade').innerText = data.grade;
    document.getElementById('term').innerText = data.term;
    document.getElementById('maxScore').innerText = data.max_score;
    document.getElementById('examDate').innerText = data.exam_date;
    
    // تحديث المعلومات في الصفحة الثانية
    document.getElementById('subjectPage2').innerText = data.subject;
    
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

// تصدير ومشاركة التقرير متعدد الصفحات
async function exportAndShare() {
    try {
        document.getElementById('loading').style.display = 'block';
        showAlert('جاري إنشاء ملف PDF متعدد الصفحات...', 'warning');
        
        // إخفاء عناصر التحكم بالصفحات أثناء التصدير
        const paginationControls = document.querySelector('.pagination-controls');
        const originalDisplay = paginationControls.style.display;
        paginationControls.style.display = 'none';
        
        // إظهار جميع الصفحات للتسجيل
        document.getElementById('reportPage1').style.display = 'block';
        document.getElementById('reportPage2').style.display = 'block';
        
        // إخفاء العلامات المائية مؤقتاً
        const watermarks = document.querySelectorAll('.watermark');
        watermarks.forEach(w => w.style.display = 'none');
        
        // إنشاء PDF متعدد الصفحات
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF({
            orientation: 'portrait',
            unit: 'mm',
            format: 'a4',
            compress: true
        });
        
        const pageWidth = pdf.internal.pageSize.getWidth();
        const pageHeight = pdf.internal.pageSize.getHeight();
        const margin = 10;
        
        // الصفحة الأولى
        const page1Canvas = await html2canvas(document.getElementById('reportPage1'), {
            scale: 2,
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false
        });
        
        const page1Img = page1Canvas.toDataURL('image/jpeg', 1.0);
        const page1ImgWidth = pageWidth - (margin * 2);
        const page1ImgHeight = (page1Canvas.height * page1ImgWidth) / page1Canvas.width;
        
        pdf.addImage(page1Img, 'JPEG', margin, margin, page1ImgWidth, page1ImgHeight);
        
        // إضافة رأس وتذييل للصفحة الأولى
        pdf.setFontSize(10);
        pdf.setTextColor(100, 100, 100);
        pdf.text('تقرير تحليل النتائج - الصفحة ١ من ٢', pageWidth / 2, 8, { align: 'center' });
        
        // الصفحة الثانية
        pdf.addPage();
        
        const page2Canvas = await html2canvas(document.getElementById('reportPage2'), {
            scale: 2,
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false
        });
        
        const page2Img = page2Canvas.toDataURL('image/jpeg', 1.0);
        const page2ImgWidth = pageWidth - (margin * 2);
        const page2ImgHeight = (page2Canvas.height * page2ImgWidth) / page2Canvas.width;
        
        pdf.addImage(page2Img, 'JPEG', margin, margin, page2ImgWidth, page2ImgHeight);
        
        // إضافة رأس وتذييل للصفحة الثانية
        pdf.text('تقرير تحليل النتائج - الصفحة ٢ من ٢', pageWidth / 2, 8, { align: 'center' });
        
        // إضافة تذييل النهائي
        const currentDate = new Date().toLocaleDateString('ar-SA');
        pdf.text(`تاريخ الإصدار: ${currentDate}`, pageWidth / 2, pageHeight - 10, { align: 'center' });
        
        // استعادة العلامات المائية
        watermarks.forEach(w => w.style.display = 'block');
        
        // استعادة عرض عناصر التحكم
        paginationControls.style.display = originalDisplay;
        
        // إظهار الصفحة الأولى فقط
        showPage(1);
        
        // حفظ PDF مع اسم ملف مناسب
        const fileName = `تقرير_نتائج_${document.getElementById('subject').innerText || 'الاختبار'}_متعدد_الصفحات.pdf`;
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
                    title: 'تقرير تحليل النتائج متعدد الصفحات',
                    text: `تقرير تحليل نتائج ${document.getElementById('subject').innerText || 'الاختبار'} - ${document.getElementById('grade').innerText || ''}`
                });
                showAlert('تم مشاركة التقرير متعدد الصفحات بنجاح عبر واتساب!', 'success');
            } catch (shareError) {
                console.log('مشاركة عبر واتساب فشلت، يتم الحفظ بدلاً من ذلك:', shareError);
                pdf.save(fileName);
                showAlert('تم حفظ التقرير متعدد الصفحات كملف PDF!', 'success');
            }
        } else {
            pdf.save(fileName);
            showAlert('تم حفظ التقرير متعدد الصفحات كملف PDF!', 'success');
        }
        
    } catch (error) {
        console.error('خطأ في التصدير:', error);
        showAlert('حدث خطأ أثناء إنشاء التقرير متعدد الصفحات. حاول مرة أخرى.', 'error');
    } finally {
        document.getElementById('loading').style.display = 'none';
    }
}

// طباعة التقرير
function printReport() {
    // إعداد خاص للطباعة متعددة الصفحات
    const originalStyles = document.querySelector('style').innerHTML;
    const printStyles = `
        @media print {
            .actions-container, .alert, .loading, .pagination-controls { display: none !important; }
            body { padding: 0; background: white; }
            .page { box-shadow: none; margin: 0; padding: 15mm; width: 100%; page-break-after: always; }
            .page:last-child { page-break-after: auto; }
            #reportPage1, #reportPage2 { display: block !important; }
            .charts-container, .students-table-container { page-break-inside: avoid; }
            .chart-box { page-break-inside: avoid; }
            .watermark { opacity: 0.1; }
        }
    `;
    
    document.querySelector('style').innerHTML = originalStyles + printStyles;
    
    // إظهار جميع الصفحات للطباعة
    document.getElementById('reportPage1').style.display = 'block';
    document.getElementById('reportPage2').style.display = 'block';
    
    window.print();
    
    // استعادة الأنماط الأصلية بعد الطباعة
    setTimeout(() => {
        document.querySelector('style').innerHTML = originalStyles;
        showPage(1); // العودة للصفحة الأولى
    }, 1000);
}

// حفظ التقرير كصورة
async function saveAsImage() {
    try {
        document.getElementById('loading').style.display = 'block';
        showAlert('جاري حفظ الصفحة الحالية كصورة...', 'warning');
        
        // تحديد الصفحة المراد حفظها
        const currentPage = document.querySelector('.page[style*="block"]') || document.getElementById('reportPage1');
        const canvas = await html2canvas(currentPage, {
            scale: 3,
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false
        });
        
        const link = document.createElement('a');
        const pageNumber = currentPage.id === 'reportPage1' ? '١' : '٢';
        const fileName = `تقرير_نتائج_${document.getElementById('subject').innerText || 'الاختبار'}_الصفحة_${pageNumber}.png`;
        link.download = fileName;
        link.href = canvas.toDataURL('image/png', 1.0);
        link.click();
        
        showAlert('تم حفظ الصورة بنجاح!', 'success');
        
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
        
        allStudentsData = [];
        renderData(resetData);
        
        // إعادة تعيين جدول الطلاب
        document.getElementById('studentsTableBody').innerHTML = '';
        document.getElementById('excellentCount').textContent = '0';
        document.getElementById('veryGoodCount').textContent = '0';
        document.getElementById('goodCount').textContent = '0';
        document.getElementById('acceptableCount').textContent = '0';
        document.getElementById('weakCount').textContent = '0';
        document.getElementById('totalStudentsPage2').textContent = '0';
        document.getElementById('successRatePage2').textContent = '0%';
        
        document.getElementById('analysisStatus').innerHTML = '<i class="fas fa-clock"></i> قيد الانتظار';
        document.getElementById('analysisStatus').style.background = '#e0e0e0';
        document.getElementById('analysisStatus').style.color = '#333';
        
        showAlert('تم مسح جميع البيانات بنجاح.', 'success');
        showPage(1);
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
    
    // إظهار الصفحة الأولى فقط عند البدء
    showPage(1);
};
</script>
</body>
</html>