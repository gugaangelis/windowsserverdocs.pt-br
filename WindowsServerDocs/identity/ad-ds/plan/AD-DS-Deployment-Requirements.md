---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Requisitos de implantação do AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 228d4d1644c3bae60dcf293540ad764fb511922a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962498"
---
# <a name="ad-ds-deployment-requirements"></a>Requisitos de implantação do AD DS

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A estrutura do ambiente existente determina sua estratégia para a implantação do Windows Server 2008 Active Directory Domain Services (AD DS). Se você estiver criando um ambiente de AD DS e não tiver uma estrutura de domínio existente, conclua seu design de AD DS antes de começar a criar seu ambiente de AD DS. Em seguida, você pode implantar um novo domínio raiz da floresta e implantar o restante da sua estrutura de domínio de acordo com seu design.

Além disso, como parte de sua implantação do AD DS, você pode decidir atualizar e reestruturar seu ambiente. Por exemplo, se sua organização tiver uma estrutura de domínio existente do Windows 2000, você poderá executar uma atualização in-loco de alguns domínios e reestruturar outros. Além disso, você pode decidir reduzir a complexidade do seu ambiente Reestruturando domínios entre florestas ou Reestruturando domínios dentro de uma floresta depois de implantar AD DS.

## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Implantando um domínio raiz de floresta do Windows Server 2008
O domínio raiz da floresta fornece a base para sua infraestrutura de floresta AD DS. Para implantar AD DS, você deve primeiro implantar um domínio raiz da floresta. Para fazer isso, você deve examinar seu design de AD DS; configurar o serviço DNS para o domínio raiz da floresta; Crie o domínio raiz da floresta, que consiste na implantação de controladores de domínio raiz da floresta, na configuração da topologia do site para o domínio raiz da floresta e na configuração das funções do mestre de operações (também conhecidas como operações de mestre único flexíveis ou FSMO); e aumente os níveis funcionais de floresta e domínio. A ilustração a seguir mostra o processo geral de implantação de um domínio raiz da floresta.

![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)

Para obter mais informações, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="deploying-windows-server-2008-regional-domains"></a>Implantando domínios regionais do Windows Server 2008
Depois de concluir a implantação do domínio raiz da floresta, você estará pronto para implantar novos domínios regionais do Windows Server 2008 especificados pelo seu design. Para fazer isso, você deve implantar controladores de domínio para cada domínio regional. A ilustração a seguir mostra o processo de implantação de domínios regionais.

![Requisitos de AD DS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)

Para obter mais informações, consulte [implantando domínios regionais do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755118(v=ws.10)).

## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Atualizando domínios de Active Directory para o Windows Server 2008
Atualizar seus domínios do Windows 2000 ou do Windows Server 2003 para os domínios do Windows Server 2008 é uma maneira eficiente e direta de aproveitar os recursos e a funcionalidade adicionais do Windows Server 2008. Você pode atualizar domínios para manter a configuração atual de rede e domínio e, ao mesmo tempo, melhorar a segurança, a escalabilidade e a capacidade de gerenciamento da sua infraestrutura de rede. A atualização do Windows 2000 ou do Windows Server 2003 para o Windows Server 2008 requer configuração mínima de rede. A atualização também tem pouco impacto nas operações do usuário. Para obter mais informações, consulte [atualizando domínios de Active Directory para os domínios do Windows server 2008 e do Windows server 2008 R2 AD DS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)).

## <a name="restructuring-ad-ds-domains"></a>Reestruturando domínios AD DS
Ao reestruturar domínios entre florestas do Windows Server 2008 (reestruturação entre florestas), você pode reduzir o número de domínios em seu ambiente e, portanto, reduzir a complexidade e a sobrecarga administrativa. Quando você migra objetos entre florestas como parte desse processo de reestruturação, o domínio de origem e os ambientes de domínio de destino existem simultaneamente. Isso possibilita que você reverta para o ambiente de origem durante a migração, se necessário.

Ao reestruturar domínios do Windows Server 2008 em uma floresta do Windows Server 2008 (reestruturação intraflorestal), você pode consolidar sua estrutura de domínio e, portanto, reduzir a complexidade e a sobrecarga administrativa. Ao reestruturar domínios em uma floresta, as contas migradas não existem mais no domínio de origem.

Para obter mais informações sobre como usar a ferramenta de migração de Active Directory (ADMT) versão 3,1 (ADMT v 3.1) para reestruturar domínios, consulte o [Guia do ADMT: migrando e Reestruturando Active Directory domínios](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)).
