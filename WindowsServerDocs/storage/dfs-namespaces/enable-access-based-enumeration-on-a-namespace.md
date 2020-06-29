---
title: Habilitar a enumeração baseada em acesso em um Namespace
description: Este artigo descreve como habilitar a enumeração baseada em acesso em um namespace.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f011bc12c26567ed3a0e912dca3c3a8de9bfff9
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474923"
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>Habilitar a enumeração baseada em acesso em um Namespace

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

A enumeração baseada em acesso esconde arquivos e pastas que os usuários não têm permissão para acessar. Por padrão, esse recurso não está habilitado para namespaces DFS. Você pode habilitar a enumeração baseada em acesso de pastas DFS usando o Gerenciamento DFS. Para controlar a enumeração baseada em acesso de arquivos e pastas em destinos de pasta, você deve habilitar a enumeração baseada em acesso em cada pasta compartilhada usando o gerenciamento de compartilhamento e armazenamento.

Para habilitar a enumeração baseada em acesso em um namespace, todos os servidores de namespace devem estar executando o Windows Server 2008 ou mais recente. Além disso, namespaces baseados em domínio devem usar o modo Windows Server 2008. Para obter mais informações sobre o modo Windows Server 2008, consulte [Escolha um tipo de namespace](choose-a-namespace-type.md).

Em alguns ambientes, a habilitação de enumeração baseada em acesso pode causar alta utilização da CPU no servidor e tempo de resposta lento para os usuários.

> [!NOTE]
> Se você fizer upgrade no nível do domínio funcional para o Windows Server 2008 enquanto existirem namespaces baseados em domínio, o Gerenciamento DFS permitirá que você habilite a enumeração baseada em acesso nesses namespaces. No entanto, você não conseguirá editar permissões para ocultar pastas de quaisquer grupos ou usuários, a menos que você migre os namespaces para o modo Windows Server 2008. Para obter mais informações, consulte [Migrar um Namespace Baseado em Domínio para o Modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).


Para usar a enumeração baseada em acesso aos Namespaces DFS, você deve seguir estas etapas:

-   Habilitar a enumeração baseada em acesso em um Namespace
-   Controlar quais usuários e grupos podem exibir pastas de DFS individuais


> [!WARNING]
> A enumeração baseada em acesso não impede que os usuários obtenham uma referência para um destino de pasta se eles já conhecem o caminho DFS. Somente as permissões de compartilhamento ou permissões do sistema NTFS de arquivos do destino da pasta (pasta compartilhada) em si podem impedir que os usuários acessem um destino de pasta. Permissões de pasta DFS são usadas apenas para exibir ou ocultar as pastas DFS, não para controlar o acesso, tornando acesso de Leitura a única permissão relevante no nível da pasta DFS. Para obter mais informações, consulte [Usando permissões herdadas com enumeração baseada em acesso](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx)

<br />
Você pode habilitar a enumeração baseada em acesso em um namespace usando a interface do Windows ou usando uma linha de comando.

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>Para habilitar a enumeração baseada em acesso usando a interface do Windows

1.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em um namespace apropriado e, em seguida, clique em **Propriedades**.

2.  Clique na guia **Avançado** e selecione a caixa de seleção **Habilitar a enumeração baseada em acesso para esse namespace**.

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>Para habilitar a enumeração baseada em acesso usando uma linha de comando

1.  Abra uma janela de prompt de comando em um servidor que tenha o **sistema de arquivos distribuído** serviço de função ou o recurso de **Ferramentas do sistema de arquivos distribuído** instalado.

2.  Digite o seguinte comando, em que *<\_>raiz do namespace* é a raiz do namespace:

    ```
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> Para gerenciar a enumeração baseada em acesso em um namespace usando o Windows PowerShell, use o [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx), [Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx), e [Revoke-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx) cmdlets. O módulo do Windows PowerShell de DFSN foi apresentado no Windows Server 2012.

Você pode controlar quais usuários e grupos podem exibir pastas DFS individuais usando a interface do Windows ou usando uma linha de comando.

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>Para controlar a visibilidade de pasta por meio da interface do Windows

1.  Na árvore de console, no nó **Namespaces**, localize a pasta com destinos para a qual você deseja controlar a visibilidade, clique com botão direito e, em seguida, clique em **Propriedades**.

2.  Clique na guia **Avançado**.

3.  Clique em **Definir permissões de exibição explícitas nessa pasta do DFS** e **Configurar permissões de exibição**.

4.  Adicionar ou remover usuários ou grupos clicando em **Adicionar** ou **Remover**.

5.  Para permitir que os usuários vejam a pasta DFS, selecione o grupo ou usuário e, em seguida, selecione a caixa de seleção **Permitir**.

    Para esconder a pasta de um grupo ou usuário, selecione o grupo ou usuário e, em seguida, selecione a caixa de seleção **Negar**.

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>Para controlar visibilidade da pasta usando uma linha de comando

1. Abra uma janela de prompt de comando em um servidor que tem o **Distributed File System** serviço de função ou **ferramentas do sistema de arquivos distribuído** recurso instalado.

2. Digite o seguinte comando, em que * &lt; &gt; DFSPath* é o caminho da pasta DFS (link), *<conta de domínio \\>* é o nome do grupo ou da conta de usuário e *(...)* é substituído por ACEs (entradas de controle de acesso) adicionais:

   ```
   dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
   ```

   Por exemplo, para substituir as permissões existentes por permissões que permitem que os administradores de domínio e os estagiários da CONTOSO tenham \\ acesso de leitura (R) à \\ pasta contoso. office\public\training, digite o seguinte comando:

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace
   ```

3. Para executar tarefas adicionais no prompt de comando, use os seguintes comandos:


| Comando | Descrição |
|---|---|
|[Propriedade Dfsutil sd negar](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|Nega a um grupo ou a usuário a capacidade de exibir a pasta.|
|[Propriedade Dfsutil sd redefinir](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |Remove todas as permissões da pasta.|
|[Propriedade Dfsutil sd revogar](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| Remove um grupo ou usuário ACE de uma pasta. |

## <a name="additional-references"></a>Referências adicionais

-   [Criar um namespace do DFS](create-a-dfs-namespace.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Instalando o DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [Usando permissões herdadas com enumeração baseada em acesso](using-inherited-permissions-with-access-based-enumeration.md)