<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculadora de Troco</title>
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
  </div>

  <div class="container mx-auto px-4 py-8 max-w-md">

    <!-- Calculadora -->
    <div class="bg-white rounded-2xl shadow-xl p-6 mb-6">
      <div class="text-center mb-6">
        <div class="bg-white rounded-full w-20 h-20 mx-auto mb-4 flex items-center justify-center shadow-lg">
          <span class="text-3xl">💰</span>
        </div>
        <h1 class="text-3xl font-bold text-gray-800 mb-2">Calculadora de Troco</h1>
        <p class="text-gray-600">Calcule o troco de forma rápida e precisa</p>
      </div>

      <form id="trocoForm" class="space-y-4">
        <div>
          <label for="valorCompra" class="block text-sm font-semibold text-gray-700 mb-1">Valor da Compra</label>
          <div class="relative">
            <span class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500 font-medium">R$</span>
            <input type="number" id="valorCompra" step="0.01" min="0" class="w-full pl-10 pr-4 py-3 border rounded-lg" placeholder="0,00" required>
          </div>
        </div>

        <div>
          <label for="valorPago" class="block text-sm font-semibold text-gray-700 mb-1">Valor Pago</label>
          <div class="relative">
            <span class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500 font-medium">R$</span>
            <input type="number" id="valorPago" step="0.01" min="0" class="w-full pl-10 pr-4 py-3 border rounded-lg" placeholder="0,00" required>
          </div>
        </div>

        <button type="submit" class="w-full bg-gradient-to-r from-blue-600 to-indigo-600 text-white py-3 rounded-lg font-bold hover:from-blue-700 hover:to-indigo-700 transform hover:scale-105 transition-all duration-200">Calcular Troco</button>
      </form>

      <div id="resultado" class="hidden mt-4 bg-gray-50 p-4 rounded-lg text-center">
        <div id="valorTroco" class="text-2xl font-bold">R$ 0,00</div>
        <div class="text-sm text-gray-600 mt-2">
          <div>Pago: <span id="valorPagoDisplay">R$ 0,00</span></div>
          <div>Compra: <span id="valorCompraDisplay">R$ 0,00</span></div>
        </div>
      </div>

      <!-- Botões de valores rápidos -->
      <div class="grid grid-cols-3 gap-2 mt-4">
        <button type="button" onclick="adicionarValor(5)" class="py-2 bg-gray-100 rounded-lg">+R$5</button>
        <button type="button" onclick="adicionarValor(10)" class="py-2 bg-gray-100 rounded-lg">+R$10</button>
        <button type="button" onclick="adicionarValor(20)" class="py-2 bg-gray-100 rounded-lg">+R$20</button>
        <button type="button" onclick="adicionarValor(50)" class="py-2 bg-gray-100 rounded-lg">+R$50</button>
        <button type="button" onclick="adicionarValor(100)" class="py-2 bg-gray-100 rounded-lg">+R$100</button>
        <button type="button" onclick="limparCampos()" class="py-2 bg-red-100 text-red-700 rounded-lg">Limpar</button>
      </div>
    </div>

  </div>

  <script>
    const form = document.getElementById('trocoForm');
    const resultado = document.getElementById('resultado');
    const valorTroco = document.getElementById('valorTroco');
    const valorPagoDisplay = document.getElementById('valorPagoDisplay');
    const valorCompraDisplay = document.getElementById('valorCompraDisplay');

    function formatarMoeda(valor) {
      return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(valor);
    }

    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const valorCompra = parseFloat(document.getElementById('valorCompra').value) || 0;
      const valorPago = parseFloat(document.getElementById('valorPago').value) || 0;
      if (valorCompra <= 0) return alert('Insira um valor de compra válido.');
      if (valorPago <= 0) return alert('Insira um valor pago válido.');

      const troco = valorPago - valorCompra;

      valorCompraDisplay.textContent = formatarMoeda(valorCompra);
      valorPagoDisplay.textContent = formatarMoeda(valorPago);
      valorTroco.textContent = formatarMoeda(troco);

      resultado.classList.remove('hidden');
    });

    window.adicionarValor = function(valor) {
      const valorPagoInput = document.getElementById('valorPago');
      const valorAtual = parseFloat(valorPagoInput.value) || 0;
      valorPagoInput.value = (valorAtual + valor).toFixed(2);
    };

    window.limparCampos = function() {
      document.getElementById('valorCompra').value = '';
      document.getElementById('valorPago').value = '';
      resultado.classList.add('hidden');
    };

    // Configurações RGB
    const btnConfig = document.getElementById('btnConfig');
    const menuConfig = document.getElementById('menuConfig');
    const toggleRGB = document.getElementById('toggleRGB');
    const body = document.getElementById('body');

    btnConfig.addEventListener('click', () => menuConfig.classList.toggle('hidden'));
    toggleRGB.addEventListener('change', () => body.classList.toggle('rgb-active', toggleRGB.checked));
  </script>
</body>
</html>
