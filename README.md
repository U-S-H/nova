<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Enterprise Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 12px; font-weight: 800; cursor: pointer; border: none; transition: 0.3s; }
        .btn-gold:active { transform: scale(0.96); }
        .screen { display: none; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
        input { border: 1px solid #e2e8f0 !important; outline: none !important; }
        input:focus { border-color: var(--gold) !important; }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-4xl font-black tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-widest mt-2">Global Asset Management</p>
            <div class="mt-10 space-y-4 max-w-xs mx-auto w-full">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-xl border bg-slate-50">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-xl border bg-slate-50">
                <button onclick="handleAuth()" class="w-full btn-gold py-4 text-xs uppercase tracking-widest">Authorize Access</button>
                <p onclick="toggleAuth()" id="auth-btn-text" class="text-[10px] text-slate-400 font-bold uppercase cursor-pointer underline">Register Corporate Account</p>
            </div>
        </section>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white border-b sticky top-0 z-[100]">
            <h2 id="logo-main" class="font-black text-2xl tracking-tighter cursor-pointer">NO<span class="text-[#D4AF37]">VA</span></h2>
            <div class="text-right">
                <p id="user-display" class="text-[10px] font-black uppercase text-slate-600">@USER</p>
                <div class="flex items-center gap-1 justify-end"><div class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></div><p class="text-[8px] font-bold text-green-500 uppercase">Secure</p></div>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[28px] p-6 mb-6 text-white shadow-xl relative overflow-hidden">
                <div class="absolute -right-4 -top-4 opacity-5"><i class="fa-solid fa-gem text-9xl"></i></div>
                <p class="text-[8px] text-slate-500 font-bold uppercase mb-1">Portfolio Balance</p>
                <h1 id="b-main" class="text-4xl font-black tracking-tight mb-8">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/5 pt-5">
                    <div><p class="text-[7px] text-slate-500 uppercase font-bold">Daily Yield</p><p id="b-daily" class="text-sm font-bold text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 uppercase font-bold">Total Profit</p><p id="b-total" class="text-sm font-bold text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            
            <div class="grid grid-cols-2 gap-3 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-5 text-center"><i class="fa-solid fa-wallet text-[#D4AF37] text-xl mb-2"></i><br><span class="text-[9px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-5 text-center"><i class="fa-solid fa-paper-plane text-slate-400 text-xl mb-2"></i><br><span class="text-[9px] font-black uppercase">Withdraw</span></button>
            </div>

            <div class="premium-card p-5 mb-6">
                <h3 class="text-[9px] font-bold text-slate-400 uppercase mb-3">Referral Network</h3>
                <div class="flex gap-2">
                    <input type="text" id="ref-link" readonly class="flex-1 bg-slate-50 p-2.5 text-[10px] rounded-lg border font-mono">
                    <button onclick="copyRef()" class="btn-gold px-4 text-[9px]">COPY</button>
                </div>
            </div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6 pb-24">
            <div class="premium-card flex flex-col h-[70vh]">
                <div class="p-4 border-b bg-slate-50 rounded-t-[20px] flex justify-between items-center">
                    <span class="text-[10px] font-black uppercase tracking-widest text-slate-600">Enterprise Support</span>
                </div>
                <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-4 text-[12px] scroll-smooth"></div>
                <div class="p-4 border-t bg-slate-50 rounded-b-[20px] flex gap-2">
                    <label class="bg-white border p-3 rounded-xl cursor-pointer"><input type="file" class="hidden" onchange="handleImage(this)"><i class="fa-solid fa-camera text-slate-400"></i></label>
                    <input type="text" id="chat-input" placeholder="Type your query..." class="flex-1 p-3 rounded-xl border text-sm">
                    <button onclick="sendMsg()" class="btn-gold px-5"><i class="fa-solid fa-paper-plane"></i></button>
                </div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-5 uppercase text-red-600">Admin Control Center</h2>
            
            <div class="premium-card p-4 mb-4 bg-slate-900 text-white">
                <h3 class="text-[9px] font-bold uppercase text-slate-500 mb-4">Support Monitor</h3>
                <div id="adm-chat-monitor" class="space-y-2 max-h-48 overflow-y-auto mb-4 text-[10px] border-b border-white/5 pb-2"></div>
                <div class="space-y-2">
                    <input type="text" id="adm-reply-uid" placeholder="User ID" class="w-full p-2.5 rounded bg-white/10 text-white text-xs border-none">
                    <div class="flex gap-2">
                        <input type="text" id="adm-reply-msg" placeholder="Write reply..." class="flex-1 p-2.5 rounded bg-white/10 text-white text-xs border-none">
                        <button onclick="adminReply()" class="btn-gold px-4 text-[9px]">SEND</button>
                    </div>
                </div>
            </div>

            <div class="premium-card p-4 mb-4">
                <h3 class="text-[9px] font-bold uppercase text-slate-400 mb-3">User Database</h3>
                <input type="text" id="user-search" onkeyup="searchUsers()" placeholder="Search username..." class="w-full p-2.5 border rounded-lg mb-4 text-xs">
                <div id="adm-user-list" class="space-y-3 max-h-60 overflow-y-auto"></div>
            </div>

            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-house"></i><span>HOME</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-comment-dots"></i><span>CHAT</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600 hidden"><i class="fa-solid fa-shield-halved"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-power-off text-red-400"></i><span>LOGOUT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl uppercase">Deposit Assets</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark text-xl cursor-pointer"></i></div>
        <div class="bg-slate-50 p-4 rounded-xl mb-6 text-[10px] font-bold border">
            <p>EASYPAISA: 03XX-XXXXXXX</p>
            <p class="mt-1">USDT (TRC20): TXXXXXXXXXXXXXXX</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-xl mb-4">
        <input type="text" id="dep-txid" placeholder="Transaction ID" class="w-full p-4 border rounded-xl mb-8">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-4">SUBMIT FOR REVIEW</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-8"><h2 class="font-black text-xl uppercase">Withdraw Assets</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark text-xl cursor-pointer"></i></div>
        <input type="number" id="wd-amt" placeholder="Payout Amount ($)" class="w-full p-4 border rounded-xl mb-4">
        <input type="text" id="wd-addr" placeholder="Wallet/Account Details" class="w-full p-4 border rounded-xl mb-8">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-4">REQUEST WITHDRAWAL</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // FIREBASE CONFIGURATION
        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;

        // ADMIN ACCESS TRIGGER
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Enter Terminal PIN:") === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    loadMasterUsers(); loadAdminMonitor(); loadAdminPending();
                    alert("ADMIN CONSOLE GRANTED");
                }
                admClicks = 0;
            }
        };

        // AUTH SYSTEM
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Required fields missing");
            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Authentication failed");
            } else {
                if(snap.exists()) return alert("Username unavailable");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, status: 'Active', created: Date.now() });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // CHAT ENGINE
        window.sendMsg = async () => {
            const input = document.getElementById('chat-input');
            if(!input.value.trim()) return;
            await addDoc(collection(db, "chats"), { sender: user, text: input.value, time: Date.now(), type: 'text' });
            input.value = "";
        };

        window.adminReply = async () => {
            const rUid = document.getElementById('adm-reply-uid').value.trim();
            const rMsg = document.getElementById('adm-reply-msg').value.trim();
            if(!rUid || !rMsg) return alert("User ID and Message required");
            await addDoc(collection(db, "chats"), { sender: 'ADMIN', receiver: rUid, text: rMsg, time: Date.now(), type: 'text' });
            document.getElementById('adm-reply-msg').value = "";
            alert("Reply sent to " + rUid);
        };

        function listenChat(uid) {
            const q = query(collection(db, "chats"), orderBy("time", "asc"));
            onSnapshot(q, (s) => {
                const box = document.getElementById('chat-box'); box.innerHTML = "";
                s.forEach(d => {
                    const c = d.data();
                    if(c.sender === uid || c.receiver === uid) {
                        const div = document.createElement('div');
                        div.className = c.sender === uid ? 'bg-gold ml-auto bg-[#D4AF37] text-white p-3 rounded-2xl max-w-[85%] shadow-sm' : 'bg-slate-100 mr-auto p-3 rounded-2xl max-w-[85%]';
                        div.innerHTML = c.type === 'image' ? `<img src="${c.text}" class="rounded-lg w-full">` : c.text;
                        box.appendChild(div);
                    }
                });
                box.scrollTop = box.scrollHeight;
            });
        }

        function loadAdminMonitor() {
            onSnapshot(query(collection(db, "chats"), orderBy("time", "desc")), s => {
                const monitor = document.getElementById('adm-chat-monitor'); monitor.innerHTML = "";
                s.forEach(d => {
                    const c = d.data();
                    if(c.sender !== 'ADMIN') {
                        monitor.innerHTML += `<div class="p-2 border-b border-white/5 cursor-pointer hover:bg-white/5" onclick="document.getElementById('adm-reply-uid').value='${c.sender}'">
                            <span class="text-blue-400">@${c.sender}:</span> ${c.text.substring(0,30)}...
                        </div>`;
                    }
                });
            });
        }

        // MASTER CONTROL
        async function loadMasterUsers() {
            const q = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list'); box.innerHTML = "";
            q.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-3 border rounded-xl flex justify-between items-center text-[10px] user-item" data-name="${uDoc.id}">
                    <div><b>@${uDoc.id}</b><br>Bal: $${(u.balance || 0).toFixed(2)}<br>PW: ${u.password}</div>
                    <div class="flex gap-2">
                        <button onclick="editBalance('${uDoc.id}')" class="text-blue-600 font-bold uppercase">Edit</button>
                        <button onclick="delUser('${uDoc.id}')" class="text-red-500 font-bold uppercase">Del</button>
                    </div>
                </div>`;
            });
        }

        window.editBalance = async (id) => {
            const amount = parseFloat(prompt("Enter New Balance for " + id + ":"));
            if(!isNaN(amount)) {
                await updateDoc(doc(db, "users", id), { balance: amount });
                alert("Balance updated"); loadMasterUsers();
            }
        };

        window.searchUsers = () => {
            const val = document.getElementById('user-search').value.toLowerCase();
            document.querySelectorAll('.user-item').forEach(el => {
                el.style.display = el.getAttribute('data-name').includes(val) ? 'flex' : 'none';
            });
        };

        // FINANCE SYSTEM
        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Incomplete data submitted");
            await addDoc(collection(db, "logs"), { user: user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Application submitted for terminal review");
            closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        function loadAdminPending() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const list = document.getElementById('adm-pending-list'); list.innerHTML = "";
                s.forEach(ds => {
                    const l = ds.data();
                    list.innerHTML += `<div class="premium-card p-4 text-[9px] flex justify-between items-center mb-3">
                        <div><b>${l.user}</b>: $${l.amount} (${l.type})<br>Ref: ${l.ref}</div>
                        <button onclick="approveRequest('${ds.id}', '${l.user}', ${l.amount}, '${l.type}')" class="bg-green-600 text-white px-4 py-2 rounded-lg font-bold uppercase">Verify</button>
                    </div>`;
                });
            });
        }

        window.approveRequest = async (lid, uid, amt, type) => {
            const uRef = doc(db, "users", uid); const s = await getDoc(uRef);
            if(type === 'Deposit') await updateDoc(uRef, { balance: (s.data().balance || 0) + amt });
            if(type === 'Withdraw') await updateDoc(uRef, { balance: (s.data().balance || 0) - amt });
            await updateDoc(doc(db, "logs", lid), { status: 'Approved' });
            alert("Transaction Confirmed");
        };

        // INITIALIZATION
        function initApp(id) {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('b-daily').innerText = `+$${(u.daily || 0).toFixed(2)}`;
                document.getElementById('b-total').innerText = `$${(u.total_profit || 0).toFixed(2)}`;
                document.getElementById('ref-link').value = `nova-corp.io/auth?ref=${id}`;
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
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Register Corporate Account" : "Back to Login"; };
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').value); alert("Referral Link Copied"); };

        if(user) initApp(user);
    </script>
</body>
</html>
