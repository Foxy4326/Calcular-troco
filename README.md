<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculadora de Troco — com Login (Firebase)</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { box-sizing: border-box; transition: background 0.8s linear; }
    @keyframes rgbBackground {
      0% { background-color: #ff0040; }
      25% { background-color: #ff8000; }
      50% { background-color: #40ff00; }
      75% { background-color: #0080ff; }
      100% { background-color: #ff00ff; }
    }
    .rgb-active { animation: rgbBackground 8s linear infinite alternate; }
  </style>
</head>
<body id="body" class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen font-sans">

  <!-- Botão Config -->
  <button id="btnConfig" class="fixed top-4 right-4 bg-gradient-to-r from-indigo-500 to-blue-500 text-white text-xl p-3 rounded-full shadow-lg z-50" title="Configurações">⚙️</button>

  <!-- Menu Config -->
  <div id="menuConfig" class="hidden fixed top-16 right-4 bg-white rounded-2xl shadow-2xl p-4 w-64 z-50 border border-gray-200">
    <h2 class="font-bold text-gray-800 mb-3 text-center">Configurações</h2>
    <div class="flex items-center justify-between">
      <span class="font-semibold text-gray-700">Ativar Modo RGB</span>
      <label class="relative inline-flex items-center cursor-pointer">
        <input type="checkbox" id="toggleRGB" class="sr-only peer">
        <div class="w-11 h-6 bg-gray-300 rounded-full peer peer-checked:bg-gradient-to-r peer-checked:from-pink-500 peer-checked:to-yellow-500 transition-all"></div>
        <span class="absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-all peer-checked:translate-x-5"></span>
      </label>
    </div>
    <hr class="my-3" />
    <div class="text-sm text-gray-600">
      <p class="mb-2">Status: <span id="userStatus">Desconectado</span></p>
      <button id="logoutBtn" class="w-full bg-red-500 text-white py-2 rounded-lg hidden">Sair</button>
    </div>
  </div>

  <div class="container mx-auto px-4 py-8 max-w-md">

    <!-- Login Card -->
    <div id="authCard" class="bg-white rounded-2xl shadow-xl p-6 mb-6">
      <h1 class="text-2xl font-bold text-gray-800 mb-4 text-center">Entrar / Registrar</h1>

      <div id="authForms" class="space-y-4">
        <!-- Email / Senha -->
        <div>
          <label class="block text-sm font-semibold text-gray-700 mb-1">Email</label>
          <input id="email" type="email" class="w-full px-4 py-2 border rounded-lg" placeholder="seu@email.com">
        </div>
        <div>
          <label class="block text-sm font-semibold text-gray-700 mb-1">Senha</label>
          <input id="password" type="password" class="w-full px-4 py-2 border rounded-lg" placeholder="senha">
        </div>

        <div class="grid grid-cols-2 gap-2">
          <button id="loginBtn" class="py-2 bg-blue-600 text-white rounded-lg">Entrar</button>
          <button id="registerBtn" class="py-2 bg-green-600 text-white rounded-lg">Registrar</button>
        </div>

        <div class="text-center text-gray-500">ou</div>

        <div class="grid grid-cols-2 gap-2">
          <button id="googleBtn" class="py-2 bg-white border rounded-lg">Entrar com Google</button>
          <button id="githubBtn" class="py-2 bg-white border rounded-lg">Entrar com GitHub</button>
        </div>

        <p id="authMsg" class="text-sm text-red-500 mt-2 hidden"></p>
      </div>
    </div>

    <!-- App (Calculadora) - visível somente quando autenticado -->
    <div id="appCard" class="hidden bg-white rounded-2xl shadow-xl p-6 mb-6">
      <div class="text-center mb-6">
        <div class="bg-white rounded-full w-20 h-20 mx-auto mb-4 flex items-center justify-center shadow-lg">
          <span class="text-3xl">💰</span>
        </div>
        <h2 class="text-2xl font-bold text-gray-800">Calculadora de Troco</h2>
        <p class="text-gray-600">Apenas usuários autenticados podem usar</p>
      </div>

      <form id="trocoForm" class="space-y-4">
        <div>
          <label class="block text-sm font-semibold text-gray-700 mb-1">Valor da Compra</label>
          <div class="relative">
            <span class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500">R$</span>
            <input id="valorCompra" type="number" step="0.01" min="0" class="w-full pl-10 pr-4 py-3 border rounded-lg" placeholder="0,00" required>
          </div>
        </div>

        <div>
          <label class="block text-sm font-semibold text-gray-700 mb-1">Valor Pago</label>
          <div class="relative">
            <span class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500">R$</span>
            <input id="valorPago" type="number" step="0.01" min="0" class="w-full pl-10 pr-4 py-3 border rounded-lg" placeholder="0,00" required>
          </div>
        </div>

        <button type="submit" class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 text-white py-3 rounded-lg">Calcular Troco</button>
      </form>

      <div id="resultado" class="hidden mt-4 bg-gray-50 p-4 rounded-lg">
        <div class="text-center">
          <div id="valorTroco" class="text-2xl font-bold">R$ 0,00</div>
          <div class="text-sm text-gray-600 mt-2">
            <div>Pago: <span id="valorPagoDisplay">R$ 0,00</span></div>
            <div>Compra: <span id="valorCompraDisplay">R$ 0,00</span></div>
          </div>
        </div>
      </div>

      <div class="grid grid-cols-3 gap-2 mt-4">
        <button onclick="adicionarValor(5)" class="py-2 bg-gray-100 rounded-lg">+R$5</button>
        <button onclick="adicionarValor(10)" class="py-2 bg-gray-100 rounded-lg">+R$10</button>
        <button onclick="adicionarValor(20)" class="py-2 bg-gray-100 rounded-lg">+R$20</button>
      </div>
    </div>

    <!-- Footer info -->
    <div class="text-center text-gray-500 text-sm mt-4">
      <p>Projeto de exemplo — substitua as credenciais do Firebase abaixo</p>
    </div>

  </div>

  <!-- Firebase SDK (modular) -->
  <script type="module">
    /***********************************************
     * 1) INSTRUÇÕES RÁPIDAS (FAÇA ANTES DE RODAR)
     *
     * - Crie um projeto no https://console.firebase.google.com
     * - Adicione um app Web e copie o objeto de configuração (apiKey, authDomain, ...)
     * - Cole o objeto FIREBASE_CONFIG abaixo (substitua os placeholders)
     * - No Console Firebase > Authentication > Sign-in method:
     *      * Habilite Email/Password
     *      * Habilite Google
     *      * Habilite GitHub (para GitHub, configure Client ID/Secret no painel do GitHub e cole no Firebase)
     * - Em Authentication > Sign-in method > GitHub, defina "Authorized domains" (ex: localhost)
     * - Em "Authorized domains" do Firebase, inclua o host que você vai usar (ex: localhost)
     *
     * 2) SEGURANÇA: No console do Firebase defina regras apropriadas para Firestore/Storage se você for usar.
     ***********************************************/

    // ====== COLE AQUI SUAS CREDENCIAIS FIREBASE ======
    // Substitua os valores abaixo com os do seu projeto no Firebase.
    const firebaseConfig = {
      apiKey: "FIREBASE_APIKEY_HERE",
      authDomain: "FIREBASE_AUTHDOMAIN_HERE",
      projectId: "FIREBASE_PROJECTID_HERE",
      storageBucket: "FIREBASE_STORAGEBUCKET_HERE",
      messagingSenderId: "FIREBASE_MESSAGINGSENDERID_HERE",
      appId: "FIREBASE_APPID_HERE"
    };
    // ================================================

    // Import SDKs via CDN (ES modules)
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.4.0/firebase-app.js";
    import {
      getAuth,
      signInWithEmailAndPassword,
      createUserWithEmailAndPassword,
      signOut,
      onAuthStateChanged,
      GoogleAuthProvider,
      GithubAuthProvider,
      signInWithPopup
    } from "https://www.gstatic.com/firebasejs/10.4.0/firebase-auth.js";

    // Init
    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);

    // Providers
    const googleProvider = new GoogleAuthProvider();
    const githubProvider = new GithubAuthProvider();

    // --- UI elements ---
    const authCard = document.getElementById('authCard');
    const appCard = document.getElementById('appCard');
    const authMsg = document.getElementById('authMsg');
    const userStatus = document.getElementById('userStatus');
    const logoutBtn = document.getElementById('logoutBtn');

    const btnConfig = document.getElementById('btnConfig');
    const menuConfig = document.getElementById('menuConfig');
    const toggleRGB = document.getElementById('toggleRGB');
    const body = document.getElementById('body');

    // Auth inputs/buttons
    const emailInput = document.getElementById('email');
    const passInput = document.getElementById('password');
    const loginBtn = document.getElementById('loginBtn');
    const registerBtn = document.getElementById('registerBtn');
    const googleBtn = document.getElementById('googleBtn');
    const githubBtn = document.getElementById('githubBtn');

    // Calculadora
    const trocoForm = document.getElementById('trocoForm');
    const resultado = document.getElementById('resultado');
    const valorTroco = document.getElementById('valorTroco');
    const valorPagoDisplay = document.getElementById('valorPagoDisplay');
    const valorCompraDisplay = document.getElementById('valorCompraDisplay');

    // Quick buttons
    window.adicionarValor = function (v) {
      const el = document.getElementById('valorPago');
      const cur = parseFloat(el.value) || 0;
      el.value = (cur + v).toFixed(2);
    };

    // ===== UI helpers =====
    function showAuthError(msg) {
      authMsg.textContent = msg;
      authMsg.classList.remove('hidden');
    }
    function hideAuthError() {
      authMsg.classList.add('hidden');
      authMsg.textContent = '';
    }

    // ===== Event handlers - Auth =====
    loginBtn.addEventListener('click', async () => {
      hideAuthError();
      const email = emailInput.value.trim();
      const password = passInput.value;
      if (!email || !password) return showAuthError('Preencha email e senha.');
      try {
        await signInWithEmailAndPassword(auth, email, password);
        // onAuthStateChanged cuidará do resto
      } catch (err) {
        showAuthError(err.message || 'Erro ao entrar.');
      }
    });

    registerBtn.addEventListener('click', async () => {
      hideAuthError();
      const email = emailInput.value.trim();
      const password = passInput.value;
      if (!email || !password) return showAuthError('Preencha email e senha.');
      try {
        await createUserWithEmailAndPassword(auth, email, password);
      } catch (err) {
        showAuthError(err.message || 'Erro ao registrar.');
      }
    });

    googleBtn.addEventListener('click', async () => {
      hideAuthError();
      try {
        await signInWithPopup(auth, googleProvider);
      } catch (err) {
        showAuthError(err.message || 'Erro Google.');
      }
    });

    githubBtn.addEventListener('click', async () => {
      hideAuthError();
      try {
        await signInWithPopup(auth, githubProvider);
      } catch (err) {
        showAuthError(err.message || 'Erro GitHub.');
      }
    });

    logoutBtn.addEventListener('click', async () => {
      await signOut(auth);
    });

    // Toggle menu
    btnConfig.addEventListener('click', () => menuConfig.classList.toggle('hidden'));

    // Toggle RGB
    toggleRGB.addEventListener('change', () => {
      body.classList.toggle('rgb-active', toggleRGB.checked);
    });

    // Show/hide UI based on auth state
    onAuthStateChanged(auth, (user) => {
      if (user) {
        // Usuário logado
        authCard.classList.add('hidden');
        appCard.classList.remove('hidden');
        logoutBtn.classList.remove('hidden');
        userStatus.textContent = user.displayName || user.email || 'Usuário';
        // Scroll to app
        appCard.scrollIntoView({ behavior: 'smooth' });
      } else {
        // Deslogado
        authCard.classList.remove('hidden');
        appCard.classList.add('hidden');
        logoutBtn.classList.add('hidden');
        userStatus.textContent = 'Desconectado';
      }
    });

    // ===== Calculadora =====
    function formatarMoeda(valor) {
      return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(valor);
    }

    trocoForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const valorCompra = parseFloat(document.getElementById('valorCompra').value) || 0;
      const valorPago = parseFloat(document.getElementById('valorPago').value) || 0;
      if (valorCompra <= 0 || valorPago <= 0) return alert('Insira valores válidos.');
      const troco = valorPago - valorCompra;
      valorTroco.textContent = formatarMoeda(troco);
      valorPagoDisplay.textContent = formatarMoeda(valorPago);
      valorCompraDisplay.textContent = formatarMoeda(valorCompra);
      resultado.classList.remove('hidden');
    });

    // Quick UX: autocalc quando inputs mudarem (se resultado estiver visível)
    document.getElementById('valorCompra').addEventListener('input', () => {
      if (!resultado.classList.contains('hidden')) trocoForm.requestSubmit();
    });
    document.getElementById('valorPago').addEventListener('input', () => {
      if (!resultado.classList.contains('hidden')) trocoForm.requestSubmit();
    });

  </script>
</body>
</html>
