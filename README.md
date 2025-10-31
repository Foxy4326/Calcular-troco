<body id="body"
  class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen font-sans transition-all duration-500">

  <!-- 🔧 Botão de Configurações (agora visível no topo direito) -->
  <button id="btnConfig"
    class="fixed top-4 right-4 bg-gradient-to-r from-indigo-500 to-blue-500 text-white text-xl p-3 rounded-full shadow-lg hover:scale-110 active:scale-95 transition-all duration-200"
    title="Configurações">
    ⚙️
  </button>

  <!-- ⚙️ Menu de Configurações -->
  <div id="menuConfig"
    class="hidden fixed top-16 right-4 bg-white rounded-2xl shadow-2xl p-4 w-64 z-50 border border-gray-200 transition-all duration-300">
    <h2 class="font-bold text-gray-800 mb-3 text-center">Configurações</h2>
    <div class="flex items-center justify-between">
      <span class="font-semibold text-gray-700">Ativar Modo RGB</span>
      <label class="relative inline-flex items-center cursor-pointer">
        <input type="checkbox" id="toggleRGB" class="sr-only peer">
        <div
          class="w-11 h-6 bg-gray-300 rounded-full peer peer-checked:bg-gradient-to-r peer-checked:from-pink-500 peer-checked:to-yellow-500 transition-all">
        </div>
        <span
          class="absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-all peer-checked:translate-x-5"></span>
      </label>
    </div>
  </div>

  <div class="container mx-auto px-4 py-8 max-w-md">
    <!-- Header -->
    <div class="text-center mb-8">
      <div
        class="bg-white rounded-full w-20 h-20 mx-auto mb-4 flex items-center justify-center shadow-lg">
        <span class="text-3xl">💰</span>
      </div>
      <h1 class="text-3xl font-bold text-gray-800 mb-2">Calculadora de Troco</h1>
      <p class="text-gray-600">Calcule o troco de forma rápida e precisa</p>
    </div>

    <!-- Calculator Card -->
    <div class="bg-white rounded-2xl shadow-xl p-6 mb-6">
      <form id="trocoForm" class="space-y-6">
        <div>
          <label for="valorCompra"
            class="block text-sm font-semibold text-gray-700 mb-2">Valor da Compra</label>
          <div class="relative">
            <span
              class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500 font-medium">R$</span>
            <input type="number" id="valorCompra" step="0.01" min="0"
              class="w-full pl-10 pr-4 py-3 border-2 border-gray-200 rounded-xl focus:border-blue-500 focus:outline-none text-lg font-medium"
              placeholder="0,00" required>
          </div>
        </div>

        <div>
          <label for="valorPago"
            class="block text-sm font-semibold text-gray-700 mb-2">Valor Pago pelo Cliente</label>
          <div class="relative">
            <span
              class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500 font-medium">R$</span>
            <input type="number" id="valorPago" step="0.01" min="0"
              class="w-full pl-10 pr-4 py-3 border-2 border-gray-200 rounded-xl focus:border-blue-500 focus:outline-none text-lg font-medium"
              placeholder="0,00" required>
          </div>
        </div>

        <button type="submit"
          class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 text-white font-bold py-4 px-6 rounded-xl hover:from-blue-700 hover:to-indigo-700 transform hover:scale-105 transition-all duration-200 shadow-lg">
          Calcular Troco
        </button>
      </form>
    </div>

    <div id="resultado" class="hidden bg-white rounded-2xl shadow-xl p-6 mb-6">
      <div class="text-center">
        <div class="mb-4">
          <span class="text-4xl" id="resultadoIcon">💵</span>
        </div>
        <h2 class="text-xl font-bold text-gray-800 mb-2" id="resultadoTitulo">Troco a Devolver</h2>
        <div class="text-4xl font-bold mb-4" id="valorTroco">R$ 0,00</div>
        <div id="detalhamento" class="text-sm text-gray-600 bg-gray-50 rounded-lg p-4">
          <div class="flex justify-between mb-1">
            <span>Valor pago:</span>
            <span id="valorPagoDisplay">R$ 0,00</span>
          </div>
          <div class="flex justify-between mb-1">
            <span>Valor da compra:</span>
            <span id="valorCompraDisplay">R$ 0,00</span>
          </div>
          <hr class="my-2">
          <div class="flex justify-between font-semibold">
            <span>Troco:</span>
            <span id="trocoDisplay">R$ 0,00</span>
          </div>
        </div>
      </div>
    </div>

    <div class="bg-white rounded-2xl shadow-xl p-6">
      <h3 class="text-lg font-bold text-gray-800 mb-4 text-center">Valores Rápidos</h3>
      <div class="grid grid-cols-3 gap-3">
        <button onclick="adicionarValor(5)"
          class="bg-gray-100 hover:bg-gray-200 text-gray-800 font-semibold py-3 px-4 rounded-lg transition-colors">+R$
          5</button>
        <button onclick="adicionarValor(10)"
          class="bg-gray-100 hover:bg-gray-200 text-gray-800 font-semibold py-3 px-4 rounded-lg transition-colors">+R$
          10</button>
        <button onclick="adicionarValor(20)"
          class="bg-gray-100 hover:bg-gray-200 text-gray-800 font-semibold py-3 px-4 rounded-lg transition-colors">+R$
          20</button>
        <button onclick="adicionarValor(50)"
          class="bg-gray-100 hover:bg-gray-200 text-gray-800 font-semibold py-3 px-4 rounded-lg transition-colors">+R$
          50</button>
        <button onclick="adicionarValor(100)"
          class="bg-gray-100 hover:bg-gray-200 text-gray-800 font-semibold py-3 px-4 rounded-lg transition-colors">+R$
          100</button>
        <button onclick="limparCampos()"
          class="bg-red-100 hover:bg-red-200 text-red-800 font-semibold py-3 px-4 rounded-lg transition-colors">Limpar</button>
      </div>
    </div>
  </div>

  <style>
    @keyframes rgbBackground {
      0% { background-color: #ff0040; }
      25% { background-color: #ff8000; }
      50% { background-color: #40ff00; }
      75% { background-color: #0080ff; }
      100% { background-color: #ff00ff; }
    }

    .rgb-active {
      animation: rgbBackground 8s linear infinite alternate;
    }
  </style>

  <script>
    const form = document.getElementById('trocoForm');
    const resultado = document.getElementById('resultado');
    const valorTroco = document.getElementById('valorTroco');
    const resultadoIcon = document.getElementById('resultadoIcon');
    const resultadoTitulo = document.getElementById('resultadoTitulo');
    const valorPagoDisplay = document.getElementById('valorPagoDisplay');
    const valorCompraDisplay = document.getElementById('valorCompraDisplay');
    const trocoDisplay = document.getElementById('trocoDisplay');

    function formatarMoeda(valor) {
      return new Intl.NumberFormat('pt-BR', {
        style: 'currency',
        currency: 'BRL'
      }).format(valor);
    }

    function calcularTroco(event) {
      event.preventDefault();
      const valorCompra = parseFloat(document.getElementById('valorCompra').value) || 0;
      const valorPago = parseFloat(document.getElementById('valorPago').value) || 0;
      if (valorCompra <= 0 || valorPago <= 0) return alert('Insira valores válidos.');
      const troco = valorPago - valorCompra;

      valorPagoDisplay.textContent = formatarMoeda(valorPago);
      valorCompraDisplay.textContent = formatarMoeda(valorCompra);
      trocoDisplay.textContent = formatarMoeda(troco);

      if (troco < 0) {
        resultadoIcon.textContent = '❌';
        resultadoTitulo.textContent = 'Valor Insuficiente';
        valorTroco.textContent = formatarMoeda(Math.abs(troco));
        valorTroco.className = 'text-4xl font-bold mb-4 text-red-600';
      } else if (troco === 0) {
        resultadoIcon.textContent = '✅';
        resultadoTitulo.textContent = 'Valor Exato';
        valorTroco.textContent = 'Sem troco';
        valorTroco.className = 'text-4xl font-bold mb-4 text-green-600';
      } else {
        resultadoIcon.textContent = '💵';
        resultadoTitulo.textContent = 'Troco a Devolver';
        valorTroco.textContent = formatarMoeda(troco);
        valorTroco.className = 'text-4xl font-bold mb-4 text-blue-600';
      }

      resultado.classList.remove('hidden');
      resultado.scrollIntoView({ behavior: 'smooth' });
    }

    function adicionarValor(valor) {
      const valorPagoInput = document.getElementById('valorPago');
      const valorAtual = parseFloat(valorPagoInput.value) || 0;
      valorPagoInput.value = (valorAtual + valor).toFixed(2);
    }

    function limparCampos() {
      document.getElementById('valorCompra').value = '';
      document.getElementById('valorPago').value = '';
      resultado.classList.add('hidden');
    }

    const btnConfig = document.getElementById('btnConfig');
    const menuConfig = document.getElementById('menuConfig');
    const toggleRGB = document.getElementById('toggleRGB');
    const body = document.getElementById('body');

    btnConfig.addEventListener('click', () => {
      menuConfig.classList.toggle('hidden');
    });

    toggleRGB.addEventListener('change', () => {
      body.classList.toggle('rgb-active', toggleRGB.checked);
    });

    form.addEventListener('submit', calcularTroco);
  </script>
</body>
