---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: Lista de verificação-implementando um design de SSO da Web federado
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 165471fd06031a68343a54d019357afee782d082
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359952"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de verificação: implementando um design de SSO da Web federado

Esta lista de verificação pai inclui links de referência de\-cruzada para conceitos importantes sobre o\-de logon\-único da Web federado em \(design de\) de SSO para Serviços de Federação do Active Directory (AD FS) \(AD FS\). Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando houver um link de referência para um tópico conceitual ou uma lista de verificação subordinada, retorne a este tópico depois de revisar aquele ou concluir as tarefas da lista de verificação subordinada para poder executar as tarefas restantes desta lista de verificação.  
  
](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação de SSO Web federado ![: Implementando um design de SSO Web federado**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Examine conceitos importantes sobre o design de SSO da Web federado e determine quais AD FS objetivos de implantação você pode usar para personalizar esse design para atender às necessidades da sua organização.|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[design de SSO Web](https://technet.microsoft.com/library/dd807050.aspx) federado de SSO web federado ![<br /><br />![SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando suas metas de implantação de AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![SSO Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejando sua implantação](https://technet.microsoft.com/library/dd807083.aspx)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Examine os requisitos de hardware, software, certificado, sistema de nomes de domínio \(DNS\), repositório de atributos e cliente para implantar AD FS em sua organização.|![o apêndice de SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[A: revisando requisitos de AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Examine conceitos importantes sobre declarações, regras de declaração, repositórios de atributos e o banco de dados de configuração de AD FS antes de implantar AD FS em ambas as organizações parceiras.|![o SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[entendendo os principais conceitos de AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|De acordo com seu plano de design, instale um ou mais servidores de Federação em cada organização parceira. **Observação:** Para o design de SSO da Web federado, você precisa de pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO Web federado ![: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|\(opcional\) determinar se sua organização precisa ou não de um proxy de servidor de Federação. Se o plano de design chamar um proxy, você poderá instalar um ou mais proxies de servidor de Federação em cada organização parceira.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO Web federado ![: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|De acordo com seu plano de design, compartilhe certificados, configure clientes e configure as relações de confiança nas duas organizações parceiras para que elas possam se comunicar por meio de confiança de federação.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO Web federado ![: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO Web federado ![: Configurando a organização do parceiro de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, as declarações\-habilitar o aplicativo de navegador da Web, o aplicativo de serviço Web ou o aplicativo Microsoft® Office SharePoint® Server usando o WIF e o SDK do WIF.|![SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK do Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) para SSO da web federado ![|  
