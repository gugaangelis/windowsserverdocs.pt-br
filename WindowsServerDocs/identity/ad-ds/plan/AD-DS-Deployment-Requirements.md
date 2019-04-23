---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Requisitos de implantação do AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d21491f5ce9c15ecc514e4be24a91de28b0fd0ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890967"
---
# <a name="ad-ds-deployment-requirements"></a>Requisitos de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A estrutura do seu ambiente existente determina a estratégia de implantação do Windows Server 2008 Active Directory serviços de domínio (AD DS). Se você estiver criando um ambiente do AD DS e você não tem uma estrutura de domínio existente, conclua o design do AD DS antes de começar a criar seu ambiente do AD DS. Em seguida, você pode implantar um novo domínio raiz e implantar o restante da estrutura de seu domínio de acordo com seu design.  
  
Além disso, como parte da implantação do AD DS, você pode decidir atualizar e reestruturar seu ambiente. Por exemplo, se sua organização tiver uma estrutura de domínio existente do Windows 2000, você pode executar uma atualização in-loco de alguns domínios e reestruturar outros. Além disso, você pode decidir reduzir a complexidade do seu ambiente, reestruturando domínios entre florestas ou reestruturando domínios dentro de uma floresta, após a implantação do AD DS.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Implantação de um domínio de raiz de floresta Windows Server 2008  
Domínio raiz da floresta fornece a base para sua infraestrutura de floresta do AD DS. Para implantar o AD DS, você deve primeiro implantar um domínio raiz da floresta. Para fazer isso, você deve examinar seu design do AD DS. Configurar o serviço DNS para o domínio raiz da floresta; criar o domínio raiz da floresta, que consiste em implantar controladores de domínio de raiz de floresta, configurar a topologia de site para o domínio raiz da floresta e configurar funções de mestre de operações (também conhecido como operações de mestre únicas flexíveis ou FSMO); e elevar os níveis funcionais de floresta e domínio. A ilustração a seguir mostra o processo geral de implantação de um domínio raiz da floresta.  
  
![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Para obter mais informações, consulte [implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Implantando domínios regionais do Windows Server 2008  
Depois de concluir a implantação do domínio raiz da floresta, você está pronto para implantar qualquer novos domínios regionais do Windows Server 2008 que são especificados pelo seu design. Para fazer isso, você deve implantar controladores de domínio para cada domínio regional. A ilustração a seguir mostra o processo de implantação de domínios regionais.  
  
![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Para obter mais informações, consulte [implantar o Windows Server 2008 domínios regionais](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Atualizando domínios do Active Directory para Windows Server 2008  
Atualizar seus domínios do Windows 2000 ou Windows Server 2003 para Windows Server 2008 é uma forma eficiente e simples para tirar proveito da funcionalidade e recursos adicionais do Windows Server 2008. Você pode atualizar domínios para manter a sua configuração de rede e domínio atual, melhorando a segurança, escalabilidade e capacidade de gerenciamento da infraestrutura de rede. Atualização do Windows 2000 ou Windows Server 2003 para o Windows Server 2008 requer a configuração de rede mínima. Também atualizar tem pouco impacto nas operações do usuário. Para obter mais informações, consulte [atualizando domínios do Active Directory para Windows Server 2008 e domínios do Windows Server 2008 R2 do AD DS](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Reestruturando domínios do AD DS  
Ao reestruturar domínios entre florestas do Windows Server 2008 (reestruturação entre florestas), você pode reduzir o número de domínios em seu ambiente e, portanto, reduzir a sobrecarga e a complexidade administrativa. Ao migrar objetos entre as florestas como parte desse processo de reestruturação, ambos os ambientes de origem destino e o domínio do domínio existem simultaneamente. Isso torna possível para que você possa reverter para o ambiente de origem durante a migração, se necessário.  
  
Ao reestruturar domínios do Windows Server 2008 em uma floresta do Windows Server 2008 (reestruturação intrafloresta), você pode consolidar sua estrutura de domínio e, portanto, reduzir a sobrecarga e a complexidade administrativa. Ao reestruturar domínios dentro de uma floresta, as contas migradas não existem mais no domínio de origem.  
  
Para obter mais informações sobre como usar a migração do Active Directory ferramenta (ADMT) versão 3.1 (ADMT v3.1) para reestruturar domínios, consulte [guia de migração do ADMT V3.1:Migrando](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


