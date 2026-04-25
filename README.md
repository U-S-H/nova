<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Global Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; --success: #10B981; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.02); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 14px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .btn-gold:active { transform: scale(0.95); }
        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: rgba(255,255,255,0.9); backdrop-filter: blur(10px); border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 15px 5px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 8px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; cursor: pointer; }
        .nav-item.active { color: var(--gold); }
        .status-dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; background-color: #00ff88; box-shadow: 0 0 8px #00ff88; }
        input { border: 1px solid #e2e8f0 !important; font-size: 14px !important; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-5xl font-black text-slate-900 tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest mt-2">Trusted Asset Management</p>
            <div class="space-y-4 max-w-xs mx-auto w-full mt-10">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-2xl outline-none bg-slate-50 border">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-2xl outline-none bg-slate-50 border">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 uppercase text-xs">Secure Login / Join</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline mt-4">Create Verified Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer select-none">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase">@USER</p>
                <div class="flex items-center gap-1 justify-end"><div class="status-dot"></div><p class="text-[8px] font-bold text-green-500 uppercase">Live</p></div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[32px] p-8 mb-6 text-white shadow-xl relative overflow-hidden">
                <div class="absolute top-0 right-0 p-4 opacity-10"><i class="fa-solid fa-shield-halved text-6xl"></i></div>
                <p class="text-[8px] text-slate-500 font-bold uppercase tracking-widest mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-5xl font-black tracking-tighter mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/10 pt-6">
                    <div><p class="text-[7px] text-slate-500 font-bold uppercase">Daily Profit</p><p id="b-daily" class="text-lg font-black text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 font-bold uppercase">Total Profit</p><p id="b-total" class="text-lg font-black text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-circle-plus text-[#D4AF37]"></i><span class="text-[10px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-slate-400"></i><span class="text-[10px] font-black uppercase">Withdraw</span></button>
            </div>
            <div class="premium-card p-5">
                <h3 class="text-[10px] font-black uppercase mb-4 text-slate-400">System Information</h3>
                <div class="space-y-3 text-[11px] font-semibold">
                    <div class="flex justify-between"><span>Status:</span><span class="text-green-500">Verified</span></div>
                    <div class="flex justify-between"><span>Network:</span><span>Nova-Secure v4.0</span></div>
                </div>
            </div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6 pb-24">
            <div class="premium-card flex flex-col h-[70vh]">
                <div class="p-4 border-b flex justify-between items-center bg-slate-50 rounded-t-[24px]">
                    <div class="flex items-center gap-3"><div class="status-dot"></div><span id="agent-display" class="text-[10px] font-black uppercase">Agent 786 (Active)</span></div>
                </div>
                <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-4 text-[12px] scroll-smooth"></div>
                <div class="p-4 border-t bg-slate-50 rounded-b-[24px]">
                    <div class="flex gap-2">
                        <label class="cursor-pointer bg-white border p-2 rounded-xl"><input type="file" id="chat-img" class="hidden" accept="image/*" onchange="handleImage(this)"><i class="fa-solid fa-camera text-slate-400"></i></label>
                        <input type="text" id="chat-input" placeholder="Enter message..." class="flex-1 p-3 rounded-xl outline-none bg-white border">
                        <button onclick="sendMsg()" class="btn-gold px-4 py-3"><i class="fa-solid fa-paper-plane"></i></button>
                    </div>
                </div>
            </div>
        </div>

        <div id="page-logs" class="screen px-4 pt-6 pb-24"><div id="logs-list" class="space-y-3"></div></div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-6 text-red-600 uppercase">Master Control</h2>
            <div class="premium-card p-5 mb-6 bg-slate-900 text-white">
                <h3 class="text-[10px] font-bold text-slate-400 uppercase mb-4">Live Support (All Users)</h3>
                <div id="adm-chat-monitor" class="space-y-2 max-h-60 overflow-y-auto border-t border-white/10 pt-3"></div>
            </div>
            <div class="premium-card p-5 mb-6">
                <h3 class="text-[10px] font-black uppercase mb-4 text-slate-400">Verified User List</h3>
                <div id="adm-user-list" class="space-y-2"></div>
            </div>
            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-comment-dots"></i><span>SUPPORT</span></div>
            <div onclick="nav('page-logs', this)" class="nav-item"><i class="fa-solid fa-receipt"></i><span>RECORDS</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600" style="display: none;"><i class="fa-solid fa-user-shield"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl">DEPOSIT FUNDS</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark cursor-pointer"></i></div>
        <div class="grid grid-cols-2 gap-2 mb-4">
            <button onclick="alert('Admin Wallets: EP 03XX, JC 03XX')" class="border p-4 rounded-xl font-bold text-[10px]">LOCAL TRANSFER</button>
            <button onclick="alert('Admin USDT: 0xXXXX')" class="border p-4 rounded-xl font-bold text-[10px]">USDT (TRC20)</button>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-2xl mb-4">
        <input type="text" id="dep-txid" placeholder="Transaction Hash/ID" class="w-full p-4 border rounded-2xl mb-6">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-4">SUBMIT FOR VERIFICATION</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl">WITHDRAW ASSETS</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark cursor-pointer"></i></div>
        <input type="number" id="wd-amt" placeholder="Payout Amount ($)" class="w-full p-4 border rounded-2xl mb-4">
        <input type="text" id="wd-addr" placeholder="Account Details" class="w-full p-4 border rounded-2xl mb-6">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-4">REQUEST WITHDRAWAL</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // CONFIG (Replace with your actual Firebase config)
        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;

        // Admin Pin Setup
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Master Pin:") === "nov786") {
                    document.getElementById('nav-admin-btn').style.display = "flex";
                    loadMasterUsers(); loadAdminChat(); loadAdminLogs();
                    alert("Admin Console Unlocked");
                }
                admClicks = 0;
            }
        };

        // AUTH SYSTEM
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("All fields required");
            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Access Denied: Invalid Credentials");
            } else {
                if(snap.exists()) return alert("Username already registered");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, status: 'Active', joined: Date.now() });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // REAL-TIME CHAT ENGINE
        window.sendMsg = async () => {
            const input = document.getElementById('chat-input');
            if(!input.value.trim()) return;
            await addDoc(collection(db, "chats"), { sender: user, text: input.value, time: Date.now(), type: 'text' });
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
                        div.className = data.sender === uid ? 'bg-[#D4AF37] text-white p-3 rounded-2xl ml-auto max-w-[85%] shadow-sm' : 'bg-slate-100 p-3 rounded-2xl mr-auto max-w-[85%]';
                        div.innerHTML = data.type === 'image' ? `<img src="${data.text}" class="rounded-xl w-full">` : data.text;
                        box.appendChild(div);
                    }
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        // FINANCE HANDLER
        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Please fill all details");
            await addDoc(collection(db, "logs"), { user: user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request submitted successfully!"); closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        // ADMIN FUNCTIONS
        function loadAdminChat() {
            onSnapshot(query(collection(db, "chats"), orderBy("time", "desc")), s => {
                const box = document.getElementById('adm-chat-monitor'); box.innerHTML = "";
                s.forEach(d => {
                    const c = d.data();
                    box.innerHTML += `<div class="p-2 border-b border-white/5 text-[9px]"><b class="text-blue-400">@${c.sender}:</b> ${c.type==='image'?'[IMAGE]':c.text}</div>`;
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
                    <div class="text-right">Bal: $${(u.balance || 0).toFixed(2)}<br><span class="text-red-500 font-bold cursor-pointer" onclick="delUser('${uDoc.id}')">DELETE</span></div>
                </div>`;
            });
        }

        function loadAdminLogs() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-4 text-[9px] flex justify-between items-center">
                        <div><b>${l.user}</b>: $${l.amount} (${l.type})<br>Ref: ${l.ref}</div>
                        <button onclick="approveReq('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-600 text-white px-4 py-2 rounded-lg font-bold uppercase">Approve</button>
                    </div>`;
                });
            });
        }

        window.approveReq = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            if(type === 'Withdraw') await updateDoc(uRef, { balance: (s.data().balance || 0) - amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Transaction Verified");
        };

        window.delUser = async (id) => { if(confirm("Confirm user deletion?")) { await deleteDoc(doc(db, "users", id)); loadMasterUsers(); } };

        // INITIALIZE
        function initApp(id) {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
            });
            listenChat(id);
            onSnapshot(query(collection(db, "logs"), where("user", "==", id)), s => {
                const list = document.getElementById('logs-list'); list.innerHTML = "";
                s.forEach(d => {
                    const l = d.data();
                    list.innerHTML += `<div class="premium-card p-4 flex justify-between text-[10px]">
                        <div><b>${l.type}</b><br>$${l.amount}</div>
                        <div class="${l.status==='Pending'?'text-orange-500':'text-green-500'} font-bold uppercase">${l.status}</div>
                    </div>`;
                });
            });
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
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Create Verified Account" : "Login Instead"; };

        if(user) initApp(user);
    </script>
</body>
</html>
