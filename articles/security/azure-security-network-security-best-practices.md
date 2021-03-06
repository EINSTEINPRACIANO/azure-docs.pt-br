---
title: Práticas recomendadas de segurança de rede - Microsoft Azure
description: Este artigo fornece um conjunto de práticas recomendadas de segurança de rede usando recursos internos do Azure.
services: security
author: TerryLanfear
manager: barbkess
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/05/2019
ms.author: TomSh
ms.openlocfilehash: f36658d57fb514ff279c39e9cb1644cf56341ec4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65761828"
---
# <a name="azure-best-practices-for-network-security"></a>Práticas recomendadas do Azure para segurança de rede
Este artigo descreve uma coleção de melhores práticas do Azure para aprimorar a segurança da rede. Essas práticas recomendadas derivam da nossa experiência de rede do Azure e da experiência de clientes como você.

Para cada prática recomendada, este artigo explica:

* O que é a prática recomendada
* Por que é ideal habilitar essa prática recomendada
* O que poderá acontecer se você não habilitar a prática recomendada
* Possíveis alternativas à prática recomendada
* Como você pode aprender a habilitar a prática recomendada

Essas práticas recomendadas são com base em um consenso e recursos da plataforma Azure e conjuntos de recursos, como existiam no momento em que este artigo foi escrito. As opiniões e as tecnologias mudam ao longo do tempo e este artigo será atualizado regularmente para refletir essas alterações.

## <a name="use-strong-network-controls"></a>Use os controles de rede de alta segurança
Você pode conectar [máquinas virtuais (VMs) do Azure](https://azure.microsoft.com/services/virtual-machines/) e dispositivos a outros dispositivos em rede, colocando-os em [redes virtuais do Azure](https://docs.microsoft.com/azure/virtual-network/). Ou seja, você pode conectar cartões de interface de rede virtual a uma rede virtual para permitir comunicações baseadas em TCP / IP entre dispositivos habilitados para rede. Máquinas virtuais conectadas a uma rede virtual do Azure podem se conectar a dispositivos na mesma rede virtual, em diferentes redes virtuais, na Internet ou em suas próprias redes locais.

Ao planejar sua rede e a segurança da sua rede, recomendamos que você centralize o:

- Gerenciamento de funções de rede principal como o ExpressRoute, rede virtual e sub-rede de provisionamento e endereçamento IP.
- Governança de elementos de segurança de rede, como funções de solução de virtualização de rede, como o ExpressRoute, rede virtual e sub-rede de provisionamento e endereçamento IP.

Se você usar um conjunto comum de ferramentas de gerenciamento para monitorar sua rede e a segurança da sua rede, você obtém visibilidade clara em ambos. Uma estratégia de segurança simples e unificada reduz erros porque ele aumenta a compreensão humana e a confiabilidade de automação.

## <a name="logically-segment-subnets"></a>Segmentar logicamente as sub-redes
Redes virtuais do Azure são semelhantes às redes locais em sua rede local. A ideia por trás de uma rede virtual do Azure é que você crie uma rede, com base em um único IP espaço de endereço privado, no qual você pode colocar todas as suas máquinas virtuais do Azure. Os espaços de endereço IP privados disponíveis estão nos intervalos de Classe A (10.0.0.0/8), de Classe B (172.16.0.0/12) e de Classe C (192.168.0.0/16).

As práticas recomendadas para segmentar logicamente sub-redes incluem:

**Melhor prática**: Não atribua permitem regras com intervalos amplos (por exemplo, permitir 0.0.0.0 por meio de 255.255.255.255).  
**Detalhe**: Certifique-se procedimentos de solução de problemas não recomendamos ou banir como configurar esses tipos de regras. Essas regras levar a uma falsa sensação de segurança e com frequência são encontradas e exploradas por equipes vermelhas.

**Melhor prática**: Segmentar o espaço de endereço maior em sub-redes.   
**Detalhe**: Use princípios de sub-redes baseados no [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) para criar suas sub-redes.

**Melhor prática**: Criar controles de acesso à rede entre sub-redes. O roteamento entre sub-redes acontece automaticamente e você não precisa configurar manualmente as tabelas de roteamento. Por padrão, não há nenhum controle de acesso de rede entre as sub-redes que você cria em uma rede virtual do Azure.   
**Detalhe**: Use uma [grupo de segurança de rede](../virtual-network/virtual-networks-nsg.md) para proteger contra o tráfego não solicitado em sub-redes do Azure. Grupos de segurança de rede são dispositivos de inspeção de pacotes com monitoração de estado simples que usam a abordagem de 5 tuplas (IP de origem, porta de origem, IP de destino, porta de destino e protocolo de camada 4) para criar regras permitir/negar o tráfego de rede. Você permite ou nega o tráfego de e para um único endereço IP, de e para vários endereços IP ou para e de sub-redes inteiras.

Quando você usa grupos de segurança de rede para controle de acesso de rede entre sub-redes, você pode colocar recursos que pertencem à mesma zona de segurança ou função em suas próprias sub-redes.

**Melhor prática**: Evite pequenas redes virtuais e sub-redes para garantir que a simplicidade e flexibilidade.   
**Detalhe**: A maioria das organizações adicionar mais recursos que o planejado inicialmente, e novamente a alocação de endereços é trabalhoso. Usando sub-redes pequeno adiciona o valor de segurança limitada e mapeamento de um grupo de segurança de rede para cada sub-rede adiciona sobrecarga. Defina sub-redes em larga escala para garantir que você tem flexibilidade para o crescimento.

**Melhor prática**: Simplificar o gerenciamento de regra de grupo de segurança de rede, definindo [grupos de segurança de aplicativo](https://docs.microsoft.com/rest/api/virtualnetwork/applicationsecuritygroups).  
**Detalhe**: Defina um grupo de segurança de aplicativo para listas de endereços IP que você acha que podem ser alteradas no futuro ou ser usadas em vários grupos de segurança de rede. Certifique-se ao nome de segurança do aplicativo grupos claramente então outras pessoas podem compreender seu conteúdo e a finalidade.

## <a name="adopt-a-zero-trust-approach"></a>Adotar uma abordagem de confiança do Zero
Redes com base em perímetro operam na suposição de que todos os sistemas em uma rede podem ser confiáveis. Mas funcionários atuais acessem recursos da sua organização de qualquer lugar em uma variedade de dispositivos e aplicativos, que torna perímetro controles de segurança irrelevante. As políticas de acesso que se concentram apenas em quem podem acessar um recurso não são suficientes. Para dominar o equilíbrio entre segurança e produtividade, os administradores de segurança também precisará considerar *como* um recurso que está sendo acessado.

Redes precisam evoluir de defesas tradicionais porque redes podem estar vulneráveis a violações: um invasor pode comprometer um único ponto de extremidade dentro do limite confiável e, em seguida, expandir rapidamente a uma presença em toda a rede. [Zero a relação de confiança](https://www.microsoft.com/security/blog/2018/06/14/building-zero-trust-networks-with-microsoft-365/) redes eliminam o conceito de confiança com base no local de rede dentro de um perímetro. Em vez disso, arquiteturas de confiança Zero usam declarações de confiança de dispositivo e de usuário como acesso a dados organizacionais e recursos. Para novas iniciativas, adote as abordagens de confiança de Zero que validar a confiança no momento do acesso.

Práticas recomendadas são:

**Melhor prática**: Conceder acesso condicional aos recursos com base em dispositivo, identidade, garantia, local de rede e muito mais.  
**Detalhe**: [Acesso condicional do Azure AD](../active-directory/conditional-access/overview.md) permite que você aplicar os controles de acesso correto ao implementar as decisões de controle de acesso automatizado com base nas condições necessárias. Para obter mais informações, consulte [gerenciar o acesso ao gerenciamento do Azure com acesso condicional](../role-based-access-control/conditional-access-azure-management.md).

**Melhor prática**: Habilite o acesso de porta somente após a aprovação do fluxo de trabalho.  
**Detalhe**: Você pode usar [acesso VM just-in-time na Central de segurança do Azure](../security-center/security-center-just-in-time.md) para bloquear o tráfego de entrada para suas VMs do Azure, reduzindo a exposição a ataques, fornecendo acesso fácil para se conectar às VMs quando necessário.

**Melhor prática**: Conceder permissões temporárias para executar tarefas com privilégios, que impede que os usuários não autorizados ou mal-intencionados obtenham acesso depois que as permissões tenham expirado. O acesso é concedido somente quando os usuários precisam dele.  
**Detalhe**: Use o acesso just-in-time no Azure AD Privileged Identity Management, ou em uma solução de terceiros para conceder permissões para executar tarefas com privilégios.

Nenhuma relação de confiança é a próxima evolução em segurança de rede. O estado de ataques cibernéticos orienta as organizações a mentalidade "supor uma violação" ao, mas essa abordagem não deve ser restritivos. Zero redes de confiança protejam dados corporativos e recursos, garantindo que as organizações podem criar um local de trabalho moderna por meio de tecnologias que capacitam os funcionários sejam produtivos a qualquer momento, em qualquer lugar, de forma alguma.

## <a name="control-routing-behavior"></a>Controlar o comportamento de roteamento
Quando você coloca uma máquina virtual em uma rede virtual do Azure, a VM pode se conectar a qualquer outra VM na mesma rede virtual, mesmo se as outras VMs estiverem em sub-redes diferentes. Isso é possível porque uma coleção de rotas do sistema ativadas por padrão permite esse tipo de comunicação. Essas rotas padrão permitem que as VMs na mesma rede virtual iniciem conexões umas com as outras e com a Internet (somente para comunicações de saída com a Internet).

Embora as rotas do sistema padrão sejam úteis para muitos cenários de implantação, há momentos em que você deseja personalizar a configuração de roteamento para suas implantações. Você pode configurar o endereço do próximo salto para alcançar destinos específicos.

Recomendamos que você configure [rotas definidas pelo usuário](../virtual-network/virtual-networks-udr-overview.md) ao implantar um dispositivo de segurança para uma rede virtual. Podemos falar sobre isso em uma seção posterior intitulada [proteger seus recursos essenciais do serviço do Azure para apenas suas redes virtuais](azure-security-network-security-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

> [!NOTE]
> Rotas definidas pelo usuário não são obrigatórias e as rotas de sistema padrão funcionam normalmente.
>
>

## <a name="use-virtual-network-appliances"></a>Usar dispositivos de rede virtual
Grupos de segurança de rede e roteamento definido pelo usuário podem fornecer uma determinada medida de segurança de rede nas camadas de rede e transporte do [modelo OSI](https://en.wikipedia.org/wiki/OSI_model). Mas, em algumas situações, você quer ou precisa ativar a segurança em níveis altos da pilha. Em tais situações, é recomendável que você implante dispositivos de segurança de rede virtual fornecidos por parceiros do Azure.

Os dispositivos de segurança de rede do Azure podem oferecer uma segurança melhor do que os controles no nível da rede. Os recursos de segurança de rede dos dispositivos de segurança de rede virtual incluem:


* Firewall
* Prevenção de intrusão/detecção de intrusões
* Gerenciamento de vulnerabilidades
* Controle de aplicativo
* Detecção de anomalias baseada em rede
* Filtragem da Web
* Antivírus
* Proteção botnet

Para encontrar dispositivos de segurança de rede virtual do Azure disponíveis, vá para [Azure Marketplace](https://azure.microsoft.com/marketplace/) e pesquise por "segurança" e "segurança de rede".

## <a name="deploy-perimeter-networks-for-security-zones"></a>Implantação de redes de perímetro para zonas de segurança
Uma [rede de perímetro](https://docs.microsoft.com/azure/best-practices-network-security) (também conhecida como DMZ) é um segmento de rede físico ou lógico que fornece uma camada adicional de segurança entre seus ativos e a Internet. Os dispositivos especializados de controle de acesso à rede na extremidade de uma rede de perímetro permitem apenas o tráfego desejado na sua rede virtual.

As redes de perímetro são úteis porque você pode concentrar seu gerenciamento, monitoramento, registro e geração de relatórios de controle de acesso de rede nos dispositivos na borda da rede virtual do Azure. Uma rede de perímetro é onde você normalmente permitem distribuída de negação de prevenção de serviço (DDoS), sistemas de prevenção de intrusão/detecção de intrusões (IDS/IPS), as regras de firewall e políticas, filtragem da web, rede antimalware e muito mais. Os dispositivos de segurança de rede ficam entre a Internet e sua rede virtual do Azure e têm uma interface nas duas redes.

Embora esse seja o design básico de uma rede de perímetro, existem muitos designs diferentes, como em frente e verso, tri-homed e multihomed.

Com base no conceito de confiança Zero mencionado anteriormente, recomendamos que você considere o uso de uma rede de perímetro para todas as implantações de alta segurança para aumentar o nível de controle de acesso e segurança de rede para recursos do Azure. Você pode usar o Azure ou uma solução de terceiros para fornecer uma camada adicional de segurança entre seus ativos e a internet:

- Controles nativos do Azure. [Firewall do Azure](../firewall/overview.md) e o [firewall do aplicativo web no Gateway de aplicativo](../application-gateway/overview.md#web-application-firewall) oferecer segurança básica com um firewall com monitoração de estado totalmente como um serviço, a alta disponibilidade interna, a escalabilidade de nuvem sem restrições, a filtragem de FQDN , suporte para conjuntos de regras principais do OWASP e uma instalação simples e configuração.
- Ofertas de terceiros. Pesquisa o [do Azure Marketplace](https://azuremarketplace.microsoft.com/) para firewall de última geração (NGFW) e outras ofertas de terceiros que fornecem ferramentas de segurança familiar e níveis consideravelmente aprimorados de segurança de rede. Configuração pode ser mais complexa, mas uma oferta de terceiros pode permitir que você use conjuntos de habilidades e os recursos existentes.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Evitar a exposição à internet com links WAN dedicados
Muitas organizações escolheram a rota de TI híbrida. Com TI híbrida, alguns dos ativos de informações da empresa estão no Azure e outros permanecem no local. Em muitos casos, alguns componentes de um serviço estão sendo executados no Azure, enquanto outros componentes permanecem no local.

Em um cenário de TI híbrida, normalmente há algum tipo de conectividade entre locais. A conectividade entre instalações permite que a empresa conecte suas redes locais às redes virtuais do Azure. Duas soluções de conectividade entre locais estão disponíveis:

* [VPN site a site](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md). é uma tecnologia estabelecida e confiável, mas a conexão ocorre pela Internet. Largura de banda é limitada a um máximo de aproximadamente 1,25 Gbps. VPN site a site é uma opção desejável em alguns cenários.
* **O Azure ExpressRoute**. é recomendável usar o [ExpressRoute](../expressroute/expressroute-introduction.md) para a conectividade entre locais. O ExpressRoute permite que você estenda suas redes locais até a nuvem da Microsoft por meio de conexão privada facilitada por um provedor de conectividade. Com o ExpressRoute, você pode estabelecer conexões com serviços de nuvem da Microsoft, como o Azure, Office 365 e Dynamics 365. O ExpressRoute é uma WAN dedicada link entre seu local ou um provedor de hospedagem do Microsoft Exchange. Como esta é uma conexão de telecomunicações, seus dados não viajam pela internet, portanto, ele não é exposto aos riscos potenciais de comunicação com a internet.

O local da sua conexão de ExpressRoute pode afetar a capacidade de firewall, escalabilidade, confiabilidade e visibilidade do tráfego de rede. Você precisará identificar onde finalizar a ExpressRoute de redes de existentes (local). Você pode:

- Encerrar fora do firewall (o paradigma de rede de perímetro), se você precisar de visibilidade sobre o tráfego, se você precisar continuar uma prática existente do isolamento de datacenters, ou se você estiver colocando exclusivamente os recursos da extranet no Azure.
- Encerrar dentro do firewall (o paradigma de extensão de rede). Essa é a recomendação de padrão. Em todos os outros casos, é recomendável tratar do Azure como um datacenter enésimo.

## <a name="optimize-uptime-and-performance"></a>Otimizar o desempenho e o tempo de atividade
Se um serviço estiver inativo, as informações não poderão ser acessadas. Se o desempenho é tão ruim que os dados não podem ser usados, você pode considerar os dados como inacessíveis. Do ponto de vista de segurança, você precisa fazer o que puder para garantir que seus serviços tenham tempo de atividade e desempenho ideais.

Um método popular e eficaz para melhorar a disponibilidade e o desempenho é o balanceamento de carga. O balanceamento de carga é um método de distribuição de tráfego de rede entre servidores que fazem parte de um serviço. Por exemplo, se você tiver servidores Web de front-end como parte de seu serviço, poderá usar o balanceamento de carga para distribuir o tráfego entre vários servidores Web de front-end.

Essa distribuição de tráfego aumenta a disponibilidade porque, se um dos servidores Web ficar indisponível, o balanceador de carga interromperá o tráfego de envio para esse servidor e redirecionará o tráfego para os servidores que ainda estiverem online. O balanceamento de carga também ajuda no desempenho porque a sobrecarga do processador, da rede e da memória para atender às solicitações é distribuída por todos os servidores com carga balanceada.

É recomendável implantar o balanceamento de carga sempre que possível e conforme apropriado para seus serviços. A seguir estão os cenários no nível da rede virtual do Azure e no nível global, juntamente com as opções de balanceamento de carga para cada um.

**Cenário**: você tem um aplicativo que:

- Requer solicitações da mesma sessão de usuário / cliente para alcançar a mesma máquina virtual de back-end. Exemplos disso são aplicativos de carrinho de compras e servidores de webmail.
- Aceita apenas uma conexão segura, portanto, a comunicação não criptografada com o servidor não é uma opção aceitável.
- Exige que várias solicitações HTTP na mesma conexão TCP de longa duração sejam roteadas ou carregadas com balanceamento para diferentes servidores de back-end.

**Opção de balanceamento de carga**: use o [Gateway de Aplicativo do Azure](../application-gateway/application-gateway-introduction.md), um balanceador de carga de tráfego da Web HTTP. O Application Gateway suporta criptografia SSL de ponta a ponta e [terminação SSL](../application-gateway/application-gateway-introduction.md) no gateway. Os servidores da Web podem então ser aliviados da sobrecarga de criptografia e descriptografia e do fluxo de tráfego não criptografado para os servidores de back-end.

**Cenário**: você precisa balancear a carga das conexões de entrada da Internet entre seus servidores localizados em uma rede virtual do Azure. Os cenários são quando você:

- Tiver aplicativos sem monitoração de estado que aceitam solicitações de entrada da internet.
- Não requerem sessões Autoadesivas ou descarregamento SSL. As sessões temporárias é um método usado com balanceamento de carga do aplicativo, para alcançar a afinidade do servidor.

**Opção de balanceamento de carga**: use o portal do Azure para [criar um balanceador de carga externo](../load-balancer/quickstart-create-basic-load-balancer-portal.md) que distribua solicitações de entrada em várias VMs para fornecer um nível mais alto de disponibilidade.

**Cenário**: é necessário balancear a carga de conexões de VMs que não estão na Internet. Na maioria dos casos, as conexões aceitas para balanceamento de carga são iniciadas por dispositivos em uma rede virtual do Azure, como instâncias do SQL Server ou servidores da Web internos.   
**Opção de balanceamento de carga**: use o portal do Azure para [criar um balanceador de carga interno](../load-balancer/quickstart-create-basic-load-balancer-powershell.md) que distribua solicitações de entrada em várias VMs para fornecer um nível mais alto de disponibilidade.

**Cenário**: você precisa de balanceamento de carga global porque:

- Tenha uma solução em nuvem amplamente distribuída em várias regiões e que exija o mais alto nível de disponibilidade (disponibilidade) possível.
- É necessário o mais alto nível de tempo de atividade possível para garantir que seu serviço esteja disponível mesmo se um datacenter inteiro ficar indisponível.

**Opção de balanceamento de carga**: Use o Gerenciador de Tráfego do Azure. O Gerenciador de Tráfego possibilita o balanceamento de carga das conexões para seus serviços com base na localização do usuário.

Por exemplo, se o usuário fizer uma solicitação ao seu serviço a partir da UE, a conexão será direcionada para os serviços localizados em um datacenter da UE. Esta parte do balanceamento de carga global do Gerenciador de Tráfego ajuda a melhorar o desempenho porque a conexão com o datacenter mais próximo é mais rápida do que conectar-se a data centers que estejam distantes.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>Desativar o acesso RDP / SSH às máquinas virtuais
É possível acessar as máquinas virtuais do Azure usando [Protocolo RDP (Remote Desktop Protocol)](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) e o protocolo [Secure Shell ](https://en.wikipedia.org/wiki/Secure_Shell) (SSH). Esses protocolos permitem o gerenciamento de VMs de locais remotos e são padrão na computação de data center.

O problema de segurança em potencial ao usar esses protocolos pela Internet é que os invasores podem usar as técnicas de [força bruta](https://en.wikipedia.org/wiki/Brute-force_attack) para obter acesso às máquinas virtuais do Azure. Depois que os invasores tiverem acesso, eles poderão usar sua VM como ponto de partida para comprometer outras máquinas em sua rede virtual ou até mesmo atacar dispositivos em rede fora do Azure.

Recomendamos que você desabilite o acesso RDP e SSH direto às máquinas virtuais do Azure a partir da Internet. Depois que o acesso RDP e SSH direto da Internet for desativado, você terá outras opções que podem ser usadas para acessar essas VMs para gerenciamento remoto.

**Cenário**: permite que um único usuário se conecte a uma rede virtual do Azure pela Internet.   
**Opção**: A [VPN ponto a site](../vpn-gateway/vpn-gateway-point-to-site-create.md) é outro termo para uma conexão de cliente/servidor de VPN de acesso remoto. Depois que a conexão ponto a ponto é estabelecida, o usuário pode usar RDP ou SSH para se conectar a qualquer VM localizada na rede virtual do Azure à qual o usuário se conectou por meio de VPN ponto a ponto. Isso pressupõe que o usuário está autorizado a alcançar essas VMs.

A VPN ponto-a-ponto é mais segura do que as conexões RDP ou SSH diretas porque o usuário precisa autenticar duas vezes antes de se conectar a uma VM. Primeiro, o usuário precisa autenticar (e ser autorizado) para estabelecer a conexão VPN ponto-a-ponto. Segundo, o usuário precisa autenticar (e ser autorizado) para estabelecer a sessão RDP ou SSH.

**Cenário**: permite que os usuários em sua rede local se conectem a VMs em sua rede virtual do Azure.   
**Opção**: Uma [VPN site a site](../vpn-gateway/vpn-gateway-site-to-site-create.md) conecta uma rede inteira a outra rede pela Internet. Você pode usar uma VPN site a site para conectar sua rede local a uma rede virtual do Azure. Os usuários em sua rede local se conectam usando o protocolo RDP ou SSH pela conexão VPN site a site. Você não precisa permitir acesso RDP ou SSH direto pela Internet.

**Cenário**: use um link WAN dedicado para fornecer funcionalidade semelhante à VPN site a site.   
**Opção**: Use [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/). Ele fornece uma funcionalidade semelhante à VPN site a site. As principais diferenças são:

- O link dedicado da WAN não atravessa a Internet.
- Os links de WAN dedicados geralmente são mais estáveis e apresentam melhor desempenho.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Proteja seus recursos de serviço críticos do Azure somente para suas redes virtuais
Use pontos de extremidade de serviço de rede virtual para estender o espaço de endereço privado de sua rede virtual e a identidade de sua rede virtual para os serviços do Azure, por meio de uma conexão direta. Os pontos de extremidade permitem que você possa garantir os recursos essenciais do serviço do Azure somente para suas redes virtuais. O tráfego da sua rede virtual para o serviço do Azure sempre permanece na rede de backbone do Microsoft Azure.

Os pontos de extremidade de serviço fornecem os seguintes benefícios:

- **Segurança aprimorada para os recursos do serviço do Azure**: Com os pontos de extremidade de serviço, os recursos do serviço do Azure podem ser garantidos para sua rede virtual. A proteção de recursos de serviço a uma rede virtual fornece segurança aprimorada, removendo totalmente o acesso público à Internet a recursos e permitindo o tráfego somente de sua rede virtual.
- **Roteamento de tráfego ideal para o serviço do Azure de sua rede virtual**: Todas as rotas em sua rede virtual que forçam o tráfego de Internet para dispositivos locais e/ou virtuais, conhecido como túnel forçado, também forçam o tráfego do serviço do Azure para seguir a mesma rota que o tráfego de Internet. Os pontos de extremidade de serviço oferecem o roteamento ideal para o tráfego do Azure.

  Os pontos de extremidade sempre levam o tráfego de serviço diretamente de sua rede virtual para o serviço na rede do backbone do Azure. Manter o tráfego na rede de backbone do Azure permite continuar a auditar e monitorar o tráfego de saída da Internet de suas redes virtuais, por meio de encapsulamento forçado, sem afetar o tráfego de serviço. Saiba mais sobre [rotas definidas pelo usuário e encapsulamento forçado](../virtual-network/virtual-networks-udr-overview.md).

- **Simples de configurar com menos sobrecarga de gerenciamento**: Você não precisa mais de endereços IP públicos reservados nas suas redes virtuais para garantir recursos do Azure pelo firewall do IP. Não é obrigatório nenhum dispositivo NAT ou de gateway para configurar os pontos de extremidade de serviço. Pontos de extremidade de serviço são configurados por meio de um clique simples em uma sub-rede. Não há sobrecarga adicional para manter os terminais.

Para saber mais sobre os pontos de extremidade de serviços e os serviços e regiões do Azure aos quais os pontos de extremidade de serviço estão disponíveis, consulte [Pontos de extremidade de serviço de rede virtual](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="next-steps"></a>Próximas etapas
Veja [Melhores práticas e padrões de segurança do Azure](security-best-practices-and-patterns.md) para obter melhores práticas segurança complementares a serem usadas ao projetar, implantar e gerenciar as soluções de nuvem, usando o Azure.
