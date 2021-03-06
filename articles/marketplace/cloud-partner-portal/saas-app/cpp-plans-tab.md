---
title: Os planos de oferta de aplicativo SaaS do Azure | O Azure Marketplace
description: Crie um plano para uma oferta de aplicativo SaaS no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pabutler
ms.openlocfilehash: 2ff86b39f67b170ce99b045f5cfa888e06057bbe
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943338"
---
# <a name="saas-application-plans-tab"></a>Guia Planos de aplicativo SaaS

Use a guia Planos para criar um plano. Pelo menos um plano precisará ser adicionado se você estiver usando a opção Venda por meio da Microsoft para o aplicativo SaaS.

![Criar um plano](./media/saas-plans-new-plan.png)

## <a name="create-a-new-plan"></a>Criar um plano

Para criar um plano:

1. Em **Planos**, selecione **+ Novo Plano**
2. Na janela pop-up **Novo Plano**, digite uma **ID do Plano**. O tamanho máximo é 50 caracteres. Essa ID precisa conter somente letras minúsculas, caracteres alfanuméricos, traços ou sublinhados. Depois que a oferta for publicada, você não poderá mais alterar essa ID.
3. Selecione **OK** para salvar a ID do Plano.

   ![ID do novo plano](./media/saas-plans-plan-ID.png)

### <a name="to-configure-the-plan"></a>Para configurar o plano:

1. Em **Detalhes do Plano**, forneça informações para os seguintes campos:

   - Título – forneça um título para o plano. O título é limitado a 50 caracteres.
   - Descrição – forneça uma descrição. A descrição é limitada a até 500 caracteres.
   - Esse é um plano privado? – se o plano estiver disponível apenas para um grupo seleto de clientes, selecione **Sim**.
   - Disponibilidade de país/região – o plano precisa estar disponível para pelo menos um país ou região. Clique em **Selecionar regiões**. Escolha um país/região na lista **Selecionar disponibilidade no país/região** e, em seguida, clique em **OK**. 
   - Preço Herdado – forneça o custo, em USD por mês.

2. Em **Preço em Moeda Simplificado**, forneça as seguintes informações:

   - Prazo de Cobrança – o preço mensal é selecionado por padrão. Você também pode fornecer o preço anual.
   - Preço Mensal – forneça o preço mensal, que precisa corresponder ao preço herdado.

   >[!NOTE] 
   >Se você adicionar Anuidade no prazo de cobrança, será solicitado o **Anuidade** em USD por ano.

3. Selecione **Salvar** para concluir a configuração do plano.

   >[!NOTE] 
   >Depois de salvar as alterações nos preços você poderá exportar/importar dados de preço.

![Configurar detalhes do plano](./media/saas-plans-plan-details.png)

## <a name="next-steps"></a>Próximas etapas

[Guia Informações do Canal](./cpp-channel-info-tab.md)

