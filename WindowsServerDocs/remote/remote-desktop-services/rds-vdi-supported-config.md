---
title: Configurações de segurança com suporte do Windows 10 para VDI de serviços de área de trabalho remota
description: Fornece informações sobre configurações com suporte para Windows 10 VDI com o RDS no Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: ff890150dcea30c425267dcaae9b1bdbc6d78b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820077"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configurações de segurança com suporte do Windows 10 para VDI de serviços de área de trabalho remota

> Aplica-se a: Windows Server 2016

Windows 10 e Windows Server 2016 têm novas camadas de proteção integradas ao sistema operacional para proteger contra violações de segurança adicional, ajudar a bloquear ataques maliciosos e aumente a segurança de máquinas virtuais, aplicativos e dados.

> [!NOTE]
> Certifique-se de examinar a [dos serviços de área de trabalho remota tem suporte a informações de configuração](rds-supported-config.md).

A tabela a seguir descreve qual desses novos recursos têm suporte em uma implantação de VDI usando RDS.

|  Tipo de coleção do VDI               |  Gerenciadas em pool |  Gerenciado pessoal |  Não gerenciadas em pool                                     |  Pessoal não gerenciado                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | Sim              | Sim                | Sim                                                    | Sim                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | Sim              | Sim                | Sim                                                    | Sim                                                    |
| [Credential Guard remoto](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | Não               | Não                 | Não                                                     | Não                                                     |
| [Blindado & criptografia de VMs com suporte](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | Não               | Não                 | VMs com suporte de criptografia sem configuração adicional | VMs com suporte de criptografia sem configuração adicional |

## <a name="remote-credential-guard"></a>Credential Guard remoto:

Do Credential Guard remoto tem suporte apenas para conexões diretas com os computadores de destino e não para aqueles por meio do agente de Conexão de área de trabalho remota e Gateway de área de trabalho remota.
> [!NOTE]
> Se você tiver um agente de Conexão em um ambiente de instância única, e o nome DNS corresponde ao nome do computador, você poderá usar o Credential Guard remoto, embora isso não é suportado.

## <a name="shielded-vms-and-encryption-supported-vms"></a>Criptografia e VMs blindadas VMs com suporte: 

- Não há suporte para VMs blindadas no VDI de serviços de área de trabalho remota 

Para aproveitar o suporte para VMs de criptografia:
- Use um conjunto não gerenciado e uma tecnologia de provisionamento fora do processo de criação de coleção de serviços de área de trabalho remota para provisionar as máquinas virtuais. 
- Discos de perfil do usuário não têm suporte pois eles dependem de discos diferenciais 

