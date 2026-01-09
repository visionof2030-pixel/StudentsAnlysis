<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>نظام تحليل نتائج الاختبارات</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

<!-- المكتبات الخارجية -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>

<style>
@page {
    size: A4;
    margin: 0;
}

* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    -webkit-tap-highlight-color: transparent;
    -webkit-touch-callout: none;
}

html, body {
    height: 100%;
    overflow-x: hidden;
}

body {
    font-family: 'Tahoma', 'Arial', 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
    margin: 0;
    padding: 12px;
    min-height: 100vh;
    -webkit-text-size-adjust: 100%;
    touch-action: manipulation;
}

.app-container {
    display: flex;
    flex-direction: column;
    height: calc(100vh - 24px);
    max-width: 100%;
    overflow: hidden;
}

/* شريط التنقل العلوي */
.top-nav {
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
    padding: 12px;
    position: sticky;
    top: 0;
    z-index: 1000;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.nav-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
}

.app-title {
    font-size: 16px;
    font-weight: bold;
    color: #1a5c9e;
    display: flex;
    align-items: center;
    gap: 8px;
    flex: 1;
}

.menu-toggle {
    background: #25d366;
    color: white;
    border: none;
    width: 40px;
    height: 40px;
    border-radius: 8px;
    font-size: 18px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
}

.menu-toggle:hover {
    background: #1da851;
}

.input-group {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
}

.input-field {
    position: relative;
    width: 100%;
}

.input-field input {
    width: 100%;
    padding: 12px 40px 12px 15px;
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    font-size: 14px;
    transition: all 0.3s ease;
    background: #f8f9fa;
    -webkit-appearance: none;
}

.input-field input:focus {
    outline: none;
    border-color: #25d366;
    box-shadow: 0 0 0 3px rgba(37, 211, 102, 0.1);
    background: white;
}

.file-input-container {
    position: relative;
    width: 100%;
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
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    width: 100%;
    padding: 12px;
    background: #2196f3;
    color: white;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s ease;
    font-size: 14px;
}

.file-label:hover {
    background: #1976d2;
}

/* القائمة الجانبية */
.sidebar {
    position: fixed;
    top: 0;
    right: -300px;
    width: 280px;
    height: 100%;
    background: white;
    box-shadow: -5px 0 15px rgba(0, 0, 0, 0.1);
    z-index: 2000;
    transition: right 0.3s ease;
    display: flex;
    flex-direction: column;
    overflow-y: auto;
    padding-top: 60px;
}

.sidebar.active {
    right: 0;
}

.sidebar-header {
    padding: 20px;
    background: #1a5c9e;
    color: white;
    font-weight: bold;
    font-size: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
    position: fixed;
    top: 0;
    right: 0;
    width: 280px;
    z-index: 2001;
    height: 60px;
}

.close-sidebar {
    background: none;
    border: none;
    color: white;
    font-size: 20px;
    cursor: pointer;
    margin-right: auto;
}

.sidebar-content {
    padding: 20px;
    flex: 1;
    overflow-y: auto;
    margin-top: 60px;
}

.sidebar-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 15px;
    margin-bottom: 10px;
    background: #f8f9fa;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
    border: 1px solid #e0e0e0;
}

.sidebar-item:hover {
    background: #e3f2fd;
    transform: translateX(-5px);
}

.sidebar-item.active {
    background: #1a5c9e;
    color: white;
    border-color: #1a5c9e;
}

/* المحتوى الرئيسي */
.main-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 12px;
    overflow: hidden;
}

.content-section {
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    display: none;
    flex: 1;
    flex-direction: column;
}

.content-section.active {
    display: flex;
}

.section-header {
    padding: 15px;
    background: linear-gradient(135deg, #1a5c9e 0%, #2a6cb0 100%);
    color: white;
    font-weight: bold;
    font-size: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
    position: sticky;
    top: 0;
    z-index: 10;
}

.section-content {
    flex: 1;
    overflow-y: auto;
    padding: 15px;
    -webkit-overflow-scrolling: touch;
}

/* صفحة التقرير */
.page {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    max-width: 100%;
    overflow-x: hidden;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    margin-bottom: 20px;
    padding-bottom: 15px;
    border-bottom: 2px solid #1a5c9e;
    flex-wrap: wrap;
    gap: 10px;
}

.header div {
    flex: 1;
    min-width: 120px;
    text-align: center;
}

.logo {
    font-size: 14px;
    font-weight: bold;
    color: #1a5c9e;
    padding: 8px 12px;
    border: 1px solid #1a5c9e;
    border-radius: 8px;
    background: #f0f7ff;
}

.title {
    text-align: center;
    font-size: 18px;
    font-weight: bold;
    margin: 20px 0;
    padding: 15px;
    background: linear-gradient(135deg, #1a5c9e 0%, #2a6cb0 100%);
    color: white;
    border-radius: 10px;
}

.info-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 10px;
    margin-bottom: 20px;
}

.info-box {
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    padding: 12px;
    text-align: center;
    font-size: 12px;
    background: #f8fafc;
}

.info-box span {
    display: block;
    font-weight: bold;
    margin-top: 5px;
    font-size: 14px;
    color: #1a5c9e;
}

.analysis-section {
    border: 1px solid #e0e0e0;
    border-radius: 10px;
    padding: 15px;
    font-size: 13px;
    margin-bottom: 20px;
    background: #f8fafc;
    line-height: 1.6;
}

.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
    gap: 12px;
    margin-bottom: 20px;
}

.stat-box {
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
    font-size: 12px;
    background: white;
}

.stat-box strong {
    display: block;
    font-size: 18px;
    margin-top: 8px;
    color: #1a5c9e;
}

.charts-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 15px;
    margin-bottom: 20px;
}

.chart-box {
    border: 1px solid #e0e0e0;
    border-radius: 10px;
    padding: 15px;
    background: white;
    height: 250px;
}

.chart-container {
    width: 100%;
    height: 200px;
}

.footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    margin-top: 20px;
    padding-top: 15px;
    border-top: 1px solid #e0e0e0;
    flex-wrap: wrap;
    gap: 15px;
}

.footer div {
    text-align: center;
    flex: 1;
    min-width: 100px;
}

/* صفحة قائمة الطلاب */
.students-container {
    display: flex;
    flex-direction: column;
    height: 100%;
}

.students-header {
    padding: 15px;
    background: #f8f9fa;
    border-bottom: 1px solid #e0e0e0;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.students-filters {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.search-box {
    flex: 1;
    min-width: 200px;
    position: relative;
}

.search-box input {
    width: 100%;
    padding: 10px 35px 10px 15px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 14px;
}

.search-icon {
    position: absolute;
    left: 10px;
    top: 50%;
    transform: translateY(-50%);
    color: #666;
}

.filter-select {
    padding: 10px 15px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 14px;
    min-width: 150px;
    background: white;
}

.students-list-container {
    flex: 1;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
}

.students-table {
    width: 100%;
    border-collapse: collapse;
}

.students-table thead {
    position: sticky;
    top: 0;
    background: #1a5c9e;
    color: white;
    z-index: 10;
}

.students-table th {
    padding: 12px 10px;
    font-size: 13px;
    font-weight: bold;
    text-align: center;
    border: none;
}

.students-table tbody tr {
    border-bottom: 1px solid #e0e0e0;
    transition: background 0.2s;
}

.students-table tbody tr:hover {
    background: #f5f7fa;
}

.students-table td {
    padding: 12px 10px;
    text-align: center;
    font-size: 13px;
    border: none;
}

.grade-badge {
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 11px;
    font-weight: bold;
    display: inline-block;
}

.grade-excellent { background: #4caf50; color: white; }
.grade-very-good { background: #009688; color: white; }
.grade-good { background: #2196f3; color: white; }
.grade-acceptable { background: #ff9800; color: white; }
.grade-weak { background: #f44336; color: white; }

.students-pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    padding: 15px;
    background: #f8f9fa;
    border-top: 1px solid #e0e0e0;
}

.pagination-btn {
    background: #1a5c9e;
    color: white;
    border: none;
    width: 40px;
    height: 40px;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
}

.pagination-btn:disabled {
    background: #cccccc;
    cursor: not-allowed;
}

.pagination-info {
    font-size: 14px;
    font-weight: bold;
    color: #333;
}

/* أزرار التحكم */
.controls-container {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    padding: 12px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    position: sticky;
    bottom: 0;
    z-index: 1000;
}

.btn {
    flex: 1;
    min-width: 120px;
    background: #25d366;
    color: white;
    border: none;
    padding: 12px;
    font-size: 14px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    -webkit-appearance: none;
}

.btn:hover {
    background: #1da851;
    transform: translateY(-2px);
}

.btn:active {
    transform: translateY(0);
}

.btn-secondary {
    background: #1a5c9e;
}

.btn-secondary:hover {
    background: #154a80;
}

.btn-excel {
    background: #4caf50;
}

.btn-excel:hover {
    background: #388e3c;
}

.btn-danger {
    background: #f44336;
}

.btn-danger:hover {
    background: #d32f2f;
}

/* رسائل التنبيه */
.alert {
    display: none;
    padding: 12px;
    margin: 10px 0;
    border-radius: 8px;
    text-align: center;
    font-weight: bold;
    font-size: 14px;
    animation: slideIn 0.3s ease;
}

@keyframes slideIn {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
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

/* مؤشر التحميل */
.loading {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(255, 255, 255, 0.9);
    z-index: 3000;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 15px;
}

.spinner {
    width: 50px;
    height: 50px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #25d366;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* نموذج إدخال البيانات يدوياً */
.data-input-container {
    background: white;
    border-radius: 12px;
    padding: 20px;
    margin: 10px 0;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.input-section-title {
    font-size: 16px;
    font-weight: bold;
    color: #1a5c9e;
    margin-bottom: 15px;
    padding-bottom: 10px;
    border-bottom: 2px solid #25d366;
}

.data-input-form {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.form-row {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.form-group {
    flex: 1;
    min-width: 200px;
}

.form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
    color: #333;
    font-size: 14px;
}

.form-group input,
.form-group select {
    width: 100%;
    padding: 10px 15px;
    border: 2px solid #e0e0e0;
    border-radius: 8px;
    font-size: 14px;
    transition: all 0.3s ease;
    background: #f8f9fa;
}

.form-group input:focus,
.form-group select:focus {
    outline: none;
    border-color: #25d366;
    background: white;
}

.form-actions {
    display: flex;
    gap: 10px;
    margin-top: 10px;
}

.btn-small {
    padding: 8px 16px;
    font-size: 13px;
    min-width: auto;
}

.manual-data-input {
    margin-top: 20px;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    overflow: hidden;
}

.manual-data-input textarea {
    width: 100%;
    min-height: 200px;
    padding: 15px;
    border: none;
    font-family: 'Courier New', monospace;
    font-size: 14px;
    line-height: 1.6;
    resize: vertical;
    background: #f8f9fa;
}

.manual-data-input textarea:focus {
    outline: none;
    background: white;
}

.data-input-help {
    background: #e3f2fd;
    border-radius: 8px;
    padding: 15px;
    margin-top: 15px;
    font-size: 13px;
    color: #1565c0;
}

.data-input-help ul {
    margin-right: 20px;
    margin-top: 10px;
}

.data-input-help li {
    margin-bottom: 8px;
}

/* تأثيرات الهاتف */
@media (hover: none) and (pointer: coarse) {
    .btn:hover {
        transform: none;
    }
    
    .sidebar-item:hover {
        transform: none;
    }
    
    .students-table tbody tr:hover {
        background: inherit;
    }
    
    .menu-toggle:active,
    .btn:active,
    .sidebar-item:active {
        opacity: 0.7;
        transform: scale(0.98);
    }
}

/* تصميم متجاوب */
@media (max-width: 768px) {
    .app-container {
        height: calc(100vh - 24px);
    }
    
    .top-nav {
        padding: 10px;
    }
    
    .app-title {
        font-size: 14px;
    }
    
    .section-header {
        font-size: 14px;
        padding: 12px;
    }
    
    .info-grid {
        grid-template-columns: repeat(2, 1fr);
    }
    
    .stats-grid {
        grid-template-columns: repeat(2, 1fr);
    }
    
    .charts-container {
        grid-template-columns: 1fr;
    }
    
    .chart-box {
        height: 220px;
    }
    
    .footer {
        font-size: 11px;
    }
    
    .students-filters {
        flex-direction: column;
    }
    
    .search-box,
    .filter-select {
        min-width: 100%;
    }
    
    .controls-container {
        flex-direction: column;
    }
    
    .btn {
        width: 100%;
    }
    
    .students-table th,
    .students-table td {
        padding: 8px 6px;
        font-size: 12px;
    }
    
    .grade-badge {
        font-size: 10px;
        padding: 3px 6px;
    }
    
    .form-row {
        flex-direction: column;
    }
    
    .form-group {
        min-width: 100%;
    }
}

@media (max-width: 480px) {
    body {
        padding: 8px;
    }
    
    .app-container {
        height: calc(100vh - 16px);
    }
    
    .info-grid,
    .stats-grid {
        grid-template-columns: 1fr;
    }
    
    .header {
        flex-direction: column;
        text-align: center;
        gap: 10px;
    }
    
    .header div {
        min-width: 100%;
    }
    
    .title {
        font-size: 16px;
        padding: 12px;
    }
    
    .page {
        padding: 15px;
    }
    
    .sidebar {
        width: 100%;
        right: -100%;
    }
    
    .sidebar.active {
        right: 0;
    }
    
    .sidebar-header {
        width: 100%;
    }
}

/* تحسينات للآيفون */
@supports (-webkit-touch-callout: none) {
    .app-container {
        height: -webkit-fill-available;
    }
    
    .section-content {
        padding-bottom: 80px;
    }
    
    .controls-container {
        padding-bottom: env(safe-area-inset-bottom, 12px);
    }
    
    input, select, button, textarea {
        font-size: 16px;
    }
}

/* تحسينات للأندرويد */
@media (-webkit-device-pixel-ratio: 2) {
    .btn, .file-label, .sidebar-item {
        min-height: 44px;
    }
    
    input, select, textarea {
        min-height: 44px;
    }
}

/* إخفاء شريط التمرير ولكن مع السماح بالتمرير */
.students-list-container::-webkit-scrollbar,
.section-content::-webkit-scrollbar {
    width: 5px;
}

.students-list-container::-webkit-scrollbar-track,
.section-content::-webkit-scrollbar-track {
    background: #f1f1f1;
}

.students-list-container::-webkit-scrollbar-thumb,
.section-content::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 10px;
}

.students-list-container::-webkit-scrollbar-thumb:hover,
.section-content::-webkit-scrollbar-thumb:hover {
    background: #555;
}

/* تنسيق لشاشات صغيرة جدا */
@media (max-width: 360px) {
    .app-title {
        font-size: 13px;
    }
    
    .btn {
        font-size: 13px;
        padding: 10px;
    }
    
    .section-header {
        font-size: 13px;
        padding: 10px;
    }
}
</style>

<!-- أيقونات Font Awesome -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>

<body>

<div class="app-container">
    <!-- شريط التنقل العلوي -->
    <div class="top-nav">
        <div class="nav-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="app-title">
                <i class="fas fa-chart-line"></i>
                نظام تحليل نتائج الاختبارات
            </div>
        </div>
        
        <div class="input-group">
            <div class="input-field">
                <i class="fas fa-key" style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); color: #666;"></i>
                <input type="text" id="apiKey" placeholder="مفتاح Gemini API (اختياري)">
            </div>
        </div>
        
        <div class="alert" id="alertMessage"></div>
    </div>
    
    <!-- القائمة الجانبية -->
    <div class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <button class="close-sidebar" onclick="toggleSidebar()">
                <i class="fas fa-times"></i>
            </button>
            <span>القائمة الرئيسية</span>
        </div>
        <div class="sidebar-content">
            <div class="sidebar-item active" onclick="showSection('report-section')">
                <i class="fas fa-chart-bar"></i>
                <span>التقرير الإحصائي</span>
            </div>
            <div class="sidebar-item" onclick="showSection('students-section')">
                <i class="fas fa-users"></i>
                <span>قائمة الطلاب</span>
            </div>
            <div class="sidebar-item" onclick="showSection('input-section')">
                <i class="fas fa-edit"></i>
                <span>إدخال البيانات</span>
            </div>
            <div class="sidebar-item" onclick="exportExcel()">
                <i class="fas fa-file-excel"></i>
                <span>تصدير Excel</span>
            </div>
            <div class="sidebar-item" onclick="exportPDF()">
                <i class="fas fa-file-pdf"></i>
                <span>تصدير PDF</span>
            </div>
            <div class="sidebar-item" onclick="shareViaWhatsApp()">
                <i class="fab fa-whatsapp"></i>
                <span>مشاركة عبر واتساب</span>
            </div>
            <div class="sidebar-item" onclick="resetForm()">
                <i class="fas fa-redo"></i>
                <span>مسح البيانات</span>
            </div>
        </div>
    </div>
    
    <!-- المحتوى الرئيسي -->
    <div class="main-content">
        <!-- قسم إدخال البيانات -->
        <div class="content-section active" id="input-section">
            <div class="section-header">
                <i class="fas fa-edit"></i>
                إدخال بيانات الطلاب
            </div>
            <div class="section-content">
                <div class="data-input-container">
                    <div class="input-section-title">
                        <i class="fas fa-info-circle"></i>
                        معلومات الاختبار
                    </div>
                    
                    <div class="data-input-form">
                        <div class="form-row">
                            <div class="form-group">
                                <label for="inputSubject"><i class="fas fa-book"></i> المادة</label>
                                <input type="text" id="inputSubject" placeholder="مثال: الرياضيات">
                            </div>
                            <div class="form-group">
                                <label for="inputGrade"><i class="fas fa-graduation-cap"></i> الصف</label>
                                <input type="text" id="inputGrade" placeholder="مثال: الثاني الثانوي">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="inputTerm"><i class="fas fa-calendar-alt"></i> الفصل</label>
                                <select id="inputTerm">
                                    <option value="">اختر الفصل</option>
                                    <option value="الأول">الأول</option>
                                    <option value="الثاني">الثاني</option>
                                    <option value="الصيفي">الصيفي</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="inputMaxScore"><i class="fas fa-star"></i> الدرجة الكاملة</label>
                                <input type="number" id="inputMaxScore" value="100" min="1" max="1000">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label for="inputExamDate"><i class="fas fa-clock"></i> تاريخ الاختبار</label>
                                <input type="text" id="inputExamDate" placeholder="مثال: 15/06/1446 هـ">
                            </div>
                        </div>
                        
                        <div class="form-actions">
                            <button class="btn btn-secondary btn-small" onclick="loadSampleData()">
                                <i class="fas fa-download"></i> تحميل بيانات تجريبية
                            </button>
                            <button class="btn btn-small" onclick="analyzeManualData()">
                                <i class="fas fa-chart-bar"></i> توليد التقرير
                            </button>
                        </div>
                    </div>
                    
                    <div class="input-section-title" style="margin-top: 30px;">
                        <i class="fas fa-users"></i>
                        بيانات الطلاب
                    </div>
                    
                    <div class="data-input-help">
                        <p><strong>طريقة إدخال البيانات:</strong></p>
                        <ul>
                            <li>أدخل بيانات الطلاب في الجدول أدناه</li>
                            <li>استخدم الصيغة: الاسم, الدرجة</li>
                            <li>مثال: أحمد محمد, 85</li>
                            <li>لكل طالب سطر جديد</li>
                            <li>يمكنك نسخ البيانات من Excel مباشرة</li>
                        </ul>
                    </div>
                    
                    <div class="manual-data-input">
                        <textarea id="studentsDataInput" placeholder="أدخل بيانات الطلاب هنا (اسم الطالب, الدرجة)...&#10;مثال:&#10;أحمد محمد, 95&#10;محمد عبدالله, 88&#10;سعيد علي, 76"></textarea>
                    </div>
                    
                    <div class="form-actions" style="margin-top: 20px;">
                        <button class="btn btn-excel" onclick="parseExcelData()">
                            <i class="fas fa-file-import"></i> استيراد من Excel
                        </button>
                        <button class="btn" onclick="analyzeManualData()">
                            <i class="fas fa-magic"></i> تحليل البيانات
                        </button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- قسم التقرير -->
        <div class="content-section" id="report-section">
            <div class="section-header">
                <i class="fas fa-chart-bar"></i>
                التقرير الإحصائي
            </div>
            <div class="section-content">
                <div class="page" id="report">
                    <div class="header">
                        <div>
                            <strong>وزارة التعليم</strong><br>
                            الإدارة العامة للتعليم
                        </div>
                        <div class="logo">
                            <i class="fas fa-school"></i> مدرسة النخبة
                        </div>
                        <div>
                            <strong>الاختبار النهائي</strong><br>
                            الفصل الدراسي الثاني
                        </div>
                    </div>

                    <div class="title">
                        <i class="fas fa-chart-bar"></i> تقرير تحليل نتائج الاختبار
                    </div>

                    <div class="info-grid">
                        <div class="info-box">
                            <i class="fas fa-book"></i>
                            المادة
                            <span id="subject">-</span>
                        </div>
                        <div class="info-box">
                            <i class="fas fa-graduation-cap"></i>
                            الصف
                            <span id="grade">-</span>
                        </div>
                        <div class="info-box">
                            <i class="fas fa-calendar-alt"></i>
                            الفصل
                            <span id="term">-</span>
                        </div>
                        <div class="info-box">
                            <i class="fas fa-star"></i>
                            الدرجة الكاملة
                            <span id="maxScore">-</span>
                        </div>
                        <div class="info-box">
                            <i class="fas fa-clock"></i>
                            تاريخ الاختبار
                            <span id="examDate">-</span>
                        </div>
                    </div>

                    <div class="analysis-section">
                        <strong>التحليل العام للنتائج:</strong><br>
                        <div id="analysisText">
                            سيظهر هنا التحليل الذكي للنتائج بعد إدخال البيانات.
                        </div>
                    </div>

                    <div class="stats-grid">
                        <div class="stat-box">
                            <i class="fas fa-users"></i>
                            عدد الطلاب
                            <strong id="studentsCount">-</strong>
                        </div>
                        <div class="stat-box">
                            <i class="fas fa-calculator"></i>
                            المتوسط
                            <strong id="averageScore">-</strong>
                        </div>
                        <div class="stat-box">
                            <i class="fas fa-trophy"></i>
                            أعلى درجة
                            <strong id="maxStudentScore">-</strong>
                        </div>
                        <div class="stat-box">
                            <i class="fas fa-exclamation-triangle"></i>
                            أدنى درجة
                            <strong id="minStudentScore">-</strong>
                        </div>
                        <div class="stat-box">
                            <i class="fas fa-percentage"></i>
                            نسبة النجاح
                            <strong id="successRate">-</strong>
                        </div>
                    </div>

                    <div class="charts-container">
                        <div class="chart-box">
                            <div style="font-weight: bold; margin-bottom: 10px; text-align: center; color: #1a5c9e;">
                                توزيع الدرجات
                            </div>
                            <div class="chart-container">
                                <canvas id="distributionChart"></canvas>
                            </div>
                        </div>
                        <div class="chart-box">
                            <div style="font-weight: bold; margin-bottom: 10px; text-align: center; color: #1a5c9e;">
                                نسبة النجاح
                            </div>
                            <div class="chart-container">
                                <canvas id="successChart"></canvas>
                            </div>
                        </div>
                    </div>

                    <div class="footer">
                        <div>
                            <strong>مدير المدرسة</strong><br>
                            د. عبدالله الشمراني
                        </div>
                        <div>
                            <strong>رئيس القسم</strong><br>
                            أ. خالد العتيبي
                        </div>
                        <div>
                            <strong>معلم المادة</strong><br>
                            أ. سعيد الغامدي
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- قسم قائمة الطلاب -->
        <div class="content-section" id="students-section">
            <div class="section-header">
                <i class="fas fa-users"></i>
                قائمة الطلاب والدرجات
            </div>
            <div class="section-content">
                <div class="students-container">
                    <div class="students-header">
                        <div class="students-filters">
                            <div class="search-box">
                                <i class="fas fa-search search-icon"></i>
                                <input type="text" id="searchInput" placeholder="ابحث عن طالب..." oninput="filterStudents()">
                            </div>
                            <select class="filter-select" id="gradeFilter" onchange="filterStudents()">
                                <option value="">جميع التقديرات</option>
                                <option value="ممتاز">ممتاز</option>
                                <option value="جيد جدًا">جيد جدًا</option>
                                <option value="جيد">جيد</option>
                                <option value="مقبول">مقبول</option>
                                <option value="ضعيف">ضعيف</option>
                            </select>
                            <select class="filter-select" id="sortSelect" onchange="sortStudents()">
                                <option value="name">ترتيب حسب الاسم</option>
                                <option value="score-desc">الدرجة (من الأعلى)</option>
                                <option value="score-asc">الدرجة (من الأدنى)</option>
                                <option value="grade">التقدير</option>
                            </select>
                        </div>
                    </div>
                    <div class="students-list-container">
                        <table class="students-table">
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
                    <div class="students-pagination">
                        <button class="pagination-btn" onclick="prevPage()" id="prevBtn" disabled>
                            <i class="fas fa-chevron-right"></i>
                        </button>
                        <span class="pagination-info" id="pageInfo">الصفحة 1 من 1</span>
                        <button class="pagination-btn" onclick="nextPage()" id="nextBtn" disabled>
                            <i class="fas fa-chevron-left"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- أزرار التحكم -->
    <div class="controls-container">
        <button class="btn btn-excel" onclick="exportExcel()">
            <i class="fas fa-file-excel"></i> Excel
        </button>
        <button class="btn btn-secondary" onclick="exportPDF()">
            <i class="fas fa-file-pdf"></i> PDF
        </button>
        <button class="btn" onclick="shareViaWhatsApp()">
            <i class="fab fa-whatsapp"></i> واتساب
        </button>
    </div>
</div>

<!-- مؤشر التحميل -->
<div class="loading" id="loading">
    <div class="spinner"></div>
    <p style="font-weight: bold; color: #1a5c9e; margin-top: 10px;">جاري معالجة البيانات...</p>
</div>

<!-- مدخل ملف Excel -->
<input type="file" id="excelFileInput" accept=".xlsx,.xls,.csv" style="display: none;" onchange="handleExcelFile(this.files[0])">

<script>
// البيانات العالمية
let studentsData = [];
let filteredStudents = [];
let currentPage = 1;
const studentsPerPage = 10;
let distributionChart = null;
let successChart = null;
let currentSort = 'name';

// إدارة القائمة الجانبية
function toggleSidebar() {
    const sidebar = document.getElementById('sidebar');
    sidebar.classList.toggle('active');
}

// إدارة الأقسام
function showSection(sectionId) {
    // إخفاء جميع الأقسام
    document.querySelectorAll('.content-section').forEach(section => {
        section.classList.remove('active');
    });
    
    // إخفاء جميع عناصر القائمة الجانبية
    document.querySelectorAll('.sidebar-item').forEach(item => {
        item.classList.remove('active');
    });
    
    // إظهار القسم المطلوب
    document.getElementById(sectionId).classList.add('active');
    
    // تفعيل العنصر المحدد في القائمة الجانبية
    const menuItems = document.querySelectorAll('.sidebar-item');
    switch(sectionId) {
        case 'report-section':
            menuItems[0].classList.add('active');
            break;
        case 'students-section':
            menuItems[1].classList.add('active');
            break;
        case 'input-section':
            menuItems[2].classList.add('active');
            break;
    }
    
    // إغلاق القائمة الجانبية على الهواتف
    if (window.innerWidth <= 768) {
        toggleSidebar();
    }
}

// تحليل البيانات المدخلة يدوياً
function analyzeManualData() {
    const subject = document.getElementById('inputSubject').value.trim();
    const grade = document.getElementById('inputGrade').value.trim();
    const term = document.getElementById('inputTerm').value.trim();
    const maxScore = parseInt(document.getElementById('inputMaxScore').value) || 100;
    const examDate = document.getElementById('inputExamDate').value.trim();
    const studentsInput = document.getElementById('studentsDataInput').value.trim();
    
    // التحقق من البيانات الأساسية
    if (!subject || !grade || !term || !examDate) {
        showAlert('الرجاء إدخال جميع معلومات الاختبار', 'error');
        return;
    }
    
    if (!studentsInput) {
        showAlert('الرجاء إدخال بيانات الطلاب', 'error');
        return;
    }
    
    showLoading(true);
    
    try {
        // تحليل بيانات الطلاب المدخلة
        const students = parseStudentsInput(studentsInput, maxScore);
        
        if (students.length === 0) {
            showAlert('لم يتم العثور على بيانات طلاب صحيحة', 'error');
            showLoading(false);
            return;
        }
        
        // حساب الإحصائيات
        const stats = calculateStatistics(students, maxScore);
        
        // حساب توزيع التقديرات
        const gradeLevels = calculateGradeLevels(students);
        
        // إنشاء تحليل نصي
        const analysis = generateAnalysis(stats, gradeLevels, students.length);
        
        // تجميع البيانات
        const reportData = {
            subject,
            grade,
            term,
            max_score: maxScore.toString(),
            exam_date: examDate,
            analysis,
            students,
            levels: gradeLevels,
            stats
        };
        
        // عرض البيانات
        renderData(reportData);
        
        showAlert(`تم تحليل بيانات ${students.length} طالب بنجاح!`, 'success');
        
        // الانتقال إلى قسم التقرير
        setTimeout(() => showSection('report-section'), 500);
        
    } catch (error) {
        console.error('خطأ في تحليل البيانات:', error);
        showAlert('حدث خطأ أثناء تحليل البيانات', 'error');
    } finally {
        showLoading(false);
    }
}

// تحليل بيانات الطلاب من النص المدخل
function parseStudentsInput(input, maxScore) {
    const students = [];
    const lines = input.split('\n');
    
    for (let line of lines) {
        line = line.trim();
        if (!line) continue;
        
        // محاولة تحليل الصيغ المختلفة
        let name, score;
        
        // الصيغة: اسم, درجة
        if (line.includes(',')) {
            const parts = line.split(',');
            name = parts[0].trim();
            score = parseFloat(parts[1].trim());
        }
        // الصيغة: اسم    درجة (TAB أو مسافات متعددة)
        else if (line.includes('\t') || line.match(/\s{2,}/)) {
            const parts = line.split(/\t|\s{2,}/);
            name = parts[0].trim();
            score = parseFloat(parts[1].trim());
        }
        // الصيغة: اسم درجة (مسافة واحدة)
        else {
            const lastSpaceIndex = line.lastIndexOf(' ');
            if (lastSpaceIndex > 0) {
                name = line.substring(0, lastSpaceIndex).trim();
                score = parseFloat(line.substring(lastSpaceIndex + 1).trim());
            }
        }
        
        // التحقق من صحة البيانات
        if (name && !isNaN(score) && score >= 0 && score <= maxScore) {
            const percentage = Math.round((score / maxScore) * 100);
            const grade = calculateGrade(percentage);
            
            students.push({
                name,
                score: Math.round(score),
                percentage,
                grade
            });
        }
    }
    
    return students;
}

// حساب التقدير بناء على النسبة
function calculateGrade(percentage) {
    if (percentage >= 90) return 'ممتاز';
    if (percentage >= 80) return 'جيد جدًا';
    if (percentage >= 70) return 'جيد';
    if (percentage >= 60) return 'مقبول';
    return 'ضعيف';
}

// حساب الإحصائيات
function calculateStatistics(students, maxScore) {
    if (students.length === 0) {
        return {
            count: 0,
            average: 0,
            max: 0,
            min: 0,
            success: 0
        };
    }
    
    const scores = students.map(s => s.score);
    const average = scores.reduce((a, b) => a + b, 0) / scores.length;
    const max = Math.max(...scores);
    const min = Math.min(...scores);
    
    // حساب نسبة النجاح (الطلاب بنسبة 60% فأكثر)
    const passingCount = students.filter(s => s.percentage >= 60).length;
    const successRate = (passingCount / students.length) * 100;
    
    return {
        count: students.length,
        average: Math.round(average * 10) / 10,
        max,
        min,
        success: Math.round(successRate * 10) / 10
    };
}

// حساب توزيع التقديرات
function calculateGradeLevels(students) {
    const levels = [0, 0, 0, 0, 0]; // ممتاز، جيد جدًا، جيد، مقبول، ضعيف
    
    students.forEach(student => {
        switch(student.grade) {
            case 'ممتاز': levels[0]++; break;
            case 'جيد جدًا': levels[1]++; break;
            case 'جيد': levels[2]++; break;
            case 'مقبول': levels[3]++; break;
            case 'ضعيف': levels[4]++; break;
        }
    });
    
    return levels;
}

// إنشاء تحليل نصي
function generateAnalysis(stats, gradeLevels, totalStudents) {
    const excellentCount = gradeLevels[0];
    const weakCount = gradeLevels[4];
    
    let analysis = `عدد الطلاب: ${stats.count} طالب\n`;
    analysis += `المتوسط العام: ${stats.average}\n`;
    analysis += `نسبة النجاح: ${stats.success}%\n\n`;
    
    if (excellentCount > 0) {
        const excellentPercent = Math.round((excellentCount / totalStudents) * 100);
        analysis += `حقق ${excellentCount} طالب (${excellentPercent}%) تقدير ممتاز.\n`;
    }
    
    if (weakCount > 0) {
        const weakPercent = Math.round((weakCount / totalStudents) * 100);
        analysis += `يحتاج ${weakCount} طالب (${weakPercent}%) إلى تحسين مستواهم.\n`;
    }
    
    if (stats.success >= 80) {
        analysis += `\nالأداء العام: ممتاز - نسبة النجاح مرتفعة تشير إلى فهم جيد للمادة.`;
    } else if (stats.success >= 60) {
        analysis += `\nالأداء العام: جيد - يحتاج بعض الطلاب إلى دعم إضافي.`;
    } else {
        analysis += `\nالأداء العام: يحتاج تحسين - يوصى بمراجعة طريقة التدريس والتقييم.`;
    }
    
    return analysis;
}

// تحميل بيانات تجريبية
function loadSampleData() {
    const sampleData = `أحمد محمد, 95
محمد عبدالله, 88
سعيد علي, 76
خالد إبراهيم, 82
عبدالرحمن سعد, 68
فهد ناصر, 91
تركي سليمان, 79
يوسف حمد, 85
عمر صالح, 73
بدر عبدالعزيز, 62
سلطان راشد, 55
نواف مطلق, 89
وليد فيصل, 94
ماجد حامد, 71
هاني عبدالمحسن, 77`;

    document.getElementById('inputSubject').value = 'الرياضيات';
    document.getElementById('inputGrade').value = 'الثاني الثانوي';
    document.getElementById('inputTerm').value = 'الثاني';
    document.getElementById('inputMaxScore').value = '100';
    document.getElementById('inputExamDate').value = '15/06/1446 هـ';
    document.getElementById('studentsDataInput').value = sampleData;
    
    showAlert('تم تحميل البيانات التجريبية', 'success');
}

// استيراد بيانات من Excel
function parseExcelData() {
    document.getElementById('excelFileInput').click();
}

// معالجة ملف Excel
function handleExcelFile(file) {
    if (!file) return;
    
    showLoading(true);
    
    const reader = new FileReader();
    
    reader.onload = function(e) {
        try {
            const data = new Uint8Array(e.target.result);
            const workbook = XLSX.read(data, { type: 'array' });
            const firstSheet = workbook.Sheets[workbook.SheetNames[0]];
            const jsonData = XLSX.utils.sheet_to_json(firstSheet, { header: 1 });
            
            // تحويل البيانات إلى النص المطلوب
            let studentsText = '';
            jsonData.forEach(row => {
                if (row.length >= 2 && row[0] && !isNaN(row[1])) {
                    studentsText += `${row[0]}, ${row[1]}\n`;
                }
            });
            
            if (studentsText) {
                document.getElementById('studentsDataInput').value = studentsText;
                showAlert(`تم استيراد ${jsonData.length - 1} سجل بنجاح`, 'success');
            } else {
                showAlert('لم يتم العثور على بيانات صحيحة في الملف', 'warning');
            }
            
        } catch (error) {
            console.error('خطأ في قراءة ملف Excel:', error);
            showAlert('حدث خطأ أثناء قراءة ملف Excel', 'error');
        } finally {
            showLoading(false);
        }
    };
    
    reader.onerror = function() {
        showAlert('حدث خطأ أثناء قراءة الملف', 'error');
        showLoading(false);
    };
    
    reader.readAsArrayBuffer(file);
}

// تحليل البيانات باستخدام Gemini API (اختياري)
async function analyzeWithGemini() {
    const apiKey = document.getElementById('apiKey').value.trim();
    
    if (!apiKey) {
        showAlert('الرجاء إدخال مفتاح Gemini API', 'warning');
        return;
    }
    
    if (studentsData.length === 0) {
        showAlert('لا توجد بيانات طلاب للتحليل', 'warning');
        return;
    }
    
    showLoading(true);
    
    try {
        // تحضير البيانات للنموذج
        const dataForAI = {
            subject: document.getElementById('subject').innerText,
            grade: document.getElementById('grade').innerText,
            stats: {
                count: studentsData.length,
                average: parseFloat(document.getElementById('averageScore').innerText),
                success: parseFloat(document.getElementById('successRate').innerText)
            },
            students: studentsData.slice(0, 10) // إرسال عينة فقط
        };
        
        const prompt = `أنت محلل تربوي محترف. قم بتحليل نتائج الطلاب التالية وقدم تقريراً تحليلياً متكاملاً:
        
        المادة: ${dataForAI.subject}
        الصف: ${dataForAI.grade}
        عدد الطلاب: ${dataForAI.stats.count}
        المتوسط: ${dataForAI.stats.average}
        نسبة النجاح: ${dataForAI.stats.success}%
        
        عينة من الطلاب:
        ${dataForAI.students.map(s => `${s.name}: ${s.score} (${s.grade})`).join('\n')}
        
        أرجو تقديم تحليل شامل يتضمن:
        1. تقييم عام للأداء
        2. نقاط القوة والضعف
        3. توصيات للتحسين
        4. مقترحات للطلاب الضعاف
        5. اقتراحات لتعزيز التفوق`;
        
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
        const analysis = data.candidates[0].content.parts[0].text;
        
        // تحديث التحليل
        document.getElementById('analysisText').innerHTML = analysis.replace(/\n/g, '<br>');
        showAlert('تم تحديث التحليل باستخدام الذكاء الاصطناعي', 'success');
        
    } catch (error) {
        console.error('خطأ في تحليل البيانات:', error);
        showAlert('حدث خطأ أثناء تحليل البيانات باستخدام الذكاء الاصطناعي', 'error');
    } finally {
        showLoading(false);
    }
}

// عرض البيانات في التقرير
function renderData(data) {
    // حفظ البيانات العالمية
    studentsData = data.students || [];
    
    // تحديث المعلومات العامة
    document.getElementById('subject').innerText = data.subject;
    document.getElementById('grade').innerText = data.grade;
    document.getElementById('term').innerText = data.term;
    document.getElementById('maxScore').innerText = data.max_score;
    document.getElementById('examDate').innerText = data.exam_date;
    
    // تحديث التحليل النصي
    document.getElementById('analysisText').innerHTML = data.analysis.replace(/\n/g, '<br>');
    
    // تحديث الإحصائيات
    document.getElementById('studentsCount').innerText = data.stats.count;
    document.getElementById('averageScore').innerText = data.stats.average.toFixed(1);
    document.getElementById('maxStudentScore').innerText = data.stats.max;
    document.getElementById('minStudentScore').innerText = data.stats.min;
    document.getElementById('successRate').innerText = data.stats.success.toFixed(1) + '%';
    
    // إنشاء المخططات
    createCharts(data);
    
    // عرض قائمة الطلاب
    displayStudents();
    
    // الانتقال تلقائياً إلى قسم الطلاب
    setTimeout(() => showSection('students-section'), 500);
}

// إنشاء المخططات
function createCharts(data) {
    const maxScore = parseInt(data.max_score) || 100;
    const averageScore = data.stats.average || 0;
    const successRate = data.stats.success || 0;
    
    // تدمير المخططات القديمة إن وجدت
    if (distributionChart) distributionChart.destroy();
    if (successChart) successChart.destroy();
    
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
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                y: {
                    beginAtZero: true,
                    max: maxScore,
                    grid: {
                        display: true
                    }
                },
                x: {
                    grid: {
                        display: false
                    }
                }
            },
            plugins: {
                legend: {
                    display: false
                }
            }
        }
    });
    
    // مخطط نسبة النجاح
    const successCtx = document.getElementById('successChart').getContext('2d');
    successChart = new Chart(successCtx, {
        type: 'doughnut',
        data: {
            labels: ['نسبة النجاح', 'نسبة الرسوب'],
            datasets: [{
                data: [successRate, 100 - successRate],
                backgroundColor: [
                    'rgba(76, 175, 80, 0.8)',
                    'rgba(244, 67, 54, 0.8)'
                ],
                borderColor: [
                    '#4caf50',
                    '#f44336'
                ],
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '60%',
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: {
                        font: {
                            size: 12
                        }
                    }
                }
            }
        }
    });
}

// عرض قائمة الطلاب مع التصفح
function displayStudents() {
    const searchTerm = document.getElementById('searchInput').value.toLowerCase();
    const gradeFilter = document.getElementById('gradeFilter').value;
    
    // تصفية الطلاب
    filteredStudents = studentsData.filter(student => {
        const matchesSearch = student.name.toLowerCase().includes(searchTerm);
        const matchesGrade = !gradeFilter || student.grade === gradeFilter;
        return matchesSearch && matchesGrade;
    });
    
    // ترتيب الطلاب
    sortFilteredStudents();
    
    // حساب عدد الصفحات
    const totalPages = Math.ceil(filteredStudents.length / studentsPerPage);
    
    // تحديث أزرار التصفح
    document.getElementById('prevBtn').disabled = currentPage <= 1;
    document.getElementById('nextBtn').disabled = currentPage >= totalPages || totalPages === 0;
    document.getElementById('pageInfo').textContent = totalPages > 0 ? `الصفحة ${currentPage} من ${totalPages}` : 'لا توجد بيانات';
    
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
                <td colspan="5" style="text-align: center; padding: 40px; color: #666;">
                    <i class="fas fa-users" style="font-size: 40px; margin-bottom: 10px; display: block; opacity: 0.5;"></i>
                    ${studentsData.length === 0 ? 'لا توجد بيانات للعرض' : 'لم يتم العثور على نتائج'}
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
            <td><span class="grade-badge ${gradeClass}">${student.grade}</span></td>
        `;
        tableBody.appendChild(row);
    });
}

// ترتيب الطلاب
function sortFilteredStudents() {
    switch(currentSort) {
        case 'name':
            filteredStudents.sort((a, b) => a.name.localeCompare(b.name, 'ar'));
            break;
        case 'score-desc':
            filteredStudents.sort((a, b) => b.score - a.score);
            break;
        case 'score-asc':
            filteredStudents.sort((a, b) => a.score - b.score);
            break;
        case 'grade':
            const gradeOrder = {'ممتاز': 0, 'جيد جدًا': 1, 'جيد': 2, 'مقبول': 3, 'ضعيف': 4};
            filteredStudents.sort((a, b) => gradeOrder[a.grade] - gradeOrder[b.grade]);
            break;
    }
}

// تغيير الترتيب
function sortStudents() {
    currentSort = document.getElementById('sortSelect').value;
    currentPage = 1;
    displayStudents();
}

// الحصول على كلاس CSS للتقدير
function getGradeClass(grade) {
    switch(grade) {
        case 'ممتاز': return 'grade-excellent';
        case 'جيد جدًا': return 'grade-very-good';
        case 'جيد': return 'grade-good';
        case 'مقبول': return 'grade-acceptable';
        case 'ضعيف': return 'grade-weak';
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
        scrollToTop();
    }
}

// الصفحة السابقة
function prevPage() {
    if (currentPage > 1) {
        currentPage--;
        displayStudents();
        scrollToTop();
    }
}

// التمرير إلى أعلى
function scrollToTop() {
    const container = document.querySelector('.students-list-container');
    if (container) {
        container.scrollTop = 0;
    }
}

// تصدير إلى Excel
function exportExcel() {
    if (studentsData.length === 0) {
        showAlert('لا توجد بيانات للتصدير', 'warning');
        return;
    }
    
    showLoading(true);
    
    try {
        // تحضير البيانات
        const excelData = [
            ['تقرير نتائج الطلاب'],
            ['المادة:', document.getElementById('subject').innerText],
            ['الصف:', document.getElementById('grade').innerText],
            ['الفصل:', document.getElementById('term').innerText],
            ['تاريخ الاختبار:', document.getElementById('examDate').innerText],
            ['الدرجة الكاملة:', document.getElementById('maxScore').innerText],
            [],
            ['م', 'اسم الطالب', 'الدرجة', 'النسبة المئوية', 'التقدير']
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
        
        // إضافة سطور فارغة
        excelData.push([]);
        excelData.push(['الإحصائيات', '', '', '', '']);
        excelData.push(['عدد الطلاب:', document.getElementById('studentsCount').innerText]);
        excelData.push(['المتوسط:', document.getElementById('averageScore').innerText]);
        excelData.push(['أعلى درجة:', document.getElementById('maxStudentScore').innerText]);
        excelData.push(['أدنى درجة:', document.getElementById('minStudentScore').innerText]);
        excelData.push(['نسبة النجاح:', document.getElementById('successRate').innerText]);
        
        // إضافة التحليل
        excelData.push([]);
        excelData.push(['التحليل العام:']);
        const analysisLines = document.getElementById('analysisText').innerText.split('\n');
        analysisLines.forEach(line => {
            excelData.push([line]);
        });
        
        // إنشاء ورقة عمل
        const ws = XLSX.utils.aoa_to_sheet(excelData);
        
        // تنسيق الأعمدة
        const wscols = [
            {wch: 5},   // العمود الأول
            {wch: 30},  // العمود الثاني
            {wch: 10},  // العمود الثالث
            {wch: 15},  // العمود الرابع
            {wch: 12}   // العمود الخامس
        ];
        ws['!cols'] = wscols;
        
        // دمج الخلايا للعناوين
        ws['!merges'] = [
            {s: {r: 0, c: 0}, e: {r: 0, c: 4}}, // عنوان التقرير
            {s: {r: 1, c: 1}, e: {r: 1, c: 4}}, // المادة
            {s: {r: 2, c: 1}, e: {r: 2, c: 4}}, // الصف
            {s: {r: 3, c: 1}, e: {r: 3, c: 4}}, // الفصل
            {s: {r: 4, c: 1}, e: {r: 4, c: 4}}, // التاريخ
            {s: {r: 5, c: 1}, e: {r: 5, c: 4}}, // الدرجة الكاملة
        ];
        
        // إنشاء مصنف
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, 'نتائج الطلاب');
        
        // اسم الملف
        const subject = document.getElementById('subject').innerText || 'الاختبار';
        const grade = document.getElementById('grade').innerText || '';
        const fileName = `نتائج_${subject}_${grade}_${new Date().toISOString().slice(0,10)}.xlsx`;
        
        // حفظ الملف
        XLSX.writeFile(wb, fileName);
        
        showAlert('تم تصدير البيانات إلى Excel بنجاح!', 'success');
        
    } catch (error) {
        console.error('خطأ في تصدير Excel:', error);
        showAlert('حدث خطأ أثناء تصدير Excel', 'error');
    } finally {
        showLoading(false);
    }
}

// مشاركة عبر واتساب (Excel)
async function shareViaWhatsApp() {
    if (studentsData.length === 0) {
        showAlert('لا توجد بيانات للمشاركة', 'warning');
        return;
    }
    
    showLoading(true);
    
    try {
        // تحضير البيانات
        const excelData = [
            ['م', 'اسم الطالب', 'الدرجة', 'النسبة', 'التقدير']
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
        
        // إنشاء ورقة عمل
        const ws = XLSX.utils.aoa_to_sheet(excelData);
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, 'النتائج');
        
        // تحويل إلى ArrayBuffer
        const excelArray = XLSX.write(wb, { bookType: 'xlsx', type: 'array' });
        const blob = new Blob([excelArray], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
        
        const subject = document.getElementById('subject').innerText || 'الاختبار';
        const fileName = `نتائج_${subject}.xlsx`;
        const file = new File([blob], fileName, { type: blob.type });
        
        // محاولة المشاركة عبر واتساب
        if (navigator.share && navigator.canShare && navigator.canShare({ files: [file] })) {
            try {
                await navigator.share({
                    files: [file],
                    title: 'نتائج الطلاب',
                    text: `نتائج ${subject} - ${document.getElementById('grade').innerText || ''}`
                });
                showAlert('تم مشاركة الملف عبر واتساب بنجاح!', 'success');
            } catch (shareError) {
                console.log('المشاركة فشلت، يتم التنزيل بدلاً من ذلك:', shareError);
                // تنزيل الملف إذا فشلت المشاركة
                XLSX.writeFile(wb, fileName);
                showAlert('تم تنزيل ملف Excel بنجاح!', 'success');
            }
        } else {
            // تنزيل الملف مباشرة إذا لم يكن المشاركة مدعومة
            XLSX.writeFile(wb, fileName);
            showAlert('تم تنزيل ملف Excel بنجاح!', 'success');
        }
        
    } catch (error) {
        console.error('خطأ في المشاركة:', error);
        showAlert('حدث خطأ أثناء مشاركة الملف', 'error');
    } finally {
        showLoading(false);
    }
}

// تصدير PDF
async function exportPDF() {
    if (studentsData.length === 0) {
        showAlert('لا توجد بيانات للتصدير', 'warning');
        return;
    }
    
    showLoading(true);
    
    try {
        const reportElement = document.getElementById('report');
        
        // إخفاء عناصر غير ضرورية أثناء التصدير
        const originalDisplay = reportElement.style.display;
        reportElement.style.display = 'block';
        
        const canvas = await html2canvas(reportElement, {
            scale: 2,
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false
        });
        
        reportElement.style.display = originalDisplay;
        
        const imgData = canvas.toDataURL('image/jpeg', 1.0);
        
        // إنشاء PDF
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF('p', 'mm', 'a4');
        
        const imgWidth = 210;
        const imgHeight = canvas.height * imgWidth / canvas.width;
        
        pdf.addImage(imgData, 'JPEG', 0, 0, imgWidth, imgHeight);
        
        // حفظ الملف
        const subject = document.getElementById('subject').innerText || 'الاختبار';
        const fileName = `تقرير_${subject}.pdf`;
        
        if (navigator.share && navigator.canShare) {
            const pdfBlob = pdf.output('blob');
            const pdfFile = new File([pdfBlob], fileName, { type: 'application/pdf' });
            
            try {
                await navigator.share({
                    files: [pdfFile],
                    title: 'تقرير النتائج',
                    text: `تقرير نتائج ${subject}`
                });
                showAlert('تم مشاركة التقرير بنجاح!', 'success');
            } catch (error) {
                pdf.save(fileName);
                showAlert('تم حفظ التقرير كملف PDF!', 'success');
            }
        } else {
            pdf.save(fileName);
            showAlert('تم حفظ التقرير كملف PDF!', 'success');
        }
        
    } catch (error) {
        console.error('خطأ في تصدير PDF:', error);
        showAlert('حدث خطأ أثناء إنشاء PDF', 'error');
    } finally {
        showLoading(false);
    }
}

// إعادة تعيين النموذج
function resetForm() {
    if (confirm('هل أنت متأكد من رغبتك في مسح جميع البيانات؟')) {
        document.getElementById('apiKey').value = '';
        document.getElementById('inputSubject').value = '';
        document.getElementById('inputGrade').value = '';
        document.getElementById('inputTerm').value = '';
        document.getElementById('inputMaxScore').value = '100';
        document.getElementById('inputExamDate').value = '';
        document.getElementById('studentsDataInput').value = '';
        document.getElementById('searchInput').value = '';
        document.getElementById('gradeFilter').value = '';
        document.getElementById('sortSelect').value = 'name';
        
        studentsData = [];
        filteredStudents = [];
        currentPage = 1;
        currentSort = 'name';
        
        // إعادة تعيين التقرير
        const resetData = {
            subject: "-",
            grade: "-",
            term: "-",
            max_score: "-",
            exam_date: "-",
            analysis: "سيظهر هنا التحليل الذكي للنتائج بعد إدخال البيانات.",
            students: [],
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
        showSection('input-section');
        showAlert('تم مسح جميع البيانات بنجاح.', 'success');
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
    // تحميل بيانات تجريبية عند البدء
    loadSampleData();
    
    // إغلاق القائمة الجانبية عند النقر خارجها
    document.addEventListener('click', function(event) {
        const sidebar = document.getElementById('sidebar');
        const menuToggle = document.querySelector('.menu-toggle');
        
        if (sidebar.classList.contains('active') && 
            !sidebar.contains(event.target) && 
            !menuToggle.contains(event.target)) {
            sidebar.classList.remove('active');
        }
    });
    
    // تحسين الأداء على الهواتف
    if ('ontouchstart' in window) {
        document.body.classList.add('touch-device');
    }
    
    // تهيئة مخططات فارغة
    const initialData = {
        subject: "-",
        grade: "-",
        term: "-",
        max_score: "100",
        exam_date: "-",
        analysis: "",
        students: [],
        levels: [0, 0, 0, 0, 0],
        stats: {
            count: 0,
            average: 0,
            max: 0,
            min: 0,
            success: 0
        }
    };
    
    createCharts(initialData);
};

// دعم اللمس للأزرار
document.querySelectorAll('.btn, .sidebar-item, .menu-toggle').forEach(button => {
    button.addEventListener('touchstart', function() {
        this.style.opacity = '0.7';
    });
    
    button.addEventListener('touchend', function() {
        this.style.opacity = '1';
    });
});
</script>
</body>
</html>