---
title: Windows Server 2019 e a compatibilidade de aplicativos para Servidores da Microsoft
description: Tabela de compatibilidade do Windows Server 2019 e de aplicativos para Servidores da Microsoft
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 40c90b179321062949af5eea6d92f6238fb9b460
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391992"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Windows Server 2019 e a compatibilidade de aplicativos para Servidores da Microsoft

>Aplica-se a: Windows Server 2019

Esta tabela lista os aplicativos de servidor da Microsoft compatíveis com a funcionalidade e a instalação no Windows Server 2019. Essas informações são indicadas para referência rápida e não se destinam a substituir as especificações individuais de produtos, os requisitos, os anúncios ou as comunicações gerais de cada aplicativo de servidor específico. confira a documentação oficial de cada produto para compreender totalmente a compatibilidade e as opções.

Se você é um parceiro fornecedor de software que procura mais informações sobre a compatibilidade do Windows Server com aplicativos que não sejam da Microsoft, visite o [portal de Certificação de aplicativos comerciais](https://commercialappcertification.microsoft.com/).

| **Produto**                                                  | **Compatível com o Server Core**             |   | **Compatível com o Servidor com a Experiência Desktop** | **Lançado?** |   | **Link da Web do produto**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Sim                                      |   | Sim                                             | Sim           |   | [Requisitos de sistema do Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Sim                                      |   | Sim                                             | Sim            |   | [Requisitos de sistema do Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Sim\*                                    |   | Sim                                             | Sim           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Sim\*                                    |   | Sim                                             | Sim           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Sim\*                                    |   | Sim                                             | Sim           |   | [Requisitos de hardware e software para instalação do SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Sim\*                                    |   | Sim                                             | Sim           |   | [Requisitos de hardware e software para instalação do SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Sim\*                                    |   | Sim                                             | Sim           |   | [Requisitos de hardware e software para instalação do SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (versão 1806) | Sim, como cliente gerenciado, não como servidor do site |   | Sim, como cliente gerenciado, não como servidor do site        | Sim           |   | [Novidades da versão 1806 do System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Sim\*                                    |   | Sim                                             | Sim           |   | [Requisitos de sistema do System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Sim\*                                    |   | Sim                                             | Sim           |   | [Requisitos de sistema para o System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | Não                                       |   | Sim                                             | Sim           |   | [Preparando seu ambiente para o System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de hardware e software do SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de hardware e software do SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de software do Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | Não                                       |   | Sim                                             | Sim           |   | [Requisitos de software do Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype for Business 2019                                      | Não                                       |   | Sim                                             | Sim           |   | [Instalar os pré-requisitos para o Skype for Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*Podem ter limitações ou exigir o [FOD (recurso sob demanda) de compatibilidade de aplicativos do Server Core](install-fod-19.md).
Confira a documentação do FOD ou específica para o produto.
