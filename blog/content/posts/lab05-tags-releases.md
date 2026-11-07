---
title: "Laboratório 5 — Tags, releases e histórico"
date: 2026-11-06
draft: false
---

**Repositório:** [lab5-tags-releases](https://github.com/Engenharia-de-Software-com-Devops/lab5-tags-releases) · **Aula 5 (06/11/2026)**

## O que é este laboratório

Publicar o blog num repositório remoto, marcar uma **versão** com tag e praticar as duas formas mais comuns de **desfazer** commits: `reset --soft` e `revert`.

## Conceitos

- **Remote (`origin`):** apelido para a URL do repositório no GitHub.
- **Upstream:** a branch remota que a local acompanha. Com ela, `git push` e `git pull` funcionam sem argumentos.
- **Tag anotada:** etiqueta permanente numa versão, com autor, data e mensagem.
- **`reset --soft`:** volta o ponteiro da branch e **mantém as alterações staged**.
- **`revert`:** cria um **novo commit** que desfaz outro, sem apagar nada.

## Passo a passo

**1. Conectar e enviar.**

```text
$ git remote add origin https://github.com/.../lab5-tags-releases.git
$ git push -u origin main
branch 'main' set up to track 'origin/main'.
$ git branch -vv
* main d932135 [origin/main] feat: add professional profile post
```

**2. Clonar.** `git clone ... blog-clonado` criou uma cópia completa, com `origin` já configurado.

**3. Tag v1.0.**

```text
$ git tag -a v1.0 -m "Primeira versão publicada do blog"
$ git push origin v1.0          # tags NÃO sobem com git push comum
 * [new tag]         v1.0 -> v1.0
```

**4. `reset --soft`, para corrigir um commit ainda não publicado.** Numa branch `test-reset`, adicionei "Também gosto de jogar basquete." e commitei com uma mensagem ruim. Depois:

```text
$ git reset --soft HEAD~1
$ git status
Mudanças a serem submetidas:
	modified:   content/posts/hobbies.md     # alteração preservada
$ git commit -m "feat: adiciona basquete à seção de esportes"
```

O commit antigo (`5c66d7f`) sumiu da branch e foi substituído por um com mensagem melhor.

**5. `revert`, para desfazer com segurança.**

```text
ddb77f6 Revert "feat: adiciona basquete à seção de esportes"
daa99e9 feat: adiciona basquete à seção de esportes
```

Os dois commits ficam no histórico: um adiciona a linha e o outro a remove.

**6. Limpar.** `git branch -d test-reset` **recusou** (*not fully merged*), o que é uma proteção contra perda de trabalho. Como a branch era só treino, usei `-D`.

## Resultado

`main` limpa, tag `v1.0` local e no GitHub, `test-reset` removida.

## Quando usar cada um

| Situação | Use |
|---|---|
| Commit errado, **ainda não enviei** | `git reset --soft HEAD~1` |
| Commit errado, **já está no GitHub** | `git revert <hash>` |
| Marcar uma versão entregue | `git tag -a vX.Y -m "..."` + `git push origin vX.Y` |

## O que aprendi

- `reset` reescreve o histórico; `revert` acrescenta ao histórico. Em código compartilhado, **revert**.
- Tags precisam de push explícito.
- O Git protege branches não integradas; forçar com `-D` exige ter certeza.
