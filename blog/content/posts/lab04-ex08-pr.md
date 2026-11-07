---
title: "Laboratório 4 — Conflito de PR na interface web"
date: 2026-11-05
draft: false
---

**Repositórios:** [lab4-ex08-pr-interface](https://github.com/Engenharia-de-Software-com-Devops/lab4-ex08-pr-interface) · [lab4-ex08-pr-interface-v2](https://github.com/Engenharia-de-Software-com-Devops/lab4-ex08-pr-interface-v2) · **Exercício 8 do Lab 4**

## O que é este exercício

O mesmo problema do conflito, só que **sem terminal**: resolver um conflito de Pull Request direto pelo editor do GitHub (botão *Resolve conflicts*).

## Cenário montado

1. `base: titulo`: arquivo `pagina.txt` com um título.
2. Branch `feature/titulo-web`: `feat: titulo da turma` muda o título.
3. Na `main`: `main: titulo do curso` muda **a mesma linha** de outro jeito.
4. PR aberto de `feature/titulo-web` para `main`. O GitHub avisa: *This branch has conflicts that must be resolved*.

## Resolução pela interface

O editor web mostra os mesmos marcadores do terminal (`<<<<<<<`, `=======`, `>>>>>>>`). Editei o texto final, cliquei em *Mark as resolved* e depois em *Commit merge*. O GitHub cria o commit `Merge branch 'main' into feature/titulo-web` **na branch do PR**, e o PR fica liberado para o merge.

```text
*   dff7923 Merge pull request #1 from .../feature/titulo-web
|\
| *   c858fc5 Merge branch 'main' into feature/titulo-web   <- resolução na web
| |\
| |/
|/|
* | d7de4f0 main: titulo do curso
| * 677ae19 feat: titulo da turma
|/
* 86bc33b base: titulo
```

**1ª tentativa ([lab4-ex08-pr-interface](https://github.com/Engenharia-de-Software-com-Devops/lab4-ex08-pr-interface)):** a resolução manteve só `titulo=Blog do curso`, e o PR foi integrado na `main` (`dff7923`). Os dois pontos contrariam o enunciado, que pede o texto combinado e o PR **não** integrado.

**2ª tentativa ([lab4-ex08-pr-interface-v2](https://github.com/Engenharia-de-Software-com-Devops/lab4-ex08-pr-interface-v2)), que cumpre o enunciado:**

| Critério | Resultado |
|---|---|
| Texto na branch do PR | `titulo=Blog da turma e do curso` |
| Commit de resolução | `def5254`, com dois pais (feat + main) |
| `main` | continua `titulo=Blog do curso` |
| PR | sem conflito e **aberto de propósito** |

**Clicar em _Commit merge_ conclui o PR?** Não. Ele só registra a resolução na branch de origem. Integrar na `main` é outro clique (*Merge pull request*), e foi justamente esse clique a mais que tornou a 1ª tentativa diferente do pedido.

## O que aprendi

- Resolver pela web é cômodo para conflitos pequenos de texto.
- O GitHub resolve trazendo a `main` **para dentro da branch do PR** (um merge no sentido inverso) e só depois integra o PR.
- Para conflitos grandes ou em código que precisa ser testado, resolver localmente continua sendo melhor.
