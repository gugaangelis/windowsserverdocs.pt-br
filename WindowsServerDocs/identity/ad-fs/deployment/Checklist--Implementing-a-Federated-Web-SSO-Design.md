---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: Lista de verificação - implementando um Design SSO da Web federado
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869667"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de verificação: implementando um design de SSO da Web Federado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de verificação pai inclui cruzadas\-links para conceitos importantes sobre a Federated Web único de referência\-sinal\-na \(SSO\) design para os serviços de Federação do Active Directory \(AD FS\). Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando houver um link de referência para um tópico conceitual ou uma lista de verificação subordinada, retorne a este tópico depois de revisar aquele ou concluir as tarefas da lista de verificação subordinada para poder executar as tarefas restantes desta lista de verificação.  
  
![sso da web federado](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Implementando um Design SSO da Web federado**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![sso da web federado](media/icon_checkboxo.gif)|Examine os conceitos importantes sobre o design de SSO da Web federado e determine quais metas de implantação de AD FS que você pode usar para personalizar esse design e atender às necessidades da sua organização.|![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando as metas de implantação do AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Planejando a implantação](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso da web federado](media/icon_checkboxo.gif)|Examinar o hardware, software, certificado, o sistema de nomes de domínio \(DNS\), repositório de atributos e requisitos do cliente para implantar o AD FS em sua organização.|![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[apêndice a: Revisar os requisitos do AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso da web federado](media/icon_checkboxo.gif)|Examine os conceitos importantes sobre declarações, regras de declaração, armazenamentos de atributos e o banco de dados de configuração do AD FS antes de implantar o AD FS nas duas organizações parceiras.|![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso da web federado](media/icon_checkboxo.gif)|De acordo com o seu plano de design, instale um ou mais servidores de Federação em cada organização parceira. **Observação:** Para o design de SSO da Web federado, você precisa de pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso.|![sso da web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso da web federado](media/icon_checkboxo.gif)|\(Opcional\) determinar se sua organização precisa de um proxy do servidor de Federação. Se seu plano de design exigir um proxy, você pode instalar um ou mais proxies de servidor de Federação em cada organização parceira.|![sso da web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso da web federado](media/icon_checkboxo.gif)|De acordo com seu plano de design, compartilhe certificados, configure clientes e configure as relações de confiança nas duas organizações parceiras para que elas possam se comunicar por meio de confiança de federação.|![sso da web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![sso da web federado](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando a organização do parceiro de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso da web federado](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, declarações\-habilitar seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo do Microsoft® Office SharePoint® Server usando o WIF e o SDK do WIF.|![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![sso da web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
