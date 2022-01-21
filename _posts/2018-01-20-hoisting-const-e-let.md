---
layout: post
title: "Hoisting, const e let"
author: "Renê Soares"
categories: programação
tags: [javascript, estudo, básico]
image: image-post-keep-calm.jpeg
---

Nos meus estudo do JavaScript estava tendo dificuldade em entender os comportamentos exclusivos das variáveis, funções e comportamento da linguagem, literalmente dando um nó na cabeça. Decidi usar como guia de estudos o material do [CodeCast](https://www.youtube.com/channel/UCTluPqMkm90zyw6mCde561A), vendo os vídeos do [Vinicius Reis](https://twitter.com/luizvinicius73) da série [Domine this](https://www.youtube.com/watch?v=fBInMy61plk&list=PLy5T05I_eQYNQs4Pta85XRSucm3IOHx2M) pois do material em vídeo que eu pesquisei foi o melhor.

## Afinal o que é hoisting?

Basicamente o javascript _“elava” (hoisting)_ todas as variáveis até o topo do contexto de execução. É por esse motivo que quando executamos o js (forma carinhosa de chamar o javascript) reconhece as variáveis antes mesmo de ser declarada. Achou meio confuso? No começo do meu estudo também fiquei, mas o exemplo abaixo vai ajudar a ilustrar.

~~~javascript
console.log(pessoa);
// veja que não tinha iniciado a variável 'pessoa' dando um undefined

var pessoa = { nome: "Renê", sobrenome: "Soares" };
// criado a variável 'pessoa' como um objeto

console.log(pessoa);
// imprimindo o objeto da variável 'pessoa'
~~~

Veja a primeira linha do exemplo, perceba que usei o console.log(pessoa) sem iniciar a variável. Se fosse na maioria das linguagens daria logo uma grande erro na tela porém no nosso querido js simplesmente da um undefined. Logo na linha 4 iniciei a variável pessoa como um objeto e na linha 7 uso novamente o console.log(pessoa) e imprime o objeto.

## Vamos ver o let?

Segundo a página do [mozilla](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/let) falando sobre let: “let permite que você declare variáveis limitando seu escopo no bloco, instrução, ou em uma expressão na qual ela é usada.”
Acredito que o exemplo abaixo vai melhorar muito o entendimento.

~~~javascript
var residencia1 = 5;
var residencia2 = 10;

if (residencia1 === 5) {
    let residencia1 = 4; // O escopo é dentro do bloco if
    let residencia3 = 41; // O escopo é dentro do bloco if
    var residencia2 = 1; // O escopo é dentro da função

    console.log(residencia1); // 4
    console.log(residencia2); // 1
    console.log(residencia3); // 41
}

console.log(residencia1); // 5
console.log(residencia2); // 1
console.log(residencia3); // vai dar erro
~~~

Veja as linhas 5, 6 e 7 onde usamos o let e sobrescrevemos a variável residencia2, perceba que enquanto os console.logs dentro do if foram sobrescritas. Perceba que na linha 14 a variável voltou para seu antigo valor, a variável da linha 15 que foi sobrescrita na linha 7 e na linha 16 deu erro pois a variável residencia3 só existe dentro do bloco.

## Chegamos ao const

Ao declarar const cria-se uma variável onde seu valor é fixo porém não significa que o valor do mesmo seja imutável. Essa variável pode pertencer tanto ao escopo global quanto ao bloco onde foi declarada. Outra coisa importante é que toda constante tem que ser iniciada, ou seja, ter um valor atribuído. Acredito que um exemplo possa ilustrar melhor.

~~~javascript
const MY_UF = 'PR';
console.log('Meu estado é ' + MY_UF);

MY_UF = 'SE';

const MY_UF = 'RS';
var MY_UF = 'SP';
let MY_UF = 'BA';

const estados = {'uf' : 'PR'};

estados.uf = 'SE';
~~~

Antes de começar a explicar o código acima é importante ressaltar que as variáveis declaradas com const podem ser em caixa alta ou baixa, porém é uma convenção usar apenas em caixa alta, certo?

Na linha 1 declaramos a variável MY_UF foi declarada e atribuído um valor, perceba que da linha 5 até a linha 11 tentei mudar seu valor e em todos eles tivemos erro. Na linha 13 eu declarei uma variável como constante e recebendo um objeto, só que na linha 15 eu consigo mudar o seu valor, isso é possível pois a variável estados está apontando para um ponteiro na memória e como ela é uma constante eu não consigo mudar porém o objeto está apontando para um ponteiro de memória diferente e esse não está protegido por const e por isso consigo mudar.

## Conclusão

Esse foi o segundo post de uma série sobre JavaScript o primeiro foi [Entendendo Scopes]({% post_url 2017-09-23-entendendo-scopes %}), os códigos de exemplo estão no meu [github](https://github.com/renesoaresse/keep-calm-learn-javascript) caso queiram baixar. Espero que tenham gostado e qualquer dúvida, crítica ou sugestão basta colocar nos comentário.