---
title: Migrando funções e recursos
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7469171005164d9ff823dad7de230d877c874dc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840877"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrando funções e recursos no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta página contém links para informações e ferramentas que ajudam a orientar você durante o processo de migração de funções e recursos do Windows Server 2012, do Windows Server 2012 R2 e do Windows Server 2016. Muitas funções e recursos podem ser migrados usando as Ferramentas de Migração do Windows Server, um conjunto de cinco cmdlets do Windows PowerShell que foi introduzido no Windows Server 2008 R2 para facilitar a migração de elementos e dados de recursos e funções.

Os guias de migração oferecem suporte para migrações de funções e recursos especificados de um servidor para outro (não upgrades locais). Salvo indicação em contrário nos guias, as migrações são compatíveis entre computadores físicos e virtuais e entre as opções de instalação completa do Windows Server e os servidores que executam a opção de instalação Server Core.  

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a migrar funções e recursos, verifique se os servidores de origem e de destino estão executando os service packs mais recentes que estão disponíveis para seus sistemas operacionais.
Um e-book do Windows Server 2012 R2 e guias de migração do Windows Server 2012 já estão disponíveis. Para obter mais informações e fazer download do e-book, consulte a [Galeria de livros eletrônicos para Microsoft Technologies](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles). 

>[!NOTE]
>Sempre que você migra ou atualiza para qualquer versão do Windows Server, deve consultar e entender a [política de ciclo de vida de suporte](https://support.microsoft.com/lifecycle) e o período para essa versão e planejar corretamente. Você pode [procurar as informações de ciclo de vida](https://support.microsoft.com/lifecycle) para a versão específica do Windows Server em que está interessado.
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>Guias de Migração
Guias de migração atualizados para Windows Server 2016 estão em desenvolvimento. Volte a esse local para conferir as atualizações quando elas forem disponibilizadas. Em muitos casos, as etapas nos guias de migração do Windows Server 2012 R2 ainda são relevantes para o Windows Server 2016.

- [Serviços de área de trabalho remota](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Servidor Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [Serviços do multiPoint](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>Guias de Migração
Siga as etapas nestes guias para migrar funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2, o Windows Server 2012 ou o Windows Server 2012 R2 para o Windows Server 2012 R2. As Ferramentas de Migração do Windows Server no Windows Server 2012 R2 permitem migrações de sub-rede cruzada.

- [Instalar, usar e remover ferramentas de migração do Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guia de migração de serviços de certificados do Active Directory para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migrando o serviço de função de serviços de Federação do Active Directory para Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guia de atualização e migração de serviços de gerenciamento de direitos do Active Directory](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrar Serviços de arquivo e armazenamento para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migrar o Hyper-V para o Windows Server 2012 R2 no Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrar o servidor de diretiva de rede para o Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar os serviços de área de trabalho remota para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migrar o Windows Server Update Services para o Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrar funções de Cluster para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrar o servidor DHCP para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>Guias de Migração
Siga as etapas nestes guias para migrar funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2 ou o Windows Server 2012 para o Windows Server 2012. As Ferramentas de Migração do Windows Server no Windows Server 2012 permitem migrações de sub-rede cruzada.

- [Instalar, usar e remover ferramentas de migração do Windows Server](https://technet.microsoft.com/library/jj134202)
- [Migrar Serviços de função de serviços de Federação do Active Directory para o Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Migrar autoridade de registro de integridade para o Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Migrar o Hyper-V para o Windows Server 2012 do Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migrar a configuração de IP para o Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Migrar o servidor de diretiva de rede para o Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migração de impressão e documentos de serviços para o Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Migrar acesso remoto para Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Migrar o Windows Server Update Services para o Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Atualizar controladores de domínio do Active Directory para o Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migrando serviços clusterizados e aplicativos para o Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Para obter recursos de migração adicionais, visite [Migrar funções e recursos do Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>Guias de Migração
Siga as etapas nestes guias para migrar funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2 para o Windows Server 2008 R2. As Ferramentas de Migração do Windows Server no Windows Server 2008 R2 não permitem migrações de sub-rede cruzada.

- [Instalação, acesso e remoção das ferramentas de migração do Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guia de migração de serviços de certificados do Active Directory](https://technet.microsoft.com/library/ee126170)
- [Serviços de domínio do Active Directory e o guia de migração de servidor do domínio nome DNS (sistema)](https://technet.microsoft.com/library/dd379558)
- [Guia de migração do BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guia de migração do servidor do Dynamic Host Configuration Protocol (DHCP)](https://technet.microsoft.com/library/dd379535)
- [Guia de migração de serviços de arquivo](https://technet.microsoft.com/library/dd379487)
- [Guia de migração de HRA](https://technet.microsoft.com/library/ee791829)
- [Guia de migração do Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guia de migração da configuração de IP](https://technet.microsoft.com/library/dd379537)
- [Guia de migração de grupo e usuário local](https://technet.microsoft.com/library/dd379531)
- [Guia de migração do NPS](https://technet.microsoft.com/library/ee791849)
- [Guia de migração de serviços de impressão](https://technet.microsoft.com/library/dd379488)
- [Guia de migração de serviços de área de trabalho remota](https://technet.microsoft.com/library/ff849223)
- [Guia de migração do RRAS](https://technet.microsoft.com/library/ee822825)
- [Informações e tarefas comuns de migração do Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guia de migração do Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)  
Para obter recursos de migração adicionais, visite [Migrar funções e recursos do Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353).
