---
title: Floresta do AD recuperação - verificar a replicação
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: fb05586f281460dc2c7a1afea4c0423493e3fc46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878307"
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para verificar a replicação está funcionando 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Depois de ter restaurado ou reinstalado em todos os controladores de domínio, você pode verificar que o AD DS e SYSVOL são recuperados e replicando corretamente usando **repadmin /replsum**, que é executado em qualquer versão do Windows Server.  
  
> [!TIP]
> Você também pode baixar e executar o [ferramenta de Status de replicação do Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), uma ferramenta gratuita que monitora o status de replicação de controladores de domínio e os relatórios de erros. ADReplStatus requer o .NET Framework 4, que será instalado se ele não ainda estiver presente.  

Verifique o log de replicação do DFS no Visualizador de eventos para 4602 de ID de evento (ou a ID de evento de serviço de replicação de arquivo 13516), que indica o que SYSVOL tiver sido inicializado.  

Se recuperado o primeiro controlador de domínio registra 4614 de ID de evento ("o controlador de domínio está esperando para realizar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que tenha replicado com seu parceiro") no log de replicação do DFS, 4602 de ID de evento não será exibida e você precisará executar as seguintes etapas manuais para recuperar o SYSVOL quando ele é replicado pelo DFSR:  

1. Executar uma restauração autoritativa manual quando DFSR evento 4612 é exibido no primeiro controlador de domínio restaurado, conforme descrito em [2218556: Como forçar uma sincronização autoritativa e não autoritativa para SYSVOL replicado por DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Definir **sinalizador SysvolReady** como 1 manualmente, conforme descrito em [947022 compartilhamento do NETLOGON não está presente, depois de instalar o Active Directory Domain Services em um novo controlador de domínio baseados no Windows Server 2008 completo ou somente leitura ](https://support.microsoft.com/kb/947022).  

Você também pode criar um relatório de diagnóstico de replicação do DFS. Para obter mais informações, consulte [criar um relatório de diagnóstico para a replicação do DFS](https://technet.microsoft.com/library/cc754227.aspx) e [DFS passo a passo guia para o Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Se o servidor estiver executando o Windows Server 2008 R2, você pode usar [opção de linha de comando ReplicationState dfsrdiag.exe](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

Você também pode executar o teste de replicações usando dcdiag.exe para verificar se há erros de replicação. Para obter mais informações, consulte a Base de Conhecimento [249256 do artigo](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
