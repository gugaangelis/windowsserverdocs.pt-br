---
title: Adicionar informações de host para atestado de Admin confiável
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7949711dbb0f89f5404b491d60938985bfa98c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849457"
---
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autorizar os hosts do Hyper-V usando o atestado de Admin confiável

>[!IMPORTANT]
>Atestado de Admin confiável (modo AD) é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 


Para autorizar um host protegido no modo do AD: 

1. No domínio do fabric, adicione os hosts do Hyper-V para um grupo de segurança.
2. No domínio HGS, registre o SID do grupo de segurança com o HGS. 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Adicione o host do Hyper-V para um grupo de segurança e a reinicialização do host

1. Criar uma **GLOBAL** segurança de grupo no domínio de malha e adicionar hosts Hyper-V que serão executados de VMs blindadas. 
   Reinicie os hosts para atualizar sua participação no grupo.

2. Use Get-ADGroup para obter o identificador de segurança (SID) do grupo de segurança e fornecê-la para o administrador HGS. 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Comando Get-AdGroup com saída](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrar o SID do grupo de segurança com o HGS  

1. Obter o SID do grupo de segurança para hosts protegidos do administrador de malha e execute o seguinte comando para registrar o grupo de segurança com o HGS. 
   Execute novamente o comando se necessário para grupos adicionais. 
   Forneça um nome amigável para o grupo. 
   Ele não precisa corresponder ao nome do grupo de segurança do Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Para verificar se o grupo foi adicionado, execute [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 


