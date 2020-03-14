---
title: Considerações sobre filiais
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 5a07553e6662fd79230d566ba2049c5e8997f4d6
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322498"
---
# <a name="branch-office-considerations"></a>Considerações das filiais

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), 

Este artigo descreve as práticas recomendadas para executar máquinas virtuais blindadas em filiais e outros cenários remotos em que os hosts do Hyper-V podem ter períodos de tempo com conectividade limitada com o HGS.

## <a name="fallback-configuration"></a>Configuração de fallback

A partir do Windows Server versão 1709, você pode configurar um conjunto adicional de URLs de serviço guardião de host em hosts Hyper-V para uso quando o HGS primário não responde.
Isso permite que você execute um cluster HGS local que é usado como um servidor primário para melhorar o desempenho com a capacidade de voltar para o HGS do datacenter corporativo se os servidores locais estiverem inativos.

Para usar a opção de fallback, você precisará configurar dois servidores HGS. Eles podem executar o Windows Server 2019 ou o Windows Server 2016 e fazer parte dos mesmos ou de clusters diferentes. Se forem clusters diferentes, você desejará estabelecer práticas operacionais para garantir que as políticas de atestado estejam em sincronia entre os dois servidores. Ambas precisam ser capazes de autorizar corretamente o host Hyper-V a executar VMs blindadas e ter o material de chave necessário para iniciar as VMs blindadas. Você pode optar por ter um par de certificados de criptografia compartilhada e de autenticação entre os dois clusters ou usar certificados separados e configurar a VM blindada de HGS para autorizar os guardiões (pares de certificado de criptografia/autenticação) no arquivo de dados de blindagem.

Em seguida, atualize seus hosts Hyper-V para o Windows Server versão 1709 ou o Windows Server 2019 e execute o seguinte comando:
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Para desconfigurar um servidor de fallback, basta omitir os dois parâmetros de fallback:
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Para que o host Hyper-V transmita o atestado com os servidores primário e de fallback, você precisará garantir que suas informações de atestado estejam atualizadas com ambos os clusters HGS.
Além disso, os certificados usados para descriptografar o TPM da máquina virtual precisam estar disponíveis em ambos os clusters HGS.
Você pode configurar cada HGS com certificados diferentes e configurar a VM para confiar em ambos ou adicionar um conjunto compartilhado de certificados a ambos os clusters HGS.

Para obter informações adicionais sobre como configurar o HGS em uma filial usando URLs de fallback, consulte a postagem de blog [melhorou o suporte de filial para VMs blindadas no Windows Server, versão 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Modo offline

O modo offline permite que a VM blindada seja ativada quando o HGS não puder ser acessado, desde que a configuração de segurança do host do Hyper-V não tenha sido alterada.
O modo offline funciona armazenando em cache uma versão especial do protetor de chave de TPM de VM no host Hyper-V.
O protetor de chave é criptografado para a configuração de segurança atual do host (usando a chave de identidade de segurança baseada em virtualização).
Se o host não puder se comunicar com o HGS e sua configuração de segurança não tiver sido alterada, ele poderá usar o protetor de chave em cache para iniciar a VM blindada.
Quando as configurações de segurança são alteradas no sistema, como uma nova política de integridade de código aplicada ou inicialização segura sendo desabilitada, os protetores de chave em cache serão invalidados e o host terá que atestar um HGS antes que qualquer VM blindada possa ser iniciada offline novamente.

O modo offline requer o Windows Server Insider Preview versão 17609 ou mais recente para o cluster do serviço guardião de host e para o host Hyper-V.
Ele é controlado por uma política no HGS, que é desabilitada por padrão.
Para habilitar o suporte para o modo offline, execute o seguinte comando em um nó HGS:

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Como os protetores de chave armazenáveis em cache são exclusivos para cada VM blindada, você precisará desligar completamente (não reiniciar) e iniciar suas VMs blindadas para obter um protetor de chave armazenável em cache depois que essa configuração estiver habilitada no HGS.
Se a sua VM blindada migrar para um host Hyper-V executando uma versão mais antiga do Windows Server ou obtiver um novo protetor de chave de uma versão mais antiga do HGS, ela não poderá ser iniciada no modo offline, mas poderá continuar sendo executada no modo online quando o acesso ao HGS estiver disponível poderá.
