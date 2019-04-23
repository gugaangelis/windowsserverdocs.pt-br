---
title: Implantar hosts protegidos
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3b20a7eb2b5097d8ddb7381fd0304581ca4e6722
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845347"
---
# <a name="deploy-guarded-hosts"></a>Implantar hosts protegidos

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Os tópicos nesta seção descrevem as etapas que um administrador de malha usa para configurar hosts Hyper-V para trabalhar com serviço de guardião de Host (HGS). Antes de iniciar essas etapas, pelo menos um nó na [HGS cluster deve ser configurado](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Para atestado de TPM confiável**:
1. [Configurar a malha DNS](guarded-fabric-configuring-fabric-dns.md): Informa como configurar um encaminhador DNS do domínio do fabric para o domínio do HGS.
2. [Capturar as informações exigidas pelo HGS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): Informa como capturar os identificadores TPM (também chamados de identificadores de plataforma), crie uma política de integridade de código e criar uma linha de base do TPM. Em seguida, você irá fornecer essas informações para o administrador HGS para configurar o Atestado.
3. [Confirme se os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Para atestado de chave de host**:
1. [Criar uma chave de host](guarded-fabric-create-host-key.md#create-a-host-key): Informa como configurar um encaminhador DNS do domínio do fabric para o domínio do HGS.
2. [Adicionar a chave de host para o serviço de Atestado](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): Informa como configurar um grupo de segurança do Active Directory no domínio do fabric, adicione hosts protegidos como membros desse grupo e, em seguida, forneça o identificador de grupo para o administrador HGS. 
3. [Confirme se os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Para atestado de Admin confiável**:
1. [Configurar a malha DNS](guarded-fabric-configuring-fabric-dns.md): Informa como configurar um encaminhador DNS do domínio do fabric para o domínio do HGS.
2. [Criar um grupo de segurança](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): Informa como configurar um grupo de segurança do Active Directory no domínio do fabric, adicione hosts protegidos como membros desse grupo e, em seguida, forneça o identificador de grupo para o administrador HGS. 
3. [Confirme se os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Consulte também

- [Tarefas de implantação para malhas de protegidos e VMs blindadas](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
