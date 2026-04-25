<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA CORP | Global Enterprise Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #D4AF37; --dark: #0F172A; }
        body { background: #F8FAFC; font-family: 'Plus Jakarta Sans', sans-serif; color: #1e293b; -webkit-tap-highlight-color: transparent; }
        .premium-card { background: white; border: 1px solid #e2e8f0; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #D4AF37, #B8860B); color: white; border-radius: 12px; font-weight: 800; cursor: pointer; border: none; }
        .screen { display: none; }
        .active-screen { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; border-top: 1px solid #e2e8f0; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
        .nav-item { color: #94a3b8; font-size: 9px; font-weight: 800; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--gold); }
    </style>
</head>
<body>

    <div id="auth-screen" class="active-screen">
        <section class="p-8 text-center bg-white min-h-screen flex flex-col justify-center">
            <h1 class="text-4xl font-black tracking-tighter">NO<span class="text-[#D4AF37]">VA</span></h1>
            <div class="mt-10 space-y-4 max-w-xs mx-auto w-full">
                <input type="text" id="u-id" placeholder="Username" class="w-full p-4 rounded-xl border bg-slate-50 outline-none">
                <input type="password" id="u-pw" placeholder="Password" class="w-full p-4 rounded-xl border bg-slate-50 outline-none">
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
                <p class="text-[8px] font-bold text-green-500 uppercase">System Secure</p>
            </div>
        </header>

        <div id="page-dash" class="screen active-screen px-4 pt-6 pb-24">
            <div class="bg-slate-900 rounded-[24px] p-6 mb-6 text-white shadow-xl">
                <p class="text-[8px] text-slate-500 font-bold uppercase mb-1">Account Liquidity</p>
                <h1 id="b-main" class="text-4xl font-black tracking-tight mb-6">$0.00</h1>
                <div class="grid grid-cols-2 gap-4 border-t border-white/5 pt-4">
                    <div><p class="text-[7px] text-slate-500 uppercase">Daily</p><p id="b-daily" class="text-sm font-bold text-green-400">+$0.00</p></div>
                    <div class="text-right"><p class="text-[7px] text-slate-500 uppercase">Total</p><p id="b-total" class="text-sm font-bold text-[#D4AF37]">$0.00</p></div>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-3 mb-6">
                <button onclick="openModal('modal-dep')" class="premium-card p-5 text-center"><i class="fa-solid fa-plus text-[#D4AF37] mb-2"></i><br><span class="text-[9px] font-black uppercase">Deposit</span></button>
                <button onclick="openModal('modal-wd')" class="premium-card p-5 text-center"><i class="fa-solid fa-bank text-slate-400 mb-2"></i><br><span class="text-[9px] font-black uppercase">Withdraw</span></button>
            </div>
            <div class="premium-card p-4">
                <p class="text-[9px] font-bold text-slate-400 uppercase mb-2">Referral ID</p>
                <div class="flex gap-2">
                    <input type="text" id="ref-link" readonly class="flex-1 bg-slate-50 p-2 text-[10px] rounded border" value="nova-corp.io/ref=user">
                    <button onclick="copyRef()" class="btn-gold px-4 text-[9px]">COPY</button>
                </div>
            </div>
        </div>

        <div id="page-chat" class="screen px-4 pt-6 pb-24">
            <div class="premium-card flex flex-col h-[70vh]">
                <div class="p-4 border-b bg-slate-50 rounded-t-[20px] flex justify-between items-center">
                    <span class="text-[10px] font-black uppercase tracking-widest text-slate-600">Live Support</span>
                </div>
                <div id="chat-box" class="flex-1 overflow-y-auto p-4 space-y-4 text-[12px]"></div>
                <div class="p-4 border-t bg-slate-50 rounded-b-[20px] flex gap-2">
                    <label class="bg-white border p-3 rounded-xl cursor-pointer"><input type="file" class="hidden" onchange="handleImage(this)"><i class="fa-solid fa-image text-slate-400"></i></label>
                    <input type="text" id="chat-input" placeholder="Enter query..." class="flex-1 p-3 rounded-xl border outline-none text-sm">
                    <button onclick="sendMsg()" class="btn-gold px-5"><i class="fa-solid fa-paper-plane"></i></button>
                </div>
            </div>
        </div>

        <div id="page-admin" class="screen px-4 pt-6 pb-24">
            <h2 class="text-xl font-black mb-4 uppercase text-red-600">Master Console</h2>
            
            <div class="premium-card p-4 mb-4 bg-slate-900 text-white">
                <h3 class="text-[9px] font-bold uppercase text-slate-500 mb-3">Support Monitor & Reply</h3>
                <div id="adm-chat-monitor" class="space-y-3 max-h-60 overflow-y-auto mb-4 text-[10px]"></div>
                <div class="flex gap-2">
                    <input type="text" id="adm-reply-uid" placeholder="User ID" class="w-1/3 p-2 rounded bg-white/10 text-white outline-none">
                    <input type="text" id="adm-reply-msg" placeholder="Reply text..." class="flex-1 p-2 rounded bg-white/10 text-white outline-none">
                    <button onclick="adminReply()" class="btn-gold px-3 text-[9px]">REPLY</button>
                </div>
            </div>

            <div class="premium-card p-4 mb-4">
                <h3 class="text-[9px] font-bold uppercase text-slate-400 mb-3">User Management</h3>
                <input type="text" id="user-search" onkeyup="searchUsers()" placeholder="Search username..." class="w-full p-2 border rounded-lg mb-3 text-sm">
                <div id="adm-user-list" class="space-y-2 max-h-40 overflow-y-auto"></div>
            </div>

            <div id="adm-pending-list" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('page-dash', this)" class="nav-item active"><i class="fa-solid fa-grid-2"></i><span>DASHBOARD</span></div>
            <div onclick="nav('page-chat', this)" class="nav-item"><i class="fa-solid fa-headset"></i><span>SUPPORT</span></div>
            <div id="nav-admin-btn" onclick="nav('page-admin', this)" class="nav-item text-red-600 hidden"><i class="fa-solid fa-lock"></i><span>ADMIN</span></div>
            <div onclick="logout()" class="nav-item"><i class="fa-solid fa-sign-out"></i><span>EXIT</span></div>
        </nav>
    </div>

    <div id="modal-dep" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-6"><h2 class="font-black text-xl">DEPOSIT</h2><i onclick="closeModal('modal-dep')" class="fa-solid fa-xmark cursor-pointer"></i></div>
        <p class="text-[10px] font-bold mb-4 bg-slate-100 p-3 rounded">Wallets: EasyPaisa 0340... / USDT: 0x99...</p>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-xl mb-3">
        <input type="text" id="dep-txid" placeholder="Transaction ID" class="w-full p-4 border rounded-xl mb-6">
        <button onclick="handleFinance('Deposit')" class="w-full btn-gold py-4">SUBMIT REQUEST</button>
    </div>

    <div id="modal-wd" class="fixed inset-0 bg-white z-[2000] p-8 hidden">
        <div class="flex justify-between items-center mb-6"><h2 class="font-black text-xl">WITHDRAW</h2><i onclick="closeModal('modal-wd')" class="fa-solid fa-xmark cursor-pointer"></i></div>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="w-full p-4 border rounded-xl mb-3">
        <input type="text" id="wd-addr" placeholder="Wallet Address" class="w-full p-4 border rounded-xl mb-6">
        <button onclick="handleFinance('Withdraw')" class="w-full btn-gold py-4">REQUEST PAYOUT</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy, getDocs, deleteDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);

        let user = localStorage.getItem('nova_session');
        let mode = 'login'; let admClicks = 0;

        // Admin Secret
        document.getElementById('logo-main').onclick = () => {
            admClicks++;
            if(admClicks >= 5) {
                if(prompt("Admin Access:") === "nov786") {
                    document.getElementById('nav-admin-btn').classList.remove('hidden');
                    loadMasterUsers(); loadAdminMonitor(); loadAdminPending();
                    alert("Master Admin Mode Active");
                }
                admClicks = 0;
            }
        };

        // Auth
        window.handleAuth = async () => {
            const id = document.getElementById('u-id').value.toLowerCase().trim();
            const pw = document.getElementById('u-pw').value;
            if(!id || !pw) return alert("Fields required");
            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);
            if(mode === 'login') {
                if(snap.exists() && snap.data().password === pw) { localStorage.setItem('nova_session', id); location.reload(); }
                else alert("Invalid credentials");
            } else {
                if(snap.exists()) return alert("User exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total_profit: 0, status: 'Active' });
                localStorage.setItem('nova_session', id); location.reload();
            }
        };

        // Chat & Admin Reply
        window.sendMsg = async () => {
            const input = document.getElementById('chat-input');
            if(!input.value.trim()) return;
            await addDoc(collection(db, "chats"), { sender: user, text: input.value, time: Date.now(), type: 'text' });
            input.value = "";
        };

        window.adminReply = async () => {
            const rUid = document.getElementById('adm-reply-uid').value.trim();
            const rMsg = document.getElementById('adm-reply-msg').value.trim();
            if(!rUid || !rMsg) return alert("Fill user ID and message");
            await addDoc(collection(db, "chats"), { sender: 'ADMIN', receiver: rUid, text: rMsg, time: Date.now(), type: 'text' });
            alert("Reply sent to " + rUid);
            document.getElementById('adm-reply-msg').value = "";
        };

        function listenChat(uid) {
            const q = query(collection(db, "chats"), orderBy("time", "asc"));
            onSnapshot(q, (s) => {
                const box = document.getElementById('chat-box'); box.innerHTML = "";
                s.forEach(d => {
                    const c = d.data();
                    if(c.sender === uid || c.receiver === uid || (c.sender === 'ADMIN' && c.receiver === uid)) {
                        const div = document.createElement('div');
                        div.className = c.sender === uid ? 'bg-gold ml-auto bg-[#D4AF37] text-white p-3 rounded-2xl max-w-[80%]' : 'bg-slate-100 mr-auto p-3 rounded-2xl max-w-[80%]';
                        div.innerHTML = c.type === 'image' ? `<img src="${c.text}" class="w-full rounded">` : c.text;
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
                    monitor.innerHTML += `<div class="p-2 border-b border-white/5 cursor-pointer" onclick="document.getElementById('adm-reply-uid').value='${c.sender}'">
                        <b class="${c.sender==='ADMIN'?'text-gold':'text-blue-400'}">@${c.sender}:</b> ${c.text.substring(0,20)}...
                    </div>`;
                });
            });
        }

        // Master User Control
        async function loadMasterUsers() {
            const q = await getDocs(collection(db, "users"));
            const box = document.getElementById('adm-user-list'); box.innerHTML = "";
            q.forEach(uDoc => {
                const u = uDoc.data();
                box.innerHTML += `<div class="p-3 border rounded-xl flex justify-between items-center text-[10px] user-item" data-name="${uDoc.id}">
                    <div><b>@${uDoc.id}</b><br>Bal: $${(u.balance || 0).toFixed(2)}</div>
                    <div class="flex gap-2">
                        <button onclick="editBal('${uDoc.id}')" class="text-blue-500 font-bold">EDIT</button>
                        <button onclick="delUser('${uDoc.id}')" class="text-red-500 font-bold">DEL</button>
                    </div>
                </div>`;
            });
        }

        window.editBal = async (id) => {
            const nBal = parseFloat(prompt("Enter New Balance:"));
            if(!isNaN(nBal)) {
                await updateDoc(doc(db, "users", id), { balance: nBal });
                alert("Balance updated"); loadMasterUsers();
            }
        };

        window.searchUsers = () => {
            const val = document.getElementById('user-search').value.toLowerCase();
            document.querySelectorAll('.user-item').forEach(i => {
                i.style.display = i.getAttribute('data-name').includes(val) ? 'flex' : 'none';
            });
        };

        // Finance Requests
        window.handleFinance = async (type) => {
            const amt = parseFloat(document.getElementById(type === 'Deposit' ? 'dep-amt' : 'wd-amt').value);
            const ref = document.getElementById(type === 'Deposit' ? 'dep-txid' : 'wd-addr').value;
            if(!amt || !ref) return alert("Error: Incomplete Details");
            await addDoc(collection(db, "logs"), { user: user, type, amount: amt, ref, status: 'Pending', time: Date.now() });
            alert("Request Registered with System");
            closeModal(type === 'Deposit' ? 'modal-dep' : 'modal-wd');
        };

        function loadAdminPending() {
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
            alert("Verified!");
        };

        // Helpers
        function initApp(id) {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const u = d.data(); if(!u) return;
                document.getElementById('user-display').innerText = `@${id}`;
                document.getElementById('b-main').innerText = `$${(u.balance || 0).toFixed(2)}`;
                document.getElementById('ref-link').value = `nova-corp.io/join?ref=${id}`;
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
        window.copyRef = () => { navigator.clipboard.writeText(document.getElementById('ref-link').value); alert("Link Copied"); };
        window.logout = () => { localStorage.clear(); location.reload(); };
        window.toggleAuth = () => { mode = mode === 'login' ? 'reg' : 'login'; document.getElementById('auth-btn-text').innerText = mode === 'login' ? "Register Corporate Account" : "Back to Login"; };

        if(user) initApp(user);
    </script>
</body>
</html>
