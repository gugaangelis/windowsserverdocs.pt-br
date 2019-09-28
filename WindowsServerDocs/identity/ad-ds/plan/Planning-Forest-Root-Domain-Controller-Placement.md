---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Planejando posicionamento do controlador de domínio raiz da floresta
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e619d5f5e9cc317f9ba1548d5ed3a32e7bd12c2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402542"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planejando posicionamento do controlador de domínio raiz da floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controladores de domínio raiz de floresta são necessários para criar caminhos de confiança para clientes que precisam acessar recursos em domínios diferentes do seu. Coloque os controladores de domínio raiz da floresta em locais de Hub e em locais que hospedam datacenters. Se os usuários em um determinado local precisarem acessar recursos de outros domínios no mesmo local e a disponibilidade de rede entre o datacenter e o local do usuário não for confiável, você poderá adicionar um controlador de domínio raiz da floresta no local ou criar um confiança de atalho entre os dois domínios. É mais econômico criar uma relação de confiança de atalho entre os domínios, a menos que você tenha outros motivos para posicionar um controlador de domínio raiz de floresta nesse local.  
  
As relações de confiança de atalho ajudam a otimizar as solicitações de autenticação feitas de usuários localizados em qualquer domínio. Para obter mais informações sobre as relações de confiança de atalho entre domínios, consulte o artigo [noções básicas sobre quando criar uma relação de confiança de atalho](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Para obter uma planilha para ajudá-lo a documentar o posicionamento do controlador de domínio raiz da floresta, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e abra "Domain Posicionamento do controlador "(DSSTOPO_4. doc).  
  
Você precisará consultar essas informações ao criar o domínio raiz da floresta. Para obter mais informações sobre como implantar o domínio raiz da floresta, consulte [implantando um domínio raiz da floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
