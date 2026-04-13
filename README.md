<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaRL Department | Admin Email Approval | Working Admin Login</title>
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
        .level-badge { display: inline-block; padding: 2px 10px; border-radius: 20px; font-size: 0.65rem; font-weight: 600; }
        .level-1 { background: #fee2e2; color: #991b1b; }
        .level-2 { background: #fed7aa; color: #9a3412; }
        .level-3 { background: #fef3c7; color: #92400e; }
        .level-4 { background: #d1fae5; color: #065f46; }
        .level-5 { background: #dbeafe; color: #1e40af; }
        .level-6 { background: #e0e7ff; color: #3730a3; }
        .footer { text-align: center; padding: 20px; color: rgba(255,255,255,0.5); font-size: 0.7rem; }
        .login-container { max-width: 500px; margin: 50px auto; }
        .role-selector { display: flex; gap: 10px; margin-bottom: 20px; }
        .role-btn { flex: 1; padding: 12px; border: 2px solid #e2e8f0; background: white; border-radius: 12px; cursor: pointer; text-align: center; transition: all 0.3s; }
        .role-btn.active { border-color: #2563eb; background: #dbeafe; color: #2563eb; }
        .user-info { display: flex; align-items: center; gap: 15px; background: #f1f5f9; padding: 8px 18px; border-radius: 40px; }
        .user-avatar { width: 35px; height: 35px; background: linear-gradient(135deg, #2563eb 0%, #7c3aed 100%); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; }
        .email-sim { background: #1e293b; color: #e2e8f0; padding: 15px; border-radius: 12px; font-family: monospace; font-size: 0.7rem; margin-top: 15px; text-align: left; white-space: pre-wrap; word-break: break-all; }
        @media (max-width: 768px) { .grid-2, .grid-3 { grid-template-columns: 1fr; } }
        .toast-msg { position: fixed; bottom: 20px; right: 20px; background: #10b981; color: white; padding: 12px 20px; border-radius: 12px; z-index: 1000; animation: slideIn 0.3s ease; }
        @keyframes slideIn { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }
        .admin-credentials { background: #d1fae5; padding: 10px; border-radius: 10px; margin-bottom: 15px; font-size: 0.7rem; text-align: center; }
    </style>
</head>
<body>
<div class="dashboard-container" id="app">
    <!-- LOGIN SECTION -->
    <div id="authSection">
        <div class="login-container">
            <div class="card" style="text-align: center;">
                <div class="logo" style="margin-bottom: 20px;">
                    <h1><i class="fas fa-chalkboard-user"></i> TaRL Department</h1>
                    <p>Teaching at the Right Level Management System</p>
                </div>
                
                <div class="admin-credentials">
                    <strong>🔐 Admin Login Credentials:</strong><br>
                    Email: <code>admin@tarl.com</code> | Password: <code>admin123</code>
                </div>

                <div class="role-selector">
                    <div class="role-btn active" data-role="user" onclick="selectLoginRole('user')"><i class="fas fa-user"></i> Data Collector Login</div>
                    <div class="role-btn" data-role="admin" onclick="selectLoginRole('admin')"><i class="fas fa-user-shield"></i> Admin Login</div>
                </div>

                <div id="userLoginForm">
                    <div class="form-group"><label>Email Address</label><input type="email" id="userEmail" placeholder="Enter your email"></div>
                    <div class="form-group"><label>Password</label><input type="password" id="userPassword" placeholder="Enter password"></div>
                    <button class="btn btn-primary" onclick="loginUser()" style="width:100%;"><i class="fas fa-sign-in-alt"></i> Login as Data Collector</button>
                    <hr style="margin: 20px 0;">
                    <p>Don't have an account? <a href="#" onclick="showRegister()">Register here</a></p>
                </div>

                <div id="adminLoginForm" style="display:none;">
                    <div class="form-group"><label>Admin Email</label><input type="email" id="adminEmail" placeholder="admin@tarl.com" value="admin@tarl.com"></div>
                    <div class="form-group"><label>Admin Password</label><input type="password" id="adminPassword" placeholder="admin123" value="admin123"></div>
                    <button class="btn btn-primary" onclick="loginAdmin()" style="width:100%;"><i class="fas fa-user-shield"></i> Login as Admin</button>
                </div>

                <div id="registerForm" style="display:none; margin-top:20px;">
                    <hr>
                    <h3>Register New Data Collector Account</h3>
                    <div class="form-group"><label>Full Name</label><input type="text" id="regName" placeholder="Your full name"></div>
                    <div class="form-group"><label>Email Address</label><input type="email" id="regEmail" placeholder="Enter your email"></div>
                    <div class="form-group"><label>Password</label><input type="password" id="regPassword" placeholder="Create password"></div>
                    <div class="form-group"><label>Confirm Password</label><input type="password" id="regConfirmPassword" placeholder="Confirm password"></div>
                    <div class="form-group"><label>Zone (Godina)</label><select id="regZone"><option value="">Select Zone</option><option value="East Wolega">East Wolega</option><option value="West Wolega">West Wolega</option><option value="North Shewa">North Shewa</option></select></div>
                    <div class="form-group"><label>Woreda (Aanaa)</label><input type="text" id="regWoreda" placeholder="e.g., Leka Dulecha, Gimbi, Kuyu"></div>
                    <button class="btn btn-success" onclick="registerUser()" style="width:100%;"><i class="fas fa-user-plus"></i> Register</button>
                    <p style="margin-top:15px;"><a href="#" onclick="showLoginForm()">Back to Login</a></p>
                </div>
            </div>
        </div>
    </div>

    <!-- ADMIN MAIN APP -->
    <div id="adminApp" style="display:none;">
        <div class="header">
            <div class="logo"><h1><i class="fas fa-user-shield"></i> TaRL Admin Panel</h1><p>Administrator Dashboard | Email Approval System</p></div>
            <div class="stats-badge">
                <div class="badge"><div class="number" id="adminTotalUsers">0</div><div class="label">Total Users</div></div>
                <div class="badge"><div class="number" id="adminPendingUsers">0</div><div class="label">Pending</div></div>
                <div class="badge"><div class="number" id="adminTotalStudents">0</div><div class="label">Students</div></div>
                <div class="user-info"><div class="user-avatar"><i class="fas fa-user-shield"></i></div><span id="adminName">Admin</span> <button class="btn btn-sm btn-danger" onclick="logout()" style="margin-left:10px;"><i class="fas fa-sign-out-alt"></i></button></div>
            </div>
        </div>
        <div class="nav-tabs">
            <button class="nav-btn active" data-section="adminDashboard"><i class="fas fa-chart-pie"></i> Dashboard</button>
            <button class="nav-btn" data-section="userManagement"><i class="fas fa-users"></i> User Management</button>
            <button class="nav-btn" data-section="emailSimulator"><i class="fas fa-envelope"></i> Email Simulator</button>
            <button class="nav-btn" data-section="adminReports"><i class="fas fa-file-alt"></i> Reports</button>
            <button class="nav-btn" onclick="logout()"><i class="fas fa-sign-out-alt"></i> Logout</button>
        </div>

        <div id="adminDashboard" class="section active">
            <div class="grid-3">
                <div class="card"><div class="card-title"><i class="fas fa-users"></i> User Statistics</div><div id="adminUserStats"></div></div>
                <div class="card"><div class="card-title"><i class="fas fa-school"></i> School Statistics</div><div id="adminSchoolStats"></div></div>
                <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> System Activity</div><div id="adminActivityLog"></div></div>
            </div>
        </div>

        <div id="userManagement" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-users"></i> User Management - Approve/Reject Data Collectors</div>
                <div style="overflow-x:auto;"><table class="data-table" id="adminUsersTable"><thead><tr><th>Name</th><th>Email</th><th>Zone</th><th>Woreda</th><th>Role</th><th>Status</th><th>Registered</th><th>Actions</th></tr></thead><tbody id="adminUsersTableBody"></tbody>}</div>
            </div>
        </div>

        <div id="emailSimulator" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-envelope"></i> Email to Admin (mohammedscho2023@gmail.com)</div>
                <div class="alert alert-info"><i class="fas fa-info-circle"></i> When a user registers, an approval email is sent to the admin. Click the links below to approve or reject users.</div>
                <div id="emailSimulatorContent" class="email-sim">No emails sent yet. Register a user to see email simulation.</div>
            </div>
        </div>

        <div id="adminReports" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-file-pdf"></i> Generate System Report</div>
                <div class="form-group"><label>Format</label><select id="adminReportFormat"><option value="html">HTML</option><option value="pdf">PDF</option></select></div>
                <button class="btn btn-primary" onclick="generateAdminReport()"><i class="fas fa-download"></i> Generate Report</button>
            </div>
        </div>
    </div>

    <!-- USER MAIN APP -->
    <div id="userApp" style="display:none;">
        <div class="header">
            <div class="logo"><h1><i class="fas fa-chalkboard-user"></i> TaRL Department</h1><p id="userLocationDisplay">Teaching at the Right Level</p></div>
            <div class="stats-badge">
                <div class="badge"><div class="number" id="userTotalStudents">0</div><div class="label">My Students</div></div>
                <div class="badge"><div class="number" id="userTotalSchools">0</div><div class="label">My Schools</div></div>
                <div class="badge"><div class="number" id="userAvgEnglish">0%</div><div class="label">Avg English</div></div>
                <div class="badge"><div class="number" id="userAvgOromo">0%</div><div class="label">Avg Afaan Oromoo</div></div>
                <div class="user-info"><div class="user-avatar"><i class="fas fa-user"></i></div><span id="userNameDisplay"></span> <button class="btn btn-sm btn-danger" onclick="logout()" style="margin-left:10px;"><i class="fas fa-sign-out-alt"></i></button></div>
            </div>
        </div>

        <div class="nav-tabs">
            <button class="nav-btn active" data-section="userDashboard"><i class="fas fa-chart-pie"></i> My Dashboard</button>
            <button class="nav-btn" data-section="userStudentReg"><i class="fas fa-user-plus"></i> Register Student</button>
            <button class="nav-btn" data-section="userGrouping"><i class="fas fa-layer-group"></i> Grouping by Grade</button>
            <button class="nav-btn" data-section="userWoreda"><i class="fas fa-map-marker-alt"></i> My Woreda Summary</button>
            <button class="nav-btn" data-section="userSchool"><i class="fas fa-school"></i> My Schools</button>
            <button class="nav-btn" data-section="userReports"><i class="fas fa-file-alt"></i> Reports</button>
            <button class="nav-btn" onclick="logout()"><i class="fas fa-sign-out-alt"></i> Logout</button>
        </div>

        <div id="userDashboard" class="section active">
            <div class="grid-3">
                <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> English Progress by Grade</div><div class="chart-container" style="height:250px;"><canvas id="userEnglishChart"></canvas></div></div>
                <div class="card"><div class="card-title"><i class="fas fa-chart-line"></i> Afaan Oromoo Progress</div><div class="chart-container" style="height:250px;"><canvas id="userOromoChart"></canvas></div></div>
                <div class="card"><div class="card-title"><i class="fas fa-chart-pie"></i> Level Distribution</div><div class="chart-container" style="height:250px;"><canvas id="userLevelChart"></canvas></div></div>
            </div>
            <div class="card"><div class="card-title"><i class="fas fa-tachometer-alt"></i> My Program Summary</div><div id="userProgramSummary"></div></div>
        </div>

        <div id="userStudentReg" class="section">
            <div class="card">
                <div class="card-title"><i class="fas fa-user-plus"></i> Register Student</div>
                <div class="grid-3">
                    <div class="form-group"><label>Zone</label><input type="text" id="userStudentZone" readonly style="background:#f1f5f9;"></div>
                    <div class="form-group"><label>Woreda</label><input type="text" id="userStudentWoreda" readonly style="background:#f1f5f9;"></div>
                    <div class="form-group"><label>School ID</label><input type="text" id="userStudentSchoolId" placeholder="School ID"></div>
                    <div class="form-group"><label>School Name</label><input type="text" id="userStudentSchoolName" placeholder="School Name"></div>
                    <div class="form-group"><label>Learner ID</label><input type="text" id="userLearnerId" placeholder="Auto-generated" readonly></div>
                    <div class="form-group"><label>Student Name</label><input type="text" id="userStudentName" placeholder="Full Name"></div>
                    <div class="form-group"><label>Sex</label><select id="userStudentSex"><option value="M">Male</option><option value="F">Female</option></select></div>
                    <div class="form-group"><label>Grade</label><select id="userStudentGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                    <div class="form-group"><label>Special Needs</label><select id="userSpecialNeeds"><option value="None">None</option><option value="Vision">Vision</option><option value="Hearing">Hearing</option><option value="Physical">Physical</option></select></div>
                </div>
                <div class="grid-2">
                    <div><h4>English Assessment (5 Components)</h4>
                        <div class="form-group"><label>Beginners</label><input type="number" id="userEngBeginners" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Arfii (Alphabet)</label><input type="number" id="userEngAlphabet" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Jechoota (Words)</label><input type="number" id="userEngWords" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Keeyyata Salphaa (Sentence)</label><input type="number" id="userEngSentence" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Seenessaa (Story)</label><input type="number" id="userEngStory" placeholder="0/1" max="1" min="0"></div>
                    </div>
                    <div><h4>Afaan Oromoo Assessment (5 Components)</h4>
                        <div class="form-group"><label>Beginners</label><input type="number" id="userOromoBeginners" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Qubee</label><input type="number" id="userOromoQubee" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Jecha</label><input type="number" id="userOromoWord" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Keeyyata Salphaa</label><input type="number" id="userOromoSentence" placeholder="0/1" max="1" min="0"></div>
                        <div class="form-group"><label>Seenessaa</label><input type="number" id="userOromoStory" placeholder="0/1" max="1" min="0"></div>
                    </div>
                </div>
                <button class="btn btn-primary" onclick="registerUserStudent()"><i class="fas fa-save"></i> Register Student</button>
            </div>
            <div class="card"><div class="card-title"><i class="fas fa-table-list"></i> My Students</div><div style="overflow-x:auto;"><table class="data-table" id="userStudentTable"><thead><tr><th>ID</th><th>Name</th><th>School</th><th>Grade</th><th>English</th><th>Oromo</th><th>Actions</th></tr></thead><tbody id="userStudentTableBody"></tbody>}</div></div>
        </div>

        <div id="userGrouping" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-layer-group"></i> Grouping by Grade & Level</div>
                <div class="form-group"><label>Select Grade</label><select id="userGroupGrade"><option value="3">Kutaa 3</option><option value="4">Kutaa 4</option><option value="5">Kutaa 5</option></select></div>
                <button class="btn btn-primary" onclick="updateUserGrouping()"><i class="fas fa-chart-line"></i> Show Grouping</button>
                <div id="userGroupingDisplay" style="margin-top:20px;"></div>
            </div>
        </div>

        <div id="userWoreda" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-map-marker-alt"></i> My Woreda Summary</div>
                <div style="overflow-x:auto;"><table class="data-table" id="userWoredaTable"><thead><tr><th>Zone</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th></tr></thead><tbody id="userWoredaTableBody"></tbody>}</div>
            </div>
        </div>

        <div id="userSchool" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-school"></i> My Schools Summary</div>
                <div style="overflow-x:auto;"><table class="data-table" id="userSchoolTable"><thead><tr><th>School</th><th>Woreda</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th></tr></thead><tbody id="userSchoolTableBody"></tbody>}</div>
            </div>
        </div>

        <div id="userReports" class="section">
            <div class="card"><div class="card-title"><i class="fas fa-file-pdf"></i> Generate My Report</div>
                <div class="grid-2"><div><div class="form-group"><label>Report Type</label><select id="userReportType"><option value="woreda">Woreda Summary</option><option value="school">School Summary</option><option value="student">Student Roster</option></select></div></div>
                <div><div class="form-group"><label>Format</label><select id="userReportFormat"><option value="html">HTML</option><option value="pdf">PDF</option></select></div></div></div>
                <button class="btn btn-primary" onclick="generateUserReport()"><i class="fas fa-download"></i> Generate Report</button>
            </div>
        </div>
        <div class="footer"><p>TaRL Department | Data Collectors only see their approved Zone & Woreda</p></div>
    </div>
</div>

<script>
    let users = [];
    let students = [];
    let currentUser = null;
    let activityLog = [];
    let pendingEmails = [];

    const ADMIN_EMAIL = "admin@tarl.com";
    const ADMIN_PASSWORD = "admin123";

    function saveData() {
        localStorage.setItem('tarl_users', JSON.stringify(users));
        localStorage.setItem('tarl_students', JSON.stringify(students));
        localStorage.setItem('tarl_activity', JSON.stringify(activityLog));
        localStorage.setItem('tarl_pendingEmails', JSON.stringify(pendingEmails));
    }

    function loadData() {
        const savedUsers = localStorage.getItem('tarl_users');
        if(savedUsers) {
            users = JSON.parse(savedUsers);
        } else {
            users = [];
        }
        
        students = JSON.parse(localStorage.getItem('tarl_students') || '[]');
        activityLog = JSON.parse(localStorage.getItem('tarl_activity') || '[]');
        pendingEmails = JSON.parse(localStorage.getItem('tarl_pendingEmails') || '[]');
        
        // Check if admin exists - if not, create it
        const adminExists = users.find(u => u.role === 'admin');
        if(!adminExists) {
            users.push({
                id: 'ADMIN_1',
                name: 'System Administrator',
                email: ADMIN_EMAIL,
                password: btoa(ADMIN_PASSWORD),
                role: 'admin',
                zone: '',
                woreda: '',
                status: 'approved',
                registeredAt: new Date().toISOString()
            });
            saveData();
            console.log("Admin account created!");
        }
        
        updateAll();
    }

    function logActivity(message) {
        activityLog.unshift({ message, timestamp: new Date().toISOString() });
        if(activityLog.length > 50) activityLog.pop();
        saveData();
        updateAdminActivityLog();
    }

    function showToast(message, type = 'success') {
        const toast = document.createElement('div');
        toast.className = 'toast-msg';
        toast.style.background = type === 'success' ? '#10b981' : '#ef4444';
        toast.innerHTML = `<i class="fas fa-${type === 'success' ? 'check-circle' : 'exclamation-circle'}"></i> ${message}`;
        document.body.appendChild(toast);
        setTimeout(() => toast.remove(), 3000);
    }

    function sendApprovalEmail(user) {
        pendingEmails.unshift({ 
            userId: user.id, 
            email: user.email, 
            name: user.name, 
            zone: user.zone, 
            woreda: user.woreda, 
            timestamp: new Date().toISOString() 
        });
        saveData();
        updateEmailSimulator();
        logActivity(`Approval request sent for user: ${user.name} (${user.email})`);
        showToast(`Registration request sent to admin!`);
    }

    function updateEmailSimulator() {
        const container = document.getElementById('emailSimulatorContent');
        if(!container) return;
        if(pendingEmails.length === 0) {
            container.innerHTML = 'No pending approval emails. When users register, emails will appear here.';
            return;
        }
        let html = '';
        pendingEmails.forEach(email => {
            html += `
            <div style="background:#0f172a; margin-bottom:15px; padding:12px; border-radius:8px; border-left:4px solid #10b981;">
                <div><i class="fas fa-envelope"></i> <strong>To: mohammedscho2023@gmail.com</strong></div>
                <div><i class="fas fa-user"></i> User: ${escapeHtml(email.name)} (${escapeHtml(email.email)})</div>
                <div><i class="fas fa-map-marker-alt"></i> Zone: ${escapeHtml(email.zone)} | Woreda: ${escapeHtml(email.woreda)}</div>
                <div><i class="fas fa-clock"></i> Requested: ${new Date(email.timestamp).toLocaleString()}</div>
                <div style="margin-top:10px;">
                    <button class="btn btn-success btn-sm" onclick="quickApprove('${email.userId}')"><i class="fas fa-check"></i> Approve via Email</button>
                    <button class="btn btn-danger btn-sm" onclick="quickReject('${email.userId}')"><i class="fas fa-times"></i> Reject via Email</button>
                </div>
            </div>`;
        });
        container.innerHTML = html;
    }

    function quickApprove(userId) {
        const user = users.find(u => u.id === userId);
        if(user && user.status === 'pending') {
            user.status = 'approved';
            pendingEmails = pendingEmails.filter(e => e.userId !== userId);
            saveData();
            logActivity(`User ${user.name} (${user.email}) was APPROVED`);
            updateAdminApp();
            updateEmailSimulator();
            showToast(`${user.name} approved successfully!`);
        }
    }

    function quickReject(userId) {
        const user = users.find(u => u.id === userId);
        if(user) {
            users = users.filter(u => u.id !== userId);
            pendingEmails = pendingEmails.filter(e => e.userId !== userId);
            saveData();
            logActivity(`User ${user?.name} was REJECTED`);
            updateAdminApp();
            updateEmailSimulator();
            showToast(`${user?.name} rejected.`, 'error');
        }
    }

    let selectedLoginRole = 'user';
    function selectLoginRole(role) {
        selectedLoginRole = role;
        document.querySelectorAll('.role-btn').forEach(btn => btn.classList.remove('active'));
        document.querySelector(`.role-btn[data-role="${role}"]`).classList.add('active');
        if(role === 'user') {
            document.getElementById('userLoginForm').style.display = 'block';
            document.getElementById('adminLoginForm').style.display = 'none';
        } else {
            document.getElementById('userLoginForm').style.display = 'none';
            document.getElementById('adminLoginForm').style.display = 'block';
        }
    }

    function showRegister() { document.getElementById('userLoginForm').style.display = 'none'; document.getElementById('registerForm').style.display = 'block'; }
    function showLoginForm() { document.getElementById('registerForm').style.display = 'none'; document.getElementById('userLoginForm').style.display = 'block'; }

    function registerUser() {
        const name = document.getElementById('regName').value;
        const email = document.getElementById('regEmail').value;
        const password = document.getElementById('regPassword').value;
        const confirm = document.getElementById('regConfirmPassword').value;
        const zone = document.getElementById('regZone').value;
        const woreda = document.getElementById('regWoreda').value;
        
        if(!name || !email || !password || !zone || !woreda) { alert('Please fill all fields'); return; }
        if(password !== confirm) { alert('Passwords do not match'); return; }
        if(users.find(u => u.email === email)) { alert('Email already registered'); return; }
        
        const newUser = { 
            id: 'USER_' + Date.now(), 
            name, 
            email, 
            password: btoa(password), 
            role: 'data_collector', 
            zone, 
            woreda, 
            status: 'pending', 
            registeredAt: new Date().toISOString() 
        };
        users.push(newUser);
        saveData();
        logActivity(`New registration request: ${name} (${email}) - Zone: ${zone}, Woreda: ${woreda}`);
        sendApprovalEmail(newUser);
        alert(`Registration submitted! An approval request has been sent to the admin.\n\nYou will be able to login once approved.`);
        showLoginForm();
        document.getElementById('regName').value = '';
        document.getElementById('regEmail').value = '';
        document.getElementById('regPassword').value = '';
        document.getElementById('regConfirmPassword').value = '';
        document.getElementById('regZone').value = '';
        document.getElementById('regWoreda').value = '';
    }

    function loginUser() {
        const email = document.getElementById('userEmail').value;
        const password = document.getElementById('userPassword').value;
        const user = users.find(u => u.email === email && atob(u.password) === password && u.role === 'data_collector');
        if(user && user.status === 'approved') {
            currentUser = user;
            logActivity(`${user.name} (${user.zone}/${user.woreda}) logged in`);
            showUserApp();
        } else if(user && user.status === 'pending') {
            alert('Your account is pending admin approval. Please wait.');
        } else {
            alert('Invalid email or password');
        }
    }

    function loginAdmin() {
        const email = document.getElementById('adminEmail').value;
        const password = document.getElementById('adminPassword').value;
        const admin = users.find(u => u.email === email && atob(u.password) === password && u.role === 'admin');
        if(admin) {
            currentUser = admin;
            logActivity(`Admin ${admin.name} logged in`);
            showAdminApp();
        } else {
            alert('Invalid admin credentials. Use: admin@tarl.com / admin123');
        }
    }

    function showUserApp() {
        document.getElementById('authSection').style.display = 'none';
        document.getElementById('adminApp').style.display = 'none';
        document.getElementById('userApp').style.display = 'block';
        document.getElementById('userLocationDisplay').innerHTML = `<i class="fas fa-map-marker-alt"></i> ${currentUser.zone} - ${currentUser.woreda}`;
        document.getElementById('userNameDisplay').innerText = currentUser.name;
        document.getElementById('userStudentZone').value = currentUser.zone;
        document.getElementById('userStudentWoreda').value = currentUser.woreda;
        updateUserApp();
    }

    function showAdminApp() {
        document.getElementById('authSection').style.display = 'none';
        document.getElementById('userApp').style.display = 'none';
        document.getElementById('adminApp').style.display = 'block';
        document.getElementById('adminName').innerText = currentUser.name;
        updateAdminApp();
        updateEmailSimulator();
    }

    function logout() {
        logActivity(`${currentUser?.name} logged out`);
        currentUser = null;
        document.getElementById('authSection').style.display = 'block';
        document.getElementById('adminApp').style.display = 'none';
        document.getElementById('userApp').style.display = 'none';
        document.getElementById('userEmail').value = '';
        document.getElementById('userPassword').value = '';
        document.getElementById('adminEmail').value = 'admin@tarl.com';
        document.getElementById('adminPassword').value = 'admin123';
        selectLoginRole('user');
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

    // ADMIN FUNCTIONS
    function updateAdminApp() {
        const dataCollectors = users.filter(u => u.role === 'data_collector');
        const pendingUsers = dataCollectors.filter(u => u.status === 'pending');
        document.getElementById('adminTotalUsers').textContent = dataCollectors.length;
        document.getElementById('adminPendingUsers').textContent = pendingUsers.length;
        document.getElementById('adminTotalStudents').textContent = students.length;
        
        document.getElementById('adminUserStats').innerHTML = `<div class="alert alert-info">Total Data Collectors: ${dataCollectors.length}<br>Approved: ${dataCollectors.filter(u=>u.status==='approved').length}<br>Pending: ${pendingUsers.length}</div>`;
        document.getElementById('adminSchoolStats').innerHTML = `<div class="alert alert-success">Total Students: ${students.length}<br>Schools: ${[...new Set(students.map(s=>s.schoolName))].length}<br>Woredas: ${[...new Set(students.map(s=>s.woreda))].length}</div>`;
        
        const tbody = document.getElementById('adminUsersTableBody');
        tbody.innerHTML = '';
        dataCollectors.forEach(u => {
            tbody.innerHTML += `<tr style="${u.status === 'pending' ? 'background:#fef3c7;' : ''}">
                <td>${escapeHtml(u.name)}</td><td>${escapeHtml(u.email)}</td><td>${escapeHtml(u.zone)}</td><td>${escapeHtml(u.woreda)}</td>
                <td>Data Collector</td><td>${u.status === 'approved' ? '<span class="level-badge level-4">Approved</span>' : '<span class="level-badge level-2">Pending</span>'}</td>
                <td>${new Date(u.registeredAt).toLocaleDateString()}</td>
                <td>${u.status === 'pending' ? `<button class="btn btn-success btn-sm" onclick="approveUser('${u.id}')"><i class="fas fa-check"></i> Approve</button> <button class="btn btn-danger btn-sm" onclick="rejectUser('${u.id}')"><i class="fas fa-times"></i> Reject</button>` : `<button class="btn btn-danger btn-sm" onclick="deleteUser('${u.id}')"><i class="fas fa-trash"></i> Delete</button>`}</td>
            <tr>`;
        });
    }

    function updateAdminActivityLog() {
        const container = document.getElementById('adminActivityLog');
        if(container) {
            container.innerHTML = '<div style="max-height:300px; overflow-y:auto;">' + activityLog.slice(0,15).map(log => `<div style="padding:5px; border-bottom:1px solid #e2e8f0; font-size:0.7rem;">${new Date(log.timestamp).toLocaleString()}: ${log.message}</div>`).join('') + '</div>';
        }
    }

    function approveUser(userId) {
        const user = users.find(u => u.id === userId);
        if(user) { 
            user.status = 'approved'; 
            pendingEmails = pendingEmails.filter(e => e.userId !== userId);
            saveData(); 
            logActivity(`User ${user.name} was APPROVED`); 
            updateAdminApp(); 
            updateEmailSimulator();
            showToast(`${user.name} approved!`);
        }
    }

    function rejectUser(userId) {
        const user = users.find(u => u.id === userId);
        if(user) { 
            users = users.filter(u => u.id !== userId); 
            pendingEmails = pendingEmails.filter(e => e.userId !== userId);
            saveData(); 
            logActivity(`User ${user.name} was REJECTED`); 
            updateAdminApp(); 
            updateEmailSimulator();
            showToast(`${user.name} rejected.`, 'error');
        }
    }

    function deleteUser(userId) {
        if(confirm('Delete this user?')) {
            const user = users.find(u => u.id === userId);
            users = users.filter(u => u.id !== userId);
            saveData();
            logActivity(`User ${user?.name} was DELETED`);
            updateAdminApp();
            showToast(`${user?.name} deleted.`);
        }
    }

    function generateAdminReport() {
        const format = document.getElementById('adminReportFormat').value;
        let html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>TaRL Admin Report</title><style>body{font-family:Arial;margin:40px;}</style></head><body>
        <h1>TaRL Department - Admin Report</h1><p>Generated: ${new Date().toLocaleString()}</p>
        <h2>Users</h2><table border="1"><tr><th>Name</th><th>Email</th><th>Zone</th><th>Woreda</th><th>Status</th></tr>`;
        users.filter(u=>u.role==='data_collector').forEach(u => html += `<tr><td>${u.name}</td><td>${u.email}</td><td>${u.zone}</td><td>${u.woreda}</td><td>${u.status}</td>`);
        html += `</table><h2>Students</h2><table border="1"><tr><th>Name</th><th>School</th><th>Zone</th><th>Woreda</th><th>English</th><th>Oromo</th></tr>`;
        students.forEach(s => html += `<tr><td>${s.name}</td><td>${s.schoolName}</td><td>${s.zone}</td><td>${s.woreda}</td><td>${s.englishScore}/5</td><td>${s.oromoScore}/5</td>`);
        html += `</table></body></html>`;
        if(format === 'html') { const w=window.open(); w.document.write(html); w.document.close(); }
        else { const div=document.createElement('div'); div.innerHTML=html; document.body.appendChild(div); html2pdf().from(div).set({margin:1}).save(); setTimeout(()=>document.body.removeChild(div),1000); }
    }

    // USER FUNCTIONS
    function getUserStudents() { return students.filter(s => s.zone === currentUser?.zone && s.woreda === currentUser?.woreda); }

    function registerUserStudent() {
        const learnerId = 'LRN_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4).toUpperCase();
        document.getElementById('userLearnerId').value = learnerId;
        const englishScore = (parseInt(document.getElementById('userEngBeginners').value)||0) + (parseInt(document.getElementById('userEngAlphabet').value)||0) + 
                             (parseInt(document.getElementById('userEngWords').value)||0) + (parseInt(document.getElementById('userEngSentence').value)||0) + 
                             (parseInt(document.getElementById('userEngStory').value)||0);
        const oromoScore = (parseInt(document.getElementById('userOromoBeginners').value)||0) + (parseInt(document.getElementById('userOromoQubee').value)||0) + 
                           (parseInt(document.getElementById('userOromoWord').value)||0) + (parseInt(document.getElementById('userOromoSentence').value)||0) + 
                           (parseInt(document.getElementById('userOromoStory').value)||0);
        students.push({
            id: learnerId, zone: currentUser.zone, woreda: currentUser.woreda,
            schoolId: document.getElementById('userStudentSchoolId').value, schoolName: document.getElementById('userStudentSchoolName').value,
            name: document.getElementById('userStudentName').value, sex: document.getElementById('userStudentSex').value,
            grade: document.getElementById('userStudentGrade').value, specialNeeds: document.getElementById('userSpecialNeeds').value,
            englishScore: englishScore, oromoScore: oromoScore, englishLevel: getEnglishLevel(englishScore), oromoLevel: getOromoLevel(oromoScore)
        });
        saveData();
        updateUserApp();
        showToast(`Student registered!`);
        logActivity(`${currentUser.name} registered student: ${document.getElementById('userStudentName').value}`);
        clearUserStudentForm();
    }

    function clearUserStudentForm() {
        ['userStudentSchoolId','userStudentSchoolName','userStudentName','userEngBeginners','userEngAlphabet','userEngWords','userEngSentence','userEngStory','userOromoBeginners','userOromoQubee','userOromoWord','userOromoSentence','userOromoStory','userLearnerId'].forEach(id => { if(document.getElementById(id)) document.getElementById(id).value = ''; });
    }

    function deleteUserStudent(id) { if(confirm('Delete student?')) { students = students.filter(s => s.id !== id); saveData(); updateUserApp(); logActivity(`${currentUser.name} deleted student`); } }

    function updateUserApp() {
        const userStudents = getUserStudents();
        document.getElementById('userTotalStudents').textContent = userStudents.length;
        document.getElementById('userTotalSchools').textContent = [...new Set(userStudents.map(s=>s.schoolName))].length;
        let eng=0,oro=0;
        userStudents.forEach(s=>{ eng+=s.englishScore; oro+=s.oromoScore; });
        document.getElementById('userAvgEnglish').textContent = userStudents.length ? ((eng/(userStudents.length*5))*100).toFixed(0) : '0';
        document.getElementById('userAvgOromo').textContent = userStudents.length ? ((oro/(userStudents.length*5))*100).toFixed(0) : '0';
        
        const tbody = document.getElementById('userStudentTableBody');
        tbody.innerHTML = '';
        userStudents.forEach(s => {
            tbody.innerHTML += `<tr><td>${s.id.substring(0,10)}</td><td>${escapeHtml(s.name)}</td><td>${escapeHtml(s.schoolName)}</td><td>Kutaa ${s.grade}</td>
            <td>${s.englishScore}/5 <span class="level-badge ${s.englishLevel.class}">${s.englishLevel.name}</span></td>
            <td>${s.oromoScore}/5 <span class="level-badge ${s.oromoLevel.class}">${s.oromoLevel.name}</span></td>
            <td><button class="btn btn-danger btn-sm" onclick="deleteUserStudent('${s.id}')"><i class="fas fa-trash"></i></button></td>
            </tr>`;
        });
        
        const wMap = new Map();
        userStudents.forEach(s => { const key = `${s.zone}|${s.woreda}|${s.grade}`; if(!wMap.has(key)) wMap.set(key, { zone:s.zone, woreda:s.woreda, grade:s.grade, count:0, eng:0, oro:0 }); const d = wMap.get(key); d.count++; d.eng+=s.englishScore; d.oro+=s.oromoScore; });
        const wTbody = document.getElementById('userWoredaTableBody');
        wTbody.innerHTML = '';
        for(let d of wMap.values()) { wTbody.innerHTML += `<tr><td>${d.zone}</td><td>${d.woreda}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td>
        <td>${((d.eng/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oro/(d.count*5))*100).toFixed(1)}%</td></tr>`; }
        
        const sMap = new Map();
        userStudents.forEach(s => { const key = `${s.schoolName}|${s.grade}`; if(!sMap.has(key)) sMap.set(key, { school:s.schoolName, woreda:s.woreda, grade:s.grade, count:0, eng:0, oro:0 }); const d = sMap.get(key); d.count++; d.eng+=s.englishScore; d.oro+=s.oromoScore; });
        const sTbody = document.getElementById('userSchoolTableBody');
        sTbody.innerHTML = '';
        for(let d of sMap.values()) { sTbody.innerHTML += `<tr><td>${d.school}</td><td>${d.woreda}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td>
        <td>${((d.eng/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oro/(d.count*5))*100).toFixed(1)}%</td></tr>`; }
        
        const grades = ['3','4','5'];
        const engData = grades.map(g => { const gs = userStudents.filter(s=>s.grade===g); if(gs.length===0) return 0; return (gs.reduce((sum,st)=>sum+st.englishScore,0)/(gs.length*5))*100; });
        const oromoData = grades.map(g => { const gs = userStudents.filter(s=>s.grade===g); if(gs.length===0) return 0; return (gs.reduce((sum,st)=>sum+st.oromoScore,0)/(gs.length*5))*100; });
        if(window.userEngChart) window.userEngChart.destroy();
        window.userEngChart = new Chart(document.getElementById('userEnglishChart'), { type: 'bar', data: { labels: ['Kutaa 3','Kutaa 4','Kutaa 5'], datasets: [{ label: 'English %', data: engData, backgroundColor: '#3b82f6' }] } });
        if(window.userOromoChart) window.userOromoChart.destroy();
        window.userOromoChart = new Chart(document.getElementById('userOromoChart'), { type: 'bar', data: { labels: ['Kutaa 3','Kutaa 4','Kutaa 5'], datasets: [{ label: 'Afaan Oromoo %', data: oromoData, backgroundColor: '#10b981' }] } });
        
        const levelCounts = [0,0,0,0,0,0];
        userStudents.forEach(s => levelCounts[s.englishLevel.code-1]++);
        if(window.userLevelChart) window.userLevelChart.destroy();
        window.userLevelChart = new Chart(document.getElementById('userLevelChart'), { type: 'pie', data: { labels: ['Beginner','Alphabet','Words','Sentence','Story','Computed'], datasets: [{ data: levelCounts, backgroundColor: ['#ef4444','#f59e0b','#fef3c7','#d1fae5','#dbeafe','#e0e7ff'] }] } });
        
        const total = userStudents.length;
        const avgEng = total ? (userStudents.reduce((s,st)=>s+st.englishScore,0)/total)*20 : 0;
        const avgOro = total ? (userStudents.reduce((s,st)=>s+st.oromoScore,0)/total)*20 : 0;
        document.getElementById('userProgramSummary').innerHTML = `<div class="grid-3"><div class="alert alert-info"><strong>My Enrollment</strong><br>Total: ${total}</div>
        <div class="alert alert-success"><strong>My Average Scores</strong><br>English: ${avgEng.toFixed(1)}%<br>Afaan Oromoo: ${avgOro.toFixed(1)}%</div>
        <div class="alert alert-info"><strong>My Coverage</strong><br>Schools: ${[...new Set(userStudents.map(s=>s.schoolName))].length}<br>Woreda: ${currentUser.woreda}</div></div>`;
    }

    function updateUserGrouping() {
        const grade = document.getElementById('userGroupGrade').value;
        const userStudents = getUserStudents();
        const gradeStudents = userStudents.filter(s => s.grade === grade);
        const container = document.getElementById('userGroupingDisplay');
        if(gradeStudents.length === 0) { container.innerHTML = '<div class="alert alert-info">No students for this grade</div>'; return; }
        const groups = {1:[],2:[],3:[],4:[],5:[],6:[]};
        gradeStudents.forEach(s => groups[s.englishLevel.code].push(s.name));
        let html = '<div><h4>English Grouping</h4>';
        const levelNames = ['Beginner','Alphabet','Words','Sentence','Story','Computed'];
        for(let i=1;i<=6;i++) { if(groups[i].length > 0) html += `<div class="group-card"><strong>Level ${i}: ${levelNames[i-1]}</strong> (${groups[i].length})<br><small>${groups[i].join(', ')}</small></div>`; }
        html += '</div>';
        container.innerHTML = html;
    }

    function generateUserReport() {
        const type = document.getElementById('userReportType').value;
        const format = document.getElementById('userReportFormat').value;
        const userStudents = getUserStudents();
        let html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>TaRL Report - ${currentUser.name}</title><style>body{font-family:Arial;margin:40px;}table{border-collapse:collapse;width:100%;}th,td{border:1px solid #ddd;padding:8px;}</style></head><body>
        <h1>TaRL Program Report</h1><p>Data Collector: ${currentUser.name}<br>Zone: ${currentUser.zone}<br>Woreda: ${currentUser.woreda}<br>Generated: ${new Date().toLocaleString()}</p>`;
        if(type === 'woreda') {
            const map = new Map();
            userStudents.forEach(s => { const key = `${s.grade}`; if(!map.has(key)) map.set(key, { grade:s.grade, count:0, eng:0, oro:0 }); const d=map.get(key); d.count++; d.eng+=s.englishScore; d.oro+=s.oromoScore; });
            html += `<h2>Woreda Summary</h2><table border="1"><tr><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th>`;
            for(let d of map.values()) html += `<tr><td>Kutaa ${d.grade}</td><td>${d.count}</td><td>${((d.eng/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oro/(d.count*5))*100).toFixed(1)}%</td>`;
            html += ` licensierad`;
        } else if(type === 'school') {
            const map = new Map();
            userStudents.forEach(s => { const key = `${s.schoolName}|${s.grade}`; if(!map.has(key)) map.set(key, { school:s.schoolName, grade:s.grade, count:0, eng:0, oro:0 }); const d=map.get(key); d.count++; d.eng+=s.englishScore; d.oro+=s.oromoScore; });
            html += `<h2>School Summary</h2><table border="1"><tr><th>School</th><th>Grade</th><th>Students</th><th>English %</th><th>Oromo %</th>`;
            for(let d of map.values()) html += `<tr><td>${d.school}</td><td>Kutaa ${d.grade}</td><td>${d.count}</td><td>${((d.eng/(d.count*5))*100).toFixed(1)}%</td><td>${((d.oro/(d.count*5))*100).toFixed(1)}%</td>`;
            html += `</table>`;
        } else {
            html += `<h2>Student Roster</h2><table border="1"><tr><th>Name</th><th>School</th><th>Grade</th><th>English</th><th>Oromo</th>`;
            userStudents.forEach(s => html += `<tr><td>${s.name}</td><td>${s.schoolName}</td><td>Kutaa ${s.grade}</td><td>${s.englishScore}/5</td><td>${s.oromoScore}/5</td>`);
            html += `</table>`;
        }
        html += `</body></html>`;
        if(format === 'html') { const w=window.open(); w.document.write(html); w.document.close(); }
        else { const div=document.createElement('div'); div.innerHTML=html; document.body.appendChild(div); html2pdf().from(div).set({margin:1}).save(); setTimeout(()=>document.body.removeChild(div),1000); }
    }

    function escapeHtml(str){ if(!str) return ''; return str.replace(/[&<>]/g, m => m==='&'?'&amp;':m==='<'?'&lt;':'&gt;'); }

    document.addEventListener('click', function(e) {
        if(e.target.closest('.nav-btn') && !e.target.closest('.nav-btn').getAttribute('onclick')) {
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            e.target.closest('.nav-btn').classList.add('active');
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            const sectionId = e.target.closest('.nav-btn').getAttribute('data-section');
            if(sectionId && document.getElementById(sectionId)) document.getElementById(sectionId).classList.add('active');
        }
    });

    function updateAll() { }
    loadData();
    selectLoginRole('user');
</script>
</body>
</html>
