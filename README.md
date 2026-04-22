<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Professional Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { --gold: #D4AF37; --bg: #050505; }
        body { background: var(--bg); color: white; font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        
        .app-card { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.05); border-radius: 24px; }
        .gold-glow { box-shadow: 0 0 20px rgba(212, 175, 55, 0.1); border: 1px solid rgba(212, 175, 55, 0.2); }
        
        .nav-bar { position: fixed; bottom: 0; left: 0; right: 0; background: rgba(10, 10, 10, 0.95); backdrop-filter: blur(20px); border-top: 1px solid #1a1a1a; padding: 15px 10px; z-index: 1000; display: flex; justify-content: space-around; }
        .nav-item { display: flex; flex-direction: column; align-items: center; gap: 5px; color: #555; font-size: 10px; font-weight: 700; }
        .nav-item.active { color: var(--gold); }

        .icon-btn { display: flex; flex-direction: column; align-items: center; gap: 8px; flex: 1; }
        .icon-circle { width: 55px; height: 55px; background: rgba(255, 255, 255, 0.03); border-radius: 18px; display: flex; align-items: center; justify-content: center; font-size: 20px; border: 1px solid rgba(255, 255, 255, 0.05); }
        .icon-circle:active { transform: scale(0.9); background: var(--gold); color: black; }

        .screen { display: none; }
        .active-screen { display: block; animation: slideUp 0.4s ease-out; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        .nova-input { background: #0f0f0f; border: 1px solid #1a1a1a; width: 100%; padding: 16px; border-radius: 16px; outline: none; color: white; font-size: 14px; }
        .nova-input:focus { border-color: var(--gold); }
        
        .modal { display: none; position: fixed; inset: 0; background: black; z-index: 2000; padding: 24px; padding-top: 48px; overflow-y: auto; }
    </style>
</head>
<body>

    <div id="auth-screen" class="min-h-screen flex flex-col items-center justify-center p-8 active-screen">
        <h1 onclick="handleTaps()" class="text-5xl font-black tracking-tighter mb-10">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <div class="app-card p-6 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Terminal ID" class="nova-input mb-3">
            <input type="password" id="u-pass" placeholder="Passkey" class="nova-input mb-3">
            <input type="text" id="adm-key" placeholder="Master Key" class="hidden nova-input border-[#D4AF37] mb-3">
            <button onclick="auth()" class="w-full bg-[#D4AF37] text-black py-4 rounded-2xl font-black text-sm tracking-widest uppercase">ACCESS TERMINAL</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-[10px] mt-6 text-gray-500 uppercase tracking-widest cursor-pointer">Register Account</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4"><h2 class="text-[#D4AF37] font-black">COMMAND</h2><button onclick="location.reload()" class="text-red-500 font-bold">CLOSE</button></div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden">
        <header class="p-6 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 app-card flex items-center justify-center text-[#D4AF37] font-black">N</div>
                <p id="user-id-txt" class="text-xs font-bold uppercase tracking-widest text-gray-400">USER</p>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-800"></i>
        </header>

        <div id="dash-screen" class="screen active-screen p-6 pb-32">
            <div class="app-card p-8 mb-8 gold-glow relative overflow-hidden text-center">
                <p class="text-[10px] text-gray-500 font-bold tracking-widest mb-2 uppercase">Portfolio Value</p>
                <h1 id="main-bal" class="text-5xl font-black tracking-tighter mb-4">$0.00</h1>
                <p id="v-daily" class="text-green-500 text-[10px] font-bold">+ $0.00 Daily Yield</p>
            </div>

            <div class="flex justify-between mb-10 gap-2">
                <div onclick="openModal('dep-modal')" class="icon-btn">
                    <div class="icon-circle text-green-500"><i class="fa-solid fa-arrow-down-long"></i></div>
                    <span class="text-[9px] font-bold text-gray-400">DEPOSIT</span>
                </div>
                <div onclick="openModal('wd-modal')" class="icon-btn">
                    <div class="icon-circle text-red-500"><i class="fa-solid fa-arrow-up-long"></i></div>
                    <span class="text-[9px] font-bold text-gray-400">WITHDRAW</span>
                </div>
                <div onclick="openModal('hist-modal')" class="icon-btn">
                    <div class="icon-circle text-[#D4AF37]"><i class="fa-solid fa-clock-rotate-left"></i></div>
                    <span class="text-[9px] font-bold text-gray-400">HISTORY</span>
                </div>
                <div onclick="openModal('support-modal')" class="icon-btn">
                    <div class="icon-circle text-blue-500"><i class="fa-solid fa-headset"></i></div>
                    <span class="text-[9px] font-bold text-gray-400">HELP</span>
                </div>
            </div>

            <h3 class="text-[10px] font-black text-gray-600 mb-4 tracking-widest uppercase px-1">Recent Updates</h3>
            <div id="hist-list-mini" class="space-y-3">
                </div>
        </div>

        <div id="mine-screen" class="screen p-6 pb-32">
            <h2 class="text-xl font-black mb-6 gold-text">STAKING PROTOCOLS</h2>
            <div class="space-y-4">
                <div class="app-card p-6 flex justify-between items-center border-l-4 border-[#D4AF37]">
                    <div><p class="text-xs font-bold">CORE NODE</p><p class="text-xl font-black">$30.00</p></div>
                    <button onclick="buyNode(30, 1.5)" class="bg-white text-black px-6 py-2 rounded-xl text-[10px] font-black">STAKE</button>
                </div>
                <div class="app-card p-6 flex justify-between items-center border-l-4 border-[#D4AF37]">
                    <div><p class="text-xs font-bold">ULTRA NODE</p><p class="text-xl font-black">$100.00</p></div>
                    <button onclick="buyNode(100, 6.0)" class="bg-white text-black px-6 py-2 rounded-xl text-[10px] font-black">STAKE</button>
                </div>
            </div>
        </div>

        <div id="office-screen" class="screen p-6 pb-32">
            <div class="app-card p-8 mb-6 text-center">
                <i class="fa-solid fa-share-nodes text-3xl text-[#D4AF37] mb-4"></i>
                <p class="text-xs font-bold text-gray-500 mb-2">My Referral ID</p>
                <p id="ref-link" class="text-[10px] font-mono text-gray-300 bg-black/50 p-4 rounded-xl border border-white/5 select-all"></p>
            </div>
        </div>

        <div id="dep-modal" class="modal">
            <div onclick="closeModal('dep-modal')" class="mb-8 text-gray-500"><i class="fa-solid fa-chevron-left mr-2"></i> BACK</div>
            <h2 class="text-2xl font-black mb-8 gold-text">DEPOSIT</h2>
            <div class="space-y-3 mb-6">
                <div class="app-card p-4 flex justify-between items-center">
                    <span class="text-xs font-bold">TRUST (BEP20)</span>
                    <button onclick="copyAddr('0xb584fBe3d7f1A72AEb2A0a76b1bC959841eC0A37',this)" class="text-[10px] font-black text-[#D4AF37]">COPY</button>
                </div>
                <div class="app-card p-4 flex justify-between items-center">
                    <span class="text-xs font-bold">META (ERC20)</span>
                    <button onclick="copyAddr('0xc270914e5a9e72C5994fb4bd9BbdD9A3425303f2',this)" class="text-[10px] font-black text-[#D4AF37]">COPY</button>
                </div>
            </div>
            <input type="number" id="dep-amt" placeholder="Amount ($)" class="nova-input mb-3">
            <input type="text" id="dep-tid" placeholder="Transaction ID (TID)" class="nova-input mb-3">
            <input type="file" id="proof-img" class="hidden" onchange="processImg(this)">
            <button onclick="document.getElementById('proof-img').click()" class="w-full py-4 border border-dashed border-gray-800 rounded-2xl text-[10px] text-gray-500 mb-4 font-bold uppercase">Attach Screenshot</button>
            <button onclick="requestFin('Deposit')" class="w-full bg-[#D4AF37] text-black py-4 rounded-2xl font-black text-xs">SUBMIT</button>
        </div>

        <div id="wd-modal" class="modal">
            <div onclick="closeModal('wd-modal')" class="mb-8 text-gray-500"><i class="fa-solid fa-chevron-left mr-2"></i> BACK</div>
            <h2 class="text-2xl font-black mb-8 text-red-500">WITHDRAWAL</h2>
            <input type="number" id="wd-amt" placeholder="Amount ($)" class="nova-input mb-3">
            <select id="wd-method" class="nova-input mb-3 bg-black text-gray-400">
                <option value="Trust Wallet">Trust Wallet (BEP20)</option>
                <option value="MetaMask">MetaMask (ERC20)</option>
                <option value="Binance">Binance (TRC20)</option>
            </select>
            <input type="text" id="wd-addr" placeholder="Your Wallet Address" class="nova-input mb-6">
            <button onclick="requestFin('Withdrawal')" class="w-full bg-white text-black py-4 rounded-2xl font-black text-xs uppercase tracking-widest">Withdraw Now</button>
        </div>

        <div id="hist-modal" class="modal">
            <div onclick="closeModal('hist-modal')" class="mb-8 text-gray-500"><i class="fa-solid fa-chevron-left mr-2"></i> BACK</div>
            <h2 class="text-2xl font-black mb-8 gold-text">HISTORY</h2>
            <div id="hist-list-full" class="space-y-4"></div>
        </div>

        <div id="support-modal" class="modal">
            <div onclick="closeModal('support-modal')" class="mb-8 text-gray-500"><i class="fa-solid fa-chevron-left mr-2"></i> BACK</div>
            <h2 class="text-2xl font-black mb-8 text-blue-500">SUPPORT</h2>
            <div class="app-card p-6 text-center">
                <i class="fa-brands fa-whatsapp text-4xl text-green-500 mb-4"></i>
                <p class="text-sm font-bold mb-6">Contact our global desk for instant assistance.</p>
                <button class="w-full bg-green-600 text-white py-4 rounded-2xl font-black text-xs uppercase">Open WhatsApp</button>
            </div>
        </div>

        <nav class="nav-bar">
            <div onclick="nav('dash-screen')" class="nav-item active" id="nav-dash"><i class="fa-solid fa-house-user text-xl"></i><span>HOME</span></div>
            <div onclick="nav('mine-screen')" class="nav-item" id="nav-mine"><i class="fa-solid fa-gem text-xl"></i><span>MINING</span></div>
            <div onclick="nav('office-screen')" class="nav-item" id="nav-office"><i class="fa-solid fa-circle-user text-xl"></i><span>OFFICE</span></div>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = { apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo", authDomain: "home-94d45.firebaseapp.com", projectId: "home-94d45", appId: "1:964390949419:web:589840cb91b7e42ecd506e" };
        const app = initializeApp(firebaseConfig); const db = getFirestore(app);
        let activeUser = null; let base64Img = ""; window.taps = 0; window.isLogin = true;

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "Register Account" : "Access Terminal"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return alert("Fill all fields");
            const uRef = doc(db, "users", id); const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, time: Date.now() }); start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) start(id);
                else alert("Error");
            }
        };

        function start(id) {
            activeUser = id; document.getElementById('auth-screen').classList.remove('active-screen');
            setTimeout(() => { document.getElementById('auth-screen').classList.add('hidden'); document.getElementById('app').classList.remove('hidden'); }, 400);
            document.getElementById('user-id-txt').innerText = id; document.getElementById('ref-link').innerText = window.location.href + "?ref=" + id;
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('main-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+ $" + (d.data().daily || 0).toFixed(2) + " Daily Yield";
            });
            loadHistory();
            setInterval(() => { let el = document.getElementById('main-bal'); let val = parseFloat(el.innerText.replace('$','')); el.innerText = "$" + (val + 0.0001).toFixed(4); }, 3000);
        }

        window.processImg = (el) => { const reader = new FileReader(); reader.onload = () => base64Img = reader.result; reader.readAsDataURL(el.files[0]); };

        window.requestFin = async (type) => {
            const amtId = type === 'Deposit' ? 'dep-amt' : 'wd-amt';
            const amt = parseFloat(document.getElementById(amtId).value);
            const uRef = doc(db, "users", activeUser); const snap = await getDoc(uRef);
            
            if(type === 'Withdrawal') {
                const method = document.getElementById('wd-method').value; const addr = document.getElementById('wd-addr').value;
                if(!amt || !addr || snap.data().balance < amt) return alert("Balance Error");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, method, addr, status: 'Pending', time: Date.now() });
                await updateDoc(uRef, { balance: snap.data().balance - amt });
                closeModal('wd-modal');
            } else {
                const tid = document.getElementById('dep-tid').value;
                if(!amt || !tid) return alert("TID Required");
                await addDoc(collection(db, "logs"), { user: activeUser, amount: amt, type, tid, status: 'Pending', proof: base64Img, time: Date.now() });
                closeModal('dep-modal');
            }
            alert("Sent to Admin"); base64Img = "";
        };

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Liquidity Error");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Active!");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const hFull = document.getElementById('hist-list-full');
                const hMini = document.getElementById('hist-list-mini');
                hFull.innerHTML = ""; hMini.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).forEach((t, i) => {
                    const card = `<div class="app-card p-4 flex justify-between items-center text-[10px]">
                        <div><p class="font-black uppercase">${t.type}</p><p class="text-gray-600">${new Date(t.time).toLocaleDateString()}</p></div>
                        <div class="text-right"><p class="font-black">$${t.amount.toFixed(2)}</p><p class="${t.status=='Pending'?'text-yellow-600':'text-green-600'} font-bold">${t.status}</p></div>
                    </div>`;
                    hFull.innerHTML += card;
                    if(i < 3) hMini.innerHTML += card;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data(); const isWD = r.type === 'Withdrawal';
                    al.innerHTML += `<div class="app-card p-5 border-l-4 ${isWD ? 'border-red-500' : 'border-green-500'}">
                        <div class="flex justify-between mb-2"><span class="text-[10px] font-bold text-gray-400">${r.user}</span><span class="font-black">$${r.amount}</span></div>
                        <div class="text-[8px] bg-black/40 p-2 rounded mb-3">
                            <p>TYPE: ${r.type} | METHOD: ${r.method || 'N/A'}</p>
                            ${isWD ? `<p class="text-orange-400">ADDR: ${r.addr}</p>` : `<p class="text-blue-400">TID: ${r.tid}</p>`}
                        </div>
                        ${!isWD && r.proof ? `<button onclick="window.open('${r.proof}')" class="w-full py-2 mb-2 bg-white/5 rounded text-[8px]">VIEW PROOF</button>` : ''}
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="w-full bg-[#D4AF37] text-black font-black p-3 rounded-lg text-[10px]">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            const uR = doc(db, "users", u); const us = await getDoc(uR);
            if(t === 'Deposit') await updateDoc(uR, { balance: (us.data().balance || 0) + a });
            await updateDoc(doc(db, "logs", id), { status: 'Verified' });
        };

        window.openModal = (id) => { document.getElementById(id).style.display = 'block'; document.body.style.overflow = 'hidden'; };
        window.closeModal = (id) => { document.getElementById(id).style.display = 'none'; document.body.style.overflow = 'auto'; };
        window.copyAddr = (a, b) => { navigator.clipboard.writeText(a); b.innerText = "COPIED"; setTimeout(() => b.innerText = "COPY", 2000); };
        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active-screen'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            setTimeout(() => {
                document.querySelectorAll('.screen').forEach(s => s.style.display = 'none');
                document.getElementById(id).style.display = 'block';
                setTimeout(() => document.getElementById(id).classList.add('active-screen'), 50);
                const map = {'dash-screen':'nav-dash','mine-screen':'nav-mine','office-screen':'nav-office'};
                document.getElementById(map[id]).classList.add('active');
            }, 200);
        };
        window.logout = () => location.reload();
    </script>
</body>
</html>
