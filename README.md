# SnakeOnCommits

Transforme o histórico de commits do seu repositório GitHub em um jogo visual e interativo com a clássica cobrinha. Este repositório contém o código necessário para adicionar a animação da cobrinha ao seu README, caçando os commits do seu repositório.

## 📜 Sobre o Projeto

Este projeto utiliza o GitHub Actions para gerar uma animação dinâmica da cobrinha baseada no seu histórico de commits. A animação é renderizada e exibida diretamente no README do seu repositório, permitindo uma interação visual divertida com seus dados de contribuição.

## 🚀 Como Funciona?

1. Uma ação do GitHub é configurada para gerar uma animação no formato SVG.
2. A animação mostra a cobrinha "caçando" os commits com base nos dados do gráfico de contribuições.
3. O arquivo SVG é atualizado automaticamente e exibido no README do repositório.


## 📝 Como configurar
Siga os passos abaixo para configurar a animação da cobrinha no seu repositório:
#### Adicionar o trecho ao README
1. Copie e cole o código abaixo no arquivo README.md do seu repositório.
```
<picture align="center">
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/**SEU-USUARIO**/**SEU-USUARIO**/output/github-contribution-grid-snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/**SEU-USUARIO**/**SEU-USUARIO**/output/github-contribution-grid-snake-dark.svg">
  <img align="center" alt="github contribution grid snake animation" src="https://raw.githubusercontent.com/**SEU-USUARIO**/**SEU-USUARIO**/output/github-contribution-grid-snake.svg">
</picture>
```

2. Substitua o texto **SEU-USUARIO** pelo seu nome de usuário no GitHub;
3. Salve as alterações e faça o commit do arquivo.

#### Acessar as Actions
1. Após salvar e enviar o README.md, acesse a aba Actions no repositório do GitHub;
2. Clique no botão **Set up a workflow yourself**;  
3. Na tela de edição, cole o seguinte código no arquivo main.yml:
```
name: gerar animação

on:
  # executa automaticamente a cada 24 horas
  schedule:
    - cron: "0 */24 * * *"
  
  # permite executar o workflow manualmente a qualquer momento
  workflow_dispatch:
  
  # executa em cada push na branch master
  push:
    branches:
    - master

jobs:
  generate:
    permissions: 
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 5
    
    steps:
      # gera um jogo da cobrinha a partir do gráfico de contribuições de um usuário do GitHub (<github_user_name>),
      # e cria uma animação SVG em <svg_out_path>
      - name: gerar github-contribution-grid-snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
          
      # envia o conteúdo de <build_dir> para uma branch
      # o conteúdo estará disponível em https://raw.githubusercontent.com/<github_user>/<repository>/<target_branch>/<file>, 
      # ou como uma página do GitHub
      - name: enviar github-contribution-grid-snake.svg para a branch de saída
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
4. Clique em **Start Commit**, selecione a opção **Commit directly to the main branch** e confirme.

#### Executar o Workflow
1. Após configurar o workflow, ele será executado automaticamente uma vez a cada 24 horas;
2. Você também pode executar manualmente:
    - Acesse a aba **Actions**;
    - Clique no workflow gerar animação;
    - Selecione a opção **Run workflow**.
