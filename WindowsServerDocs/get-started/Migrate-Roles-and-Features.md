---
title: Migrar funções e recursos
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
ms.openlocfilehash: 486c11ebd46c6fd23b3bd16cd90463f8d607287e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443539"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrando funções e recursos no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta página contém links para informações e ferramentas que o ajudam durante o processo de migração de funções e recursos do Windows Server 2012, do Windows Server 2012 R2 e do Windows Server 2016. Muitas funções e recursos podem ser migrados usando as Ferramentas de Migração do Windows Server, um conjunto de cinco cmdlets do Windows PowerShell que foi introduzido no Windows Server 2008 R2 para facilitar a migração de dados e elementos de recursos e funções.

Os guias de migração dão suporte para migrações de funções e recursos especificados de um servidor para outro (não atualizações in-loco). Salvo indicação em contrário nos guias, as migrações são compatíveis entre computadores físicos e virtuais e entre as opções de instalação completa do Windows Server e os servidores que executam a opção de instalação Server Core.  

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a migrar funções e recursos, verifique se os servidores de origem e destino estão executando os service packs mais recentes disponíveis para seus sistemas operacionais.
Já estão disponíveis um livro eletrônico do Windows Server 2012 R2 e os guias de migração do Windows Server 2012. Para saber mais e baixar o livro eletrônico, confira a [Galeria de livros eletrônicos para Microsoft Technologies](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles). 

>[!NOTE]
>Sempre que você migrar ou atualizar para qualquer versão do Windows Server, deve consultar e entender a [política de ciclo de vida de suporte](https://support.microsoft.com/lifecycle) e o período para essa versão, e planejar corretamente. Você pode [procurar as informações sobre o ciclo de vida](https://support.microsoft.com/lifecycle) para a versão específica do Windows Server em que está interessado.
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>Guias de Migração
Os guias de migração atualizados para o Windows Server 2016 estão em desenvolvimento. Volte a esse local para conferir as atualizações quando elas forem disponibilizadas. Em muitos casos, as etapas nos guias de migração do Windows Server 2012 R2 ainda são relevantes para o Windows Server 2016.

- [Serviços de área de trabalho remota](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Servidor Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>Guias de Migração
Siga as etapas nestes guias para migrar as funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2, o Windows Server 2012 ou o Windows Server 2012 R2 para o Windows Server 2012 R2. As Ferramentas de Migração do Windows Server no Windows Server 2012 R2 permitem migrações entre sub-redes.

- [Instalar, usar e remover as Ferramentas de Migração do Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guia de Migração de Serviços de Certificados do Active Directory para Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migração do Serviço da Função Serviços de Federação do Active Directory para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guia de atualização e migração do Active Directory Rights Management Services](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrar os Serviços de Arquivo e Armazenamento para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migrar o Hyper-V para o Windows Server 2012 R2 no Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrar o Servidor de Políticas de Rede para o Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar os Serviços de Área de Trabalho Remota para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migrar o Windows Server Update Services para o Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrar Funções de Cluster para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrar o Servidor DHCP para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>Guias de Migração
Siga as etapas nestes guias para migrar as funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2 ou o Windows Server 2012 para o Windows Server 2012. As Ferramentas de Migração do Windows Server no Windows Server 2012 permitem migrações entre sub-redes.

- [Instalar, usar e remover as Ferramentas de Migração do Windows Server](https://technet.microsoft.com/library/jj134202)
- [Migrar os Serviços de Função dos Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Migrar a Autoridade de Registro de Integridade para o Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Migrar o Hyper-V para o Windows Server 2012 no Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migrar a Configuração IP para o Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Migrar o Servidor de Políticas de Rede para o Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar os Serviços de Impressão e Documentos para o Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Migrar o Acesso Remoto para o Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Migrar o Windows Server Update Services para o Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Atualizar os Controladores de Domínio do Active Directory para o Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migrar Serviços e Aplicativos Clusterizados para o Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Para saber mais sobre outros recursos de migração, visite [Migrar funções e recursos do Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>Guias de Migração
Siga as etapas nestes guias para migrar as funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2 para o Windows Server 2008 R2. As Ferramentas de Migração do Windows Server no Windows Server 2008 R2 não permitem migrações entre sub-redes.

- [Instalação, acesso e remoção das Ferramentas de Migração do Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guia de Migração de Serviços de Certificados do Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guia de Migração de Servidor do Sistema de Nomes de Domínio (DNS) e de Active Directory Domain Services](https://technet.microsoft.com/library/dd379558)
- [Guia de Migração do BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guia de Migração de Servidor DHCP (Dynamic Host Configuration Protocol)](https://technet.microsoft.com/library/dd379535)
- [Guia de Migração de Serviços de Arquivo](https://technet.microsoft.com/library/dd379487)
- [Guia de Migração de HRA](https://technet.microsoft.com/library/ee791829)
- [Guia de Migração do Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guia de Migração da Configuração de IP](https://technet.microsoft.com/library/dd379537)
- [Guia de Migração de Usuários e Grupos Locais](https://technet.microsoft.com/library/dd379531)
- [Guia de Migração do NPS](https://technet.microsoft.com/library/ee791849)
- [Guia de Migração de Serviços de Impressão](https://technet.microsoft.com/library/dd379488)
- [Guia de Migração dos Serviços de Área de Trabalho Remota](https://technet.microsoft.com/library/ff849223)
- [Guia de Migração de RRAS](https://technet.microsoft.com/library/ee822825)
- [Informações e Tarefas Comuns de Migração do Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guia de Migração do Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)
 
Para saber mais sobre outros recursos de migração, visite [Migrar funções e recursos do Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353).
