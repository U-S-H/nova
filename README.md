<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Global Mining Protocol</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;500;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --nova-gold: #D4AF37; --nova-bg: #F8F9FC; }
        body { background: var(--nova-bg); color: #1A1A1A; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        .glass-card { background: #FFFFFF; border: 1px solid #E2E8F0; border-radius: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.02); }
        .btn-nova { background: linear-gradient(135deg, #1A1A1A, #333); color: #fff; font-weight: 700; border-radius: 14px; transition: 0.3s; text-transform: uppercase; font-size: 11px; }
        .btn-nova:active { transform: scale(0.95); }
        .screen { display: none; padding-bottom: 120px; animation: fadeIn 0.4s ease; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: #fff; border-top: 1px solid #F1F5F9; display: flex; justify-content: space-around; padding: 18px; z-index: 1000; }
        .nav-item { color: #94A3B8; font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 4px; }
        .nav-item.active { color: var(--nova-gold); }
        .n-input { background: #F1F5F9; border: 1.5px solid transparent; width: 100%; padding: 16px; border-radius: 14px; color: #1A1A1A; outline: none; margin-bottom: 12px; transition: 0.3s; }
        .n-input:focus { border-color: var(--nova-gold); background: #fff; }
        .modal { display: none; position: fixed; inset: 0; background: #fff; z-index: 2000; padding: 25px; overflow-y: auto; }
        .broadcast-bar { background: #1A1A1A; color: var(--nova-gold); font-size: 10px; padding: 10px; font-weight: 800; text-align: center; text-transform: uppercase; letter-spacing: 1px; }
    </style>
</head>
<body>

    <div id="broadcast" class="broadcast-bar sticky top-0 z-[100]">System: Initializing Secure Hashrate...</div>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen bg-white">
        <div class="mb-12">
            <h1 onclick="handleTaps()" class="text-4xl font-extrabold tracking-tighter text-gray-900 italic">NOVA<span class="text-[#D4AF37]">.</span></h1>
            <p class="text-[10px] text-gray-400 font-bold uppercase tracking-[5px] mt-2">Protocol Access Terminal</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-email" placeholder="Terminal Identity" class="n-input">
            <input type="password" id="u-pass" placeholder="Security Passkey" class="n-input">
            <input type="password" id="adm-key" placeholder="System Override Code" class="hidden n-input border-[#D4AF37]">
            <button onclick="auth()" class="w-full btn-nova py-5 shadow-xl">Connect Protocol</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[11px] mt-10 text-gray-400 font-bold uppercase cursor-pointer">Request New Identity</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center bg-white/80 backdrop-blur-md sticky top-0 z-50 border-b border-gray-100">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 bg-black rounded-lg flex items-center justify-center text-[#D4AF37] font-bold text-xs">N</div>
                <p id="user-display" class="text-[10px] font-bold uppercase text-gray-800"></p>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-300"></i>
        </header>

        <div id="dash-screen" class="screen active-screen px-5 pt-4">
            <div class="glass-card p-8 mb-6 text-center bg-gradient-to-br from-white to-gray-50 border-b-4 border-[#D4AF37]">
                <p class="text-[10px] text-gray-400 font-bold uppercase mb-2">Net Portfolio Balance</p>
                <h1 id="main-bal" class="text-6xl font-extrabold tracking-tighter text-gray-900">$0.00</h1>
                <div id="v-daily" class="mt-4 text-[11px] font-bold text-green-600 bg-green-50 inline-block px-4 py-1 rounded-full uppercase tracking-wide">Yield: +$0.00 / 24h</div>
            </div>

            <div class="glass-card p-6 mb-6">
                <div class="flex justify-between items-center mb-4">
                    <p class="text-[10px] font-bold text-gray-400 uppercase">Yield Refresh</p>
                    <span id="t-display" class="font-mono text-lg font-extrabold text-gray-800">00:00:00</span>
                </div>
                <div class="w-full bg-gray-100 h-1.5 rounded-full overflow-hidden">
                    <div id="t-prog" class="bg-[#D4AF37] h-full transition-all duration-1000" style="width: 0%"></div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="glass-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-plus-square text-[#D4AF37] text-xl"></i><span class="text-[10px] font-bold uppercase">Funding</span></button>
                <button onclick="openModal('wd-modal')" class="glass-card p-6 flex flex-col items-center gap-3"><i class="fa-solid fa-arrow-up-right-from-square text-xl"></i><span class="text-[10px] font-bold uppercase">Extract</span></button>
            </div>

            <div class="glass-card p-6 mb-6 flex items-center gap-4 border-l-4 border-[#D4AF37]">
                <div class="w-12 h-12 bg-gray-50 rounded-2xl flex items-center justify-center"><i class="fa-solid fa-shield-halved text-[#D4AF37]"></i></div>
                <div>
                    <h4 class="text-[10px] font-extrabold uppercase">Official Distributor</h4>
                    <p class="text-[9px] text-gray-500 leading-tight">NOVA is a licensed institutional distributor of ASIC hardware since 2018.</p>
                </div>
            </div>
        </div>

        <div id="mine-screen" class="screen px-5 pt-4">
            <h2 class="text-2xl font-extrabold mb-8 italic">THE <span class="text-[#D4AF37]">FLEET</span></h2>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <div id="hist-screen" class="screen px-5 pt-4">
            <h2 class="text-2xl font-extrabold mb-8 italic text-gray-900">SYSTEM <span class="text-[#D4AF37]">LOGS</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-layer-group text-xl"></i><span>CORE</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-xl"></i><span>FLEET</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-clock-rotate-left text-xl"></i><span>AUDIT</span></div>
        </nav>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-[#F1F5F9] z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-10 pb-4 border-b border-gray-200">
            <h2 class="font-extrabold text-black text-sm tracking-widest uppercase">Nova Root Admin</h2>
            <button onclick="location.reload()" class="bg-red-500 text-white px-4 py-2 rounded-lg text-[10px] font-bold">EXIT</button>
        </div>

        <div class="space-y-6">
            <div class="glass-card p-6">
                <h3 class="text-[11px] font-bold mb-4 uppercase">Global Broadcast</h3>
                <input type="text" id="adm-msg" placeholder="Live message for all users..." class="n-input bg-white">
                <button onclick="broadcastMsg()" class="w-full btn-nova py-4">Push Update</button>
            </div>

            <div class="glass-card p-6">
                <h3 class="text-[11px] font-bold mb-4 uppercase">Inject Balance (By User ID)</h3>
                <input type="text" id="adm-uid" placeholder="User ID" class="n-input bg-white">
                <input type="number" id="adm-amt" placeholder="Amount ($)" class="n-input bg-white">
                <button onclick="injectBal()" class="w-full btn-nova py-4">Execute Injection</button>
            </div>

            <div class="glass-card p-6">
                <h3 class="text-[11px] font-bold mb-4 uppercase text-[#D4AF37]">Active Audit Requests</h3>
                <div id="adm-requests" class="space-y-4"></div>
            </div>
        </div>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-8 text-gray-400 font-bold text-[10px] uppercase">Back</div>
        <h2 class="text-3xl font-extrabold mb-8 italic">FUNDING</h2>
        <div class="glass-card p-6 mb-8 text-center bg-gray-50 border-dashed border-gray-300">
            <p id="dep-addr" class="text-[11px] font-mono break-all text-gray-500">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Amount ($)" class="n-input">
        <input type="text" id="dep-tid" placeholder="Transaction Reference (TID)" class="n-input">
        <input type="file" id="dep-proof" accept="image/*" class="text-[11px] mb-10 font-bold">
        <button onclick="requestFin('Deposit')" class="w-full btn-nova py-5 shadow-lg">Authenticate Funding</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-8 text-gray-400 font-bold text-[10px] uppercase">Back</div>
        <h2 class="text-3xl font-extrabold mb-8 text-red-600 italic">EXTRACTION</h2>
        <input type="number" id="wd-amt" placeholder="Extraction Amount ($)" class="n-input">
        <input type="text" id="wd-acc" placeholder="Target Account" class="n-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-black text-white py-5 rounded-2xl text-[11px] font-bold uppercase">Verify & Extract</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const config = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(config); const db = getFirestore(app);
        let user = localStorage.getItem('nova_session'); let login = true;

        if(user) start(user);

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadA(); return; }
            if(!id || !pw) return alert("Identify Required");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!login) {
                if(s.exists()) return alert("Terminal Taken");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, ref: "NOVA-" + Math.floor(1000+Math.random()*9000) });
                localStorage.setItem('nova_session', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('nova_session', id); start(id);
            } else alert("Access Denied");
        };

        function start(id) {
            user = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const r = d.data();
                document.getElementById('main-bal').innerText = "$" + (r.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Yield: +$" + (r.daily || 0).toFixed(2) + " / 24h";
                document.getElementById('ref-id').innerText = r.ref;
                document.getElementById('user-display').innerText = id;
            });
            onSnapshot(doc(db, "meta", "broadcast"), d => { if(d.exists()) document.getElementById('broadcast').innerText = d.data().msg; });
            loadH(); loadN(); startTimer();
        }

        window.logout = () => { localStorage.removeItem('nova_session'); location.reload(); };

        function startTimer() {
            setInterval(() => {
                const now = new Date();
                const h = 23 - now.getHours(), m = 59 - now.getMinutes(), s = 59 - now.getSeconds();
                document.getElementById('t-display').innerText = `${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
                document.getElementById('t-prog').style.width = ((now.getHours() * 3600 + now.getMinutes() * 60 + now.getSeconds()) / 86400 * 100) + "%";
            }, 1000);
        }

        function loadN() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const imgs = ["https://images.unsplash.com/photo-1624365169192-6997bb1f43c8?q=80&w=400", "https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=400"];
            for(let i=1; i<=20; i++) {
                let p = i===1?20:(i*1000); let d = (p*0.15).toFixed(2);
                c.innerHTML += `<div class="glass-card overflow-hidden">
                    <img src="${imgs[i%2]}" class="w-full h-36 object-cover grayscale">
                    <div class="p-6 flex justify-between items-center">
                        <div><h4 class="font-extrabold text-sm uppercase">NOVA NODE v.${i}</h4><p class="text-[10px] text-green-600 font-bold">+$${d} DAILY</p></div>
                        <button onclick="buyN(${p},${d})" class="btn-nova px-6 py-3">$${p}</button>
                    </div>
                </div>`;
            }
        }

        window.buyN = async (p, d) => {
            const uR = doc(db, "users", user); const s = await getDoc(uR);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(uR, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Node Online");
        };

        window.requestFin = async (type) => {
            let p = { user, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') {
                p.amount = parseFloat(document.getElementById('dep-amt').value); p.tid = document.getElementById('dep-tid').value;
                const f = document.getElementById('dep-proof').files[0];
                if(f) { const r = new FileReader(); r.onloadend = async () => { p.proof = r.result; await addDoc(collection(db, "logs"), p); alert("Sent"); closeModal('dep-modal'); }; r.readAsDataURL(f); }
                else { await addDoc(collection(db, "logs"), p); alert("Sent"); closeModal('dep-modal'); }
            } else {
                p.amount = parseFloat(document.getElementById('wd-amt').value); p.acc = document.getElementById('wd-acc').value;
                const uR = doc(db, "users", user); const s = await getDoc(uR);
                if(s.data().balance < p.amount) return alert("Insufficient Balance");
                await updateDoc(uR, { balance: s.data().balance - p.amount });
                await addDoc(collection(db, "logs"), p); alert("Pending Audit"); closeModal('wd-modal');
            }
        };

        function loadH() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc")), s => {
                const c = document.getElementById('hist-container'); c.innerHTML = "";
                s.forEach(d => { const r = d.data(); c.innerHTML += `<div class="glass-card p-5 flex justify-between"><div><p class="text-[8px] font-bold text-gray-400 uppercase">${r.type}</p><p class="text-sm font-extrabold">$${r.amount}</p></div><span class="text-[9px] font-bold ${r.status=='Pending'?'text-yellow-500':'text-green-500'}">${r.status}</span></div>`; });
            });
        }

        // 👑 SUPER ADMIN LOGIC
        function loadA() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const rList = document.getElementById('adm-requests'); rList.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    rList.innerHTML += `<div class="p-4 bg-white border-b flex justify-between items-center">
                        <div><p class="text-[10px] font-bold">${r.user}</p><p class="text-lg font-black">$${r.amount}</p></div>
                        <div class="flex gap-2">
                            ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[10px] font-bold">IMAGE</button>` : ''}
                            <button onclick="admAct('${d.id}','${r.user}',${r.amount},'${r.type}')" class="bg-green-600 text-white px-4 py-2 rounded text-[9px] font-bold">APPROVE</button>
                        </div>
                    </div>`;
                });
            });
        }

        window.broadcastMsg = async () => { const msg = document.getElementById('adm-msg').value; await setDoc(doc(db, "meta", "broadcast"), { msg }); alert("Broadcast Live!"); };
        window.injectBal = async () => { const uid = document.getElementById('adm-uid').value; const amt = parseFloat(document.getElementById('adm-amt').value); const uR = doc(db, "users", uid); const s = await getDoc(uR); if(s.exists()) { await updateDoc(uR, { balance: (s.data().balance || 0) + amt }); alert("Balance Injected!"); } };
        window.admAct = async (id, u, a, t) => { if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: 'Success' }); };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { login = !login; document.getElementById('m-text').innerText = login ? "Request New Identity" : "Return to Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); document.getElementById('nav-' + id.split('-')[0]).classList.add('active'); };
    </script>
</body>
</html>
