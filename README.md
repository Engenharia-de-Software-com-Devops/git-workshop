# Laboratório 1 — Blog com Hugo, Markdown e Git

Primeiro laboratório da disciplina: criar um projeto real, este blog, versionando cada passo com Git, e publicá-lo automaticamente no GitHub Pages.

**Site publicado:** https://engenharia-de-software-com-devops.github.io/git-workshop/

O blog também reúne o diário de todos os laboratórios da disciplina, com links para os repositórios de cada um.

## Sumário
1. [Conceitos](#1-conceitos)
2. [Estrutura do repositório](#2-estrutura-do-repositório)
3. [O que foi feito, passo a passo](#3-o-que-foi-feito-passo-a-passo)
4. [Como o deploy funciona](#4-como-o-deploy-funciona)
5. [Executar localmente](#5-executar-localmente)
6. [Evoluções posteriores](#6-evoluções-posteriores)
7. [Revisão rápida](#7-revisão-rápida)

## 1. Conceitos

| Termo | O que é |
|---|---|
| **Repositório** | Pasta cujo histórico o Git acompanha (dados em `.git/`) |
| **Commit** | Snapshot do projeto com autor, data e mensagem |
| **Staging area (index)** | Rascunho do próximo commit; você escolhe o que entra com `git add` |
| **Remote / push** | Cópia do repositório no GitHub; `git push` envia os commits locais |
| **Hugo** | Gerador de site estático: Markdown + layout = HTML |
| **GitHub Actions** | Automação que roda a cada evento (aqui, a cada push) |
| **GitHub Pages** | Hospedagem gratuita de sites estáticos |

## 2. Estrutura do repositório

```
git-workshop/
├── README.md                       # este documento
├── .gitignore                      # ignora public/, resources/ e o lock do Hugo
├── .github/workflows/hugo.yml      # pipeline de build e deploy
└── blog/
    ├── hugo.toml                   # configuração do site (título, idioma, baseURL)
    ├── archetypes/default.md       # modelo usado por `hugo new`
    ├── layouts/all.html            # layout único: CSS, cabeçalho, lista de posts, tema e busca
    └── content/posts/*.md          # um arquivo Markdown por post
```

## 3. O que foi feito, passo a passo

**Parte 1: o ciclo básico do Git com o README.**
```bash
git init
git add README.md && git commit -m "Adiciona README inicial"
# edita README...
git commit -am "Adiciona seção de uso"
git commit -am "Adiciona seção de instalação"
```
Cada commit tem uma intenção só, o que deixa o histórico fácil de ler e de desfazer.

**Parte 2: o blog.**
1. `hugo new site blog`: estrutura do site.
2. `layouts/all.html`: layout próprio (sem tema externo). A home lista `.Site.RegularPages` e as outras páginas mostram `.Content`.
3. `content/posts/hello-world.md`: primeiro post, com *front matter* (`title`, `date`, `draft`).

**Parte 3: publicação automática.** O workflow `.github/workflows/hugo.yml` faz o deploy a cada push na `main` (detalhes na seção 4).

**Parte 4: conteúdo e estilo.** Seção "sobre mim" no post e CSS com tema claro e escuro.

Histórico do laboratório:
```text
eec29c1 style: add base styling to blog layout
27436d1 docs: add about me section to hello-world post
9ce2ea0 ci: deploy Hugo blog to GitHub Pages
259fe68 feat: add Hugo blog with hello-world post
9b58378 Adiciona seção de instalação
fdce99e Adiciona seção de uso
93cb636 Adiciona README inicial
```
Os commits do blog seguem *Conventional Commits*: `feat:` (funcionalidade), `docs:` (texto), `ci:` (automação), `style:` (visual), `chore:` (manutenção).

## 4. Como o deploy funciona

```
git push (main) ──► GitHub Actions
                     ├─ job build:  instala Hugo 0.147.9
                     │              hugo --minify --baseURL <url do Pages>
                     │              envia blog/public como artefato
                     └─ job deploy: publica o artefato no GitHub Pages
```

- `on: push: branches: [main]`: só a `main` publica. `workflow_dispatch` permite rodar o workflow manualmente.
- `permissions: pages: write, id-token: write`: o mínimo necessário para publicar.
- `--baseURL` vem do `configure-pages`. Por isso o `hugo.toml` pode continuar com `localhost`.
- `concurrency: pages`: dois deploys nunca rodam ao mesmo tempo.

Para acompanhar: aba **Actions** do repositório, ou `gh run list`.

## 5. Executar localmente

Requer Hugo >= 0.146.
```bash
cd blog
hugo server        # http://localhost:1313/ com recarregamento automático
hugo new posts/meu-post.md   # cria um post a partir do archetype
```

## 6. Evoluções posteriores

Depois do Lab 1, o blog passou a servir de diário da disciplina:
- Um post didático para cada laboratório (Labs 1 a 5) e um post-índice.
- Tema preto e branco com botão de alternância (◐). A escolha fica salva no `localStorage`.
- Busca nos posts (título e texto inteiro) sem bibliotecas: um filtro em JavaScript no próprio `layouts/all.html`.
- Os posts de exemplo (Hello World, Hobbies, Perfil) foram removidos para o blog focar nos laboratórios.

## 7. Revisão rápida

- [ ] Sei explicar a diferença entre *working directory*, *staging area* e *commit*.
- [ ] Sei o ciclo `editar → git add → git commit → git push`.
- [ ] Sei criar um post: arquivo em `content/posts/` com front matter.
- [ ] Sei por que o site atualiza sozinho (workflow em `.github/workflows/hugo.yml`).

## Referências
- [Hugo — Quick start](https://gohugo.io/getting-started/quick-start/)
- [Hugo — Host on GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/)
- [Pro Git — Getting Started](https://git-scm.com/book/pt-br/v2)
