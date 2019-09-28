---
title: Configurar o encaminhamento DNS e a confiança do domínio
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5d8ffe82065caeee27c5d13f5243f13addc6c325
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386742"
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

    Substitua `bastion.local` pelo nome do domínio HGS e `fabrikam.com` pelo nome do domínio da malha. Forneça a senha para um administrador do domínio de malha.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Próximas etapas 

> [!div class="nextstepaction"]
> [Configurar HTTPS](guarded-fabric-configure-hgs-https.md)
