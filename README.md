<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaRL Department | User Registration & Admin Control | Teaching at the Right Level</title>
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
        .btn-warning { background: #fed7aa; color: #ea580c; }
        .btn-sm { padding: 4px 8px; font-size: 0.65rem; }
        .data-table { width: 100%; border-collapse: collapse; font-size: 0.7rem; overflow-x: auto; display: block; }
        .data-table th, .data-table td { padding: 8px 6px; text-align: left; border-bottom: 1px solid #e2e8f0; }
        .data-table th { background: #f8fafc; font-weight: 700; color: #1e293b; position: sticky; top: 0; }
        .data-table tr:hover { background: #f8fafc; }
        .alert { padding: 10px 14px; border-radius: 10px; margin-bottom: 15px; font-size: 0.75rem; }
        .alert-info { background: #dbeafe; color: #1e40af; border-left: 4px solid #2563eb; }
        .alert-success { background: #d1fae5; color: #065f46; border-left: 4px solid #10b981; }
        .alert-warning { background: #fed7aa; color: #92400e; border-left: 4px solid #f59e0b; }
        .alert-danger { background: #fee2e2; color: #991b1b; border-left: 4px solid #ef4444; }
        .level-badge { display: inline-block; padding: 2px 10px; border-radius: 20px; font-size: 0.65rem; font-weight: 600; }
        .level-1 { background: #fee2e2; color: #991b1b; }
        .level-2 { background: #fed7aa; color: #9a3412; }
        .level-3 { background: #fef3c7; color: #92400e; }
        .level-4 { background: #d1fae5; color: #065f46; }
        .level-5 { background: #dbeafe; color: #1e40af; }
        .level-6 { background: #e0e7ff; color: #3730a3; }
        .footer { text-align: center; padding: 20px; color: rgba(255,255,255,0.5); font-size: 0.7rem; }
        .login-container { max-width: 500px; margin: 50px auto; }
        .user-info { display: flex; align-items: center; gap: 15px; background: #f1f5f9; padding: 10px 20px; border-radius: 40px; }
        .user-avatar { width: 35px; height: 35px; background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; }
        @media (max-width: 768px) { .grid-2, .grid-3 { grid-template-columns: 1fr; } }
    </style>
</head>
<body>
<div class="dashboard-container" id="app">
    <!-- Login/Register Section (Shown when not logged in) -->
    <div id="authSection">
        <div class="login-container">
            <div class="card" style="text-align: center;">
                <div class="logo" style="margin-bottom: 20px;">
                    <h1><i class="fas fa-chalkboard-user"></i> TaRL Department</h1>
                    <p>Teaching at the Right Level Management System</p>
                </div>
                <div id="loginForm">
                    <div class="form-group"><label>Email Address</label><input type="email" id="loginEmail" placeholder="Enter your email"></div>
                    <div class="form-group"><label>Password</label><input type="password" id="loginPassword" placeholder="Enter password"></div>
                    <button class="btn btn-primary" onclick="loginUser()" style="width:100%;"><i class="fas fa-sign-in-alt"></i> Login</button>
                    <hr style="margin: 20px 0;">
                    <p>Don't have an account? <a href="#" onclick="showRegister()">Register here</a></p>
                </div>
                <div id="registerForm" style="display:none;">
                    <div class="form-group"><label>Full Name</label><input type="text" id="regName" placeholder="Your full name"></div>
                    <div class="form-group"><label>Email Address</label><input type="email" id="regEmail" placeholder="Enter your email"></div>
                    <div class="form-group"><label>Password</label><input type="password" id="regPassword" placeholder="Create password"></div>
                    <div class="form-group"><label>Confirm Password</label><input type="password" id="regConfirmPassword" placeholder="Confirm password"></div>
                    <div class="form-group"><label>Role</label><select id="regRole"><option value="teacher">Teacher (Barsiisaa)</option><option value="supervisor">Supervisor (Illee)</option><option value="admin">Admin (Admin)</option></select></div>
                    <button class="btn btn-primary" onclick="registerUser()" style="width:100%;"><i class="fas fa-user-plus"></i> Register</button>
                    <hr style="margin: 20px 0;">
                    <p>Already have an account? <a href="#" onclick="showLogin()">Login here</a></p>
                </div>
            </div>
        </div>
    </div>

    <!-- Main App Section (Shown after login) -->
    <div id="mainApp" style="display:none;">
        <div class="header">
            <div class="logo"><h1><i class="fas fa-chalkboard-user"></i> TaRL Department</h1><p>Teaching at the Right Level | Student Registration → Woreda/School Feed</p></div>
            <div class="stats-badge">
                <div class="badge"><div class="number" id="totalStudents">0</div><div class="label">Total Students</div></div>
                <div class="badge"><div class="number" id="totalSchools">0</div><div class="label">Schools</div></div>
                <div class="badge"><div class="number" id="avgEnglish">0%</div><div class="label">Avg English</div></div>
                <div class="badge"><div class="number" id="avgOromo">0%</div><div class="label">Avg Afaan Oromoo</div></div>
                <div class="user-info" id="userInfoDisplay"></div>
            </div>
        </div>

        <div class="nav-tabs" id="navTabs">
            <button class="nav-btn active" data-section="dashboard"><i class="fas fa-chart-pie"></i> Dashboard</button>
            <button class="nav-btn" data-section="studentReg"><i class="fas fa-user-plus"></i> Student Registration</button>
            <button class="nav-btn" data-section="grouping"><i class="fas fa-layer-group"></i> Grouping by Grade</button>
            <button class="nav-btn" data-section="woreda"><i class="fas fa-map-marker-alt"></i> Woreda Level</button>
            <button class="nav-btn" data-section="school"><i class="fas fa-school"></i> School Level</button>
            <button class="nav-btn" data-section="reports"><i class="fas fa-file-alt"></i> Reports</button>
            <button class="nav-btn admin-only" data-section="admin" style="display:none;"><i class="fas fa-user-shield"></i> Admin Panel</button>
            <button class="nav-btn" onclick="logoutUser()"><i class="fas fa-sign-out-alt"></i> Logout</button>
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

        <!-- STUDENT REGISTRATION -->
        <div id="studentReg" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-user-plus"></i> Register Student (Data Feeds to Woreda & School)</div>
                <div class="grid-3">
                    <div class="form-group"><label>Zone (Godina)</label><input type="text" id="studentZone" placeholder="e.g., East Wolega"></div>
                    <div class="form-group"><label>Woreda (Aanaa)</label><input type="text" id="studentWoreda" placeholder="e.g., Leka Dulecha"></div>
                    <div class="form-group"><label>School ID</label><input type="text" id="studentSchoolId" placeholder="School ID"></div>
                    <div class="form-group"><label>School Name</label><input type="text" id="studentSchoolName" placeholder="School Name"></div>
                    <div class="form-group"><label>Learner ID</label><input type="text" id="learnerId" placeholder="Auto-generated" readonly></div>
                    <div class="form-group"><label>Student Name</label><input type="text" id="studentName" placeholder="Full Name"></div>
                    <div class="form-group"><label>Sex</label><select id="studentSex"><option value="M">Male (Dhiirra)</option><option value="F">Female (Dubartii)</option></select></div>
                    <div class="form-group"><label>Grade</label><select id="studentGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                    <div class="form-group"><label>Special Needs</label><select id="specialNeeds"><option value="None">None</option><option value="Vision">Vision</option><option value="Hearing">Hearing</option><option value="Physical">Physical</option></select></div>
                </div>
                <div class="grid-2">
                    <div><h4>English Assessment (5 Components)</h4>
                        <div class="form-group"><label>Beginners</label><input type="number" id="studentEngBeginners" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Arfii (Alphabet)</label><input type="number" id="studentEngAlphabet" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Jechoota (Words)</label><input type="number" id="studentEngWords" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Keeyyata Salphaa (Sentence)</label><input type="number" id="studentEngSentence" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Seenessaa (Story)</label><input type="number" id="studentEngStory" placeholder="0/1" max="1" min="0"></div>
                    </div>
                    <div><h4>Afaan Oromoo Assessment (5 Components)</h4>
                        <div class="form-group"><label>Beginners</label><input type="number" id="studentOromoBeginners" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Qubee</label><input type="number" id="studentOromoQubee" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Jecha</label><input type="number" id="studentOromoWord" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Keeyyata Salphaa</label><input type="number" id="studentOromoSentence" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Seenessaa</label><input type="number" id="studentOromoStory" placeholder="0/1" max="1" min="0"></div>
                    </div>
                </div>
                <button class="btn btn-primary" onclick="registerStudent()"><i class="fas fa-save"></i> Register Student</button>
            </div>
            <div class="card"><div class="card-title"><i class="fas fa-table-list"></i> Registered Students</div><div style="overflow-x:auto;"><table class="data-table" id="studentTable"><thead><tr><th>ID</th><th>Name</th><th>School</th><th>Grade</th><th>English</th><th>Oromo</th><th>Actions</th></tr></thead><tbody id="studentTableBody"></tbody></table></div></div>
        </div>

        <!-- GROUPING SECTION -->
        <div id="grouping" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-layer-group"></i> Grouping by Grade & TaRL Level</div>
                <div class="form-group"><label>Select Grade</label><select id="groupGradeSelect"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                <button class="btn btn-primary" onclick="updateGrouping()"><i class="fas fa-chart-line"></i> Show Grouping</button>
                <div id="groupingDisplay" style="margin-top:20px;"></div>
            </div>
        </div>

        <!-- WOREDA LEVEL -->
        <div id="woreda" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-map-marker-alt"></i> Woreda Level Summary</div>
                <div style="overflow-x:auto;"><table class="data-table" id="woredaTable"><thead><tr><th>Zone</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th></tr></thead><tbody id="woredaTableBody"></tbody></table></div>
            </div>
        </div>

        <!-- SCHOOL LEVEL -->
        <div id="school" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-school"></i> School Level Summary</div>
                <div style="overflow-x:auto;"><table class="data-table" id="schoolTable"><thead><tr><th>School</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th></tr></thead><tbody id="schoolTableBody"></tbody></table></div>
            </div>
        </div>

        <!-- REPORTS SECTION -->
        <div id="reports" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-file-pdf"></i> Generate Report</div>
                <div class="grid-2">
                    <div><div class="form-group"><label>Report Type</label><select id="reportType"><option value="woreda">Woreda Level</option><option value="school">School Level</option><option value="student">Student Roster</option></select></div></div>
                    <div><div class="form-group"><label>Format</label><select id="reportFormat"><option value="html">HTML</option><option value="pdf">PDF</option></select></div></div>
                </div>
                <button class="btn btn-primary" onclick="generateReport()"><i class="fas fa-download"></i> Generate Report</button>
            </div>
        </div>

        <!-- ADMIN PANEL SECTION -->
        <div id="admin" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-user-shield"></i> Admin Panel - User Management</div>
                <div style="overflow-x:auto;"><table class="data-table" id="usersTable"><thead><tr><th>Name</th><th>Email</th><th>Role</th><th>Status</th><th>Registered</th><th>Actions</th></tr></thead><tbody id="usersTableBody"></tbody></table></div>
                <div class="alert alert-info" style="margin-top:15px;"><i class="fas fa-envelope"></i> Approval emails are sent to: <strong>mohammed.scho2023@gmail.com</strong>. Click the link in the email to approve/reject users.</div>
            </div>
            <div class="card">
                <div class="card-title"><i class="fas fa-chart-line"></i> System Activity Log</div>
                <div id="activityLog"></div>
            </div>
        </div>
        <div class="footer"><p>TaRL Department | User Registration with Email Verification | Admin Approval System</p></div>
    </div>
</div>

<script>
    // Data Structures
    let users = [];
    let students = [];
    let pendingUsers = [];
    let currentUser = null;
    let activityLog = [];

    // Admin email
    const ADMIN_EMAIL = "mohammed.scho2023@gmail.com";

    // Load/Save
    function saveData() {
        localStorage.setItem('tarl_users', JSON.stringify(users));
        localStorage.setItem('tarl_students', JSON.stringify(students));
        localStorage.setItem('tarl_pending', JSON.stringify(pendingUsers));
        localStorage.setItem('tarl_activity', JSON.stringify(activityLog));
        localStorage.setItem('tarl_currentUser', JSON.stringify(currentUser));
    }

    function loadData() {
        users = JSON.parse(localStorage.getItem('tarl_users') || '[]');
        students = JSON.parse(localStorage.getItem('tarl_students') || '[]');
        pendingUsers = JSON.parse(localStorage.getItem('tarl_pending') || '[]');
        activityLog = JSON.parse(localStorage.getItem('tarl_activity') || '[]');
        const savedUser = localStorage.getItem('tarl_currentUser');
        if(savedUser && savedUser !== 'null') {
            currentUser = JSON.parse(savedUser);
            if(currentUser && currentUser.status === 'approved') showMainApp();
            else showAuth();
        } else {
            showAuth();
        }
        // Create default admin if none exists
        if(users.length === 0) {
            users.push({ id: 'ADMIN_1', name: 'System Admin', email: ADMIN_EMAIL, password: btoa('admin123'), role: 'admin', status: 'approved', registeredAt: new Date().toISOString() });
            saveData();
        }
        updateAll();
    }

    function showAuth() { document.getElementById('authSection').style.display = 'block'; document.getElementById('mainApp').style.display = 'none'; }
    function showMainApp() { document.getElementById('authSection').style.display = 'none'; document.getElementById('mainApp').style.display = 'block'; updateUserDisplay(); updateAll(); }

    function showRegister() { document.getElementById('loginForm').style.display = 'none'; document.getElementById('registerForm').style.display = 'block'; }
    function showLogin() { document.getElementById('registerForm').style.display = 'none'; document.getElementById('loginForm').style.display = 'block'; }

    function registerUser() {
        const name = document.getElementById('regName').value;
        const email = document.getElementById('regEmail').value;
        const password = document.getElementById('regPassword').value;
        const confirm = document.getElementById('regConfirmPassword').value;
        const role = document.getElementById('regRole').value;
        
        if(!name || !email || !password) { alert('Please fill all fields'); return; }
        if(password !== confirm) { alert('Passwords do not match'); return; }
        if(users.find(u => u.email === email)) { alert('Email already registered'); return; }
        
        // Check if already pending
        if(pendingUsers.find(u => u.email === email)) { alert('Registration already pending approval'); return; }
        
        const pendingUser = { id: 'PEND_' + Date.now(), name, email, password: btoa(password), role, status: 'pending', registeredAt: new Date().toISOString() };
        pendingUsers.push(pendingUser);
        saveData();
        
        // Generate approval link (simulated email)
        const approvalLink = `https://tarl-system.approve?user=${pendingUser.id}&action=approve`;
        const rejectLink = `https://tarl-system.approve?user=${pendingUser.id}&action=reject`;
        
        // Log activity
        logActivity(`New registration request from ${name} (${email}) with role ${role}. Waiting for admin approval.`);
        
        // Simulate sending email to admin (display in console and alert)
        console.log(`\n========== EMAIL TO ADMIN (${ADMIN_EMAIL}) ==========`);
        console.log(`Subject: New TaRL User Registration - Action Required`);
        console.log(`\nDear Admin,\n\nA new user has registered for the TaRL Department Management System.\n\nUser Details:\n- Name: ${name}\n- Email: ${email}\n- Role: ${role}\n- Requested: ${new Date().toLocaleString()}\n\nTo APPROVE this user, click the link below (or copy and paste):\n${approvalLink}\n\nTo REJECT this user, click:\n${rejectLink}\n\nYou can also manage users from the Admin Panel in the system.\n\nRegards,\nTaRL Department System`);
        console.log(`===================================================\n`);
        
        alert(`Registration request submitted!\n\nAn approval request has been sent to the admin (${ADMIN_EMAIL}).\nYou will receive an email confirmation once approved.\n\nPlease check your email for the approval link.`);
        
        // Store pending approval in a way that can be accessed via URL params
        sessionStorage.setItem('pendingAction', JSON.stringify({ userId: pendingUser.id, email: email }));
        
        showLogin();
        document.getElementById('regName').value = '';
        document.getElementById('regEmail').value = '';
        document.getElementById('regPassword').value = '';
        document.getElementById('regConfirmPassword').value = '';
    }

    function loginUser() {
        const email = document.getElementById('loginEmail').value;
        const password = document.getElementById('loginPassword').value;
        const user = users.find(u => u.email === email && atob(u.password) === password);
        if(user && user.status === 'approved') {
            currentUser = user;
            saveData();
            showMainApp();
            logActivity(`${user.name} (${user.role}) logged in`);
            alert(`Welcome back, ${user.name}!`);
        } else if(user && user.status === 'pending') {
            alert('Your account is pending admin approval. Please wait for confirmation email.');
        } else {
            alert('Invalid email or password');
        }
    }

    function logoutUser() {
        logActivity(`${currentUser?.name} logged out`);
        currentUser = null;
        saveData();
        showAuth();
        document.getElementById('loginEmail').value = '';
        document.getElementById('loginPassword').value = '';
    }

    function updateUserDisplay() {
        if(currentUser) {
            document.getElementById('userInfoDisplay').innerHTML = `<div class="user-avatar"><i class="fas fa-user"></i></div><span>${currentUser.name} (${currentUser.role})</span>`;
            // Show admin panel only for admins
            const adminBtn = document.querySelector('.admin-only');
            if(adminBtn) adminBtn.style.display = currentUser.role === 'admin' ? 'inline-block' : 'none';
        }
    }

    function logActivity(message) {
        activityLog.unshift({ message, timestamp: new Date().toISOString() });
        if(activityLog.length > 50) activityLog.pop();
        saveData();
        updateActivityLog();
    }

    function updateActivityLog() {
        const container = document.getElementById('activityLog');
        if(container) {
            container.innerHTML = '<div style="max-height:300px; overflow-y:auto;">' + activityLog.slice(0,20).map(log => `<div style="padding:8px; border-bottom:1px solid #e2e8f0; font-size:0.7rem;"><strong>${new Date(log.timestamp).toLocaleString()}</strong><br>${escapeHtml(log.message)}</div>`).join('') + '</div>';
        }
    }

    function approveUser(userId) {
        const pending = pendingUsers.find(u => u.id === userId);
        if(pending) {
            users.push({ id: pending.id.replace('PEND_', 'USER_'), name: pending.name, email: pending.email, password: pending.password, role: pending.role, status: 'approved', registeredAt: pending.registeredAt });
            pendingUsers = pendingUsers.filter(u => u.id !== userId);
            saveData();
            updateAll();
            logActivity(`User ${pending.name} (${pending.email}) was APPROVED by ${currentUser?.name}`);
            alert(`User ${pending.name} approved successfully!`);
        }
    }

    function rejectUser(userId) {
        const pending = pendingUsers.find(u => u.id === userId);
        if(pending) {
            pendingUsers = pendingUsers.filter(u => u.id !== userId);
            saveData();
            updateAll();
            logActivity(`User ${pending.name} (${pending.email}) was REJECTED by ${currentUser?.name}`);
            alert(`User ${pending.name} rejected.`);
        }
    }

    function deleteUser(userId) {
        if(confirm('Delete this user?')) {
            const user = users.find(u => u.id === userId);
            users = users.filter(u => u.id !== userId);
            saveData();
            updateAll();
            logActivity(`User ${user?.name} (${user?.email}) was DELETED by ${currentUser?.name}`);
            alert('User deleted');
        }
    }

    function updateUsersTable() {
        const tbody = document.getElementById('usersTableBody');
        if(!tbody) return;
        tbody.innerHTML = '';
        // Show pending users first
        pendingUsers.forEach(u => {
            tbody.innerHTML += `<tr style="background:#fef3c7;"><td>${escapeHtml(u.name)}</td><td>${escapeHtml(u.email)}</td><td>${u.role}</td><td><span class="level-badge level-2">Pending</span></td><td>${new Date(u.registeredAt).toLocaleDateString()}</td>
            <td><button class="btn btn-success btn-sm" onclick="approveUser('${u.id}')"><i class="fas fa-check"></i> Approve</button> <button class="btn btn-danger btn-sm" onclick="rejectUser('${u.id}')"><i class="fas fa-times"></i> Reject</button></td></tr>`;
        });
        users.forEach(u => {
            if(u.email !== ADMIN_EMAIL) {
                tbody.innerHTML += `<tr><td>${escapeHtml(u.name)}</td><td>${escapeHtml(u.email)}</td><td>${u.role}</td><td><span class="level-badge level-4">Approved</span></td><td>${new Date(u.registeredAt).toLocaleDateString()}</td>
                <td><button class="btn btn-danger btn-sm" onclick="deleteUser('${u.id}')"><i class="fas fa-trash"></i> Delete</button></td></tr>`;
            }
        });
    }

    // Student Management Functions
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
        const englishScore = (parseInt(document.getElementById('studentEngBeginners').value)||0) + (parseInt(document.getElementById('studentEngAlphabet').value)||0) + 
                             (parseInt(document.getElementById('studentEngWords').value)||0) + (parseInt(document.getElementById('studentEngSentence').value)||0) + 
                             (parseInt(document.getElementById('studentEngStory').value)||0);
        const oromoScore = (parseInt(document.getElementById('studentOromoBeginners').value)||0) + (parseInt(document.getElementById('studentOromoQubee').value)||0) + 
                           (parseInt(document.getElementById('studentOromoWord').value)||0) + (parseInt(document.getElementById('studentOromoSentence').value)||0) + 
                           (parseInt(document.getElementById('studentOromoStory').value)||0);
        students.push({
            id: learnerId, zone: document.getElementById('studentZone').value, woreda: document.getElementById('studentWoreda').value,
            schoolId: document.getElementById('studentSchoolId').value, schoolName: document.getElementById('studentSchoolName').value,
            name: document.getElementById('studentName').value, sex: document.getElementById('studentSex').value,
            grade: document.getElementById('studentGrade').value, specialNeeds: document.getElementById('specialNeeds').value,
            englishScore: englishScore, oromoScore: oromoScore, englishLevel: getEnglishLevel(englishScore), oromoLevel: getOromoLevel(oromoScore)
        });
        saveData();
        updateAll();
        alert(`Student registered! ID: ${learnerId}`);
        clearStudentForm();
        logActivity(`${currentUser?.name} registered student: ${document.getElementById('studentName').value}`);
    }

    function clearStudentForm() {
        ['studentZone','studentWoreda','studentSchoolId','studentSchoolName','studentName','studentEngBeginners','studentEngAlphabet','studentEngWords','studentEngSentence','studentEngStory','studentOromoBeginners','studentOromoQubee','studentOromoWord','studentOromoSentence','studentOromoStory','learnerId'].forEach(id => { if(document.getElementById(id)) document.getElementById(id).value = ''; });
    }

    function deleteStudent(id) { if(confirm('Delete student?')) { students = students.filter(s => s.id !== id); saveData(); updateAll(); logActivity(`${currentUser?.name} deleted student`); } }

    function updateStudentTable() {
        const tbody = document.getElementById('studentTableBody');
        if(!tbody) return;
        if(students.length === 0) { tbody.innerHTML = '<tr><td colspan="7" style="text-align:center;">No students registered</td></tr>'; return; }
        tbody.innerHTML = '';
        students.forEach(s => {
            tbody.innerHTML += `<tr><td>${s.id.substring(0,10)}</td><td>${escapeHtml(s.name)}</td><td>${escapeHtml(s.schoolName)}</td><td>Kutaa ${s.grade}</td>
            <td>${s.englishScore}/5 <span class="level-badge ${s.englishLevel.class}">${s.englishLevel.name}</span></td>
            <td>${s.oromoScore}/5 <span class="level-badge ${s.oromoLevel.class}">${s.oromoLevel.name}</span></td>
            <td><button class="btn btn-danger btn-sm" onclick="deleteStudent('${s.id}')"><i class="fas fa-trash"></i></button></td></tr>`;
        });
    }

    function updateWoredaTable() {
        const tbody = document.getElementById('woredaTableBody');
        if(!tbody) return;
        const map = new Map();
        students.forEach(s => {
            const key = `${s.zone}|${s.woreda}|${s.grade}`;
            if(!map.has(key)) map.set(key, { zone: s.zone, woreda: s.woreda, grade: s.grade, count: 0, engTotal: 0, oromoTotal: 0 });
            const d = map.get(key); d.count++; d.engTotal += s.englishScore; d.oromoTotal += s.oromoScore;
        });
        if(map.size === 0) { tbody.innerHTML = '<tr><td colspan="6">No data</tr>'; return; }
        tbody.innerHTML = '';
        for(let d of map.values()) {
            tbody.innerHTML += `<tr><td>${escapeHtml(d.zone)}</td><td>${escapeHtml(d.woreda)}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td>
            <td>${((d.engTotal/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oromoTotal/(d.count*5))*100).toFixed(1)}%</td></tr>`;
        }
    }

    function updateSchoolTable() {
        const tbody = document.getElementById('schoolTableBody');
        if(!tbody) return;
        const map = new Map();
        students.forEach(s => {
            const key = `${s.schoolName}|${s.woreda}|${s.grade}`;
            if(!map.has(key)) map.set(key, { school: s.schoolName, woreda: s.woreda, grade: s.grade, count: 0, engTotal: 0, oromoTotal: 0 });
            const d = map.get(key); d.count++; d.engTotal += s.englishScore; d.oromoTotal += s.oromoScore;
        });
        if(map.size === 0) { tbody.innerHTML = '<tr><td colspan="6">No data</tr>'; return; }
        tbody.innerHTML = '';
        for(let d of map.values()) {
            tbody.innerHTML += `<tr><td>${escapeHtml(d.school)}</td><td>${escapeHtml(d.woreda)}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td>
            <td>${((d.engTotal/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oromoTotal/(d.count*5))*100).toFixed(1)}%</td></tr>`;
        }
    }

    function updateGrouping() {
        const grade = document.getElementById('groupGradeSelect')?.value;
        const container = document.getElementById('groupingDisplay');
        if(!container) return;
        const gradeStudents = students.filter(s => s.grade === grade);
        if(gradeStudents.length === 0) { container.innerHTML = '<div class="alert alert-info">No students for this grade</div>'; return; }
        const groups = {1:[],2:[],3:[],4:[],5:[],6:[]};
        gradeStudents.forEach(s => groups[s.englishLevel.code].push(s.name));
        let html = '<div class="grid-2"><div><h4>English Grouping</h4>';
        const levelNames = ['Beginner','Alphabet','Words','Sentence','Story','Computed'];
        for(let i=1;i<=6;i++) {
            if(groups[i].length > 0) html += `<div class="group-card"><strong>Level ${i}: ${levelNames[i-1]}</strong> (${groups[i].length})<br><small>${groups[i].join(', ')}</small></div>`;
        }
        html += '</div></div>';
        container.innerHTML = html;
    }

    function updateDashboardCharts() {
        const grades = ['3','4','5'];
        const engData = grades.map(g => { const gs = students.filter(s=>s.grade===g); if(gs.length===0) return 0; return (gs.reduce((s,st)=>s+st.englishScore,0)/(gs.length*5))*100; });
        const oromoData = grades.map(g => { const gs = students.filter(s=>s.grade===g); if(gs.length===0) return 0; return (gs.reduce((s,st)=>s+st.oromoScore,0)/(gs.length*5))*100; });
        if(window.engChart) window.engChart.destroy();
        window.engChart = new Chart(document.getElementById('englishGradeChart'), { type: 'bar', data: { labels: ['Kutaa 3','Kutaa 4','Kutaa 5'], datasets: [{ label: 'English %', data: engData, backgroundColor: '#3b82f6' }] } });
        if(window.oromoChart) window.oromoChart.destroy();
        window.oromoChart = new Chart(document.getElementById('oromoGradeChart'), { type: 'bar', data: { labels: ['Kutaa 3','Kutaa 4','Kutaa 5'], datasets: [{ label: 'Afaan Oromoo %', data: oromoData, backgroundColor: '#10b981' }] } });
        
        const levelCounts = [0,0,0,0,0,0];
        students.forEach(s => levelCounts[s.englishLevel.code-1]++);
        if(window.levelChart) window.levelChart.destroy();
        window.levelChart = new Chart(document.getElementById('levelDistributionChart'), { type: 'pie', data: { labels: ['Beginner','Alphabet','Words','Sentence','Story','Computed'], datasets: [{ data: levelCounts, backgroundColor: ['#ef4444','#f59e0b','#fef3c7','#d1fae5','#dbeafe','#e0e7ff'] }] } });
    }

    function updateProgramSummary() {
        const total = students.length;
        const males = students.filter(s=>s.sex==='M').length;
        const females = students.filter(s=>s.sex==='F').length;
        const avgEng = total ? (students.reduce((s,st)=>s+st.englishScore,0)/total)*20 : 0;
        const avgOro = total ? (students.reduce((s,st)=>s+st.oromoScore,0)/total)*20 : 0;
        const container = document.getElementById('programSummary');
        if(container) container.innerHTML = `<div class="grid-3"><div class="alert alert-info"><strong>Enrollment</strong><br>Total: ${total}<br>Male: ${males}<br>Female: ${females}</div>
        <div class="alert alert-success"><strong>Average Scores</strong><br>English: ${avgEng.toFixed(1)}%<br>Afaan Oromoo: ${avgOro.toFixed(1)}%</div>
        <div class="alert alert-info"><strong>Coverage</strong><br>Schools: ${[...new Set(students.map(s=>s.schoolName))].length}<br>Woredas: ${[...new Set(students.map(s=>s.woreda))].length}</div></div>`;
    }

    function generateReport() {
        const type = document.getElementById('reportType').value;
        const format = document.getElementById('reportFormat').value;
        let html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>TaRL Report</title><style>body{font-family:Arial;margin:40px;}table{border-collapse:collapse;width:100%;}th,td{border:1px solid #ddd;padding:8px;}</style></head><body><h1>TaRL Program Report</h1><p>Generated: ${new Date().toLocaleString()}</p>`;
        if(type === 'woreda') {
            const map = new Map();
            students.forEach(s => { const key = `${s.zone}|${s.woreda}|${s.grade}`; if(!map.has(key)) map.set(key, { zone:s.zone, woreda:s.woreda, grade:s.grade, count:0, eng:0, oro:0 }); const d=map.get(key); d.count++; d.eng+=s.englishScore; d.oro+=s.oromoScore; });
            html += `<h2>Woreda Summary</h2><table><th>Zone</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th>`;
            for(let d of map.values()) html += `<tr><td>${d.zone}</td><td>${d.woreda}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td><td>${((d.eng/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oro/(d.count*5))*100).toFixed(1)}%</td></tr>`;
            html += ` licensierad`;
        } else if(type === 'school') {
            const map = new Map();
            students.forEach(s => { const key = `${s.schoolName}|${s.grade}`; if(!map.has(key)) map.set(key, { school:s.schoolName, woreda:s.woreda, grade:s.grade, count:0, eng:0, oro:0 }); const d=map.get(key); d.count++; d.eng+=s.englishScore; d.oro+=s.oromoScore; });
            html += `<h2>School Summary</h2><tr><th>School</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th>`;
            for(let d of map.values()) html += `<tr><td>${d.school}</td><td>${d.woreda}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td><td>${((d.eng/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oro/(d.count*5))*100).toFixed(1)}%</td></tr>`;
            html += `</table>`;
        } else {
            html += `<h2>Student Roster</h2><table><th>Name</th><th>School</th><th>Grade</th><th>English</th><th>Oromo</th>`;
            students.forEach(s => html += `<tr><td>${s.name}</td><td>${s.schoolName}</td><td>Kutaa ${s.grade}</td><td>${s.englishScore}/5</td><td>${s.oromoScore}/5</td></tr>`);
            html += `</table>`;
        }
        html += `</body></html>`;
        if(format === 'html') { const w=window.open(); w.document.write(html); w.document.close(); }
        else { const div=document.createElement('div'); div.innerHTML=html; document.body.appendChild(div); html2pdf().from(div).set({margin:1}).save(); setTimeout(()=>document.body.removeChild(div),1000); }
    }

    function updateAll() {
        updateStudentTable();
        updateWoredaTable();
        updateSchoolTable();
        updateUsersTable();
        updateActivityLog();
        updateDashboardCharts();
        updateProgramSummary();
        updateStats();
    }

    function updateStats() {
        document.getElementById('totalStudents').textContent = students.length;
        document.getElementById('totalSchools').textContent = [...new Set(students.map(s=>s.schoolName))].length;
        let eng=0,oro=0;
        students.forEach(s=>{ eng+=s.englishScore; oro+=s.oromoScore; });
        document.getElementById('avgEnglish').textContent = students.length ? ((eng/(students.length*5))*100).toFixed(0) : '0';
        document.getElementById('avgOromo').textContent = students.length ? ((oro/(students.length*5))*100).toFixed(0) : '0';
    }

    function escapeHtml(str){ if(!str) return ''; return str.replace(/[&<>]/g, m => m==='&'?'&amp;':m==='<'?'&lt;':'&gt;'); }

    // Check URL for approval links (simulated)
    function checkUrlForApproval() {
        const params = new URLSearchParams(window.location.search);
        const userId = params.get('user');
        const action = params.get('action');
        if(userId && action === 'approve') {
            approveUser(userId);
            window.history.replaceState({}, document.title, window.location.pathname);
        } else if(userId && action === 'reject') {
            rejectUser(userId);
            window.history.replaceState({}, document.title, window.location.pathname);
        }
    }

    // Navigation
    document.addEventListener('click', function(e) {
        if(e.target.closest('.nav-btn') && !e.target.closest('.nav-btn').getAttribute('onclick')) {
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            e.target.closest('.nav-btn').classList.add('active');
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            const sectionId = e.target.closest('.nav-btn').getAttribute('data-section');
            if(sectionId && document.getElementById(sectionId)) document.getElementById(sectionId).classList.add('active');
        }
    });

    loadData();
    checkUrlForApproval();
</script>
</body>
</html>
