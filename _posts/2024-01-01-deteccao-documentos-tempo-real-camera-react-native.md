---
layout: post
title: "Detecção de documentos em tempo real usando a câmera no React Native"
author: "Renê Soares"
categories: programação
tags: [react native, desenvolvimento, mobile]
image: image-post-deteccao-documentos-tempo-real-camera-react-native.jpg
---

Fala pessoal, tudo bem?

Hoje venho falar com vocês sobre a viabilidade de fazer a detecção de documentos utilizando a biblioteca React Native Fast OpenCV

### Vamos lá?

Ao usar o [Vision Camera](https://react-native-vision-camera.com) juntamente com o [Fast OpenCV](https://github.com/lukaszkurantdev/react-native-fast-opencv) em aplicativos React Native, temos a possibilidade de criar diversas funcionalidades para detectação de objetos em tempo real. Uma dessas funcionalidades é a detecção e digitalização de documentos, que pode ser utilizada, por exemplo, para digitalizar faturas, identificar documentos ou formulários. Neste artigo, mostrarei como criar um processador de frames com o Vision Camera para permitir a detecção de documentos em tempo real usando a câmera do dispositivo.

### Instalação

O processo de instalção é simples, para isso vamos utilizar as seguntes bibliotecas:

- react-native-fast-opencv
- react-native-vision-camera
- vision-camera-resize-plugin
- @shopify/react-native-skia

### Processamento dos frames

Como a biblioteca FastOpenCV permite criar funcionalidades avançadas usando JavaScript, essa é a abordagem que adotei, pode ter outras melhores.


#### Passo 1: Preparando

O framework fornecido contém a função responsável por criar o processador de frames juntamente com as dependências necessárias. Aqui, tenho tanto a definição das cores para marcar o objeto detectado a partir da biblioteca Skia.

~~~javascript
import type { SkPoint } from '@shopify/react-native-skia';
import { PaintStyle, PointMode, Skia, vec } from '@shopify/react-native-skia';
import type { PointVector } from 'react-native-fast-opencv';
import {
  ColorConversionCodes,
  ContourApproximationModes,
  MorphShapes,
  MorphTypes,
  ObjectType,
  OpenCV,
  RetrievalModes,
} from 'react-native-fast-opencv';
import { useSkiaFrameProcessor } from 'react-native-vision-camera';
import { useResizePlugin } from 'vision-camera-resize-plugin';

const paint = Skia.Paint();
const border = Skia.Paint();

paint.setStyle(PaintStyle.Fill);
paint.setColor(Skia.Color(0x66e7a649));
border.setStyle(PaintStyle.Fill);
border.setColor(Skia.Color(0xffe7a649));
border.setStrokeWidth(4);

type Point = { x: number; y: number };

export const useDocumentScannerProcessor = () => {
  const { resize } = useResizePlugin();

  const frameProcessor = useSkiaFrameProcessor((frame) => {
    'worklet';
  });
};
~~~

#### Passo 2: Redimensionamento

Calculei com qual proporção escalarei a foto. Redimensionar e reformatar permite um processamento mais simples através da biblioteca FastOpenCV.

~~~javascript
const ratio = 500 / frame.width;
const height = frame.height * ratio;
const width = frame.width * ratio;

const resized = resize(frame, {
  dataType: 'uint8',
  pixelFormat: 'bgr',
  scale: {
    height: height,
    width: width,
  },
});
~~~

#### Passo 3: Pré-processamento e filtros

Os próximos passos são o pré-processamento com as seguintes funções:

- cvtColor — alterei a cor da imagem para tons de cinza.
- morphologyEx — tratamento na imagem.
- GaussianBlur — apliquei um desfoque na foto.
- Canny — marquei as bordas visíveis da foto.

~~~javascript
const source = OpenCV.frameBufferToMat(height, width, 3, resized);

OpenCV.invoke(
  'cvtColor',
  source,
  source,
  ColorConversionCodes.COLOR_BGR2GRAY,
);

const kernel = OpenCV.createObject(ObjectType.Size, 4, 4);
const blurKernel = OpenCV.createObject(ObjectType.Size, 7, 7);
const structuringElement = OpenCV.invoke(
  'getStructuringElement',
  MorphShapes.MORPH_ELLIPSE,
  kernel,
);

OpenCV.invoke(
  'morphologyEx',
  source,
  source,
  MorphTypes.MORPH_OPEN,
  structuringElement,
);
OpenCV.invoke(
  'morphologyEx',
  source,
  source,
  MorphTypes.MORPH_CLOSE,
  structuringElement,
);
OpenCV.invoke('GaussianBlur', source, source, blurKernel, 0);
OpenCV.invoke('Canny', source, source, 75, 100);
~~~

#### Passo 4: Detecção de contornos

Detectei os objetos em cada frame.

~~~javascript
const contours = OpenCV.createObject(ObjectType.PointVectorOfVectors);

OpenCV.invoke(
  'findContours',
  source,
  contours,
  RetrievalModes.RETR_LIST,
  ContourApproximationModes.CHAIN_APPROX_SIMPLE,
);

const contoursMats = OpenCV.toJSValue(contours);
~~~

#### Passo 5: Detecção do documento

Encontrei o maior objeto na imagem que atenda aos critérios estabelecidos, presumivelmente é o documento.

~~~javascript
let biggestContour = null;
let maxArea = 0;

for (let i = 0; i < contoursMats.length; i++) {
  const contour = contoursMats[i];
  const area = OpenCV.invoke('contourArea', contour);

  if (area > maxArea) {
    const perimeter = OpenCV.invoke('arcLength', contour, true);
    const approx = OpenCV.createObject(ObjectType.PointVector);
    OpenCV.invoke(
      'approxPolyDP',
      contour,
      approx,
      0.02 * perimeter,
      true,
    );

    if (OpenCV.invoke('isContourConvex', approx) && approx.size() === 4) {
      biggestContour = approx;
      maxArea = area;
    }
  }
}
~~~

#### Passo 6: Implemento a borda ao documento

Utilizei a biblioteca Skia para desenhar uma borda ao redor do documento detectado, para destaca-lo.

~~~javascript
if (greatestPolygon) {
  const points: Point[] = OpenCV.toJSValue(greatestPolygon).array;

  if (points.length === 4) {
    const path = Skia.Path.Make();
    const pointsToShow: SkPoint[] = [];

    const lastPointX = points[3].x / ratio;
    const lastPointY = points[3].y / ratio;

    path.moveTo(lastPointX, lastPointY);
    pointsToShow.push(vec(lastPointX, lastPointY));

    for (let index = 0; index < 4; index++) {
      const pointX = points[index].x / ratio;
      const pointY = points[index].y / ratio;

      path.lineTo(pointX, pointY);
      pointsToShow.push(vec(pointX, pointY));
    }

    path.close();

    frame.drawPath(path, paint);
    frame.drawPoints(PointMode.Polygon, pointsToShow, border);
  }
}

OpenCV.clearBuffers();
~~~

### Conclusão

Com essas bibliotecas, consegui detectar objetos em fotos de forma eficiente, o que é particularmente útil para digitalizar documentos. O próximo passo seria transformar o objeto detectado em uma imagem separada para armazenamento ou processamento adicional. Quem sabe no próximo post eu faça isso? Continue acompanhando.
