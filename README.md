# trabalho-do-start
function filtrarPalavrasChave(texto, palavrasChave) {
    // 1. Converter texto para minúsculas e dividir em palavras
    const palavras = texto.toLowerCase().split(/\s+/);
    
    // 2. Objeto para armazenar a contagem
    const contagem = {};
    
    // 3. Filtrar e contar palavras-chave
    palavras.forEach(palavra => {
        // Verificar se a palavra está na lista de palavras-chave
        if (palavrasChave.map(p => p.toLowerCase()).includes(palavra)) {
            contagem[palavra] = (contagem[palavra] || 0) + 1;
        }
    });
    
    // 4. Ordenar por frequência (mais frequentes primeiro)
    const resultado = Object.entries(contagem)
        .sort((a, b) => b[1] - a[1])
        .map(([palavra, count]) => ({ palavra, count }));
    
    return resultado;
}

// Exemplo de uso:
const texto = "JavaScript é uma linguagem poderosa. Start é uma ótima plataforma para aprender JavaScript. javascript e Start são excelentes!";
const palavrasChave = ["JavaScript", "Start"];

const palavrasFiltradas = filtrarPalavrasChave(texto, palavrasChave);
console.log(palavrasFiltradas);

/* Saída:
[
    { palavra: "javascript", count: 3 },
    { palavra: "start", count: 2 }
]
*/
