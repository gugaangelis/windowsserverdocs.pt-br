---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Planejando posicionamento do controlador de domínio raiz da floresta
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57eafcc884a827d98c249e2da0c0af6888abc5b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823637"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planejando posicionamento do controlador de domínio raiz da floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controladores de domínio de raiz de floresta são necessárias para criar caminhos de relação de confiança para os clientes que precisam acessar recursos em domínios que não seja o seu próprio. Coloque os controladores de domínio de raiz de floresta em locais de hub e nos locais que hospedam os data centers. Se os usuários em um determinado local precisam acessar recursos de outros domínios no mesmo local e a disponibilidade de rede entre o datacenter e o local do usuário não é confiável, você pode adicionar um controlador de domínio de raiz da floresta no local ou criar um relação de confiança de atalho entre os dois domínios. Ele é mais econômico para criar uma relação de confiança de atalho entre os domínios, a menos que você tenha outras razões para colocar um controlador de domínio de raiz de floresta em que local.  
  
Ajuda para otimizar as solicitações de autenticação de usuários localizados em qualquer domínio de confianças de atalho. Para obter mais informações sobre confianças de atalho entre domínios, consulte o artigo [Noções básicas sobre quando criar um atalho de relação de confiança](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Para uma planilha ajudar a documentar o posicionamento de controlador de domínio de raiz de floresta, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abra o "posicionamento de controlador de domínio (DSSTOPO_4.doc).  
  
Você precisará consultar essas informações quando você cria o domínio raiz da floresta. Para obter mais informações sobre a implantação do domínio raiz da floresta, consulte [implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
