---
layout: post
title: "O que saber sobre Conventional Commits?"
author: "Renê Soares"
categories: programação
tags: [git, projetos, automação]
image: conventionalcommits.png
---

## História

O conventional commits baseia-se na documentação de contribuição do projeto [Angular](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#commit). A equipe de desenvolvimento submeteu esse documento no dia _11 de Março de 2015_ e vem sendo utilizado até os dias de hoje no projeto do angular e em muitos outros.

## Como funciona?

O conventional commits é uma padronização de como escrever commits. Ela fornece um conjunto de regras que facilita a leitura do histórico de commits e torna fácil a criação de changelogs baseados nos recursos do [SemVer](https://semver.org/lang/pt-BR/).

O commit deve ser estruturado da forma abaixo:

```
<tipo>[escopo opcional]:

<descrição> [corpo opcional]

[rodapé opcional]
```

Os commits contém os seguintes elementos estruturais para comunicar as mudanças da biblioteca:

1. **fix**: um commit do tipo `fix` corrige um bug em sua base de código (isso se correlaciona com o `PATCH` controle de versão semântica).
2. **feat**: um commit do tipo `feat` introduz um novo recurso na base de código (isso se correlaciona com o `MINOR` (controle de versão semântica).
3. **BREAKING CHANGE**: um commit que tem um rodapé `BREAKING CHANGE:`, ou anexar um `!` após o tipo/escopo, introduz uma mudança de API (correlacionando com o `MAJOR` (controle de versão semântica). Uma `BREAKING CHANGE` pode fazer parte de commits de qualquer tipo.
4. _tipos_ diferentes de `fix:` e `feat:` são permitidos, por exemplo `build:`, `chore:`, `ci:`, `docs:`, `style:`, `refactor:`, `perf:`, `test:` e outros.
5. _rodapés_ diferentes dos que `BREAKING CHANGE: <description>` podem ser fornecidos.

Tipos adicionais não são obrigatórios pela especificação e não têm efeito implícito no Controle de Versão Semântica (a menos que incluam uma BREAKING CHANGE). Um escopo pode ser fornecido para um tipo de confirmação, para fornecer informações contextuais adicionais e está contido entre parênteses.

## Por que usar conventional commits?

* Gerando automaticamente _CHANGELOGs_.
* Determinar automaticamente um aumento de versão semântica (com base nos tipos de commits recebidos).
* Comunicar a natureza das mudanças aos colegas de equipe, ao público e a outras partes interessadas.
* Acionando processos de compilação e publicação.
* Tornando mais fácil para as pessoas contribuírem com os projetos, permitindo que elas explorem um histórico de commits mais estruturado.

## Conclusão

Principalmente para mim que trabalho com desenvolvimento de aplicações mobile o conventional commits juntamente com o _SemVer_ automatiza a geração do changelog para submisão de versão nas loja.
