---
layout: post
title: "Entendendo Scopes"
author: "Renê Soares"
categories: programação
tags: [javascript, estudo, básico]
image: image-post-keep-calm.jpeg
---

E ai pessoal, tudo certinho? Esse é o primeiro post de uma série que aborda os conceitos básicos do Javascript e nesse post vou falar de scopes seus conceitos e aplicações. Vamos a mão na massa? Uma das primeiras coisas vou falar sobre Javascript é que basicamente tudo gera um escopo e que as vezes não fica muito intuitivo aplicação do mesmo. Existem 2 tipos de escopos: global e local, vou começar a falar do escopo global, esse tipo de escopo está acessível em todos os scripts do que você utilizar e um exemplo dele é o objeto window. Tudo que for declarado dentro do objeto window é uma global e pode ser acessada a qualquer momento e de qualquer parte do código, pois no Javascript os elementos filhos herdam os escopos dos pais. Eu sei que fica meio complicado entender a teoria então vamos para a prática!!!

Vou começar declarando uma variável com pessoa com 2 propriedades nome e idade, depois vou criar uma função com o nome iniciar e chamar a função logo abaixo.

~~~javascript
var PESSOA = {
  nome: "Valdir",
  idade: 24
};

function iniciar() {
  console.log(PESSOA);
};

iniciar();
~~~

Perceba que quando o código for executado o retorno dele vai ser um objeto: Object {idade: 24, nome: “Valdir”}. Até agora nenhum mistério, mas veja a diferença quando eu crio outra variável dentro da função e dou um console.log fora da mesma:

~~~javascript
var PESSOA = {
  nome: "Valdir",
  idade: 24
};

function iniciar() {
  var ocupacao = "Manutenção"
  console.log(PESSOA, ocupacao);
};

iniciar();

console.log(ocupacao);
~~~

Veja que agora ele mostrou o objeto e depois deu erro. Isso acontece por que a variável ocupacao somente existe no escopo da função. Tá e como eu faço para acessar ela dentro do meu script e fora da função? É só declara a variável fora da função:

~~~javascript
var PESSOA = {
  nome: "Valdir",
  idade: 24
};

var ocupacao = "Programador";
function iniciar() {
  var ocupacao = "Manutenção"
  console.log(PESSOA, ocupacao);
};

iniciar();
console.log(ocupacao);
~~~

Quando você rodar o script vai perceber que a variável ocupacao tem dois valores diferentes, um é Manutenção e o outro é Programador. Isso ocorre devido a criação da variável com a palavra reservada var e fora da função, caso queria sobrescrever o valor da variável basta retirar o var do começo da mesma (Isso já me deu tanto problema que vocês nem sabem kkkkk).

Ficamos por aqui pessoal espero que tenham gostado conteúdo, qualquer pergunta só colocar aqui nos comentários, vou criar um repositório no [github](https://medium.com/@reenesoares/entendendo-scopes-3972c9280ddf#:~:text=um%20reposit%C3%B3rio%20no-,github,-para%20essa%20s%C3%A9rie) para essa série de posts e logo logo vai sair outro post para vocês.