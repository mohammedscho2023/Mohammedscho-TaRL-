<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaRL Department | English & Afaan Oromoo | Student Registration to Woreda/School Feed</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <script src="https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%); min-height: 100vh; padding: 20px; }
        .dashboard-container { max-width: 1600px; margin: 0 auto; }
        .header { background: rgba(255,255,255,0.95); border-radius: 24px; padding: 20px 30px; margin-bottom: 25px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }
        .logo h1 { font-size: 1.6rem; background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); -webkit-background-clip: text; background-clip: text; color: transparent; }
        .logo p { color: #64748b; font-size: 0.75rem; }
        .stats-badge { display: flex; gap: 12px; flex-wrap: wrap; }
        .badge { background: #f8fafc; padding: 8px 15px; border-radius: 14px; text-align: center; border: 1px solid #e2e8f0; }
        .badge .number { font-size: 1.3rem; font-weight: bold; color: #2563eb; }
        .badge .label { font-size: 0.65rem; color: #64748b; }
        .nav-tabs { display: flex; gap: 5px; background: rgba(255,255,255,0.95); border-radius: 50px; padding: 5px 15px; margin-bottom: 25px; flex-wrap: wrap; overflow-x: auto; }
        .nav-btn { padding: 8px 18px; border: none; background: transparent; font-size: 0.8rem; font-weight: 600; cursor: pointer; border-radius: 40px; transition: all 0.3s; color: #475569; white-space: nowrap; }
        .nav-btn:hover { background: #e2e8f0; }
        .nav-btn.active { background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); color: white; }
        .section { display: none; animation: fadeIn 0.3s ease; }
        .section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .card { background: white; border-radius: 20px; padding: 20px; margin-bottom: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); border: 1px solid #e2e8f0; }
        .card-title { font-size: 1.1rem; font-weight: 700; margin-bottom: 15px; display: flex; align-items: center; gap: 8px; color: #1e293b; border-left: 4px solid #2563eb; padding-left: 12px; }
        .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(450px, 1fr)); gap: 20px; }
        .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 15px; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: 600; font-size: 0.75rem; color: #334155; }
        input, select, textarea { width: 100%; padding: 8px 12px; border: 2px solid #e2e8f0; border-radius: 10px; font-size: 0.8rem; font-family: inherit; }
        input:focus, select:focus { outline: none; border-color: #2563eb; }
        .btn { padding: 8px 16px; border: none; border-radius: 10px; font-weight: 600; cursor: pointer; transition: all 0.2s; font-size: 0.75rem; display: inline-flex; align-items: center; gap: 6px; }
        .btn-primary { background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); color: white; }
        .btn-primary:hover { transform: translateY(-2px); }
        .btn-secondary { background: #f1f5f9; color: #334155; border: 1px solid #e2e8f0; }
        .btn-danger { background: #fee2e2; color: #dc2626; }
        .btn-success { background: #d1fae5; color: #059669; }
        .btn-sm { padding: 4px 8px; font-size: 0.65rem; }
        .data-table { width: 100%; border-collapse: collapse; font-size: 0.7rem; overflow-x: auto; display: block; }
        .data-table th, .data-table td { padding: 8px 6px; text-align: left; border-bottom: 1px solid #e2e8f0; }
        .data-table th { background: #f8fafc; font-weight: 700; color: #1e293b; position: sticky; top: 0; }
        .data-table tr:hover { background: #f8fafc; }
        .progress-bar-container { background: #e2e8f0; border-radius: 20px; overflow: hidden; height: 6px; }
        .progress-bar { height: 100%; border-radius: 20px; transition: width 0.5s ease; }
        .progress-high { background: #10b981; }
        .progress-medium { background: #f59e0b; }
        .progress-low { background: #ef4444; }
        .alert { padding: 10px 14px; border-radius: 10px; margin-bottom: 15px; font-size: 0.75rem; }
        .alert-info { background: #dbeafe; color: #1e40af; border-left: 4px solid #2563eb; }
        .alert-success { background: #d1fae5; color: #065f46; border-left: 4px solid #10b981; }
        .formula-box { background: #fef3c7; padding: 8px; border-radius: 8px; font-family: monospace; font-size: 0.7rem; margin-top: 8px; }
        .footer { text-align: center; padding: 20px; color: rgba(255,255,255,0.5); font-size: 0.7rem; }
        .level-badge { display: inline-block; padding: 2px 10px; border-radius: 20px; font-size: 0.65rem; font-weight: 600; }
        .level-1 { background: #fee2e2; color: #991b1b; }
        .level-2 { background: #fed7aa; color: #9a3412; }
        .level-3 { background: #fef3c7; color: #92400e; }
        .level-4 { background: #d1fae5; color: #065f46; }
        .level-5 { background: #dbeafe; color: #1e40af; }
        .level-6 { background: #e0e7ff; color: #3730a3; }
        .group-card { background: #f8fafc; border-radius: 12px; padding: 15px; margin-bottom: 15px; border-left: 4px solid #2563eb; }
        @media (max-width: 768px) { .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; } .data-table { font-size: 0.55rem; } }
    </style>
</head>
<body>
<div class="dashboard-container">
    <div class="header">
        <div class="logo"><h1><i class="fas fa-chalkboard-user"></i> TaRL Department</h1><p>Teaching at the Right Level | Student Registration → Woreda/School Feed | Grouping by Grade & Level</p></div>
        <div class="stats-badge">
            <div class="badge"><div class="number" id="totalStudents">0</div><div class="label">Total Students</div></div>
            <div class="badge"><div class="number" id="totalSchools">0</div><div class="label">Schools</div></div>
            <div class="badge"><div class="number" id="avgEnglish">0%</div><div class="label">Avg English</div></div>
            <div class="badge"><div class="number" id="avgOromo">0%</div><div class="label">Avg Afaan Oromoo</div></div>
        </div>
    </div>

    <div class="nav-tabs">
        <button class="nav-btn active" data-section="dashboard"><i class="fas fa-chart-pie"></i> Dashboard</button>
        <button class="nav-btn" data-section="studentReg"><i class="fas fa-user-plus"></i> Student Registration</button>
        <button class="nav-btn" data-section="grouping"><i class="fas fa-layer-group"></i> Grouping by Grade & Level</button>
        <button class="nav-btn" data-section="woreda"><i class="fas fa-map-marker-alt"></i> Woreda Level (Auto-Feed)</button>
        <button class="nav-btn" data-section="school"><i class="fas fa-school"></i> School Level (Auto-Feed)</button>
        <button class="nav-btn" data-section="reports"><i class="fas fa-file-alt"></i> Reports</button>
        <button class="nav-btn" data-section="settings"><i class="fas fa-cog"></i> Settings</button>
    </div>

    <!-- DASHBOARD SECTION -->
    <div id="dashboard" class="section active">
        <div class="grid-3">
            <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> English Progress by Grade</div><div class="chart-container" style="height:250px;"><canvas id="englishGradeChart"></canvas></div></div>
            <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> Afaan Oromoo Progress by Grade</div><div class="chart-container" style="height:250px;"><canvas id="oromoGradeChart"></canvas></div></div>
            <div class="card"><div class="card-title"><i class="fas fa-chart-pie"></i> Student Distribution by Level</div><div class="chart-container" style="height:250px;"><canvas id="levelDistributionChart"></canvas></div></div>
        </div>
        <div class="card"><div class="card-title"><i class="fas fa-tachometer-alt"></i> TaRL Program Summary</div><div id="programSummary"></div></div>
    </div>

    <!-- STUDENT REGISTRATION SECTION - Feeds to Woreda & School -->
    <div id="studentReg" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-user-plus"></i> Register Student (Barataa) - Data Feeds to Woreda & School Levels</div>
            <div class="grid-3">
                <div class="form-group"><label>Zone (Godina)</label><input type="text" id="studentZone" placeholder="e.g., East Wolega"></div>
                <div class="form-group"><label>Woreda (Aanaa)</label><input type="text" id="studentWoreda" placeholder="e.g., Leka Dulecha"></div>
                <div class="form-group"><label>School ID</label><input type="text" id="studentSchoolId" placeholder="School ID"></div>
                <div class="form-group"><label>School Name</label><input type="text" id="studentSchoolName" placeholder="School Name"></div>
                <div class="form-group"><label>Learner ID</label><input type="text" id="learnerId" placeholder="Auto-generated" readonly></div>
                <div class="form-group"><label>Student Name (Maqaa Barataa)</label><input type="text" id="studentName" placeholder="Full Name"></div>
                <div class="form-group"><label>Sex</label><select id="studentSex"><option value="M">Dhiirra (Male)</option><option value="F">Dubartii (Female)</option></select></div>
                <div class="form-group"><label>Grade (Kutaa)</label><select id="studentGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                <div class="form-group"><label>Special Needs (Fedhii Addaa)</label><select id="specialNeeds"><option value="None">None</option><option value="Vision">Rakkoo Arguu (Vision)</option><option value="Hearing">Rakkoo Dhaga'uu (Hearing)</option><option value="Physical">Hirdhiina Qaamaa (Physical)</option><option value="Communication">Rakkoo Waliifgaluu (Communication)</option><option value="Cognitive">Hanqiina Guddina Sammu (Cognitive)</option><option value="Behavioral">Rakkoo Amalaa (Behavioral)</option></select></div>
            </div>
            <div class="grid-2">
                <!-- ENGLISH ASSESSMENT - 5 components (Beginners instead of Initial) -->
                <div><h4><i class="fas fa-language"></i> English Assessment (5 Components)</h4>
                    <div class="form-group"><label>Beginners (Gulantaa Jalqabaa / Baseline)</label><input type="number" id="studentEngBeginners" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Arfii (Alphabet / Letter Recognition)</label><input type="number" id="studentEngAlphabet" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Jechoota (Word Recognition / Vocabulary)</label><input type="number" id="studentEngWords" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Simple Sentence Reading)</label><input type="number" id="studentEngSentence" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Seenessaa (Story / Reading Comprehension)</label><input type="number" id="studentEngStory" placeholder="0/1" step="1" min="0" max="1"></div>
                </div>
                <!-- AFAAN OROMOO ASSESSMENT - 5 components (Beginners instead of Initial) -->
                <div><h4><i class="fas fa-book"></i> Afaan Oromoo Assessment (5 Components)</h4>
                    <div class="form-group"><label>Beginners (Gulantaa Jalqabaa / Baseline)</label><input type="number" id="studentOromoBeginners" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Qubee (Alphabet Recognition)</label><input type="number" id="studentOromoQubee" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Jecha (Word Recognition)</label><input type="number" id="studentOromoWord" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Keeyyata Salphaa (Simple Sentence)</label><input type="number" id="studentOromoSentence" placeholder="0/1" step="1" min="0" max="1"></div>
                    <div class="form-group"><label>Seenessaa (Story / Comprehension)</label><input type="number" id="studentOromoStory" placeholder="0/1" step="1" min="0" max="1"></div>
                </div>
            </div>
            <button class="btn btn-primary" onclick="registerStudent()"><i class="fas fa-save"></i> Register Student (Auto-Feeds to Woreda & School)</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-table-list"></i> Registered Students</div>
            <div style="overflow-x: auto; max-height: 400px; overflow-y: auto;"><table class="data-table" id="studentTable"><thead><tr><th>ID</th><th>Name</th><th>Zone</th><th>Woreda</th><th>School</th><th>Grade</th><th>Sex</th><th>English</th><th>Oromo</th><th>English Level</th><th>Oromo Level</th><th>Actions</th></tr></thead><tbody id="studentTableBody"></tbody></table></div>
        </div>
    </div>

    <!-- GROUPING BY GRADE & LEVEL SECTION -->
    <div id="grouping" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-layer-group"></i> Grouping by Grade & TaRL Level (Per TaRL Guideline)</div>
            <div class="form-group"><label>Select Grade</label><select id="groupGradeSelect"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
            <button class="btn btn-primary" onclick="updateGrouping()"><i class="fas fa-chart-line"></i> Show Grouping</button>
            <div id="groupingDisplay" style="margin-top: 20px;"></div>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-chart-simple"></i> TaRL Level Distribution Summary</div>
            <div id="levelSummary"></div>
        </div>
    </div>

    <!-- WOREDA LEVEL SECTION (Auto-populated from Student Registration) -->
    <div id="woreda" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-map-marker-alt"></i> Woreda Level Summary (Auto-Generated from Student Data)</div>
            <div style="overflow-x: auto;"><table class="data-table" id="woredaTable"><thead><tr><th>Zone</th><th>Woreda</th><th>Phase</th><th>Grade</th><th>Total Students</th><th>English Score</th><th>English %</th><th>Oromo Score</th><th>Oromo %</th></tr></thead><tbody id="woredaTableBody"></tbody>}</div>
            <div class="alert alert-info" style="margin-top:15px;"><i class="fas fa-info-circle"></i> This data is automatically aggregated from student registrations. When you register a student, the woreda and school level data updates automatically.</div>
        </div>
    </div>

    <!-- SCHOOL LEVEL SECTION (Auto-populated from Student Registration) -->
    <div id="school" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-school"></i> School Level Summary (Auto-Generated from Student Data)</div>
            <div style="overflow-x: auto;"><table class="data-table" id="schoolTable"><thead><tr><th>Zone</th><th>Woreda</th><th>School Name</th><th>Grade</th><th>Total Students</th><th>English Score</th><th>English %</th><th>Oromo Score</th><th>Oromo %</th></tr></thead><tbody id="schoolTableBody"></tbody>}</div>
        </div>
    </div>

    <!-- REPORTS SECTION -->
    <div id="reports" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-file-pdf"></i> Generate TaRL Report</div>
            <div class="grid-2">
                <div><div class="form-group"><label>Report Type</label><select id="reportType"><option value="woreda">Woreda Level Summary</option><option value="school">School Level Summary</option><option value="student">Student Roster</option><option value="grouping">Grouping by Level</option></select></div></div>
                <div><div class="form-group"><label>Format</label><select id="reportFormat"><option value="html">HTML Report</option><option value="pdf">PDF Report</option></select></div></div>
            </div>
            <button class="btn btn-primary" onclick="generateTaRLReport()"><i class="fas fa-download"></i> Generate Report</button>
        </div>
    </div>

    <!-- SETTINGS SECTION -->
    <div id="settings" class="section">
        <div class="card">
            <div class="card-title"><i class="fas fa-database"></i> Data Management</div>
            <button class="btn btn-secondary" onclick="exportAllData()"><i class="fas fa-download"></i> Export All Data (JSON)</button>
            <button class="btn btn-danger" onclick="clearAllData()"><i class="fas fa-trash"></i> Clear All Data</button>
            <button class="btn btn-success" onclick="loadSampleData()"><i class="fas fa-chalkboard"></i> Load Sample Student Data</button>
        </div>
        <div class="card">
            <div class="card-title"><i class="fas fa-info-circle"></i> TaRL Guidelines Reference</div>
            <div class="grid-2">
                <div><strong>English Levels (6 Levels)</strong><br>
                    1️⃣ Beginner (0 points)<br>
                    2️⃣ Alphabet (1 point)<br>
                    3️⃣ Words (2 points)<br>
                    4️⃣ Sentence (3 points)<br>
                    5️⃣ Story (4 points)<br>
                    6️⃣ Computed (5 points)
                </div>
                <div><strong>Afaan Oromoo Levels (6 Levels)</strong><br>
                    1️⃣ Beginner (0 points)<br>
                    2️⃣ Qubee (1 point)<br>
                    3️⃣ Jecha (2 points)<br>
                    4️⃣ Keeyyata (3 points)<br>
                    5️⃣ Seenessaa (4 points)<br>
                    6️⃣ Computed (5 points)
                </div>
            </div>
            <div class="formula-box" style="margin-top:15px;"><strong>Formula:</strong> English Total = Beginners + Arfii + Jechoota + Keeyyata Salphaa + Seenessaa (Max 5)<br>
            <strong>Formula:</strong> Afaan Oromoo Total = Beginners + Qubee + Jecha + Keeyyata Salphaa + Seenessaa (Max 5)</div>
        </div>
    </div>
    <div class="footer"><p>TaRL Department | Student Registration Auto-Feeds to Woreda & School Levels | Grouping by Grade & Level per TaRL Guideline</p></div>
</div>

<script>
    // Data Structures
    let students = [];

    function saveData() {
        localStorage.setItem('tarl_students', JSON.stringify(students));
    }

    function loadData() {
        students = JSON.parse(localStorage.getItem('tarl_students') || '[]');
        updateAll();
    }

    function updateAll() {
        updateStats();
        updateStudentTable();
        updateWoredaTable();
        updateSchoolTable();
        updateDashboardCharts();
        updateProgramSummary();
        updateGrouping();
    }

    function updateStats() {
        document.getElementById('totalStudents').textContent = students.length;
        const uniqueSchools = [...new Set(students.map(s => s.schoolName))];
        document.getElementById('totalSchools').textContent = uniqueSchools.length;
        let engTotal = 0, oromoTotal = 0;
        students.forEach(s => { engTotal += s.englishScore || 0; oromoTotal += s.oromoScore || 0; });
        document.getElementById('avgEnglish').textContent = students.length ? ((engTotal / (students.length * 5)) * 100).toFixed(0) : '0';
        document.getElementById('avgOromo').textContent = students.length ? ((oromoTotal / (students.length * 5)) * 100).toFixed(0) : '0';
    }

    function getEnglishLevel(score) {
        if(score === 0) return { name: 'Beginner', class: 'level-1', code: 1 };
        if(score === 1) return { name: 'Alphabet', class: 'level-2', code: 2 };
        if(score === 2) return { name: 'Words', class: 'level-3', code: 3 };
        if(score === 3) return { name: 'Sentence', class: 'level-4', code: 4 };
        if(score === 4) return { name: 'Story', class: 'level-5', code: 5 };
        return { name: 'Computed', class: 'level-6', code: 6 };
    }

    function getOromoLevel(score) {
        if(score === 0) return { name: 'Beginner', class: 'level-1', code: 1 };
        if(score === 1) return { name: 'Qubee', class: 'level-2', code: 2 };
        if(score === 2) return { name: 'Jecha', class: 'level-3', code: 3 };
        if(score === 3) return { name: 'Keeyyata', class: 'level-4', code: 4 };
        if(score === 4) return { name: 'Seenessaa', class: 'level-5', code: 5 };
        return { name: 'Computed', class: 'level-6', code: 6 };
    }

    function registerStudent() {
        const learnerId = 'LRN_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4).toUpperCase();
        document.getElementById('learnerId').value = learnerId;
        
        const englishScore = (parseInt(document.getElementById('studentEngBeginners').value)||0) + 
                             (parseInt(document.getElementById('studentEngAlphabet').value)||0) + 
                             (parseInt(document.getElementById('studentEngWords').value)||0) + 
                             (parseInt(document.getElementById('studentEngSentence').value)||0) + 
                             (parseInt(document.getElementById('studentEngStory').value)||0);
        
        const oromoScore = (parseInt(document.getElementById('studentOromoBeginners').value)||0) + 
                           (parseInt(document.getElementById('studentOromoQubee').value)||0) + 
                           (parseInt(document.getElementById('studentOromoWord').value)||0) + 
                           (parseInt(document.getElementById('studentOromoSentence').value)||0) + 
                           (parseInt(document.getElementById('studentOromoStory').value)||0);
        
        const englishLevel = getEnglishLevel(englishScore);
        const oromoLevel = getOromoLevel(oromoScore);
        
        const newStudent = {
            id: learnerId,
            zone: document.getElementById('studentZone').value,
            woreda: document.getElementById('studentWoreda').value,
            schoolId: document.getElementById('studentSchoolId').value,
            schoolName: document.getElementById('studentSchoolName').value,
            name: document.getElementById('studentName').value,
            sex: document.getElementById('studentSex').value,
            grade: document.getElementById('studentGrade').value,
            specialNeeds: document.getElementById('specialNeeds').value,
            englishScore: englishScore,
            oromoScore: oromoScore,
            englishLevel: englishLevel,
            oromoLevel: oromoLevel,
            engComponents: {
                beginners: parseInt(document.getElementById('studentEngBeginners').value)||0,
                alphabet: parseInt(document.getElementById('studentEngAlphabet').value)||0,
                words: parseInt(document.getElementById('studentEngWords').value)||0,
                sentence: parseInt(document.getElementById('studentEngSentence').value)||0,
                story: parseInt(document.getElementById('studentEngStory').value)||0
            },
            oromoComponents: {
                beginners: parseInt(document.getElementById('studentOromoBeginners').value)||0,
                qubee: parseInt(document.getElementById('studentOromoQubee').value)||0,
                word: parseInt(document.getElementById('studentOromoWord').value)||0,
                sentence: parseInt(document.getElementById('studentOromoSentence').value)||0,
                story: parseInt(document.getElementById('studentOromoStory').value)||0
            }
        };
        
        students.push(newStudent);
        saveData();
        updateAll();
        alert(`Student registered! ID: ${learnerId}\n\nEnglish Score: ${englishScore}/5 (${englishLevel.name})\nAfaan Oromoo Score: ${oromoScore}/5 (${oromoLevel.name})`);
        clearStudentForm();
    }

    function clearStudentForm() {
        ['studentZone','studentWoreda','studentSchoolId','studentSchoolName','studentName',
         'studentEngBeginners','studentEngAlphabet','studentEngWords','studentEngSentence','studentEngStory',
         'studentOromoBeginners','studentOromoQubee','studentOromoWord','studentOromoSentence','studentOromoStory',
         'learnerId'].forEach(id => { if(document.getElementById(id)) document.getElementById(id).value = ''; });
    }

    function updateStudentTable() {
        const tbody = document.getElementById('studentTableBody');
        if(students.length === 0) { tbody.innerHTML = '<tr><td colspan="12" style="text-align:center;">No students registered. Add above.</td></tr>'; return; }
        tbody.innerHTML = '';
        students.forEach(s => {
            tbody.innerHTML += `<tr>
                <td>${s.id.substring(0,12)}</td><td>${escapeHtml(s.name)}</td><td>${escapeHtml(s.zone)}</td>
                <td>${escapeHtml(s.woreda)}</td><td>${escapeHtml(s.schoolName)}</td><td>Kutaa ${s.grade}</td>
                <td>${s.sex === 'M' ? 'Dhiirra' : 'Dubartii'}</td>
                <td>${s.englishScore}/5 <span class="level-badge ${s.englishLevel.class}">${s.englishLevel.name}</span></td>
                <td>${s.oromoScore}/5 <span class="level-badge ${s.oromoLevel.class}">${s.oromoLevel.name}</span></td>
                <td><span class="level-badge ${s.englishLevel.class}">${s.englishLevel.name}</span></td>
                <td><span class="level-badge ${s.oromoLevel.class}">${s.oromoLevel.name}</span></td>
                <td><button class="btn btn-danger btn-sm" onclick="deleteStudent('${s.id}')"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
    }

    function updateWoredaTable() {
        const tbody = document.getElementById('woredaTableBody');
        const woredaMap = new Map();
        
        students.forEach(s => {
            const key = `${s.zone}|${s.woreda}|${s.grade}`;
            if(!woredaMap.has(key)) {
                woredaMap.set(key, { zone: s.zone, woreda: s.woreda, grade: s.grade, phase: 'baseline', count: 0, engTotal: 0, oromoTotal: 0 });
            }
            const data = woredaMap.get(key);
            data.count++;
            data.engTotal += s.englishScore;
            data.oromoTotal += s.oromoScore;
        });
        
        if(woredaMap.size === 0) { tbody.innerHTML = '<tr><td colspan="9" style="text-align:center;">No data - Register students first.</td></tr>'; return; }
        tbody.innerHTML = '';
        for(let data of woredaMap.values()) {
            const engPercent = (data.engTotal / (data.count * 5)) * 100;
            const oromoPercent = (data.oromoTotal / (data.count * 5)) * 100;
            tbody.innerHTML += `<tr>
                <td>${escapeHtml(data.zone)}</td><td>${escapeHtml(data.woreda)}</td><td>${data.phase}</td>
                <td>Kutaa ${data.grade}</td><td>${data.count}</td>
                <td>${data.engTotal}/${data.count*5}</td><td>${engPercent.toFixed(1)}%</td>
                <td>${data.oromoTotal}/${data.count*5}</td><td>${oromoPercent.toFixed(1)}%</td>
            </tr>`;
        }
    }

    function updateSchoolTable() {
        const tbody = document.getElementById('schoolTableBody');
        const schoolMap = new Map();
        
        students.forEach(s => {
            const key = `${s.zone}|${s.woreda}|${s.schoolName}|${s.grade}`;
            if(!schoolMap.has(key)) {
                schoolMap.set(key, { zone: s.zone, woreda: s.woreda, schoolName: s.schoolName, grade: s.grade, count: 0, engTotal: 0, oromoTotal: 0 });
            }
            const data = schoolMap.get(key);
            data.count++;
            data.engTotal += s.englishScore;
            data.oromoTotal += s.oromoScore;
        });
        
        if(schoolMap.size === 0) { tbody.innerHTML = '<tr><td colspan="9" style="text-align:center;">No data - Register students first.</td></tr>'; return; }
        tbody.innerHTML = '';
        for(let data of schoolMap.values()) {
            const engPercent = (data.engTotal / (data.count * 5)) * 100;
            const oromoPercent = (data.oromoTotal / (data.count * 5)) * 100;
            tbody.innerHTML += `<tr>
                <td>${escapeHtml(data.zone)}</td>
                <td>${escapeHtml(data.woreda)}</td>
                <td>${escapeHtml(data.schoolName)}</td>
                <td>Kutaa ${data.grade}</td>
                <td>${data.count}</td>
                <td>${data.engTotal}/${data.count*5}</td>
                <td>${engPercent.toFixed(1)}%</td>
                <td>${data.oromoTotal}/${data.count*5}</td>
                <td>${oromoPercent.toFixed(1)}%</td>
            </tr>`;
        }
    }

    function updateGrouping() {
        const selectedGrade = document.getElementById('groupGradeSelect').value;
        const gradeStudents = students.filter(s => s.grade === selectedGrade);
        const container = document.getElementById('groupingDisplay');
        
        if(gradeStudents.length === 0) {
            container.innerHTML = `<div class="alert alert-info">No students registered for Kutaa ${selectedGrade}</div>`;
            return;
        }
        
        // Group by English Level
        const englishGroups = { 1: [], 2: [], 3: [], 4: [], 5: [], 6: [] };
        const oromoGroups = { 1: [], 2: [], 3: [], 4: [], 5: [], 6: [] };
        
        gradeStudents.forEach(s => {
            englishGroups[s.englishLevel.code].push(s);
            oromoGroups[s.oromoLevel.code].push(s);
        });
        
        const levelNames = { 1: 'Beginner', 2: 'Alphabet/Qubee', 3: 'Words/Jecha', 4: 'Sentence/Keeyyata', 5: 'Story/Seenessaa', 6: 'Computed' };
        
        let html = `<div class="grid-2"><div><h4><i class="fas fa-language"></i> English Grouping (Kutaa ${selectedGrade})</h4>`;
        for(let i=1; i<=6; i++) {
            if(englishGroups[i].length > 0) {
                html += `<div class="group-card"><strong>Level ${i}: ${levelNames[i]}</strong> (${englishGroups[i].length} students)<br>`;
                html += `<small>Students: ${englishGroups[i].map(s => s.name).join(', ')}</small></div>`;
            }
        }
        html += `</div><div><h4><i class="fas fa-book"></i> Afaan Oromoo Grouping (Kutaa ${selectedGrade})</h4>`;
        for(let i=1; i<=6; i++) {
            if(oromoGroups[i].length > 0) {
                html += `<div class="group-card"><strong>Level ${i}: ${levelNames[i]}</strong> (${oromoGroups[i].length} students)<br>`;
                html += `<small>Students: ${oromoGroups[i].map(s => s.name).join(', ')}</small></div>`;
            }
        }
        html += `</div></div>`;
        container.innerHTML = html;
        
        // Update level summary
        const levelSummary = document.getElementById('levelSummary');
        let engLevelCount = {1:0,2:0,3:0,4:0,5:0,6:0};
        let oromoLevelCount = {1:0,2:0,3:0,4:0,5:0,6:0};
        students.forEach(s => {
            engLevelCount[s.englishLevel.code]++;
            oromoLevelCount[s.oromoLevel.code]++;
        });
        levelSummary.innerHTML = `<div class="grid-2">
            <div class="alert alert-info"><strong>📊 English Level Distribution</strong><br>
            Beginner: ${engLevelCount[1]} | Alphabet: ${engLevelCount[2]} | Words: ${engLevelCount[3]}<br>
            Sentence: ${engLevelCount[4]} | Story: ${engLevelCount[5]} | Computed: ${engLevelCount[6]}
            </div>
            <div class="alert alert-success"><strong>📊 Afaan Oromoo Level Distribution</strong><br>
            Beginner: ${oromoLevelCount[1]} | Qubee: ${oromoLevelCount[2]} | Jecha: ${oromoLevelCount[3]}<br>
            Keeyyata: ${oromoLevelCount[4]} | Seenessaa: ${oromoLevelCount[5]} | Computed: ${oromoLevelCount[6]}
            </div>
        </div>`;
    }

    function updateDashboardCharts() {
        const grades = ['3', '4', '5'];
        const engData = grades.map(g => {
            const gradeStudents = students.filter(s => s.grade === g);
            if(gradeStudents.length === 0) return 0;
            const total = gradeStudents.reduce((sum, s) => sum + s.englishScore, 0);
            return (total / (gradeStudents.length * 5)) * 100;
        });
        const oromoData = grades.map(g => {
            const gradeStudents = students.filter(s => s.grade === g);
            if(gradeStudents.length === 0) return 0;
            const total = gradeStudents.reduce((sum, s) => sum + s.oromoScore, 0);
            return (total / (gradeStudents.length * 5)) * 100;
        });
        
        if(window.engGradeChart) window.engGradeChart.destroy();
        window.engGradeChart = new Chart(document.getElementById('englishGradeChart'), { 
            type: 'bar', data: { labels: ['Kutaa 3', 'Kutaa 4', 'Kutaa 5'], datasets: [{ label: 'English (%)', data: engData, backgroundColor: '#3b82f6' }] } 
        });
        if(window.oromoGradeChart) window.oromoGradeChart.destroy();
        window.oromoGradeChart = new Chart(document.getElementById('oromoGradeChart'), { 
            type: 'bar', data: { labels: ['Kutaa 3', 'Kutaa 4', 'Kutaa 5'], datasets: [{ label: 'Afaan Oromoo (%)', data: oromoData, backgroundColor: '#10b981' }] } 
        });
        
        const levelCounts = [0,0,0,0,0,0];
        students.forEach(s => { levelCounts[s.englishLevel.code-1]++; });
        if(window.levelChart) window.levelChart.destroy();
        window.levelChart = new Chart(document.getElementById('levelDistributionChart'), { 
            type: 'pie', data: { labels: ['Beginner', 'Alphabet', 'Words', 'Sentence', 'Story', 'Computed'], datasets: [{ data: levelCounts, backgroundColor: ['#ef4444','#f59e0b','#fef3c7','#d1fae5','#dbeafe','#e0e7ff'] }] } 
        });
    }

    function updateProgramSummary() {
        const total = students.length;
        const males = students.filter(s => s.sex === 'M').length;
        const females = students.filter(s => s.sex === 'F').length;
        const cwd = students.filter(s => s.specialNeeds !== 'None').length;
        const avgEng = total ? (students.reduce((s, st) => s + st.englishScore, 0) / total) * 20 : 0;
        const avgOromo = total ? (students.reduce((s, st) => s + st.oromoScore, 0) / total) * 20 : 0;
        document.getElementById('programSummary').innerHTML = `<div class="grid-3">
            <div class="alert alert-info"><strong>📊 Enrollment Summary</strong><br>Total: ${total}<br>Male: ${males}<br>Female: ${females}<br>CWD: ${cwd}</div>
            <div class="alert alert-success"><strong>📈 Average Scores</strong><br>English: ${avgEng.toFixed(1)}%<br>Afaan Oromoo: ${avgOromo.toFixed(1)}%</div>
            <div class="alert alert-info"><strong>🏫 Coverage</strong><br>Woredas: ${[...new Set(students.map(s=>s.woreda))].length}<br>Schools: ${[...new Set(students.map(s=>s.schoolName))].length}</div>
        </div>`;
    }

    function deleteStudent(id) { if(confirm('Delete student? This will update woreda/school reports.')) { students = students.filter(s => s.id !== id); saveData(); updateAll(); } }

    function generateTaRLReport() {
        const type = document.getElementById('reportType').value;
        const format = document.getElementById('reportFormat').value;
        let html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>TaRL Program Report</title><style>body{font-family:Arial;margin:40px;}h1{color:#2563eb;}table{border-collapse:collapse;width:100%;}th,td{border:1px solid #ddd;padding:8px;}th{background:#f0f0f0;}</style></head><body><h1>📊 TaRL Program Report</h1><p>English & Afaan Oromoo | Generated: ${new Date().toLocaleString()}</p>`;
        
        if(type === 'woreda') {
            const woredaMap = new Map();
            students.forEach(s => {
                const key = `${s.zone}|${s.woreda}|${s.grade}`;
                if(!woredaMap.has(key)) woredaMap.set(key, { zone: s.zone, woreda: s.woreda, grade: s.grade, count: 0, engTotal: 0, oromoTotal: 0 });
                const d = woredaMap.get(key);
                d.count++; d.engTotal += s.englishScore; d.oromoTotal += s.oromoScore;
            });
            html += `<h2>Woreda Level Summary</h2><table><th>Zone</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Afaan Oromoo %</th>`;
            for(let d of woredaMap.values()) {
                html += `<tr><td>${d.zone}</td><td>${d.woreda}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td><td>${((d.engTotal/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oromoTotal/(d.count*5))*100).toFixed(1)}%</td></tr>`;
            }
            html += `</table>`;
        } else if(type === 'school') {
            const schoolMap = new Map();
            students.forEach(s => {
                const key = `${s.schoolName}|${s.grade}`;
                if(!schoolMap.has(key)) schoolMap.set(key, { school: s.schoolName, woreda: s.woreda, grade: s.grade, count: 0, engTotal: 0, oromoTotal: 0 });
                const d = schoolMap.get(key);
                d.count++; d.engTotal += s.englishScore; d.oromoTotal += s.oromoScore;
            });
            html += `<h2>School Level Summary</h2><table><th>School</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Afaan Oromoo %</th>`;
            for(let d of schoolMap.values()) {
                html += `<tr><td>${d.school}</td><td>${d.woreda}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td><td>${((d.engTotal/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oromoTotal/(d.count*5))*100).toFixed(1)}%</td></tr>`;
            }
            html += `</table>`;
        } else if(type === 'grouping') {
            html += `<h2>Student Grouping by Level</h2>`;
            for(let g of ['3','4','5']) {
                const gradeStudents = students.filter(s => s.grade === g);
                html += `<h3>Kutaa ${g}</h3>`;
                for(let lvl=1; lvl<=6; lvl++) {
                    const levelStudents = gradeStudents.filter(s => s.englishLevel.code === lvl);
                    if(levelStudents.length > 0) {
                        html += `<p><strong>Level ${lvl}: ${levelStudents[0].englishLevel.name}</strong> - ${levelStudents.length} students<br><small>${levelStudents.map(s=>s.name).join(', ')}</small></p>`;
                    }
                }
            }
        } else {
            html += `<h2>Student Roster</h2><tr><th>Name</th><th>School</th><th>Grade</th><th>English Score</th><th>English Level</th><th>Oromo Score</th><th>Oromo Level</th>`;
            students.forEach(s => {
                html += `<tr><td>${s.name}</td><td>${s.schoolName}</td><td>Kutaa ${s.grade}</td><td>${s.englishScore}/5</td><td>${s.englishLevel.name}</td><td>${s.oromoScore}/5</td><td>${s.oromoLevel.name}</td></tr>`;
            });
            html += `</table>`;
        }
        html += `<h3>📐 Formulas Used</h3><p>English Total = Beginners + Arfii + Jechoota + Keeyyata Salphaa + Seenessaa (Max 5)</p><p>Afaan Oromoo Total = Beginners + Qubee + Jecha + Keeyyata Salphaa + Seenessaa (Max 5)</p></body></html>`;
        
        if(format === 'html') { const w=window.open(); w.document.write(html); w.document.close(); }
        else { const div=document.createElement('div'); div.innerHTML=html; document.body.appendChild(div); html2pdf().from(div).set({margin:1}).save(); setTimeout(()=>document.body.removeChild(div),1000); }
    }

    function loadSampleData() {
        students = [
            { id:'STU1', zone:'East Wolega', woreda:'Leka Dulecha', schoolId:'001', schoolName:"Ka'umsa Primary School", name:'Almaz Tesfaye', sex:'F', grade:'3', specialNeeds:'None', englishScore:1, oromoScore:2, englishLevel:{name:'Alphabet',class:'level-2',code:2}, oromoLevel:{name:'Jecha',class:'level-3',code:3} },
            { id:'STU2', zone:'East Wolega', woreda:'Leka Dulecha', schoolId:'001', schoolName:"Ka'umsa Primary School", name:'Biruk Abebe', sex:'M', grade:'3', specialNeeds:'None', englishScore:0, oromoScore:1, englishLevel:{name:'Beginner',class:'level-1',code:1}, oromoLevel:{name:'Qubee',class:'level-2',code:2} },
            { id:'STU3', zone:'West Wolega', woreda:'Gimbi', schoolId:'002', schoolName:'Chuta Goch Primary School', name:'Chaltu Hussen', sex:'F', grade:'4', specialNeeds:'None', englishScore:3, oromoScore:4, englishLevel:{name:'Sentence',class:'level-4',code:4}, oromoLevel:{name:'Keeyyata',class:'level-4',code:4} },
            { id:'STU4', zone:'West Wolega', woreda:'Gimbi', schoolId:'002', schoolName:'Chuta Goch Primary School', name:'Dawit Getachew', sex:'M', grade:'5', specialNeeds:'Vision', englishScore:4, oromoScore:5, englishLevel:{name:'Story',class:'level-5',code:5}, oromoLevel:{name:'Seenessaa',class:'level-5',code:5} }
        ];
        saveData(); updateAll(); alert('Sample student data loaded! Woreda and School reports auto-generated.');
    }

    function exportAllData() { const a=document.createElement('a'); a.href=URL.createObjectURL(new Blob([JSON.stringify({students},null,2)],{type:'application/json'})); a.download=`tarl_data.json`; a.click(); }
    function clearAllData() { if(confirm('Delete ALL TaRL data?')) { localStorage.clear(); students=[]; updateAll(); alert('All data cleared.'); } }
    function escapeHtml(str){ if(!str) return ''; return str.replace(/[&<>]/g, m => m==='&'?'&amp;':m==='<'?'&lt;':'&gt;'); }
    document.querySelectorAll('.nav-btn').forEach(btn=>{ btn.addEventListener('click',()=>{ document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); document.querySelectorAll('.section').forEach(s=>s.classList.remove('active')); document.getElementById(btn.dataset.section).classList.add('active'); updateGrouping(); }); });
    loadData();
</script>
</body>
</html>
