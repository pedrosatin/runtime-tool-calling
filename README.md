# Runtime de tool calling

Página estática que fecha o ciclo completo de tool calling dentro do navegador. O aluno declara a
ferramenta, escreve o corpo da função em JavaScript e a própria página executa esse corpo com os
argumentos que o modelo escolheu, devolve o retorno e mostra a resposta final.

No AI Studio e no playground do Groq o ciclo para na chamada, porque não existe onde escrever a
função. Aqui os cinco passos aparecem em duas raias, à esquerda o que roda na máquina do aluno e à
direita as decisões do modelo.

## Provedores e modelos

Google AI Studio é o padrão. Os modelos abaixo responderam com chamada de ferramenta em teste direto
contra a API em 03/09/2026.

| Modelo | Observação |
| --- | --- |
| `gemini-3.5-flash` | padrão da página, entre 1,3 s e 2,5 s por chamada |
| `gemini-3.7-flash` | responde em 3 s a 7 s |
| `gemini-3.5-flash-lite` | menor latência |
| `gemini-3.1-flash-lite` | alternativa leve |
| `gemini-flash-lite-latest` | apelido sempre atualizado |
| `gemini-3-flash-preview` | preview |
| `gemini-3.6-flash` | devolveu 503 em um terço das tentativas |
| `gemini-3.8-flash` | devolveu 503 em todas as tentativas |
| `gemini-pro-latest` | cota gratuita bem menor, devolve 429 com facilidade |

Modelos da família 2.5 devolvem 404 em contas criadas recentemente, com a mensagem `no longer
available to new users`.

O nível gratuito do AI Studio limita a 20 requisições por dia em cada modelo, contadas separadamente
por modelo. Um ciclo completo gasta 2 requisições, o que dá 10 execuções por modelo por dia. Ao
esgotar, trocar o modelo no bloco 1 devolve um saldo novo. A página traduz o 429 e diz isso na tela.

No Groq a página lista `llama-3.3-70b-versatile`, `llama-3.1-8b-instant`, `openai/gpt-oss-120b` e
`openai/gpt-oss-20b`.

O botão `Buscar` consulta o endpoint de modelos do provedor com a chave informada e substitui a
lista pelos modelos liberados para aquela conta.

## Entrega da atividade

Depois de um ciclo concluído a página gera um código de verificação de 8 caracteres, calculado com
SHA-256 sobre a declaração, o corpo da função, o pedido e todos os passos executados. Dois alunos
que entregarem o mesmo trabalho recebem o mesmo código.

`Copiar relatório` coloca na área de transferência o texto completo da execução, pronto para colar
no formulário. `Baixar .md` grava o mesmo conteúdo em arquivo.

## Realce de sintaxe

Os dois campos de edição têm realce próprio, sem biblioteca externa. Um `pre` colorido fica atrás de
um `textarea` de texto transparente, com fonte, espaçamento e recuo idênticos. Os payloads JSON da
trilha usam o mesmo tokenizador.

## Cenários

Quatro botões carregam declaração, função e pedido de uma vez: ciclo completo, declaração opaca,
falta de argumento obrigatório e pedido fora do escopo da ferramenta.

## Chave de API

A chave fica no `localStorage` do navegador e segue apenas para o provedor escolhido. Não existe
servidor intermediário. O botão `Apagar chave` remove o valor guardado.

## Publicação

O workflow em `.github/workflows/deploy.yml` publica no GitHub Pages a cada push na `main`.
