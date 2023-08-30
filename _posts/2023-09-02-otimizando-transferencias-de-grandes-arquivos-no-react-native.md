---
layout: post
title: "Otimizando Transferências de grandes arquivos no React Native"
author: "Renê Soares"
categories: programação
tags: [react native, desenvolvimento, mobile, upload arquivos]
image: image-post-otimizando-transferencias-de-grandes-arquivos.jpg
---

Fala pessoal, tudo bem?

Hoje venho falar com vocês sobre o envio em partes. O `chunk upload` uma técnica utilizada para dividir arquivos grandes em partes menores, facilitando o manuseio e o envio deles para um servidor. No React Native, as operações de leitura e escrita de arquivos podem ser realizadas usando várias bibliotecas e APIs.

### Benefícios da técnica

O `chunk upload` oferece várias vantagens em relação ao envio normal ao lidar com arquivos grandes. Aqui estão algumas vantagens de usar o envio em partes:

1. Uso Eficiente de Memória:
Ao enviar arquivos grandes, o uso de memória pode se tornar uma preocupação. O envio em partes mitiga esse problema lendo e enviando partes menores de dados de cada vez, reduzindo o uso geral de memória. Isso permite uma gestão eficiente da memória e previne falhas ou erros de falta de memória.

2. Velocidade de Upload Mais Rápida:
Enviar um arquivo grande como uma única unidade pode levar um tempo significativo, especialmente se a conexão de rede for lenta ou instável. Com o envio em partes, o arquivo é dividido em partes menores que podem ser enviadas em paralelo ou sequencialmente. Essa paralelização melhora a velocidade do upload e reduz o tempo total de transferência, resultando em uma melhor experiência.

3. Maior Confiabilidade de Rede:
As redes podem ser propensas a interrupções ou problemas de conectividade intermitente. O envio em partes reduz o impacto de tais interrupções na rede, pois permite que o upload continue a partir da última parte enviada com sucesso. Se a conexão de rede for perdida, apenas as partes restantes precisam ser enviadas assim que a conexão for restabelecida.

4. Escalabilidade
O envio em partes permite lidar com tamanhos de arquivos maiores sem sobrecarregar os recursos do servidor. Ao dividir o arquivo em partes menores, o servidor pode processar e armazenar cada parte de forma independente, permitindo uma melhor escalabilidade e alocação eficiente de recursos.

No geral, o envio em partes otimiza o processo de upload para arquivos grandes, oferecendo vantagens como retomabilidade, uso eficiente de memória, velocidade de upload mais rápida, maior confiabilidade de rede, escalabilidade e uma melhor experiência do usuário. É particularmente útil em cenários onde uma transferência de grandes arquivos é crucial, como aplicativos de compartilhamento de arquivos, serviços de armazenamento na nuvem ou uploads de conteúdo de mídia.

### Pré-requisitos:

Antes de prosseguirmos, certifique-se de ter os seguintes pré-requisitos em ordem:
1. Ambiente de desenvolvimento React Native configurado.
2. Conhecimento básico de React Native e JavaScript.
3. Entendimento sobre operações de arquivo e requisições HTTP.

#### Passo 1: Instale as Dependências:
Para começar, crie um novo projeto React Native e navegue até o seu diretório raiz. Abra um terminal e execute o seguinte comando para instalar as dependências necessárias:

~~~shell
yarn add react-native-fs axios
~~~

A biblioteca `react-native-fs` é usada para operações de sistema de arquivos, e `axios` é uma biblioteca cliente HTTP popular.

#### Passo 2: Implementar a Lógica de Upload por Partes:
Crie um novo arquivo chamado `ChunkUploader.js` e importe as dependências necessárias da seguinte maneira:

~~~javascript
import RNFS from 'react-native-fs';
import axios from 'axios';
~~~

Em seguida, defina uma função que lê um arquivo do armazenamento e o envia em partes:
~~~javascript
const uploadFileInChunks = async (filePath, chunkSize, uploadUrl) => {
  try {
    const stats = await RNFS.stat(filePath);
    const fileSize = stats.size;
    let offset = 0;

    while (offset < fileSize) {
      const chunk = await RNFS.read(filePath, chunkSize, offset, ‘base64’);
      const formData = new FormData();
      formData.append('chunk', chunk);
      formData.append('offset', offset.toString());
      formData.append('totalSize', fileSize.toString());

      await axios.post(uploadUrl, formData, {
      headers: { 'Content-Type': 'multipart/form-data' } });
      offset += chunkSize;
    }
    console.log('Envio completo');
  } catch (error) {
    console.error('Erro durante o envio:', error);
  }
};
~~~

Vamos decompor a lógica:

* Primeiramente, recuperamos o tamanho do arquivo usando `RNFS.stat()` para determinar o número total de bytes.
* Em seguida, definimos `offset` inicial como `0`.
* No loop, lemos uma parte do arquivo usando `RNFS.read()` com o `chunkSize` especificado, `offset`, e como `base64`.
* Criamos um novo objeto `FormData` e anexamos a parte, o deslocamento e o tamanho total do arquivo como dados do formulário.
* A parte é então enviada ao servidor usando uma solicitação HTTP POST feita com `axios`.
* Após cada upload bem-sucedido, incrementamos o deslocamento pelo tamanho da parte e repetimos o processo até que o arquivo inteiro tenha sido enviado.
* Por fim, registramos uma mensagem indicando que o upload está completo.

#### Passo 3: Uso:
Agora que implementamos a lógica de upload em partes, podemos usá-la em nossos componentes React Native.

~~~javascript
import React, { useEffect } from 'react';
import { View, Button } from 'react-native';
import { requestExternalStoragePermission } from './utils/permissions';
~~~

Você pode implementar essa função para solicitar permissões de armazenamento.

~~~javascript
const App = () => {
  useEffect(() => {
    requestExternalStoragePermission(); // Solicitar permissão de armazenamento externo
  }, []);

  const handleUpload = () => {
    const filePath = 'path_to_file';
    const chunkSize = 1024 * 1024;
    const uploadUrl = 'https://example.com/upload';

    uploadFileInChunks(filePath, chunkSize, uploadUrl);
  };

  return (
    <View>
      <Button onPress={handleUpload} title='Envio do arquivo' />
    </View>
  );
};

export default App;
~~~

No exemplo acima, criamos um componente simples do React Native que aciona a função `handleUpload` quando um botão é pressionado.
Certifique-se de substituir `path_to_file` pelo caminho real do arquivo que você deseja fazer upload, e `https://example.com/upload` pelo endpoint de upload do seu servidor.

### Conclusão:
Neste artigo, exploramos como realizar o upload em partes no React Native usando operações de leitura e escrita de arquivos do armazenamento do dispositivo. Ao dividir arquivos grandes em partes menores, podemos enviá-los de forma eficiente para um servidor, reduzindo o risco de timeouts ou perda de dados. Lembre-se de lidar com cenários de erro e personalizar a implementação de acordo com suas necessidades específicas.