---
layout: post
title: "Tela de bloqueio no React Native"
author: "Renê Soares"
categories: programação
tags: [typescript, react native, desenvolvimento, mobile, segurança]
image: image-post-tela-boloqueio-react-native.jpeg
---

Fala pessoal, tudo bem?

Hoje venho trazer um assunto bem legal, como fazer uma tela de bloqueio para seu app, onde você tenha uma sessão cronometrada e certas telas só são acessíveis se a sessão não tiver expirado.

Essa implementação é um método eficaz para melhorar a segurança no seu app e implementar um cronômetro que solicita ao usuário a entrada da sua senha a cada vez que o tempo expirar, trazendo assim uma camada adicional de proteção, verificando regularmente a identidade do usuário e evitando acessos não autorizados.

Meu objetivo neste post é demonstrar como implementar um middleware que é executado sempre que o usuário navega para uma tela diferente. Este middleware permite que faça a autenticação antes de conceder a acesso a tela navegada. Caso o usuário não consiga autenticar ou fornecer uma senha incorreta, vou redirecionar o usuário de volta à tela inicial, evitando assim a exibição de dados sensíveis.

### Dado o disclaimer, vamos para a codificação!

Primeiramente vou criar um hook personalizado chamado `useScreenGuard`, nele vou encapsular a lógica. Usando esse hook, posso executar a lógica sempre que a sessão expirar. Isso garante a proteção da tela acionada no momento da chamada.

~~~javascript
export const useScreenGuard = (componentId: string, callback?: () => void) => {
  useEffect(() => {
    if (currentComponentId === componentId && session.isExpired) {
      if (callback) {
        callback();
      } else {
        console.log('Requer credenciais');
        onScreenGuardRequiresLogin();
      }
    }
  }, [session.isExpired]);
};
~~~

### O que seriam essas variáveis currentComponentId e isExpired?

O `currentComponentId` serve para ser o identificador da tela exibida. Eu consigo essa informação ouvindo o evento global componentDidAppear e ele atualiza o currentComponentId sempre que uma nova tela é renderizada.


~~~javascript
const deniedComponents = ['componentAlert'];

Navigation.events().registerComponentDidAppearListener(({ componentId }) => {
      console.log(componentId, 'Tela apareceu);
      if (!deniedComponents.includes(componentId)) {
        currentComponentId = componentId
      }
});
~~~

O `isExpired` é um boleano que indica se a sessão expirou ou não.

### Usando o hook de useScreenGuard!

Não há muito a elaborar aqui. Se você está familiarizado com hooks, já sabe que eles podem ser usados dentro de outros hooks ou componentes funcionais. Os hooks fornecem uma maneira de encapsular lógica reutilizável e gerenciamento de estado de uma maneira concisa e modular. Aproveitando os hooks, podemos aprimorar a funcionalidade e reutilização de nosso código em vários componentes no aplicativo.

~~~javascript
interface Props extends NavigationComponentProps {
  ...
}

const MySecuredScreen = ({ componentId }: Props) => {
    // Solicita credenciais ao usuário quando a sessão expira.
    useScreenGuard(componentId)

    ...resto do código
}
~~~

Chamo dentro do componente funcional e forneço o `componentId` do próprio componente como argumento. Permitindo que o hook associe o componente específico à lógica de proteção de sessão necessária. Ao incorporar este hook no componente, garanto que a sessão é devidamente monitorada e solicita as credenciais do usuário quando necessário, aprimorando a segurança geral do aplicativo.

### Impedir a tela de ser exibida se a sessão tiver expirado.

Mesmo se a sessão tiver expirado, a tela ainda aparece, permitindo que o usuário veja o conteúdo por alguns milissegundos. Para resolver este problema, precisei criar um hook adicional que gerencie a visibilidade das telas antes de empurrá-las para a pilha ou exibi-las em modais. Utilizando este hook, pude renderizar condicionalmente as telas com base no status ativo da sessão, garantindo assim que as sessões expiradas restrinjam o acesso ao conteúdo sensível.

~~~javascript
export const useLazyScreenGuard = () => {
  useEffect(() => {
    const navigationCommandListener = Navigation.events().registerCommandListener(async (name) => {
      if ((name === 'showModal' || name === 'push') && session.isExpired) {
        await onScreenGuardRequiresLogin();
      }
    });
    return navigationCommandListener.remove;
  });
};
~~~

Na função onScreenGuardRequiresLogin, exibe uma tela que solicita ao usuário que insira suas credenciais para redefinir a sessão e conceder-lhes acesso à tela solicitada. Isso garante que apenas usuários autenticados com uma sessão ativa possam acessar telas sensíveis. 

Em resumo, a proteção de tela é uma abordagem eficaz para melhorar a segurança do aplicativo. Ao monitorar a atividade do usuário e solicitar autenticação periodicamente, posso reduzir o risco de acesso não autorizado e garantir que informações confidenciais permaneçam seguras.