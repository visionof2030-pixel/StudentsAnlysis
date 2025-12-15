
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام تحليل نتائج الطلاب - النسخة المحسنة</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jspdf-autotable@3.5.28/dist/jspdf.plugin.autotable.min.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #0b3c5d;
            --primary-light: #0f528c;
            --secondary: #1d6fa5;
            --accent: #2a9d8f;
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text: #2d3748;
            --text-light: #718096;
            --good: #2e7d32;
            --good-light: #4caf50;
            --mid: #f9a825;
            --mid-light: #ffb74d;
            --bad: #c62828;
            --bad-light: #ef5350;
            --border: #e2e8f0;
            --shadow: rgba(0, 0, 0, 0.08);
            --shadow-hover: rgba(0, 0, 0, 0.12);
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Cairo', 'Segoe UI', Tahoma, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            line-height: 1.6;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 1.5rem;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
        }
        
        header::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
            background-image: url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M11 18c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm48 25c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm-43-7c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm63 31c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM34 90c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm56-76c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM12 86c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm28-65c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm23-11c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-6 60c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm29 22c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zM32 63c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm57-13c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-9-21c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM60 91c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM35 41c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM12 60c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2z' fill='%23ffffff' fill-opacity='0.05' fill-rule='evenodd'/%3E%3C/svg%3E");
            opacity: 0.3;
        }
        
        header h1 {
            font-size: 2.2rem;
            margin-bottom: 0.5rem;
            position: relative;
            z-index: 1;
        }
        
        header p {
            font-size: 1.1rem;
            opacity: 0.9;
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        
        .header-icon {
            margin-left: 0.5rem;
            font-size: 1.8rem;
            vertical-align: middle;
        }
        
        nav {
            display: flex;
            background: var(--card-bg);
            border-bottom: 3px solid var(--primary);
            box-shadow: 0 2px 8px var(--shadow);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        nav button {
            flex: 1;
            border: none;
            padding: 1rem;
            background: none;
            font-weight: 600;
            cursor: pointer;
            color: var(--text);
            font-size: 1rem;
            transition: all 0.3s ease;
            position: relative;
        }
        
        nav button:hover {
            background: rgba(11, 60, 93, 0.05);
        }
        
        nav button.active {
            color: var(--primary);
            background: rgba(11, 60, 93, 0.08);
        }
        
        nav button.active::after {
            content: '';
            position: absolute;
            bottom: -3px;
            right: 0;
            width: 100%;
            height: 3px;
            background: var(--primary);
        }
        
        .nav-icon {
            margin-left: 0.5rem;
        }
        
        main {
            padding: 1.5rem;
            max-width: 1400px;
            margin: 0 auto;
        }
        
        section {
            display: none;
            animation: fadeIn 0.5s ease;
        }
        
        section.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            box-shadow: 0 4px 12px var(--shadow);
            border: 1px solid var(--border);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .card:hover {
            box-shadow: 0 6px 16px var(--shadow-hover);
            transform: translateY(-2px);
        }
        
        .card-header {
            display: flex;
            align-items: center;
            margin-bottom: 1.5rem;
            padding-bottom: 0.75rem;
            border-bottom: 2px solid var(--primary);
        }
        
        .card-icon {
            background: var(--primary);
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-left: 0.75rem;
            font-size: 1.2rem;
        }
        
        h2, h3 {
            color: var(--primary);
            margin: 0;
        }
        
        h2 {
            font-size: 1.6rem;
        }
        
        h3 {
            font-size: 1.3rem;
        }
        
        textarea, input {
            width: 100%;
            padding: 0.75rem;
            margin: 0.5rem 0;
            border: 1px solid var(--border);
            border-radius: 8px;
            font-family: 'Cairo', sans-serif;
            font-size: 1rem;
            transition: border 0.3s ease, box-shadow 0.3s ease;
        }
        
        textarea:focus, input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(11, 60, 93, 0.1);
        }
        
        textarea {
            resize: vertical;
            min-height: 120px;
        }
        
        .btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 1rem;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }
        
        .btn:hover {
            background: var(--primary-light);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
        }
        
        .btn i {
            margin-left: 0.5rem;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.95rem;
            margin-top: 1rem;
            overflow: hidden;
            border-radius: 8px;
            box-shadow: 0 2px 8px var(--shadow);
        }
        
        th, td {
            border: 1px solid var(--border);
            padding: 0.75rem;
            text-align: center;
        }
        
        th {
            background: rgba(11, 60, 93, 0.08);
            color: var(--primary);
            font-weight: 700;
        }
        
        tr:hover {
            background: rgba(11, 60, 93, 0.03);
        }
        
        .badge {
            color: white;
            padding: 0.25rem 0.75rem;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            display: inline-block;
        }
        
        .good {
            background: linear-gradient(135deg, var(--good), var(--good-light));
        }
        
        .mid {
            background: linear-gradient(135deg, var(--mid), var(--mid-light));
        }
        
        .bad {
            background: linear-gradient(135deg, var(--bad), var(--bad-light));
        }
        
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-top: 1rem;
        }
        
        .stat-card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 1.5rem;
            text-align: center;
            box-shadow: 0 4px 12px var(--shadow);
            border-top: 5px solid var(--primary);
        }
        
        .stat-value {
            font-size: 2.5rem;
            font-weight: 700;
            color: var(--primary);
            margin: 0.5rem 0;
        }
        
        .stat-label {
            font-size: 1rem;
            color: var(--text-light);
        }
        
        .stat-icon {
            font-size: 2rem;
            color: var(--primary);
            margin-bottom: 0.5rem;
        }
        
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
            gap: 1.5rem;
            margin-top: 1rem;
        }
        
        .chart-card {
            height: 100%;
            min-height: 400px;
            display: flex;
            flex-direction: column;
        }
        
        .chart-container {
            flex: 1;
            position: relative;
            margin-top: 1rem;
        }
        
        canvas {
            max-width: 100%;
        }
        
        .interpretation {
            background: rgba(11, 60, 93, 0.03);
            border-radius: 8px;
            padding: 1.5rem;
            margin-top: 1.5rem;
            border-right: 4px solid var(--accent);
        }
        
        .interpretation h4 {
            color: var(--accent);
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
        }
        
        .interpretation h4 i {
            margin-left: 0.5rem;
        }
        
        .interpretation ul {
            padding-right: 1.5rem;
        }
        
        .interpretation li {
            margin-bottom: 0.75rem;
            line-height: 1.7;
        }
        
        footer {
            background: var(--primary);
            color: white;
            text-align: center;
            padding: 1.5rem;
            margin-top: 2rem;
        }
        
        .footer-content {
            max-width: 800px;
            margin: 0 auto;
        }
        
        .footer-logo {
            font-size: 1.8rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
            color: white;
        }
        
        .footer-logo span {
            color: var(--accent);
        }
        
        .footer-text {
            opacity: 0.8;
            font-size: 0.95rem;
            line-height: 1.6;
        }
        
        /* تحسينات للجوال */
        @media (max-width: 768px) {
            header h1 {
                font-size: 1.8rem;
            }
            
            nav {
                flex-direction: column;
            }
            
            nav button {
                padding: 0.75rem;
                border-bottom: 1px solid var(--border);
            }
            
            nav button.active::after {
                width: 5px;
                right: 0;
                height: 100%;
                bottom: 0;
            }
            
            .charts-container {
                grid-template-columns: 1fr;
            }
            
            table {
                font-size: 0.85rem;
            }
            
            th, td {
                padding: 0.5rem;
            }
            
            .stats-container {
                grid-template-columns: 1fr;
            }
        }
        
        /* تأثيرات للعناصر التفاعلية */
        input[type="number"] {
            text-align: center;
        }
        
        .export-buttons {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
            flex-wrap: wrap;
        }
        
        .export-btn {
            background: var(--accent);
        }
        
        .export-btn:hover {
            background: #238277;
        }
        
        .clear-btn {
            background: var(--bad);
        }
        
        .clear-btn:hover {
            background: #b02424;
        }
        
        .progress-container {
            margin-top: 1.5rem;
        }
        
        .progress-bar {
            height: 10px;
            background: var(--border);
            border-radius: 5px;
            overflow: hidden;
            margin-bottom: 0.5rem;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--bad), var(--mid), var(--good));
            border-radius: 5px;
            transition: width 0.5s ease;
        }
        
        .progress-labels {
            display: flex;
            justify-content: space-between;
            font-size: 0.85rem;
            color: var(--text-light);
        }
        
        .alert {
            padding: 1rem;
            border-radius: 8px;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
        }
        
        .alert-success {
            background: rgba(46, 125, 50, 0.1);
            color: var(--good);
            border-right: 4px solid var(--good);
        }
        
        .alert-warning {
            background: rgba(249, 168, 37, 0.1);
            color: var(--mid);
            border-right: 4px solid var(--mid);
        }
        
        .alert-info {
            background: rgba(11, 60, 93, 0.1);
            color: var(--primary);
            border-right: 4px solid var(--primary);
        }
        
        .alert-error {
            background: rgba(198, 40, 40, 0.1);
            color: var(--bad);
            border-right: 4px solid var(--bad);
        }
        
        .alert-icon {
            margin-left: 0.75rem;
            font-size: 1.2rem;
        }
        
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
        }
        
        .modal-content {
            background-color: var(--card-bg);
            margin: 5% auto;
            padding: 2rem;
            border-radius: 12px;
            width: 90%;
            max-width: 600px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            animation: modalFadeIn 0.3s ease;
        }
        
        @keyframes modalFadeIn {
            from { opacity: 0; transform: translateY(-50px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
            padding-bottom: 1rem;
            border-bottom: 2px solid var(--primary);
        }
        
        .modal-close {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--text-light);
        }
        
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
    </style>
</head>

<body>
    <!-- Modal للتحميل -->
    <div id="loadingModal" class="modal">
        <div class="modal-content" style="text-align: center; padding: 3rem;">
            <div class="loading"></div>
            <h3 style="margin-top: 1rem;">جارٍ التصدير، يرجى الانتظار...</h3>
            <p id="exportProgress"></p>
        </div>
    </div>

    <header>
        <h1><i class="fas fa-chart-line header-icon"></i> نظام تحليل نتائج الطلاب</h1>
        <p>منصة متكاملة للتحليل الإحصائي والتربوي لنتائج الطلاب مع رسوم بيانية تفاعلية ودعم متقدم لاتخاذ القرارات</p>
    </header>

    <nav>
        <button class="active" onclick="showSection('input')">
            <i class="fas fa-edit nav-icon"></i> إدخال البيانات
        </button>
        <button onclick="showSection('analysis')">
            <i class="fas fa-chart-bar nav-icon"></i> التحليل الإحصائي
        </button>
        <button onclick="showSection('charts')">
            <i class="fas fa-chart-pie nav-icon"></i> الرسوم البيانية
        </button>
        <button onclick="showSection('export')">
            <i class="fas fa-download nav-icon"></i> التصدير والتقارير
        </button>
    </nav>

    <main>
        <!-- قسم إدخال البيانات -->
        <section id="input" class="active">
            <div class="card">
                <div class="card-header">
                    <div class="card-icon">
                        <i class="fas fa-user-plus"></i>
                    </div>
                    <h2>إضافة طلاب جديدين</h2>
                </div>
                
                <div class="alert alert-info">
                    <i class="fas fa-info-circle alert-icon"></i>
                    <div>
                        <strong>تعليمات:</strong> قم بإدخال أسماء الطلاب (كل اسم في سطر جديد)، ثم انقر على زر "إضافة الطلاب"
                    </div>
                </div>
                
                <textarea id="names" rows="6" placeholder="أحمد محمد
فاطمة علي
خالد سعيد
سارة عبدالله
..."></textarea>
                
                <div class="alert alert-warning">
                    <i class="fas fa-exclamation-triangle alert-icon"></i>
                    <div>
                        <strong>ملاحظة:</strong> يمكنك تعديل الدرجات مباشرة في الجدول بعد إضافة الطلاب
                    </div>
                </div>
                
                <button class="btn" onclick="addStudents()">
                    <i class="fas fa-plus"></i> إضافة الطلاب
                </button>
            </div>

            <div class="card">
                <div class="card-header">
                    <div class="card-icon">
                        <i class="fas fa-table"></i>
                    </div>
                    <h2>درجات الطلاب</h2>
                </div>
                
                <div class="progress-container">
                    <div class="progress-labels">
                        <span>المتعثرون</span>
                        <span>المتوسطون</span>
                        <span>المتفوقون</span>
                    </div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="progressFill"></div>
                    </div>
                </div>
                
                <div style="overflow-x: auto;">
                    <table>
                        <thead>
                            <tr>
                                <th>الطالب</th>
                                <th>المهام الأدائية (40)</th>
                                <th>الحضور والمشاركة (20)</th>
                                <th>المستمر (60)</th>
                                <th>الاختبار النهائي (40)</th>
                                <th>المجموع (160)</th>
                                <th>النسبة المئوية</th>
                                <th>المستوى</th>
                                <th>الإجراءات</th>
                            </tr>
                        </thead>
                        <tbody id="table"></tbody>
                    </table>
                </div>
                
                <div class="export-buttons">
                    <button class="btn clear-btn" onclick="clearAllData()">
                        <i class="fas fa-trash"></i> مسح جميع البيانات
                    </button>
                    <button class="btn" onclick="addSampleData()">
                        <i class="fas fa-vial"></i> إضافة بيانات تجريبية
                    </button>
                </div>
            </div>
        </section>

        <!-- قسم التحليل الإحصائي -->
        <section id="analysis">
            <div class="card">
                <div class="card-header">
                    <div class="card-icon">
                        <i class="fas fa-chart-bar"></i>
                    </div>
                    <h2>التحليل الإحصائي المتقدم</h2>
                </div>
                
                <div class="alert alert-success">
                    <i class="fas fa-chart-line alert-icon"></i>
                    <div>
                        <strong>معلومات إحصائية:</strong> يتم حساب المؤشرات الإحصائية تلقائياً عند إدخال أو تعديل الدرجات
                    </div>
                </div>
                
                <div class="stats-container">
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="fas fa-calculator"></i>
                        </div>
                        <div class="stat-label">المتوسط الحسابي</div>
                        <div class="stat-value" id="avg">0</div>
                        <div class="stat-label">من 160</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="fas fa-ruler"></i>
                        </div>
                        <div class="stat-label">الانحراف المعياري</div>
                        <div class="stat-value" id="std">0</div>
                        <div class="stat-label">مؤشر لتشتت الدرجات</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="fas fa-arrow-up"></i>
                        </div>
                        <div class="stat-label">أعلى درجة</div>
                        <div class="stat-value" id="max">0</div>
                        <div class="stat-label">من 160</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="fas fa-arrow-down"></i>
                        </div>
                        <div class="stat-label">أقل درجة</div>
                        <div class="stat-value" id="min">0</div>
                        <div class="stat-label">من 160</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="fas fa-medal"></i>
                        </div>
                        <div class="stat-label">عدد المتفوقين</div>
                        <div class="stat-value" id="excellentCount">0</div>
                        <div class="stat-label">(90% فما فوق)</div>
                    </div>
                    
                    <div class="stat-card">
                        <div class="stat-icon">
                            <i class="fas fa-user-graduate"></i>
                        </div>
                        <div class="stat-label">عدد المتعثرين</div>
                        <div class="stat-value" id="weakCount">0</div>
                        <div class="stat-label">(أقل من 60%)</div>
                    </div>
                </div>
                
                <div class="interpretation">
                    <h4><i class="fas fa-lightbulb"></i> تفسير النتائج الإحصائية</h4>
                    <ul>
                        <li><strong>المتوسط الحسابي:</strong> يعبر عن مستوى أداء المجموعة ككل. كلما ارتفعت قيمته، دل ذلك على ارتفاع مستوى الطلاب.</li>
                        <li><strong>الانحراف المعياري:</strong> يقيس مدى تشتت الدرجات حول المتوسط. تشير القيمة المنخفضة إلى تجانس مستوى الطلاب، بينما تشير القيمة المرتفعة إلى تباين كبير في المستويات.</li>
                        <li><strong>عدد المتفوقين والمتعثرين:</strong> يساعد في تحديد نسبة الطلاب الذين يحتاجون إلى دعم إضافي أو تحفيز وتشجيع.</li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- قسم الرسوم البيانية -->
        <section id="charts">
            <div class="card">
                <div class="card-header">
                    <div class="card-icon">
                        <i class="fas fa-chart-area"></i>
                    </div>
                    <h2>التحليل البصري لمكونات الدرجات</h2>
                </div>
                
                <div class="chart-container">
                    <canvas id="barChart"></canvas>
                </div>
                
                <div class="interpretation">
                    <h4><i class="fas fa-chart-bar"></i> تفسير الرسم البياني للأعمدة</h4>
                    <ul>
                        <li>يوضح الرسم البياني توزيع الدرجات لكل طالب حسب المكونات المختلفة (المهام، الحضور، الاختبار النهائي).</li>
                        <li>يساعد في تحديد نقاط القوة والضعف لكل طالب بناءً على أدائه في كل مكون.</li>
                        <li>يمكن مقارنة أداء الطلاب ببعضهم البعض ومعرفة الأنماط السائدة في أداء المجموعة.</li>
                    </ul>
                </div>
            </div>
            
            <div class="charts-container">
                <div class="card chart-card">
                    <div class="card-header">
                        <div class="card-icon">
                            <i class="fas fa-chart-pie"></i>
                        </div>
                        <h3>توزيع مستويات الأداء</h3>
                    </div>
                    
                    <div class="chart-container">
                        <canvas id="pieChart"></canvas>
                    </div>
                </div>
                
                <div class="card chart-card">
                    <div class="card-header">
                        <div class="card-icon">
                            <i class="fas fa-chart-line"></i>
                        </div>
                        <h3>منحنى توزيع الدرجات</h3>
                    </div>
                    
                    <div class="chart-container">
                        <canvas id="lineChart"></canvas>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <div class="interpretation">
                    <h4><i class="fas fa-book-open"></i> تفسير تربوي متكامل</h4>
                    <ul>
                        <li>يعكس <strong>مكون المهام الأدائية</strong> مستوى مهارات <strong>التطبيق والتحليل والتركيب</strong> لدى الطلاب وفق تصنيف بلوم للمستويات المعرفية العليا.</li>
                        <li>يمثل <strong>مكون الحضور والمشاركة</strong> مهارات <strong>التذكر والفهم</strong> والمستويات المعرفية الدنيا، والتي تعد أساسية للبناء المعرفي.</li>
                        <li>يوضح <strong>الاختبار النهائي</strong> قدرة الطالب على <strong>التكامل المعرفي</strong> وتطبيق المعرفة في سياقات جديدة.</li>
                        <li>يساعد <strong>التحليل البصري</strong> في تحديد الأنماط التعليمية الفعالة واستراتيجيات الدعم المناسبة لكل فئة من الطلاب.</li>
                        <li>يُعد التوزيع الطبيعي للدرجات مؤشراً على عدالة الاختبار وتناسب مستواه مع قدرات الطلاب.</li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- قسم التصدير والتقارير -->
        <section id="export">
            <div class="card">
                <div class="card-header">
                    <div class="card-icon">
                        <i class="fas fa-file-export"></i>
                    </div>
                    <h2>تصدير البيانات والتقارير</h2>
                </div>
                
                <div class="alert alert-info">
                    <i class="fas fa-file-alt alert-icon"></i>
                    <div>
                        <strong>ميزات التصدير:</strong> يمكنك تصدير البيانات والتقارير بعدة صيغ للاستفادة منها في إعداد التقارير الرسمية أو العروض التقديمية
                    </div>
                </div>
                
                <div class="export-buttons">
                    <button class="btn export-btn" onclick="exportToPDF()">
                        <i class="fas fa-file-pdf"></i> تصدير إلى PDF
                    </button>
                    <button class="btn export-btn" onclick="exportToExcel()">
                        <i class="fas fa-file-excel"></i> تصدير إلى Excel
                    </button>
                    <button class="btn export-btn" onclick="exportToCSV()">
                        <i class="fas fa-file-csv"></i> تصدير إلى CSV
                    </button>
                    <button class="btn export-btn" onclick="exportCharts()">
                        <i class="fas fa-image"></i> تصدير الرسوم البيانية
                    </button>
                    <button class="btn export-btn" onclick="printReport()">
                        <i class="fas fa-print"></i> طباعة التقرير
                    </button>
                </div>
                
                <div class="interpretation" style="margin-top: 2rem;">
                    <h4><i class="fas fa-clipboard-list"></i> نموذج تقرير تحليلي</h4>
                    <div id="reportPreview">
                        <p><strong>التقرير التحليلي لنتائج الطلاب</strong></p>
                        <p>تاريخ التقرير: <span id="reportDate"></span></p>
                        <p>إجمالي عدد الطلاب: <span id="reportTotalStudents">0</span></p>
                        <p>المتوسط العام: <span id="reportAvg">0</span></p>
                        <p>نسبة النجاح: <span id="reportPassRate">0%</span></p>
                        <p>التوصيات:</p>
                        <ul>
                            <li>توفير جلسات علاجية للطلاب المتعثرين (<span id="reportWeakCount">0</span> طالب)</li>
                            <li>تطوير أنشطة إثرائية للطلاب المتفوقين (<span id="reportExcellentCount">0</span> طالب)</li>
                            <li>مراجعة استراتيجيات التدريس بناءً على نتائج التحليل</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <footer>
        <div class="footer-content">
            <div class="footer-logo">نظام <span>تحليل</span> نتائج الطلاب</div>
            <div class="footer-text">
                مشروع تربوي تحليلي متكامل – صُمم خصيصاً لتلبية متطلبات العملية التعليمية في مدارس المملكة العربية السعودية
                <br>يدعم اتخاذ القرارات التربوية ويعزز جودة التعليم
            </div>
        </div>
    </footer>

    <script>
        // بيانات التطبيق
        let students = [];
        let barChart, pieChart, lineChart;
        
        // تهيئة التاريخ في التقرير
        document.getElementById('reportDate').textContent = new Date().toLocaleDateString('ar-SA');
        
        // عرض القسم المحدد
        function showSection(id) {
            // إخفاء جميع الأقسام
            document.querySelectorAll('section').forEach(s => s.classList.remove('active'));
            // إزالة النشاط من جميع أزرار التنقل
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
            // عرض القسم المحدد
            document.getElementById(id).classList.add('active');
            // تفعيل زر التنقل المناسب
            document.querySelector(`nav button[onclick="showSection('${id}')"]`).classList.add('active');
            
            // عند عرض قسم الرسوم البيانية، نقوم بتحديثها
            if (id === 'charts' || id === 'analysis') {
                setTimeout(drawCharts, 100);
            }
            
            // عند عرض قسم التصدير، نقوم بتحديث التقرير
            if (id === 'export') {
                updateReportPreview();
            }
        }
        
        // إضافة طلاب من منطقة النص
        function addStudents() {
            const namesInput = document.getElementById('names');
            const names = namesInput.value.split('\n');
            let addedCount = 0;
            
            names.forEach(name => {
                const trimmedName = name.trim();
                if (trimmedName && !students.some(s => s.name === trimmedName)) {
                    // توليد درجات عشوائية للتوضيح
                    const tasks = Math.floor(Math.random() * 20) + 20; // بين 20 و40
                    const att = Math.floor(Math.random() * 10) + 10; // بين 10 و20
                    const final = Math.floor(Math.random() * 20) + 20; // بين 20 و40
                    
                    students.push({
                        name: trimmedName,
                        tasks: tasks,
                        att: att,
                        final: final
                    });
                    addedCount++;
                }
            });
            
            // إظهار رسالة نجاح
            if (addedCount > 0) {
                showAlert(`تم إضافة ${addedCount} طالب بنجاح`, 'success');
                namesInput.value = '';
                render();
            } else {
                showAlert('لم يتم إضافة أي طلاب. قد تكون الأسماء مكررة أو فارغة', 'warning');
            }
        }
        
        // إضافة بيانات تجريبية
        function addSampleData() {
            const sampleNames = [
                "أحمد محمد", "فاطمة علي", "خالد سعيد", "سارة عبدالله", 
                "محمد حسن", "نورة خالد", "عبدالرحمن عمر", "لينا فارس",
                "يوسف إبراهيم", "هدى سالم", "علي محمود", "مريم كامل"
            ];
            
            let addedCount = 0;
            
            sampleNames.forEach(name => {
                if (!students.some(s => s.name === name)) {
                    const tasks = Math.floor(Math.random() * 25) + 15; // بين 15 و40
                    const att = Math.floor(Math.random() * 15) + 5; // بين 5 و20
                    const final = Math.floor(Math.random() * 25) + 15; // بين 15 و40
                    
                    students.push({
                        name: name,
                        tasks: tasks,
                        att: att,
                        final: final
                    });
                    addedCount++;
                }
            });
            
            if (addedCount > 0) {
                showAlert(`تم إضافة ${addedCount} طالب تجريبي بنجاح`, 'success');
                render();
            }
        }
        
        // مسح جميع البيانات
        function clearAllData() {
            if (confirm('هل أنت متأكد من رغبتك في مسح جميع البيانات؟ لا يمكن التراجع عن هذا الإجراء.')) {
                students = [];
                render();
                showAlert('تم مسح جميع البيانات بنجاح', 'success');
            }
        }
        
        // تحديد المستوى بناءً على النسبة المئوية
        function getLevel(percentage) {
            if (percentage >= 90) return ["متفوق", "good"];
            if (percentage >= 75) return ["جيد جدًا", "good"];
            if (percentage >= 60) return ["جيد", "mid"];
            return ["متعثر", "bad"];
        }
        
        // عرض الجدول
        function render() {
            const tableBody = document.getElementById('table');
            tableBody.innerHTML = '';
            
            students.forEach((student, index) => {
                const cont = student.tasks + student.att;
                const total = cont + student.final;
                const percentage = Math.round((total / 160) * 100);
                const [levelText, levelClass] = getLevel(percentage);
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${student.name}</td>
                    <td>
                        <input type="number" min="0" max="40" value="${student.tasks}" 
                               onchange="updateStudent(${index}, 'tasks', this.value)">
                    </td>
                    <td>
                        <input type="number" min="0" max="20" value="${student.att}" 
                               onchange="updateStudent(${index}, 'att', this.value)">
                    </td>
                    <td>${cont}</td>
                    <td>
                        <input type="number" min="0" max="40" value="${student.final}" 
                               onchange="updateStudent(${index}, 'final', this.value)">
                    </td>
                    <td><strong>${total}</strong></td>
                    <td>${percentage}%</td>
                    <td><span class="badge ${levelClass}">${levelText}</span></td>
                    <td>
                        <button class="btn" style="padding: 0.25rem 0.5rem; font-size: 0.8rem;" 
                                onclick="removeStudent(${index})">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                `;
                tableBody.appendChild(row);
            });
            
            // تحديث التحليل
            analyze();
            // تحديث شريط التقدم
            updateProgressBar();
        }
        
        // تحديث بيانات الطالب
        function updateStudent(index, field, value) {
            students[index][field] = parseInt(value) || 0;
            render();
        }
        
        // حذف طالب
        function removeStudent(index) {
            if (confirm(`هل تريد حذف الطالب ${students[index].name}؟`)) {
                students.splice(index, 1);
                render();
                showAlert('تم حذف الطالب بنجاح', 'success');
            }
        }
        
        // التحليل الإحصائي
        function analyze() {
            if (students.length === 0) {
                document.getElementById('avg').textContent = '0';
                document.getElementById('std').textContent = '0';
                document.getElementById('max').textContent = '0';
                document.getElementById('min').textContent = '0';
                document.getElementById('excellentCount').textContent = '0';
                document.getElementById('weakCount').textContent = '0';
                return;
            }
            
            // حساب المجموع لكل طالب
            const totals = students.map(s => s.tasks + s.att + s.final);
            const percentages = totals.map(t => Math.round((t / 160) * 100));
            
            // حساب المتوسط
            const avgValue = totals.reduce((a, b) => a + b, 0) / totals.length;
            document.getElementById('avg').textContent = avgValue.toFixed(1);
            
            // حساب الانحراف المعياري
            const stdValue = Math.sqrt(
                totals.reduce((a, b) => a + Math.pow(b - avgValue, 2), 0) / totals.length
            );
            document.getElementById('std').textContent = stdValue.toFixed(2);
            
            // أعلى وأقل درجة
            document.getElementById('max').textContent = Math.max(...totals);
            document.getElementById('min').textContent = Math.min(...totals);
            
            // عدد المتفوقين والمتعثرين
            const excellentCount = percentages.filter(p => p >= 90).length;
            const weakCount = percentages.filter(p => p < 60).length;
            
            document.getElementById('excellentCount').textContent = excellentCount;
            document.getElementById('weakCount').textContent = weakCount;
        }
        
        // تحديث شريط التقدم
        function updateProgressBar() {
            if (students.length === 0) {
                document.getElementById('progressFill').style.width = '0%';
                return;
            }
            
            const percentages = students.map(s => {
                const total = s.tasks + s.att + s.final;
                return Math.round((total / 160) * 100);
            });
            
            // حساب متوسط النسبة
            const avgPercentage = percentages.reduce((a, b) => a + b, 0) / percentages.length;
            
            // تحديث عرض شريط التقدم
            document.getElementById('progressFill').style.width = `${avgPercentage}%`;
        }
        
        // رسم الرسوم البيانية
        function drawCharts() {
            if (students.length === 0) {
                // إخفاء الرسوم البيانية إذا لم تكن هناك بيانات
                return;
            }
            
            const labels = students.map(s => s.name);
            const tasks = students.map(s => s.tasks);
            const att = students.map(s => s.att);
            const fin = students.map(s => s.final);
            const totals = students.map(s => s.tasks + s.att + s.final);
            
            // رسم بياني أعمدة
            const barChartCanvas = document.getElementById('barChart').getContext('2d');
            
            if (barChart) barChart.destroy();
            barChart = new Chart(barChartCanvas, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: 'المهام الأدائية',
                            data: tasks,
                            backgroundColor: '#1976d2',
                            borderColor: '#0d47a1',
                            borderWidth: 1
                        },
                        {
                            label: 'الحضور والمشاركة',
                            data: att,
                            backgroundColor: '#4caf50',
                            borderColor: '#2e7d32',
                            borderWidth: 1
                        },
                        {
                            label: 'الاختبار النهائي',
                            data: fin,
                            backgroundColor: '#f57c00',
                            borderColor: '#e65100',
                            borderWidth: 1
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'top',
                            rtl: true
                        },
                        title: {
                            display: true,
                            text: 'توزيع درجات الطلاب حسب المكونات',
                            font: {
                                size: 16
                            }
                        }
                    },
                    scales: {
                        x: {
                            ticks: {
                                maxRotation: 45,
                                minRotation: 0
                            }
                        },
                        y: {
                            beginAtZero: true,
                            max: 160,
                            title: {
                                display: true,
                                text: 'الدرجة'
                            }
                        }
                    }
                }
            });
            
            // رسم بياني دائري
            const pieChartCanvas = document.getElementById('pieChart').getContext('2d');
            const levels = {
                متفوق: 0,
                "جيد جدًا": 0,
                جيد: 0,
                متعثر: 0
            };
            
            students.forEach(s => {
                const total = s.tasks + s.att + s.final;
                const percentage = Math.round((total / 160) * 100);
                const [levelText] = getLevel(percentage);
                levels[levelText]++;
            });
            
            if (pieChart) pieChart.destroy();
            pieChart = new Chart(pieChartCanvas, {
                type: 'pie',
                data: {
                    labels: Object.keys(levels),
                    datasets: [{
                        data: Object.values(levels),
                        backgroundColor: [
                            '#2e7d32',
                            '#4caf50',
                            '#f9a825',
                            '#c62828'
                        ],
                        borderWidth: 2,
                        borderColor: '#fff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'right',
                            rtl: true
                        },
                        title: {
                            display: true,
                            text: 'توزيع الطلاب حسب المستوى',
                            font: {
                                size: 16
                            }
                        }
                    }
                }
            });
            
            // رسم بياني خطي
            const lineChartCanvas = document.getElementById('lineChart').getContext('2d');
            
            // ترتيب الطلاب حسب المجموع
            const sortedStudents = [...students]
                .map(s => ({...s, total: s.tasks + s.att + s.final}))
                .sort((a, b) => a.total - b.total);
            
            const sortedLabels = sortedStudents.map(s => s.name);
            const sortedTotals = sortedStudents.map(s => s.total);
            
            if (lineChart) lineChart.destroy();
            lineChart = new Chart(lineChartCanvas, {
                type: 'line',
                data: {
                    labels: sortedLabels,
                    datasets: [{
                        label: 'الدرجة الكلية',
                        data: sortedTotals,
                        backgroundColor: 'rgba(42, 157, 143, 0.1)',
                        borderColor: '#2a9d8f',
                        borderWidth: 3,
                        fill: true,
                        tension: 0.3
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        },
                        title: {
                            display: true,
                            text: 'توزيع الدرجات الكلية (مرتبة)',
                            font: {
                                size: 16
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 160,
                            title: {
                                display: true,
                                text: 'الدرجة'
                            }
                        },
                        x: {
                            ticks: {
                                maxRotation: 45,
                                minRotation: 0
                            }
                        }
                    }
                }
            });
        }
        
        // تحديث معاينة التقرير
        function updateReportPreview() {
            if (students.length === 0) {
                document.getElementById('reportTotalStudents').textContent = '0';
                document.getElementById('reportAvg').textContent = '0';
                document.getElementById('reportPassRate').textContent = '0%';
                document.getElementById('reportWeakCount').textContent = '0';
                document.getElementById('reportExcellentCount').textContent = '0';
                return;
            }
            
            const totals = students.map(s => s.tasks + s.att + s.final);
            const percentages = totals.map(t => Math.round((t / 160) * 100));
            
            // إحصائيات التقرير
            const avgValue = totals.reduce((a, b) => a + b, 0) / totals.length;
            const passRate = (percentages.filter(p => p >= 60).length / percentages.length * 100).toFixed(1);
            const excellentCount = percentages.filter(p => p >= 90).length;
            const weakCount = percentages.filter(p => p < 60).length;
            
            document.getElementById('reportTotalStudents').textContent = students.length;
            document.getElementById('reportAvg').textContent = avgValue.toFixed(1);
            document.getElementById('reportPassRate').textContent = `${passRate}%`;
            document.getElementById('reportWeakCount').textContent = weakCount;
            document.getElementById('reportExcellentCount').textContent = excellentCount;
        }
        
        // وظائف التصدير الحقيقية
        function exportToPDF() {
            if (students.length === 0) {
                showAlert('لا توجد بيانات لتصديرها. الرجاء إضافة بيانات أولاً.', 'error');
                return;
            }
            
            showLoadingModal('جارٍ إنشاء ملف PDF...');
            
            setTimeout(() => {
                try {
                    const { jsPDF } = window.jspdf;
                    const doc = new jsPDF({
                        orientation: 'portrait',
                        unit: 'mm',
                        format: 'a4'
                    });
                    
                    // إعداد الخط العربي (نستخدم خط افتراضي مع دعم Unicode)
                    doc.setFont("helvetica", "normal");
                    
                    // العنوان
                    doc.setFontSize(20);
                    doc.text('تقرير تحليل نتائج الطلاب', 105, 15, { align: 'center' });
                    
                    // التاريخ
                    doc.setFontSize(12);
                    doc.text(`تاريخ التقرير: ${new Date().toLocaleDateString('ar-SA')}`, 105, 25, { align: 'center' });
                    
                    // إحصائيات عامة
                    doc.setFontSize(14);
                    doc.text('الإحصائيات العامة', 105, 35, { align: 'center' });
                    
                    const totals = students.map(s => s.tasks + s.att + s.final);
                    const percentages = totals.map(t => Math.round((t / 160) * 100));
                    const avgValue = totals.reduce((a, b) => a + b, 0) / totals.length;
                    const passRate = (percentages.filter(p => p >= 60).length / percentages.length * 100).toFixed(1);
                    const excellentCount = percentages.filter(p => p >= 90).length;
                    const weakCount = percentages.filter(p => p < 60).length;
                    
                    doc.setFontSize(12);
                    doc.text(`إجمالي عدد الطلاب: ${students.length}`, 20, 45);
                    doc.text(`المتوسط العام: ${avgValue.toFixed(1)}`, 20, 52);
                    doc.text(`نسبة النجاح: ${passRate}%`, 20, 59);
                    doc.text(`عدد المتفوقين: ${excellentCount}`, 20, 66);
                    doc.text(`عدد المتعثرين: ${weakCount}`, 20, 73);
                    
                    // جدول الطلاب
                    doc.setFontSize(14);
                    doc.text('تفاصيل درجات الطلاب', 105, 85, { align: 'center' });
                    
                    // إعداد بيانات الجدول
                    const tableData = students.map((student, index) => {
                        const cont = student.tasks + student.att;
                        const total = cont + student.final;
                        const percentage = Math.round((total / 160) * 100);
                        const [levelText] = getLevel(percentage);
                        
                        return [
                            index + 1,
                            student.name,
                            student.tasks,
                            student.att,
                            cont,
                            student.final,
                            total,
                            `${percentage}%`,
                            levelText
                        ];
                    });
                    
                    // إضافة الجدول
                    doc.autoTable({
                        startY: 90,
                        head: [['#', 'الطالب', 'المهام', 'الحضور', 'المستمر', 'النهائي', 'المجموع', 'النسبة', 'المستوى']],
                        body: tableData,
                        theme: 'grid',
                        styles: { font: 'helvetica', fontSize: 10, textAlign: 'center' },
                        headStyles: { fillColor: [11, 60, 93], textColor: 255, fontStyle: 'bold' },
                        alternateRowStyles: { fillColor: [240, 240, 240] },
                        margin: { right: 20, left: 20 }
                    });
                    
                    // التوصيات
                    const finalY = doc.lastAutoTable.finalY + 10;
                    doc.setFontSize(14);
                    doc.text('التوصيات', 105, finalY, { align: 'center' });
                    
                    doc.setFontSize(11);
                    let yPos = finalY + 10;
                    doc.text('1. توفير جلسات علاجية للطلاب المتعثرين', 20, yPos);
                    yPos += 7;
                    doc.text('2. تطوير أنشطة إثرائية للطلاب المتفوقين', 20, yPos);
                    yPos += 7;
                    doc.text('3. مراجعة استراتيجيات التدريس بناءً على نتائج التحليل', 20, yPos);
                    
                    // حفظ الملف
                    doc.save(`تقرير_نتائج_الطلاب_${new Date().toISOString().split('T')[0]}.pdf`);
                    
                    hideLoadingModal();
                    showAlert('تم تصدير البيانات إلى ملف PDF بنجاح', 'success');
                } catch (error) {
                    hideLoadingModal();
                    showAlert('حدث خطأ أثناء إنشاء ملف PDF: ' + error.message, 'error');
                    console.error('PDF Export Error:', error);
                }
            }, 1000);
        }
        
        function exportToExcel() {
            if (students.length === 0) {
                showAlert('لا توجد بيانات لتصديرها. الرجاء إضافة بيانات أولاً.', 'error');
                return;
            }
            
            showLoadingModal('جارٍ إنشاء ملف Excel...');
            
            setTimeout(() => {
                try {
                    // إعداد بيانات الورقة
                    const data = students.map((student, index) => {
                        const cont = student.tasks + student.att;
                        const total = cont + student.final;
                        const percentage = Math.round((total / 160) * 100);
                        const [levelText] = getLevel(percentage);
                        
                        return {
                            'الرقم': index + 1,
                            'اسم الطالب': student.name,
                            'المهام الأدائية': student.tasks,
                            'الحضور والمشاركة': student.att,
                            'المستمر': cont,
                            'الاختبار النهائي': student.final,
                            'المجموع الكلي': total,
                            'النسبة المئوية': `${percentage}%`,
                            'المستوى': levelText
                        };
                    });
                    
                    // إضافة إحصائيات عامة
                    const totals = students.map(s => s.tasks + s.att + s.final);
                    const percentages = totals.map(t => Math.round((t / 160) * 100));
                    const avgValue = totals.reduce((a, b) => a + b, 0) / totals.length;
                    const passRate = (percentages.filter(p => p >= 60).length / percentages.length * 100).toFixed(1);
                    const excellentCount = percentages.filter(p => p >= 90).length;
                    const weakCount = percentages.filter(p => p < 60).length;
                    
                    const statsData = [
                        {},
                        { 'اسم الطالب': 'الإحصائيات العامة', 'المجموع الكلي': '' },
                        { 'اسم الطالب': 'إجمالي عدد الطلاب', 'المجموع الكلي': students.length },
                        { 'اسم الطالب': 'المتوسط العام', 'المجموع الكلي': avgValue.toFixed(1) },
                        { 'اسم الطالب': 'نسبة النجاح', 'المجموع الكلي': `${passRate}%` },
                        { 'اسم الطالب': 'عدد المتفوقين', 'المجموع الكلي': excellentCount },
                        { 'اسم الطالب': 'عدد المتعثرين', 'المجموع الكلي': weakCount }
                    ];
                    
                    // دمج البيانات
                    const allData = [...statsData, {}, ...data];
                    
                    // إنشاء ورقة العمل
                    const ws = XLSX.utils.json_to_sheet(allData, { skipHeader: true });
                    
                    // إضافة عنوان
                    const title = [['تقرير تحليل نتائج الطلاب'], [`تاريخ التقرير: ${new Date().toLocaleDateString('ar-SA')}`], []];
                    XLSX.utils.sheet_add_aoa(ws, title, { origin: "A1" });
                    
                    // إنشاء المصنف وإضافة الورقة
                    const wb = XLSX.utils.book_new();
                    XLSX.utils.book_append_sheet(wb, ws, "نتائج الطلاب");
                    
                    // حفظ الملف
                    XLSX.writeFile(wb, `نتائج_الطلاب_${new Date().toISOString().split('T')[0]}.xlsx`);
                    
                    hideLoadingModal();
                    showAlert('تم تصدير البيانات إلى ملف Excel بنجاح', 'success');
                } catch (error) {
                    hideLoadingModal();
                    showAlert('حدث خطأ أثناء إنشاء ملف Excel: ' + error.message, 'error');
                    console.error('Excel Export Error:', error);
                }
            }, 1000);
        }
        
        function exportToCSV() {
            if (students.length === 0) {
                showAlert('لا توجد بيانات لتصديرها. الرجاء إضافة بيانات أولاً.', 'error');
                return;
            }
            
            showLoadingModal('جارٍ إنشاء ملف CSV...');
            
            setTimeout(() => {
                try {
                    // إعداد رأس CSV
                    let csvContent = "الرقم,اسم الطالب,المهام الأدائية,الحضور والمشاركة,المستمر,الاختبار النهائي,المجموع الكلي,النسبة المئوية,المستوى\n";
                    
                    // إضافة بيانات الطلاب
                    students.forEach((student, index) => {
                        const cont = student.tasks + student.att;
                        const total = cont + student.final;
                        const percentage = Math.round((total / 160) * 100);
                        const [levelText] = getLevel(percentage);
                        
                        csvContent += `${index + 1},${student.name},${student.tasks},${student.att},${cont},${student.final},${total},${percentage}%,${levelText}\n`;
                    });
                    
                    // إضافة إحصائيات
                    const totals = students.map(s => s.tasks + s.att + s.final);
                    const percentages = totals.map(t => Math.round((t / 160) * 100));
                    const avgValue = totals.reduce((a, b) => a + b, 0) / totals.length;
                    const passRate = (percentages.filter(p => p >= 60).length / percentages.length * 100).toFixed(1);
                    const excellentCount = percentages.filter(p => p >= 90).length;
                    const weakCount = percentages.filter(p => p < 60).length;
                    
                    csvContent += `\n\nالإحصائيات العامة\n`;
                    csvContent += `إجمالي عدد الطلاب,${students.length}\n`;
                    csvContent += `المتوسط العام,${avgValue.toFixed(1)}\n`;
                    csvContent += `نسبة النجاح,${passRate}%\n`;
                    csvContent += `عدد المتفوقين,${excellentCount}\n`;
                    csvContent += `عدد المتعثرين,${weakCount}\n`;
                    
                    // إنشاء رابط تحميل
                    const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
                    const link = document.createElement("a");
                    const url = URL.createObjectURL(blob);
                    
                    link.setAttribute("href", url);
                    link.setAttribute("download", `نتائج_الطلاب_${new Date().toISOString().split('T')[0]}.csv`);
                    link.style.visibility = 'hidden';
                    
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                    
                    hideLoadingModal();
                    showAlert('تم تصدير البيانات إلى ملف CSV بنجاح', 'success');
                } catch (error) {
                    hideLoadingModal();
                    showAlert('حدث خطأ أثناء إنشاء ملف CSV: ' + error.message, 'error');
                    console.error('CSV Export Error:', error);
                }
            }, 500);
        }
        
        function exportCharts() {
            if (students.length === 0) {
                showAlert('لا توجد رسوم بيانية لتصديرها. الرجاء إضافة بيانات أولاً.', 'error');
                return;
            }
            
            showLoadingModal('جارٍ حفظ الرسوم البيانية كصور...');
            
            setTimeout(() => {
                try {
                    // تحويل كل رسم بياني إلى صورة
                    const charts = ['barChart', 'pieChart', 'lineChart'];
                    let savedCount = 0;
                    
                    charts.forEach((chartId, index) => {
                        const chartCanvas = document.getElementById(chartId);
                        if (chartCanvas) {
                            const image = chartCanvas.toDataURL('image/png');
                            const link = document.createElement('a');
                            link.href = image;
                            link.download = `رسم_بياني_${chartId}_${new Date().toISOString().split('T')[0]}.png`;
                            link.click();
                            savedCount++;
                        }
                    });
                    
                    hideLoadingModal();
                    
                    if (savedCount > 0) {
                        showAlert(`تم حفظ ${savedCount} رسم بياني كصور بنجاح`, 'success');
                    } else {
                        showAlert('لم يتم العثور على رسوم بيانية لحفظها', 'warning');
                    }
                } catch (error) {
                    hideLoadingModal();
                    showAlert('حدث خطأ أثناء حفظ الرسوم البيانية: ' + error.message, 'error');
                    console.error('Charts Export Error:', error);
                }
            }, 1500);
        }
        
        function printReport() {
            if (students.length === 0) {
                showAlert('لا توجد بيانات للطباعة. الرجاء إضافة بيانات أولاً.', 'error');
                return;
            }
            
            // إنشاء نافذة طباعة
            const printWindow = window.open('', '_blank');
            
            const totals = students.map(s => s.tasks + s.att + s.final);
            const percentages = totals.map(t => Math.round((t / 160) * 100));
            const avgValue = totals.reduce((a, b) => a + b, 0) / totals.length;
            const passRate = (percentages.filter(p => p >= 60).length / percentages.length * 100).toFixed(1);
            const excellentCount = percentages.filter(p => p >= 90).length;
            const weakCount = percentages.filter(p => p < 60).length;
            
            // إنشاء محتوى الطباعة
            let printContent = `
                <!DOCTYPE html>
                <html lang="ar" dir="rtl">
                <head>
                    <meta charset="UTF-8">
                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                    <title>تقرير نتائج الطلاب</title>
                    <style>
                        body { font-family: 'Cairo', Arial, sans-serif; line-height: 1.6; color: #333; margin: 20px; }
                        h1, h2, h3 { color: #0b3c5d; }
                        .header { text-align: center; border-bottom: 3px solid #0b3c5d; padding-bottom: 20px; margin-bottom: 30px; }
                        table { width: 100%; border-collapse: collapse; margin: 20px 0; }
                        th, td { border: 1px solid #ddd; padding: 10px; text-align: center; }
                        th { background-color: #0b3c5d; color: white; }
                        tr:nth-child(even) { background-color: #f9f9f9; }
                        .stats { background-color: #f0f8ff; padding: 20px; border-radius: 10px; margin: 20px 0; }
                        .recommendations { background-color: #fff8e1; padding: 20px; border-radius: 10px; margin: 20px 0; }
                        @media print {
                            body { margin: 0; padding: 10px; }
                            .no-print { display: none; }
                        }
                    </style>
                </head>
                <body>
                    <div class="header">
                        <h1>تقرير تحليل نتائج الطلاب</h1>
                        <p>تاريخ التقرير: ${new Date().toLocaleDateString('ar-SA')}</p>
                        <p>نظام تحليل نتائج الطلاب - النسخة المحسنة</p>
                    </div>
                    
                    <div class="stats">
                        <h2>الإحصائيات العامة</h2>
                        <p><strong>إجمالي عدد الطلاب:</strong> ${students.length}</p>
                        <p><strong>المتوسط العام:</strong> ${avgValue.toFixed(1)} من 160</p>
                        <p><strong>نسبة النجاح:</strong> ${passRate}%</p>
                        <p><strong>عدد المتفوقين:</strong> ${excellentCount} طالب</p>
                        <p><strong>عدد المتعثرين:</strong> ${weakCount} طالب</p>
                    </div>
                    
                    <h2>تفاصيل درجات الطلاب</h2>
                    <table>
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>اسم الطالب</th>
                                <th>المهام الأدائية</th>
                                <th>الحضور والمشاركة</th>
                                <th>المستمر</th>
                                <th>الاختبار النهائي</th>
                                <th>المجموع الكلي</th>
                                <th>النسبة المئوية</th>
                                <th>المستوى</th>
                            </tr>
                        </thead>
                        <tbody>
            `;
            
            // إضافة بيانات الطلاب
            students.forEach((student, index) => {
                const cont = student.tasks + student.att;
                const total = cont + student.final;
                const percentage = Math.round((total / 160) * 100);
                const [levelText] = getLevel(percentage);
                
                printContent += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>${student.name}</td>
                        <td>${student.tasks}</td>
                        <td>${student.att}</td>
                        <td>${cont}</td>
                        <td>${student.final}</td>
                        <td><strong>${total}</strong></td>
                        <td>${percentage}%</td>
                        <td>${levelText}</td>
                    </tr>
                `;
            });
            
            printContent += `
                        </tbody>
                    </table>
                    
                    <div class="recommendations">
                        <h2>التوصيات</h2>
                        <ol>
                            <li>توفير جلسات علاجية للطلاب المتعثرين (${weakCount} طالب)</li>
                            <li>تطوير أنشطة إثرائية للطلاب المتفوقين (${excellentCount} طالب)</li>
                            <li>مراجعة استراتيجيات التدريس بناءً على نتائج التحليل</li>
                            <li>توزيع الطلاب على مجموعات متجانسة حسب مستوياتهم</li>
                            <li>تطوير خطط دعم فردية للطلاب المتعثرين</li>
                        </ol>
                    </div>
                    
                    <div class="no-print" style="text-align: center; margin-top: 30px;">
                        <button onclick="window.print()" style="padding: 10px 20px; background-color: #0b3c5d; color: white; border: none; border-radius: 5px; cursor: pointer;">
                            طباعة التقرير
                        </button>
                        <button onclick="window.close()" style="padding: 10px 20px; background-color: #c62828; color: white; border: none; border-radius: 5px; cursor: pointer; margin-right: 10px;">
                            إغلاق النافذة
                        </button>
                    </div>
                    
                    <footer style="margin-top: 50px; text-align: center; color: #666; font-size: 12px;">
                        <p>تم إنشاء هذا التقرير بواسطة نظام تحليل نتائج الطلاب - النسخة المحسنة</p>
                        <p>© ${new Date().getFullYear()} - جميع الحقوق محفوظة</p>
                    </footer>
                </body>
                </html>
            `;
            
            printWindow.document.write(printContent);
            printWindow.document.close();
            
            showAlert('تم فتح نافذة الطباعة. يرجى استخدام قائمة الطباعة في المتصفح.', 'info');
        }
        
        // وظائف مساعدة للواجهة
        function showLoadingModal(message = 'جارٍ التحميل...') {
            document.getElementById('exportProgress').textContent = message;
            document.getElementById('loadingModal').style.display = 'block';
        }
        
        function hideLoadingModal() {
            document.getElementById('loadingModal').style.display = 'none';
        }
        
        // عرض رسائل التنبيه
        function showAlert(message, type) {
            // إنشاء عنصر التنبيه
            const alertDiv = document.createElement('div');
            alertDiv.className = `alert alert-${type}`;
            
            // اختيار الأيقونة المناسبة
            let icon = 'info-circle';
            if (type === 'success') icon = 'check-circle';
            if (type === 'warning') icon = 'exclamation-triangle';
            if (type === 'error') icon = 'exclamation-circle';
            
            alertDiv.innerHTML = `
                <i class="fas fa-${icon} alert-icon"></i>
                <div>${message}</div>
            `;
            
            // إضافة التنبيه إلى بداية main
            const main = document.querySelector('main');
            main.insertBefore(alertDiv, main.firstChild);
            
            // إزالة التنبيه بعد 5 ثوان
            setTimeout(() => {
                if (alertDiv.parentNode) {
                    alertDiv.remove();
                }
            }, 5000);
        }
        
        // تهيئة التطبيق عند التحميل
        document.addEventListener('DOMContentLoaded', function() {
            // إضافة بعض البيانات التجريبية تلقائياً للعرض التوضيحي
            setTimeout(addSampleData, 500);
        });
    </script>
</body>
</html>
