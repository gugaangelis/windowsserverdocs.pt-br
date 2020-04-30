---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mapear seus requisitos para uma estratégia de implantação do AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4862116253f51b7072f112c5f6ece72605b63ddb
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624094"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mapear seus requisitos para uma estratégia de implantação do AD DS

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de concluir a revisão e a identificação dos requisitos de design e implantação do Active Directory Domain Services (AD DS) e determinar quais deles estão relacionados à sua implantação específica, você pode mapear esses requisitos para uma estratégia de implantação específica do AD DS.

Use a tabela a seguir para determinar qual AD DS estratégia de implantação mapeia para a combinação apropriada de requisitos de design e implantação de AD DS para sua organização. ("Sim" significa que um requisito específico é necessário para sua estratégia de implantação; "Não" significa que um requisito específico não é necessário para sua estratégia de implantação.)

Esta tabela refere-se apenas às três principais estratégias de implantação de AD DS, conforme descrito neste guia:

-   [Como implantar o AD DS em uma nova organização](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)

-   [Como implantar o AD DS em uma organização com o Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)

-   [Como implantar o AD DS em uma organização com o Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)

No entanto, você pode criar uma estratégia de implantação de AD DS híbrida ou personalizada usando qualquer combinação dos requisitos de design e implantação de AD DS para atender às necessidades da sua organização.

| Requisitos de design e implantação de AD DS | Implantar o AD DS em uma nova organização | Implantar o AD DS em uma organização com o Windows Server 2003 | Implantar o AD DS em uma organização com o Windows 2000 |
| ---------------------------------------- | ------------------------------------- | ----------------------------------------------------- |----------------------------------------------- |
| [Criando a estrutura lógica para o Windows Server 2008 AD DS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770806(v=ws.10)) | Sim | Sim | Sim |
| [Criando a topologia do site para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md) | Sim | Sim | Sim |
| Planejando a capacidade do controlador de domínio | Sim | Sim | Sim |
| [Implantando um domínio raiz de floresta do Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)) | Sim | Não | Não |
| [Implantando domínios regionais do Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)) | Sim | Sim | Sim |
| [Como habilitar recursos avançados para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md) | Sim |Sim, mas todos os controladores de domínio no ambiente devem executar o Windows Server 2008 antes de definir o nível funcional de domínio ou floresta para o Windows Server 2008. | Sim, mas todos os controladores de domínio no ambiente devem executar o Windows Server 2008 antes de definir o nível funcional de domínio ou floresta para o Windows Server 2008. |
| [Atualizando domínios Active Directory para domínios do Windows Server 2008 e do Windows Server 2008 R2 AD DS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)) | Não | Sim | Sim |
| [Guia de ADMT: migrando e Reestruturando domínios Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)) | Sim, se você quiser migrar um domínio piloto para seu ambiente de produção, mescle com outra organização e consolide as duas infraestruturas de ti (tecnologia de informação) ou consolide os domínios de recursos e contas que você atualizou em vigor em ambientes Windows 2000 ou Windows Server 2003. | Sim, se você quiser mesclar com outra organização e consolidar as duas infraestruturas de ti ou consolidar domínios de recursos e contas que você atualizou em vigor em ambientes Windows 2000 ou Windows Server 2003. | Sim, se você quiser mesclar com outra organização e consolidar as duas infraestruturas de ti ou consolidar domínios de recursos e contas que você atualizou em vigor em ambientes Windows 2000 ou Windows Server 2003. |
| [Guia de ADMT: migrando e Reestruturando domínios Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)) | Não | Sim, se você precisar reduzir o número de domínios, reduzir o tráfego de replicação e a quantidade necessária de administração de usuários e grupos, ou simplificar a administração de Política de Grupo. | Sim, se você precisar reduzir o número de domínios, reduzir o tráfego de replicação e a quantidade necessária de administração de usuários e grupos, ou simplificar a administração de Política de Grupo. |
