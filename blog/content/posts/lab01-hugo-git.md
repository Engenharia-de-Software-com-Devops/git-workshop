---
title: "Laboratório 1 — Blog com Hugo, Markdown e Git"
date: 2026-10-22
draft: false
---

**Repositório:** [git-workshop](https://github.com/Engenharia-de-Software-com-Devops/git-workshop) · **Aula 1 (22/10/2026)**

## O que é este laboratório

O primeiro contato com controle de versão. A ideia foi criar um projeto real, este blog, e versionar cada passo com Git. No fim, todo `push` na `main` publica o site automaticamente no GitHub Pages.

## Conceitos

- **Repositório:** pasta cujo histórico o Git acompanha (fica em `.git/`).
- **Commit:** foto (snapshot) do projeto num momento, com autor, data e mensagem.
- **Staging area (index):** "rascunho" do próximo commit; você escolhe o que entra com `git add`.
- **Hugo:** gerador de site estático. Transforma arquivos Markdown em HTML.

## O que foi feito, passo a passo

1. **README e primeiros commits.** O repositório começou com um `README.md`, evoluído em três commits pequenos (inicial, seção de uso e seção de instalação). Cada commit tem uma intenção só.
2. **Criação do blog.** Site Hugo dentro de `blog/`: `hugo.toml` (configuração), `layouts/all.html` (layout único, sem tema externo) e o primeiro post, `content/posts/hello-world.md`.
3. **Deploy automático.** Um workflow do GitHub Actions (`.github/workflows`) compila o Hugo e publica no Pages a cada push.
4. **Conteúdo e estilo.** Seção "sobre mim" no post e CSS com tema claro e escuro.

```text
eec29c1 style: add base styling to blog layout
27436d1 docs: add about me section to hello-world post
9ce2ea0 ci: deploy Hugo blog to GitHub Pages
259fe68 feat: add Hugo blog with hello-world post
9b58378 Adiciona seção de instalação
fdce99e Adiciona seção de uso
93cb636 Adiciona README inicial
```

## Resultado

Este blog no ar em <https://engenharia-de-software-com-devops.github.io/git-workshop/>, com histórico organizado. Os commits seguem o padrão *Conventional Commits* (`feat:`, `docs:`, `ci:`, `style:`).

## O que aprendi

- O ciclo básico: **editar → `git add` → `git commit` → `git push`**.
- Commits pequenos e com mensagem clara deixam o histórico legível.
- Automatizar a publicação (CI/CD) tira o trabalho manual e o erro humano do deploy.
