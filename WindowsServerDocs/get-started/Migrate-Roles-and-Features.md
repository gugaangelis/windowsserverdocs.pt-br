---
title: Migrar funções e recursos
description: Informações sobre como migrar funções e recursos para uma versão mais recente do Windows Server.
ms.prod: windows-server
ms.date: 08/28/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 99a684cc90d47e1e80dc84ef9c3705a2ed79728b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182022"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrando funções e recursos no Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta página contém links para informações e ferramentas que o ajudam durante o processo de migração de funções e recursos para uma nova versão do Windows Server. Você pode migrar servidores de arquivos e armazenamento usando o [Serviço de Migração de Armazenamento](../storage/storage-migration-service/overview.md), enquanto muitas outras funções e recursos podem ser migrados usando as Ferramentas de Migração do Windows Server, um conjunto de cmdlets do PowerShell que foram introduzidos no Windows Server 2008 R2 para migração de funções e recursos.

Os guias de migração dão suporte para migrações de funções e recursos especificados de um servidor para outro (não atualizações in-loco). Salvo indicação em contrário nos guias, as migrações são compatíveis entre computadores físicos e virtuais e entre as opções de instalação completa do Windows Server e os servidores que executam a opção de instalação Server Core.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a migrar funções e recursos, verifique se os servidores de origem e destino estão executando os service packs mais recentes disponíveis para seus sistemas operacionais.

> [!NOTE]
> Sempre que você migrar ou atualizar para qualquer versão do Windows Server, deve consultar e entender a [política de ciclo de vida de suporte](https://support.microsoft.com/lifecycle) e o período para essa versão, e planejar corretamente. Você pode [procurar as informações sobre o ciclo de vida](https://support.microsoft.com/lifecycle) para a versão específica do Windows Server em que está interessado.

## <a name="windows-server-2019"></a>Windows Server 2019

Para migrar servidores de arquivos e armazenamento para o Windows Server 2019 ou o Windows Server 2016, é recomendável usar o [Serviço de Migração de Armazenamento](../storage/storage-migration-service/overview.md). Para migrar outras funções, consulte as diretrizes para o Windows Server 2016 e o Windows Server 2012 R2.

## <a name="windows-server-2016"></a>Windows Server 2016

Aqui estão os guias de migração para Windows Server 2016. Observe que, em muitos casos, você também pode usar os guias de migração do Windows Server 2012 R2.

- [Serviços de área de trabalho remota](../remote/remote-desktop-services/migrate-rds-role-services.md)
- [Servidor Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [MultiPoint Services](../remote/multipoint-services/multipoint-services-migrate.md)

Para migrar servidores de arquivos para o Windows Server 2019 ou o Windows Server 2016, é recomendável usar o [Serviço de Migração de Armazenamento](../storage/storage-migration-service/overview.md).

## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

Siga as etapas nestes guias para migrar as funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2, o Windows Server 2012 ou o Windows Server 2012 R2 para o Windows Server 2012 R2. As Ferramentas de Migração do Windows Server no Windows Server 2012 R2 permitem migrações entre sub-redes.

- [Instalar, usar e remover as Ferramentas de Migração do Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134202(v=ws.11))
- [Guia de Migração de Serviços de Certificados do Active Directory para Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486797(v=ws.11))
- [Migração do Serviço da Função Serviços de Federação do Active Directory para o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486815(v=ws.11))
- [Guia de atualização e migração do Active Directory Rights Management Services](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754277(v=ws.10))
- [Migrar os Serviços de Arquivo e Armazenamento para o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn479292(v=ws.11))
- [Migrar o Hyper-V para o Windows Server 2012 R2 no Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486799(v=ws.11))
- [Migrar o Servidor de Políticas de Rede para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831652(v=ws.11))
- [Migrar os Serviços de Área de Trabalho Remota para o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn479239(v=ws.11))
- [Migrar o Windows Server Update Services para o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [Migrar Funções de Cluster para o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn530779(v=ws.11))
- [Migrar o Servidor DHCP para o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn495425(v=ws.11))

Já estão disponíveis um livro eletrônico do Windows Server 2012 R2 e os guias de migração do Windows Server 2012. Para saber mais e baixar o livro eletrônico, confira a [Galeria de livros eletrônicos para Microsoft Technologies](https://download.microsoft.com/download/8/D/3/8D33661A-7E21-4FEE-9AAA-C17C3004B5AA/Windows-Migration-and-Upgrade-Guide.pdf).

## <a name="windows-server-2012"></a>Windows Server 2012

Siga as etapas nestes guias para migrar as funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008, o Windows Server 2008 R2 ou o Windows Server 2012 para o Windows Server 2012. As Ferramentas de Migração do Windows Server no Windows Server 2012 permitem migrações entre sub-redes.

- [Instalar, usar e remover as Ferramentas de Migração do Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134202(v=ws.11))
- [Migrar os Serviços de Função dos Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012](../identity/ad-fs/deployment/migrate-ad-fs-role-services-to-windows-server-2012.md)
- [Migrar a Autoridade de Registro de Integridade para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831513(v=ws.11))
- [Migrar o Hyper-V para o Windows Server 2012 no Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574113(v=ws.11))
- [Migrar a Configuração IP para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574133(v=ws.11))
- [Migrar o Servidor de Políticas de Rede para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831652(v=ws.11))
- [Migrar os Serviços de Impressão e Documentos para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134150(v=ws.11))
- [Migrar o Acesso Remoto para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831423(v=ws.11))
- [Migrar o Windows Server Update Services para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [Atualizar os Controladores de Domínio do Active Directory para o Windows Server 2012](../identity/ad-ds/deploy/upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012.md)
- [Migrar Serviços e Aplicativos Clusterizados para o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486790(v=ws.11))


Para saber mais sobre outros recursos de migração, visite [Migrar funções e recursos do Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134039(v=ws.11)).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

Siga as etapas nestes guias para migrar as funções e recursos de servidores que executam o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2 para o Windows Server 2008 R2. As Ferramentas de Migração do Windows Server no Windows Server 2008 R2 não permitem migrações entre sub-redes.

- [Instalação, acesso e remoção das Ferramentas de Migração do Windows Server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379545(v=ws.10))
- [Guia de Migração de Serviços de Certificados do Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))
- [Guia de Migração de Servidor do Sistema de Nomes de Domínio (DNS) e de Active Directory Domain Services](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379558(v=ws.10))
- [Guia de Migração do BranchCache](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548365(v=ws.10))
- [Guia de Migração de Servidor DHCP (Dynamic Host Configuration Protocol)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379535(v=ws.10))
- [Guia de Migração de Serviços de Arquivo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379487(v=ws.10))
- [Guia de Migração de HRA](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee791829(v=ws.10))
- [Guia de Migração do Hyper-V](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee849855(v=ws.10))
- [Guia de Migração da Configuração de IP](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379537(v=ws.10))
- [Guia de Migração de Usuários e Grupos Locais](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379531(v=ws.10))
- [Guia de Migração do NPS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee791849(v=ws.10))
- [Guia de Migração de Serviços de Impressão](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379488(v=ws.10))
- [Guia de Migração dos Serviços de Área de Trabalho Remota](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff849223(v=ws.10))
- [Guia de Migração de RRAS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee822825(v=ws.10))
- [Informações e Tarefas Comuns de Migração do Windows Server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff400258(v=ws.10))
- [Guia de Migração do Windows Server Update Services 3.0 SP2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee822826(v=ws.10))

Para saber mais sobre outros recursos de migração, visite [Migrar funções e recursos do Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd365353(v=ws.10)).
