# Runtime de Tool Calling

Página estática usada na disciplina de Tecnologias Emergentes para executar o ciclo
completo de Tool Calling dentro do navegador.

O aluno declara o JSON Schema de uma ferramenta, escreve o corpo da função em
JavaScript, digita um pedido e observa as cinco etapas do ciclo:

1. A aplicação envia o pedido e o schema para o modelo.
2. O modelo devolve a chamada de ferramenta, com nome e argumentos.
3. A aplicação executa a função localmente.
4. A aplicação devolve o resultado ao modelo.
5. O modelo produz a resposta final.

O Google AI Studio e o Groq Console param na etapa 2 e deixam o resto por conta de
quem hospeda o modelo. Esta página faz esse trabalho e mostra cada passo rotulado
com a caixa correspondente do diagrama da aula.

## Provedores

Funciona com Groq e com a API do Gemini. As duas aceitam chamada direta do
navegador, então a página não tem servidor próprio.

O mesmo JSON Schema serve para os dois. O Groq embrulha em
`{ "type": "function", "function": { ... } }` e o Gemini em
`{ "functionDeclarations": [ ... ] }`.

## Chave de API

A chave fica no `localStorage` do navegador de quem usa a página, sai dali apenas
nas requisições para o provedor escolhido e pode ser apagada pelo botão da própria
tela. A página é estática e não tem para onde enviar a chave.

Modelos padrão: `llama-3.3-70b-versatile` no Groq e `gemini-3.6-flash` no Gemini.
O campo de modelo é editável, porque provedor troca nome de modelo com frequência.

## Execução local

Qualquer servidor estático serve:

```bash
python3 -m http.server 8000
```

## Publicação

O workflow em `.github/workflows/deploy.yml` publica no GitHub Pages a cada push
na branch `main`.
