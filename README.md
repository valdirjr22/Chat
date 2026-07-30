<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>What Chat</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { font-family: 'Inter', sans-serif; }
        
        .bg-neon-orange { background-color: #ff6b00; }
        .bg-neon-orange:hover { background-color: #e05e00; }
        .text-neon-orange { color: #ff6b00; }
        .shadow-neon { box-shadow: 0 0 15px rgba(255, 107, 0, 0.4); }
        .shadow-blue-glow { box-shadow: 0 0 15px rgba(14, 165, 233, 0.3); }

        .tab-btn.active { 
            color: #ff6b00; 
            border-bottom: 3px solid #ff6b00; 
            font-weight: 700;
        }
        .hidden { display: none !important; }
        
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #38bdf8; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #0284c7; }
    </style>
</head>
<body class="bg-slate-900 h-screen w-screen flex items-center justify-center p-0 md:p-4 overflow-hidden">

    <div id="auth-screen" class="bg-slate-800 border border-slate-700 rounded-2xl shadow-blue-glow w-full max-w-md p-8 z-50 text-white">
        <div class="text-center mb-6">
            <div class="w-16 h-16 bg-gradient-to-tr from-sky-500 to-orange-500 rounded-2xl flex items-center justify-center mx-auto mb-3 shadow-neon">
                <i class="fas fa-bolt text-white text-3xl"></i>
            </div>
            <h2 class="text-3xl font-extrabold bg-gradient-to-r from-sky-400 to-orange-500 bg-clip-text text-transparent">What Chat</h2>
            <p class="text-xs text-slate-400 mt-1">Cadastre-se ou entre com seu número de celular</p>
        </div>

        <div id="phone-step" class="space-y-4">
            <div>
                <label class="block text-xs font-semibold text-slate-300 mb-1">Seu Nome</label>
                <div class="relative">
                    <i class="fas fa-user absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="text" id="user-name-input" placeholder="Ex: Lucas Silva" class="w-full pl-9 pr-3 py-2.5 bg-slate-900 border border-slate-700 rounded-lg text-sm text-white focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-300 mb-1">Número de Celular (ex: +5581986422797)</label>
                <div class="relative">
                    <i class="fas fa-phone absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="tel" id="phone-input" placeholder="+5581986422797" class="w-full pl-9 pr-3 py-2.5 bg-slate-900 border border-slate-700 rounded-lg text-sm text-white focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <div id="recaptcha-container" class="my-2"></div>
            <button onclick="sendSMS()" class="w-full bg-neon-orange hover:bg-orange-600 text-white font-bold py-3 rounded-lg transition shadow-neon flex items-center justify-center gap-2">
                <span>Enviar Código SMS</span> <i class="fas fa-paper-plane text-xs"></i>
            </button>
        </div>

        <div id="code-step" class="space-y-4 hidden">
            <p class="text-xs text-center text-slate-300">Insira o código de 6 dígitos enviado por SMS</p>
            <div>
                <input type="text" id="code-input" maxlength="6" placeholder="000000" class="w-full py-3 bg-slate-900 border border-orange-500 rounded-lg text-center tracking-widest text-2xl font-bold text-orange-400 focus:outline-none">
            </div>
            <button onclick="verifySMSCode()" class="w-full bg-sky-600 hover:bg-sky-500 text-white font-bold py-3 rounded-lg transition shadow-blue-glow">
                Confirmar e Entrar
            </button>
        </div>
    </div>

    <div id="chat-screen" class="bg-slate-900 w-full h-full max-w-[1600px] max-h-[920px] rounded-none md:rounded-2xl shadow-2xl flex overflow-hidden hidden border border-slate-800">

        <div class="w-full md:w-[380px] lg:w-[420px] bg-slate-900 border-r border-slate-800 flex flex-col h-full flex-shrink-0">
            
            <div class="bg-slate-800 p-3.5 flex justify-between items-center border-b border-slate-700">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-gradient-to-tr from-sky-500 to-orange-500 rounded-xl flex items-center justify-center text-white font-bold shadow-neon">
                        <span id="user-avatar-initial">W</span>
                    </div>
                    <div>
                        <h3 id="my-profile-name" class="font-bold text-white text-sm">Meu Nome</h3>
                        <p id="my-profile-phone" class="text-[11px] text-orange-400">+55...</p>
                    </div>
                </div>
                <div class="flex items-center gap-3 text-slate-300 text-lg">
                    <button title="Adicionar Contato" onclick="toggleAddContactModal()" class="hover:text-orange-400 transition"><i class="fas fa-user-plus"></i></button>
                    <button title="Sair" onclick="logout()" class="hover:text-red-400 transition"><i class="fas fa-power-off"></i></button>
                </div>
            </div>

            <div class="flex border-b border-slate-800 bg-slate-900 text-xs font-semibold text-slate-400">
                <button onclick="switchTab('chats')" id="tab-btn-chats" class="tab-btn active flex-1 py-3 text-center flex items-center justify-center gap-2">
                    <i class="fas fa-comments"></i> Conversas
                </button>
                <button onclick="switchTab('status')" id="tab-btn-status" class="tab-btn flex-1 py-3 text-center flex items-center justify-center gap-2">
                    <i class="fas fa-bolt"></i> Status
                </button>
            </div>

            <div id="tab-content-chats" class="flex-grow overflow-y-auto bg-slate-900 divide-y divide-slate-800">
                <div id="contacts-list"></div>
            </div>

            <div id="tab-content-status" class="p-4 hidden text-slate-400 text-xs text-center">
                <p>Nenhuma atualização recente.</p>
            </div>
        </div>

        <div class="hidden md:flex flex-col flex-grow bg-slate-950 relative">

            <div id="welcome-view" class="flex flex-col items-center justify-center h-full text-center p-8">
                <div class="w-28 h-28 bg-gradient-to-tr from-sky-500 to-orange-500 text-white rounded-3xl flex items-center justify-center text-5xl mb-6 shadow-neon">
                    <i class="fas fa-bolt"></i>
                </div>
                <h1 class="text-3xl font-extrabold bg-gradient-to-r from-sky-400 to-orange-500 bg-clip-text text-transparent mb-2">What Chat Web</h1>
                <p class="text-xs text-slate-400 max-w-md">
                    Selecione um contato ou adicione um novo número para começar a trocar mensagens em tempo real.
                </p>
            </div>

            <div id="active-chat-view" class="flex-col h-full hidden">
                <div class="p-3.5 bg-slate-900 border-b border-slate-800 flex justify-between items-center">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 bg-sky-600 rounded-xl flex items-center justify-center text-white font-bold shadow-blue-glow">
                            <span id="active-contact-avatar">C</span>
                        </div>
                        <div>
                            <h3 id="active-contact-name" class="font-bold text-white text-sm">Contato</h3>
                            <p id="active-contact-phone" class="text-[10px] text-orange-400">+55...</p>
                        </div>
                    </div>
                </div>

                <div id="messages-container" class="flex-grow p-6 overflow-y-auto space-y-3 bg-slate-950 flex flex-col"></div>

                <form onsubmit="sendMessage(event)" class="p-3 bg-slate-900 border-t border-slate-800 flex items-center gap-3">
                    <input type="text" id="message-input" placeholder="Digite sua mensagem..." class="flex-grow py-2.5 px-4 bg-slate-800 border border-slate-700 rounded-xl text-xs text-white focus:outline-none focus:border-orange-500">
                    <button type="submit" class="bg-neon-orange hover:bg-orange-600 text-white w-10 h-10 rounded-xl flex items-center justify-center shadow-neon transition">
                        <i class="fas fa-paper-plane text-sm"></i>
                    </button>
                </form>
            </div>

        </div>

    </div>

    <div id="add-contact-modal" class="fixed inset-0 bg-black/70 backdrop-blur-sm flex items-center justify-center hidden z-50 p-4">
        <div class="bg-slate-800 border border-slate-700 rounded-2xl shadow-neon w-full max-w-sm p-6 space-y-4 text-white">
            <h3 class="text-lg font-bold text-white flex items-center gap-2">
                <i class="fas fa-user-plus text-orange-400"></i> Novo Contato
            </h3>
            <p class="text-xs text-slate-400">Digite o telefone exatamente como está no banco (ex: +5581986422797).</p>
            <div>
                <label class="block text-xs font-semibold text-slate-300 mb-1">Telefone</label>
                <input type="tel" id="new-contact-phone" placeholder="+5581986422797" class="w-full p-2.5 bg-slate-900 border border-slate-700 rounded-xl text-sm text-white focus:outline-none focus:border-orange-500">
            </div>
            <div class="flex justify-end gap-2 pt-2">
                <button onclick="toggleAddContactModal()" class="px-4 py-2 text-xs font-semibold text-slate-400 hover:text-white rounded-lg">Cancelar</button>
                <button onclick="addContact()" class="px-4 py-2 text-xs font-bold bg-neon-orange text-white rounded-lg shadow-neon hover:bg-orange-600">Buscar e Adicionar</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, RecaptchaVerifier, signInWithPhoneNumber, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, query, orderBy, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // Cole aqui suas credenciais do Firebase Console
        const firebaseConfig = {
            apiKey: "SUA_API_KEY_AQUI",
            authDomain: "SEU_PROJETO.firebaseapp.com",
            projectId: "SEU_PROJETO_ID",
            storageBucket: "SEU_PROJETO.appspot.com",
            messagingSenderId: "123456789",
            appId: "1:123456789:web:abcdef"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        let confirmationResult = null;
        let currentUserData = null;
        let activeContactPhone = null;
        let unsubscribeMessages = null;

        window.onload = () => {
            window.recaptchaVerifier = new RecaptchaVerifier(auth, 'recaptcha-container', { 'size': 'invisible' });
        };

        // Enviar SMS de autenticação
        window.sendSMS = async () => {
            const name = document.getElementById('user-name-input').value.trim();
            const phone = document.getElementById('phone-input').value.trim();

            if (!name || !phone) return alert("Preencha seu nome e telefone (+55...).");

            try {
                confirmationResult = await signInWithPhoneNumber(auth, phone, window.recaptchaVerifier);
                sessionStorage.setItem('pending_name', name);
                document.getElementById('phone-step').classList.add('hidden');
                document.getElementById('code-step').classList.remove('hidden');
            } catch (err) {
                console.error(err);
                alert("Erro ao enviar SMS: " + err.message);
            }
        };

        // Validar código SMS e gravar exatamente como na coleção 'Users' da sua foto
        window.verifySMSCode = async () => {
            const code = document.getElementById('code-input').value.trim();
            try {
                const res = await confirmationResult.confirm(code);
                const name = sessionStorage.getItem('pending_name') || "Usuário";

                // Salva na coleção 'Users' exatamente com a estrutura id, name, phone
                await setDoc(doc(db, "Users", res.user.phoneNumber), {
                    id: res.user.phoneNumber,
                    name: name,
                    phone: res.user.phoneNumber
                }, { merge: true });

            } catch (err) {
                console.error(err);
                alert("Código SMS incorreto!");
            }
        };

        // Escuta o login do usuário
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                // Busca na coleção 'Users'
                const userDoc = await getDoc(doc(db, "Users", user.phoneNumber));
                currentUserData = userDoc.exists() ? userDoc.data() : { phone: user.phoneNumber, name: "Usuário" };

                document.getElementById('my-profile-name').textContent = currentUserData.name;
                document.getElementById('my-profile-phone').textContent = currentUserData.phone;
                document.getElementById('user-avatar-initial').textContent = currentUserData.name.charAt(0).toUpperCase();

                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('chat-screen').classList.remove('hidden');
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
                document.getElementById('chat-screen').classList.add('hidden');
            }
        });

        // Adiciona contato buscando na coleção 'Users'
        window.addContact = async () => {
            const phone = document.getElementById('new-contact-phone').value.trim();
            if (!phone || phone === currentUserData.phone) return alert("Insira um número válido e diferente do seu.");

            const userDoc = await getDoc(doc(db, "Users", phone));
            if (!userDoc.exists()) return alert("Telefone não encontrado no banco de dados!");

            renderContactItem(userDoc.data());
            window.toggleAddContactModal();
        };

        function renderContactItem(user) {
            const list = document.getElementById('contacts-list');
            const item = document.createElement('div');
            item.className = "p-3 hover:bg-slate-800 cursor-pointer flex items-center gap-3 transition border-b border-slate-800/50";
            item.onclick = () => selectContact(user);
            item.innerHTML = `
                <div class="w-11 h-11 bg-sky-600 rounded-xl flex items-center justify-center text-white font-bold text-sm shadow-blue-glow">
                    ${user.name.charAt(0).toUpperCase()}
                </div>
                <div class="flex-grow">
                    <h4 class="font-semibold text-white text-sm">${user.name}</h4>
                    <p class="text-xs text-orange-400 truncate">${user.phone}</p>
                </div>
            `;
            list.appendChild(item);
        }

        // Seleciona a conversa
        function selectContact(user) {
            activeContactPhone = user.phone;
            document.getElementById('welcome-view').classList.add('hidden');
            document.getElementById('active-chat-view').classList.remove('hidden');
            document.getElementById('active-chat-view').classList.add('flex');

            document.getElementById('active-contact-name').textContent = user.name;
            document.getElementById('active-contact-phone').textContent = user.phone;
            document.getElementById('active-contact-avatar').textContent = user.name.charAt(0).toUpperCase();

            listenToMessages();
        }

        // Carrega as mensagens do Firestore
        function listenToMessages() {
            if (unsubscribeMessages) unsubscribeMessages();

            const chatId = [currentUserData.phone, activeContactPhone].sort().join('_');
            const q = query(collection(db, "chats", chatId, "messages"), orderBy("timestamp", "asc"));

            const container = document.getElementById('messages-container');
            container.innerHTML = '';

            unsubscribeMessages = onSnapshot(q, (snapshot) => {
                container.innerHTML = '';
                snapshot.forEach((doc) => {
                    const msg = doc.data();
                    const isSent = msg.sender === currentUserData.phone;

                    const bubble = document.createElement('div');
                    bubble.className = `max-w-[65%] p-3 rounded-2xl text-xs ${
                        isSent 
                            ? 'bg-sky-600 text-white self-end rounded-tr-none shadow-blue-glow' 
                            : 'bg-slate-800 text-slate-100 border border-slate-700 self-start rounded-tl-none'
                    }`;
                    bubble.innerHTML = `
                        <div>${msg.text}</div>
                        <div class="text-[9px] ${isSent ? 'text-sky-200' : 'text-slate-400'} text-right mt-1">
                            ${msg.timestamp ? new Date(msg.timestamp.seconds * 1000).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}) : '...'}
                        </div>
                    `;
                    container.appendChild(bubble);
                });
                container.scrollTop = container.scrollHeight;
            });
        }

        // Enviar mensagem
        window.sendMessage = async (e) => {
            e.preventDefault();
            const input = document.getElementById('message-input');
            const text = input.value.trim();
            if (!text || !activeContactPhone) return;

            const chatId = [currentUserData.phone, activeContactPhone].sort().join('_');
            await addDoc(collection(db, "chats", chatId, "messages"), {
                text: text,
                sender: currentUserData.phone,
                timestamp: new Date()
            });

            input.value = '';
        };

        window.toggleAddContactModal = () => {
            document.getElementById('add-contact-modal').classList.toggle('hidden');
        };

        window.switchTab = (tabName) => {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(`tab-btn-${tabName}`).classList.add('active');

            document.getElementById('tab-content-chats').classList.add('hidden');
            document.getElementById('tab-content-status').classList.add('hidden');

            document.getElementById(`tab-content-${tabName}`).classList.remove('hidden');
        };

        window.logout = () => {
            signOut(auth);
            location.reload();
        };
    </script>
</body>
</html>
