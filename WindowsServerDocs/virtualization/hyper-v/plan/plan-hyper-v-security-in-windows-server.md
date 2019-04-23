---
title: Planejar a segurança do Hyper-V no Windows Server
description: Fornece uma lista de considerações de segurança para hosts Hyper-v e máquinas virtuais
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 462124e1907ef03aa7746dd050be3e8c84c7abda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877407"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Planejar a segurança do Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Proteger o sistema operacional do host do Hyper-V, as máquinas virtuais, arquivos de configuração e dados da máquina virtual. Use a seguinte lista de práticas recomendadas como uma lista de verificação para ajudá-lo a proteger seu ambiente do Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Proteger o host Hyper-V
- **Manter o host segura do sistema operacional.**
    - Minimize a superfície de ataque usando a opção de instalação mínima do Windows Server que você precisa para o sistema operacional de gerenciamento. Para obter mais informações, consulte o [seção de opções de instalação](/windows-server/windows-server#installation-options) da biblioteca de conteúdo técnica do Windows Server. Não recomendamos que você execute cargas de trabalho de produção no Hyper-V no Windows 10.
    - Manter o sistema de operacional de host do Hyper-V, firmware e drivers de dispositivo atualizado com as últimas atualizações de segurança. Verificar as recomendações do seu fornecedor para atualizar o firmware e os drivers.
    - Não use o host Hyper-V como uma estação de trabalho ou instalar nenhum software desnecessário.
    - Gerencie remotamente o host Hyper-V. Se você precisa gerenciar o host do Hyper-V localmente, use o Credential Guard. Para obter mais informações, consulte [Proteger credenciais de domínio derivadas com o Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Habilite políticas de integridade de código. Usar a segurança baseada em virtualização protegidos dos serviços de integridade de código. Para obter mais informações, consulte [guia de implantação do Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Use uma rede segura.**
    - Use uma rede separada com um adaptador de rede dedicada para o computador físico do Hyper-V.
    - Use uma rede privada ou segura para as configurações de VM de acesso e os arquivos de disco rígido virtual.
    - Use uma rede privada/dedicado para o tráfego da migração ao vivo. Considere habilitar IPSec nesta rede para usar a criptografia e proteger os dados da sua VM entrar na rede durante a migração. Para obter mais informações, consulte [configurar hosts para migração ao vivo sem Clustering de Failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Proteger o tráfego de migração de armazenamento.** 

    Use o SMB 3.0 para criptografia de ponta a ponta de dados SMB e proteção de dados, violação ou a escuta em redes não confiáveis. Use uma rede privada para acessar o conteúdo do compartilhamento SMB para impedir ataques man-in-the-middle. Para obter mais informações, consulte [aprimoramentos de segurança SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configure hosts para fazer parte de uma malha protegida.** 

    Para obter mais informações, consulte [protegidos do fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Proteger dispositivos.** 

    Proteger os dispositivos de armazenamento em que você mantenha os arquivos de recurso de máquina virtual.
    
- **Proteger o disco rígido.** 

    Use a criptografia de unidade de disco BitLocker para proteger recursos.
    
- **Proteger o sistema operacional do host do Hyper-V.** 

    Use as recomendações de configuração de segurança da linha de base descritas as [linha de base de segurança do Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Conceda as permissões apropriadas.**
    - Adicione usuários que precisam gerenciar o host do Hyper-V para o grupo de administradores do Hyper-V.
    - Não conceda os administradores de máquina virtual o sistema operacional do host de permissões no Hyper-V.

- **Configure opções para o Hyper-V e as exclusões de antivírus.**  

    Já tem o Windows Defender [exclusões automáticas](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) configurado. Para obter mais informações sobre as exclusões, consulte [recomendado exclusões de antivírus para hosts Hyper-V](https://support.microsoft.com/kb/3105657). 

- **Não monte VHDs desconhecidos.** Isso pode expor o host a ataques de nível de sistema de arquivo.

- **Não habilite o aninhamento em seu ambiente de produção, a menos que seja necessário.**

    Se você habilitar o aninhamento, não execute hipervisores sem suporte em uma máquina virtual.  

Para ambientes mais seguros:

- **Use o hardware com um chip Trusted Platform Module (TPM) 2.0 para configurar uma malha protegida.** 

    Para obter mais informações, consulte [requisitos de sistema do Hyper-V no Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Proteger máquinas virtuais
- **Crie a geração 2 máquinas virtuais para sistemas operacionais convidados com suporte.** 

    Para obter mais informações, consulte [configurações de segurança de geração 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Habilite a inicialização segura.** 

    Para obter mais informações, consulte [configurações de segurança de geração 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Manter seguro de sistema operacional convidado.**

    - Instale as atualizações de segurança mais recentes antes de ativar uma máquina virtual em um ambiente de produção.
    - Instale o integration services para os sistemas operacionais de convidados com suporte que precisar dele e mantê-lo atualizado. Atualizações de serviço de integração para convidados que executam versões do Windows com suporte estão disponíveis por meio do Windows Update.
    - Proteger o sistema operacional que é executado em cada máquina virtual com base em função que ele executa. Use as recomendações de configuração de segurança de linha de base que são descritas na [base de segurança do Windows](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Use uma rede segura.** 

    Certifique-se de adaptadores de rede virtual se conectar ao comutador virtual correto e os limites e configurações de segurança adequadas aplicou.
    
- **Store discos rígidos virtuais e arquivos de instantâneo em um local seguro.**

- **Proteger dispositivos.** 

    Configure apenas os dispositivos necessários para uma máquina virtual. Não habilite a atribuição de dispositivo discretos em seu ambiente de produção, a menos que você precisará dela para um cenário específico. Se você habilitá-la, certifique-se de expor somente os dispositivos de fornecedores confiáveis. 
    
- **Configurar antivírus, firewall e o software de detecção de intrusão** em máquinas virtuais conforme apropriado com base na função de máquina virtual.

- **Habilite a segurança de virtualização com base para convidados que executam o Windows 10 ou Windows Server 2016 ou posterior.** 

    Para obter mais informações, consulte o [guia de implantação do Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Habilitar apenas a atribuição de dispositivo discretos se necessário, para uma carga de trabalho específica**. 

    Devido à natureza de passar por meio de um dispositivo físico, trabalhe com o fabricante do dispositivo para entender se ele deve ser usado em um ambiente seguro.

Para ambientes mais seguros:

- **Implantar máquinas virtuais com proteção habilitada e implantá-los em uma malha protegida.** 

    Para obter mais informações, consulte [configurações de segurança de geração 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) e [protegidos do fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
