---
title: Como usar permissões herdadas com enumeração baseada em acesso
description: Este artigo descreve como usar as permissões herdadas com enumeração baseada em acesso
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 433fe53a3d580aafc50b152ec20156436b05481f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402137"
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>Usando permissões herdadas com Enumeração baseada em acesso

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Por padrão, as permissões utilizadas para uma pasta DFS são herdadas do sistema de arquivos local do servidor do namespace. As permissões são herdadas do diretório raiz da unidade do sistema e concedem ao domínio\\permissões de leitura do grupo usuários. Como resultado, mesmo após a habilitação da enumeração baseada em acesso, todas as pastas no namespace permanecem visíveis para todos os usuários de domínio.

## <a name="advantages-and-limitations-of-inherited-permissions"></a>Vantagens e limitações das permissões herdadas

Há duas vantagens principais ao usar permissões herdadas para controlar quais usuários podem exibir pastas em um namespace DFS:

-   Você pode rapidamente aplicar permissões herdadas para muitas pastas sem usar scripts.
-   Você pode aplicar permissões herdadas para raízes de namespace e pastas sem destinos.

Apesar dos benefícios, permissões herdadas nos Namespaces DFS têm muitas limitações que tornam inadequadas para a maioria dos ambientes:

-   Modificações de permissões herdadas não são duplicadas para outros servidores de namespace. Portanto, use as permissões herdadas somente em namespaces autônomos ou em ambientes onde você pode implementar um sistema de replicação de terceiros para manter o controle de listas de acesso (ACLs) em todos os servidores de namespace sincronizados.
-   Gerenciamento DFS e **Dfsutil** não é possível visualizar ou modificar permissões herdadas. Portanto, você deve usar o Windows Explorer ou o **Icacls** comando além de Gerenciamento DFS ou **Dfsutil** para gerenciar o namespace.
-   Ao utilizar permissões herdadas, você não pode modificar as permissões de uma pasta com destinos, exceto usando o **Dfsutil** comando. Namespaces DFS removem automaticamente permissões das pastas com destinos definidas utilizando outras ferramentas ou métodos.
-   Se você definir as permissões em uma pasta com destinos enquanto você estiver usando permissões herdadas, a ACL que você defina na pasta com destinos combina com as permissões herdadas do pai da pasta no sistema de arquivos. Você deve examinar os dois conjuntos de permissões para determinar quais são as permissões de rede.

> [!NOTE]
> Ao utilizar permissões herdadas, é mais simples definir as permissões em raízes de namespace e pastas sem destinos. Em seguida, use as permissões herdadas nas pastas com destinos para que eles herdam as permissões de todos os seus pais.

## <a name="using-inherited-permissions"></a>Usando permissões herdadas

Para limitar quais usuários podem exibir uma pasta DFS, você deve executar uma das seguintes tarefas:

-   **Defina permissões explícitas para a pasta, desabilitando a herança.** Para definir permissões explícitas em uma pasta com destinos (um link) usando o Gerenciamento DFS ou o **Dfsutil** de comando, consulte [Enable Access-Based enumeração em um Namespace](enable-access-based-enumeration-on-a-namespace.md).
-   **Modificar permissões herdadas no pai no sistema de arquivos local**. Para modificar as permissões herdadas por uma pasta com destinos, se você já tiver definido permissões explícitas na pasta, alterne para as permissões herdadas de permissões explícitas, conforme discutido no procedimento a seguir. Use o Windows Explorer ou o **Icacls** comando para modificar as permissões da pasta da qual a pasta com destinos herda suas permissões.

> [!NOTE]
> A enumeração baseada em acesso não impede que os usuários obtenham uma referência para um destino de pasta se eles já conhecem o caminho DFS de pasta com alvos. Permissões configuradas usando o Windows Explorer ou o **Icacls** comando no namespace raízes ou pastas sem destinos controlam se os usuários podem acessar a raiz de pasta ou do namespace DFS. No entanto, eles não impedir que os usuários acessem diretamente uma pasta com destinos. Somente as permissões de compartilhamento ou permissões do sistema NTFS de arquivos (pasta compartilhada) em si podem impedir que os usuários acessem destinos de pasta.

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>Para alternar das permissões explícitas para permissões herdadas

1.  Na árvore de console, no nó **Namespaces**, localize a pasta com destinos para a qual você deseja controlar a visibilidade, clique com botão direito e, em seguida, clique em **Propriedades**.

2.  Clique na guia **Avançado**.

3.  Clique em **Use as permissões herdadas do sistema de arquivos local** e, em seguida, clique em **OK** no **Confirmar uso de permissões herdadas** caixa de diálogo. Isso remove todos os explicitamente definir permissões nessa pasta, restaurando as permissões NTFS herdadas do sistema de arquivos local do servidor do namespace.

4.  Para alterar as permissões herdadas para raízes de namespace em um namespace DFS ou pastas, use o Windows Explorer ou o **ICacls** comando.

## <a name="see-also"></a>Consulte também

-   [Criar um namespace do DFS](create-a-dfs-namespace.md)