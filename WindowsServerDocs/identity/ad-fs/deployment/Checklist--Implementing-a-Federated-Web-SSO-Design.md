---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: Lista de verificação-implementando um design de SSO da Web federado
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e3c9f94ee1ce1e1fef2429a12fdb77604987fdb8
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520035"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de verificação: implementando um design de SSO da Web federado

Esta lista de verificação pai inclui \- links de referência cruzada para conceitos importantes sobre o design de SSO de logon único da Web federada \- \- \( \) para serviços de Federação do Active Directory (AD FS) \( AD FS \) . Ela também contém links para listas de verificação subordinadas que ajudarão a concluir as tarefas exigidas para implementar esse design.

> [!NOTE]
> Execute as tarefas desta lista de verificação na ordem indicada. Quando houver um link de referência para um tópico conceitual ou uma lista de verificação subordinada, retorne a este tópico depois de revisar aquele ou concluir as tarefas da lista de verificação subordinada para poder executar as tarefas restantes desta lista de verificação.

![](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação de SSO Web federado: Implementando um design de SSO Web federado**

|Tarefa|Referência|
|--------|-------------|
|Examine conceitos importantes sobre o design de SSO da Web federado e determine quais AD FS objetivos de implantação você pode usar para personalizar esse design para atender às necessidades da sua organização.|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[design de SSO Web federado](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11)) de SSO da Web federado<p>![SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando suas metas de implantação de AD FS](../design/identifying-your-ad-fs-deployment-goals.md)<p>![SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejando sua implantação](../design/planning-your-deployment.md)|
|Examine os requisitos de hardware, software, certificado, DNS do sistema de nome de domínio \( \) , repositório de atributos e cliente para a implantação de AD FS em sua organização.|![Apêndice A de SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[A: revisando requisitos de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11))|
|Examine conceitos importantes sobre declarações, regras de declaração, repositórios de atributos e o banco de dados de configuração de AD FS antes de implantar AD FS em ambas as organizações parceiras.|![SSO Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[entendendo os principais conceitos de AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|
|De acordo com seu plano de design, instale um ou mais servidores de Federação em cada organização parceira. **Observação:** Para o design de SSO da Web federado, você precisa de pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO Web federado: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|
|\(Opcional \) determine se sua organização precisa ou não de um proxy de servidor de Federação. Se o plano de design chamar um proxy, você poderá instalar um ou mais proxies de servidor de Federação em cada organização parceira.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO Web federado: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|De acordo com seu plano de design, compartilhe certificados, configure clientes e configure as relações de confiança nas duas organizações parceiras para que elas possam se comunicar por meio de confiança de federação.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO da Web federada: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação de SSO da Web federada: Configurando a organização do parceiro de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|
|Se você for um administrador na organização do parceiro de recurso, \- as declarações habilitam seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo do Microsoft &reg; Office SharePoint &reg; Server usando o WIF e o SDK do WIF.|![SSO da Web federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<p>![SDK Web federado do](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)|
