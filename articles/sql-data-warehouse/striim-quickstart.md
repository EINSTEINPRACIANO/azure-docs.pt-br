---
title: Início Rápido do Striim com o SQL Data Warehouse do Azure | Microsoft Docs
description: Comece rapidamente a usar o Striim e o SQL Data Warehouse do Azure.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 8ed9936884a648d736942caecade2ac3c2980e67
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873406"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Guia de instalação da oferta do Marketplace do Striim Azure SQL DW

Este início rápido pressupõe que você já tem uma instância já existente do SQL Data Warehouse.

Procure o Striim no Azure Marketplace e selecione-o para a opção de integração de dados com o SQL Data Warehouse (Preparado) 

![Instalar o Striim][install]

Configurar a VM do Striim com as propriedades especificadas, anotando o nome do cluster Striim, a senha e a senha do administrador

![Configurar o Striim][configure]

Uma vez implantado, clique em \<nome da VM >-masternode no portal do Azure, clique em conectar e copiar o logon usando a conta local da VM 

![Conectar o Striim ao SQL Data Warehouse][connect]

Baixe o sqljdbc42.jar de <https://www.microsoft.com/en-us/download/details.aspx?id=54671> para seu computador local. 

Abra uma janela de linha de comando e altere os diretórios em que você baixou o jar do JDBC. Realize SCP no arquivo jar para sua VM do Striim, obtendo o endereço e a senha do portal do Azure

![Copie o arquivo jar para sua VM][copy-jar]

Abra outra janela de linha de comando ou use um utilitário SSH para usar SSH no cluster do Striim

![Usar SSH no cluster][ssh]

Execute os seguintes comandos para mover o arquivo jar do JDBC para o diretório lib do Striim, e iniciar e parar o servidor.

   1. sudo su
   2. cd /tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl stop striim-node
   5. systemctl stop striim-dbms
   6. systemctl start striim-dbms
   7. systemctl start striim-node

![Iniciar o cluster Striim][start-striim]

Agora, abra seu navegador favorito e navegue até \<nome DNS >: 9080

![Navegue até a tela de logon][navigate]

Faça logon com o nome de usuário e a senha que você configurou no portal do Azure e selecione seu assistente preferido para começar, ou acesse a página de Aplicativos para começar usando a operação de arrastar e soltar da interface do usuário

![Fazer logon com as credenciais do servidor][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
