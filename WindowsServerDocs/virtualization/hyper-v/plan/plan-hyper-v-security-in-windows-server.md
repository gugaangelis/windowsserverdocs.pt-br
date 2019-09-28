---
title: Planejar a segurança do Hyper-V no Windows Server
description: Fornece uma lista de considerações de segurança para hosts e máquinas virtuais do Hyper-v
ms.prod: windows-server
ms.service: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathyDav
ms.date: 08/03/2018
ms.openlocfilehash: 8fd86ae500fff1e6b8c27b0d34d1dcbeeade9f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392955"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Planejar a segurança do Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Proteja o sistema operacional do host Hyper-V, as máquinas virtuais, os arquivos de configuração e os dados da máquina virtual. Use a lista de práticas recomendadas a seguir para ajudá-lo a proteger seu ambiente do Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Proteger o host Hyper-V
- **Mantenha o sistema operacional do host seguro.**
    - Minimize a superfície de ataque usando a opção de instalação mínima do Windows Server necessária para o sistema operacional de gerenciamento. Para obter mais informações, consulte a [seção Opções de instalação](/windows-server/windows-server#installation-options) da biblioteca de conteúdo técnico do Windows Server. Não recomendamos que você execute cargas de trabalho de produção no Hyper-V no Windows 10.
    - Mantenha o sistema operacional do host do Hyper-V, o firmware e os drivers de dispositivo atualizados com as atualizações de segurança mais recentes. Verifique as recomendações do seu fornecedor para atualizar o firmware e os drivers.
    - Não use o host Hyper-V como uma estação de trabalho ou instale qualquer software desnecessário.
    - Gerencie remotamente o host Hyper-V. Se você precisar gerenciar o host Hyper-V localmente, use o Credential Guard. Para obter mais informações, consulte [Proteger credenciais de domínio derivadas com o Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Habilitar políticas de integridade de código. Use serviços de integridade de código protegidos por segurança baseada em virtualização. Para obter mais informações, consulte [Guia de implantação do Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Use uma rede segura.**
    - Use uma rede separada com um adaptador de rede dedicado para o computador físico do Hyper-V.
    - Use uma rede privada ou segura para acessar configurações de VM e arquivos de disco rígido virtual.
    - Use uma rede privada/dedicada para o tráfego de migração ao vivo. Considere habilitar o IPSec nessa rede para usar a criptografia e proteger os dados da VM pela rede durante a migração. Para obter mais informações, consulte [Configurar hosts para migração ao vivo sem clustering de failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Tráfego de migração de armazenamento seguro.** 

    Use SMB 3,0 para criptografia de ponta a ponta de dados SMB e violação de proteção de dados ou espionagem em redes não confiáveis. Use uma rede privada para acessar o conteúdo do compartilhamento SMB para evitar ataques man-in-the-Middle. Para obter mais informações, consulte [melhorias de segurança SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configure os hosts para que façam parte de uma malha protegida.** 

    Para obter mais informações, consulte [malha protegida](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Proteger dispositivos.** 

    Proteja os dispositivos de armazenamento em que você mantém os arquivos de recursos da máquina virtual.
    
- **Proteja o disco rígido.** 

    Use Criptografia de Unidade de Disco BitLocker para proteger os recursos.
    
- **Proteger o sistema operacional do host Hyper-V.** 

    Use as recomendações de configuração de segurança de linha de base descritas na [linha de base de segurança do Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Conceda as permissões apropriadas.**
    - Adicione usuários que precisam gerenciar o host Hyper-V para o grupo Administradores do Hyper-V.
    - Não conceda permissões de administradores de máquina virtual no sistema operacional do host Hyper-V.

- **Configure as exclusões e as opções de antivírus para o Hyper-V.**  

    O Windows Defender já tem [exclusões automáticas](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) configuradas. Para obter mais informações sobre exclusões, consulte [exclusões de antivírus recomendadas para hosts Hyper-V](https://support.microsoft.com/kb/3105657). 

- **Não monte VHDs desconhecidos.** Isso pode expor o host a ataques de nível do sistema de arquivos.

- **Não habilite o aninhamento em seu ambiente de produção, a menos que seja necessário.**

    Se você habilitar o aninhamento, não execute hipervisores sem suporte em uma máquina virtual.  

Para ambientes mais seguros:

- **Use o hardware com um chip Trusted Platform Module (TPM) 2,0 para configurar uma malha protegida.** 

    Para obter mais informações, consulte [requisitos do sistema para o Hyper-V no Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Proteger máquinas virtuais
- **Crie máquinas virtuais de geração 2 para sistemas operacionais convidados com suporte.** 

    Para obter mais informações, consulte [configurações de segurança de geração 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Habilite a inicialização segura.** 

    Para obter mais informações, consulte [configurações de segurança de geração 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Mantenha o sistema operacional convidado seguro.**

    - Instale as atualizações de segurança mais recentes antes de ativar uma máquina virtual em um ambiente de produção.
    - Instale o Integration Services para os sistemas operacionais convidados com suporte que precisam dele e mantenha-o atualizado. As atualizações do serviço de integração para convidados que executam versões do Windows com suporte estão disponíveis por meio de Windows Update.
    - Proteger o sistema operacional executado em cada máquina virtual com base na função que ele executa. Use as recomendações de configuração de segurança de linha de base descritas na [linha de base de segurança do Windows](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Use uma rede segura.** 

    Verifique se os adaptadores de rede virtual se conectam ao comutador virtual correto e têm a configuração de segurança e os limites apropriados aplicados.
    
- **Armazene discos rígidos virtuais e arquivos de instantâneo em um local seguro.**

- **Proteger dispositivos.** 

    Configure somente os dispositivos necessários para uma máquina virtual. Não habilite a atribuição de dispositivo discreta em seu ambiente de produção, a menos que você precise dele para um cenário específico. Se você habilitá-la, certifique-se de expor apenas os dispositivos de fornecedores confiáveis. 
    
- **Configure antivírus, firewall e software de detecção de intrusão** em máquinas virtuais, conforme apropriado, com base na função de máquina virtual.

- **Habilite a segurança baseada em virtualização para convidados que executam o Windows 10 ou o Windows Server 2016 ou posterior.** 

    Para obter mais informações, consulte o [Guia de implantação do Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Habilite apenas a atribuição de dispositivo discreta, se necessário, para uma carga de trabalho específica**. 

    Devido à natureza de passar por um dispositivo físico, trabalhe com o fabricante do dispositivo para entender se ele deve ser usado em um ambiente seguro.

Para ambientes mais seguros:

- **Implante máquinas virtuais com a blindagem habilitada e implante-as em uma malha protegida.** 

    Para obter mais informações, consulte [configurações de segurança de geração 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) e [malha protegida](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
