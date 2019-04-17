---
title: Compatibilidade de aplicativos do Windows Server 2019 e Microsoft Server
description: Tabela de compatibilidade para aplicativos de servidor Windows Server 2019 e da Microsoft
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2afe7c32-1fda-4441-989b-4115dabdcd34
author: coreyp
ms.author: coreyp-at-msft
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: e8bb8cda003ca151216e3b86d5884dfd05e73e6d
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256939"
---
# Compatibilidade de aplicativos do Windows Server 2019 e Microsoft Server

>Aplica-se a: Windows Server 2019

Esta tabela lista os aplicativos de servidor da Microsoft que suportam a funcionalidade e a instalação no Windows Server 2019. Essas informações são indicadas para referência rápida e não se destinam a substituir as especificações individuais de produtos, os requisitos, os anúncios ou as comunicações gerais de cada aplicativo de servidor específico. confira a documentação oficial de cada produto para compreender totalmente a compatibilidade e as opções.

Se você for um parceiro do fornecedor de software olhando para obter mais informações sobre a compatibilidade do Windows Server com aplicativos não-Microsoft, visite o [portal de certificação de aplicativo comercial](https://commercialappcertification.microsoft.com/).
| **Produto**                                                  | **Com suporte no Server Core**             |   | **Compatível com o servidor com experiência Desktop** | **Lançado?** |   | **Link do produto da Web**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Sim                                      |   | Sim                                             | Sim           |   | [Requisitos de sistema do Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Sim                                      |   | Sim                                             | Não            |   | [Requisitos do sistema host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Yes\ *                                    |   | Sim                                             | Sim           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Yes\ *                                    |   | Sim                                             | Sim           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Yes\ *                                    |   | Sim                                             | Sim           |   | [Requisitos de hardware e software para instalação do SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Yes\ *                                    |   | Sim                                             | Sim           |   | [Requisitos de hardware e Software para instalação do SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Yes\ *                                    |   | Sim                                             | Sim           |   | [Requisitos de hardware e Software para instalação do SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (versão 1806) | Sim como cliente gerenciado, não como o servidor do site |   | Sim como cliente gerenciado, não como o servidor do site        | Sim           |   | [Novidades na versão 1806 do System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Yes\ *                                    |   | Sim                                             | Sim           |   | [Requisitos do sistema para o System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Yes\ *                                    |   | Sim                                             | Sim           |   | [Requisitos do sistema para o System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | Não                                       |   | Sim                                             | Sim           |   | [Preparando seu ambiente para o System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de hardware e software do SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de hardware e software para o SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de software do Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de software do Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype para empresas de 2019                                      | Não                                       |   | Sim                                             | Sim           |   | [Instalar os pré-requisitos do Skype for Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*May possuem limitações ou pode exigir a [o recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD). ](install-fod-19.md)
Consulte a documentação de FOD ou produto específico.
