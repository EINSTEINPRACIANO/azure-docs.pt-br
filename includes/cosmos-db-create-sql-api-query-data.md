---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 9971b16da42cdf1de0464857291c74a947535735
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151361"
---
Você pode usar consultas no Data Explorer para recuperar e filtrar os dados.

1. Na parte superior da guia **Documentos** no Data Explorer, examine a consulta padrão `SELECT * FROM c`. Essa consulta recupera e exibe todos os documentos na coleção na ordem da ID. 
   
   ![A consulta padrão no Data Explorer é `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Para alterar a consulta, selecione **Editar Filtro**, substitua a consulta padrão por `ORDER BY c._ts DESC` e selecione **Aplicar Filtro**.
   
   ![Altere a consulta padrão adicionando ORDER BY c._ts DESC e clicando Aplicar Filtro](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   A consulta modificada exibe os documentos em ordem decrescente com base em seu carimbo de data/hora, portanto, agora o segundo documento é listado primeiro. 
   
   ![Alterada a consulta para ORDER BY c._ts DESC e clicando em Aplicar Filtro](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

Se você estiver familiarizado com a sintaxe SQL, poderá inserir qualquer [consulta SQL](../articles/cosmos-db/sql-api-sql-query.md) compatível na caixa de predicado de consulta. Você também pode usar o Data Explorer para criar procedimentos armazenados, UDFs e gatilhos para lógica de negócios do lado do servidor. 

O Data Explorer fornece acesso fácil ao portal do Azure para todos os recursos de acesso a dados programático interno por disponível nas APIs. Você também pode usar o portal para dimensionar a taxa de transferência, obter as chaves e as cadeias de conexão e examinar as métricas e os SLAs para sua conta do Azure Cosmos DB. 

