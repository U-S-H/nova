<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Official Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; --accent: #00aaff; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; cursor: pointer; border: none; }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(10px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        .status-dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; background-color: #00ff88; box-shadow: 0 0 8px #00ff88; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <div class="space-y-4 max-w-xs mx-auto w-full mt-10">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl outline-none bg-slate-50 border border-slate-100">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl outline-none bg-slate-50 border border-slate-100">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs">Login / Join</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-4">Create New Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase">@USER</p>
                <p class="text-[8px] font-bold text-green-500 uppercase">System Active</p>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-xl">
                <p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest mb-1">Available Liquidity</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Daily Yield</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-circle-plus text-[#D4AF37]"></i><span class="text-[10px] font-black">DEPOSIT</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-slate-400"></i><span class="text-[10px] font-black">WITHDRAW</span></button>
            </div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6 pb-24">
            <div class="premium-card flex flex-col h-[70vh]">
                <div class="p-4 border-b flex justify-between items-center bg-slate-50 rounded-t-[24px]">
                    <div class="flex items-center gap-3"><div class="status-dot"></div><span id="agent-display" class="text-[10px] font-black uppercase">Support Agent</span></div>
                </div>
                <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-4 text-[12px]"></div>
                <div class="p-4 border-t bg-slate-50 rounded-b-[24px]">
                    <div class="flex gap-2">
                        <label class="cursor-pointer bg-white border p-2 rounded-xl"><input type="file" id="chat-img" class="hidden" accept="image/*" onchange="handleImage(this)"><i class="fa-solid fa-camera text-slate-400"></i></label>
                        <input type="text" id="chat-input" placeholder="Type message..." class="flex-1 p-3 rounded-xl outline-none bg-white border">
                        <button onclick="sendMsg()" class="btn-gold px-4 py-3"><i class="fa-solid fa-paper-plane"></i></button>
                    </div>
                </div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-6 text-red-600 uppercase">Command Center</h2>
            <div class="premium-card p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4">Live Support Monitor</h3>
                <div id="adm-chat-monitor" class="space-y-4 max-h-80 overflow-y-auto border-t pt-4"></div>
            </div>
            <div class="premium-card p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4">User List</h3>
                <div id="adm-user-list" class="space-y-2 max-h-60 overflow-y-auto"></div>
            </div>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-comment-dots"></i><span>SUPPORT</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600" style="display: none;"><i class="fa-solid fa-lock"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400"></i><span>EXIT</span></div>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;

        // Admin Secret Toggle
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Admin Pin:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    loadMasterUsers(); loadAllChats();
                    alert("Admin Access Active");
                }
                admClicks = 0;
            }
        };

        // Auth
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Required fields");
            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Invalid credentials");
            } else {
                if(snap.exists()) return alert("Username taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // REAL-TIME CHAT LOGIC
        window.sendMsg = async () => {
            const input = document.getElementById('chat-input');
            const msg = input.value.trim();
            if(!msg) return;
            await addDoc(collection(db, "chats"), { sender: user, text: msg, time: Date.now(), type: 'text' });
            input.value = "";
        };

        window.handleImage = (input) => {
            if(input.files && input.files[0]) {
                const reader = new FileReader();
                reader.onload = async (e) => {
                    await addDoc(collection(db, "chats"), { sender: user, text: e.target.result, time: Date.now(), type: 'image' });
                };
                reader.readAsDataURL(input.files[0]);
            }
        };

        function listenChat(uid) {
            const q = query(collection(db, "chats"), orderBy("time", "asc"));
            onSnapshot(q, (snapshot) => {
                const box = document.getElementById('chat-box'); box.innerHTML = "";
                snapshot.forEach(doc => {
                    const data = doc.data();
                    if(data.sender === uid || data.receiver === uid) {
                        const div = document.createElement('div');
                        div.className = data.sender === uid ? 'bg-[#D4AF37] text-white p-3 rounded-2xl ml-auto max-w-[80%]' : 'bg-slate-100 p-3 rounded-2xl mr-auto max-w-[80%]';
                        if(data.type === 'image') div.innerHTML = `<img src="${data.text}" class="rounded-lg w-full">`;
                        else div.innerText = data.text;
                        box.appendChild(div);
                    }
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // ADMIN CHAT MONITOR
        function loadAllChats() {
            const q = query(collection(db, "chats"), orderBy("time", "desc"));
            onSnapshot(q, (snapshot) => {
                const monitor = document.getElementById('adm-chat-monitor'); monitor.innerHTML = "";
                snapshot.forEach(doc => {
                    const d = doc.data();
                    monitor.innerHTML += `<div class="p-3 border-b text-[10px]">
                        <b class="text-blue-500">@${d.sender}:</b> ${d.type === 'image' ? '[Image Sent]' : d.text}
                    </div>`;
                });
            });
        }

        async function loadMasterUsers() {
            const q = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list'); box.innerHTML = "";
            q.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-3 border rounded-xl flex justify-between bg-slate-50 text-[10px] mb-2">
                    <div><b>@${uDoc.id}</b><br>PW: ${u.password}</div>
                    <div class="text-right">Bal: $${(u.balance || 0).toFixed(2)}<br><span class="text-red-600 font-bold cursor-pointer" onclick="delUser('${uDoc.id}')">Delete</span></div>
                </div>`;
            });
        }

        window.delUser = async (id) => { if(confirm("Delete user?")) { await deleteDoc(doc(db, "users", id)); loadMasterUsers(); } };

        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
            });
            listenChat(id);
        }

        window.nav = (id, el) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.getElementById(id).classList.add('active-screen');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            el.classList.add('active');
        };

        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => document.getElementById(id).classList.add('hidden');
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Account" : "Login Instead"; };

        if(user) initApp(user);
    </script>
</body>
</html>
