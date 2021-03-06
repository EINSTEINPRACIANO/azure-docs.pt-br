---
title: Adicionar uma camada de bloco para o Azure Mapas | Microsoft Docs
description: Como adicionar uma camada de bloco ao mapa no Javascript
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 82a6d6b2af7df91696844b09fb7650c547cb6bd1
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108627"
---
# <a name="add-a-heat-map-layer"></a>Adicionar uma camada do mapa de calor

Mapas de calor, também conhecidos como ponto de mapas de densidade, são um tipo de visualização de dados usado para representar a densidade de dados usando um intervalo de cores. Geralmente são usados para mostrar os "pontos de acesso" dos dados em um mapa e são uma ótima maneira de renderizar grandes conjuntos de dados de ponto.  Por exemplo, a renderização de dezenas de milhares de pontos dentro da exibição de mapa como símbolos, aborda a maioria da área do mapa e resultaria em sobreposição trocarem, dificultando a Obtenha muito informações sobre os dados de muitos símbolos. No entanto, visualizar esse mesmo conjunto de dados como um mapa de calor torna mais fácil ver onde os dados de ponto são o densest e a densidade relativa para outras áreas. Há muitos cenários nos quais os mapas de calor são usados. Veja alguns exemplos;

* Dados de temperatura normalmente são renderizados como mapa de calor, pois ele fornece aproximações para a temperatura entre dois pontos de dados.
* Renderização de dados de sensores de ruído, como um mapa de calor não só mostra a intensidade do ruído em que o sensor está, mas também pode fornecer ideias sobre a dissipação de uma distância. O nível de ruído em um site não pode ser alto, no entanto, se a área de cobertura de ruído de vários sensores é sobreposta, é possível que essa área de sobreposição possa enfrentar níveis mais altos de ruído e, portanto, deve estar visível no mapa de calor.
* Visualizando um GPS rastreamento que inclua a velocidade, como um mapa de altura ponderada em que a intensidade de cada ponto de dados se baseia na velocidade é uma ótima maneira de ver onde o veículo foi acelerando.

> [!TIP]
> Por padrão, as camadas do mapa de calor renderizarão as coordenadas de todas as geometrias em uma fonte de dados. Para limitar a camada para que ele apenas renderiza ponto funcionalidades da geometria, defina as `filter` propriedade da camada para `['==', ['geometry-type'], 'Point']` ou `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` se você quiser incluir recursos MultiPoint também.

## <a name="add-a-heat-map-layer"></a>Adicionar uma camada do mapa de calor

Para renderizar uma fonte de dados de pontos como mapa de calor, passe sua fonte de dados para uma instância da classe `HeatMapLayer` e adicione-a ao mapa conforme exibido aqui.

<br/>

<iframe height='500' scrolling='no' title='Camada do Mapa de calor simples' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Caneta <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Camada do Mapa de calor Simples</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

Neste exemplo, cada ponto de calor tem um raio de 10 pixels em todos os níveis de zoom. Ao adicionar a camada do mapa de calor no mapa, este exemplo insere-o abaixo da camada de rótulo para criar uma melhor experiência de usuário, como os rótulos são evidentes acima do mapa de calor. Os dados neste exemplo são originados do [USGS terremoto perigos programa](https://earthquake.usgs.gov/) e representa terremotos significativos que ocorreram nos últimos 30 dias.

## <a name="customizing-the-heat-map-layer"></a>Personalizando a camada do mapa de calor

O exemplo anterior personalizou o mapa de calor, definindo as opções de raio e opacidade. A camada do mapa de calor fornece várias opções de personalização;

* `radius`: Define um raio de pixel no qual renderizar cada ponto de dados. O raio pode ser definido como um número fixo ou como uma expressão. Usando uma expressão, é possível dimensionar o raio com base no nível de zoom, o que é exibido representar uma área espacial consistente no mapa (por exemplo, o raio de 5 milhas).
* `color`: Especifica como o mapa de calor é colorido. Um gradiente de cores é frequentemente usado para mapas de calor e pode ser a atingir com um `interpolate` expressão. Usando um `step` expressão para colorir o mapa de calor divide a densidade visualmente em intervalos que mais ainda se parece com um mapa de estilo do contorno ou radar. Essas paletas de cores definem as cores do valor de densidade mínimo ao máximo. Valores de cor para mapas de calor são especificados como uma expressão sobre o `heatmap-density` valor. A cor no índice 0 em uma expressão de interpolação ou a cor padrão de uma expressão de etapa define a cor da área em que não há nenhum dado e pode ser usado para definir uma cor de plano de fundo. Muitos preferem definir esse valor como transparente ou semitransparente preto. Aqui estão exemplos de expressões de cores;

| Expressão de cores de interpolação | Stepped Color Expression | 
|--------------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,50, 'amarelo',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,75, "vermelhos"<br/>\] | 

* `opacity`: Especifica como opaco ou transparente a camada do mapa de calor.
* `intensity`: Aplica um multiplicador para o peso de cada ponto de dados para aumentar a intensidade geral do mapa de calor e ajuda a tornar as pequenas diferenças no peso de pontos de dados que se tornam mais fáceis de visualizar.
* `weight`: Por padrão, todos os pontos de dados têm um peso de 1, portanto, todos os pontos de dados são ponderados igualmente. A opção de peso atua como uma multiplicadora e pode ser definida como um número ou uma expressão. Se um número fosse definido como o peso, digamos 2, seria o equivalente a colocação de cada ponto de dados no mapa duas vezes, dobrando a densidade. Definir a opção de peso para um número renderiza o mapa de calor de maneira semelhante a usar a opção de intensidade. No entanto, se uma expressão for usada, o peso de cada ponto de dados pode ser com base nas propriedades de cada ponto de dados. Use os dados de terremoto como exemplo. Cada ponto de dados representa um terremoto. Uma métrica importante que cada ponto de dados de terremoto tem é um valor de magnitude. Terremotos ocorrem o tempo todo, mas a maioria tem uma magnitude baixa e não são sentidos. Usando o valor de magnitude em uma expressão para atribuir o peso aos dados de cada ponto será permite terremotos mais significativos ser representado melhor dentro do mapa de calor.
* Além das opções de camada base; mín./máx. de zoom, visíveis e filtrar, há também uma opção `source` se você deseja atualizar a fonte de dados e uma opção `source-layer` se a fonte de dados é uma fonte de bloco do vetor.

Aqui está uma ferramenta para testar as opções de camada do mapa de calor diferentes.

<br/>

<iframe height='700' scrolling='no' title='Opções da Camada do Mapa de Calor' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Caneta <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Opções de Camada de Bloco</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Mapa de calor de cartaz consistente

Por padrão, o raio dos pontos de dados renderizados na camada do mapa de calor têm um raio de pixel fixo para todos os níveis de zoom. Como o mapa é ampliado as agregações de dados juntas e a camada do mapa de calor parece diferente. Um `zoom` expressão pode ser usada para dimensionar o radius para cada nível de zoom, de modo que cada ponto de dados abrange a mesma área física do mapa. Isso tornará a camada do mapa de calor parecer mais estática e consistente. Cada nível de zoom do mapa tem duas vezes mais pixels verticalmente e horizontalmente como o nível de zoom anterior. Dimensionar o radius, de modo que ela duplica a cada nível de zoom, você criará um mapa de calor parece consistente em todos os níveis de zoom. Isso pode ser feito usando o `zoom` com uma base 2 `exponential interpolation` expressão, conforme mostrado no exemplo a seguir. Aplique zoom no mapa para ver como o mapa de calor é dimensionado com o nível de zoom.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa de calor de cartaz consistente" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=light&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte a caneta <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>mapa de calor de cartaz consistente</a> por mapas do Azure (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Habilitar o clustering na fonte de dados, os pontos que estão próximos uns dos outros são agrupados como um ponto em cluster. A contagem de ponto de cada cluster pode ser usada como a expressão de peso para o mapa de calor e reduzir significativamente o número de pontos que precisam ser renderizados. A contagem de ponto de um cluster é armazenada em um `point_count` propriedade do recurso de ponto, conforme mostrado abaixo. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Se o raio de clustering é apenas alguns pixels lá será uma diferença um pouco visual da renderização. Um raio maior agrupará mais pontos em cada cluster e melhorará o desempenho de mapa de calor, mas veja quais serão as diferenças mais notáveis.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Para obter mais exemplos de código para adicionar aos seus mapas, consulte os seguintes artigos:

> [!div class="nextstepaction"]
> [Adicionar uma camada de símbolo](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Usar expressões de estilo controladas por dados](data-driven-style-expressions-web-sdk.md)