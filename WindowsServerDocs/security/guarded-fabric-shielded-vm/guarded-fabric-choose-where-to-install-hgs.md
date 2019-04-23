---
title: Escolha se deseja instalar o HGS em sua própria nova floresta ou em uma floresta de bastiões existente
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4e02cd37391e629c9b947095fe32626bd15726ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827497"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Escolha se deseja instalar o HGS em sua própria floresta dedicada ou em uma floresta de bastiões existente

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


Floresta do Active Directory para HGS é confidencial porque os administradores têm acesso às chaves que controle VMs blindadas. A instalação padrão irá configurar uma nova floresta dedicada para HGS e configurar outras dependências. Essa opção é recomendada porque o ambiente é independente e conhecida para ser seguro quando ele é criado. 

O requisito técnico apenas para instalar o HGS em uma floresta existente é que ele ser adicionado ao domínio raiz; Não há suporte para domínios não raiz. Mas há também os requisitos operacionais e práticas recomendadas relacionadas a segurança para o uso de uma floresta existente. Florestas adequadas propositadamente são criadas para atender a uma função confidencial, como a floresta usada pelo [Privileged Access Management para o AD DS](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) ou um [floresta Enhanced Security ESAE (ambiente administrativo)](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). Tais florestas geralmente apresentam as seguintes características:

- Eles têm alguns administradores (separados de administradores de malha)
- Eles têm um número baixo de logons
- Eles não são para fins gerais por natureza 

Florestas de finalidade geral, como florestas de produção não são adequadas para uso pelo HGS. Florestas de malha também são inadequadas porque HGS precisam ser isolados dos administradores da malha.

## <a name="next-step"></a>Próximas etapas

Escolha a opção de instalação mais adequada para seu ambiente:

- [Instalar o HGS em sua própria floresta dedicada](guarded-fabric-install-hgs-default.md)
- [Instalar o HGS em uma floresta de bastiões existente](guarded-fabric-install-hgs-in-a-bastion-forest.md)


