---
title: "Diário dos laboratórios — Engenharia de Software com DevOps"
date: 2026-11-07
draft: false
---

Resumo da trilha de laboratórios da disciplina de controle de versão (turma 04). Cada laboratório tem um repositório próprio com **README didático**, todos os comandos executados, as saídas reais e um log completo em `docs/`.

| Aula | Laboratório | Tema | Post | Repositório |
|---|---|---|---|---|
| 1 | Lab 1 | Git básico + blog Hugo + deploy | [ler](../lab01-hugo-git/) | [git-workshop](https://github.com/Engenharia-de-Software-com-Devops/git-workshop) |
| 2 | Lab 2 | Branches, fast-forward, `.gitignore` | [ler](../lab02-branches/) | [lab2-branches-fast-forward](https://github.com/Engenharia-de-Software-com-Devops/lab2-branches-fast-forward) |
| 3 | Lab 3 | Git Internals: plumbing | [ler](../lab03-git-internals/) | [lab3-git-internals](https://github.com/Engenharia-de-Software-com-Devops/lab3-git-internals) |
| 4 | Lab 4 | Merge, rebase e conflitos | [ler](../lab04-merge-rebase/) | [lab4-merge-rebase](https://github.com/Engenharia-de-Software-com-Devops/lab4-merge-rebase) |
| 4 | Lab 4 · Ex. 8 | Conflito de PR na interface web | [ler](../lab04-ex08-pr/) | [lab4-ex08-pr-interface](https://github.com/Engenharia-de-Software-com-Devops/lab4-ex08-pr-interface) |
| 5 | Lab 5 | Remote, tags, `reset --soft`, `revert` | [ler](../lab05-tags-releases/) | [lab5-tags-releases](https://github.com/Engenharia-de-Software-com-Devops/lab5-tags-releases) |

## A linha de raciocínio

1. **Lab 1:** registrar o trabalho (commit) e publicar.
2. **Lab 2:** trabalhar em paralelo sem bagunçar a `main` (branches).
3. **Lab 3:** entender *como* o Git guarda tudo (objetos e referências).
4. **Lab 4:** juntar trabalhos que divergiram (merge, rebase, conflitos).
5. **Lab 5:** compartilhar, versionar e desfazer com segurança (remote, tags, reset, revert).

Cada passo usa o anterior: depois do Lab 3, por exemplo, fica claro que uma branch é só um arquivo com um hash. Isso explica por que criar branches (Lab 2) é barato e por que um `reset` (Lab 5) não apaga commits de verdade.
