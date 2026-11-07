---
title: "Laboratório 2 — Branches e fast-forward"
date: 2026-10-23
draft: false
---

**Repositório:** [lab2-branches-fast-forward](https://github.com/Engenharia-de-Software-com-Devops/lab2-branches-fast-forward) · **Aula 2 (23/10/2026)**

## O que é este laboratório

Trabalhar em **branches**, linhas de desenvolvimento isoladas, e integrá-las à `main` do jeito mais simples possível: o **fast-forward**. Também foi configurado o `.gitignore` para o Git não acompanhar arquivos temporários.

## Conceitos

- **Branch:** só um ponteiro (um arquivo com um hash) para um commit. Criar branch é instantâneo e barato.
- **HEAD:** indica em qual branch você está.
- **Fast-forward:** quando a `main` não andou desde que a branch foi criada, o merge só "empurra" o ponteiro da `main` para frente. Não nasce nenhum commit de merge.
- **`--ff-only`:** pede ao Git "só faça o merge se der fast-forward; senão, recuse".

## Parte 1 — Branch `feature`

```bash
git branch feature        # cria o ponteiro, mas continuo na main
git switch feature        # agora HEAD -> feature
# acrescento "Nova funcionalidade" no README.md
git commit -am "Adiciona nova funcionalidade"
```

```text
* 8b7e11b (HEAD -> feature) Adiciona nova funcionalidade
* 84c5154 (main) Commit inicial
```

De volta à `main`, a linha nova **não existe** (`grep -c` retornou `0`). Ela está só no commit da `feature`. Depois do merge:

```text
$ git merge --ff-only feature
Updating 84c5154..8b7e11b
Fast-forward
* 8b7e11b (HEAD -> main, feature) Adiciona nova funcionalidade
```

As duas branches passaram a apontar para o **mesmo commit**. Então `git branch -d feature` apagou a branch sem reclamar, porque o trabalho já estava na `main`.

## Parte 2 — Um post por branch

O mesmo fluxo, aplicado ao blog:

| Branch | Arquivo criado | Merge |
|---|---|---|
| `perfil-profissional` | `content/posts/perfil-profissional.md` | `8b7e11b..ba93eb4 Fast-forward` |
| `hobbies` | `content/posts/hobbies.md` | `ba93eb4..88608a7 Fast-forward` |

O detalhe importante: a branch `hobbies` foi criada **depois** de integrar o perfil, a partir da `main` já atualizada. Por isso o segundo merge também foi fast-forward.

## Parte 3 — `.gitignore`

```text
$ git status --short --untracked-files=all
?? teste-lab2.tmp                         # Git "vê" o arquivo
# adiciono a regra *.tmp no .gitignore
$ git check-ignore -v teste-lab2.tmp
.gitignore:4:*.tmp	teste-lab2.tmp      # arquivo:linha:regra que ignorou
```

O `.tmp` continua na pasta, mas o Git o ignora. Só o `.gitignore` foi commitado.

## Resultado

```text
* c13f122 (HEAD -> main) Ignora arquivos temporarios
* 88608a7 Adiciona post de hobbies
* ba93eb4 Adiciona post de perfil profissional
* 8b7e11b Adiciona nova funcionalidade
* 84c5154 Commit inicial
```

Histórico **linear**, só a `main` e área de trabalho limpa.

## O que aprendi

- Branch não copia arquivos, é só um ponteiro. Por isso trocar de branch é rápido.
- O fast-forward mantém o histórico limpo quando não há trabalho paralelo.
- Criar a próxima branch a partir da `main` **atualizada** evita merges desnecessários.
- `git check-ignore -v` é o jeito de descobrir *por que* um arquivo está sendo ignorado.
