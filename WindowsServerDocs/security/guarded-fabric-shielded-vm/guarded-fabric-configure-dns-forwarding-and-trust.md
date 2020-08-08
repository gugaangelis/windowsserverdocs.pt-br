---
title: Configurar o encaminhamento DNS e a confiança do domínio
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 102d1267fe2b15e50ab2d078647ff86f7c937a6d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966163"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurar o encaminhamento de DNS no domínio HGS e uma relação de confiança unidirecional com o domínio de malha

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

>[!IMPORTANT]
>O modo AD é preterido a partir do Windows Server 2019. Para ambientes em que o atestado do TPM não é possível, configure o [atestado de chave do host](guarded-fabric-initialize-hgs-key-mode.md). O atestado de chave de host fornece garantia semelhante ao modo AD e é mais simples de configurar.

Use as etapas a seguir para configurar o encaminhamento de DNS e estabelecer uma relação de confiança unidirecional com o domínio de malha. Essas etapas permitem que o HGS Localize os controladores de domínio de malha e valide a associação de grupo dos hosts Hyper-V.

1.  Execute o comando a seguir em uma sessão do PowerShell com privilégios elevados para configurar o encaminhamento de DNS. Substitua fabrikam.com pelo nome do domínio de malha e digite os endereços IP dos servidores DNS no domínio de malha. Para maior disponibilidade, aponte para mais de um servidor DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Para criar uma relação de confiança de floresta unidirecional, execute o seguinte comando em um prompt de comando com privilégios elevados:

    Substitua `bastion.local` pelo nome do domínio HgS e `fabrikam.com` pelo nome do domínio de malha. Forneça a senha para um administrador do domínio de malha.

    ```powershell
    netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add
    ```

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Configurar HTTPS](guarded-fabric-configure-hgs-https.md)
