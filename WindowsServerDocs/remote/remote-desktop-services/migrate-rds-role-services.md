---
title: Migrar sua implantação de serviços de área de trabalho remota para Windows Server 2016
description: Este artigo descreve como migrar sua implantação de serviços de área de trabalho remota para novos servidores do Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 73beb1539420f4b4aad818ffe0b0bdaabe901748
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870257"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Migrar sua implantação de serviços de área de trabalho remota para Windows Server 2016

Se atualmente você estiver executando serviços de área de trabalho remota no Windows Server 2012 R2, você pode mover para o Windows Server 2016 para tirar proveito dos novos recursos, como suporte para SQL Azure e espaços de armazenamento diretos.

Há suporte para a migração para uma implantação de serviços de área de trabalho remota de servidores de origem executando o Windows Server 2016 para servidores de destino executando o Windows Server 2016. Em outras palavras, não há nenhuma migração in-loco direta do RDS no Windows Server 2012 R2 para o Windows Server 2016. Em vez disso, para a maioria dos componentes do RDS, você primeiro atualizar para o Windows Server 2016 e, em seguida, migrar dados e licenças. Os únicos componentes com uma migração direta são Web da área de trabalho remota, Gateway de área de trabalho remota e o servidor de licenciamento.

Para obter mais informações sobre os requisitos e o processo de atualização, consulte [Atualizando suas implantações dos serviços de área de trabalho remota para o Windows Server 2016](upgrade-to-rds-2016.md).

Use as seguintes etapas para migrar sua implantação de serviços de área de trabalho remota: 
- [Migrar servidores do agente de Conexão de área de trabalho remota](#migrate-rd-connection-broker-servers) 
- [Migre as coleções de sessão](#migrate-session-collections)
- [Migre as coleções de área de trabalho virtual](#migrate-virtual-desktop-collections)
- [Migrar servidores de acesso via Web RD](#migrate-rd-web-access-servers)
- [Migrar servidores de Gateway de área de trabalho remota](#migrate-rd-gateway-servers)
- [Migrar servidores de licenciamento de área de trabalho remota](#migrate-rd-licensing-servers)
- [Migrar certificados](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Migrar servidores do agente de Conexão de área de trabalho remota

Isso é a primeira e mais importante etapa para a migração: Migrando seus agentes de Conexão de área de trabalho remota para servidores de destino que executam o Windows Server 2016.
> [!IMPORTANT] 
> Os servidores de origem do agente de Conexão de área de trabalho remota (agente de Conexão de área de trabalho remota) devem ser configurados para alta disponibilidade suportar a migração. Para obter mais informações, consulte [implantar um cluster do agente de Conexão de área de trabalho remota](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md).

1. Se você tiver mais de um servidor do Agente de Conexão de Área de Trabalho na configuração de alta disponibilidade, remova todos os servidores do Agente de Conexão de Área de Trabalho, exceto o que estiver ativo no momento.
2. [Atualizar](upgrade-to-rds-2016.md) o servidor do agente de Conexão de área de trabalho restante na implantação para o Windows Server 2016.
3. Adicione servidores do agente de Conexão de área de trabalho do Windows Server 2016 à implantação de alta disponibilidade.

> [!NOTE] 
> Não há suporte para uma configuração de alta disponibilidade mista com o Windows Server 2016 e Windows Server 2012 R2 para servidores do agente de Conexão de área de trabalho remota. Um agente de Conexão de área de trabalho remota executando o Windows Server 2016 pode atender coleções de sessão com servidores de Host de sessão de área de trabalho remota executando o Windows Server 2012 R2, e pode atender coleções de área de trabalho virtuais com servidores de Host de virtualização de área de trabalho remota executando o Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Migrar coleções de sessão

Siga estas etapas para migrar uma coleção de sessão no Windows Server 2012 R2 para uma coleção de sessão no Windows Server 2016.
> [!IMPORTANT] 
> Migre as coleções de sessão somente após concluir com êxito a etapa anterior, [servidores do agente de Conexão de área de trabalho remota migrar](#Migrate-RD-Connection-Broker-servers).

1. [Atualize a coleção de sessão](Upgrade-to-RDSH-2016.md) do Windows Server 2012 R2 para o Windows Server 2016.
2. Adicione o novo servidor de Host de sessão de área de trabalho remota executando o Windows Server 2016 à coleção de sessão.
3. Saia de todas as sessões em servidores de Host de Sessão da Área de Trabalho Remota e remova os servidores que requerem migração da coleção de sessão. 
> [!NOTE]
> Se o modelo UVHD (UVHD-Template. vhdx) estiver habilitado na coleção de sessão e o servidor de arquivos tiver sido migrado para um novo servidor, atualize os discos de perfil do usuário: Propriedade de coleção de local com o novo caminho. Os Discos de Perfil de Usuário devem estar disponíveis no mesmo caminho relativo no novo local, assim como estavam no servidor de origem.
>
> Não há suporte para uma coleção de sessão de servidores de Host de sessão de área de trabalho remota com uma mistura de servidores que executam o Windows Server 2012 R2 e Windows Server 2016.

## <a name="migrate-virtual-desktop-collections"></a>Migrar coleções de áreas de trabalho virtuais

Siga estas etapas para migrar uma coleção de área de trabalho virtuais de um servidor de origem executando o Windows Server 2012 R2 para um servidor de destino executando o Windows Server 2016.

> [!IMPORTANT] 
> Migre as coleções de área de trabalho virtual somente após concluir com êxito a etapa anterior, [servidores do agente de Conexão de área de trabalho remota migrar](#Migrate-RD-Connection-Broker-servers).

1. [Atualize a coleção da área de trabalho virtuais](Upgrade-to-RDVH-2016.md) do servidor executando o Windows Server 2012 R2 para o Windows Server 2016.
2. Adicione os novos servidores de Host de virtualização de área de trabalho remota do Windows Server 2016 à coleção da área de trabalho virtual.
3. Migre todas as máquinas virtuais na coleção atual de áreas de trabalho virtuais que estão sendo executadas em servidores de Host de Virtualização de Área de Trabalho Remota para os novos servidores. 
4. Remova todos os servidores de Host de Virtualização de Área de Trabalho Remota que exigiam a migração da coleção de áreas de trabalho virtuais no servidor de origem.

> [!NOTE] 
> Se o modelo UVHD (UVHD-Template. vhdx) estiver habilitado na coleção de sessão e o servidor de arquivos tiver sido migrado para um novo servidor, atualize os discos de perfil do usuário: Propriedade de coleção de local com o novo caminho. Os Discos de Perfil de Usuário devem estar disponíveis no mesmo caminho relativo no novo local, assim como estavam no servidor de origem.
>
> Não há suporte para uma coleção de área de trabalho virtuais de servidores de Host de virtualização de área de trabalho remota com uma mistura de servidores que executam o Windows Server 2012 R2 e Windows Server 2016.

## <a name="migrate-rdweb-access-servers"></a>Migrar servidores de Acesso via Web da Área de Trabalho Remota
Siga estas etapas para migrar servidores de acesso via Web RD:
- Ingressar nos servidores de destino executando o Windows Server 2016 para a implantação dos serviços de área de trabalho remota e instalar a função da Web da área de trabalho remota
- Use [ferramenta de implantação da Web IIS](https://www.iis.net/) para migrar as configurações de site da Web da área de trabalho remota de servidores de acesso via Web RD atuais para os servidores de destino executando o Windows Server 2016.
- [Migrar certificados](#Migrate-certificates) para os servidores de destino executando o Windows Server 2016.
- Remova os servidores de origem da implantação dos serviços de área de trabalho remota  

## <a name="migrate-rdgateway-servers"></a>Migrar servidores de Gateway de Área de Trabalho Remota
Siga estas etapas para migrar servidores de Gateway de área de trabalho remota:
- Ingressar nos servidores de destino executando o Windows Server 2016 para a implantação dos serviços de área de trabalho remota e instalar a função de Gateway de área de trabalho remota
- Use [ferramenta de implantação da Web IIS](https://www.iis.net/) para migrar as configurações de ponto de extremidade de Gateway de área de trabalho remota de servidores do Gateway de área de trabalho remota do atuais para os servidores de destino executando o Windows Server 2016.
- [Migrar certificados](#Migrate-certificates) para os servidores de destino executando o Windows Server 2016.
- Remova os servidores de origem da implantação de serviços de área de trabalho remota  

## <a name="migrate-rdlicensing-servers"></a>Migrar servidores de Licenciamento de Área de Trabalho Remota

Siga estas etapas para migrar um servidor de licenciamento de área de trabalho remota de um servidor de origem executando o Windows Server 2012 ou Windows Server 2012 R2 para um servidor de destino executando o Windows Server 2016.

1. [Migre as licenças de acesso de cliente dos serviços de área de trabalho remota (RDS CALs)](migrate-rds-cals.md) do servidor de origem para o servidor de destino.
2. Editar o **propriedades de implantação** na **Gerenciador do servidor** no servidor de gerenciamento de área de trabalho remota (que normalmente está sendo executado no primeiro servidor do agente de Conexão de área de trabalho) para incluir somente o licenciamento de área de trabalho remota nova servidores que executam o Windows Server 2016.
3. Desative o servidor de licenciamento de área de trabalho remota de origem: Na **o Gerenciador de licenciamento de área de trabalho remota**, clique com botão direito no servidor adequado, passe o mouse sobre **avançado** para selecionar **desativar servidor**e, em seguida, siga as etapas no Assistente .
4. Remova os servidores de licenciamento de área de trabalho remota de origem da implantação no **Gerenciador do servidor** no servidor de gerenciamento de área de trabalho remota.

## <a name="migrate-certificates"></a>Migrar certificados

Migração de certificado bem-sucedida requer o processo real de migração de certificados e atualizar informações de certificado nas propriedades de implantação de serviços de área de trabalho remota.

Migração de certificados típico inclui as seguintes etapas:
- Exporte o certificado para um arquivo PFX com a chave privada.
- Importe o certificado de um arquivo PFX.

Depois de migrar os certificados apropriados, atualize os certificados necessários para a implantação de serviços de área de trabalho remota no Gerenciador do servidor ou o PowerShell a seguir: 
- Agente de Conexão de área de trabalho - sign-on único
- Agente de Conexão de área de trabalho - a publicação do arquivo RDP
- Gateway de área de trabalho remota - conexão HTTPS
- Acesso via Web RD - conexão HTTPS e a assinatura de conexão de RemoteApp/área de trabalho
