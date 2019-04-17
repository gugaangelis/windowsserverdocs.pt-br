---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: "Planejar o posicionamento de controladores de domínio de raiz de floresta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 374ce31c61c666302e2b00a8f365c289cb8799f8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planejar o posicionamento de controladores de domínio de raiz de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controladores de domínio raiz de floresta são necessárias para criar caminhos de confiança para clientes que precisam acessar recursos em domínios diferentes dos seus próprios. Coloque os controladores de domínio raiz de floresta em locais de hub e em locais que hospedam os datacenters. Se os usuários em um determinado local precisam acessar os recursos de outros domínios no mesmo local e a disponibilidade de rede entre o datacenter e o local do usuário é confiável, você pode adicionar um controlador de domínio raiz no local ou criar um atalho de confiança entre os dois domínios. Ele é custo mais eficiente para criar uma relação de confiança de atalho entre os domínios, a menos que você tenha outras razões para colocar um controlador de domínio raiz nesse local.  
  
Atalho confia ajuda a otimizar a solicitações de autenticação feitas de usuários localizados em qualquer domínio. Para obter mais informações sobre o atalho de relações de confiança entre domínios, consulte Noções básicas sobre quando criar um atalho de relação de confiança ([https://go.microsoft.com/fwlink/?LinkId=107061](https://go.microsoft.com/fwlink/?LinkId=107061)).  
  
Para uma planilha para ajudá-lo a documentar o posicionamento de controlador de domínio de raiz de floresta, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Abra (DSSTOPO_4.doc) "Posicionamento do controlador de domínio".  
  
Você precisará fazer referência a essas informações quando você cria o domínio raiz da floresta. Para obter mais informações sobre a implantação do domínio raiz da floresta, consulte [Implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


