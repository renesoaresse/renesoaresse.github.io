---
layout: post
title: "Uma análise comparativa: React Navigation, React Native Navigation (Wix) e React Native Router Flux"
author: "Renê Soares"
categories: programação
tags: [react native, desenvolvimento, mobile, lib, navegação]
image: image-post-uma-analise-comparativa.png
---

Fala pessoal, tudo bem?

Hoje venho falar com vocês sobre as libs de navegação com react native. Dito isso, vamos aproveitar todo poder do JavaScript e do React, o React Native capacita os desenvolvedores a criar aplicativos verdadeiramente nativos usando uma linguagem familiar e versátil, aproveitando a abordagem “aprender uma vez, escrever em qualquer lugar”.

A beleza do React Native reside em sua capacidade de preencher as lacunas entre os componentes nativos e o código JavaScript.

Além de seus benefícios de eficiência e desempenho, o React Native oferece um ecossistema de componentes, bibliotecas e ferramentas de desenvolvedor pré-construídos.

Para desenvolvedores React Native, escolher a biblioteca de navegação correta é vital para criar experiências de usuário intuitivas e contínuas.

O gerenciamento da apresentação e da transição entre várias telas normalmente é feito pelo que é conhecido como navegador.

### React Navigation

React Navigation é uma biblioteca de navegação amplamente utilizada e altamente flexível para React Native.

Ele oferece uma abordagem baseada em JavaScript e oferece suporte à navegação baseada em pilha e em guias e navegação em gaveta.

Ele fornece uma API declarativa e navegadores personalizáveis, além de permitir links diretos para lidar com URLs.

Vamos explorar como usar o React Navigation com alguns trechos de código:

**Instalação:** Primeiro, instale os pacotes necessários

~~~shell
yarn add @react-navigation/native react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
~~~

**Configure o contêiner de navegação:** envolva seu componente raiz com o `NavigationContainercomponente` para fornecer funcionalidade de navegação a sua aplicação.

~~~javascript
import { NavigationContainer } from '@react-navigation/native';

function App() {
  return (
    <NavigationContainer>
    </NavigationContainer>
  );
}
~~~

**Exemplo de navegação em Stack:** vamos criar uma navegação simples usando React Navigation. Neste exemplo, temos duas telas: HomeScreen e DetailsScreen.

~~~javascript
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
~~~

**Exemplo de navegação em Tab:** se quiser usar a navegação baseada em tabs, você pode usar `createBottomTabNavigator` ou `createMaterialBottomTabNavigator` do React Navigation. Aqui está um exemplo usando `createBottomTabNavigator2`.

~~~javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Tab = createBottomTabNavigator();

function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Settings" component={SettingsScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
~~~

Lembre-se de importar os componentes necessários do React Navigation e agrupar seu componente principal com o `NavigationContainer` para ativar a funcionalidade de navegação.

Em seguida, defina o navegador utilizando a função de navegador apropriada (`createStackNavigator`, `createBottomTabNavigator`, etc.) e configure as telas.

Documentação do React Navigation [aqui](https://reactnative.dev/docs/navigation).

### React Native Navigation (Wix)

React Native Navigation, desenvolvida pelo Wix, é uma biblioteca de navegação de alto desempenho que oferece experiências de navegação nativas nas plataformas iOS e Android.

Ele utiliza controladores de navegação nativos e fornece transições e gestos suaves, resultando em uma experiência de usuário altamente responsiva e envolvente.

Vamos explorar como usar o React Native Navigation (Wix) com uma breve explicação e trechos de código:

**Instalação:** Primeiro, instale o pacote:

~~~shell
yarn add react-native-navigation
~~~

**Definição do componente de navegação:** no componente de entrada do seu aplicativo (por exemplo, index.tsx), registre as telas do seu aplicativo e defina o componente de navegação usando a API do React Native Navigation

~~~javascript
import { Navigation } from 'react-native-navigation';
import HomeScreen from './screens/HomeScreen';
import ProfileScreen from './screens/ProfileScreen';

Navigation.registerComponent('HomeScreen', () => HomeScreen);
Navigation.registerComponent('ProfileScreen', () => ProfileScreen);

Navigation.setRoot({
  root: {
    component: {
      name: 'HomeScreen',
    },
  },
});
~~~

**Navegando entre telas:** para navegar entre as telas, use o método `push` fornecido pela biblioteca.

~~~javascript
import { Navigation } from 'react-native-navigation';

function navigateToProfileScreen() {
  Navigation.push('HomeScreen', {
    component: {
      name: 'ProfileScreen',
      passProps: {
        userId: 123,
      },
      options: {
        topBar: {
          title: {
            text: 'Profile',
          },
        },
      },
    },
  });
}
~~~

No exemplo acima, navegamos de 'HomeScreen' para 'ProfileScreen' usando o método `Navigation.push`. Passando parâmetros adicionais (por exemplo, userId) para a tela de destino e configuramos as opções da barra superior, como definir um título personalizado.

**Personalizando opções de navegação:** Há lib oferece várias opções para personalizar a navegação, incluindo estilizar a barra de navegação, manipular botões de voltar e controlar animações. Aqui está um exemplo de configuração da barra de navegação.

~~~javascript
import { Navigation } from 'react-native-navigation';

Navigation.setDefaultOptions({
  topBar: {
    title: {
      color: 'white',
    },
    background: {
      color: '#333',
    },
  },
});
~~~

Ela oferece uma ampla variedade de padrões de navegação, incluindo navegação em stack, navegação em tab, navegação com menu lateral e muito mais.

Você pode explorar esses padrões e opções de configuração adicionais na [documentação oficial](https://wix.github.io/react-native-navigation/docs/before-you-start/).

### React Native Router Flux:

React Native Router Flux é uma biblioteca de navegação para React Native inspirada em React Router.

React Native Router Flux usa uma abordagem centralizada onde você define cenas e navega entre elas usando ações.

React Native Router Flux oferece suporte a vários tipos de navegação, incluindo navegação por guias, navegação modal e navegação por stack.

Aqui está uma breve explicação do React Native Router Flux junto com um trecho de código para ilustrar seu uso:

**Instalação:** Para usar o React Native Router Flux, instale o pacote.

~~~shell
yarn add react-native-router-flux
~~~

**Importando componentes necessários:** importe os componentes necessários.

~~~javascript
import { Router, Scene } from 'react-native-router-flux';
~~~

**Configurando roteador e cenas:** Defina seu roteador e cenas usando os componentes `Router` e `Scene`. Cada cena representa uma tela no seu aplicativo.

~~~javascript
function App() {
  return (
    <Router>
      <Scene key="root">
        <Scene key="home" component={HomeScreen} initial />
        <Scene key="details" component={DetailsScreen} />
      </Scene>
    </Router>
  );
}
~~~

Neste exemplo temos duas cenas: "home" e "details". A cena "home" é definida como cena inicial usando o parâmetro `initial`.

**Navegando:** para navegar, use o "Actions" fornecido pela lib. Você pode acionar ações de navegação de forma programática ou usando componentes de UI, como botões.

~~~javascript
import { Actions } from 'react-native-router-flux';

function HomeScreen() {
  return (
    <View>
      <Text>Bem vindo a Home!</Text>
      <Button title="Go to Details" onPress={() => Actions.details()} />
    </View>
  );
}
~~~

Neste exemplo, pressionar o botão navegará para a cena "details" usando a função `Actions.details()`.

React Native Router Flux fornece recursos adicionais, como passagem de parâmetros entre cenas, manipulação de cenas aninhadas e personalização de estilos de barra de navegação.

É importante ressaltar que a lib é indicada para aplicações de pequeno e médio porte com requisitos de navegação menos complexos.

No entanto, observe que o React Native Router Flux não é mais mantido ativamente e foi sucedido por outras bibliotecas de navegação como o React Navigation, que oferece recursos mais extensos e atualizações contínuas.

[Leia a documentação do React Native Router Flux](https://github.com/aksonov/react-native-router-flux/blob/master/docs/API.md) para obter instruções de uso detalhadas e explore recursos mais avançados para aprimorar o fluxo de navegação do seu aplicativo React Native.

### Conclusão

Escolher a biblioteca de navegação é uma das principais decisões que nós como desenvolvedores temos que tomar, pois através dela, a experiência dos usuários fica mais fluida e intuitiva. Expero que essa pequena análise ajude a quem ler esse artigo escolher a lib mais adequada para seu projeto.

