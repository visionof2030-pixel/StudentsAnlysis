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
    padding: 14mm;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
    border-radius: 4px;
    position: relative;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
    margin-bottom: 20px;
    padding-bottom: 15px;
    border-bottom: 2px solid #1a5c9e;
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
    font-size: 18px;
    font-weight: bold;
    color: #1a5c9e;
    padding: 8px 16px;
    border: 2px solid #1a5c9e;
    border-radius: 8px;
    background: linear-gradient(135deg, #f0f7ff 0%, #e3eeff 100%);
}

.title {
    text-align: center;
    font-size: 24px;
    font-weight: bold;
    margin: 25px 0;
    padding: 20px;
    background: linear-gradient(135deg, #1a5c9e 0%, #2a6cb0 100%);
    color: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(26, 92, 158, 0.2);
    position: relative;
    overflow: hidden;
}

.title::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 4px;
    background: linear-gradient(90deg, #25d366, #4caf50, #2196f3);
}

.info-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 12px;
    margin-bottom: 25px;
}

.info-box {
    border: 1px solid #ddd;
    border-radius: 10px;
    padding: 15px;
    text-align: center;
    font-size: 14px;
    background: #f8fafc;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
}

.info-box:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
    border-color: #1a5c9e;
}

.info-box::after {
    content: "";
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: linear-gradient(90deg, #25d366, #1a5c9e);
}

.info-box span {
    display: block;
    font-weight: bold;
    margin-top: 8px;
    font-size: 16px;
    color: #1a5c9e;
}

.analysis-section {
    border: 1px solid #ddd;
    border-radius: 12px;
    padding: 20px;
    font-size: 14px;
    margin-bottom: 25px;
    background: #f8fafc;
    line-height: 1.8;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.05);
}

.analysis-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
    padding-bottom: 10px;
    border-bottom: 2px solid #e0e0e0;
}

.section-title {
    font-size: 18px;
    font-weight: bold;
    color: #1a5c9e;
    margin: 20px 0 15px;
    padding-right: 10px;
    border-right: 4px solid #25d366;
}

.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 15px;
    margin-bottom: 25px;
}

.stat-box {
    border: 1px solid #ddd;
    border-radius: 10px;
    padding: 20px;
    text-align: center;
    font-size: 14px;
    background: white;
    transition: all 0.3s ease;
}

.stat-box:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
}

.stat-box strong {
    display: block;
    font-size: 28px;
    margin-top: 10px;
    color: #1a5c9e;
    font-weight: bold;
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
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
    margin-bottom: 30px;
}

.chart-box {
    border: 1px solid #ddd;
    border-radius: 12px;
    padding: 20px;
    background: white;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
}

.chart-box:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
}

.chart-box canvas {
    width: 100% !important;
    height: 200px !important;
}

.chart-title {
    text-align: center;
    font-weight: bold;
    margin-bottom: 15px;
    color: #1a5c9e;
    font-size: 16px;
}

.footer {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    font-size: 14px;
    margin-top: 40px;
    padding-top: 20px;
    border-top: 2px solid #e0e0e0;
}

.footer div {
    text-align: center;
    flex: 1;
    padding: 0 15px;
}

.footer strong {
    display: block;
    margin-bottom: 5px;
    color: #1a5c9e;
    font-size: 15px;
}

.signature-line {
    width: 150px;
    height: 1px;
    background: #333;
    margin: 10px auto;
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
    font-size: 80px;
    color: rgba(26, 92, 158, 0.05);
    font-weight: bold;
    pointer-events: none;
    z-index: 1;
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
        padding: 10mm;
    }
}

@media (max-width: 1200px) {
    .page {
        width: 100%;
        margin: 10px auto;
    }
}

@media (max-width: 768px) {
    .header {
        flex-direction: column;
        gap: 10px;
    }
    
    .header div {
        text-align: center !important;
    }
    
    .charts-container {
        grid-template-columns: 1fr;
    }
    
    .footer {
        flex-direction: column;
        gap: 20px;
        text-align: center;
    }
    
    .input-field {
        min-width: 100%;
    }
    
    .btn {
        min-width: 100%;
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
    <div class="watermark">نموذج التقرير</div>
    
    <div class="header">
        <div>
            <strong>وزارة التعليم</strong><br>
            الإدارة العامة للتعليم<br>
            منطقة الرياض التعليمية
        </div>
        <div class="logo">مدرسة النخبة النموذجية</div>
        <div>
            <strong>الاختبار النهائي</strong><br>
            الفصل الدراسي الثاني<br>
            1446 هـ / 2024 م
        </div>
    </div>

    <div class="title">
        <i class="fas fa-chart-bar"></i> تقرير تحليل نتائج الاختبار
    </div>

    <div class="info-grid">
        <div class="info-box">
            <i class="fas fa-book" style="color: #1a5c9e; margin-bottom: 5px;"></i>
            المادة
            <span id="subject">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-graduation-cap" style="color: #1a5c9e; margin-bottom: 5px;"></i>
            الصف
            <span id="grade">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-calendar-alt" style="color: #1a5c9e; margin-bottom: 5px;"></i>
            الفصل
            <span id="term">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-star" style="color: #1a5c9e; margin-bottom: 5px;"></i>
            الدرجة الكاملة
            <span id="maxScore">-</span>
        </div>
        <div class="info-box">
            <i class="fas fa-clock" style="color: #1a5c9e; margin-bottom: 5px;"></i>
            تاريخ الاختبار
            <span id="examDate">-</span>
        </div>
    </div>

    <div class="section-title">
        <i class="fas fa-analysis"></i> التحليل العام للنتائج
    </div>
    
    <div class="analysis-section">
        <div class="analysis-header">
            <strong>ملخص التحليل</strong>
            <span class="badge" id="analysisStatus" style="background: #e0e0e0; padding: 5px 10px; border-radius: 20px; font-size: 12px;">قيد الانتظار</span>
        </div>
        <div id="analysisText" style="text-align: justify; line-height: 1.8;">
            سيظهر هنا التحليل الذكي للنتائج بعد رفع ملف PDF وإدخال مفتاح API.
        </div>
    </div>

    <div class="section-title">
        <i class="fas fa-chart-pie"></i> الإحصائيات الرئيسية
    </div>

    <div class="stats-grid">
        <div class="stat-box info">
            <i class="fas fa-users" style="font-size: 24px; color: #2196f3; margin-bottom: 10px;"></i>
            عدد الطلاب
            <strong id="studentsCount">-</strong>
        </div>
        <div class="stat-box success">
            <i class="fas fa-calculator" style="font-size: 24px; color: #4caf50; margin-bottom: 10px;"></i>
            المتوسط
            <strong id="averageScore">-</strong>
        </div>
        <div class="stat-box">
            <i class="fas fa-trophy" style="font-size: 24px; color: #ff9800; margin-bottom: 10px;"></i>
            أعلى درجة
            <strong id="maxStudentScore">-</strong>
        </div>
        <div class="stat-box warning">
            <i class="fas fa-exclamation-triangle" style="font-size: 24px; color: #ff9800; margin-bottom: 10px;"></i>
            أدنى درجة
            <strong id="minStudentScore">-</strong>
        </div>
        <div class="stat-box success">
            <i class="fas fa-percentage" style="font-size: 24px; color: #4caf50; margin-bottom: 10px;"></i>
            نسبة النجاح
            <strong id="successRate">-</strong>
        </div>
    </div>

    <div class="section-title">
        <i class="fas fa-chart-area"></i> الرسوم البيانية
    </div>

    <div class="charts-container">
        <div class="chart-box">
            <div class="chart-title">المتوسط العام</div>
            <canvas id="avgChart"></canvas>
        </div>
        <div class="chart-box">
            <div class="chart-title">نسبة النجاح</div>
            <canvas id="lowChart"></canvas>
        </div>
        <div class="chart-box">
            <div class="chart-title">توزيع التقديرات</div>
            <canvas id="pieChart"></canvas>
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
let avgChart, lowChart, pieChart;

// تهيئة المخططات البيانية
function initCharts(gradesDist) {
    // تدمير المخططات القديمة إن وجدت
    if (avgChart) avgChart.destroy();
    if (lowChart) lowChart.destroy();
    if (pieChart) pieChart.destroy();
    
    // مخطط المتوسط
    const avgCtx = document.getElementById('avgChart').getContext('2d');
    avgChart = new Chart(avgCtx, {
        type: 'bar',
        data: {
            labels: ['المتوسط'],
            datasets: [{
                label: 'درجة',
                data: [gradesDist.average],
                backgroundColor: '#1a5c9e',
                borderColor: '#154a80',
                borderWidth: 2,
                borderRadius: 8,
                barPercentage: 0.5
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                y: {
                    beginAtZero: true,
                    max: gradesDist.maxScore || 100,
                    grid: {
                        color: 'rgba(0,0,0,0.05)'
                    },
                    ticks: {
                        font: {
                            size: 12
                        }
                    }
                },
                x: {
                    grid: {
                        display: false
                    },
                    ticks: {
                        font: {
                            size: 14,
                            weight: 'bold'
                        }
                    }
                }
            },
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return `المتوسط: ${context.raw}`;
                        }
                    }
                }
            }
        }
    });
    
    // مخطط نسبة النجاح
    const lowCtx = document.getElementById('lowChart').getContext('2d');
    lowChart = new Chart(lowCtx, {
        type: 'doughnut',
        data: {
            labels: ['نسبة النجاح', 'نسبة الرسوب'],
            datasets: [{
                data: [gradesDist.success, 100 - gradesDist.success],
                backgroundColor: ['#4caf50', '#f44336'],
                borderColor: ['#388e3c', '#d32f2f'],
                borderWidth: 2,
                hoverOffset: 10
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '65%',
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: {
                        font: {
                            size: 12
                        },
                        padding: 15
                    }
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return `${context.label}: ${context.raw}%`;
                        }
                    }
                }
            }
        }
    });
    
    // مخطط التوزيع
    const pieCtx = document.getElementById('pieChart').getContext('2d');
    pieChart = new Chart(pieCtx, {
        type: 'pie',
        data: {
            labels: ['ممتاز', 'جيد جدًا', 'جيد', 'مقبول', 'ضعيف'],
            datasets: [{
                data: gradesDist.levels,
                backgroundColor: [
                    '#4caf50',
                    '#009688',
                    '#2196f3',
                    '#ff9800',
                    '#f44336'
                ],
                borderColor: [
                    '#388e3c',
                    '#00796b',
                    '#1976d2',
                    '#f57c00',
                    '#d32f2f'
                ],
                borderWidth: 2,
                hoverOffset: 15
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: {
                        font: {
                            size: 11
                        },
                        padding: 10
                    }
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            const label = context.label;
                            const value = context.raw;
                            const total = context.dataset.data.reduce((a, b) => a + b, 0);
                            const percentage = Math.round((value / total) * 100);
                            return `${label}: ${value} طالب (${percentage}%)`;
                        }
                    }
                }
            }
        }
    });
}

// معالجة ملف PDF
async function handlePDF(file) {
    if (!file) {
        showAlert('الرجاء اختيار ملف PDF أولاً', 'error');
        return;
    }
    
    // عرض مؤشر التحميل
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
                }
                
                // تحليل النص باستخدام Gemini API
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
        // استخدام بيانات تجريبية إذا لم يتم إدخال مفتاح API
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
${text.substring(0, 5000)}`; // تحديد حجم النص المرسل
        
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
        
        // استخراج JSON من النص
        const responseText = data.candidates[0].content.parts[0].text;
        const jsonMatch = responseText.match(/\{[\s\S]*\}/);
        
        if (!jsonMatch) {
            throw new Error('لم يتم العثور على JSON في الاستجابة');
        }
        
        const jsonData = JSON.parse(jsonMatch[0]);
        
        // عرض البيانات
        renderData(jsonData);
        
        showAlert('تم تحليل البيانات بنجاح!', 'success');
        
    } catch (error) {
        console.error('خطأ في تحليل البيانات:', error);
        showAlert(`خطأ في التحليل: ${error.message}. يتم استخدام بيانات تجريبية.`, 'error');
        
        // استخدام بيانات تجريبية في حالة الفشل
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
        analysis: "أظهرت نتائج الاختبار تحسناً ملحوظاً في مستوى الطلاب مقارنة بالاختبار السابق. لوحظ تركيز الضعف في الأسئلة المتعلقة بالتفاضل والتكامل، بينما أبدى الطلاب تفوقاً في الأسئلة الجبرية. نوصي بتكثيف التدريبات العملية في مواضيع التفاضل وحل المسائل التطبيقية.",
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
    document.getElementById('analysisStatus').style.background = '#d4edda';
    document.getElementById('analysisStatus').style.color = '#155724';
    
    // تحديث الإحصائيات
    document.getElementById('studentsCount').innerText = data.stats.count;
    document.getElementById('averageScore').innerText = data.stats.average.toFixed(1);
    document.getElementById('maxStudentScore').innerText = data.stats.max;
    document.getElementById('minStudentScore').innerText = data.stats.min;
    document.getElementById('successRate').innerText = data.stats.success.toFixed(1) + '%';
    
    // إنشاء المخططات
    initCharts({
        average: data.stats.average,
        success: data.stats.success,
        levels: data.levels,
        maxScore: parseInt(data.max_score)
    });
}

// تصدير ومشاركة التقرير
async function exportAndShare() {
    try {
        document.getElementById('loading').style.display = 'block';
        showAlert('جاري إنشاء ملف PDF...', 'warning');
        
        // إنشاء صورة من التقرير
        const canvas = await html2canvas(document.getElementById('report'), {
            scale: 2,
            backgroundColor: '#ffffff',
            useCORS: true,
            logging: false
        });
        
        const imgData = canvas.toDataURL('image/jpeg', 1.0);
        
        // إنشاء PDF
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF('p', 'mm', 'a4');
        const imgWidth = 210;
        const imgHeight = canvas.height * imgWidth / canvas.width;
        
        pdf.addImage(imgData, 'JPEG', 0, 0, imgWidth, imgHeight);
        
        // حفظ PDF
        const pdfBlob = pdf.output('blob');
        const fileName = `تقرير_نتائج_${document.getElementById('subject').innerText}_${new Date().toISOString().slice(0,10)}.pdf`;
        const pdfFile = new File([pdfBlob], fileName, { type: 'application/pdf' });
        
        // مشاركة عبر واتساب أو حفظ
        if (navigator.share && navigator.canShare && navigator.canShare({ files: [pdfFile] })) {
            await navigator.share({
                files: [pdfFile],
                title: 'تقرير تحليل النتائج',
                text: `تقرير تحليل نتائج ${document.getElementById('subject').innerText}`
            });
            showAlert('تم مشاركة التقرير بنجاح!', 'success');
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
    window.print();
}

// حفظ التقرير كصورة
async function saveAsImage() {
    try {
        document.getElementById('loading').style.display = 'block';
        showAlert('جاري حفظ الصورة...', 'warning');
        
        const canvas = await html2canvas(document.getElementById('report'), {
            scale: 2,
            backgroundColor: '#ffffff',
            useCORS: true
        });
        
        const link = document.createElement('a');
        link.download = `تقرير_نتائج_${document.getElementById('subject').innerText}_${new Date().toISOString().slice(0,10)}.png`;
        link.href = canvas.toDataURL('image/png');
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
    
    // إخفاء الرسالة بعد 5 ثواني
    setTimeout(() => {
        alertDiv.style.display = 'none';
    }, 5000);
}

// إعادة تعيين النموذج
function resetForm() {
    if (confirm('هل أنت متأكد من رغبتك في مسح جميع البيانات؟')) {
        document.getElementById('apiKey').value = '';
        document.getElementById('pdfFile').value = '';
        
        // إعادة تعيين التقرير إلى حالته الأصلية
        const resetData = {
            subject: "-",
            grade: "-",
            term: "-",
            max_score: "-",
            exam_date: "-",
            analysis: "سيظهر هنا التحليل الذكي للنتائج بعد رفع ملف PDF وإدخال مفتاح API.",
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
        document.getElementById('analysisStatus').innerHTML = 'قيد الانتظار';
        document.getElementById('analysisStatus').style.background = '#e0e0e0';
        document.getElementById('analysisStatus').style.color = '#333';
        
        showAlert('تم مسح جميع البيانات بنجاح.', 'success');
    }
}

// تهيئة المخططات فارغة عند التحميل
window.onload = function() {
    initCharts({
        average: 0,
        success: 0,
        levels: [0, 0, 0, 0, 0],
        maxScore: 100
    });
};
</script>
</body>
</html>