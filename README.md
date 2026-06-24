# FoldQuest 🧬 — Aprenda AlphaFold de verdade

App web interativo e gamificado para aprender **tudo sobre o AlphaFold**, do zero ao avançado,
projetado para **retenção de longo prazo** usando técnicas comprovadas de ciência da aprendizagem.

## Idiomas (PT / EN)

O app é bilíngue. Há um botão **PT / EN** no canto superior direito (ao lado do tema) que
alterna toda a interface E todo o conteúdo (lições, exercícios, legendas dos SVGs, flashcards,
conquistas). O idioma escolhido fica salvo no navegador; o padrão é português.

Para adicionar/editar conteúdo, há dois arrays espelhados: `MODULES_PT` e `MODULES_EN` (mesma
ordem de itens em ambos, para o progresso continuar válido). As strings de interface ficam no
objeto `UI` (`UI.pt` / `UI.en`).

## Como abrir

**Dê duplo-clique em `index.html`.** Pronto. Não precisa instalar nada, não precisa de internet,
não precisa de servidor. Funciona em qualquer navegador moderno.

Seu progresso (XP, streak, conquistas e o agendamento da repetição espaçada) é salvo
automaticamente no próprio navegador (`localStorage`). Para zerar tudo, limpe os dados do site
ou rode `localStorage.removeItem('foldquest_v1')` no console.

## Técnicas de aprendizagem implementadas

| Técnica | Onde está no app |
|---|---|
| **Recordação ativa** | Cada conceito termina com pergunta/recordação, não releitura. |
| **Repetição espaçada (SM-2)** | Cards com intervalos crescentes; tela "Revisão de hoje" mostra o que vence. |
| **Efeito de teste + feedback explicativo** | Quizzes explicam por que a certa é certa e por que as erradas são erradas. |
| **Interleaving** | Na revisão, cards de módulos diferentes são embaralhados. |
| **Scaffolding** | Módulos avançados ficam travados até concluir os pré-requisitos. |
| **Elaboração** | Campo aberto "explique com suas palavras" + resposta-modelo para autoavaliação. |

## Gamificação

XP por acerto/revisão, níveis (100 XP por nível), streak diário, conquistas/badges,
barras de progresso por módulo. Tudo ligado à **maestria**, não ao tempo gasto.

## Tipos de exercício

`lesson` (lição) · `quiz` (múltipla escolha) · `fill` (preencher lacuna) ·
`order` (ordenar arrastando/tocando) · `elaborate` (elaboração) · `flashcard` ·
`diagram` (diagrama clicável do pipeline) · `plddt` (demonstração interativa de confiança).

## Conteúdo

Os **6 módulos** da trilha estão completos, com lições ilustradas (SVG), quizzes, ordenação,
preenchimento, elaboração, flashcards e diagramas:

1. **Fundamentos Biológicos** — proteínas, aminoácidos, níveis de estrutura, forma = função.
2. **O Problema do Enovelamento** — paradoxo de Levinthal (funil de energia), métodos
   experimentais (raios-X, crio-EM, RMN), o abismo sequência×estrutura e o desafio CASP.
3. **AlphaFold 1** — distograma/mapa de contatos, otimização por gradiente (CASP13, 2018).
4. **AlphaFold 2** — MSA/coevolução, representações de MSA e de pares, Evoformer e atenção
   triangular, Structure Module, reciclagem, métricas pLDDT e PAE.
5. **AlphaFold 3** — modelo de difusão, Pairformer e previsão de complexos (ligantes, DNA, RNA).
6. **Impacto e Limitações** — AlphaFold DB (200M+ estruturas), aplicações, Nobel de 2024 e o
   que o AlphaFold NÃO faz.

### Desbloqueio progressivo (scaffolding)

Cada módulo abre quando o anterior é 100% concluído (`requires` no objeto do módulo). Um módulo
já iniciado nunca volta a travar. Para mudar a ordem ou liberar tudo, edite/remova o campo
`requires` de cada módulo.

## Como adicionar conteúdo

Tudo vive no array `MODULES`, no topo do `<script>` em `index.html`. Cada módulo tem `items`
(passos sequenciais). Exemplos de cada tipo:

```js
// Lição
{ type: "lesson", title: "Título", html: `<p>...</p>`,
  srs: { front: "Pergunta do card", back: "Resposta do card" } } // srs é opcional

// Quiz (answer = índice da opção correta, base 0)
{ type: "quiz", q: "Pergunta?", options: ["a","b","c","d"], answer: 1, why: "Explicação." }

// Preencher lacuna (accept = respostas aceitas, sem diferenciar acento/maiúscula)
{ type: "fill", q: "Complete: ___", accept: ["resposta","sinônimo"], hint: "Dica", why: "..." }

// Ordenar (a ordem listada já é a CORRETA; o app embaralha)
{ type: "order", q: "Ordene:", items: ["1º","2º","3º"], why: "..." }

// Elaboração
{ type: "elaborate", q: "Explique...", model: "Resposta-modelo.",
  srs: { front: "...", back: "..." } }

// Flashcard (vira card de repetição espaçada automaticamente)
{ type: "flashcard", front: "Pergunta", back: "Resposta" }

// Diagrama clicável
{ type: "diagram", q: "Clique nas etapas:", nodes: [ {h:"Título", d:"Descrição"}, ... ] }
```

Para **desbloquear** um módulo, mude `locked: true` para `locked: false` no objeto do módulo
e preencha seus `items`. Para criar uma nova badge, adicione ao array `BADGES` e chame
`awardBadge("id")` no ponto apropriado.

## Arquitetura (resumo)

Arquivo único `index.html`: CSS no `<head>` (com variáveis para tema claro/escuro), conteúdo
em `MODULES`, e a lógica em vanilla JS — estado/persistência, XP/níveis, badges, algoritmo
SM-2 de repetição espaçada, e um renderizador por tipo de exercício. Sem dependências externas.

---
Feito para durar. Bons estudos! 🧠
