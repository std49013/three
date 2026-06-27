<!DOCTYPE html>
<html lang="th" data-theme="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="ระบบแนะนำห้องเรียนตามความถนัดและความสนใจ โรงเรียนศรียาภัย วิเคราะห์ด้วย AI เพื่อค้นหาแผนการเรียนที่เหมาะสมที่สุด">
    <meta name="keywords" content="โรงเรียนศรียาภัย, ระบบแนะนำห้องเรียน, SMTE, SMT, IEP, EIS, SMEP, แบบทดสอบค้นหาตัวเอง">
    <title>ระบบแนะนำห้องเรียน | โรงเรียนศรียาภัย</title>
    
    <style>
        /* =========================================
           1. CSS Variables & Theme (Light/Dark - ธีมเหลือง/แดง)
        ========================================= */
        :root {
            /* Light Theme Colors */
            --primary-color: #d32f2f; /* แดงเข้ม */
            --secondary-color: #f57c00; /* ส้ม */
            --accent-color: #fbc02d; /* เหลือง */
            --text-main: #333333;
            --text-muted: #666666;
            --bg-gradient: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%); /* พื้นหลังไล่สีเหลือง-ส้มอ่อน */
            
            /* Glassmorphism Variables */
            --glass-bg: rgba(255, 255, 255, 0.45);
            --glass-border: rgba(255, 255, 255, 0.7);
            --glass-shadow: 0 8px 32px 0 rgba(211, 47, 47, 0.15); /* เงาอมแดง */
            --glass-text: #333;
            --card-hover-bg: rgba(255, 255, 255, 0.8);
        }

        [data-theme="dark"] {
            /* Dark Theme Colors */
            --primary-color: #ff5252; /* แดงสว่าง */
            --secondary-color: #ffd600; /* เหลืองสว่าง */
            --accent-color: #ffab00; /* ส้มทอง */
            --text-main: #f0f0f0;
            --text-muted: #dddddd;
            --bg-gradient: linear-gradient(135deg, #3e1414 0%, #2a0808 50%, #4a3b1a 100%); /* พื้นหลังไล่สีแดงมืด-น้ำตาลทอง */
            
            /* Dark Glassmorphism */
            --glass-bg: rgba(20, 10, 10, 0.6);
            --glass-border: rgba(255, 255, 255, 0.1);
            --glass-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.6);
            --glass-text: #fff;
            --card-hover-bg: rgba(60, 20, 20, 0.8);
        }

        /* =========================================
           2. Global Reset & Typography
        ========================================= */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Sarabun', 'Prompt', sans-serif;
            transition: background 0.3s, color 0.3s;
        }

        body {
            background: var(--bg-gradient);
            background-attachment: fixed;
            color: var(--text-main);
            min-height: 100vh;
            line-height: 1.6;
            overflow-x: hidden;
        }

        /* =========================================
           3. Glassmorphism Utilities
        ========================================= */
        .glass {
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--glass-border);
            box-shadow: var(--glass-shadow);
            border-radius: 16px;
            color: var(--glass-text);
        }

        /* =========================================
           4. Layout & Navigation
        ========================================= */
        header {
            position: sticky;
            top: 0;
            z-index: 100;
            padding: 1rem 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo h1 {
            font-size: 1.5rem;
            color: var(--glass-text);
            text-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .logo span { font-size: 0.9rem; font-weight: normal; }

        nav ul {
            display: flex;
            gap: 1.5rem;
            list-style: none;
        }

        nav a {
            color: var(--glass-text);
            text-decoration: none;
            font-weight: bold;
            cursor: pointer;
            padding: 0.5rem 1rem;
            border-radius: 8px;
            transition: 0.3s;
        }

        nav a:hover { background: var(--glass-border); }

        .btn-toggle {
            background: transparent;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--glass-text);
        }

        /* =========================================
           5. Main Content & Sections (SPA)
        ========================================= */
        main {
            padding: 2rem 5%;
            max-width: 1200px;
            margin: auto;
        }

        section {
            display: none;
            animation: fadeIn 0.6s ease forwards;
        }

        section.active {
            display: block;
        }

        /* Hero Section */
        #home { text-align: center; padding: 4rem 2rem; }
        #home h2 { font-size: 3rem; margin-bottom: 1rem; color: var(--glass-text); }
        #home p { font-size: 1.2rem; margin-bottom: 2rem; }

        .btn {
            padding: 10px 24px;
            border: none;
            border-radius: 30px;
            font-size: 1.1rem;
            cursor: pointer;
            font-weight: bold;
            background: var(--primary-color);
            color: #fff;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .btn:hover { transform: translateY(-3px); box-shadow: 0 6px 20px rgba(0,0,0,0.3); }
        .btn-outline { background: transparent; border: 2px solid var(--primary-color); color: var(--glass-text); }

        /* Cards Layout (Grid) */
        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .card {
            padding: 1.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .card:hover { transform: translateY(-10px); background: var(--card-hover-bg); }
        .card h3 { color: var(--primary-color); border-bottom: 2px solid var(--accent-color); padding-bottom: 0.5rem; margin-bottom: 1rem;}
        
        [data-theme="dark"] .card h3 { color: var(--accent-color); }

        /* Quiz Form */
        .quiz-item { margin-bottom: 1.5rem; padding: 1rem; }
        .quiz-item h4 { margin-bottom: 0.5rem; }
        .options { display: flex; gap: 1rem; flex-wrap: wrap; }
        .options label {
            display: flex; align-items: center; gap: 0.5rem; cursor: pointer;
            padding: 0.5rem 1rem; background: var(--glass-border); border-radius: 20px;
        }

        /* Result Dashboard */
        .result-card { margin-bottom: 1.5rem; padding: 2rem; border-left: 5px solid var(--accent-color); }
        .progress-bar { width: 100%; background: #ddd; border-radius: 10px; overflow: hidden; margin-top: 10px; height: 20px;}
        .progress-fill { height: 100%; background: var(--secondary-color); width: 0%; transition: width 1.5s ease-out; }
        .tag { display: inline-block; padding: 4px 12px; background: var(--glass-border); border-radius: 15px; font-size: 0.9rem; margin: 4px;}

        /* Modal */
        .modal {
            display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%;
            background-color: rgba(0,0,0,0.5); backdrop-filter: blur(5px);
            align-items: center; justify-content: center;
        }
        .modal-content {
            width: 90%; max-width: 600px; padding: 2rem; position: relative;
            animation: slideUp 0.4s ease;
        }
        .close-btn { position: absolute; top: 15px; right: 20px; font-size: 1.5rem; cursor: pointer; color: var(--glass-text); }

        /* Compare Table */
        table { width: 100%; border-collapse: collapse; margin-top: 1rem; color: var(--glass-text); }
        th, td { padding: 12px; border: 1px solid var(--glass-border); text-align: left; }
        th { background: var(--glass-border); }

        /* =========================================
           6. Animations
        ========================================= */
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes slideUp { from { opacity: 0; transform: translateY(50px); } to { opacity: 1; transform: translateY(0); } }

        /* Responsive */
        @media (max-width: 768px) {
            nav ul { display: none; }
            header { flex-direction: column; gap: 1rem; text-align: center; }
            .logo h1 { font-size: 1.2rem; }
        }
    </style>
</head>
<body>

    <header class="glass">
        <div class="logo">
            <h1 aria-label="โรงเรียนศรียาภัย">🏫 ศรียาภัย <span>ระบบแนะนำห้องเรียน</span></h1>
        </div>
        <nav aria-label="Main navigation">
            <ul id="nav-links">
                <li><a onclick="navigateTo('home')">หน้าแรก</a></li>
                <li><a onclick="navigateTo('programs')">ข้อมูลห้องเรียน</a></li>
                <li><a onclick="navigateTo('compare')">เปรียบเทียบ</a></li>
                <li><a onclick="navigateTo('admin')">Admin</a></li>
            </ul>
        </nav>
        <button class="btn-toggle" id="themeToggle" aria-label="สลับโหมดหน้าจอ">🌙</button>
    </header>

    <main>
        
        <section id="home" class="active glass">
            <h2>ค้นหาเส้นทางอนาคตที่ใช่ สำหรับคุณ!</h2>
            <p>ระบบ AI จะช่วยวิเคราะห์ความถนัด ความสนใจ และเป้าหมายอาชีพ เพื่อแนะนำห้องเรียนในโรงเรียนศรียาภัยที่เหมาะสมกับคุณที่สุด</p>
            <div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
                <button class="btn" onclick="navigateTo('quiz')">🚀 เริ่มทำแบบทดสอบ (AI)</button>
                <button class="btn btn-outline" onclick="navigateTo('programs')">📖 ดูข้อมูลห้องเรียนทั้งหมด</button>
            </div>
        </section>

        <section id="programs">
            <h2 style="text-align: center; margin-bottom: 2rem;">ข้อมูลห้องเรียน (ม.ต้น / ม.ปลาย)</h2>
            <div class="grid-container" id="program-list">
                </div>
        </section>

        <section id="quiz">
            <div class="glass" style="padding: 2rem;">
                <h2 style="margin-bottom: 1rem; border-bottom: 2px solid var(--accent-color); padding-bottom: 0.5rem;">แบบทดสอบค้นหาตัวเอง (15 ข้อ)</h2>
                <p style="margin-bottom: 2rem; color: var(--text-muted);">เลือกระดับความตรงกับตัวคุณที่สุด (1 = ไม่ตรงเลย, 5 = ตรงมากที่สุด)</p>
                
                <form id="quizForm">
                    <div id="quiz-container">
                        </div>
                    <div style="text-align: center; margin-top: 2rem;">
                        <button type="button" class="btn" onclick="processQuiz()">📊 วิเคราะห์ผลลัพธ์ด้วย AI</button>
                    </div>
                </form>
            </div>
        </section>

        <section id="result">
            <h2 style="text-align: center; margin-bottom: 2rem;">🏆 ผลการวิเคราะห์จาก AI ของคุณ</h2>
            <div id="result-container">
                </div>
            <div style="text-align: center; margin-top: 2rem;">
                <button class="btn btn-outline" onclick="navigateTo('quiz')">🔄 ทำแบบทดสอบอีกครั้ง</button>
                <button class="btn" onclick="navigateTo('compare')">⚖️ นำผลลัพธ์ไปเปรียบเทียบ</button>
            </div>
        </section>

        <section id="compare">
            <div class="glass" style="padding: 2rem;">
                <h2 style="margin-bottom: 1.5rem;">⚖️ เปรียบเทียบห้องเรียน</h2>
                <div style="display: flex; gap: 1rem; margin-bottom: 2rem; flex-wrap: wrap;">
                    <select id="comp1" class="glass" style="padding: 10px; width: 100%; max-width: 300px; border-radius: 8px;"></select>
                    <select id="comp2" class="glass" style="padding: 10px; width: 100%; max-width: 300px; border-radius: 8px;"></select>
                    <button class="btn" onclick="comparePrograms()">เปรียบเทียบ</button>
                </div>
                <div id="compare-result" style="overflow-x: auto;">
                    </div>
            </div>
        </section>

        <section id="admin">
            <div class="glass" style="padding: 2rem;">
                <h2>⚙️ Admin Dashboard (Mockup)</h2>
                <p>จำลองระบบจัดการข้อมูลห้องเรียน (เพิ่มข้อมูลลง Array ชั่วคราว)</p>
                <div style="margin-top: 1rem; display: flex; flex-direction: column; gap: 10px; max-width: 400px;">
                    <input type="text" id="adminName" placeholder="ชื่อห้องเรียน (เช่น Arts-Eng)" class="glass" style="padding:10px;">
                    <input type="text" id="adminFocus" placeholder="จุดเน้น (เช่น ภาษา, ศิลปะ)" class="glass" style="padding:10px;">
                    <button class="btn" onclick="addProgramMock()">➕ เพิ่มห้องเรียน</button>
                </div>
                <p id="adminStatus" style="color: green; margin-top: 10px;"></p>
            </div>
        </section>

    </main>

    <div id="programModal" class="modal" aria-hidden="true" role="dialog">
        <div class="glass modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <h2 id="modalTitle" style="color: var(--primary-color);"></h2>
            <p id="modalLevel" style="font-weight: bold; margin-bottom: 1rem;"></p>
            <hr style="border: 0; border-top: 1px solid var(--glass-border); margin-bottom: 1rem;">
            <p><strong>🎯 จุดเน้น:</strong> <span id="modalFocus"></span></p>
            <p style="margin-top: 10px;"><strong>📚 วิชาหลัก:</strong></p>
            <div id="modalSubjects" style="margin-bottom: 10px;"></div>
            <p><strong>💼 อาชีพในอนาคต:</strong></p>
            <div id="modalCareers"></div>
        </div>
    </div>

    <script>
        // --- ข้อมูลฐานของระบบ (Database Mockup) ---
        const programsData = [
            { id: 'smte', name: 'SMTE (Science Math and Tech Education)', level: 'ม.ต้น / ม.ปลาย', focus: 'วิทย์, คณิต, เทคโนโลยี, วิจัย/นวัตกรรม', subjects: ['ฟิสิกส์', 'เคมี', 'ชีววิทยา', 'คอมพิวเตอร์'], careers: ['แพทย์', 'วิศวกร', 'นักวิจัย', 'นักวิเคราะห์ข้อมูล'], desc: 'เหมาะสำหรับผู้ที่ชอบคิดค้นนวัตกรรมและการทดลอง' },
            { id: 'smt', name: 'SMT (Science Math and Tech)', level: 'ม.ต้น / ม.ปลาย', focus: 'พื้นฐานวิทย์-คณิต พัฒนาการคิดวิเคราะห์', subjects: ['คณิตศาสตร์', 'วิทยาศาสตร์', 'เทคโนโลยี'], careers: ['วิศวกร', 'ครู', 'นักวิทยาศาสตร์', 'โปรแกรมเมอร์'], desc: 'เน้นความรู้พื้นฐานแน่น เพื่อประยุกต์ใช้ในวิชาชีพสายวิทย์' },
            { id: 'iep', name: 'IEP (Intensive English Program)', level: 'ม.ต้น / ม.ปลาย', focus: 'เรียนวิชาหลักเป็นภาษาอังกฤษ สื่อสารระดับสากล', subjects: ['ภาษาอังกฤษ', 'คณิตศาสตร์', 'วิทย์เพื่อการสื่อสาร'], careers: ['ล่าม', 'นักการทูต', 'พนักงานสายการบิน', 'พนักงานบริษัทต่างประเทศ'], desc: 'เน้นความชำนาญทางภาษาเพื่อการทำงานระดับนานาชาติ' },
            { id: 'eis', name: 'EIS (English for Integrated Studies)', level: 'ม.ต้น', focus: 'ใช้ภาษาอังกฤษเป็นสื่อการสอนควบคู่กับวิชาหลัก', subjects: ['ภาษาอังกฤษ', 'วิทยาศาสตร์', 'คณิตศาสตร์', 'สังคมศึกษา'], careers: ['ครู', 'นักแปล', 'นักธุรกิจ', 'นักการตลาดข้ามชาติ'], desc: 'สร้างความคุ้นเคยกับภาษาอังกฤษผ่านวิชาการต่างๆ' },
            { id: 'smep', name: 'SMEP (Science Math & English Program)', level: 'ม.ปลาย', focus: 'ผสมผสาน STEM ควบคู่ภาษาอังกฤษระดับสูง', subjects: ['วิทยาศาสตร์', 'คณิตศาสตร์', 'ภาษาอังกฤษ', 'เทคโนโลยี'], careers: ['แพทย์', 'วิศวกร', 'นักวิจัย', 'โปรแกรมเมอร์', 'นักธุรกิจระหว่างประเทศ'], desc: 'โปรแกรมสุดยอดที่รวมสายวิทย์และภาษาเข้าด้วยกัน' }
        ];

        // --- ชุดคำถาม 15 ข้อ และค่าน้ำหนัก (Weights) ---
        const quizQuestions = [
            { q: "1. คุณชอบแก้โจทย์ปัญหาหรือสมการคณิตศาสตร์ที่ซับซ้อน", w: { smte: 3, smt: 3, smep: 2, iep: 0, eis: 1 } },
            { q: "2. คุณสนุกกับการทำแล็บ หรือทดลองทางวิทยาศาสตร์", w: { smte: 3, smt: 2, smep: 2, iep: 0, eis: 0 } },
            { q: "3. คุณชอบเขียนโปรแกรม หรือสนใจการทำงานของคอมพิวเตอร์", w: { smte: 3, smt: 2, smep: 2, iep: 0, eis: 0 } },
            { q: "4. คุณมีความสุขเมื่อได้สื่อสารด้วยภาษาอังกฤษ", w: { smte: 0, smt: 0, smep: 2, iep: 3, eis: 3 } },
            { q: "5. คุณมีความฝันอยากทำงานในองค์กรระดับนานาชาติ หรือต่างประเทศ", w: { smte: 1, smt: 0, smep: 2, iep: 3, eis: 2 } },
            { q: "6. คุณชอบประดิษฐ์หุ่นยนต์ หรือสร้างนวัตกรรมใหม่ๆ", w: { smte: 3, smt: 2, smep: 2, iep: 0, eis: 0 } },
            { q: "7. คุณสนใจอ่านบทความวิจัย หรือข่าวสารทางวิทยาศาสตร์", w: { smte: 3, smt: 2, smep: 2, iep: 0, eis: 0 } },
            { q: "8. คุณอยากเรียนรู้แบบเจาะลึกในวิชา ฟิสิกส์ เคมี ชีววิทยา", w: { smte: 3, smt: 3, smep: 3, iep: 0, eis: 0 } },
            { q: "9. คุณมีเป้าหมายอยากเป็น แพทย์ วิศวกร หรือนักวิทยาศาสตร์", w: { smte: 3, smt: 3, smep: 3, iep: 0, eis: 0 } },
            { q: "10. คุณอยากเป็น ล่าม นักการทูต หรือนักธุรกิจข้ามชาติ", w: { smte: 0, smt: 0, smep: 1, iep: 3, eis: 2 } },
            { q: "11. คุณยินดีที่จะเรียนวิชาคณิตฯ และวิทย์ฯ โดยใช้ตำราภาษาอังกฤษ", w: { smte: 1, smt: 0, smep: 3, iep: 3, eis: 3 } },
            { q: "12. คุณมักจะเป็นผู้นำในการนำเสนอผลงาน (Presentation)", w: { smte: 1, smt: 1, smep: 2, iep: 2, eis: 2 } },
            { q: "13. คุณชอบรวบรวมข้อมูล สถิติ และนำมาวิเคราะห์หาข้อสรุป", w: { smte: 3, smt: 2, smep: 1, iep: 0, eis: 1 } },
            { q: "14. คุณชอบเรียนรู้โครงสร้างไวยากรณ์ และการแปลภาษา", w: { smte: 0, smt: 0, smep: 1, iep: 3, eis: 2 } },
            { q: "15. คุณต้องการเรียนหนักเพื่อเตรียมตัวสอบเข้ามหาวิทยาลัยชั้นนำ", w: { smte: 3, smt: 2, smep: 3, iep: 2, eis: 1 } }
        ];

        // --- ระบบ SPA Navigation ---
        function navigateTo(sectionId) {
            document.querySelectorAll('main section').forEach(sec => sec.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            window.scrollTo(0, 0);
        }

        // --- ระบบ Dark Mode ---
        const themeToggle = document.getElementById('themeToggle');
        themeToggle.addEventListener('click', () => {
            const currentTheme = document.documentElement.getAttribute('data-theme');
            if(currentTheme === 'light') {
                document.documentElement.setAttribute('data-theme', 'dark');
                themeToggle.textContent = '☀️';
            } else {
                document.documentElement.setAttribute('data-theme', 'light');
                themeToggle.textContent = '🌙';
            }
        });

        // --- Render UI: แสดงการ์ดข้อมูลห้องเรียน ---
        function renderPrograms() {
            const list = document.getElementById('program-list');
            list.innerHTML = '';
            programsData.forEach(p => {
                const card = document.createElement('div');
                card.className = 'glass card';
                card.innerHTML = `
                    <h3>${p.name}</h3>
                    <p style="font-size: 0.9rem; color: var(--text-muted); margin-bottom:10px;">${p.level}</p>
                    <p>${p.focus}</p>
                `;
                card.onclick = () => openModal(p);
                list.appendChild(card);
            });
            updateCompareSelects();
        }

        // --- Modal Logic ---
        const modal = document.getElementById('programModal');
        function openModal(data) {
            document.getElementById('modalTitle').textContent = data.name;
            document.getElementById('modalLevel').textContent = "ระดับชั้น: " + data.level;
            document.getElementById('modalFocus').textContent = data.focus;
            
            document.getElementById('modalSubjects').innerHTML = data.subjects.map(s => `<span class="tag">${s}</span>`).join('');
            document.getElementById('modalCareers').innerHTML = data.careers.map(c => `<span class="tag">${c}</span>`).join('');
            
            modal.style.display = 'flex';
        }
        function closeModal() { modal.style.display = 'none'; }
        window.onclick = function(event) { if (event.target == modal) closeModal(); }

        // --- Render UI: สร้างแบบทดสอบ ---
        function renderQuiz() {
            const container = document.getElementById('quiz-container');
            container.innerHTML = '';
            quizQuestions.forEach((qObj, index) => {
                const item = document.createElement('div');
                item.className = 'glass quiz-item';
                let optionsHtml = '';
                for(let i=1; i<=5; i++) {
                    optionsHtml += `
                        <label>
                            <input type="radio" name="q${index}" value="${i}" required> 
                            ${i}
                        </label>
                    `;
                }
                item.innerHTML = `
                    <h4>${qObj.q}</h4>
                    <div class="options">${optionsHtml}</div>
                `;
                container.appendChild(item);
            });
        }

        // --- 🧠 AI Analysis Engine ---
        function processQuiz() {
            const form = document.getElementById('quizForm');
            if(!form.checkValidity()) {
                alert("กรุณาตอบคำถามให้ครบทุกข้อครับ!");
                form.reportValidity();
                return;
            }

            let scores = { smte: 0, smt: 0, iep: 0, eis: 0, smep: 0 };
            let maxPossible = { smte: 0, smt: 0, iep: 0, eis: 0, smep: 0 };

            quizQuestions.forEach((qObj, index) => {
                const userAnswer = parseInt(document.querySelector(`input[name="q${index}"]:checked`).value);
                for(let program in qObj.w) {
                    const weight = qObj.w[program];
                    scores[program] += userAnswer * weight;
                    maxPossible[program] += 5 * weight;
                }
            });

            let resultsArray = [];
            for(let program in scores) {
                const percent = maxPossible[program] > 0 ? ((scores[program] / maxPossible[program]) * 100).toFixed(1) : 0;
                const progData = programsData.find(p => p.id === program);
                resultsArray.push({
                    id: program,
                    name: progData.name,
                    percent: parseFloat(percent),
                    desc: progData.desc,
                    careers: progData.careers
                });
            }

            resultsArray.sort((a, b) => b.percent - a.percent);
            renderResultDashboard(resultsArray.slice(0, 3));
            navigateTo('result');
        }

        // --- Render UI: Dashboard ผลลัพธ์ AI ---
        function renderResultDashboard(top3) {
            const container = document.getElementById('result-container');
            container.innerHTML = '';

            top3.forEach((res, index) => {
                const rankHtml = index === 0 ? '👑 แนะนำอันดับ 1 (ดีที่สุด)' : `แนะนำอันดับ ${index + 1}`;
                const card = document.createElement('div');
                card.className = 'glass result-card';
                card.innerHTML = `
                    <h3 style="color: var(--primary-color);">${rankHtml}</h3>
                    <h2 style="margin: 10px 0;">${res.name}</h2>
                    <p style="margin-bottom: 10px;"><strong>เหตุผลที่แนะนำ:</strong> ${res.desc}</p>
                    
                    <div style="margin: 15px 0;">
                        <span style="font-weight:bold;">ความเหมาะสม: ${res.percent}%</span>
                        <div class="progress-bar">
                            <div class="progress-fill" style="width: 0%;" data-width="${res.percent}%"></div>
                        </div>
                    </div>

                    <p><strong>💡 อาชีพที่สอดคล้อง:</strong></p>
                    <div>${res.careers.map(c => `<span class="tag">${c}</span>`).join('')}</div>
                `;
                container.appendChild(card);
            });

            setTimeout(() => {
                document.querySelectorAll('.progress-fill').forEach(bar => {
                    bar.style.width = bar.getAttribute('data-width');
                });
            }, 100);
        }

        // --- Feature: เปรียบเทียบห้องเรียน ---
        function updateCompareSelects() {
            const sel1 = document.getElementById('comp1');
            const sel2 = document.getElementById('comp2');
            sel1.innerHTML = ''; sel2.innerHTML = '';
            programsData.forEach(p => {
                const opt = `<option value="${p.id}">${p.name}</option>`;
                sel1.innerHTML += opt;
                sel2.innerHTML += opt;
            });
            if(programsData.length > 1) sel2.selectedIndex = 1; 
        }

        function comparePrograms() {
            const id1 = document.getElementById('comp1').value;
            const id2 = document.getElementById('comp2').value;
            const p1 = programsData.find(p => p.id === id1);
            const p2 = programsData.find(p => p.id === id2);

            const resDiv = document.getElementById('compare-result');
            resDiv.innerHTML = `
                <table class="glass">
                    <thead>
                        <tr>
                            <th>หัวข้อ</th>
                            <th>${p1.name}</th>
                            <th>${p2.name}</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><strong>ระดับชั้น</strong></td>
                            <td>${p1.level}</td>
                            <td>${p2.level}</td>
                        </tr>
                        <tr>
                            <td><strong>จุดเน้นหลัก</strong></td>
                            <td>${p1.focus}</td>
                            <td>${p2.focus}</td>
                        </tr>
                        <tr>
                            <td><strong>วิชาหลัก</strong></td>
                            <td>${p1.subjects.join(', ')}</td>
                            <td>${p2.subjects.join(', ')}</td>
                        </tr>
                        <tr>
                            <td><strong>อาชีพในอนาคต</strong></td>
                            <td>${p1.careers.join('<br>')}</td>
                            <td>${p2.careers.join('<br>')}</td>
                        </tr>
                    </tbody>
                </table>
            `;
        }

        // --- Feature: Admin Mockup ---
        function addProgramMock() {
            const name = document.getElementById('adminName').value;
            const focus = document.getElementById('adminFocus').value;
            if(!name || !focus) {
                alert("กรุณากรอกข้อมูลให้ครบ");
                return;
            }
            
            programsData.push({
                id: 'new_' + Date.now(),
                name: name,
                level: 'ทั่วไป',
                focus: focus,
                subjects: ['วิชาทั่วไป'],
                careers: ['อาชีพอิสระ'],
                desc: 'แผนการเรียนใหม่ (จำลอง)'
            });

            document.getElementById('adminStatus').innerText = "✅ เพิ่มข้อมูลสำเร็จ! ลองกลับไปดูหน้า 'ข้อมูลห้องเรียน' หรือ 'เปรียบเทียบ'";
            document.getElementById('adminName').value = '';
            document.getElementById('adminFocus').value = '';
            
            renderPrograms();
        }

        // --- Initialization ---
        window.onload = () => {
            renderPrograms();
            renderQuiz();
        };
    </script>
</body>
</html>
