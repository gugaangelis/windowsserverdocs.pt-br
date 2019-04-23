---
title: Configurar o encaminhamento de DNS e a relação de confiança de domínio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ebc9c2a3cac85ab998075d784111808b3d590d46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854137"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurar o encaminhamento de DNS no domínio do HGS e uma relação de confiança unidirecional com o domínio de malha

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

>[!IMPORTANT]
>Modo AD é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 

Use as seguintes etapas para configurar o encaminhamento de DNS e estabelecer uma relação de confiança unidirecional com o domínio de malha. Estas etapas permitem que o HGS para localizar a malha de controladores de domínio e valide a associação de grupo de hosts do Hyper-V.

1.  Execute o seguinte comando em uma sessão elevada do PowerShell para configurar o encaminhamento de DNS. Substitua fabrikam.com com o nome do domínio do fabric e digite os endereços IP dos servidores DNS no domínio do fabric. Para maior disponibilidade, aponte para mais de um servidor DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Para criar uma relação de confiança de floresta unidirecional, execute o seguinte comando no Prompt de comando elevado:

    Substitua `bastion.local` com o nome do domínio HGS e `fabrikam.com` com o nome do domínio do fabric. Forneça a senha para um administrador do domínio do fabric.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Próximas etapas 

>[!div class="nextstepaction"]
[Configurar o HTTPS](guarded-fabric-configure-hgs-https.md)
