---
title: "Laboratório 3 — Git Internals (plumbing)"
date: 2026-10-24
draft: false
---

**Repositório:** [lab3-git-internals](https://github.com/Engenharia-de-Software-com-Devops/lab3-git-internals) · **Aula 3 (24/10/2026)**

## O que é este laboratório

Abrir o "capô" do Git. Os comandos do dia a dia (`add`, `commit`, `branch`) são chamados de **porcelain**. Por baixo deles existem os comandos **plumbing**, que manipulam diretamente o banco de objetos. Em 10 exercícios, montei manualmente tudo o que o `git commit` faz sozinho.

## O modelo mental: 4 tipos de objeto

| Objeto | O que guarda | Analogia |
|---|---|---|
| **blob** | O conteúdo de um arquivo (sem o nome!) | Uma página solta |
| **tree** | Lista de nomes → blobs/trees (uma pasta) | O índice de um fichário |
| **commit** | Uma tree + pai + autor + data + mensagem | Uma foto datada do fichário |
| **tag** (anotada) | Aponta um commit + autor + mensagem | Uma etiqueta "versão 1.0" |

Cada objeto é identificado pelo **hash SHA-1 do seu conteúdo** e fica em `.git/objects/`. **Branches e tags leves não são objetos**, são só arquivos de texto em `.git/refs/` com um hash dentro.

## Os exercícios

**1 — O diretório `.git`.** Um único commit gerou 3 objetos (blob, tree, commit). `.git/HEAD` contém `ref: refs/heads/main`, e esse arquivo contém o hash do commit. Ou seja: HEAD → branch → commit.

**2 — Blob.** `git hash-object arquivo.txt` só calcula o hash; com `-w` grava o objeto (a contagem passou de 3 para 4). Um `!` a mais no conteúdo gerou um hash totalmente diferente.

**3 — Deduplicação.** `primeiro.txt` e `segundo.txt` com o mesmo texto tiveram **o mesmo hash** (`90dc552`), então são um único blob. O Git nunca guarda o mesmo conteúdo duas vezes.

**4 — Index sem `git add`.** Com `git update-index --add --cacheinfo 100644,<hash>,manual.txt`, o `git status` passou a mostrar `A manual.txt`. Conclusão: **`git add` = `hash-object -w` + `update-index`**.

**5 — Trees.** `git write-tree` transformou o index numa tree. Com `src/main.py`, apareceu uma **tree dentro da tree** (`040000 tree ... src`). É assim que o Git representa pastas.

**6 — Commit sem `git commit`.** `git commit-tree <tree> -p <pai>` criou um commit válido, **mas a `main` não andou**. Criar o objeto e mover a branch são coisas separadas.

**7 — Referências.** `git update-ref refs/heads/experimento <hash>` criou uma branch, que é literalmente um arquivo de 41 bytes com o hash.

**8 — Tags.** `git cat-file -t` retornou `commit` para a tag leve e `tag` para a anotada, que tem objeto próprio com autor e mensagem. `v1.0.0^{}` "descasca" a tag até o commit.

**9 — Objetos inacessíveis.** `git fsck --unreachable` listou 5 objetos válidos que nenhuma ref alcança (sobras dos exercícios). O `git gc` só os apaga depois de um prazo, o que permite recuperá-los.

**10 — Desafio: histórico só com plumbing.** O repositório publicado foi construído assim:

```bash
BLOB1=$(git hash-object -w app.txt)                       # conteúdo
git update-index --add --cacheinfo 100644,$BLOB1,app.txt  # index
TREE1=$(git write-tree)                                   # snapshot
COMMIT1=$(echo "Primeiro commit" | git commit-tree $TREE1)
git update-ref refs/heads/main $COMMIT1                   # move a branch
# ...mesma coisa para o segundo commit, com -p $COMMIT1...
printf "object $COMMIT2\ntype commit\ntag v1.0.0\n..." | git mktag   # tag anotada
```

```text
6cb0beb (HEAD -> main, tag: v1.0.0) Segundo commit
2f0b4a3 Primeiro commit
$ git fsck --full        # sem erros
```

## O que aprendi

- `git commit` é um atalho para **`write-tree` + `commit-tree` + `update-ref`**.
- **Objetos são imutáveis, referências são móveis.** Isso explica por que "perder" um commit quase nunca é definitivo.
- Arquivos iguais ocupam espaço uma vez só (*content-addressable storage*).
- Entender isso deixa `reset`, `rebase` e `reflog` muito menos misteriosos.

As respostas completas das perguntas de análise dos 10 exercícios estão no README do repositório.
