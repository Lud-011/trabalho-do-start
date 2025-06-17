# trabalho-do-start 2
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Análise de Palavras-Chave</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            line-height: 1.6;
        }
        .container {
            display: flex;
            gap: 20px;
        }
        .group {
            flex: 1;
            padding: 15px;
            border-radius: 8px;
            background-color: #f5f5f5;
        }
        h2 {
            color: #333;
            border-bottom: 2px solid #ddd;
            padding-bottom: 10px;
        }
        .word {
            margin: 5px 0;
            padding: 5px;
            background-color: white;
            border-radius: 4px;
            display: flex;
            justify-content: space-between;
        }
        .frequent {
            border-left: 4px solid #4CAF50;
        }
        .less-frequent {
            border-left: 4px solid #2196F3;
        }
        textarea {
            width: 100%;
            height: 150px;
            margin-bottom: 15px;
            padding: 10px;
            border-radius: 4px;
            border: 1px solid #ddd;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Análise de Palavras-Chave</h1>
    
    <textarea id="texto" placeholder="Cole seu texto aqui..."></textarea>
    <button onclick="analisarTexto()">Analisar Texto</button>
    
    <div class="container">
        <div class="group">
            <h2>Palavras Mais Frequentes</h2>
            <div id="frequentes"></div>
        </div>
        <div class="group">
            <h2>Palavras Menos Frequentes</h2>
            <div id="menos-frequentes"></div>
        </div>
    </div>

    <script>
        // Dicionário para unificação de palavras
        const palavrasBase = {
            'aluno': ['aluna', 'alunos', 'alunas'],
            'aprender': ['aprendendo', 'aprendi', 'aprenderam'],
            'professor': ['professora', 'professores', 'professoras'],
            'curso': ['cursos'],
            'linguagem': ['linguagens'],
            'programar': ['programação', 'programador', 'programadora']
        };

        // Função para normalizar palavras
        function normalizarPalavra(palavra) {
            // Remove pontuação
            palavra = palavra.toLowerCase().replace(/[.,!?;:"]/g, '');
            
            // Verifica se a palavra tem uma forma base definida
            for (const [base, variacoes] of Object.entries(palavrasBase)) {
                if (variacoes.includes(palavra) || palavra === base) {
                    return base;
                }
            }
            
            return palavra;
        }

        // Função para analisar o texto
        function analisarTexto() {
            const texto = document.getElementById('texto').value;
            const palavras = texto.split(/\s+/);
            
            // Contagem de palavras normalizadas
            const contagem = {};
            
            palavras.forEach(palavra => {
                const normalizada = normalizarPalavra(palavra);
                if (normalizada.length > 2) { // Ignora palavras muito curtas
                    contagem[normalizada] = (contagem[normalizada] || 0) + 1;
                }
            });
            
            // Converter para array e ordenar
            const palavrasOrdenadas = Object.entries(contagem)
                .sort((a, b) => b[1] - a[1]);
            
            // Determinar o ponto de corte para frequência
            const metade = Math.floor(palavrasOrdenadas.length / 2);
            const frequentes = palavrasOrdenadas.slice(0, metade);
            const menosFrequentes = palavrasOrdenadas.slice(metade);
            
            // Exibir resultados
            exibirResultados(frequentes, 'frequentes');
            exibirResultados(menosFrequentes, 'menos-frequentes');
        }

        // Função para exibir os resultados
        function exibirResultados(palavrasContagem, elementoId) {
            const container = document.getElementById(elementoId);
            container.innerHTML = '';
            
            palavrasContagem.forEach(([palavra, count]) => {
                const div = document.createElement('div');
                div.className = `word ${elementoId === 'frequentes' ? 'frequent' : 'less-frequent'}`;
                div.innerHTML = `<span>${palavra}</span> <span>${count}x</span>`;
                container.appendChild(div);
            });
        }
    </script>
</body>
</html>
