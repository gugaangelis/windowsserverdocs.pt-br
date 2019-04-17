---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: "Lista de verificação - implementando um Design SSO da Web federado"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Lista de verificação: Implementando um Design SSO da Web federado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de verificação de pai inclui links cross\ referência aos conceitos importantes sobre o design federados da Web Single\-Sign\-On \(SSO\) para \(AD FS\) serviços de Federação do Active Directory. Ele também contém links para subordinadas listas de verificação que ajudarão você a concluir as tarefas que são necessárias para implementar esse design.  
  
> [!NOTE]  
> Conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um tópico conceitual ou uma lista de verificação subordinada, retorne para este tópico depois que você revise o tópico conceitual ou você concluir as tarefas na lista de verificação subordinada para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Federados da web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Implementando um Design de SSO federados da Web**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Sso da web federado](media/icon_checkboxo.gif)|Revise conceitos importantes sobre o design de SSO da Web federado e determine quais metas de implantação do AD FS você pode usar para personalizar esse design para atender às necessidades da sua organização.|![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[federados Design de SSO da Web](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identificando as metas de implantação do AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejar a implantação](https://technet.microsoft.com/library/dd807083.aspx)|  
|![Sso da web federado](media/icon_checkboxo.gif)|Examine o hardware, software, certificado, \(DNS\) Domain Name System, loja de atributo e requisitos do cliente para a implantação do AD FS em sua organização.|![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[apêndice a: examinando o AD FS requisitos](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Sso da web federado](media/icon_checkboxo.gif)|Revise os conceitos importantes sobre declarações, reivindicar regras, repositórios de atributo e o banco de dados de configuração do AD FS antes de implantar o AD FS em ambas as organizações parceiras.|![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conceitos-chave de Noções básicas sobre AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![Sso da web federado](media/icon_checkboxo.gif)|Acordo com seu plano de design, instale um ou mais servidores de Federação em cada organização do parceiro. **Observação:** para o design de SSO da Web federado, você precisa de servidor de Federação pelo menos um na organização do parceiro de conta e o servidor de Federação pelo menos um na organização do parceiro de recurso.|![Federados da web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: configuração de backup de um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Sso da web federado](media/icon_checkboxo.gif)|\(Optional\) determinar se sua organização precisa de um proxy de servidor de Federação. Se chama seu plano de design para um proxy, você pode instalar um ou mais proxies de servidor de Federação em cada organização do parceiro.|![Federados da web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: configuração de backup de Federação servidor Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Sso da web federado](media/icon_checkboxo.gif)|Acordo com seu plano de design, compartilhar certificados, configurar clientes e configurar as relações de confiança em ambas as organizações parceiras para que eles podem se comunicar por uma relação de confiança de Federação.|![Federados da web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![Federados da web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de verificação: Configurando a organização do parceiro de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![Sso da web federado](media/icon_checkboxo.gif)|Se você for um administrador na organização do parceiro de recurso, claims\ habilitar seu aplicativo de navegador da Web, aplicativo de serviço Web ou aplicativo do Microsoft® Office SharePoint® Server usando WIF e o WIF SDK.|![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Federados da web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
