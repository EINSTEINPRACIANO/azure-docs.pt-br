---
title: Corrigir recursos sem conformidade
description: Estas instruções o orientam pela correção de recursos que não estão em conformidade com as políticas no Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: d6753b319bc5bc4cbda18fe486695e5b0266acae
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66169649"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Corrigir recursos que não estão em conformidade com o Azure Policy

Recursos que não estão em conformidade com uma política de **deployIfNotExists** podem ser colocados em um estado de conformidade por meio de **Correção**. Correção é realizada pela instruindo a política do Azure para executar o **deployIfNotExists** efeito da diretiva atribuída em seus recursos existentes. Este artigo mostra as etapas necessárias para entender e realizar a correção com o Azure Policy.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="how-remediation-security-works"></a>Como funciona a correção de segurança

Quando a política do Azure é executado o modelo na **deployIfNotExists** definição de política, ele faz isso usando uma [identidade gerenciada](../../../active-directory/managed-identities-azure-resources/overview.md).
A política do Azure cria uma identidade gerenciada para cada atribuição, mas deve ter detalhes sobre quais são as funções para conceder a identidade gerenciada. Se a identidade gerenciada não tiver funções, esse erro será exibido durante a atribuição da política ou uma iniciativa. Ao usar o portal, política do Azure automaticamente concederá a identidade gerenciada funções listadas depois que a atribuição é iniciada.

![Identidade gerenciada – função ausente](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Se um recurso modificado por **deployIfNotExists** está fora do escopo da atribuição de política ou o modelo acessar propriedades em recursos que estão fora do escopo da atribuição de política, deve ser [concedido acesso manualmente](#manually-configure-the-managed-identity) à identidade gerenciada da atribuição, ou a implantação de correção falha.

## <a name="configure-policy-definition"></a>Configurar a definição de política

A primeira etapa é definir as funções de que **deployIfNotExists** precisa na definição de política para implantar com êxito o conteúdo do seu modelo incluído. Na propriedade **detalhes**, adicione uma propriedade **roleDefinitionIds**. Essa propriedade é uma matriz de cadeias de caracteres que correspondem a funções no ambiente. Para obter um exemplo completo, confira o [exemplo deployIfNotExists](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**roleDefinitionIds** usa o identificador de recurso completo e não assume o **roleName** curto da função. Para obter a ID para a função 'Colaborador' em seu ambiente, use o seguinte código:

```azurecli-interactive
az role definition list --name 'Contributor'
```

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Configurar manualmente a identidade gerenciada

Ao criar uma atribuição usando o portal, política do Azure gera a identidade gerenciada e concede a ele as funções definidas no **roleDefinitionIds**. Nas seguintes condições, as etapas usadas para criar a identidade gerenciada e atribuir permissões a ela precisam ser feitas manualmente:

- Ao usar o SDK (como o Azure PowerShell)
- Quando um recurso fora do escopo de atribuição é modificado pelo modelo
- Quando um recurso fora do escopo de atribuição é lido pelo modelo

> [!NOTE]
> O Azure PowerShell e o .NET são os únicos SDKs que atualmente dão suporte a essa funcionalidade.

### <a name="create-managed-identity-with-powershell"></a>Criar identidade gerenciada com o PowerShell

Para criar uma identidade gerenciada durante a atribuição da política, o **Local** deve ser definido e **AssignIdentity** deve ser usada. O exemplo a seguir obtém a definição da política interna **Implantar Transparent Data Encryption do Banco de Dados SQL**, define o grupo de recursos de destino e, em seguida, cria a atribuição.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

A variável `$assignment` agora contém a ID da entidade de segurança referente à identidade gerenciada, juntamente com os valores padrão retornados durante a criação de uma atribuição de política. Ela pode ser acessada por meio de `$assignment.Identity.PrincipalId`.

### <a name="grant-defined-roles-with-powershell"></a>Conceder funções definidas com o PowerShell

A nova identidade gerenciada deve concluir a replicação por meio do Azure Active Directory antes que possam ser concedidas a ela as funções necessárias. Após a conclusão da replicação, o exemplo a seguir itera a definição de política em `$policyDef` para **roleDefinitionIds** e usa [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) para conceder as funções à nova identidade gerenciada.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Conceder funções definidas por meio do portal

Há duas maneiras de conceder uma identidade gerenciada da atribuição às funções definidas usando o portal: usando **Controle de acesso (IAM)** ou editando a atribuição de política ou iniciativa e clicando em **Salvar**.

Para adicionar uma função à identidade gerenciada da atribuição, siga estas etapas:

1. Inicie o serviço de Azure Policy no portal do Azure clicando em**Todos os serviços**, em seguida pesquisando e selecionando **Política**.

1. Selecione **Atribuições** no lado esquerdo da página de Política do Azure.

1. Localize a atribuição que tem uma identidade gerenciada e clique no nome.

1. Localize a propriedade **ID de Atribuição** na página de edição. A ID de atribuição será algo como:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   O nome da identidade gerenciada é a última parte da ID do recurso de atribuição, que é `2802056bfc094dfb95d4d7a5` neste exemplo. Copie essa parte da ID do recurso da atribuição.

1. Navegue até o recurso ou os recursos do contêiner pai (grupo de recursos, assinatura, grupo de gerenciamento) que precisa na definição de função adicionada manualmente.

1. Clique no link **Controle de acesso (IAM)** na página de recursos e clique em **+ Adicionar atribuição de função** na parte superior da página do controle de acesso.

1. Selecione a função apropriada que corresponde a **roleDefinitionIds** da definição de política.
   Deixe **Atribuir acesso** definido como o padrão de 'Usuário, grupo ou aplicativo do Azure AD'. Na caixa **Selecionar**, cole ou digite a parte da ID do recurso de atribuição localizada anteriormente. Depois que a pesquisa for concluída, clique no objeto com o mesmo nome para selecionar a ID e clique em **Salvar**.

## <a name="create-a-remediation-task"></a>Criar uma tarefa de correção

### <a name="create-a-remediation-task-through-portal"></a>Criar uma tarefa de atualização por meio do portal

Durante a avaliação, a atribuição de política com o efeito **deployIfNotExists** determinará se há recursos sem conformidade. Quando forem encontrados recursos sem conformidade, os detalhes serão fornecidos na página **Correção**. Junto com a lista de políticas que têm recursos sem conformidade está a opção para disparar uma **tarefa de correção**. Essa opção é a que cria uma implantação usando o modelo **deployIfNotExists**.

Para criar uma **tarefas de correção**, siga estas etapas:

1. Inicie o serviço de Azure Policy no portal do Azure clicando em**Todos os serviços**, em seguida pesquisando e selecionando **Política**.

   ![Pesquisar Política em Todos os Serviços](../media/remediate-resources/search-policy.png)

1. Selecione **Correção** no lado esquerdo da página do Azure Policy.

   ![Selecione correção na página de política](../media/remediate-resources/select-remediation.png)

1. Todas as atribuições de política **deployIfNotExists** com recursos sem conformidade estão incluídas na tabela de dados e na guia **Políticas a corrigir**. Clique em uma política com recursos sem conformidade. A página **Nova tarefa de correção** é aberta.

   > [!NOTE]
   > Uma maneira alternativa de abrir a página **tarefa de correção** é localizar e clicar na política na página **Conformidade** e, em seguida, clicar no botão **Criar tarefa de correção**.

1. Na página **Nova tarefa de correção**, filtre os recursos a serem corrigidos usando as reticências de **Escopo** para escolher os recursos filho nos quais a política está atribuída (incluindo até os objetos do recurso individuais). Além disso, use a lista suspensa **Locais** para filtrar ainda mais os recursos. Somente os recursos listados na tabela serão corrigidos.

   ![Corrigir - selecionar quais recursos para corrigir](../media/remediate-resources/select-resources.png)

1. Inicie a tarefa de correção depois que os recursos forem filtrados clicando em **Corrigir**. A página de conformidade de política será aberta na guia **Tarefas de correção** para mostrar o estado do progresso das tarefas.

   ![Corrigir - progresso das tarefas de correção](../media/remediate-resources/task-progress.png)

1. Clique na **tarefa de correção** na página de conformidade de política para obter detalhes sobre o progresso. A filtragem usada para a tarefa é mostrada junto com uma lista dos recursos que estão sendo corrigidos.

1. Na página **Tarefa de correção**, clique com o botão direito do mouse em um recurso para exibir o recurso ou a implantação da tarefa de correção. No final da linha, clique em **Eventos relacionados** para ver detalhes como uma mensagem de erro.

   ![Remedir – menu de contexto da tarefa de recursos](../media/remediate-resources/resource-task-context-menu.png)

Os recursos implantados por meio de uma **tarefa de correção** são adicionados à guia **Recursos Implantados** na página de conformidade com a política.

### <a name="create-a-remediation-task-through-azure-cli"></a>Criar uma tarefa de atualização por meio da CLI do Azure

Para criar uma **tarefas de correção** com a CLI do Azure, use o `az policy remediation` comandos. Substitua `{subscriptionId}` com sua ID de assinatura e `{myAssignmentId}` com seu **deployIfNotExists** ID de atribuição de política.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Para outros comandos de correção e exemplos, consulte o [correção de política az](/cli/azure/policy/remediation) comandos.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Criar uma tarefa de atualização por meio do PowerShell do Azure

Para criar uma **tarefas de correção** com o Azure PowerShell, use o `Start-AzPolicyRemediation` comandos. Substitua `{subscriptionId}` com sua ID de assinatura e `{myAssignmentId}` com seu **deployIfNotExists** ID de atribuição de política.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Para outros cmdlets de correção e exemplos, consulte o [Az.PolicyInsights](/powershell/module/az.policyinsights/#policy_insights) módulo.

## <a name="next-steps"></a>Próximas etapas

- Examine os exemplos na [exemplos do Azure Policy](../samples/index.md).
- Revise a [estrutura de definição do Azure Policy](../concepts/definition-structure.md).
- Revisar [Compreendendo os efeitos da política](../concepts/effects.md).
- Entender como [criar políticas de forma programática](programmatically-create.md).
- Saiba como [obter dados de conformidade](getting-compliance-data.md).
- Examine o que um grupo de gerenciamento com [organizar seus recursos com grupos de gerenciamento do Azure](../../management-groups/overview.md).