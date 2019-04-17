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
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121355"
---
# Migrando funções e recursos no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta página contém links para informações e ferramentas que ajudam a orientar você durante o processo de migração de funções e recursos do Windows Server 2012, do Windows Server 2012 R2 e do Windows Server 2016. Muitas funções e recursos podem ser migrados usando as Ferramentas de Migração do Windows Server, um conjunto de cinco cmdlets do Windows PowerShell que foi introduzido no Windows Server 2008 R2 para facilitar a migração de elementos e dados de recursos e funções.

Os guias de migração oferecem suporte para migrações de funções e recursos especificados de um servidor para outro (não upgrades locais). Salvo indicação em contrário nos guias, as migrações são compatíveis entre computadores físicos e virtuais e entre as opções de instalação completa do Windows Server e os servidores que executam a opção de instalação Server Core.  

## Antes de começar

Antes de começar a migrar funções e recursos, verifique se os servidores de origem e de destino estão executando os service packs mais recentes que estão disponíveis para seus sistemas operacionais.
Um e-book do Windows Server 2012 R2 e guias de migração do Windows Server 2012 já estão disponíveis. Para obter mais informações e fazer download do e-book, consulte a [Galeria de livros eletrônicos para Microsoft Technologies](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles). 

>[!NOTE]
>Sempre que você migra ou atualiza para qualquer versão do Windows Server, deve consultar e entender a [política de ciclo de vida de suporte](https://support.microsoft.com/lifecycle) e o período para essa versão e planejar corretamente. Você pode [procurar as informações de ciclo de vida](https://support.microsoft.com/lifecycle) para a versão específica do Windows Server em que está interessado.
 
## Windows Server 2016

### Guias de Migração
Guias de migração atualizados para Windows Server 2016 estão em desenvolvimento. Volte a esse local para conferir as atualizações quando elas forem disponibilizadas. Em muitos casos, as etapas nos guias de migração do Windows Server 2012 R2 ainda são relevantes para o Windows Server 2016.

- [Serviços da área de trabalho Remota](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Servidor Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## Windows Server 2012 R2

### Guias de Migração
Siga as etapas nestes guias para migrar funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2, o Windows Server 2012 ou o Windows Server 2012 R2 para o Windows Server 2012 R2. As Ferramentas de Migração do Windows Server no Windows Server 2012 R2 permitem migrações de sub-rede cruzada.

- [Instalar, usar e remover as ferramentas de migração do Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guia de Migração de Serviços de Certificados do ActiveDirectory para WindowsServer2012R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migração do Serviço da Função Serviços de Federação do Active Directory para o WindowsServer2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guia de upgrade e migração de Active Directory Rights Management Services](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrar serviços de arquivo e armazenamento para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migrar o Hyper-V para o Windows Server 2012 R2 por meio do Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrar o Servidor de Políticas de Rede para o Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar os Serviços de Área de Trabalho Remota para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migrar o Windows Server Update Services para o Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrar Funções de Cluster para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrar o DHCP Server para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## Windows Server 2012

### Guias de Migração
Siga as etapas nestes guias para migrar funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2 ou o Windows Server 2012 para o Windows Server 2012. As Ferramentas de Migração do Windows Server no Windows Server 2012 permitem migrações de sub-rede cruzada.

- [Instalar, usar e remover as ferramentas de migração do Windows Server](https://technet.microsoft.com/library/jj134202)
- [Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Migrar a Autoridade de Registro de Integridade para o Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Migrar o Hyper-V para o Windows Server 2012 por meio do Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migrar a Configuração IP para o Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Migrar o Servidor de Políticas de Rede para o Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrar Serviços de Impressão e Documentos para o Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Migrar o Acesso Remoto para o Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Migrar o Windows Server Update Services para o Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Fazer upgrade de controladores de domínio do Active Directory para Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migração de Serviços e Aplicativos Clusterizados para o Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Para obter recursos de migração adicionais, visite [Migrar funções e recursos do Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## Windows Server 2008 R2

### Guias de Migração
Siga as etapas nestes guias para migrar funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2 para o Windows Server 2008 R2. As Ferramentas de Migração do Windows Server no Windows Server 2008 R2 não permitem migrações de sub-rede cruzada.

- [Instalação, acesso e remoção das Ferramentas de Migração do Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guia de Migração de Serviços de Certificados do Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guia de migração de Serviços de Domínio do Active Directory e Servidor do Sistema de Nomes de Domínio (DNS)](https://technet.microsoft.com/library/dd379558)
- [Guia de migração do BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guia de migração de Servidor DHCP](https://technet.microsoft.com/library/dd379535)
- [Guia de Migração de Serviços de Arquivo](https://technet.microsoft.com/library/dd379487)
- [Guia de Migração de HRA](https://technet.microsoft.com/library/ee791829)
- [Guia de migração do Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guia de Migração da Configuração de IP](https://technet.microsoft.com/library/dd379537)
- [Guia de Migração de Usuários e Grupos Locais](https://technet.microsoft.com/library/dd379531)
- [Guia de Migração do NPS](https://technet.microsoft.com/library/ee791849)
- [Guia de migração de Serviços de Impressão](https://technet.microsoft.com/library/dd379488)
- [Guia de Migração dos Serviços de Área de Trabalho Remota](https://technet.microsoft.com/library/ff849223)
- [Guia de Migração de RRAS](https://technet.microsoft.com/library/ee822825)
- [Informações e tarefas comuns de migração do Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guia de Migração do Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)
 
Para obter recursos de migração adicionais, visite [Migrar funções e recursos do Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353).
