<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RB‑PMS Pro | Results‑Based Project Management System</title>
    <!-- Fonts & Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        :root {
            --primary: #0a4a6b;
            --primary-light: #1a6a94;
            --secondary: #2a9d8f;
            --accent: #e9c46a;
            --bg-light: #f4f9ff;
            --border-light: #d9e6f2;
            --text-dark: #0b2a40;
            --text-muted: #4a6f89;
            --success: #2a9d8f;
            --warning: #e9c46a;
            --danger: #e76f51;
            --card-shadow: 0 10px 30px -8px rgba(0,35,70,0.08);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(145deg, #edf3f9 0%, #f7fbfe 100%);
            color: var(--text-dark);
            padding: 28px 32px;
            line-height: 1.5;
            min-height: 100vh;
        }

        .app-container {
            max-width: 1920px;
            margin: 0 auto;
        }

        /* ===== HEADER ===== */
        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 28px;
            flex-wrap: wrap;
            gap: 20px;
        }

        .brand-section h1 {
            font-weight: 800;
            font-size: 2.2rem;
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            display: flex;
            align-items: center;
            gap: 14px;
            margin-bottom: 8px;
        }

        .brand-section h1 i {
            background: var(--primary);
            -webkit-text-fill-color: white;
            padding: 14px;
            border-radius: 18px;
            font-size: 1.6rem;
        }

        .version-tag {
            background: var(--primary-light);
            color: white;
            padding: 6px 16px;
            border-radius: 40px;
            font-size: 0.8rem;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .project-setup-card {
            background: white;
            padding: 20px 28px;
            border-radius: 30px;
            box-shadow: var(--card-shadow);
            border: 1px solid var(--border-light);
            min-width: 400px;
        }

        .setup-title {
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .setup-controls {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            align-items: center;
        }

        .setup-select, .setup-input {
            padding: 12px 18px;
            border-radius: 40px;
            border: 1.5px solid var(--border-light);
            font-family: 'Inter', sans-serif;
            background: #fafcff;
            font-size: 0.9rem;
            flex: 1;
            min-width: 150px;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.2s;
            box-shadow: 0 4px 12px rgba(10,74,107,0.2);
        }

        .btn-primary:hover {
            background: var(--primary-light);
            transform: translateY(-2px);
        }

        .btn-outline {
            background: transparent;
            border: 1.5px solid var(--border-light);
            padding: 10px 20px;
            border-radius: 40px;
            font-weight: 500;
            cursor: pointer;
            color: var(--text-dark);
        }

        /* ===== FRAMEWORK BADGES ===== */
        .framework-cloud {
            display: flex;
            flex-wrap: wrap;
            gap: 12px 20px;
            padding: 20px 0;
            margin: 20px 0;
            border-top: 2px solid rgba(10,74,107,0.1);
            border-bottom: 2px solid rgba(10,74,107,0.1);
        }

        .fw-badge {
            background: white;
            padding: 10px 22px;
            border-radius: 40px;
            font-weight: 600;
            color: var(--primary);
            box-shadow: 0 2px 8px rgba(0,0,0,0.02);
            border: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .fw-badge i {
            color: var(--primary-light);
        }

        /* ===== CYCLE VISUALIZATION ===== */
        .cycle-wrapper {
            background: white;
            border-radius: 50px;
            padding: 32px 20px;
            margin: 30px 0;
            box-shadow: var(--card-shadow);
            border: 1px solid var(--border-light);
        }

        .cycle-steps {
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 8px;
        }

        .cycle-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            min-width: 95px;
        }

        .cycle-icon {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 22px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            margin-bottom: 12px;
            box-shadow: 0 8px 16px rgba(10,74,107,0.15);
        }

        .cycle-label {
            font-weight: 700;
            font-size: 0.8rem;
            color: var(--primary);
            text-transform: uppercase;
            letter-spacing: 0.3px;
        }

        .cycle-arrow {
            color: var(--text-muted);
            font-size: 1.8rem;
            opacity: 0.5;
        }

        /* ===== KPI DASHBOARD ===== */
        .kpi-dashboard {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 24px;
            margin-bottom: 32px;
        }

        .kpi-panel {
            background: white;
            border-radius: 28px;
            padding: 26px 24px;
            box-shadow: var(--card-shadow);
            border: 1px solid var(--border-light);
            transition: transform 0.2s;
        }

        .kpi-panel:hover {
            transform: translateY(-4px);
        }

        .kpi-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
        }

        .kpi-title {
            font-weight: 700;
            text-transform: uppercase;
            font-size: 0.8rem;
            letter-spacing: 0.5px;
            color: var(--text-muted);
        }

        .kpi-icon {
            background: var(--bg-light);
            padding: 8px;
            border-radius: 14px;
            color: var(--primary);
        }

        .kpi-main-value {
            font-size: 2.5rem;
            font-weight: 800;
            color: var(--primary);
            margin-bottom: 8px;
        }

        .kpi-trend {
            display: flex;
            align-items: center;
            gap: 8px;
            color: var(--success);
            font-weight: 600;
            margin-bottom: 16px;
        }

        .progress-container {
            height: 10px;
            background: var(--bg-light);
            border-radius: 20px;
            overflow: hidden;
        }

        .progress-bar {
            height: 10px;
            background: linear-gradient(90deg, var(--primary) 0%, var(--primary-light) 100%);
            border-radius: 20px;
            transition: width 0.3s;
        }

        /* ===== MODULE TABS ===== */
        .module-nav {
            display: flex;
            gap: 6px;
            margin: 32px 0 0;
            border-bottom: 2px solid var(--border-light);
        }

        .nav-tab {
            background: transparent;
            border: none;
            padding: 18px 32px;
            font-weight: 700;
            color: var(--text-muted);
            border-radius: 30px 30px 0 0;
            cursor: pointer;
            font-size: 1rem;
            display: flex;
            align-items: center;
            gap: 14px;
            transition: all 0.2s;
        }

        .nav-tab i {
            font-size: 1.3rem;
        }

        .nav-tab.active {
            background: white;
            color: var(--primary);
            box-shadow: 0 -6px 14px rgba(0,0,0,0.02);
            border-bottom: 4px solid var(--primary);
        }

        .nav-tab:hover:not(.active) {
            color: var(--primary-light);
            background: rgba(255,255,255,0.5);
        }

        .module-panel {
            display: none;
            background: white;
            border-radius: 0 28px 28px 28px;
            padding: 36px 34px;
            box-shadow: var(--card-shadow);
            border: 1px solid var(--border-light);
            margin-bottom: 30px;
        }

        .module-panel.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(6px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ===== PANEL LAYOUT ===== */
        .panel-grid-2 {
            display: grid;
            grid-template-columns: 1.5fr 1fr;
            gap: 34px;
        }

        .info-block {
            background: var(--bg-light);
            border-radius: 24px;
            padding: 26px;
            border: 1px solid var(--border-light);
        }

        .section-title {
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 1.3rem;
        }

        .data-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.9rem;
        }

        .data-table th {
            background: var(--primary);
            color: white;
            padding: 16px 14px;
            text-align: left;
            font-weight: 600;
        }

        .data-table td {
            padding: 16px 14px;
            border-bottom: 1px solid var(--border-light);
        }

        .data-table tr:hover td {
            background: rgba(26,106,148,0.03);
        }

        .status-badge {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .status-dot {
            width: 12px;
            height: 12px;
            border-radius: 12px;
        }

        .status-dot.green { background: var(--success); box-shadow: 0 0 8px rgba(42,157,143,0.4); }
        .status-dot.amber { background: var(--warning); }
        .status-dot.red { background: var(--danger); }

        .editable-field {
            border-bottom: 1px dashed var(--primary-light);
            cursor: pointer;
        }

        .chart-container {
            margin-top: 24px;
            padding: 20px;
            background: white;
            border-radius: 20px;
        }

        /* ===== FOOTER ===== */
        .system-footer {
            margin-top: 40px;
            padding: 28px 34px;
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            border-radius: 40px;
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }

        .footer-links {
            display: flex;
            gap: 30px;
        }

        .footer-links span {
            display: flex;
            align-items: center;
            gap: 8px;
        }
    </style>
</head>
<body>
<div class="app-container">
    <!-- HEADER -->
    <div class="app-header">
        <div class="brand-section">
            <h1>
                <i class="fas fa-cubes"></i>
                RB‑PMS Professional
                <span class="version-tag">v3.0 · PM4DEV · UNICEF · GPE</span>
            </h1>
            <p style="color: var(--text-muted); margin-top: 8px; font-weight: 500;">
                <i class="fas fa-arrows-spin"></i> Results‑Based · Fully Customizable · MEAL Integrated
            </p>
        </div>
        <div class="project-setup-card">
            <div class="setup-title">
                <i class="fas fa-sliders-h"></i> Project Configuration
            </div>
            <div class="setup-controls">
                <select id="projectTypeSelect" class="setup-select">
                    <option value="education" selected>📚 Education (STEP‑GPE)</option>
                    <option value="cp">🛡️ Child Protection</option>
                    <option value="eie">🚨 EiE / Humanitarian</option>
                    <option value="wash">💧 WASH</option>
                    <option value="health">🏥 Health & Nutrition</option>
                </select>
                <input type="text" id="projectNameInput" class="setup-input" placeholder="Project name" value="Ethiopia Education CAN 2026">
                <button id="applyConfigBtn" class="btn-primary"><i class="fas fa-check-circle"></i> Apply</button>
            </div>
            <div style="margin-top: 14px; display: flex; gap: 12px;">
                <button id="saveTemplateBtn" class="btn-outline"><i class="far fa-bookmark"></i> Save as template</button>
                <button id="exportConfigBtn" class="btn-outline"><i class="fas fa-download"></i> Export config</button>
            </div>
        </div>
    </div>

    <!-- FRAMEWORK ALIGNMENT -->
    <div class="framework-cloud">
        <span class="fw-badge"><i class="fas fa-check-circle"></i> PM4DEV RBPM</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> UNICEF Module 5</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> GPE Indicators</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> INEE Minimum Standards</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> UNDP ToC</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> World Bank EdStats</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> EiE Competency</span>
        <span class="fw-badge"><i class="fas fa-check-circle"></i> UIS Repository</span>
    </div>

    <!-- PROJECT CYCLE -->
    <div class="cycle-wrapper">
        <div class="cycle-steps">
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-clipboard-list"></i></div><div class="cycle-label">Needs Assessment</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-sitemap"></i></div><div class="cycle-label">Theory of Change</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-chart-bar"></i></div><div class="cycle-label">Results Framework</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-list-check"></i></div><div class="cycle-label">Activity Planning</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-calculator"></i></div><div class="cycle-label">Budget & Costing</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-gears"></i></div><div class="cycle-label">Implementation</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-desktop"></i></div><div class="cycle-label">Monitoring</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-file-alt"></i></div><div class="cycle-label">Reporting</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-lightbulb"></i></div><div class="cycle-label">Learning</div></div>
            <span class="cycle-arrow"><i class="fas fa-chevron-right"></i></span>
            <div class="cycle-item"><div class="cycle-icon"><i class="fas fa-door-open"></i></div><div class="cycle-label">Close‑out</div></div>
        </div>
    </div>

    <!-- KPI DASHBOARD -->
    <div class="kpi-dashboard">
        <div class="kpi-panel">
            <div class="kpi-header"><span class="kpi-title">Children Reached</span><span class="kpi-icon"><i class="fas fa-child"></i></span></div>
            <div class="kpi-main-value">48,320</div>
            <div class="kpi-trend"><i class="fas fa-arrow-up"></i> +12.4% vs target</div>
            <div class="progress-container"><div class="progress-bar" style="width:89%"></div></div>
        </div>
        <div class="kpi-panel">
            <div class="kpi-header"><span class="kpi-title">Teachers Trained</span><span class="kpi-icon"><i class="fas fa-chalkboard-user"></i></span></div>
            <div class="kpi-main-value">1,284</div>
            <div class="kpi-trend">Target: 1,600 · 82%</div>
            <div class="progress-container"><div class="progress-bar" style="width:82%"></div></div>
        </div>
        <div class="kpi-panel">
            <div class="kpi-header"><span class="kpi-title">Budget Execution</span><span class="kpi-icon"><i class="fas fa-sack-dollar"></i></span></div>
            <div class="kpi-main-value">$3.24M</div>
            <div class="kpi-trend">72% · Variance +3.2%</div>
            <div class="progress-container"><div class="progress-bar" style="width:72%"></div></div>
        </div>
        <div class="kpi-panel">
            <div class="kpi-header"><span class="kpi-title">Safeguarding Cases</span><span class="kpi-icon"><i class="fas fa-shield-heart"></i></span></div>
            <div class="kpi-main-value">23</div>
            <div class="kpi-trend"><i class="fas fa-check-circle"></i> 21 resolved</div>
            <div class="progress-container"><div class="progress-bar" style="width:91%"></div></div>
        </div>
    </div>

    <!-- MODULE NAVIGATION -->
    <div class="module-nav">
        <button class="nav-tab active" data-tab="panel1"><i class="fas fa-folder-tree"></i> Planning & ToC</button>
        <button class="nav-tab" data-tab="panel2"><i class="fas fa-table-list"></i> Results & Indicators</button>
        <button class="nav-tab" data-tab="panel3"><i class="fas fa-coins"></i> Budget & Costing</button>
        <button class="nav-tab" data-tab="panel4"><i class="fas fa-chart-pie"></i> MEAL & Dashboards</button>
        <button class="nav-tab" data-tab="panel5"><i class="fas fa-file-export"></i> Reporting & Close</button>
    </div>

    <!-- PANEL 1: PLANNING -->
    <div class="module-panel active" id="panel1">
        <div class="panel-grid-2">
            <div>
                <div class="section-title"><i class="fas fa-magnifying-glass"></i> Needs Assessment & Baseline</div>
                <p><strong>Configurable data sources:</strong> EMIS, school mapping, CP risk assessments, rapid EiE, community consultations, safety assessments.</p>
                <div class="info-block" style="margin-top: 24px;">
                    <h4 style="margin-bottom: 16px;"><i class="fas fa-diagram-project"></i> Theory of Change (UNDP / PM4NGOs)</h4>
                    <p><strong>IF</strong> children access safe, inclusive education, teachers trained, CP services exist, emergency continuity ensured</p>
                    <p style="margin-top: 12px;"><strong>THEN</strong> learning outcomes improve, protection risks reduce, resilience strengthened.</p>
                    <p style="margin-top: 16px; color: var(--success);"><i class="fas fa-check-circle"></i> Assumptions: school accessibility, community buy-in, government coordination.</p>
                </div>
                <p style="margin-top: 20px;" class="editable-field" title="Click to edit"><i class="far fa-pen-to-square"></i> Activity Breakdown (ABS): Output → Activities → Location → Responsible</p>
            </div>
            <div>
                <div class="section-title"><i class="fas fa-sliders"></i> Template Configuration</div>
                <div class="info-block">
                    <p><strong>Active template:</strong> <span id="activeTemplateDisplay">Education (STEP‑GPE) with GPE/UIS indicators & INEE overlay</span></p>
                    <p style="margin-top: 12px;"><i class="fas fa-database"></i> Indicator banks auto-load based on project type.</p>
                    <button id="loadTemplatePanelBtn" class="btn-primary" style="margin-top: 20px; width: 100%;"><i class="fas fa-rotate"></i> Load template & refresh ToC</button>
                </div>
                <div class="chart-container">
                    <canvas id="tocChartCanvas" style="max-height: 180px;"></canvas>
                </div>
            </div>
        </div>
    </div>

    <!-- PANEL 2: INDICATORS -->
    <div class="module-panel" id="panel2">
        <div class="section-title"><i class="fas fa-ruler-combined"></i> Results Framework (GPE / UIS / INEE aligned)</div>
        <table class="data-table">
            <thead><tr><th>Level</th><th>Indicator (disaggregated)</th><th>Baseline</th><th>Target</th><th>Current</th><th>Status</th></tr></thead>
            <tbody id="indicatorTableBody">
                <tr><td>Impact</td><td>% children achieving minimum learning</td><td>42%</td><td>65%</td><td>58%</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>
                <tr><td>Outcome</td><td># children accessing safe education</td><td>18.2k</td><td>35k</td><td>31.5k</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>
                <tr><td>Output</td><td>Teachers trained (female/male)</td><td>340</td><td>1,600</td><td>1,284</td><td><span class="status-badge"><span class="status-dot amber"></span>Moderate</span></td></tr>
                <tr><td>Output</td><td>CP cases referred & managed</td><td>112</td><td>500</td><td>389</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>
            </tbody>
        </table>
        <p style="margin-top: 20px;"><i class="fas fa-tags"></i> Disaggregation: sex, age, disability, IDP/refugee. Source: EMIS, Kobo, CPIMS+.</p>
        <canvas id="indicatorBarCanvas" style="max-height: 180px; margin-top: 24px;"></canvas>
    </div>

    <!-- PANEL 3: BUDGET -->
    <div class="module-panel" id="panel3">
        <div class="panel-grid-2">
            <div>
                <div class="section-title"><i class="fas fa-layer-group"></i> Results‑Based Budget</div>
                <p>Outcome → Output → Activity → Budget Line. Every birr traceable.</p>
                <ul style="margin: 24px 0; list-style: none; line-height: 2.2;">
                    <li><i class="fas fa-user" style="width: 24px;"></i> Personnel: $1,200,000</li>
                    <li><i class="fas fa-chalkboard"></i> Training & Capacity: $680,000</li>
                    <li><i class="fas fa-box"></i> Supplies & Materials: $420,000</li>
                    <li><i class="fas fa-chart-simple"></i> MEAL: $310,000</li>
                    <li><i class="fas fa-shield"></i> Safeguarding/AAP: $90,000</li>
                </ul>
                <div class="info-block">
                    <strong>📊 Unit Costing (World Bank VfM):</strong>
                    <p>Per school: $8,200 · Per child: $67 · Per teacher trained: $190 · Per session: $42</p>
                </div>
            </div>
            <div>
                <div class="section-title"><i class="fas fa-chart-gantt"></i> Budget Control</div>
                <p>Approved: $4.5M · Forecast: $4.38M · Commitments: $3.1M</p>
                <div class="progress-container" style="margin: 16px 0;"><div class="progress-bar" style="width:72%"></div></div>
                <p>Actual expenditures: $3.24M · Variance: +3.2% · Burn rate: 89%</p>
                <canvas id="budgetDonutCanvas" style="margin-top: 30px;"></canvas>
            </div>
        </div>
    </div>

    <!-- PANEL 4: MEAL -->
    <div class="module-panel" id="panel4">
        <div class="panel-grid-2">
            <div>
                <div class="section-title"><i class="fas fa-tachometer-alt"></i> Core Dashboards</div>
                <ul style="line-height: 2;">
                    <li>📊 Results RAG & trend analysis</li>
                    <li>💰 Budget vs actual, cost per output</li>
                    <li>🧑‍🎓 Beneficiary: 52% girls, 7.3% CWD, 38% IDP</li>
                    <li>🛡️ Safeguarding & AAP dashboard</li>
                </ul>
                <div class="section-title" style="margin-top: 30px;"><i class="fas fa-clipboard-check"></i> Monitoring & DQA</div>
                <p>Accuracy, completeness, timeliness, reliability. Kobo/ODK, CPIMS.</p>
            </div>
            <div>
                <div class="section-title"><i class="fas fa-comment-dots"></i> Accountability (AAP)</div>
                <p>Complaints mechanisms: 42 feedback cases, 38 resolved. Child‑friendly channels active.</p>
                <div class="info-block" style="margin-top: 24px;">
                    <i class="fas fa-chart-line"></i> <strong>INEE EiE competencies</strong> integrated into staff monitoring.
                </div>
            </div>
        </div>
    </div>

    <!-- PANEL 5: REPORTING -->
    <div class="module-panel" id="panel5">
        <div class="panel-grid-2">
            <div>
                <div class="section-title"><i class="fas fa-file-alt"></i> Multi‑purpose Reporting</div>
                <p><strong>Donor (STEP‑GPE, UNICEF, ECW):</strong> results matrix, financial utilization.</p>
                <p><strong>Cluster:</strong> 4Ws/5Ws, monthly submissions.</p>
                <p><strong>Government:</strong> MoE‑aligned, EMIS‑compatible.</p>
                <div class="section-title" style="margin-top: 30px;"><i class="fas fa-rotate-left"></i> Learning & Adaptation</div>
                <p>Monthly reflection, mid‑term review, real‑time EiE adjustments.</p>
            </div>
            <div>
                <div class="section-title"><i class="fas fa-door-closed"></i> Close‑out & Sustainability</div>
                <p>Endline evaluation, financial closure, asset handover.</p>
                <p style="margin-top: 12px;">Capacity building, system integration, community ownership.</p>
                <div class="info-block" style="margin-top: 24px;">
                    ✅ STEP‑GPE perfect fit · Results‑based · Equity‑focused · Strong MEAL
                </div>
            </div>
        </div>
    </div>

    <!-- FOOTER -->
    <div class="system-footer">
        <div><i class="fas fa-book-open"></i> <strong>Learning integration:</strong> PM4DEV RBPM · UNICEF Module 5 · GPE · INEE · UNDP ToC · World Bank</div>
        <div class="footer-links">
            <span><i class="fas fa-file-excel"></i> Excel RB‑PMS</span>
            <span><i class="fas fa-chart-pie"></i> Power BI</span>
            <span><i class="fas fa-mobile-alt"></i> Kobo tools</span>
            <span><i class="fas fa-print"></i> SOPs</span>
        </div>
    </div>
</div>

<script>
    (function() {
        "use strict";

        // ----- TAB SYSTEM -----
        const tabs = document.querySelectorAll('.nav-tab');
        const panels = document.querySelectorAll('.module-panel');
        
        tabs.forEach(tab => {
            tab.addEventListener('click', (e) => {
                e.preventDefault();
                const targetId = tab.dataset.tab;
                tabs.forEach(t => t.classList.remove('active'));
                panels.forEach(p => p.classList.remove('active'));
                tab.classList.add('active');
                document.getElementById(targetId).classList.add('active');
            });
        });

        // ----- PROJECT CONFIGURATION (FULLY FUNCTIONAL) -----
        const projectSelect = document.getElementById('projectTypeSelect');
        const projectNameInput = document.getElementById('projectNameInput');
        const applyBtn = document.getElementById('applyConfigBtn');
        const saveTemplateBtn = document.getElementById('saveTemplateBtn');
        const exportBtn = document.getElementById('exportConfigBtn');
        const loadTemplateBtn = document.getElementById('loadTemplatePanelBtn');
        const activeDisplay = document.getElementById('activeTemplateDisplay');

        const templateMap = {
            'education': 'Education (STEP‑GPE) with GPE/UIS indicators & INEE overlay',
            'cp': 'Child Protection · CPMS/INEE Minimum Standards',
            'eie': 'EiE / Humanitarian · INEE MS · Cluster coordination',
            'wash': 'WASH · Sphere standards · UNICEF WASH indicators',
            'health': 'Health & Nutrition · WHO/UNICEF · IYCF indicators'
        };

        function updateActiveDisplay() {
            const type = projectSelect.value;
            const name = projectNameInput.value || 'Unnamed Project';
            activeDisplay.innerText = `${name} · ${templateMap[type] || templateMap.education}`;
        }

        applyBtn.addEventListener('click', (e) => {
            e.preventDefault();
            updateActiveDisplay();
            // Update indicator table based on selection (simulated)
            const indicatorBody = document.getElementById('indicatorTableBody');
            const type = projectSelect.value;
            let newRows = '';
            if (type === 'education') {
                newRows = `<tr><td>Impact</td><td>% children achieving minimum learning</td><td>42%</td><td>65%</td><td>58%</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>
                           <tr><td>Outcome</td><td># children accessing safe education</td><td>18.2k</td><td>35k</td><td>31.5k</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>`;
            } else if (type === 'cp') {
                newRows = `<tr><td>Impact</td><td>% children reporting feeling safe</td><td>68%</td><td>85%</td><td>74%</td><td><span class="status-badge"><span class="status-dot amber"></span>Moderate</span></td></tr>
                           <tr><td>Outcome</td><td># children accessing CP services</td><td>2,450</td><td>6,000</td><td>4,120</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>`;
            } else {
                newRows = `<tr><td>Impact</td><td>% children with improved well-being</td><td>55%</td><td>75%</td><td>63%</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>
                           <tr><td>Outcome</td><td># beneficiaries reached</td><td>12k</td><td>30k</td><td>22k</td><td><span class="status-badge"><span class="status-dot green"></span>On track</span></td></tr>`;
            }
            indicatorBody.innerHTML = newRows + indicatorBody.innerHTML.split('</tr>').slice(2).join('</tr>');
            alert(`✅ Project "${projectNameInput.value}" configured with ${projectSelect.options[projectSelect.selectedIndex].text} template.\nIndicators updated.`);
        });

        saveTemplateBtn.addEventListener('click', () => {
            const config = { type: projectSelect.value, name: projectNameInput.value };
            localStorage.setItem('rbpms_template', JSON.stringify(config));
            alert('💾 Configuration saved as template.');
        });

        exportBtn.addEventListener('click', () => {
            const config = { type: projectSelect.value, name: projectNameInput.value, timestamp: new Date().toISOString() };
            const blob = new Blob([JSON.stringify(config, null, 2)], {type: 'application/json'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url; a.download = `rbpms_config_${Date.now()}.json`; a.click();
            URL.revokeObjectURL(url);
        });

        loadTemplateBtn.addEventListener('click', (e) => {
            e.preventDefault();
            updateActiveDisplay();
            alert('📌 Indicator banks & ToC assumptions loaded based on selected project type.');
        });

        updateActiveDisplay();

        // ----- CHARTS -----
        // ToC trajectory
        new Chart(document.getElementById('tocChartCanvas'), {
            type: 'line',
            data: { labels: ['Baseline', 'Year 1', 'Year 2'], datasets: [{ label: 'Learning outcome %', data: [42, 58, 72], borderColor: '#0a4a6b', borderWidth: 3, tension: 0.2 }] },
            options: { responsive: true, plugins: { legend: { display: false } } }
        });

        // Indicator bar
        new Chart(document.getElementById('indicatorBarCanvas'), {
            type: 'bar',
            data: { labels: ['Impact', 'Outcome', 'Teachers', 'CP'], datasets: [{ data: [89, 90, 82, 78], backgroundColor: '#1a6a94', borderRadius: 8 }] },
            options: { indexAxis: 'y', responsive: true, plugins: { legend: { display: false } } }
        });

        // Budget donut
        new Chart(document.getElementById('budgetDonutCanvas'), {
            type: 'doughnut',
            data: { labels: ['Personnel', 'Training', 'Supplies', 'MEAL', 'Safeguarding'], datasets: [{ data: [1.2, 0.68, 0.42, 0.31, 0.09], backgroundColor: ['#0a4a6b', '#1a6a94', '#2a9d8f', '#4a9db5', '#7fb8d0'] }] },
            options: { plugins: { legend: { position: 'bottom' } } }
        });

        // Editable field simulation
        document.querySelectorAll('.editable-field').forEach(el => {
            el.addEventListener('click', () => alert('✏️ Edit mode: Activity breakdown can be customized.'));
        });
    })();
</script>
</body>
</html>
