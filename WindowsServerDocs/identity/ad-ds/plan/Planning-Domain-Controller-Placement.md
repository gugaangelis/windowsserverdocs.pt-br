---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planejar o posicionamento de controlador de domínio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 68406d4973dd585bf0a98562c987c1b1512c095c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883357"
---
# <a name="planning-domain-controller-placement"></a>Planejar o posicionamento de controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois que você reuniu todas as informações de rede que serão usadas para projetar a topologia de site, planeje onde você deseja colocar os controladores de domínio, incluindo controladores de domínio de raiz de floresta, controladores de domínio regional, proprietários da função de mestre de operações, e servidores de catálogo global.  
  
No Windows Server 2008, você também pode tirar proveito dos controladores de domínio somente leitura (RODCs). Um RODC é um novo tipo de controlador de domínio que hospeda partições somente leitura do banco de dados do Active Directory. Com exceção das senhas de conta, o RODC tem todos os objetos do Active Directory e os atributos de um controlador de domínio gravável. No entanto, não é possível fazer alterações no banco de dados que é armazenada no RODC. Alterações devem ser feitas em um controlador de domínio gravável e, em seguida, replicadas para o RODC.  
  
Um RODC é projetado principalmente para ser implantado no remoto ou ambientes de filiais, que geralmente têm relativamente poucos usuários, segurança física inadequada, relativamente pouca largura de banda para um site de hub e funcionários com conhecimento limitado de informações TI (tecnologia). Implantando RODCs resultados mais eficiente do acesso aos recursos de rede e segurança aprimorada. Para obter mais informações sobre os recursos do RODC, consulte o AD DS: Controladores de domínio somente leitura ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Para obter informações sobre como implantar um RODC, consulte o guia passo a passo para controladores de domínio somente leitura ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Este guia explica como você pode determinar o número adequado de controladores de domínio e os requisitos de hardware do controlador de domínio para cada domínio que é representado em cada site.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Planejar o posicionamento de controlador de domínio de raiz de floresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planejar o posicionamento de controlador de domínio Regional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planejando o posicionamento do servidor de Catálogo Global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planejando o posicionamento de função de mestre de operações](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


