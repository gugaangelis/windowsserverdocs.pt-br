---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mapear seus requisitos para uma estratégia de implantação do AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a61e5e6d429acd92e48a353bc829cb620dd66054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881827"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mapear seus requisitos para uma estratégia de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de concluir a revisão e identificando o design de serviços de domínio Active Directory (AD DS) e requisitos de implantação e determinar quais deles estão relacionados à sua implantação específica, você pode mapear esses requisitos para uma implantação específica do AD DS estratégia.  
  
Use a tabela a seguir para determinar qual estratégia de implantação do AD DS é mapeado para a combinação apropriada de design do AD DS e a implantação, requisitos de sua organização. ("Sim" significa que um requisito específico é necessário para sua estratégia de implantação; "Não" significa que um requisito específico não é necessário para sua estratégia de implantação.)  
  
Esta tabela refere-se somente para as três estratégias de implantação primárias do AD DS conforme descrito neste guia:  
  
-   [Implantando o AD DS em uma nova organização](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Implantando o AD DS em uma organização do Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Implantando o AD DS em uma organização do Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
No entanto, você pode criar um híbrido ou personalizada estratégia de implantação do AD DS, usando qualquer combinação dos requisitos de design e implantação de AD DS para atender às necessidades da sua organização.  
  
|Requisitos de design e implantação de AD DS|Implantar o AD DS em uma nova organização|Implantar o AD DS em uma organização com o Windows Server 2003|Implantar o AD DS em uma organização com o Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Criando a estrutura lógica para o Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Sim|Sim|Sim|  
|[Projetando a topologia de Site para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Sim|Sim|Sim|  
|Planejando a capacidade do controlador de domínio|Sim|Sim|Sim|  
|[Implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Sim|Não|Não|  
|[Implantando domínios regionais do Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Sim|Sim|Sim|  
|[Habilitar recursos avançados para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Sim|Sim, mas todos os controladores de domínio no ambiente devem executar o Windows Server 2008 antes de definir o nível funcional do domínio ou floresta para o Windows Server 2008.|Sim, mas todos os controladores de domínio no ambiente devem executar o Windows Server 2008 antes de definir o nível funcional do domínio ou floresta para o Windows Server 2008.|  
|[Atualizando domínios do Active Directory para Windows Server 2008 e o AD DS domínios do Windows Server 2008 R2](https://technet.microsoft.com/library/cc731188.aspx)|Não|Sim|Sim|  
|[Reestruturando domínios entre florestas do AD DS](https://go.microsoft.com/fwlink/?LinkId=93678)|Sim, se você quiser migrar um domínio do piloto no ambiente de produção, mesclar com outra organização e consolidar as duas infraestruturas de TI (tecnologia) informações ou consolidar recursos e conta domínios de atualização in-loco do Windows 2000 ou em ambientes Windows Server 2003.|Sim, se você deseja mesclar com outra organização e consolidar as duas infra-estruturas de TI ou consolidar os domínios de recurso e a conta que foi atualizado em vigor a partir de ambientes do Windows 2000 ou Windows Server 2003.|Sim, se você deseja mesclar com outra organização e consolidar as duas infra-estruturas de TI ou consolidar os domínios de recurso e a conta que foi atualizado em vigor a partir de ambientes do Windows 2000 ou Windows Server 2003.|  
|[Reestruturando domínios dentro de florestas do AD DS](https://go.microsoft.com/fwlink/?LinkId=82740))|Não|Sim, se você precisar reduzir o número de domínios, reduzir o tráfego de replicação e a quantidade de administração de grupo e usuário exigida ou simplificam a administração da diretiva de grupo.|Sim, se você precisar reduzir o número de domínios, reduzir o tráfego de replicação e a quantidade de administração de grupo e usuário exigida ou simplificam a administração da diretiva de grupo.|  
  


