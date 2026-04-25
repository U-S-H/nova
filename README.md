<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Official Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; overflow-x: hidden; }
        
        /* IMPORTANT: Hidden screens must stay hidden */
        .hidden { display: none !important; }
        
        .screen { display: none; padding-bottom: 100px; }
        .active-screen { display: block !important; animation: fadeIn 0.4s ease; }
        
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; cursor: pointer; }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 10px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="flex flex-col justify-center items-center min-h-screen p-8 text-center bg-white">
        <h1 class="text-5xl font-black tracking-tighter mb-2">NO<span class="text-[#D4AF37]">VA</span></h1>
        <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest mb-10">Global Asset Management</p>
        <div class="space-y-4 w-full max-w-xs">
            <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl border bg-slate-50 outline-none">
            <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl border bg-slate-50 outline-none">
            <div id="ref-field" class="hidden">
                <input type="text" id="u-ref" placeholder="Referral Code" class="w-full p-4 rounded-2xl border bg-slate-100">
            </div>
            <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs">Authorize Access</button>
            <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-4">Create Corporate Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
            <p id="user-display" class="text-[10px] font-black uppercase text-slate-600">@USER</p>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-xl">
                <p class="text-[8px] text-slate-500 font-bold uppercase mb-1">Available Assets</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Daily Profit</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            
            <div class="bg-white border rounded-3xl p-5">
                <h3 class="text-[10px] font-black uppercase mb-3 text-slate-400">Referral Link</h3>
                <input type="text" id="ref-link-display" readonly class="w-full bg-slate-50 p-3 text-[10px] rounded-xl border mb-3">
                <button onclick="copyRef()" class="w-full btn-gold py-3 text-[10px]">COPY LINK</button>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6">
            <h2 class="text-xl font-black mb-6 text-red-600 uppercase">Command Center</h2>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-comment-dots"></i><span>SUPPORT</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-clock-rotate-left"></i><span>RECORDS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600 hidden"><i class="fa-solid fa-lock"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400"></i><span>EXIT</span></div>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let admClicks = 0;

        // Check if user is logged in
        function checkSession() {
            if (user) {
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('app').classList.remove('hidden');
                initDashboard(user);
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('app').classList.add('hidden');
            }
        }

        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("All fields required");

            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);

            if(snap.exists() && snap.data().password === pw) {
                localStorage.setItem('nova_session', id);
                location.reload(); 
            } else {
                alert("Access Denied");
            }
        };

        function initDashboard(id) {
            document.getElementById('user-display').innerText = `@${id}`;
            document.getElementById('ref-link-display').value = `${window.location.origin}${window.location.pathname}?ref=${id}`;
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data();
                if(u) {
                    document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                    document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                    document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                }
            });
        }

        // Navigation fix
        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        // Admin Secret Mode
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Admin Pin:") === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    alert("Admin Active");
                }
                admClicks = 0;
            }
        };

        window.logout = () => { localStorage.clear(); location.reload(); };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link-display').value); alert("Link Copied!"); };

        checkSession();
    </script>
</body>
</html>
