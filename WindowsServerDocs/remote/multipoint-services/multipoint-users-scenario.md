---
title: Contas de usuário do multiPoint Services
description: Saiba mais sobre contas de usuário do MultiPoint Services, especialmente o tipo a ser usado para cenários diferentes
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 31279f81d5af597b0b1f1729c953fefaf24a214f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855357"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Cenários de exemplo: Contas de usuário do multiPoint Services
O que você precisa fazer para implementar o cenário de conta de usuário que você escolheu para o seu ambiente do MultiPoint Services? As tabelas a seguir descrevem cada tarefa a ser executada para configurar contas de usuário e preparar estações para contas de usuário individuais ou compartilhado em um computador do MultiPoint autônomo ou em servidores em rede em um grupo de trabalho ou um domínio do Active Directory. Escolha o cenário que se aplica ao seu ambiente. Em seguida, siga os links na tabela para concluir cada tarefa de configuração necessárias.  
  
> [!NOTE]  
> Se você ainda não tiver decidido como configurar suas contas de usuário, consulte [plano de contas de usuário para o seu ambiente do MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md) para obter mais informações sobre como cada opção afeta os usuários.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Computador de serviços do MultiPoint único em um ambiente autônomo (não há rede)  
  
|||  
|-|-|  
|**Não é necessário que meus usuários fazer logon.** As estações podem estar disponíveis para qualquer pessoa que percorre a eles. Eles precisam de uma experiência individual da área de trabalho do Windows que inclui pastas privadas para armazenar dados ou áreas de trabalho personalizadas.|1.  Criar uma conta de usuário local único (para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md)<br />3.  [Configurar estações para logon automático](Configure-stations-for-automatic-logon.md)|  
|**Meus usuários podem compartilhar o mesmo logon do usuário.** Eles precisam de uma experiência individual da área de trabalho do Windows que inclui pastas privadas para armazenar dados ou áreas de trabalho personalizadas.|1.  Criar uma conta de usuário local único (para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md)|  
|**Meus usuários devem ter sua própria experiência de área de trabalho individual do Windows.**|Criar uma conta de usuário local para cada usuário (para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Vários computadores do MultiPoint Services em uma rede, mas sem domínio  
  
|||  
|-|-|  
|**Não é necessário que meus usuários fazer logon.** As estações podem estar disponíveis para qualquer pessoa que percorre a eles. Eles precisam de uma experiência individual da área de trabalho do Windows que inclui pastas privadas para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma conta de usuário local único em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor<br />3.  [Configurar estações para logon automático](Configure-stations-for-automatic-logon.md) em cada servidor|  
|**Meus usuários podem compartilhar o mesmo logon do usuário.** Eles precisam de uma experiência individual da área de trabalho do Windows que inclui pastas privadas para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma conta de usuário local único em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor.|  
|**Meus usuários devem ter sua própria experiência de área de trabalho individual do Windows.**<br /><br />-   **Opção A** -meus usuários sempre usará o locais estações conectadas ao mesmo computador do MultiPoint Services.<br />-   **Opção B** -meus usuários usarão estações locais em mais de um computador do MultiPoint Services.<br />-   **Opção C** -meus usuários usarão a clientes remotos na rede local.|-   **Opção A** -criar uma conta de usuário local único em cada servidor para os usuários do servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />-   **Opção B** -criar contas de usuário local para todos os usuários em todos os servidores. **Observação:** Isso significa que cada usuário tenha um perfil em cada servidor. Em outras palavras, se eles salvarem um arquivo em Meus documentos enquanto está conectado ao estação do servidor, eles não verão o arquivo ao fazer logon na estação do servidor B. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br />-   **Opção C** -atribuir a cada usuário a um computador específico do MultiPoint Services. Crie contas de usuário local para os usuários atribuídos em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Um ou mais computadores de MultiPoint Services em um ambiente de rede de domínio  
  
|||  
|-|-|  
|**Não é necessário que meus usuários fazer logon.** As estações podem estar disponíveis para qualquer pessoa que percorre a eles. Eles precisam de uma experiência individual da área de trabalho do Windows que inclui pastas privadas para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma conta de domínio para fazer logon nos servidores.<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor.<br />3.  [Configurar estações para logon automático](Configure-stations-for-automatic-logon.md) em cada servidor.|  
|**Meus usuários podem compartilhar o mesmo logon do usuário.** Eles precisam de uma experiência individual da área de trabalho do Windows que inclui pastas privadas para armazenar dados ou áreas de trabalho personalizadas.|1.  Crie uma conta de domínio para um grupo ou para cada usuário.<br />2.  [Permitir que uma conta tenha várias sessões](Allow-one-account-to-have-multiple-sessions.md) em cada servidor.|  
|**Meus usuários devem ter sua própria experiência de área de trabalho individual do Windows.**<br /><br />-   **Opção A** -qualquer usuário com uma conta de domínio pode usar o computador do MultiPoint Services.<br />-   **Opção B** -desejo limitar quais contas de domínio podem acessar o servidor.|-   **Opção A** -nenhuma configuração é necessária. Por padrão, todos os usuários de domínio têm acesso a qualquer computador do MultiPoint Services na rede.<br />-   **Opção B** -limitar o acesso de contas de usuário de domínio para o computador do MultiPoint Services. Para obter instruções, consulte [limitar o acesso de usuários para o servidor](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Eu quero usar contas de usuário locais e gerenciá-los separadamente das minhas contas de domínio.** Por exemplo, você deseja que alguém para gerenciar o MultiPoint Services, mas não o domínio ou você não queira conceder a contas de domínio para todos os usuários do MultiPoint Services.|Crie uma ou mais contas de usuário local em cada servidor. (Para obter instruções, consulte [criar contas de usuário local](Create-local-user-accounts.md).)<br /><br />**Observação:** Isso significa que cada conta de usuário terá um perfil em cada servidor. Em outras palavras, se eles salvarem um arquivo em Meus documentos enquanto está conectado ao estação do servidor, eles não verão o arquivo ao fazer logon na estação do servidor B.|  