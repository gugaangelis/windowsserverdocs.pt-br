---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: "Requisitos de implantação do AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7edd7de8077a077245416f859838a6bc55415edc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-deployment-requirements"></a>Requisitos de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A estrutura do ambiente existente determina a estratégia de implantação do Windows Server 2008 Active Directory serviços de domínio (AD DS). Se você estiver criando um ambiente AD DS e você não tem uma estrutura de domínio existente, conclua seu design do AD DS antes de começar a criar seu ambiente AD DS. Em seguida, você pode implantar um novo domínio raiz e implante o resto da estrutura do domínio acordo com seu design.  
  
Além disso, como parte da implantação do AD DS, você pode decidir atualizar e reestruturação seu ambiente. Por exemplo, caso a organização tenha uma estrutura de domínio existente do Windows 2000, você pode realizar uma atualização in-loco de alguns domínios e reestruturação outras pessoas. Além disso, você pode decidir reduzir a complexidade do ambiente de reestruturação domínios entre florestas ou reestruturação domínios em uma floresta depois de implantar o AD DS.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Implantação de um domínio de raiz de floresta do Windows Server 2008  
Domínio raiz da floresta fornece a base para sua infraestrutura de floresta do AD DS. Para implantar o AD DS, primeiro você deve implantar um domínio raiz da floresta. Para fazer isso, você deve analisar seu design do AD DS. Configurar o serviço DNS para o domínio raiz da floresta; criar o domínio raiz da floresta, que consiste em implantar controladores de domínio raiz da floresta, configurando a topologia de site para o domínio raiz da floresta e configurar funções mestre de operações (também conhecida como operações mestre únicas flexíveis ou FSMO); e gere os níveis funcionais de floresta e domínio. A ilustração a seguir mostra o processo geral de implantação de um domínio raiz da floresta.  
  
![Requisitos do AD DS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Para obter mais informações, consulte [Implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Implantando domínios regionais do Windows Server 2008  
Depois de concluir a implantação do domínio raiz da floresta, você estará pronto para implantar qualquer novo domínios regionais do Windows Server 2008 que são especificados por seu design. Para fazer isso, você deve implantar controladores de domínio para cada domínio regional. A ilustração a seguir mostra o processo de implantação de domínios regionais.  
  
![Requisitos do AD DS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Para obter mais informações, consulte [implantação do Windows Server 2008 regionais domínios](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Atualizando domínios do Active Directory para Windows Server 2008  
Atualizar seus domínios do Windows 2000 ou Windows Server 2003 para Windows Server 2008 é uma maneira eficiente e direta para tirar proveito dos recursos adicionais do Windows Server 2008 e funcionalidade. Você pode atualizar domínios para manter a configuração de rede e domínio atual e melhorando a capacidade de gerenciamento de sua infraestrutura de rede, segurança e escalabilidade. Atualizando do Windows 2000 ou Windows Server 2003 para o Windows Server 2008 requer a configuração de rede mínima. Também atualização tem pouco impacto nas operações do usuário. Para obter mais informações, consulte [atualizando domínios do Active Directory para Windows Server 2008 e Windows Server 2008 R2 AD DS domínios](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Reestruturação domínios do AD DS  
Quando você reestruturação domínios entre florestas do Windows Server 2008 (reestruturação entre florestas), você pode reduzir o número de domínios em seu ambiente e, portanto, reduzir a sobrecarga e complexidade administrativa. Ao migrar objetos entre florestas como parte desse processo de reestruturação, ambos os ambientes de origem domínio e o destino de domínio existem simultaneamente. Isso torna possível para a reversão para o ambiente de origem durante a migração, se necessário.  
  
Quando reestruturação domínios do Windows Server 2008 em uma floresta do Windows Server 2008 (entre florestas reestruturação), você pode consolidar sua estrutura de domínio e, portanto, reduzir a sobrecarga e complexidade administrativa. Quando você reestruturação domínios em uma floresta, as contas migradas não existem mais no domínio de origem.  
  
Para saber mais sobre como usar o Active Directory ferramenta de migração (ADMT) versão 3.1 (ADMT v. 3.1) para reestruturação domínios, veja [o guia de migração do ADMT v 3.1](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


