---
title: Recuperação de floresta do AD – verificar replicação
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.openlocfilehash: beb7968dee3b2948aed695864015a02f0aa3b47e
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938146"
---
# <a name="resources-to-verify-replication-is-working"></a>Recursos para verificar se a replicação está funcionando

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Depois de ter restaurado ou reinstalado todos os DCs, você pode verificar se AD DS e o SYSVOL são recuperados e replicados corretamente usando **repadmin/replsum**, que é executado em qualquer versão do Windows Server.

> [!TIP]
> Você também pode baixar e executar a [ferramenta de status de replicação do Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), uma ferramenta gratuita que monitora o status de replicação de DCS e relata erros. O ADReplStatus requer o .NET Framework 4, que será instalado se ainda não estiver presente.

Verifique o log de Replicação do DFS em Visualizador de Eventos para a ID de evento 4602 (ou a ID de evento 13516 do serviço de replicação de arquivo), que indica que o SYSVOL foi inicializado.

Se o primeiro log de controladores de domínio recuperado a ID de evento 4614 ("o controlador de domínios está aguardando para executar a replicação inicial. A pasta replicada permanecerá no estado de sincronização inicial até que seja replicada com seu parceiro ") no log de Replicação do DFS, a ID de evento 4602 não será exibida e você precisará executar as seguintes etapas manuais para recuperar o SYSVOL se ele for replicado pelo DFSR:

1. Quando o evento 4612 do DFSR aparece no primeiro DC restaurado, execute uma restauração manual autoritativa, conforme descrito em [2218556: como forçar uma sincronização autoritativa e não autoritativa para o SYSVOL replicado pelo DFSR (como "D4/D2" para o FRS)](https://support.microsoft.com/kb/2218556) ( https://support.microsoft.com/kb/2218556) .
2. Defina o **sinalizador SysvolReady** como 1 manualmente, conforme descrito em [947022 o compartilhamento Netlogon não está presente após a instalação do Active Directory Domain Services em um novo controlador de domínio baseado em Windows Server 2008 completo ou somente leitura](https://support.microsoft.com/kb/947022).

Você também pode criar um relatório de diagnóstico Replicação do DFS. Para obter mais informações, consulte [criar um relatório de diagnóstico para replicação do DFS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11)) e [guia passo a passo do DFS para o Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11)). Se o servidor estiver executando o Windows Server 2008 R2, você poderá usar a [ opção de linha de comandodfsrdiag.exe replicationstate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11)).

Você também pode executar o teste de replicações usando dcdiag.exe para verificar se há erros de replicação. Para obter mais informações, consulte o [artigo 249256](https://support.microsoft.com/kb/249256)da base de dados de conhecimento.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
