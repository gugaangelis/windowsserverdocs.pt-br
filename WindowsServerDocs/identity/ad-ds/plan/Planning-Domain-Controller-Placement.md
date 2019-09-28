---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planejar o posicionamento de controlador de domínio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ff0cba67454080db7cca4b012ae0a2d5cb40e412
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408753"
---
# <a name="planning-domain-controller-placement"></a>Planejar o posicionamento de controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de coletar todas as informações de rede que serão usadas para criar a topologia do site, planeje o local em que você deseja posicionar os controladores de domínio, incluindo controladores de domínio raiz da floresta, controladores de domínio regionais, detentores de função de mestre de operações e servidores de catálogo global.  
  
No Windows Server 2008, você também pode aproveitar os RODCs (controladores de domínio somente leitura). Um RODC é um novo tipo de controlador de domínio que hospeda partições somente leitura do banco de dados Active Directory. Exceto para senhas de contas, um RODC mantém todos os objetos Active Directory e atributos que um controlador de domínio gravável mantém. No entanto, as alterações não podem ser feitas no banco de dados armazenado no RODC. As alterações devem ser feitas em um controlador de domínio gravável e, em seguida, replicadas de volta para o RODC.  
  
Um RODC foi projetado principalmente para ser implantado em ambientes remotos ou de filiais, que normalmente têm relativamente poucos usuários, pouca segurança física, largura de banda de rede relativamente ruim a um site de Hub e pessoal com conhecimento limitado de informações tecnologia (TI). A implantação de RODCs resulta em segurança aprimorada e acesso mais eficiente aos recursos de rede. Para obter mais informações sobre os recursos do RODC, consulte AD DS: Controladores de domínio somente leitura ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Para obter informações sobre como implantar um RODC, consulte o guia passo a passo para controladores de domínio somente leitura ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Este guia não explica como determinar o número correto de controladores de domínio e os requisitos de hardware do controlador de domínio para cada domínio representado em cada site.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Como planejar o posicionamento do controlador de domínio raiz da floresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planejando o posicionamento do controlador de domínio regional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Como planejar o posicionamento do servidor de catálogo global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Como planejar o posicionamento da função de mestre das operações](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


