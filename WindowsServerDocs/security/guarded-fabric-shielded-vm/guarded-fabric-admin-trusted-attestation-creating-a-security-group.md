---
title: Criar um grupo de segurança para hosts protegidos e registrar o grupo com o HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: fb84720b94746a3c5757037ceb5c9bc8c965ff7f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447165"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Criar um grupo de segurança para hosts protegidos e registrar o grupo com o HGS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

>[!IMPORTANT]
>Modo AD é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 


Este tópico descreve as etapas intermediárias para preparar os hosts do Hyper-V para se tornam hosts protegidos usando o atestado de Admin confiável (modo AD). Antes de executar essas etapas, conclua as etapas em [Configurando a malha DNS para hosts que se tornarão hosts protegidos](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Criar um grupo de segurança e adicionar hosts

1. Criar um novo **GLOBAL** segurança de grupo no domínio de malha e adicionar hosts Hyper-V que serão executados de VMs blindadas. Reinicie os hosts para atualizar sua participação no grupo.

2. Use Get-ADGroup para obter o identificador de segurança (SID) do grupo de segurança e fornecê-la para o administrador HGS. 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Comando Get-AdGroup com saída](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrar o SID do grupo de segurança com o HGS  

1. Em um servidor HGS, execute o seguinte comando para registrar o grupo de segurança com o HGS. 
   Execute novamente o comando se necessário para grupos adicionais. 
   Forneça um nome amigável para o grupo. 
   Ele não precisa corresponder ao nome do grupo de segurança do Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para verificar se o grupo foi adicionado, execute [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Confirmar atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Consulte também

- [Implantar o serviço guardião de Host para hosts protegidos e VMs blindadas](guarded-fabric-deploying-hgs-overview.md)
