---
title: "AD floresta recuperação - verificar replicação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adfs
ms.openlocfilehash: d85336a10e808755677a99777af8ecf5c07b05ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para verificar replicação está funcionando 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Depois de ter restaurado ou reinstalado em todos os controladores de domínio, você pode verificar que AD DS e SYSVOL são recuperados e replicação corretamente usando **repadmin /replsum**, que é executado em qualquer versão do Windows Server.  
  
> [!TIP]
>  Você também pode baixar e executar o [ferramenta de Status de replicação do Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), uma ferramenta gratuita que monitora o status de replicação de controladores de domínio e relatórios de erros. ADReplStatus requer o .NET Framework 4, que será instalado se ele já não estiver presente.  
  
 Verifique o log de replicação DFS no Visualizador de eventos para 4602 de ID de evento (ou ID de evento do serviço de replicação de arquivo 13516), que indica que SYSVOL foi inicializado.  
  
 Se o primeiro recuperado DC registra em log 4614 de ID de evento ("o controlador de domínio está esperando para realizar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que ele tenha replicada com seu parceiro") no log de replicação DFS, em seguida, ID de evento 4602 não aparecer e você precisa executar as seguintes etapas manuais para recuperar SYSVOL quando ele é replicado por DFSR:  
  
1.  Quando a DFSR evento 4612 aparece no controlador de domínio restaurado primeiro executar uma restauração autoritativa manual conforme descrito em [2218556: como forçar uma sincronização autorizada e não autorizados para SYSVOL replicados DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
  
2.  Defina **SysvolReady sinalizador** como 1 manualmente, conforme descrito em [947022 compartilhar o logon de rede não está presente após a instalação do Active Directory Domain Services em um novo controlador de domínio baseados em Windows Server 2008 total ou somente leitura](https://support.microsoft.com/kb/947022).  
  
 Você também pode criar um relatório de diagnóstico replicação DFS. Para obter mais informações, consulte [criar um relatório de diagnóstico para replicação DFS](https://technet.microsoft.com/library/cc754227.aspx) e [DFS Step-by-Step guia para o Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Se o servidor está executando o Windows Server 2008 R2, você pode usar [opção de linha de comando ReplicationState dfsrdiag.exe](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  
  
 Você também pode executar o teste de replicação usando dcdiag.exe para verificar se há erros de replicação. Para obter mais informações, consulte Knowledge Base [249256 do artigo](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
