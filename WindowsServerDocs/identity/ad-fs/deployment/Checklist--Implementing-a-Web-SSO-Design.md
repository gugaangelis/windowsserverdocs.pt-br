---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: Lista de verificação-implementando um design de SSO da Web
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8488e0c9195930374aacd959e72d0eff34142ca7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408467"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Lista de verificação: implementando um Design SSO da Web

Esta lista de verificação pai inclui links Cross no__t-0reference para conceitos importantes sobre o design do Web Single @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 para Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-6. Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência o encaminhar para um tópico conceitual ou para uma lista de verificação subordinada, volte para este tópico após revisar o tópico conceitual ou conclua as tarefas na lista de verificação subordinada, de forma que você possa continuar com as tarefas restantes nesta lista de verificação.  
  
SSO de @no__t 0web @ no__t-1Checklist: Como implementar um design SSO da Web**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![SSO da Web](media/icon_checkboxo.gif)|Examine conceitos importantes sobre o design de SSO da Web e determine quais AD FS objetivos de implantação você pode usar para personalizar esse design para atender às necessidades da sua organização. **Observação:**|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[design de SSO da Web](https://technet.microsoft.com/library/dd807033.aspx) do ![web SSO<br /><br />SSO de @no__t 0web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando suas metas de implantação de AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![SSO da Web](media/icon_checkboxo.gif)|Examine o hardware, o software, o certificado, o sistema de nomes de domínio \(DNS @ no__t-1, o repositório de atributos e os requisitos do cliente para implantar AD FS em sua organização.|SSO de @no__t 0web @ no__t-1Appendix A: Como examinar os requisitos do AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO da Web](media/icon_checkboxo.gif)|De acordo com seu plano de design, instale um ou mais servidores de Federação na rede corporativa ou na rede de perímetro. **Observação:** O design de SSO da Web requer que apenas um servidor de Federação funcione com êxito. Um único servidor de Federação atua na função de provedor de declarações e na função de terceira parte confiável.|SSO de @no__t 0web @ no__t-1Checklist: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO da Web](media/icon_checkboxo.gif)|\(Optional @ no__t-1 determina se sua organização precisa ou não de um proxy de servidor de Federação na rede de perímetro.|SSO de @no__t 0web @ no__t-1Checklist: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO da Web](media/icon_checkboxo.gif)|Dependendo de seu plano de design SSO da Web e como você pretende usá-lo, adicione o repositório de atributos apropriado, os objetos de confiança da terceira parte confiável, declarações e regras de declarações ao Serviço de Federação.|SSO de @no__t 0web @ no__t-1Checklist: Configurando a organização do parceiro de conta @ no__t-0|  
|![SSO da Web](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, o Claims @ no__t-0enable seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo Microsoft® Office SharePoint® Server usando o WIF e o SDK do WIF. **Observação:**|SSO de @no__t 0web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK do Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para sso do @no__t 0web| 
