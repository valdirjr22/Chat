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
        .border-neon-orange { border-color: #ff6b00; }

        .shadow-neon { box-shadow: 0 0 15px rgba(255, 107, 0, 0.2); }
        .shadow-blue-glow { box-shadow: 0 0 15px rgba(14, 165, 233, 0.2); }

        .tab-btn.active {
            color: #ff6b00;
            border-bottom: 3px solid #ff6b00;
            font-weight: 700;
        }

        .hidden { display: none !important; }

        @keyframes vibrate-screen {
            0% { transform: translate(0, 0); }
            20% { transform: translate(-3px, 3px); }
            40% { transform: translate(-3px, -3px); }
            60% { transform: translate(3px, 3px); }
            80% { transform: translate(3px, -3px); }
            100% { transform: translate(0, 0); }
        }

        .vibrate-effect {
            animation: vibrate-screen 0.2s ease-in-out 2;
        }

        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body id="app-body" class="bg-slate-100 h-screen w-screen flex items-center justify-center p-0 md:p-4 overflow-hidden">

    <div id="loading-screen" class="fixed inset-0 bg-white flex items-center justify-center z-[100]">
        <div class="text-slate-600 text-sm flex items-center gap-2">
            <i class="fas fa-circle-notch fa-spin text-orange-500"></i> Carregando...
        </div>
    </div>

    <div id="auth-screen" class="bg-white border border-slate-200 rounded-2xl shadow-xl w-full max-w-md p-8 z-50 text-slate-800">
        <div class="text-center mb-6">
            <div class="w-16 h-16 bg-gradient-to-tr from-sky-500 to-orange-500 rounded-2xl flex items-center justify-center mx-auto mb-3 shadow-neon transform rotate-3">
                <i class="fas fa-bolt text-white text-3xl"></i>
            </div>
            <h2 class="text-3xl font-extrabold tracking-tight bg-gradient-to-r from-sky-500 to-orange-500 bg-clip-text text-transparent">What Chat</h2>
            <p class="text-xs text-slate-500 mt-1">Entre com usuário e senha</p>
        </div>

        <div id="login-step" class="space-y-4">
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Usuário</label>
                <div class="relative">
                    <i class="fas fa-at absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="text" id="login-username-input" placeholder="Ex: lucas.silva" class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Senha</label>
                <div class="relative">
                    <i class="fas fa-lock absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="password" id="login-password-input" placeholder="••••••••" class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <button id="login-btn" onclick="fazerLogin()" class="w-full bg-neon-orange hover:bg-orange-600 text-white font-bold py-3 rounded-lg transition shadow-neon flex items-center justify-center gap-2">
                <span>Entrar</span> <i class="fas fa-arrow-right text-xs"></i>
            </button>
            <button onclick="mostrarCadastro()" class="w-full text-xs text-slate-500 hover:text-orange-500 text-center block transition">Não tem conta? Criar conta</button>
        </div>

        <div id="cadastro-step" class="space-y-4 hidden">
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Seu Nome</label>
                <div class="relative">
                    <i class="fas fa-user absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="text" id="cadastro-nome-input" placeholder="Ex: Lucas Silva" class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Escolha um Usuário</label>
                <div class="relative">
                    <i class="fas fa-at absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="text" id="cadastro-username-input" placeholder="Ex: lucas.silva" class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Foto de Perfil (Arquivo do Dispositivo)</label>
                <input type="file" id="cadastro-avatar-file" accept="image/*" class="w-full text-xs text-slate-500 bg-slate-50 border border-slate-300 rounded-lg p-2 file:mr-3 file:py-1 file:px-3 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-orange-500 file:text-white hover:file:bg-orange-600 cursor-pointer">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Mensagem de Perfil (Opcional)</label>
                <div class="relative">
                    <i class="fas fa-quote-left absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="text" id="cadastro-status-input" placeholder="Ex: Disponível no What Chat!" class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Crie uma Senha</label>
                <div class="relative">
                    <i class="fas fa-lock absolute left-3 top-3.5 text-slate-400 text-sm"></i>
                    <input type="password" id="cadastro-senha-input" placeholder="••••••••" class="w-full pl-9 pr-3 py-2.5 bg-slate-50 border border-slate-300 rounded-lg text-sm text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>
            <button id="cadastro-btn" onclick="criarConta()" class="w-full bg-sky-600 hover:bg-sky-500 text-white font-bold py-3 rounded-lg transition shadow-blue-glow">
                Criar Conta no What Chat
            </button>
            <button onclick="mostrarLogin()" class="w-full text-xs text-slate-500 hover:text-orange-500 text-center block transition">Já tem conta? Entrar</button>
        </div>
    </div>

    <div id="chat-screen" class="bg-white w-full h-full max-w-[1600px] max-h-[920px] rounded-none md:rounded-2xl shadow-xl flex overflow-hidden border border-slate-200 hidden">

        <div class="w-full md:w-[380px] lg:w-[420px] bg-white border-r border-slate-200 flex flex-col h-full flex-shrink-0">
            <div class="bg-slate-50 p-3.5 flex justify-between items-center border-b border-slate-200">
                <div class="flex items-center gap-3 cursor-pointer" onclick="toggleEditProfileModal()">
                    <div id="my-profile-avatar-box" class="w-10 h-10 bg-gradient-to-tr from-sky-500 to-orange-500 rounded-xl flex items-center justify-center text-white font-bold shadow-neon overflow-hidden">
                        <span id="user-avatar-initial">W</span>
                    </div>
                    <div>
                        <h3 id="my-profile-name" class="font-bold text-slate-800 text-sm leading-tight">Meu Nome</h3>
                        <p id="my-profile-status" class="text-[11px] text-slate-500 truncate max-w-[160px]">Disponível</p>
                    </div>
                </div>
                <div class="flex items-center gap-3 text-slate-600 text-lg">
                    <button title="Editar Perfil" onclick="toggleEditProfileModal()" class="hover:text-sky-600 transition"><i class="fas fa-pen"></i></button>
                    <button title="Ativar Notificações" onclick="pedirPermissaoNotificacao()" class="hover:text-yellow-500 transition"><i class="fas fa-bell"></i></button>
                    <button title="Novo Contato" onclick="toggleAddContactModal()" class="hover:text-orange-500 transition"><i class="fas fa-user-plus"></i></button>
                    <button title="Sair" onclick="logout()" class="hover:text-red-500 transition"><i class="fas fa-power-off"></i></button>
                </div>
            </div>

            <div class="flex border-b border-slate-200 bg-white text-xs font-semibold text-slate-500">
                <button onclick="switchTab('chats')" id="tab-btn-chats" class="tab-btn active flex-1 py-3 text-center transition flex items-center justify-center gap-2">
                    <i class="fas fa-comments"></i> Conversas
                </button>
                <button onclick="switchTab('status')" id="tab-btn-status" class="tab-btn flex-1 py-3 text-center transition flex items-center justify-center gap-2">
                    <i class="fas fa-bolt"></i> Status
                </button>
                <button onclick="switchTab('calls')" id="tab-btn-calls" class="tab-btn flex-1 py-3 text-center transition flex items-center justify-center gap-2">
                    <i class="fas fa-phone-alt"></i> Chamadas
                </button>
            </div>

            <div class="p-3 bg-white border-b border-slate-200">
                <div class="relative">
                    <i class="fas fa-search absolute left-3 top-2.5 text-slate-400 text-xs"></i>
                    <input type="text" id="search-input" onkeyup="filterContacts()" placeholder="Buscar no What Chat..." class="w-full pl-9 pr-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs text-slate-800 focus:outline-none focus:border-orange-500">
                </div>
            </div>

            <div class="flex-grow overflow-y-auto bg-white">
                <div id="tab-content-chats" class="divide-y divide-slate-100">
                    <div id="contacts-list"></div>
                    <p id="contacts-empty" class="text-xs text-slate-400 text-center py-6 hidden">Nenhum contato ainda. Clique em <i class="fas fa-user-plus"></i> para adicionar.</p>
                </div>

                <div id="tab-content-status" class="p-4 hidden space-y-4">
                    <div class="flex items-center gap-3 cursor-pointer p-3 bg-slate-50 hover:bg-slate-100 rounded-xl border border-slate-200 transition" onclick="toggleEditProfileModal()">
                        <div class="w-12 h-12 bg-sky-100 text-orange-500 rounded-full flex items-center justify-center font-bold border border-orange-500">
                            <i class="fas fa-plus"></i>
                        </div>
                        <div>
                            <h4 class="font-bold text-sm text-slate-800">Atualizar Perfil & Recado</h4>
                            <p class="text-xs text-slate-500">Edite seu status ou foto no What Chat</p>
                        </div>
                    </div>
                </div>

                <div id="tab-content-calls" class="p-4 hidden">
                    <div class="text-center py-10 text-slate-400">
                        <i class="fas fa-phone-slash text-3xl mb-2 text-slate-300"></i>
                        <p class="text-xs">Nenhuma chamada registrada.</p>
                    </div>
                </div>
            </div>
        </div>

        <div class="flex flex-col flex-grow bg-slate-50 relative">

            <div id="welcome-view" class="flex flex-col items-center justify-center h-full text-center p-8 bg-slate-50">
                <div class="w-28 h-28 bg-gradient-to-tr from-sky-500 to-orange-500 text-white rounded-3xl flex items-center justify-center text-5xl mb-6 shadow-neon">
                    <i class="fas fa-bolt"></i>
                </div>
                <h1 class="text-3xl font-extrabold bg-gradient-to-r from-sky-500 to-orange-500 bg-clip-text text-transparent mb-2">What Chat Web</h1>
                <p class="text-xs text-slate-500 max-w-md leading-relaxed">
                    Selecione um contato na lista ao lado para começar a conversar.
                </p>
            </div>

            <div id="active-chat-view" class="flex-col h-full hidden">
                <div class="p-3.5 bg-white border-b border-slate-200 flex justify-between items-center">
                    <div class="flex items-center gap-3">
                        <div id="active-contact-avatar-box" class="w-10 h-10 bg-sky-600 rounded-xl flex items-center justify-center text-white font-bold shadow-blue-glow overflow-hidden">
                            <span id="active-contact-avatar">C</span>
                        </div>
                        <div>
                            <h3 id="active-contact-name" class="font-bold text-slate-800 text-sm">Contato</h3>
                            <p id="active-contact-status" class="text-[10px] text-orange-500 truncate max-w-[200px]">@usuario</p>
                        </div>
                    </div>

                    <div class="flex items-center gap-4 text-slate-600 text-lg pr-2">
                        <button title="Chamada de Voz" onclick="iniciarChamadaVoz()" class="hover:text-sky-600 transition p-1">
                            <i class="fas fa-phone"></i>
                        </button>
                        <button title="Chamada de Vídeo" onclick="iniciarChamadaVideo()" class="hover:text-orange-500 transition p-1">
                            <i class="fas fa-video"></i>
                        </button>
                    </div>
                </div>

                <div id="messages-container" class="flex-grow p-6 overflow-y-auto space-y-3 bg-slate-50 flex flex-col"></div>

                <form onsubmit="sendMessage(event)" class="p-3 bg-white border-t border-slate-200 flex items-center gap-2">
                    <input type="file" id="media-file-input" accept="image/*,video/*" class="hidden" onchange="enviarArquivoMidia(event)">
                    <input type="file" id="media-camera-input" accept="image/*,video/*" capture="environment" class="hidden" onchange="enviarArquivoMidia(event)">

                    <button type="button" title="Anexar Foto/Vídeo do Equipamento" onclick="document.getElementById('media-file-input').click()" class="text-slate-500 hover:text-sky-600 p-2 transition text-base">
                        <i class="fas fa-paperclip"></i>
                    </button>
                    <button type="button" title="Tirar Foto ou Gravar Vídeo" onclick="document.getElementById('media-camera-input').click()" class="text-slate-500 hover:text-orange-500 p-2 transition text-base">
                        <i class="fas fa-camera"></i>
                    </button>

                    <input type="text" id="message-input" placeholder="Digite sua mensagem..." class="flex-grow py-2.5 px-4 bg-slate-100 border border-slate-200 rounded-xl text-xs text-slate-800 focus:outline-none focus:border-orange-500">
                    
                    <button type="submit" id="send-btn" class="bg-neon-orange hover:bg-orange-600 text-white w-10 h-10 rounded-xl flex items-center justify-center shadow-neon transition flex-shrink-0">
                        <i class="fas fa-paper-plane text-sm"></i>
                    </button>
                </form>
            </div>

        </div>

    </div>

    <div id="edit-profile-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm flex items-center justify-center hidden z-50 p-4">
        <div class="bg-white border border-slate-200 rounded-2xl shadow-xl w-full max-w-sm p-6 space-y-4 text-slate-800">
            <h3 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                <i class="fas fa-user-edit text-orange-500"></i> Editar Meu Perfil
            </h3>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Nome</label>
                <input type="text" id="edit-name-input" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm text-slate-800 focus:outline-none focus:border-orange-500">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Escolher Nova Foto do Equipamento</label>
                <input type="file" id="edit-avatar-file" accept="image/*" class="w-full text-xs text-slate-500 bg-slate-50 border border-slate-300 rounded-xl p-2 file:mr-3 file:py-1 file:px-3 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-orange-500 file:text-white hover:file:bg-orange-600 cursor-pointer">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Recado / Mensagem de Perfil</label>
                <input type="text" id="edit-status-input" placeholder="Ex: Disponível no What Chat!" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm text-slate-800 focus:outline-none focus:border-orange-500">
            </div>
            <div class="flex justify-end gap-2 pt-2">
                <button onclick="toggleEditProfileModal()" class="px-4 py-2 text-xs font-semibold text-slate-500 hover:text-slate-800 rounded-lg">Cancelar</button>
                <button onclick="salvarPerfil()" class="px-4 py-2 text-xs font-bold bg-neon-orange text-white rounded-lg shadow-neon hover:bg-orange-600">Salvar</button>
            </div>
        </div>
    </div>

    <div id="add-contact-modal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-sm flex items-center justify-center hidden z-50 p-4">
        <div class="bg-white border border-slate-200 rounded-2xl shadow-xl w-full max-w-sm p-6 space-y-4 text-slate-800">
            <h3 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                <i class="fas fa-user-plus text-orange-500"></i> Novo Contato
            </h3>
            <div>
                <label class="block text-xs font-semibold text-slate-600 mb-1">Usuário</label>
                <input type="text" id="new-contact-username" placeholder="Ex: victorblox" class="w-full p-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm text-slate-800 focus:outline-none focus:border-orange-500">
            </div>
            <div class="flex justify-end gap-2 pt-2">
                <button onclick="toggleAddContactModal()" class="px-4 py-2 text-xs font-semibold text-slate-500 hover:text-slate-800 rounded-lg">Cancelar</button>
                <button onclick="addContact()" class="px-4 py-2 text-xs font-bold bg-neon-orange text-white rounded-lg shadow-neon hover:bg-orange-600">Adicionar</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
        import { getFirestore, collection, doc, setDoc, getDoc, updateDoc, arrayUnion, addDoc, query, orderBy, onSnapshot, deleteDoc } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAyYH6zCoOZhrg30e076s3trA-0PYi7ARY",
            authDomain: "chatray-95091.firebaseapp.com",
            projectId: "chatray-95091",
            storageBucket: "chatray-95091.firebasestorage.app",
            messagingSenderId: "834696140367",
            appId: "1:834696140367:web:0c31ca1bb2e28c6689fc3a"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        let currentUserData = null;
        let activeContactUsername = null;
        let activeContactName = "Contato";
        let unsubscribeMessages = null;
        let contatosCache = [];
        let isFirstLoad = true;

        function normalizarUsuario(u) {
            return (u || '').trim().toLowerCase().replace(/\s+/g, '');
        }

        function fileToBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.onerror = error => reject(error);
                reader.readAsDataURL(file);
            });
        }

        window.apagarMensagem = async (msgId) => {
            if (!confirm("Tem certeza que deseja apagar esta mensagem?")) return;

            try {
                const chatId = [currentUserData.username, activeContactUsername].sort().join('_');
                await deleteDoc(doc(db, "chats", chatId, "messages", msgId));
            } catch (err) {
                console.error(err);
                alert("Erro ao apagar mensagem.");
            }
        };

        window.enviarArquivoMidia = async (e) => {
            const file = e.target.files[0];
            if (!file || !activeContactUsername) return;

            try {
                const base64Data = await fileToBase64(file);
                const isVideo = file.type.startsWith('video/');
                const isImage = file.type.startsWith('image/');
                const fileName = file.name || (isVideo ? 'video.mp4' : 'imagem.png');

                const chatId = [currentUserData.username, activeContactUsername].sort().join('_');
                
                await addDoc(collection(db, "chats", chatId, "messages"), {
                    text: '',
                    image: isImage ? base64Data : null,
                    video: isVideo ? base64Data : null,
                    fileName: fileName,
                    sender: currentUserData.username,
                    timestamp: new Date()
                });

                e.target.value = '';
            } catch (err) {
                console.error(err);
                alert("Erro ao processar o arquivo de mídia.");
            }
        };

        window.iniciarChamadaVoz = () => {
            if (activeContactName) {
                alert(`Iniciando chamada de voz com ${activeContactName}...`);
            }
        };

        window.iniciarChamadaVideo = () => {
            if (activeContactName) {
                alert(`Iniciando chamada de vídeo com ${activeContactName}...`);
            }
        };

        window.pedirPermissaoNotificacao = () => {
            if ("Notification" in window) {
                Notification.requestPermission().then(permission => {
                    if (permission === "granted") alert("Notificações ativadas com sucesso!");
                });
            } else {
                alert("Seu navegador não suporta notificações.");
            }
        };

        function dispararNotificacaoEVibracao(textoMensagem) {
            if ("vibrate" in navigator) navigator.vibrate([200, 100, 200]);
            
            const body = document.getElementById('app-body');
            body.classList.add('vibrate-effect');
            setTimeout(() => body.classList.remove('vibrate-effect'), 500);

            if ("Notification" in window && Notification.permission === "granted") {
                new Notification(`Nova mensagem de ${activeContactName}`, {
                    body: textoMensagem || "Enviou uma mídia",
                    icon: "https://cdn-icons-png.flaticon.com/512/732/732200.png"
                });
            }
        }

        window.mostrarCadastro = () => {
            document.getElementById('login-step').classList.add('hidden');
            document.getElementById('cadastro-step').classList.remove('hidden');
        };

        window.mostrarLogin = () => {
            document.getElementById('cadastro-step').classList.add('hidden');
            document.getElementById('login-step').classList.remove('hidden');
        };

        window.criarConta = async () => {
            const nome = document.getElementById('cadastro-nome-input').value.trim();
            const username = normalizarUsuario(document.getElementById('cadastro-username-input').value);
            const avatarFile = document.getElementById('cadastro-avatar-file').files[0];
            const statusMsg = document.getElementById('cadastro-status-input').value.trim() || "Disponível no What Chat!";
            const senha = document.getElementById('cadastro-senha-input').value;

            if (!nome || !username || !senha) return alert("Preencha nome, usuário e senha.");

            try {
                const ref = doc(db, "chatUsers", username);
                const existente = await getDoc(ref);
                if (existente.exists()) return alert("Esse usuário já existe.");

                let avatarBase64 = '';
                if (avatarFile) {
                    avatarBase64 = await fileToBase64(avatarFile);
                }

                const newUser = { username, name: nome, avatar: avatarBase64, status: statusMsg, password: senha, contatos: [] };
                await setDoc(ref, newUser);
                entrarComoUsuario(newUser);
            } catch (e) {
                console.error(e);
                alert("Erro ao criar conta.");
            }
        };

        window.fazerLogin = async () => {
            const username = normalizarUsuario(document.getElementById('login-username-input').value);
            const senha = document.getElementById('login-password-input').value;

            if (!username || !senha) return alert("Preencha usuário e senha.");

            try {
                const snap = await getDoc(doc(db, "chatUsers", username));
                if (!snap.exists() || snap.data().password !== senha) {
                    return alert("Usuário ou senha incorretos.");
                }
                entrarComoUsuario(snap.data());
            } catch (e) {
                console.error(e);
                alert("Erro ao fazer login.");
            }
        };

        function entrarComoUsuario(userData) {
            currentUserData = userData;
            localStorage.setItem('whatchat_username', userData.username);

            atualizarUIPerfilProprio();

            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('chat-screen').classList.remove('hidden');

            if ("Notification" in window && Notification.permission !== "granted") {
                Notification.requestPermission();
            }

            carregarContatos();
        }

        function atualizarUIPerfilProprio() {
            document.getElementById('my-profile-name').textContent = currentUserData.name;
            document.getElementById('my-profile-status').textContent = currentUserData.status || "Disponível";

            const avatarBox = document.getElementById('my-profile-avatar-box');
            if (currentUserData.avatar) {
                avatarBox.innerHTML = `<img src="${currentUserData.avatar}" class="w-full h-full object-cover rounded-xl" onerror="this.src=''; this.parentElement.innerHTML='<span id=\'user-avatar-initial\'>${currentUserData.name.charAt(0).toUpperCase()}</span>'">`;
            } else {
                avatarBox.innerHTML = `<span id="user-avatar-initial">${currentUserData.name.charAt(0).toUpperCase()}</span>`;
            }
        }

        window.toggleEditProfileModal = () => {
            const modal = document.getElementById('edit-profile-modal');
            modal.classList.toggle('hidden');
            if (!modal.classList.contains('hidden')) {
                document.getElementById('edit-name-input').value = currentUserData.name || '';
                document.getElementById('edit-status-input').value = currentUserData.status || '';
                document.getElementById('edit-avatar-file').value = '';
            }
        };

        window.salvarPerfil = async () => {
            const novoNome = document.getElementById('edit-name-input').value.trim();
            const avatarFile = document.getElementById('edit-avatar-file').files[0];
            const novoStatus = document.getElementById('edit-status-input').value.trim();

            if (!novoNome) return alert("O nome não pode estar vazio.");

            try {
                let novoAvatar = currentUserData.avatar || '';
                if (avatarFile) {
                    novoAvatar = await fileToBase64(avatarFile);
                }

                const userRef = doc(db, "chatUsers", currentUserData.username);
                await updateDoc(userRef, {
                    name: novoNome,
                    avatar: novoAvatar,
                    status: novoStatus
                });

                currentUserData.name = novoNome;
                currentUserData.avatar = novoAvatar;
                currentUserData.status = novoStatus;

                atualizarUIPerfilProprio();
                toggleEditProfileModal();
            } catch (e) {
                console.error(e);
                alert("Erro ao atualizar perfil.");
            }
        };

        window.onload = async () => {
            const savedUsername = localStorage.getItem('whatchat_username');
            if (savedUsername) {
                try {
                    const snap = await getDoc(doc(db, "chatUsers", savedUsername));
                    if (snap.exists()) entrarComoUsuario(snap.data());
                    else document.getElementById('auth-screen').classList.remove('hidden');
                } catch (e) {
                    document.getElementById('auth-screen').classList.remove('hidden');
                }
            } else {
                document.getElementById('auth-screen').classList.remove('hidden');
            }
            document.getElementById('loading-screen').classList.add('hidden');
        };

        window.switchTab = (tabName) => {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(`tab-btn-${tabName}`).classList.add('active');

            document.getElementById('tab-content-chats').classList.add('hidden');
            document.getElementById('tab-content-status').classList.add('hidden');
            document.getElementById('tab-content-calls').classList.add('hidden');

            document.getElementById(`tab-content-${tabName}`).classList.remove('hidden');
        };

        window.toggleAddContactModal = () => {
            document.getElementById('add-contact-modal').classList.toggle('hidden');
        };

        window.addContact = async () => {
            const username = normalizarUsuario(document.getElementById('new-contact-username').value);
            if (!username || username === currentUserData.username) return alert("Usuário inválido.");

            try {
                const snap = await getDoc(doc(db, "chatUsers", username));
                if (!snap.exists()) return alert("Usuário não encontrado.");

                const data = snap.data();
                const contatoInfo = { username: data.username, name: data.name, avatar: data.avatar || '', status: data.status || '' };
                await updateDoc(doc(db, "chatUsers", currentUserData.username), { contatos: arrayUnion(contatoInfo) });

                contatosCache.push(contatoInfo);
                renderContactsList();
                toggleAddContactModal();
                document.getElementById('new-contact-username').value = '';
            } catch (e) {
                console.error(e);
                alert("Erro ao adicionar contato.");
            }
        };

        async function carregarContatos() {
            try {
                const snap = await getDoc(doc(db, "chatUsers", currentUserData.username));
                contatosCache = (snap.exists() && Array.isArray(snap.data().contatos)) ? snap.data().contatos : [];
                renderContactsList();
            } catch (e) {
                console.error(e);
            }
        }

        function renderContactsList(filtro = '') {
            const list = document.getElementById('contacts-list');
            const vazio = document.getElementById('contacts-empty');
            list.innerHTML = '';

            const filtrados = contatosCache.filter(c => c.name.toLowerCase().includes(filtro.toLowerCase()) || c.username.toLowerCase().includes(filtro.toLowerCase()));

            if (filtrados.length === 0) {
                vazio.classList.remove('hidden');
                return;
            }
            vazio.classList.add('hidden');

            filtrados.forEach(user => {
                const item = document.createElement('div');
                item.className = "p-3 hover:bg-slate-50 cursor-pointer flex items-center gap-3 transition border-b border-slate-100";
                item.onclick = () => window.selectContact(user);

                const avatarHTML = user.avatar
                    ? `<img src="${user.avatar}" class="w-11 h-11 rounded-xl object-cover shadow-blue-glow">`
                    : `<div class="w-11 h-11 bg-sky-600 rounded-xl flex items-center justify-center text-white font-bold text-sm shadow-blue-glow">${user.name.charAt(0).toUpperCase()}</div>`;

                item.innerHTML = `
                    ${avatarHTML}
                    <div class="flex-grow min-w-0">
                        <div class="flex justify-between items-center">
                            <h4 class="font-semibold text-slate-800 text-sm truncate">${user.name}</h4>
                            <span class="text-[9px] text-orange-500 font-bold uppercase ml-2">@${user.username}</span>
                        </div>
                        <p class="text-xs text-slate-400 truncate">${user.status || 'Clique para abrir a conversa'}</p>
                    </div>
                `;
                list.appendChild(item);
            });
        }

        window.filterContacts = () => {
            renderContactsList(document.getElementById('search-input').value);
        };

        window.selectContact = async (user) => {
            activeContactUsername = user.username;
            activeContactName = user.name;
            isFirstLoad = true;
            
            document.getElementById('welcome-view').classList.add('hidden');
            const activeView = document.getElementById('active-chat-view');
            activeView.classList.remove('hidden');
            activeView.classList.add('flex');

            let freshUser = user;
            try {
                const snap = await getDoc(doc(db, "chatUsers", user.username));
                if (snap.exists()) freshUser = snap.data();
            } catch(e) {}

            document.getElementById('active-contact-name').textContent = freshUser.name;
            document.getElementById('active-contact-status').textContent = freshUser.status ? `"${freshUser.status}"` : `@${freshUser.username}`;

            const activeAvatarBox = document.getElementById('active-contact-avatar-box');
            if (freshUser.avatar) {
                activeAvatarBox.innerHTML = `<img src="${freshUser.avatar}" class="w-full h-full object-cover rounded-xl">`;
            } else {
                activeAvatarBox.innerHTML = `<span id="active-contact-avatar">${freshUser.name.charAt(0).toUpperCase()}</span>`;
            }

            listenToMessages();
        };

        function listenToMessages() {
            if (unsubscribeMessages) unsubscribeMessages();

            const chatId = [currentUserData.username, activeContactUsername].sort().join('_');
            const q = query(collection(db, "chats", chatId, "messages"), orderBy("timestamp", "asc"));

            const container = document.getElementById('messages-container');
            container.innerHTML = '';

            unsubscribeMessages = onSnapshot(q, (snapshot) => {
                container.innerHTML = '';

                snapshot.forEach((docSnap) => {
                    const msgId = docSnap.id;
                    const msg = docSnap.data();
                    const isSent = msg.sender === currentUserData.username;

                    const bubble = document.createElement('div');
                    bubble.className = `max-w-[70%] p-3 rounded-2xl text-xs relative group ${
                        isSent
                            ? 'bg-sky-600 text-white self-end rounded-tr-none shadow-blue-glow'
                            : 'bg-white text-slate-800 border border-slate-200 self-start rounded-tl-none shadow-sm'
                    }`;

                    let contentHTML = '';
                    if (msg.text) {
                        contentHTML += `<div class="pr-6">${msg.text}</div>`;
                    }
                    if (msg.image) {
                        const fileName = msg.fileName || 'imagem.png';
                        contentHTML += `
                            <div class="relative group my-1">
                                <img src="${msg.image}" class="rounded-xl max-w-full max-h-60 object-cover border border-slate-200">
                                <a href="${msg.image}" download="${fileName}" title="Baixar Imagem" class="absolute bottom-2 right-2 bg-black/60 hover:bg-black/80 text-white p-2 rounded-lg text-xs transition flex items-center gap-1">
                                    <i class="fas fa-download"></i> Baixar
                                </a>
                            </div>
                        `;
                    }
                    if (msg.video) {
                        const fileName = msg.fileName || 'video.mp4';
                        contentHTML += `
                            <div class="relative group my-1">
                                <video src="${msg.video}" controls class="rounded-xl max-w-full max-h-60 border border-slate-200"></video>
                                <a href="${msg.video}" download="${fileName}" title="Baixar Vídeo" class="mt-1 inline-flex items-center gap-1.5 text-[11px] font-semibold ${isSent ? 'text-sky-100 hover:text-white' : 'text-sky-600 hover:text-sky-700'}">
                                    <i class="fas fa-download"></i> Baixar Vídeo
                                </a>
                            </div>
                        `;
                    }

                    // Botão para apagar mensagem
                    contentHTML += `
                        <button onclick="apagarMensagem('${msgId}')" title="Apagar Mensagem" class="absolute top-2 right-2 opacity-0 group-hover:opacity-100 transition ${isSent ? 'text-sky-200 hover:text-white' : 'text-slate-400 hover:text-red-500'}">
                            <i class="fas fa-trash-alt text-[11px]"></i>
                        </button>
                    `;

                    bubble.innerHTML = contentHTML;
                    container.appendChild(bubble);
                });

                if (!isFirstLoad) {
                    snapshot.docChanges().forEach((change) => {
                        if (change.type === "added") {
                            const msgNova = change.doc.data();
                            if (msgNova.sender !== currentUserData.username) {
                                dispararNotificacaoEVibracao(msgNova.text);
                            }
                        }
                    });
                }

                container.scrollTop = container.scrollHeight;
                isFirstLoad = false;
            });
        }

        window.sendMessage = async (e) => {
            e.preventDefault();
            const input = document.getElementById('message-input');
            const text = input.value.trim();
            if (!text || !activeContactUsername) return;

            const chatId = [currentUserData.username, activeContactUsername].sort().join('_');
            await addDoc(collection(db, "chats", chatId, "messages"), {
                text: text,
                image: null,
                video: null,
                sender: currentUserData.username,
                timestamp: new Date()
            });

            input.value = '';
        };

        window.logout = () => {
            localStorage.removeItem('whatchat_username');
            location.reload();
        };
    </script>
</body>
</html>
