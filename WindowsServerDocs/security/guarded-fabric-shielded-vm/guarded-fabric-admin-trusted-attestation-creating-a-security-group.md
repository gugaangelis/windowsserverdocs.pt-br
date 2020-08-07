---
title: Criar um grupo de segurança para hosts protegidos e registrar o grupo com o HGS
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: efcf148356910a250a06ee9165c544c96226e10e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971413"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Criar um grupo de segurança para hosts protegidos e registrar o grupo com o HGS

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

> [!IMPORTANT]
> O modo AD é preterido a partir do Windows Server 2019. Para ambientes em que o atestado do TPM não é possível, configure o [atestado de chave do host](guarded-fabric-initialize-hgs-key-mode.md). O atestado de chave de host fornece garantia semelhante ao modo AD e é mais simples de configurar.

Este tópico descreve as etapas intermediárias para preparar hosts Hyper-V para se tornarem hosts protegidos usando o atestado confiável de administrador (modo AD). Antes de executar essas etapas, conclua as etapas em [Configurando o DNS de malha para hosts que se tornarão hosts protegidos](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Criar um grupo de segurança e adicionar hosts

1. Crie um novo grupo de segurança **global** no domínio de malha e adicione hosts Hyper-V que executarão VMs blindadas. Reinicie os hosts para atualizar sua associação de grupo.

2. Use Get-ADGroup para obter o SID (identificador de segurança) do grupo de segurança e fornecê-lo ao administrador do HGS.

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Comando Get-AdGroup com saída](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrar o SID do grupo de segurança com HGS

1. Em um servidor HGS, execute o seguinte comando para registrar o grupo de segurança com o HGS.
   Execute novamente o comando, se necessário, para grupos adicionais.
   Forneça um nome amigável para o grupo.
   Ele não precisa corresponder ao nome do grupo de segurança Active Directory.

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para verificar se o grupo foi adicionado, execute [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx).

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Confirmar atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="additional-references"></a>Referências adicionais

- [Implantar o Serviço Guardião de Host para hosts protegidos e VMs blindadas](guarded-fabric-deploying-hgs-overview.md)
