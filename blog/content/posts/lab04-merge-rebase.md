---
title: "Laboratório 4 — Merge, Rebase e Conflitos"
date: 2026-11-05
draft: false
---

**Repositório:** [lab4-merge-rebase](https://github.com/Engenharia-de-Software-com-Devops/lab4-merge-rebase) · **Aula 4 (05/11/2026)**

## O que é este laboratório

O que fazer quando duas pessoas (ou duas branches) mexem **na mesma linha do mesmo arquivo**. O Git não adivinha qual versão vale: ele para e pede uma decisão. Também comparei as duas formas de integrar trabalho: **merge** e **rebase**.

## Conceitos

- **Merge com commit de merge:** quando as duas branches andaram, o Git cria um commit novo com **dois pais**, que junta as duas linhas.
- **Conflito:** as duas branches alteraram o mesmo trecho. O Git marca o arquivo assim:

```text
<<<<<<< HEAD
versao=3
=======
versao=2
>>>>>>> feature
```

Resolver significa editar o arquivo deixando o conteúdo certo, remover os marcadores e rodar `git add` + `git commit`.

- **`git merge --abort` / `git rebase --abort`:** desistem da operação e voltam ao estado anterior.
- **Rebase:** em vez de juntar, **reaplica** os commits da branch em cima da `main` atual. Resultado: histórico linear, com hashes novos.

## O que foi feito

**Conflito de configuração.** Na `main`, `config.txt` virou `versao=3`; numa branch, `versao=2`. O merge parou em conflito. Escolhi a versão final manualmente e registrei a resolução.

**Conflito de Pull Request.** A descrição do projeto no README foi alterada de formas diferentes na `main` e na branch do PR. O conflito foi resolvido **localmente** e enviado de volta.

```text
*   4aa4655 merge: resolve conflito do pull request
|\
| * 32401af docs: atualiza descricao principal
* | 90aa066 docs: altera descricao do projeto
|/
* a08e271 chore: adiciona atualizacao na main
*   70e712c merge: resolve conflito de configuracao
|\
| * 4e7c2e0 feat: atualiza versao para 2
* | b9656df chore: atualiza versao para 3
|/
* 0e0985f chore: adiciona configuracao inicial
```

As "bolhas" no grafo são os pontos em que as linhas divergiram e foram reunidas por um commit de merge.

## Merge × Rebase

| | Merge | Rebase |
|---|---|---|
| Histórico | Mostra a divergência (bolhas) | Linear |
| Commits existentes | Preservados | Reescritos (hashes novos) |
| Seguro em branch compartilhada? | Sim | **Não**: só em branch local/pessoal |

## O que aprendi

- Conflito não é erro, é o Git pedindo uma decisão humana.
- Ler os marcadores `<<<<<<<`, `=======` e `>>>>>>>` com calma resolve 90% dos casos.
- `--abort` é a saída de emergência: dá para sempre voltar atrás.
- Rebase deixa o histórico bonito, mas reescreve commits; nunca usar em branch que outros já baixaram.
