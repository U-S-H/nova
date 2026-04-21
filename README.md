<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NOVA TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { background: #050505; color: white; font-family: sans-serif; }
        .glass { background: rgba(20, 20, 20, 0.9); border: 1px solid #333; border-radius: 20px; }
        .btn-gold { background: #D4AF37; color: black; font-weight: bold; }
        .screen { display: none; }
        .active { display: block; }
    </style>
</head>
<body>

    <div id="auth-screen" class="p-10 flex flex-col items-center justify-center min-h-screen">
        <h1 onclick="taps++" id="logo" class="text-5xl font-black mb-10 tracking-tighter">NOVA<span class="text-[#D4AF37]">.</span></h1>
        <div class="glass p-6 w-full max-w-sm">
            <input type="text" id="email" placeholder="Account ID" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3 outline-none">
            <input type="password" id="pass" placeholder="Password" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3 outline-none">
            <input type="text" id="admin-key" placeholder="Admin Key" class="hidden w-full p-4 bg-black border border-[#D4AF37] rounded-xl mb-3 outline-none">
            <button onclick="handleAuth()" class="btn-gold w-full p-4 rounded-xl">INITIALIZE</button>
            <p onclick="isLogin=!isLogin; this.innerText=isLogin?'Need Account?':'Already User?'" class="text-center text-xs mt-4 text-gray-500 cursor-pointer">Need Account?</p>
        </div>
    </div>

    <div id="admin-panel" class="hidden fixed inset-0 bg-black z-50 p-6 overflow-y-auto">
        <div class="flex justify-between mb-6">
            <h2 class="text-[#D4AF37] font-bold">ADMIN OVERRIDE</h2>
            <button onclick="location.reload()" class="bg-red-600 px-4 py-1 rounded">EXIT</button>
        </div>
        <div id="admin-list" class="space-y-4"></div>
    </div>

    <div id="app" class="hidden pb-24">
        <div class="p-6">
            <div class="glass p-6 mb-6">
                <p class="text-xs text-gray-500">LIVE CAPITAL</p>
                <h1 id="user-bal" class="text-4xl font-bold my-2">$0.00</h1>
            </div>

            <div class="glass p-6">
                <h3 class="mb-4 font-bold">FINANCE GATEWAY</h3>
                <input type="number" id="amt" placeholder="Enter Amount" class="w-full p-4 bg-black border border-gray-800 rounded-xl mb-3">
                <div class="flex gap-2">
                    <button onclick="req('Deposit')" class="btn-gold flex-1 p-3 rounded-lg">DEPOSIT</button>
                    <button onclick="req('Withdrawal')" class="bg-gray-800 flex-1 p-3 rounded-lg text-white">WITHDRAW</button>
                </div>
            </div>

            <h4 class="mt-8 mb-4 font-bold text-gray-400">TRANSACTION HISTORY</h4>
            <div id="hist" class="space-y-3"></div>
        </div>

        <nav class="fixed bottom-0 w-full bg-black border-t border-gray-900 flex justify-around p-4">
            <i class="fa-solid fa-chart-line text-[#D4AF37]"></i>
            <i class="fa-solid fa-wallet text-gray-600"></i>
            <i onclick="logout()" class="fa-solid fa-power-off text-red-900"></i>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, addDoc, collection, onSnapshot, query, where, orderBy } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAXlQ1tKTJbcnIXNeww9I3d-ukzD7_mUCo",
            authDomain: "home-94d45.firebaseapp.com",
            projectId: "home-94d45",
            appId: "1:964390949419:web:589840cb91b7e42ecd506e"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        window.taps = 0; window.isLogin = true; let user = null;

        setInterval(() => { if(taps >= 6) document.getElementById('admin-key').classList.remove('hidden'); }, 500);

        window.handleAuth = async () => {
            const id = document.getElementById('email').value.toLowerCase().trim();
            const pw = document.getElementById('pass').value;
            const ak = document.getElementById('admin-key').value;

            if(ak === "nov786") { document.getElementById('admin-panel').classList.remove('hidden'); loadAdmin(); return; }
            if(!id || !pw) return;

            const uRef = doc(db, "users", id);
            const snap = await getDoc(uRef);

            if(!isLogin) {
                if(snap.exists()) return alert("User exists");
                await setDoc(uRef, { balance: 0, password: pw });
                start(id);
            } else {
                if(snap.exists() && snap.data().password === pw) start(id);
                else alert("Failed");
            }
        };

        function start(id) {
            user = id; 
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            
            onSnapshot(doc(db, "users", id), d => {
                document.getElementById('user-bal').innerText = "$" + (d.data().balance || 0).toFixed(2);
            });
            loadHist();
        }

        window.req = async (type) => {
            const a = document.getElementById('amt').value;
            if(!a) return;
            await addDoc(collection(db, "logs"), { user, amount: parseFloat(a), type, status: 'Pending', time: Date.now() });
            alert("Request Sent");
        }

        function loadHist() {
            const q = query(collection(db, "logs"), where("user", "==", user), orderBy("time", "desc"));
            onSnapshot(q, s => {
                const h = document.getElementById('hist'); h.innerHTML = "";
                s.forEach(d => {
                    const t = d.data();
                    h.innerHTML += `<div class="glass p-4 text-xs flex justify-between">
                        <span>${t.type} - $${t.amount}</span>
                        <span class="${t.status=='Pending'?'text-yellow-500':'text-green-500'}">${t.status}</span>
                    </div>`;
                });
            });
        }

        function loadAdmin() {
            const q = query(collection(db, "logs"), where("status", "==", "Pending"));
            onSnapshot(q, s => {
                const al = document.getElementById('admin-list'); al.innerHTML = "";
                s.forEach(d => {
                    const r = d.data();
                    al.innerHTML += `<div class="glass p-4">
                        <p>${r.user} | ${r.type} | $${r.amount}</p>
                        <button onclick="approve('${d.id}','${r.user}',${r.amount},'${r.type}')" class="bg-green-600 p-2 rounded mt-2">APPROVE</button>
                    </div>`;
                });
            });
        }

        window.approve = async (id, u, a, t) => {
            const uR = doc(db, "users", u); const us = await getDoc(uR);
            let nb = t === 'Deposit' ? (us.data().balance + a) : (us.data().balance - a);
            await updateDoc(uR, { balance: nb });
            await updateDoc(doc(db, "logs", id), { status: 'Verified' });
        }

        window.logout = () => location.reload();
    </script>
</body>
</html>
