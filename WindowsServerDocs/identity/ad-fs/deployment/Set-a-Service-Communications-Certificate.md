---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: "Defina um certificado de comunicações de serviço"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="set-a-service-communications-certificate"></a>Defina um certificado de comunicações de serviço

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de Federação em serviços de Federação do Active Directory \(AD FS\) usam o certificado de comunicações de serviço para proteger o tráfego de serviços da Web para comunicação Secure Sockets Layer \(SSL\) com clientes da Web ou com proxies de servidor de Federação. Isso é o mesmo certificado que usa um servidor de federação como o certificado SSL em \(IIS\) Internet Information Services.  
  
Você pode usar o procedimento a seguir para alterar o certificado de comunicações de serviço com o AD FS snap\-in Gerenciamento.  
  
> [!NOTE]  
> O AD FS snap\-in Gerenciamento refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação do serviço.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-set-a-service-communications-certificate"></a>Para definir um certificado de comunicações de serviço  
  
1.  Sobre o **iniciar** de tela, digite**AD FS gerenciamento**, e pressione ENTER.  
  
2.  Na árvore de console, clique em double\ **Service**e clique em **certificados**.  
  
3.  No **ações** painel, clique no **definir certificado de comunicações de serviço** link.  
  
4.  No **selecionar um certificado de comunicações de serviço** caixa de diálogo caixa, navegue até o arquivo do certificado que você deseja definir como o certificado de comunicações de serviço, selecione o arquivo de certificado e clique em **abrir**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx)  
  

