---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: Lista de verificação - implementando um Design SSO da Web
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864037"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de verificação: implementando um Design SSO da Web

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de verificação pai inclui cruzadas\-links para conceitos importantes sobre o único da Web de referência\-sinal\-na \(SSO\) design para serviços de Federação do Active Directory \(do AD FS \). Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência o encaminhar para um tópico conceitual ou para uma lista de verificação subordinada, volte para este tópico após revisar o tópico conceitual ou conclua as tarefas na lista de verificação subordinada, de forma que você possa continuar com as tarefas restantes nesta lista de verificação.  
  
![sso da Web](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Implementando um Design SSO da Web**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![sso da Web](media/icon_checkboxo.gif)|Examine os conceitos importantes sobre o design de SSO da Web e determine quais metas de implantação de AD FS que você pode usar para personalizar esse design e atender às necessidades da sua organização. **Observação:**|![sso da Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Design SSO da Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![sso da Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando as metas de implantação do AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![sso da Web](media/icon_checkboxo.gif)|Examinar o hardware, software, certificado, o sistema de nomes de domínio \(DNS\), repositório de atributos e requisitos do cliente para implantar o AD FS em sua organização.|![sso da Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[apêndice a: Revisar os requisitos do AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso da Web](media/icon_checkboxo.gif)|Conforme o plano de design, instale um ou mais servidores de federação na rede corporativa ou na rede de perímetro. **Observação:** O design de SSO da Web requer apenas um servidor de federação para funcionar com êxito. Um servidor de Federação único atua na função de provedor de declarações e a função de terceira parte confiável.|![sso da Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso da Web](media/icon_checkboxo.gif)|\(Opcional\) determinar se sua organização precisa de um proxy do servidor de federação na rede de perímetro.|![sso da Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso da Web](media/icon_checkboxo.gif)|Dependendo de seu plano de design SSO da Web e como você pretende usá-lo, adicione o repositório de atributos apropriado, os objetos de confiança da terceira parte confiável, declarações e regras de declarações ao Serviço de Federação.|![sso da Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![sso da Web](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, declarações\-habilitar seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo do Microsoft® Office SharePoint® Server usando o WIF e o SDK do WIF. **Observação:**|![sso da Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![sso da Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
