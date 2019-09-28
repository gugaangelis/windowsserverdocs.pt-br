---
title: Contas de usuário dos serviços do MultiPoint
description: Saiba mais sobre as contas de usuário nos serviços do MultiPoint, especialmente o tipo a ser usado em diferentes cenários
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: ff81af2a0aa8da7f801ec14c27dc00c9bae6e8aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388962"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Cenários de exemplo: Contas de usuário dos serviços do MultiPoint
O que você precisa fazer para implementar o cenário de conta de usuário que você escolheu para o ambiente de serviços do MultiPoint? As tabelas a seguir descrevem cada tarefa a ser executada para configurar contas de usuário e preparar estações para contas de usuário compartilhadas ou individuais em um computador MultiPoint autônomo ou em servidores em rede em um grupo de trabalho ou em um domínio Active Directory. Escolha o cenário que se aplica ao seu ambiente. Em seguida, siga os links na tabela para concluir cada tarefa de configuração necessária.  
  
> [!NOTE]  
> Se você ainda não decidiu como configurar suas contas de usuário, consulte [planejar contas de usuário para o ambiente de serviços do MultiPoint](Plan-user-accounts-for-your-MultiPoint-services-environment.md) para obter mais informações sobre como cada opção afeta os usuários.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Um único computador de serviços do MultiPoint em um ambiente autônomo (sem rede)  
  
|||  
|-|-|  
|**Meus usuários não precisam fazer logon.** As estações podem estar disponíveis para qualquer pessoa que possa orientá-las. Eles não precisam de uma experiência individual na área de trabalho do Windows que inclua pastas particulares para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma única conta de usuário local (para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md)<br />3.  [Configurar estações para logon automático](Configure-stations-for-automatic-logon.md)|  
|**Meus usuários podem compartilhar o mesmo logon de usuário.** Eles não precisam de uma experiência individual na área de trabalho do Windows que inclua pastas particulares para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma única conta de usuário local (para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md)|  
|**Meus usuários devem ter sua própria experiência de área de trabalho individual do Windows.**|Crie uma conta de usuário local para cada usuário (para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Vários computadores de serviços do MultiPoint em uma rede, mas sem domínio  
  
|||  
|-|-|  
|**Meus usuários não precisam fazer logon.** As estações podem estar disponíveis para qualquer pessoa que possa orientá-las. Eles não precisam de uma experiência individual na área de trabalho do Windows que inclua pastas particulares para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma única conta de usuário local em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor<br />3.  [Configurar estações para logon automático](Configure-stations-for-automatic-logon.md) em cada servidor|  
|**Meus usuários podem compartilhar o mesmo logon de usuário.** Eles não precisam de uma experiência individual na área de trabalho do Windows que inclua pastas particulares para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma única conta de usuário local em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor.|  
|**Meus usuários devem ter sua própria experiência de área de trabalho individual do Windows.**<br /><br />-   **Opção a** -meus usuários sempre usarão estações locais conectadas ao mesmo computador de serviços do MultiPoint.<br />-   **Opção B** -meus usuários usarão estações locais em mais de um computador dos serviços do MultiPoint.<br />-   **Opção C** -meus usuários usarão clientes remotos na LAN.|-   **Opção a** -criar uma única conta de usuário local em cada servidor para os usuários desse servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />-   **Opção B** -criar contas de usuário local para cada usuário em cada servidor. **Observação:** Isso significa que cada usuário terá um perfil em cada servidor. Em outras palavras, se salvar um arquivo em meus documentos enquanto estiver conectado na estação do servidor A, ele não verá o arquivo ao fazer logon na estação do servidor B. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />-   **Opção C** -atribuir cada usuário a um computador de serviços do MultiPoint específico. Crie contas de usuário local para os usuários atribuídos em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Um ou mais computadores de serviços do MultiPoint em um ambiente de rede de domínio  
  
|||  
|-|-|  
|**Meus usuários não precisam fazer logon.** As estações podem estar disponíveis para qualquer pessoa que possa orientá-las. Eles não precisam de uma experiência individual na área de trabalho do Windows que inclua pastas particulares para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma conta de domínio para fazer logon nos servidores.<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor.<br />3.  [Configure estações para logon automático](Configure-stations-for-automatic-logon.md) em cada servidor.|  
|**Meus usuários podem compartilhar o mesmo logon de usuário.** Eles não precisam de uma experiência individual na área de trabalho do Windows que inclua pastas particulares para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma conta de domínio para um grupo ou para cada usuário.<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor.|  
|**Meus usuários devem ter sua própria experiência de área de trabalho individual do Windows.**<br /><br />-   **Opção a** -qualquer usuário com uma conta de domínio pode usar o computador dos serviços do MultiPoint.<br />-   **Opção B** -quero limitar quais contas de domínio podem acessar o servidor.|-   **Opção a** -nenhuma configuração é necessária. Por padrão, todos os usuários de domínio têm acesso a qualquer computador dos serviços do MultiPoint na rede.<br />-   **Opção B** -limitar o acesso de contas de usuário de domínio ao computador dos serviços do MultiPoint. Para obter instruções, confira [limitar o acesso de usuários ao servidor](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Quero usar contas de usuário local e gerenciá-las separadamente de minhas contas de domínio.** Por exemplo, você deseja que alguém gerencie serviços do MultiPoint, mas não o domínio, ou não queira conceder contas de domínio a todos os usuários de serviços do MultiPoint.|Crie uma ou mais contas de usuário local em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br /><br />**Observação:** Isso significa que cada conta de usuário terá um perfil em cada servidor. Em outras palavras, se salvar um arquivo em meus documentos enquanto estiver conectado na estação do servidor A, ele não verá o arquivo ao fazer logon na estação do servidor B.|  