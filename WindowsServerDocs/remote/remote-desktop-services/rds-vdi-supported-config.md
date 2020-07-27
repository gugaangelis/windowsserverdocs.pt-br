---
title: Configurações de segurança do Windows 10 com suporte para os Serviços de Área de Trabalho Remota VDI
description: Fornece informações sobre configurações compatíveis com Windows 10 VDI com RDS no Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: 4ecd290a910ddbb1aba85b26764fb86e30731172
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963348"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configurações de segurança do Windows 10 com suporte para os Serviços de Área de Trabalho Remota VDI

> Aplica-se a: Windows Server 2016

O Windows 10 e o Windows Server 2016 têm novas camadas de proteção integradas ao sistema operacional para proteger ainda mais contra violações de segurança, ajudar a bloquear ataques maliciosos e aprimorar a segurança de máquinas virtuais, aplicativos e dados.

> [!NOTE]
> Certifique-se de examinar as [Informações de configuração com suporte nos Serviços de Área de Trabalho Remota](rds-supported-config.md).

A tabela a seguir descreve qual desses novos recursos têm suporte em uma implantação de VDI usando RDS.

|  Tipo de coleção do VDI               |  Gerenciamento em pool |  Gerenciamento pessoal |  Sem gerenciamento em pool                                     |  Sem gerenciamento pessoal                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)                    | Sim              | Sim                | Sim                                                    | Sim                                                    |
| [Device Guard](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)                        | Sim              | Sim                | Sim                                                    | Sim                                                    |
| [Credential Guard Remoto](/windows/security/identity-protection/remote-credential-guard)             | Não               | Não                 | Não                                                     | Não                                                     |
| [VMs Blindadas e Criptografadas com Suporte](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | Não               | Não                 | VMs com suporte de criptografia com configuração adicional | VMs com suporte de criptografia com configuração adicional |

## <a name="remote-credential-guard"></a>Credential Guard Remoto:

O Credential Guard remoto tem suporte apenas para conexões diretas com as máquinas de destino e não para aquelas pelo Agente de Conexão de Área de Trabalho Remota e Gateway de Área de Trabalho Remota.
> [!NOTE]
> Se você tiver um Agente de Conexão em um ambiente de instância única e o nome DNS corresponde ao nome do computador, você poderá usar o Credential Guard Remoto, embora não haja suporte para isso.

## <a name="shielded-vms-and-encryption-supported-vms"></a>VMs blindadas e criptografia com suporte: 

- Não há suporte para VMs blindadas no VDI de Serviços de Área de Trabalho Remota 

Para aproveitar VMs com Suporte para Criptografia:
- Use uma coleção não gerenciada e uma tecnologia de provisionamento fora do processo de criação de coleção de Serviços de Área de Trabalho Remota para provisionar as máquinas virtuais. 
- Discos de Perfil do Usuário não têm suporte, pois eles dependem de discos diferenciais 
