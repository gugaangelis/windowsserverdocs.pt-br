---
title: Considerações das filiais
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: d93c37227af1eb62368fbcd4ec5d6a48374b45ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877057"
---
# <a name="branch-office-considerations"></a>Considerações das filiais

> Aplica-se a: Windows Server do Windows Server (canal semestral), de 2019 

Este artigo descreve as práticas recomendadas para executar máquinas virtuais blindadas em filiais e outros cenários remotos em que os hosts Hyper-V podem ter períodos de tempo com conectividade limitada para HGS.

## <a name="fallback-configuration"></a>Configuração de fallback

Começando com o Windows Server versão 1709, você pode configurar um conjunto adicional de URLs de serviço guardião de Host nos hosts do Hyper-V para uso quando o HGS primário não está respondendo.
Isso permite que você execute um cluster HGS local que é usado como um servidor primário para melhorar o desempenho com a capacidade de voltar para HGS do seu datacenter corporativo se os servidores locais estão inativos.

Para usar a opção de fallback, você precisará configurar dois servidores HGS. Eles podem executar o Windows Server 2019 ou Windows Server 2016 e o fazer parte dos mesmos ou em diferentes clusters. Se eles forem diferentes clusters, você desejará estabelecer práticas operacionais para garantir que as políticas de Atestado estão em sincronia entre os dois servidores. Ambos precisam ser capaz de autorizar corretamente o host do Hyper-V para executar VMs blindadas e fazer o material da chave necessário para iniciar o backup de VMs blindadas. Você pode optar por ter um par de criptografia compartilhada e certificados entre os dois clusters, de assinatura ou use certificados separados e configurar o HGS blindado VM para autorizar a ambos os guardiões (pares de certificado de criptografia/assinatura) nos dados de blindagem arquivo.

Em seguida, atualizar seus hosts do Hyper-V para Windows Server versão 1709 ou Windows Server 2019 e execute o seguinte comando:
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Para cancelar um servidor de fallback, basta omita os dois parâmetros de fallback:
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Na ordem do host do Hyper-V passar o Atestado com os servidores primários e de fallback, você precisará garantir que suas informações de Atestado são atualizadas com os dois clusters HGS.
Além disso, os certificados usados para descriptografar o TPM da máquina virtual precisam estar disponível em ambos os clusters HGS.
Você pode configurar cada HGS com certificados diferentes e configurar a VM para confiar em ambos ou adicionar um conjunto compartilhado de certificados para ambos os clusters HGS.

Para obter informações adicionais sobre como configurar o HGS em uma filial usando URLs de fallback, consulte o postagem no blog [melhor suporte a filiais para VMs blindadas no Windows Server, versão 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Modo offline

O modo offline permite que sua VM blindada Ativar quando HGS não pode ser alcançado, desde que a configuração de segurança do host do Hyper-V não foi alterado.
O modo offline funciona armazenando em cache uma versão especial do protetor de chave TPM VM no host Hyper-V.
O protetor de chave é criptografado para a configuração de segurança atual do host (usando a chave de identidade de segurança baseada em virtualização).
Se seu host é capaz de se comunicar com o HGS e sua configuração de segurança não foi alterado, ele poderá usar o protetor de chave em cache para iniciar a VM blindada.
Quando alterar as configurações de segurança no sistema, como uma nova política de integridade de código que está sendo aplicada ou a inicialização segura que está sendo desativado, os protetores de chave em cache serão invalidados e o host terá atestar com um HGS antes de qualquer VMs blindadas podem ser iniciadas off-line novamente.

O modo offline requer a compilação do Windows Server Insider Preview 17609 ou mais recente para o cluster do serviço guardião de Host e o host Hyper-V.
Ele é controlado por uma política de HGS, que é desabilitado por padrão.
Para habilitar o suporte para o modo offline, execute o seguinte comando em um nó HGS:

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Como os protetores de chave armazenáveis em cache são exclusivos para cada máquina virtual blindada, você precisará totalmente desligado (não reiniciar) e iniciar o backup de VMs blindadas para obter um protetor de chave armazenáveis em cache depois que essa configuração está habilitada no HGS.
Se sua máquina virtual blindada migra para um host Hyper-V executando uma versão anterior do Windows Server, ou obtém um novo protetor de chave de uma versão mais antiga do HGS, ele não poderão iniciar em si em modo offline, mas pode continuar em execução no modo online quando o acesso ao HGS for disp é possível.
