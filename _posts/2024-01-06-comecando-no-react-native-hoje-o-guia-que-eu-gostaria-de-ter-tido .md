---
layout: post
title: "Começando no React Native hoje: O guia que gostaria de ter tido"
author: "Renê Soares"
categories: programação
tags: [react native, desenvolvimento, mobile, carreira]
---

Fala pessoal, tudo bem?

Hoje venho falar com vocês sobre um guia que gostaria de ter tido quando começei a programar com React Native. A ideia é facilitar para quem quer aprender o react native com a minha experiência.

## Pré-requisitos

Para entender React Native, você precisa de um conhecimento de JavaScript, então foque nisso. Aprender CSS e HTML ao longo do caminho também ajudará.

Sugiro que você comece aprendendo o básico de HTML (você não vai precisar muito disso no React Native, mas ajuda a entender). Você pode conferir o [W3Schools](https://www.w3schools.com).

Depois, familiarize-se com CSS e estilização, incluindo flexboxes, grids e variáveis. Foca em aprender os conceitros, pois pode revisitar isso mais tarde com SASS e styled components.

O mais importante é focar em JavaScript. Leve o tempo que precisar, pois será a principal linguagem de programação que você usará em sua carreira como desenvolvedor front-end ou React Native, então você precisa dominá-la.

Gosto muito deste roadmap de JavaScript: [JavaScript Roadmap](https://roadmap.sh/javascript). Certifique-se de cobrir tudo nele, especialmente os itens marcados em verde ou roxo, pois são essenciais. Quando encontrar algo desconhecido, pesquise, tente escrever e executar algum código JavaScript e explore os conceitos básicos e seus fundamentos.

### Requisito Opcional

Depois de dominar JavaScript, você pode mergulhar diretamente no React e no React Native. No entanto, tenha em mente que a maioria das empresas agora prefere usar TypeScript ao vez de JavaScript.

TypeScript é essencialmente JavaScript com recursos adicionais, como anotações de tipo e classes, o que torna o código mais organizado e fácil de gerenciar. Embora ele seja compilado para JavaScript, o TypeScript ajuda você a escrever código mais limpo e sustentável.

Recomendo não gastar muito tempo com TypeScript no início. Seus benefícios ficam claros com o uso diário e a aplicação prática e trabalho com uma equipe. Foque em aprender React primeiro. Você pode voltar a estudar o TypeScript mais tarde, quando tiver uma base sólida em React.

Em resumo, embora o TypeScript seja importante, não é essencial nos estágios iniciais.

## React

Quero que você saiba que esta etapa considero muito importante porque você pode se tornar um engenheiro front-end apenas sabendo React. É uma carreira inteira que as pessoas exploram profundamente, então é importante manter o foco no motivo pelo qual você quer aprender isso.

Você quer aprender isso para ser um desenvolvedor front-end? Se sim, você pode estudar um framework como Next.js ou uma alternativa como Vite. Verifique as versões mais recentes do React para ver os recursos mais recentes. Você também deve se aprofundar em alguns conceitos extras, como componentes de servidor, que ainda não existem no React Native e no desenvolvimento móvel.

Você aprendendo isso, para ser um desenvolvedor React Native? Legal, você precisará dominar o básico do React primeiro. Não se preocupe em gastar muito tempo com isso, pois ao aprender React, você está essencialmente aprendendo React Native também. Escrever código React Native será muito semelhante a escrever React JS, especialmente no início.

### Checklist para React

Fiz essa lista de verificação dos seguintes conceitos e para você se certificar de entender cada um deles.

- O que são componentes?
- Ciclo de vida dos componentes
- Componentes funcionais (Havia outro tipo chamado componentes de classe, mas não é mais recomendado usá-los, então não foque nisso.)
- JSX
- Roteamento (React Router)
- PROPS e Prop Drilling
- STATES
- Manipulação de eventos
- Renderização condicional
- Listas e chaves
- Hooks

Entenda os seguintes hooks nesta ordem:

- useState
- useEffect
- useContext
- useReducer
- useMemo
- useCallback
- useRef
- useLayoutEffect
- Custom Hooks

Há outros hooks que você pode aprender, mas esses são os essenciais nos quais você deve se concentrar.

Em seguida, passe para estes conceitos:

- Componentes de ordem superior (Higher-order components)
- Context API
- Gerenciamento de estado (Sugiro Zustand, mas algumas empresas usam Redux e também o MobX.)
- Componentes de UI
- Material UI / ShadCN (Estes não funcionam com React Native, mas é importante entender como eles funcionam agora para usar styled components como React Native Paper no futuro.)
- Chamadas de API
- Fetch básico
- Tanstack query (Anteriormente conhecido como React Query, é muito útil, mas não é estritamente necessário. Ainda não tenho dominio completo, porém notei que é muito usado pelas empresas.)
- Axios

Todos esses são para APIs REST, que eu usei exclusivamente. Para GraphQL, eu usei Apollo.

### Conceitos Adicionais

- Limites de erro (Error Boundaries)
- Divisão de código (Code Splitting)
- Carregamento lento (Lazy Loading)
- Contexto e Portais
- Testes (Jest, React Testing Library)
- Otimização de desempenho
- Acessibilidade

Esses tópicos adicionais darão a você uma compreensão mais ampla e profunda, tornando-o mais proficiente e preparado para vários cenários em projetos reais.

## React Native

Eu sei que você provavelmente está pensando: "Ah, finalmente! Vou aprender React Native agora!" Mas surpresa! Você já aprendeu cerca de 80% disso na seção anterior, então as coisas serão muito mais fáceis a partir daqui.

Configurar o React Native CLI pode ser um processo tedioso e desafiador, porém sugiro você aprender tanto ele, quanto o Expo.

Só para você saber, o Expo para React Native é como o Next.js para React.js. Ele facilita a configuração do seu projeto e adiciona alguns recursos legais. Por exemplo, o Expo Go me permite ver meu aplicativo em dispositivos reais sem precisar de ferramentas pesadas como o Xcode. Muitas empresas dependem do Expo porque ele oferece coisas como EAS Build e ajuda a enviar aplicativos para as lojas.

Claro, há algumas desvantagens, como módulos nativos limitados, então você não conseguirá escrever código `Kotlin` ou `Swift` facilmente em seu projeto. Aplicativos Expo também tendem a ser maiores em tamanho, mas você provavelmente não se importará com isso, especialmente enquanto ainda está aprendendo. Honestamente, precisei poucas vezes escrever um módulo nativo, porém é bom ter isso em mente.

Sinta-se à vontade para conferir a documentação tanto do Expo quanto do React Native CLI. É incrível e vai ajudá-lo muito, seja você um iniciante ou um desenvolvedor com experiencia.

A documentação é super clara, descritiva e fácil de entender, tornando simples encontrar o que você precisa.

Sempre que você ficar preso em algo ou quiser aprender mais sobre um tópico que mencionei, verifique a documentação antes de procurar em outro lugar:

- [Documentação do Expo](https://docs.expo.dev/)
- [Documentação do React Native](https://reactnative.dev/docs/getting-started)

Depois de configurar o Expo, estude os componentes principais. Você os usará em seu JSX/TSX em vez de tags HTML como `div` (esqueça o `div`, não temos isso no mobile). Aqui estão os componentes nos quais você deve se concentrar: `Text`, `TextInput`, `Button`, `Image`, `TouchableOpacity`, `Pressable`, `ActivityIndicator`, `View`, `SafeAreaView`, `ScrollView`, `FlatList`.

Agora, estude folhas de estilo e flexboxes; seria útil saber CSS aqui.

Para rede e chamadas de API, você pode continuar usando o Axios da mesma forma que fez com o React JS.

Aprenda mais sobre armazenamento. Claro, você não usará `localStorage` ou `sessionStorage` como está acostumado na web, então confira coisas como `AsyncStorage` e `ExpoSqlLite`.

Estude como enviar notificações push usando uma biblioteca de terceiros. Sugiro o Firebase Cloud Messaging, pois ele suporta Android e iOS. Ou, se você estiver usando o Expo, confira isso aqui: [Expo Push Notifications](https://docs.expo.dev/push-notifications/overview/)

Confira também a autenticação aqui: [Expo Authentication](https://docs.expo.dev/develop/authentication/)

Escolha uma biblioteca de componentes de interface do usuário e aprenda mais sobre ela. Seja [React Native Paper](https://callstack.github.io/react-native-paper/), [Tamagui](https://tamagui.dev/) ou outra, a escolha é totalmente sua.

Para testes, você pode usar Jest (você provavelmente já está familiarizado com ele se já trabalhou como desenvolvedor React JS, pois é muito usado). Você pode usar [Detox](https://wix.github.io/Detox/) para testes de ponta a ponta ou o [Maestro](https://maestro.mobile.dev/).

Além disso, estude como melhorar o desempenho do seu aplicativo. Entenda as taxas de quadros e como elas funcionam, como otimizar um `FlatList` e usá-lo em vez de outros componentes, usar sprites de imagem e CDN. Coisas assim podem ser difíceis de entender agora, mas acredito que, quando você chegar lá, saberá exatamente o que precisa pesquisar.

## Conclusão

Você chegou ao final deste roteiro! Aprender React Native pode parecer uma tarefa grande, mas vá passo a passo, e você chegará lá. Lembre-se, não há pressa — cada um aprende no seu próprio ritmo. Continue experimentando, fazendo perguntas e, mais importante, divirta-se com isso. Mergulhe na documentação, junte-se à comunidade e não hesite em pesquisar no Google para superar desafios. Esses são os conselhos que gostaria de ter tido e não tive quando começei com o react native, espero te ajudado.