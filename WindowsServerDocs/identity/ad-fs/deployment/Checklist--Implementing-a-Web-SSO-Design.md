---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: Lista de verificação-implementando um design de SSO da Web
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 298233eb20bd54e3e07eb87e029211502142b215
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855209"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de verificação: implementando um design de SSO da Web

Esta lista de verificação pai inclui links de referência de\-cruzada para conceitos importantes sobre o logon único de\-da Web\-no design \(SSO\) para Serviços de Federação do Active Directory (AD FS) \(AD FS\). Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência o encaminhar para um tópico conceitual ou para uma lista de verificação subordinada, volte para este tópico após revisar o tópico conceitual ou conclua as tarefas na lista de verificação subordinada, de forma que você possa continuar com as tarefas restantes nesta lista de verificação.  
  
Lista de verificação de SSO da Web do ![](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: Implementando um design de SSO da Web**  
  
||{1&gt;Tarefa&lt;1}|Referência|  
|-|--------|-------------|  
|![SSO da Web](media/icon_checkboxo.gif)|Examine conceitos importantes sobre o design de SSO da Web e determine quais AD FS objetivos de implantação você pode usar para personalizar esse design para atender às necessidades da sua organização. **Observação:**|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[design de SSO da](https://technet.microsoft.com/library/dd807033.aspx) web do ![Web SSO<p>![SSO da Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando suas metas de implantação de AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![SSO da Web](media/icon_checkboxo.gif)|Examine os requisitos de hardware, software, certificado, sistema de nomes de domínio \(DNS\), repositório de atributos e cliente para implantar AD FS em sua organização.|Apêndice A ![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[A: revisando requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO da Web](media/icon_checkboxo.gif)|De acordo com seu plano de design, instale um ou mais servidores de Federação na rede corporativa ou na rede de perímetro. **Observação:** O design de SSO da Web requer que apenas um servidor de Federação funcione com êxito. Um único servidor de Federação atua na função de provedor de declarações e na função de terceira parte confiável.|Lista de verificação de SSO da Web do ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO da Web](media/icon_checkboxo.gif)|\(opcional\) determinar se sua organização precisa ou não de um proxy de servidor de Federação na rede de perímetro.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO da Web do ![: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO da Web](media/icon_checkboxo.gif)|Dependendo de seu plano de design SSO da Web e como você pretende usá-lo, adicione o repositório de atributos apropriado, os objetos de confiança da terceira parte confiável, declarações e regras de declarações ao Serviço de Federação.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO da Web do ![: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![SSO da Web](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, as declarações\-habilitar o aplicativo de navegador da Web, o aplicativo de serviço Web ou o aplicativo Microsoft&reg; Office SharePoint&reg; Server usando o WIF e o SDK do WIF. **Observação:**|SSO da Web do ![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK do Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para SSO da web do ![| 
