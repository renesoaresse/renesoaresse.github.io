---
layout: post
title: "Covertendo e enviando vídeos no React Native"
author: "Renê Soares"
categories: programação
tags: [react native, desenvolvimento, mobile, ffmpeg]
---

Fala pessoal, tudo bem?

Hoje venho falar com vocês sobre FFmpeg no React Native. A ideia era adicionar a capacidade de obter um vídeo da galeria, convertê-lo para um formato diferente e enviá-lo para um armazenamento de arquivos de terceiros.

### Vamos lá?

Comecei adicionando as bibliotecas necessárias. A primeira será [react-native-ffmpeg](https://www.npmjs.com/package/react-native-ffmpeg). O FFmpeg em si é um conjunto de bibliotecas que nos permite trabalhar com arquivos de diversas maneiras (cortar, converter, transformar etc.), exatamente o que preciso.

> [react-native-ffmpeg](https://www.npmjs.com/package/react-native-ffmpeg) está obsoleto e não é mais mantido;
> Você pode usar [ffmpeg-kit-react-native](https://www.npmjs.com/package/ffmpeg-kit-react-native) (que é basicamente o mesmo, mas com suporte).


Outra biblioteca que estou usando para enviar arquivos de mídia, como imagens/vídeos, é [rn-fetch-blob](https://www.npmjs.com/package/rn-fetch-blob). Está um pouco desatualizada e você pode querer usar algo que ainda seja mantido (como [react-native-blob-util](https://www.npmjs.com/package/react-native-blob-util), por exemplo). Além disso, você precisará de [react-native-fs](https://www.npmjs.com/package/react-native-fs) para trabalhar com arquivos.

### iOS

Imagine que vai selecionar um vídeo de um dispositivo iOS. O trecho de código que precisará para converter o vídeo para um formato diferente seria:

~~~javascript
import { Platform } from 'react-native';
import RNFS from 'react-native-fs';
import { FFmpegKit } from 'ffmpeg-kit-react-native';

const convertVideo = async (videoUri) => {
  const outputVideoName = 'convertedVideo.mp4';
  const outputVideoUri = `${RNFS.DocumentDirectoryPath}/${outputVideoName}`;

  const ffmpegCommand = `-y -i ${videoUri} ${outputVideoUri}`;

  try {
    await FFmpegKit.execute(ffmpegCommand);
    return outputVideoUri;
  } catch (error) {
    console.error('Erro na conversão do vídeo:', error);
    throw error;
  }
};

~~~

Depois de selecionar o vídeo da galeria, basta passar o `URI` desse vídeo para a função de conversão. Criei um caminho de saída para o vídeo convertido e o armazeno na variável `outputVideoUri`. Depois disso, chamo a função `FFmpegKit.execute`, que aceita uma string onde escreve os comandos do FFmpeg para trabalhar com arquivos de mídia.

#### O que está escrito lá?

1. -y é uma flag que permite sobrescrever o vídeo de saída. Sem essa flag, o FFmpeg não sobrescreverá um vídeo já existente com o mesmo nome, e não tenho como obter um novo vídeo convertido;

2. -i é uma flag que escreve antes de especificar os URIs dos arquivos;

3. Depois, tem `videoUri` — URL do vídeo original — e `outputVideoUri` — URI do vídeo convertido.


A conversão ocorre porque a extensão de saída é `.mp4` (que está escrita na variável `outputVideoName`), e o FFmpeg usa codificadores de áudio e vídeo para converter o vídeo. Como codificadores padrão, geralmente, o FFmpeg usa `mpeg4` para vídeo e `AAC` para áudio.

> Dispositivos iOS suportam apenas os formatos de vídeo
> H.264, MP4, .MOV, .M4V, MP3 e AAC. Portanto, se você converter para
> qualquer outro tipo, pode não conseguir reproduzi-lo no iOS.


O resultado da função `convertVideo` será o URI do vídeo convertido.

Durante a conversão do vídeo, verá alguns logs que são gerados automaticamente pelo FFmpeg. Mas eles nem sempre são descritivos. Para entender que o vídeo está de fato sendo convertido, você precisará encontrar algumas linhas de logs que se parecem com isto:

~~~shell
LOG      Metadata:
 LOG        creation_time   :
 LOG  2025-01-01T12:36:51.000000Z
 LOG  
 LOG        handler_name    :
 LOG  Core Media Audio
 LOG  
 LOG        encoder         :
 LOG  Lavc58.96.100 libvorbis
 LOG  
 LOG  frame=    8 fps=0.0 q=0.0 size=       5kB time=00:00:01.25 bitrate=  29.7kbits/s speed=2.34x
 LOG  frame=   20 fps= 18 q=0.0 size=       5kB time=00:00:01.66 bitrate=  22.3kbits/s speed=1.47x
 LOG  frame=   28 fps= 17 q=0.0 size=       5kB time=00:00:01.96 bitrate=  18.9kbits/s speed= 1.2x
 LOG  frame=   38 fps= 18 q=0.0 size=       5kB time=00:00:02.06 bitrate=  18.0kbits/s speed=0.966x
 LOG  frame=   47 fps= 14 q=0.0 size=       5kB time=00:00:02.06 bitrate=  18.0kbits/s speed=0.613x
 LOG  frame=   51 fps= 13 q=29.0 size=       5kB time=00:00:02.06 bitrate=  18.0kbits/s speed=0.532x
 LOG  frame=   54 fps= 12 q=29.0 size=       5kB time=00:00:02.06 bitrate=  18.0kbits/s speed=0.45x
 LOG  frame=   58 fps= 11 q=29.0 size=       5kB time=00:00:02.06 bitrate=  18.0kbits/s speed=0.39x
 LOG  frame=   62 fps= 11 q=29.0 size=       5kB time=00:00:02.06 bitrate=  18.0kbits/s speed=0.35x
 LOG  frame=   64 fps=5.7 q=-1.0 Lsize=    1165kB time=00:00:02.12 bitrate=4488.2kbits/s speed=0.191x
 LOG  video:1141kB audio:18kB subtitle:0kB other streams:0kB global headers:3kB muxing overhead:
 LOG  0.515257%
 LOG  
 LOG  frame I:3     Avg QP:19.95  size: 39164
 LOG  frame P:21    Avg QP:21.95  size: 24924
 LOG  frame B:40    Avg QP:22.17  size: 13183
 LOG  consecutive B-frames: 14.1%  6.2%  4.7% 75.0%
~~~

O que interessa são as linhas `frame=`, que indicam o progresso da conversão.

Para enviar um vídeo, vou escrever o seguinte script simples:

~~~javascript
import RNFetchBlob from 'rn-fetch-blob';

const uploadVideo = async (videoUri) => {
  const type = 'video/mp4';
  const fileUri = Platform.OS === 'ios' ? decodeURIComponent(videoUri.replace('file://', '')) : videoUri;
  const fileData = RNFetchBlob.wrap(fileUri);

  const headers = {
    'Content-Type': 'multipart/form-data',
  };

  const body = [
    {
      name: 'file',
      filename: 'video.mp4',
      type,
      data: fileData,
    },
  ];

  try {
    const response = await RNFetchBlob.fetch('POST', 'https://example.com/upload', headers, body);
    return response.json();
  } catch (error) {
    console.error('Erro no upload do vídeo:', error);
    throw error;
  }
};
~~~

Analisar este código linha por linha:

1. Criamos uma variável `type` com o valor `video/mp4`. Este é o tipo MIME do conteúdo que vamos enviar. É perfeitamente adequado enviar um vídeo `.mp4` com um tipo MIME `video/mp4`;

2. No iOS, antes de acessar quaisquer arquivos para buscá-los, precisei chamar `decodeURIComponent`, que decodificará alguns dos caracteres especiais do caminho URI no iOS (por exemplo, "%41" para "A"). Também precisei remover o prefixo `file://`, pois não preciso dele no iOS;

3. Em seguida, chamo o método `RNFetchBlob.wrap`. Ele apenas adicionará um novo prefixo à string, que é `RNFetchBlob-file://`;

4. E, finalmente, formei os cabeçalhos e o corpo da solicitação, ai chamo e retorno a resposta analisada.

Isso é suficiente para enviar um vídeo no iOS.

### Android

Agora, vou abordar o processo no Android, que é um pouco mais complexo em termos de conversão de vídeo.

> As informações a seguir são aplicáveis apenas ao uso do React Native no
> Android, juntamente com a biblioteca `react-native-image-picker`
> configurada com mediaType definido como 'mixed'.

Ao selecionar um vídeo da galeria no Android, você obtém um URI de conteúdo que começa com o prefixo `content://`. Esse tipo de URI não pode ser utilizado diretamente para upload de vídeos, pois bibliotecas como `RNFetchBlob` não conseguem localizar um arquivo com esse URI. Portanto, é necessário convertê-lo para um URI de arquivo.

Para isso, criei uma função que converte o URI de conteúdo em um URI de arquivo e, em seguida, copia o vídeo para esse novo local:

~~~javascript
import RNFS from 'react-native-fs';

const createFileUriFromContentUri = async (contentUri) => {
  const fileName = 'tempVideo.mp4';
  const fileUri = `${RNFS.TemporaryDirectoryPath}/${fileName}`;

  try {
    await RNFS.copyFile(contentUri, fileUri);
    return fileUri;
  } catch (error) {
    console.error('Erro ao copiar o arquivo:', error);
    throw error;
  }
};
~~~

Após obter o `fileUri` resultante, podemos utilizá-lo na função de conversão de vídeo, semelhante ao processo no iOS:

~~~javascript
import { FFmpegKit } from 'ffmpeg-kit-react-native';

const convertVideo = async (videoUri) => {
  const outputVideoName = 'convertedVideo.mp4';
  const outputVideoUri = `${RNFS.DocumentDirectoryPath}/${outputVideoName}`;

  const ffmpegCommand = `-y -i ${videoUri} ${outputVideoUri}`;

  try {
    await FFmpegKit.execute(ffmpegCommand);
    return outputVideoUri;
  } catch (error) {
    console.error('Erro na conversão do vídeo:', error);
    throw error;
  }
};
~~~

Finalmente, para fazer o upload do vídeo convertido, utilizei uma função semelhante à do iOS, ajustando conforme necessário para o ambiente Android:

~~~javascript
import RNFetchBlob from 'rn-fetch-blob';

const uploadVideo = async (videoUri) => {
  const type = 'video/mp4';
  const fileData = RNFetchBlob.wrap(videoUri);

  const headers = {
    'Content-Type': 'multipart/form-data',
  };

  const body = [
    {
      name: 'file',
      filename: 'video.mp4',
      type,
      data: fileData,
    },
  ];

  try {
    const response = await RNFetchBlob.fetch('POST', 'https://example.com/upload', headers, body);
    return response.json();
  } catch (error) {
    console.error('Erro no upload do vídeo:', error);
    throw error;
  }
};
~~~

Com essas funções, fui capaz de selecionar um vídeo da galeria no Android, convertê-lo para o formato desejado e enviá-lo para um servidor ou serviço de armazenamento externo.

### Conclusão

Implementei a funcionalidade de seleção, conversão e upload de vídeos no React Native utilizando bibliotecas específicas como `react-native-image-picker`, `ffmpeg-kit-react-native` e `react-native-blob-util`. Enquanto no iOS o processo foi direto, no Android foi necessário converter URIs de conteúdo para URIs de arquivo antes de continuar. Com essas soluções, consegui atender às necessidades em ambas as plataformas de forma eficiente.