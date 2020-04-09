---
title: Implantar hosts protegidos
ms.prod: windows-server
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0f678172d397ff61fd336b7c844d43f77bea7fad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856829"
---
# <a name="deploy-guarded-hosts"></a>Implantar hosts protegidos

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Os tópicos nesta seção descrevem as etapas que um administrador de malha usa para configurar os hosts do Hyper-V para trabalhar com o serviço de guardião de host (HGS). Para poder iniciar essas etapas, pelo menos um nó no [cluster HgS deve ser configurado](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Para atestado confiável de TPM**:
1. [Configurar o DNS de malha](guarded-fabric-configuring-fabric-dns.md): informa como configurar um encaminhador DNS do domínio de malha para o domínio HgS.
2. [Capturar informações exigidas pelo HgS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): informa como capturar identificadores do TPM (também chamados de identificadores de plataforma), criar uma política de integridade de código e criar uma linha de base do TPM. Em seguida, você fornecerá essas informações ao administrador do HGS para configurar o atestado.
3. [Confirmar que os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Para atestado de chave de host**:
1. [Criar uma chave de host](guarded-fabric-create-host-key.md#create-a-host-key): informa como configurar um encaminhador DNS do domínio de malha para o domínio HgS.
2. [Adicionar a chave de host ao serviço de atestado](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): informa como configurar um grupo de segurança Active Directory no domínio de malha, adicionar hosts protegidos como membros desse grupo e fornecer esse identificador de grupo ao administrador do HgS. 
3. [Confirmar que os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Para o atestado confiável de administrador**:
1. [Configurar o DNS de malha](guarded-fabric-configuring-fabric-dns.md): informa como configurar um encaminhador DNS do domínio de malha para o domínio HgS.
2. [Criar um grupo de segurança](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): informa como configurar um grupo de segurança Active Directory no domínio de malha, adicionar hosts protegidos como membros desse grupo e fornecer esse identificador de grupo ao administrador do HgS. 
3. [Confirmar que os hosts protegidos podem atestar](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Consulte também

- [Tarefas de implantação para malhas protegidas e VMs blindadas](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
