---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: 4cdcec850f32d7e94f33eb28e5bf7839e511f347
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124575"
---
Preencher e enviar o [formulário de solicitação de contêineres de visão de serviços Cognitivos](https://aka.ms/VisionContainersPreview) para solicitar acesso ao contêiner. O formulário solicita informações sobre você, sua empresa e o cenário do usuário para o qual você usará o contêiner. Depois de enviar o formulário, a equipe de serviços Cognitivos do Azure analisa-o para verificar se você atende os critérios para acesso ao registro de contêiner privado.

> [!IMPORTANT]
> Você deve usar um endereço de email associado a uma conta da Microsoft (MSA) ou uma conta do Azure Active Directory (Azure AD) no formulário.

Se sua solicitação for aprovada, você receberá um email com instruções que descrevem como obter suas credenciais e acessar o registro de contêiner privado.

## <a name="log-in-to-the-private-container-registry"></a>Efetue login no registro do contêiner particular

Há várias maneiras de autenticar com o registro de contêiner privado para contêineres de serviços Cognitivos. É recomendável que você use o método de linha de comando usando o [CLI do Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Use o [logon do docker](https://docs.docker.com/engine/reference/commandline/login/) de comando, conforme mostrado no exemplo a seguir, para fazer logon no `containerpreview.azurecr.io`, que é o registro de contêiner privado para contêineres de serviços Cognitivos. Substitua *\<nome de usuário\>* pelo nome de usuário e *\<senha\>* com a senha fornecida nas credenciais recebidas da equipe do Azure Cognitive Services.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Se você protegeu suas credenciais em um arquivo de texto, você pode concatenar o conteúdo desse arquivo de texto para o `docker login` comando. Use o `cat` de comando, conforme mostrado no exemplo a seguir. Substitua *\<passwordFile\>* com o caminho e nome do arquivo de texto que contém a senha. Substitua *\<nome de usuário\>* com o nome de usuário fornecido no suas credenciais.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

