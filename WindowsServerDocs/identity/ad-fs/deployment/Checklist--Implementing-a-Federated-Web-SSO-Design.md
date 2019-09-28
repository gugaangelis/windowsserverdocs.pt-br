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
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de verificação: implementando um design de SSO da Web Federado

Esta lista de verificação pai inclui links Cross no__t-0reference para conceitos importantes sobre o design da Web federado @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 para Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-6. Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando houver um link de referência para um tópico conceitual ou uma lista de verificação subordinada, retorne a este tópico depois de revisar aquele ou concluir as tarefas da lista de verificação subordinada para poder executar as tarefas restantes desta lista de verificação.  
  
SSO da Web ![federated @ no__t-1Checklist: Como implementar um design de SSO da Web federado**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Examine conceitos importantes sobre o design de SSO da Web federado e determine quais AD FS objetivos de implantação você pode usar para personalizar esse design para atender às necessidades da sua organização.|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[design de SSO Web federado](https://technet.microsoft.com/library/dd807050.aspx) de SSO da Web ![federated<br /><br />SSO da Web ![federated](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando suas metas de implantação de AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />SSO da Web ![federated](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejando sua implantação](https://technet.microsoft.com/library/dd807083.aspx)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Examine o hardware, o software, o certificado, o sistema de nomes de domínio \(DNS @ no__t-1, o repositório de atributos e os requisitos do cliente para implantar AD FS em sua organização.|SSO da Web ![federated @ no__t-1Appendix A: Como examinar os requisitos do AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Examine conceitos importantes sobre declarações, regras de declaração, repositórios de atributos e o banco de dados de configuração de AD FS antes de implantar AD FS em ambas as organizações parceiras.|SSO da Web ![federated](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[entendendo os principais conceitos AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|De acordo com seu plano de design, instale um ou mais servidores de Federação em cada organização parceira. **Observação:** Para o design de SSO da Web federado, você precisa de pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso.|SSO da Web ![federated @ no__t-1Checklist: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|\(Optional @ no__t-1 determina se sua organização precisa ou não de um proxy de servidor de Federação. Se o plano de design chamar um proxy, você poderá instalar um ou mais proxies de servidor de Federação em cada organização parceira.|SSO da Web ![federated @ no__t-1Checklist: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO da Web federado](media/icon_checkboxo.gif)|De acordo com seu plano de design, compartilhe certificados, configure clientes e configure as relações de confiança nas duas organizações parceiras para que elas possam se comunicar por meio de confiança de federação.|SSO da Web ![federated @ no__t-1Checklist: Configurando a organização do parceiro de conta @ no__t-0<br /><br />SSO da Web ![federated @ no__t-1Checklist: Configurando a organização do parceiro de recurso @ no__t-0|  
|![SSO da Web federado](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, o Claims @ no__t-0enable seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo Microsoft® Office SharePoint® Server usando o WIF e o SDK do WIF.|SSO da Web do ![federated](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />0federated-Web SSO do](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266) @no__t|  
