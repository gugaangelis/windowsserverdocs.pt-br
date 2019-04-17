---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: "Lista de verificação - implementando um Design SSO da Web"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de verificação: Implementando um Design SSO da Web

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de verificação de pai inclui links cross\ referência aos conceitos importantes sobre a Web \(SSO\) Sign\ Single\ no design para os serviços de Federação do Active Directory \(AD FS\). Ele também contém links para subordinadas listas de verificação que ajudarão você a concluir as tarefas que são necessárias para implementar esse design.  
  
> [!NOTE]  
> Conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um tópico conceitual ou uma lista de verificação subordinada, retorne para este tópico depois que você revise o tópico conceitual ou concluir as tarefas na lista de verificação subordinada para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Implementando um Design de SSO da Web**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Sso da Web](media/icon_checkboxo.gif)|Revise conceitos importantes sobre o design de SSO da Web e determine quais metas de implantação do AD FS você pode usar para personalizar esse design para atender às necessidades da sua organização. **Observação:**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Design de SSO da Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando as metas de implantação do AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Sso da Web](media/icon_checkboxo.gif)|Examine o hardware, software, certificado, \(DNS\) Domain Name System, loja de atributo e requisitos do cliente para a implantação do AD FS em sua organização.|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[apêndice a: examinando o AD FS requisitos](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Sso da Web](media/icon_checkboxo.gif)|Acordo com seu plano de design, instale um ou mais servidores de federação na rede corporativa ou na rede de perímetro. **Observação:** o design da Web SSO requer apenas um servidor de federação para funcionar com êxito. Um servidor de Federação único funciona na função de provedor de declarações e a terceira função de terceiros.|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: configuração de backup de um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Sso da Web](media/icon_checkboxo.gif)|\(Optional\) determinar ou não a organização precisa um proxy de servidor de federação na rede de perímetro.|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: configuração de backup de Federação servidor Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Sso da Web](media/icon_checkboxo.gif)|Dependendo do seu plano de design de SSO da Web e como pretende usá-lo, adicione o repositório de atributo apropriado, contar relações de confiança de terceiros, requerimentos judiciais ou Extrajudiciais e reivindicar regras para o serviço de Federação.|![Web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Sso da Web](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, claims\ habilitar seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo do Microsoft® Office SharePoint® Server usando WIF e o WIF SDK. **Observação:**|![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
