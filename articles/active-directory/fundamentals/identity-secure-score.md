---
title: O que é a pontuação segura de identidade? - Azure Active Directory
description: Como você pode usar a pontuação segura de identidade para melhorar a postura de segurança do seu diretório
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tilarso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cdff2305914ca6e4144f7784d1a60026a1d27c0
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988717"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory"></a>O que é a classificação de segurança de identidade no Azure Active Directory?

Quão seguro é seu locatário do Azure AD? Se você não souber como responder a essa pergunta, este artigo explica como a pontuação de identidade seguro ajuda a monitorar e melhorar sua postura de segurança de identidade.

## <a name="what-is-an-identity-secure-score"></a>O que é uma classificação de segurança de identidade?

A pontuação segura de identidade é um número entre 1 e 223 que funciona como um indicador de como está alinhado com práticas recomendadas da Microsoft para segurança. Cada ação de melhoria na pontuação de identidade seguro é personalizada para sua configuração específica.  

![Classificação de segurança](./media/identity-secure-score/identity-secure-score-overview.png)

A classificação ajuda você a:

- Medir objetivamente a sua postura de segurança de identidade
- Planejar aprimoramentos de segurança de identidade
- Examinar o sucesso das suas melhorias

Você pode acessar a classificação e informações relacionadas a no painel de classificação de segurança de identidade. Neste painel, você encontrará:

- Sua pontuação de seguro de identidade
- Um gráfico de comparação mostrando como sua pontuação de identidade segura se compara a outros locatários no mesmo setor e tamanho semelhante
- Um gráfico de tendência que mostra como sua pontuação de seguro de identidade foi alterada ao longo do tempo
- Uma lista de possíveis aprimoramentos

Ao seguir as ações de melhoria, você pode:

- Melhorar sua postura de segurança e sua pontuação
- Aproveite os recursos disponíveis para sua organização como parte de seus investimentos de identidade

## <a name="how-do-i-get-my-secure-score"></a>Como faço para obter minha classificação de segurança?

A pontuação segura de identidade está disponível em todas as edições do AD do Azure. Para acessar sua classificação, vá para o [painel Visão Geral do Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore).

## <a name="how-does-it-work"></a>Como ele funciona?

A cada 48 horas, o Azure analisa sua configuração de segurança e compara suas configurações com as práticas recomendadas. Com base no resultado da avaliação, uma nova pontuação é calculada para seu diretório. É possível que sua configuração de segurança não está totalmente alinhada com diretrizes de práticas recomendadas e as ações de melhoria apenas parcialmente forem atendidas. Nesses cenários, você só receberá uma parte da pontuação máxima disponível para o controle.

Cada recomendação é medida com base na sua configuração do Azure AD. Se você estiver usando os produtos de terceiros para habilitar uma prática recomendada, você pode indicar essa configuração nas configurações de uma ação de melhoria. Você também tem a opção de definir as recomendações a serem ignorados se eles não se aplicam ao seu ambiente. Uma recomendação ignorada não contribui para o cálculo de sua classificação.

![Ignorar ou marcar a ação conforme abordado por terceiros](./media/identity-secure-score/identity-secure-score-ignore-or-third-party-reccomendations.png)

## <a name="how-does-it-help-me"></a>Como isso me ajuda?

A classificação de segurança ajuda você a:

- Medir objetivamente a sua postura de segurança de identidade
- Planejar aprimoramentos de segurança de identidade
- Examinar o sucesso das suas melhorias

## <a name="what-you-should-know"></a>O que você deve saber

### <a name="who-can-use-the-identity-secure-score"></a>Quem pode usar a classificação de segurança de identidade?

A classificação de segurança de identidade pode ser usada pelas funções a seguir:

- Administrador global
- Administrador de segurança
- Leitores de segurança

### <a name="how-are-controls-scored"></a>Como os controles são pontuados?

Controles podem ser pontuados de duas maneiras. Algumas são pontuadas de forma binária - você tem 100% da pontuação de se você tiver o recurso ou configuração definida com base em nossa recomendação. Outras pontuações são calculadas como uma porcentagem da configuração do total. Por exemplo, se a recomendação de melhoria informa você terá 30 pontos se você proteger todos os seus usuários com o MFA e tiver apenas 5 do total de 100 usuários protegidos, você receberia uma pontuação parcial cerca de 2 pontos (5 protegidos / total de 100 * 30 pts máximo = 2 pontuação parcial de pts) .

### <a name="what-does-not-scored-mean"></a>O que [Não Pontuado] significa?

Ações rotuladas como [Não Pontuada] são aquelas você pode executar em sua organização, mas não serão pontuadas porque não estão conectadas à ferramenta (ainda!). Portanto, você ainda pode melhorar sua segurança, mas não receberá crédito por essas ações no momento.

### <a name="how-often-is-my-score-updated"></a>Com que frequência minha classificação é atualizada?

A classificação é calculada uma vez por dia (aproximadamente à 1h PST). Se você fizer uma alteração a uma ação de medida, a classificação será atualizada automaticamente no dia seguinte. Podem ser necessárias até 48 horas para uma alteração ser refletida na sua classificação.

### <a name="my-score-changed-how-do-i-figure-out-why"></a>Minha classificação mudou. Como faço para descobrir por quê?

Vá para o [Central de segurança do Microsoft 365](https://security.microsoft.com/), onde você encontrará sua pontuação segura do Microsoft completa. Você pode ver facilmente todas as alterações à sua pontuação segura, revisando as alterações detalhadas na guia Histórico.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>A pontuação segura medir meu risco de Introdução violado?

Em breve, não. A pontuação segura não expressa uma medida absoluta de como provavelmente você devem obter violado. Expressa a extensão em que você adotou recursos que podem compensar o risco de sofrer uma violação. Nenhum serviço pode garantir que você não será ser violada e a pontuação segura não deve ser interpretada como uma garantia de nenhuma forma.

### <a name="how-should-i-interpret-my-score"></a>Como devo interpretar a minha classificação?

Você recebe pontos para configuração recursos de segurança recomendados ou realizar tarefas relacionadas a segurança (como a leitura de relatórios). Algumas ações são pontuadas para conclusão parcial, como habilitar MFA (autenticação multifator) para seus usuários. Sua pontuação segura diretamente é representativa dos serviços de segurança da Microsoft que você usa. Lembre-se de que a segurança deve ser equilibrada com facilidade de uso. Todos os controles de segurança têm um componente de impacto ao usuário. Controles com baixo impacto ao usuário devem ter pouco ou nenhum efeito sobre as operações diárias dos usuários.

Para ver o histórico da pontuação, vá para o [Central de segurança do Microsoft 365](https://security.microsoft.com/) e examine sua pontuação geral de seguro de Microsoft. Você pode examinar as alterações para geral pontuação segura estar clicando em Exibir histórico. Escolha uma data específica para ver quais controles foram habilitados para aquele dia e que pontos você ganhou para cada um.

### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>Como a classificação de segurança de identidade se relaciona à classificação de segurança do Office 365?

O [Microsoft secure score](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score) contém cinco categorias de pontuação e controle distinto:

- Identidade
- Dados
- Dispositivos
- Infraestrutura
- Aplicativos

A pontuação de identidade seguro representa a parte de identidade da pontuação de seguro de Microsoft. Essa sobreposição significa que suas recomendações para a identidade do secure score e a pontuação de identidade no Microsoft é os mesmos.

## <a name="next-steps"></a>Próximas etapas

[Saiba mais sobre a pontuação segura da Microsoft](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score)
