<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PRIME SOLUTIONS | Global Asset Management</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --prime-gold: #B8860B; --prime-silver: #F8F9FA; --prime-text: #1A1A1A; }
        body { background: var(--prime-silver); color: var(--prime-text); font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; }
        .prime-card { background: #FFFFFF; border: 1px solid #E5E7EB; border-radius: 20px; box-shadow: 0 10px 25px rgba(0,0,0,0.03); }
        .btn-gold { background: linear-gradient(135deg, #B8860B 0%, #D4AF37 100%); color: #fff; font-weight: 700; border-radius: 12px; transition: 0.3s; text-transform: uppercase; font-size: 11px; }
        .btn-gold:active { transform: scale(0.97); }
        .screen { display: none; padding-bottom: 110px; animation: fadeIn 0.5s ease; }
        .active-screen { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: #fff; border-top: 1px solid #EEE; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; box-shadow: 0 -5px 20px rgba(0,0,0,0.02); }
        .nav-item { color: #999; font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-item.active { color: var(--prime-gold); }
        .prime-input { background: #FFF; border: 1.5px solid #E5E7EB; width: 100%; padding: 15px; border-radius: 12px; color: #1A1A1A; font-size: 14px; outline: none; margin-bottom: 12px; transition: 0.3s; }
        .prime-input:focus { border-color: var(--prime-gold); }
        .modal { display: none; position: fixed; inset: 0; background: #fff; z-index: 2000; padding: 25px; overflow-y: auto; }
        .machine-badge { background: #F3F4F6; color: #374151; font-size: 9px; padding: 4px 10px; border-radius: 50px; font-weight: 700; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col p-8 justify-center active-screen">
        <div class="text-center mb-12">
            <h1 onclick="handleTaps()" class="text-3xl font-extrabold tracking-tight text-gray-900">PRIME<span class="text-[#B8860B]">SOLUTIONS</span></h1>
            <p class="text-[10px] text-gray-400 font-bold uppercase tracking-[4px] mt-2">Certified Asset Distributor</p>
        </div>
        <div class="space-y-2">
            <input type="text" id="u-email" placeholder="Institutional ID" class="prime-input">
            <input type="password" id="u-pass" placeholder="Secure Passkey" class="prime-input">
            <input type="text" id="adm-key" placeholder="Admin Access Code" class="hidden prime-input border-gold">
            <button onclick="auth()" class="w-full btn-gold py-5 shadow-lg">Access Portal</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[11px] mt-8 text-gray-500 font-bold uppercase cursor-pointer tracking-wider">Register Professional Account</p>
        </div>
    </div>

    <div id="app" class="hidden">
        <header class="p-5 flex justify-between items-center sticky top-0 bg-white/90 backdrop-blur-md z-50 border-b border-gray-100">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 bg-[#B8860B] rounded-lg flex items-center justify-center text-white text-xs font-bold shadow-md">P</div>
                <p id="user-display" class="text-[11px] font-bold uppercase text-gray-800"></p>
            </div>
            <i onclick="logout()" class="fa-solid fa-arrow-right-from-bracket text-gray-400"></i>
        </header>

        <div id="dash-screen" class="screen active-screen px-5 pt-4">
            <div class="prime-card p-8 mb-6 text-center bg-gradient-to-b from-white to-gray-50">
                <p class="text-[10px] text-gray-400 font-bold uppercase mb-2 tracking-widest">Global Portfolio Balance</p>
                <h1 id="main-bal" class="text-5xl font-extrabold text-gray-900 tracking-tighter">$0.00</h1>
                <div class="mt-5 flex justify-center items-center gap-2">
                    <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
                    <p id="v-daily" class="text-[11px] font-bold text-gray-600 uppercase">Live Yield: +$0.00 / day</p>
                </div>
            </div>

            <div class="prime-card p-5 mb-6 flex items-center gap-4 border-l-4 border-[#B8860B]">
                <i class="fa-solid fa-shield-check text-[#B8860B] text-2xl"></i>
                <div>
                    <h4 class="text-[11px] font-bold uppercase tracking-wide">Prime Trusted Distributor</h4>
                    <p class="text-[10px] text-gray-500">Official license certified for global mining assets.</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <button onclick="openModal('dep-modal')" class="prime-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-wallet text-[#B8860B]"></i><span class="text-[10px] font-bold uppercase">Funding</span></button>
                <button onclick="openModal('wd-modal')" class="prime-card p-6 flex flex-col items-center gap-2"><i class="fa-solid fa-money-bill-transfer text-gray-600"></i><span class="text-[10px] font-bold uppercase">Withdraw</span></button>
            </div>

            <div class="prime-card p-5 mb-6">
                <p class="text-[10px] font-bold text-gray-400 mb-3 uppercase">Promo Redemption</p>
                <div class="flex gap-2">
                    <input type="text" id="promo-inp" placeholder="VOUCHER CODE" class="prime-input mb-0 flex-1 py-3 text-xs uppercase bg-gray-50 border-none">
                    <button onclick="applyPromo()" class="btn-gold px-6 text-[10px]">Claim</button>
                </div>
            </div>

            <div class="prime-card p-5 flex justify-between items-center mb-6 bg-white">
                <div>
                    <p class="text-[10px] font-bold text-gray-400 mb-1 uppercase">Affiliate Node</p>
                    <span id="ref-id" class="text-xs font-bold text-gray-800">---</span>
                </div>
                <button onclick="alert('Link Copied')" class="text-[10px] font-bold text-[#B8860B] border-b border-[#B8860B]">COPY LINK</button>
            </div>
        </div>

        <div id="mine-screen" class="screen px-5 pt-4">
            <h2 class="text-xl font-extrabold mb-6 text-gray-900 uppercase">Premium <span class="text-[#B8860B]">Mining Nodes</span></h2>
            <div id="nodes-container" class="space-y-6"></div>
        </div>

        <div id="hist-screen" class="screen px-5 pt-4">
            <h2 class="text-xl font-extrabold mb-6 text-gray-900 uppercase">Transaction <span class="text-[#B8860B]">Audit</span></h2>
            <div id="hist-container" class="space-y-3"></div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-chimney text-lg"></i><span>PORTFOLIO</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-microchip text-lg"></i><span>NODES</span></div>
            <div onclick="nav('hist-screen')" class="nav-item" id="nav-hist"><i class="fa-solid fa-list-ul text-lg"></i><span>AUDIT</span></div>
        </nav>
    </div>

    <div id="dep-modal" class="modal">
        <div onclick="closeModal('dep-modal')" class="mb-10 text-gray-400 font-bold text-[11px] uppercase"><i class="fa-solid fa-chevron-left mr-2"></i>Exit Funding</div>
        <h2 class="text-3xl font-extrabold mb-8 text-gray-900">DEPOSIT ASSETS</h2>
        <select id="dep-method" class="prime-input" onchange="updateAddr()">
            <option value="Binance">USDT - TRC20 (Most Secure)</option>
            <option value="Local">Local Payment Distributor</option>
        </select>
        <div class="prime-card p-6 mb-8 text-center bg-gray-50 border-dashed border-gray-300">
            <p id="dep-addr" class="text-[11px] font-mono break-all text-gray-500">0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37</p>
        </div>
        <input type="number" id="dep-amt" placeholder="Deposit Amount ($)" class="prime-input">
        <input type="text" id="dep-tid" placeholder="Transaction Reference (TID)" class="prime-input">
        <input type="file" id="dep-proof" accept="image/*" class="text-[11px] mb-8">
        <button onclick="requestFin('Deposit')" class="w-full btn-gold py-5">Verify & Fund Account</button>
    </div>

    <div id="wd-modal" class="modal">
        <div onclick="closeModal('wd-modal')" class="mb-10 text-gray-400 font-bold text-[11px] uppercase"><i class="fa-solid fa-chevron-left mr-2"></i>Exit Extraction</div>
        <h2 class="text-3xl font-extrabold mb-8 text-red-600">WITHDRAWAL</h2>
        <input type="number" id="wd-amt" placeholder="Amount ($)" class="prime-input">
        <input type="text" id="wd-acc" placeholder="Target Account Title" class="prime-input">
        <button onclick="requestFin('Withdrawal')" class="w-full bg-gray-900 text-white font-bold py-5 rounded-xl uppercase text-[11px]">Authorize Payout</button>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-white z-[9999] p-8 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 pb-4 border-b">
            <h2 class="text-gray-900 font-extrabold uppercase text-sm">PRIME ADMIN HUB</h2>
            <button onclick="location.reload()" class="text-red-500 font-bold text-[10px]">DISCONNECT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = localStorage.getItem('prime_user'); let isLogin = true;

        if(activeUser) start(activeUser);

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Required: Login Credentials");
            const uRef = doc(db, "users", id); const s = await getDoc(uRef);
            if(!isLogin) {
                if(s.exists()) return alert("Identity already registered");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, ref: "PRIME-" + Math.floor(1000+Math.random()*9000) });
                localStorage.setItem('prime_user', id); start(id);
            } else if(s.exists() && s.data().password === pw) {
                localStorage.setItem('prime_user', id); start(id);
            } else alert("Invalid Authorization");
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').style.display='none'; document.getElementById('app').classList.remove('hidden');
            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "Live Yield: +$" + (data.daily || 0).toFixed(2) + " / day";
                document.getElementById('ref-id').innerText = data.ref;
                document.getElementById('user-display').innerText = id;
            });
            loadHistory(); renderNodes();
        }

        window.logout = () => { localStorage.removeItem('prime_user'); location.reload(); };

        window.requestFin = async (type) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            let payload = { user: activeUser, type, status: 'Pending', time: Date.now() };
            if(type === 'Deposit') {
                payload.amount = parseFloat(document.getElementById('dep-amt').value);
                payload.tid = document.getElementById('dep-tid').value;
                const file = document.getElementById('dep-proof').files[0];
                if(file) {
                    const r = new FileReader(); r.onloadend = async () => { payload.proof = r.result; await addDoc(collection(db, "logs"), payload); alert("Funding Request Sent"); closeModal('dep-modal'); }; r.readAsDataURL(file);
                } else { await addDoc(collection(db, "logs"), payload); alert("Funding Sent"); closeModal('dep-modal'); }
            } else {
                payload.amount = parseFloat(document.getElementById('wd-amt').value);
                payload.acc = document.getElementById('wd-acc').value;
                if(s.data().balance < payload.amount) return alert("Liquidity Low");
                await updateDoc(uRef, { balance: s.data().balance - payload.amount });
                await addDoc(collection(db, "logs"), payload); alert("Extraction in Progress"); closeModal('wd-modal');
            }
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser), orderBy("time", "desc")), s => {
                const h = document.getElementById('hist-container'); h.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    h.innerHTML += `<div class="prime-card p-5 flex justify-between items-center bg-white">
                        <div><p class="text-[9px] font-bold uppercase text-gray-400">${r.type}</p><p class="text-sm font-bold text-gray-900">$${r.amount}</p></div>
                        <span class="text-[9px] font-bold px-3 py-1 rounded-full ${r.status=='Pending'?'bg-yellow-50 text-yellow-600':'bg-green-50 text-green-600'}">${r.status.toUpperCase()}</span>
                    </div>`;
                });
            });
        }

        function renderNodes() {
            const c = document.getElementById('nodes-container'); c.innerHTML = "";
            const imgs = ["https://images.unsplash.com/photo-1614064641938-3bbee52942c7?q=80&w=400", "https://images.unsplash.com/photo-1620712943543-bcc4628c71d0?q=80&w=400"];
            for(let i=1; i<=12; i++) {
                let p = i*60; let y = (p*0.1).toFixed(2);
                c.innerHTML += `<div class="prime-card overflow-hidden">
                    <img src="${imgs[i%2]}" class="w-full h-32 object-cover grayscale hover:grayscale-0 transition-all">
                    <div class="p-6">
                        <div class="flex justify-between items-center mb-4"><h4 class="font-extrabold text-sm text-gray-800">PRIME CLUSTER v.${i}</h4><span class="machine-badge">$${p}</span></div>
                        <div class="flex justify-between items-end">
                            <div><p class="text-[9px] text-gray-400 font-bold uppercase">Daily Profit</p><p class="text-lg font-extrabold text-green-600">+$${y}</p></div>
                            <button onclick="buyNode(${p},${y})" class="btn-gold px-8 py-3">Purchase</button>
                        </div>
                    </div>
                </div>`;
            }
        }

        window.buyNode = async (p, d) => {
            const uRef = doc(db, "users", activeUser); const s = await getDoc(uRef);
            if(s.data().balance < p) return alert("Authorization Failed: Low Funds");
            await updateDoc(uRef, { balance: s.data().balance - p, daily: (s.data().daily || 0) + parseFloat(d) });
            alert("Hardware Activated Successfully");
        };

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="prime-card p-5 border-l-4 ${r.type=='Deposit'?'border-green-500':'border-red-500'} bg-gray-50">
                        <div class="flex justify-between mb-4"><span class="text-xs font-bold text-gray-500">${r.user}</span><span class="text-xl font-bold">$${r.amount}</span></div>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 text-[10px] block mb-4 underline font-bold uppercase">View Proof</button>` : ''}
                        <div class="flex gap-2"><button onclick="action('${d.id}','${r.user}',${r.amount},'${r.type}','Success')" class="bg-green-600 text-white flex-1 py-3 rounded-xl text-[9px] font-bold">APPROVE</button></div>
                    </div>`;
                });
            });
        }

        window.action = async (id, u, a, t, s) => { if(t === 'Deposit') { const uR = doc(db, "users", u); const us = await getDoc(uR); await updateDoc(uR, { balance: (us.data().balance || 0) + a }); } await updateDoc(doc(db, "logs", id), { status: s }); };
        window.applyPromo = async () => { if(document.getElementById('promo-inp').value === "PRIME100") { const uR = doc(db, "users", activeUser); const s = await getDoc(uR); await updateDoc(uR, { balance: s.data().balance + 100 }); alert("Coupon Success!"); } };
        window.handleTaps = () => { window.taps = (window.taps || 0) + 1; if(window.taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register Professional Account" : "Return to Login"; };
        window.openModal = (id) => document.getElementById(id).style.display = 'block';
        window.closeModal = (id) => document.getElementById(id).style.display = 'none';
        window.nav = (id) => { document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen')); document.getElementById(id).classList.add('active-screen'); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active')); document.getElementById('nav-' + id.split('-')[0]).classList.add('active'); };
    </script>
</body>
</html>
