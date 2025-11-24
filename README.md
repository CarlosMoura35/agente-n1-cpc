const modeRadios = document.querySelectorAll('input[name="mode"]');
const inputLabel = document.getElementById('inputLabel');
const inputText = document.getElementById('inputText');
const outputText = document.getElementById('outputText');
const btn = document.getElementById('convertBtn');

let mode = 'nl2cpc';

modeRadios.forEach(radio => {
  radio.addEventListener('change', () => {
    mode = radio.value;
    if (mode === 'nl2cpc') {
      inputLabel.textContent = 'Frase em português:';
      inputText.placeholder = 'Ex: Se chover, então a grama ficará molhada.';
      outputText.value = '';
    } else {
      inputLabel.textContent = 'Fórmula em lógica proposicional:';
      inputText.placeholder = 'Ex: (P ∧ Q) → R';
      outputText.value = '';
    }
  });
});

btn.addEventListener('click', () => {
  const txt = inputText.value.trim();
  const p = document.getElementById('propP').value.trim();
  const q = document.getElementById('propQ').value.trim();
  const r = document.getElementById('propR').value.trim();

  if (!txt) {
    outputText.value = 'Digite algo na entrada antes de converter.';
    return;
  }

  if (mode === 'nl2cpc') {
    outputText.value = nlToCpc(txt);
  } else {
    outputText.value = cpcToNl(txt, { P: p, Q: q, R: r });
  }
});

function nlToCpc(sentence) {
  // protótipo bem simples para padrão "Se X, então Y"
  const lower = sentence.toLowerCase();

  if (lower.startsWith('se ') && lower.includes('então')) {
    const partes = lower.split('então');
    const antes = partes[0].replace(/^se/, '').trim();
    const depois = partes[1].trim();

    let formula = 'P → Q';
    let mapping = P = ${antes || 'proposição 1'}\nQ = ${depois || 'proposição 2'};
    return Fórmula: ${formula}\n${mapping};
  }

  return 'Conversão não implementada para este padrão de frase.\nUse algo como: "Se X, então Y".';
}

function cpcToNl(formula, props) {
  let out = '';
  let f = formula.replace(/\s+/g, '');

  const map = (symbol) => {
    if (symbol === 'P' && props.P) return props.P;
    if (symbol === 'Q' && props.Q) return props.Q;
    if (symbol === 'R' && props.R) return props.R;
    return symbol;
  };

  if (f.includes('->') || f.includes('→')) {
    const parts = f.split(/->|→/);
    const left = parts[0];
    const right = parts[1];

    out = Se ${expandSimple(left, map)}, então ${expandSimple(right, map)}.;
  } else if (f.includes('∧') || f.includes('&')) {
    const parts = f.split(/∧|&/);
    out = ${expandSimple(parts[0], map)} e ${expandSimple(parts[1], map)}.;
  } else if (f.includes('∨') || f.toLowerCase().includes('v')) {
    const parts = f.split(/∨|v/i);
    out = ${expandSimple(parts[0], map)} ou ${expandSimple(parts[1], map)}.;
  } else if (f.startsWith('¬')) {
    out = não ${expandSimple(f.slice(1), map)}.;
  } else {
    out = 'Padrão de fórmula não suportado nesta demo.';
  }

  return out;
}

function expandSimple(expr, map) {
  let e = expr.replace(/[()]/g, '');
  let neg = false;
  if (e.startsWith('¬')) {
    neg = true;
    e = e.slice(1);
  }
  let base = map(e) || e;
  if (neg) {
    return 'não ' + base;


    * {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  background: radial-gradient(circle at top, #1f2937, #020617);
  color: #e5e7eb;
  line-height: 1.5;
}

.container {
  width: 100%;
  max-width: 960px;
  margin: 0 auto;
  padding: 0 1rem;
}

.topbar {
  background: linear-gradient(90deg, #0ea5e9, #6366f1, #a855f7);
  padding: 2.5rem 0 2rem;
  text-align: center;
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.35);
}

.topbar h1 {
  font-size: 2.2rem;
  font-weight: 700;
  color: #0b1120;
}

.subtitle {
  margin-top: 0.3rem;
  font-size: 1rem;
  color: #020617;
  font-weight: 500;
}

.members {
  margin-top: 0.4rem;
  font-size: 0.9rem;
  color: #020617;
}

main {
  margin-top: 2rem;
  margin-bottom: 2.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.card {
  background: rgba(15, 23, 42, 0.9);
  border-radius: 0.75rem;
  padding: 1.5rem 1.5rem 1.7rem;
  box-shadow: 0 14px 35px rgba(0, 0, 0, 0.45);
  border: 1px solid rgba(148, 163, 184, 0.25);
}

.card h2 {
  font-size: 1.3rem;
  margin-bottom: 0.75rem;
  color: #e5e7eb;
}

.card h3 {
  font-size: 1.05rem;
  margin-top: 0.8rem;
  margin-bottom: 0.25rem;
  color: #c4c9d4;
}

.card p {
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
  color: #cbd5f5;
}

.card ul,
.card ol {
  margin-left: 1.2rem;
  font-size: 0.95rem;
  color: #cbd5f5;
}

.card li {
  margin-bottom: 0.3rem;
}

code {
  background: rgba(15, 23, 42, 0.8);
  border-radius: 4px;
  padding: 0.1rem 0.35rem;
  font-size: 0.9rem;
}

/* Demo */

.mode-selector {
  display: flex;
  gap: 1rem;
  margin: 0.75rem 0 1rem;
  font-size: 0.95rem;
}

.mode-selector input {
  margin-right: 0.3rem;
}

.field {
  margin-bottom: 1rem;
}

.field label {
  display: block;
  margin-bottom: 0.35rem;
  font-size: 0.9rem;
  color: #e5e7eb;
}

textarea,
input[type="text"] {
  width: 100%;
  border-radius: 0.5rem;
  border: 1px solid rgba(148, 163, 184, 0.6);
  background: #020617;
  color: #e5e7eb;
  padding: 0.55rem 0.7rem;
  font-size: 0.9rem;
  resize: vertical;
}

textarea:focus,
input[type="text"]:focus {
  outline: none;
  border-color: #6366f1;
  box-shadow: 0 0 0 1px rgba(99, 102, 241, 0.4);
}

.props-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 0.75rem;
  margin-bottom: 1rem;
}

button#convertBtn {
  display: inline-block;
  margin-bottom: 1rem;
  padding: 0.6rem 1.4rem;
  border-radius: 999px;
  border: none;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  background: linear-gradient(90deg, #22c55e, #16a34a);
  color: #022c22;
  box-shadow: 0 12px 30px rgba(22, 163, 74, 0.45);
  transition: transform 0.08s ease, box-shadow 0.08s ease, filter 0.1s ease;
}

button#convertBtn:hover {
  transform: translateY(-1px);
  filter: brightness(1.03);
  box-shadow: 0 16px 40px rgba(22, 163, 74, 0.6);
}

button#convertBtn:active {
  transform: translateY(0);
  box-shadow: 0 10px 24px rgba(22, 163, 74, 0.4);
}

.footer {
  border-top: 1px solid rgba(148, 163, 184, 0.3);
  padding: 1rem 0 1.3rem;
  text-align: center;
  font-size: 0.8rem;
  color: #9ca3af;
}

/* Responsivo */

@media (max-width: 768px) {
  .props-grid {
    grid-template-columns: 1fr;
  }

  .topbar h1 {
    font-size: 1.6rem;
  }
}

<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <title>UNIFACEF – Agente NL ↔ CPC</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <header class="topbar">
    <div class="container">
      <h1>Agente NL ↔ CPC</h1>
      <p class="subtitle">UNIFACEF • Inteligência Artificial</p>
      <p class="members">Carlos Eduardo Souza • Ygor Caparelli</p>
    </div>
  </header>

  <main class="container">
    <!-- Seção de demo do agente -->
    <section class="card">
      <h2>Demo do Agente</h2>
      <p>Teste a conversão entre linguagem natural e lógica proposicional.</p>

      <div class="mode-selector">
        <label>
          <input type="radio" name="mode" value="nl2cpc" checked />
          NL → CPC
        </label>
        <label>
          <input type="radio" name="mode" value="cpc2nl" />
          CPC → NL
        </label>
      </div>

      <div class="field">
        <label for="inputText" id="inputLabel">Frase em português:</label>
        <textarea id="inputText" rows="4" placeholder="Ex: Se chover, então a grama ficará molhada."></textarea>
      </div>

      <div class="props-grid">
        <div>
          <label for="propP">Significado de P:</label>
          <input id="propP" type="text" placeholder="Ex: chover" />
        </div>
        <div>
          <label for="propQ">Significado de Q:</label>
          <input id="propQ" type="text" placeholder="Ex: a grama ficará molhada" />
        </div>
        <div>
          <label for="propR">Significado de R:</label>
          <input id="propR" type="text" placeholder="Ex: a aula será cancelada" />
        </div>
      </div>

      <button id="convertBtn">Converter</button>

      <div class="field">
        <label for="outputText">Saída gerada:</label>
        <textarea id="outputText" rows="4" readonly></textarea>
      </div>
    </section>

    <!-- Arquitetura -->
    <section class="card">
      <h2>1. Arquitetura do Sistema</h2>
      <p>O agente é dividido em quatro módulos principais:</p>
      <ol>
        <li><strong>Parser NL</strong> identifica proposições e conectivos na frase em português.</li>
        <li><strong>Mapeamento de proposições</strong> associa eventos a variáveis P, Q, R.</li>
        <li><strong>Tradutor NL ↔ CPC</strong> aplica regras de conversão e pode ser estendido com LLMs.</li>
        <li><strong>Gerador de saída</strong> formata o resultado em NL ou em fórmula proposicional.</li>
      </ol>
    </section>

    <!-- Estratégia de tradução -->
    <section class="card">
      <h2>2. Estratégia de Tradução</h2>
      <p>São usadas regras fixas de mapeamento entre conectivos da linguagem natural e da lógica proposicional:</p>
      <ul>
        <li>"se ... então ..." → <code>→</code></li>
        <li>"e" → <code>∧</code></li>
        <li>"ou" → <code>∨</code></li>
        <li>"não" → <code>¬</code></li>
        <li>"se e somente se" → <code>↔</code></li>
      </ul>
      <p>Um LLM pode complementar o sistema para resolver ambiguidades, interpretar frases mais complexas e sugerir reformulações mais naturais.</p>
    </section>

    <!-- Exemplos com análise -->
    <section class="card">
      <h2>3. Exemplos de Input e Output</h2>

      <h3>Exemplo 1 – NL → CPC</h3>
      <p><strong>Entrada:</strong> "Se o aluno estudar e não faltar, então ele será aprovado."</p>
      <p><strong>Mapeamento:</strong></p>
      <ul>
        <li>P = o aluno estudar</li>
        <li>Q = o aluno faltar</li>
        <li>R = ele será aprovado</li>
      </ul>
      <p><strong>Saída:</strong> <code>(P ∧ ¬Q) → R</code></p>
      <p><strong>Análise:</strong> a estrutura condicional foi preservada e a negação aplicada apenas a Q.</p>

      <h3>Exemplo 2 – CPC → NL</h3>
      <p><strong>Entrada:</strong> <code>¬P ∨ (Q ∧ R)</code></p>
      <p><strong>Saída:</strong> "Ou não P, ou Q e R acontecem."</p>
      <p><strong>Análise:</strong> a disjunção principal foi mantida e a conjunção interna agrupada corretamente.</p>
    </section>

    <!-- Limitações e melhorias -->
    <section class="card">
      <h2>4. Limitações e Melhorias Possíveis</h2>
      <h3>Limitações</h3>
      <ul>
        <li>Não há interpretação semântica profunda.</li>
        <li>Frases longas ou muito ambíguas podem exigir ajustes manuais.</li>
        <li>O protótipo foca apenas em lógica proposicional.</li>
      </ul>

      <h3>Possíveis melhorias</h3>
      <ul>
        <li>Suporte a quantificadores (∀, ∃).</li>
        <li>Parser sintático completo.</li>
        <li>Interface gráfica mais avançada para editar fórmulas.</li>
        <li>Banco de proposições salvas por usuário.</li>
      </ul>
    </section>

    <!-- Vídeo -->
    <section class="card">
      <h2>5. Vídeo de Demonstração</h2>
      <p>Após gravar o vídeo mostrando o uso do agente, atualize o link abaixo:</p>
      <p><strong>Link:</strong> <a href="https://youtu.be/EXEMPLO_DEMO" target="_blank">https://youtu.be/EXEMPLO_DEMO</a></p>
    </section>
  </main>

  <footer class="footer">
    <div class="container">
      <p>UNIFACEF • Projeto Agente NL ↔ CPC • 2025</p>
    </div>
  </footer>

  <script src="app.js"></script>
</body>
</html>
  }
  return base;
}# agente-n1-cpc
