---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: "Planejar o posicionamento do controlador de domínio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c6d9ca064699f95390e5b95863a6871ea2dc9d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-domain-controller-placement"></a>Planejar o posicionamento do controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois que você coletou todas as informações de rede que serão usadas para projetar a topologia de site, planeje onde você quer colocar controladores de domínio, incluindo controladores de domínio de raiz da floresta, controladores de domínio regionais, mestre de operações titulares de função e servidores de catálogo global.  
  
No Windows Server 2008, você também pode tirar proveito de controladores de domínio somente leitura (RODCs). Um RODC é um novo tipo de controlador de domínio que hospeda partições somente leitura do banco de dados do Active Directory. Com exceção das senhas de conta, o RODC tem todos os objetos do Active Directory e atributos que mantém um controlador de domínio gravável. No entanto, não é fazer alterações no banco de dados que é armazenado no RODC. Alterações devem ser feitas em um controlador de domínio gravável e, em seguida, replicadas de volta para o RODC.  
  
Um RODC foi projetado principalmente para ser implantado no controle remoto ou ambientes de filiais, que geralmente têm relativamente poucos usuários, segurança física ruim, largura de banda de rede relativamente ruim para um site de hub e pessoal com conhecimento limitado de tecnologia da informação (TI). Implantando RODCs resultados segurança aprimorada e mais eficiente acesso aos recursos de rede. Para obter mais informações sobre os recursos RODC, veja AD DS: Read-Only controladores de domínio ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Para obter informações sobre como implantar um RODC, consulte o Step-by-Step guia para Read-Only controladores de domínio ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Este guia não explica como você determina o número correto de controladores de domínio e os requisitos de hardware do controlador de domínio para cada domínio que é representado em cada site.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Planejar o posicionamento de controladores de domínio de raiz de floresta](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planejar o posicionamento do controlador de domínio Regional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planejar o posicionamento do servidor de Catálogo Global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planejar o posicionamento da função mestre de operações](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


