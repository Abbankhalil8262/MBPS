<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>MAGAJI BALA PRIMARY SCHOOL - Offline Management</title>
<style>
  :root{--bg:#f6f8fb;--card:#fff;--accent:#0b5ed7;--muted:#666}
  *{box-sizing:border-box}
  body{font-family:system-ui,Segoe UI,Roboto,Helvetica,Arial;background:var(--bg);margin:0;color:#111}
  header{background:linear-gradient(90deg,#082f6b,#0b5ed7);color:#fff;padding:18px}
  .container{max-width:980px;margin:18px auto;padding:12px}
  .card{background:var(--card);border-radius:10px;box-shadow:0 6px 18px rgba(18,38,63,0.06);padding:16px}
  h1,h2,h3,h4{margin:6px 0}
  .muted{color:var(--muted)}
  .grid{display:grid;gap:12px}
  .grid.cols-2{grid-template-columns:1fr 320px}
  input,select,textarea{width:100%;padding:8px;border-radius:6px;border:1px solid #ddd}
  button{background:var(--accent);color:#fff;padding:8px 10px;border:none;border-radius:8px;cursor:pointer}
  .small{font-size:13px}
  nav{display:flex;gap:8px;flex-wrap:wrap}
  .table{width:100%;border-collapse:collapse}
  .table th,.table td{border-bottom:1px solid #eee;padding:8px;text-align:left}
  .hidden{display:none}
  footer{font-size:12px;color:var(--muted);text-align:center;margin-top:18px}
  .toolbar{display:flex;gap:8px;align-items:center}
  .center{display:flex;align-items:center;justify-content:center}
  .top-actions{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
  .badge{display:inline-block;padding:6px 8px;border-radius:999px;background:#e9f2ff;color:#0b5ed7;font-weight:600}
  .muted-2{color:#888;font-size:13px}
  pre{white-space:pre-wrap;word-break:break-word;background:#f8f9fa;padding:8px;border-radius:8px;border:1px solid #eee}
</style>
</head>
<body>
  <header>
    <div style="max-width:980px;margin:0 auto;display:flex;justify-content:space-between;align-items:center">
      <div>
        <h1>MAGAJI BALA PRIMARY SCHOOL, YARI-BORI</h1>
        <div class="small">Offline school management — Admissions · Exams · Report cards · Subjects</div>
      </div>
      <div class="small">Offline • Local-only • Data saved in browser</div>
    </div>
  </header>

  <div class="container">
    <div id="splash" class="card center" style="flex-direction:column;gap:12px;margin-bottom:12px">
      <div style="font-size:22px;font-weight:700">MAGAJI BALA PRIMARY SCHOOL</div>
      <div class="muted">Offline Management • YARI-BORI</div>
      <div><button onclick="goToLogin()">Open App</button></div>
    </div>

    <div id="appArea" class="card hidden">
      <!-- LOGIN -->
      <div id="loginScreen">
        <div class="grid cols-2">
          <div>
            <h2>Login</h2>
            <p class="muted">Enter username and password to continue. This app works fully offline using your device storage.</p>
            <div style="margin-top:12px">
              <label>Username</label>
              <input id="loginUser" placeholder="admin or staff" />
            </div>
            <div style="margin-top:8px">
              <label>Password</label>
              <input id="loginPwd" type="password" />
            </div>
            <div style="margin-top:12px;display:flex;gap:8px">
              <button onclick="attemptLogin()">Login</button>
              <button onclick="prefillTest()" style="background:#6c757d">Fill demo</button>
            </div>
            <p class="small muted" style="margin-top:12px">Default admin: <span id="adminCred"></span> — change in Settings.</p>
          </div>
          <div>
            <div class="card">
              <h3>Quick demo accounts</h3>
              <ul>
                <li><strong>Admin</strong>: <span id="adminCred2"></span></li>
                <li><strong>Staff</strong>: staff1 / staff123</li>
              </ul>
              <p class="small muted">Admin can access Settings and export/import data.</p>
            </div>

            <div style="margin-top:12px" class="card">
              <h4>About</h4>
              <p class="small muted">This single-file app stores data in your browser (localStorage). Use Export / Import JSON for backup.</p>
            </div>
          </div>
        </div>
      </div>

      <!-- MAIN APP -->
      <div id="mainScreen" class="hidden">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <strong id="currentUser"></strong>
            <div class="muted small" id="currentRole"></div>
          </div>
          <div class="top-actions">
            <span class="badge" id="onlineBadge">Offline</span>
            <button onclick="logout()" style="background:#d63384">Logout</button>
          </div>
        </div>

        <nav style="margin-top:12px">
          <button onclick="showSection('dashboard')">Dashboard</button>
          <button onclick="showSection('admissions')">Admissions</button>
          <button onclick="showSection('exams')">Exams</button>
          <button onclick="showSection('reportcards')">Report Cards</button>
          <button onclick="showSection('subjects')">Subjects</button>
          <button onclick="showSection('settings')">Settings</button>
        </nav>

        <div id="sections" style="margin-top:12px">
          <div id="dashboard" class="card section">
            <h3>Dashboard</h3>
            <div style="display:flex;gap:12px;flex-wrap:wrap">
              <div class="card" style="flex:1;min-width:220px">
                <h4>Students</h4>
                <div id="statStudents">0</div>
              </div>
              <div class="card" style="flex:1;min-width:220px">
                <h4>Exams recorded</h4>
                <div id="statExams">0</div>
              </div>
              <div class="card" style="flex:1;min-width:220px">
                <h4>Subjects</h4>
                <div id="statSubjects">0</div>
              </div>
            </div>
          </div>

          <div id="admissions" class="card section hidden">
            <h3>Admissions / Student Register</h3>
            <div style="display:flex;gap:12px">
              <div style="flex:1">
                <label>Search</label>
                <input id="searchStudent" oninput="renderStudents()" placeholder="search by name or id" />
              </div>
              <div style="width:260px">
                <label>Class</label>
                <select id="filterClass" onchange="renderStudents()">
                  <option value="">All</option>
                </select>
              </div>
            </div>

            <div style="margin-top:12px;display:flex;gap:12px;align-items:flex-start">
              <div style="flex:1">
                <table class="table" id="studentsTable">
                  <thead><tr><th>ID</th><th>Name</th><th>Class</th><th>Actions</th></tr></thead>
                  <tbody></tbody>
                </table>
              </div>
              <div style="width:320px">
                <h4>Add / Edit student</h4>
                <div>
                  <label>Full name</label>
                  <input id="stuName" />
                </div>
                <div style="margin-top:8px">
                  <label>Class</label>
                  <select id="stuClass"></select>
                </div>
                <div style="margin-top:8px">
                  <label>Gender</label>
                  <select id="stuGender"><option>Male</option><option>Female</option></select>
                </div>
                <div style="margin-top:8px">
                  <label>Notes</label>
                  <textarea id="stuNotes" rows="3"></textarea>
                </div>
                <div style="margin-top:8px;display:flex;gap:8px">
                  <button onclick="saveStudent()">Save</button>
                  <button onclick="resetStudentForm()" style="background:#6c757d">Clear</button>
                </div>
              </div>
            </div>
          </div>

          <div id="exams" class="card section hidden">
            <h3>Exams</h3>
            <div style="display:flex;gap:12px">
              <div style="flex:1">
                <label>Choose student</label>
                <select id="examStudent"></select>
                <label style="margin-top:8px">Subject</label>
                <select id="examSubject"></select>
                <label style="margin-top:8px">Score</label>
                <input id="examScore" type="number" min="0" max="100" />
                <div style="margin-top:8px;display:flex;gap:8px">
                  <button onclick="saveExam()">Record Exam</button>
                  <button onclick="renderExams()" style="background:#6c757d">Refresh</button>
                </div>

                <h4 style="margin-top:12px">Recorded exams</h4>
                <table class="table" id="examsTable"><thead><tr><th>Student</th><th>Subject</th><th>Score</th><th>Date</th><th>Action</th></tr></thead><tbody></tbody></table>
              </div>
              <div style="width:260px">
                <h4>Quick actions</h4>
                <button onclick="bulkGenerateReport()" style="width:100%">Generate Report Card for Selected Student</button>
              </div>
            </div>
          </div>

          <div id="reportcards" class="card section hidden">
            <h3>Report Cards</h3>
            <div>
              <label>Pick student</label>
              <select id="reportStudent"></select>
              <div style="margin-top:8px">
                <button onclick="renderReport()">View Report</button>
                <button onclick="downloadReportPDF()" style="background:#198754">Download PDF</button>
              </div>
              <div id="reportArea" style="margin-top:12px" class="card hidden"></div>
            </div>
          </div>

          <div id="subjects" class="card section hidden">
            <h3>Subjects</h3>
            <div style="display:flex;gap:12px">
              <div style="flex:1">
                <table class="table" id="subjectsTable"><thead><tr><th>Subject</th><th>Action</th></tr></thead><tbody></tbody></table>
              </div>
              <div style="width:320px">
                <h4>Add Subject</h4>
                <input id="newSubject" placeholder="e.g. Mathematics" />
                <div style="margin-top:8px"><button onclick="addSubject()">Add</button></div>
              </div>
            </div>
          </div>

          <div id="settings" class="card section hidden">
            <h3>Settings</h3>
            <div style="display:flex;gap:12px">
              <div style="flex:1">
                <h4>Data</h4>
                <div class="toolbar">
                  <button onclick="exportData()">Export JSON</button>
                  <label style="display:inline-block;padding:8px;background:#eee;border-radius:6px;cursor:pointer">Import JSON
                    <input id="importFile" type="file" accept="application/json" style="display:none" onchange="importFile(event)"/>
                  </label>
                  <button onclick="clearAll()" style="background:#dc3545">Clear all</button>
                </div>

                <h4 style="margin-top:12px">Admin account</h4>
                <div>
                  <label>Admin username</label>
                  <input id="adminUser" />
                  <label style="margin-top:8px">Admin password</label>
                  <input id="adminPwd" />
                  <div style="margin-top:8px"><button onclick="saveSettings()">Save</button></div>
                </div>
              </div>

              <div style="width:320px">
                <h4>Info</h4>
                <p class="small muted">App author: Offline demo. Keep a backup of exported JSON.</p>
              </div>
            </div>
          </div>

        </div>

        <footer>MAGAJI BALA PRIMARY SCHOOL • Offline App • Data stored locally in your browser</footer>
      </div>
    </div>
  </div>

<script>
/* ---------------------------
   DEFAULT CONFIG & BOOT
   --------------------------- */
const DEFAULT_ADMIN = {username: 'Abbankhlil1', password: '826211'};
const DEFAULT_CLASSES = ['Nursery','Primary 1','Primary 2','Primary 3','Primary 4','Primary 5','Primary 6','Primary 7','Married Women'];
const DEFAULT_SUBJECTS = ['Mathematics','English','Science'];

const STORAGE_KEY = 'magaji_offline_data_v1';

function loadState(){
  const raw = localStorage.getItem(STORAGE_KEY);
  if(!raw) {
    const init = {
      admin: DEFAULT_ADMIN,
      staff: [{username:'staff1', password:'staff123', name:'Staff One'}],
      classes: DEFAULT_CLASSES.slice(),
      students: [],
      subjects: DEFAULT_SUBJECTS.slice(),
      exams: []
    };
    localStorage.setItem(STORAGE_KEY, JSON.stringify(init));
    return init;
  }
  try { return JSON.parse(raw); } catch(e) {
    console.error('corrupt storage',e);
    localStorage.removeItem(STORAGE_KEY);
    return loadState();
  }
}
let state = loadState();
let currentUser = null;
let editingStudentId = null;

/* ---------------------------
   UI BOOT
   --------------------------- */
document.getElementById('adminCred').innerText = `${state.admin.username} / ${state.admin.password}`;
document.getElementById('adminCred2').innerText = `${state.admin.username} / ${state.admin.password}`;
document.getElementById('splash').classList.remove('hidden');

function goToLogin(){
  document.getElementById('splash').classList.add('hidden');
  document.getElementById('appArea').classList.remove('hidden');
  showLogin();
}

function showLogin(){
  document.getElementById('loginScreen').classList.remove('hidden');
  document.getElementById('mainScreen').classList.add('hidden');
}

function showApp(){
  document.getElementById('loginScreen').classList.add('hidden');
  document.getElementById('mainScreen').classList.remove('hidden');
  showSection('dashboard');
  renderAll();
}

/* ---------------------------
   AUTH
   --------------------------- */
function attemptLogin(){
  const u = document.getElementById('loginUser').value.trim();
  const p = document.getElementById('loginPwd').value;
  if(!u||!p){ alert('Enter username and password'); return; }
  if(u === state.admin.username && p === state.admin.password){
    currentUser = {role:'admin', name:'Administrator', username:u};
    loginSuccess(); return;
  }
  const staff = state.staff.find(s=>s.username===u && s.password===p);
  if(staff){ currentUser = {role:'staff', name:staff.name, username:staff.username}; loginSuccess(); return; }
  alert('Invalid credentials');
}
function prefillTest(){ document.getElementById('loginUser').value = state.admin.username; document.getElementById('loginPwd').value = state.admin.password; }
function loginSuccess(){
  document.getElementById('currentUser').innerText = `Hello, ${currentUser.name}`;
  document.getElementById('currentRole').innerText = `Role: ${currentUser.role}`;
  showApp();
}
function logout(){ currentUser = null; document.getElementById('loginUser').value=''; document.getElementById('loginPwd').value=''; showLogin(); }

/* ---------------------------
   NAV / SECTIONS
   --------------------------- */
function showSection(id){
  document.querySelectorAll('.section').forEach(s=>s.classList.add('hidden'));
  const el = document.getElementById(id);
  if(el) el.classList.remove('hidden');
  if(id==='admissions') renderStudents();
  if(id==='exams') renderExams();
  if(id==='subjects') renderSubjects();
  if(id==='reportcards') populateReportSelectors();
}

/* ---------------------------
   STORAGE HELPERS
   --------------------------- */
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); updateStats(); }
function exportData(){
  const blob = new Blob([JSON.stringify(state,null,2)],{type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download='magaji_data_export.json'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
}
function importFile(ev){
  const f = ev.target.files[0];
  if(!f) return;
  const reader = new FileReader();
  reader.onload = e => {
    try {
      const obj = JSON.parse(e.target.result);
      if(!confirm('Replace local data with imported data?')) return;
      state = obj;
      saveState();
      renderAll();
      alert('Import successful');
    } catch(err){ alert('Invalid JSON file'); }
  };
  reader.readAsText(f);
}
function clearAll(){
  if(!confirm('Clear ALL local data? This cannot be undone.')) return;
  localStorage.removeItem(STORAGE_KEY);
  state = loadState();
  renderAll();
  alert('Cleared');
}

/* ---------------------------
   STUDENTS (Admissions)
   --------------------------- */
function idGen(prefix='ST'){ return prefix + Date.now().toString().slice(-6); }
function resetStudentForm(){ editingStudentId = null; document.getElementById('stuName').value=''; document.getElementById('stuClass').value = state.classes[0] || ''; document.getElementById('stuGender').value='Male'; document.getElementById('stuNotes').value=''; }
function saveStudent(){
  const name = document.getElementById('stuName').value.trim();
  const klass = document.getElementById('stuClass').value;
  const gender = document.getElementById('stuGender').value;
  const notes = document.getElementById('stuNotes').value;
  if(!name){ alert('Student name required'); return; }
  if(editingStudentId){
    const st = state.students.find(s=>s.id===editingStudentId);
    if(st) Object.assign(st,{name,klass,gender,notes});
    editingStudentId = null;
  } else {
    state.students.push({id:idGen(), name, klass, gender, notes});
  }
  saveState(); resetStudentForm(); renderStudents(); populateExamSelectors();
}
function renderStudents(){
  const q = document.getElementById('searchStudent').value.toLowerCase();
  const cls = document.getElementById('filterClass').value;
  const tbody = document.querySelector('#studentsTable tbody'); tbody.innerHTML='';
  const list = state.students.filter(s=> (s.name.toLowerCase().includes(q) || s.id.toLowerCase().includes(q)) && (cls? s.klass===cls : true) );
  list.forEach(s=>{
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${s.id}</td><td>${s.name}</td><td>${s.klass}</td><td><button onclick="editStudent('${s.id}')">Edit</button> <button onclick="deleteStudent('${s.id}')" style="background:#dc3545">Del</button></td>`;
    tbody.appendChild(tr);
  });
}
function editStudent(id){
  const s = state.students.find(x=>x.id===id); if(!s) return;
  editingStudentId = id; document.getElementById('stuName').value=s.name; document.getElementById('stuClass').value=s.klass || state.classes[0]; document.getElementById('stuGender').value=s.gender; document.getElementById('stuNotes').value=s.notes;
}
function deleteStudent(id){
  if(!confirm('Delete student?')) return;
  state.students = state.students.filter(s=>s.id!==id);
  state.exams = state.exams.filter(e=>e.studentId!==id);
  saveState(); renderStudents(); renderExams(); populateReportSelectors();
}

/* ---------------------------
   SUBJECTS
   --------------------------- */
function addSubject(){
  const name = document.getElementById('newSubject').value.trim();
  if(!name) return alert('Enter subject');
  if(state.subjects.includes(name)) return alert('Subject exists');
  state.subjects.push(name); saveState(); document.getElementById('newSubject').value=''; renderSubjects(); populateExamSelectors();
}
function renderSubjects(){
  const tbody = document.querySelector('#subjectsTable tbody'); tbody.innerHTML='';
  state.subjects.forEach((s,idx)=>{
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${s}</td><td><button onclick="removeSubject(${idx})" style="background:#dc3545">Remove</button></td>`;
    tbody.appendChild(tr);
  });
  updateStats();
}
function removeSubject(idx){ if(!confirm('Remove subject?')) return; state.subjects.splice(idx,1); saveState(); renderSubjects(); }

/* ---------------------------
   EXAMS
   --------------------------- */
function populateExamSelectors(){
  const examStudent = document.getElementById('examStudent'); const examSubject = document.getElementById('examSubject'); const reportStudent = document.getElementById('reportStudent');
  examStudent.innerHTML = state.students.map(s=>`<option value="${s.id}">${s.name} (${s.id})</option>`).join('') || '<option value="">-- no students --</option>';
  reportStudent.innerHTML = '<option value="">-- pick --</option>' + state.students.map(s=>`<option value="${s.id}">${s.name}</option>`).join('');
  examSubject.innerHTML = state.subjects.map(s=>`<option>${s}</option>`).join('') || '<option value="">-- no subjects --</option>';
}
function saveExam(){
  const sid = document.getElementById('examStudent').value; const subj = document.getElementById('examSubject').value; const score = Number(document.getElementById('examScore').value);
  if(!sid || !subj || isNaN(score)) return alert('Choose student, subject and score');
  state.exams.push({id:'E'+Date.now().toString().slice(-6), studentId:sid, subject:subj, score, date:new Date().toISOString()});
  saveState(); renderExams(); renderReport(); populateReportSelectors();
}
function renderExams(){
  const tbody = document.querySelector('#examsTable tbody'); tbody.innerHTML='';
  state.exams.forEach(e=>{
    const s = state.students.find(st=>st.id===e.studentId);
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${s? s.name : e.studentId}</td><td>${e.subject}</td><td>${e.score}</td><td>${(new Date(e.date)).toLocaleString()}</td><td><button onclick="deleteExam('${e.id}')" style="background:#dc3545">Del</button></td>`;
    tbody.appendChild(tr);
  });
  updateStats();
}
function deleteExam(id){ if(!confirm('Delete exam?')) return; state.exams = state.exams.filter(e=>e.id!==id); saveState(); renderExams(); }

/* ---------------------------
   REPORTS
   --------------------------- */
function populateReportSelectors(){ populateExamSelectors(); }
function renderReport(){
  const sid = document.getElementById('reportStudent').value; if(!sid) return alert('Pick a student');
  const student = state.students.find(s=>s.id===sid);
  const exams = state.exams.filter(e=>e.studentId===sid);
  const area = document.getElementById('reportArea'); area.classList.remove('hidden');
  area.innerHTML = `<h4>Report for ${student.name} (${student.id})</h4>` + renderReportHtml(student,exams);
}
function renderReportHtml(student,exams){
  const rows = state.subjects.map(sub=>{
    const ex = exams.filter(e=>e.subject===sub);
    const avg = ex.length? Math.round(ex.reduce((a,b)=>a+b.score,0)/ex.length) : '-';
    return `<tr><td>${sub}</td><td>${avg}</td></tr>`;
  }).join('');
  return `<table class='table'><thead><tr><th>Subject</th><th>Avg score</th></tr></thead><tbody>${rows}</tbody></table>`;
}
function downloadReportPDF(){ alert('Use browser Print → Save as PDF to generate a PDF from this page.'); }

/* ---------------------------
   SETTINGS
   --------------------------- */
function saveSettings(){
  const u = document.getElementById('adminUser').value.trim();
  const p = document.getElementById('adminPwd').value;
  if(!u||!p) return alert('Enter admin username and password');
  state.admin.username = u; state.admin.password = p; saveState();
  document.getElementById('adminCred').innerText = `${u} / ${p}`;
  document.getElementById('adminCred2').innerText = `${u} / ${p}`;
  alert('Saved');
}

/* ---------------------------
   UTIL
   --------------------------- */
function updateStats(){ document.getElementById('statStudents').innerText = state.students.length; document.getElementById('statExams').innerText = state.exams.length; document.getElementById('statSubjects').innerText = state.subjects.length; }
function renderAll(){ renderSubjects(); populateClassFilters(); renderStudents(); renderExams(); populateExamSelectors(); populateReportSelectors(); updateStats(); document.getElementById('adminUser').value = state.admin.username; document.getElementById('adminPwd').value = state.admin.password; }
function populateClassFilters(){
  const sel = document.getElementById('filterClass'); const stuClass = document.getElementById('stuClass');
  sel.innerHTML = '<option value=\"\">All</option>' + state.classes.map(c=>`<option>${c}</option>`).join('');
  stuClass.innerHTML = state.classes.map(c=>`<option>${c}</option>`).join('');
}

/* ---------------------------
   INITIALIZE ON LOAD
   --------------------------- */
renderAll();

/* ---------------------------
   Helpful shortcuts (import sample data)
   --------------------------- */
function seedDemo(){
  state.students = [
    {id:idGen(),name:'Abdulrahman Usman',klass:'Primary 1',gender:'Male',notes:''},
    {id:idGen(),name:'Fatima Sani',klass:'Primary 2',gender:'Female',notes:''},
    {id:idGen(),name:'Amina Mohammed',klass:'Nursery',gender:'Female',notes:''}
  ];
  state.exams = [
    {id:'E'+Date.now().toString().slice(-6),studentId:state.students[0].id,subject:'Mathematics',score:78,date:new Date().toISOString()},
    {id:'E'+(Date.now()+1).toString().slice(-6),studentId:state.students[1].id,subject:'English',score:85,date:new Date().toISOString()}
  ];
  saveState(); renderAll();
}
function prefillTest(){
  document.getElementById('loginUser').value = state.admin.username; document.getElementById('loginPwd').value = state.admin.password;
}
function bulkGenerateReport(){ const sid = document.getElementById('examStudent').value; if(!sid) return alert('Select a student'); document.getElementById('reportStudent').value = sid; showSection('reportcards'); renderReport(); }

/* ---------------------------
   Run initial UI
   --------------------------- */
document.addEventListener('visibilitychange', ()=>{ if(document.visibilityState==='visible'){ renderAll(); } });

</script>
</body>
</html>![IMG_20251029_012450_984~2](https://github.com/user-attachments/assets/0069c9fc-5895-413d-8b88-255d590c0529)
