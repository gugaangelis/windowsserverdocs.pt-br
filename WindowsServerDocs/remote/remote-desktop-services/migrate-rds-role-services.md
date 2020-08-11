---
title: Migrar sua implantação dos Serviços de Área de Trabalho Remota para o Windows Server 2016
description: Este artigo descreve como migrar sua implantação dos Serviços de Área de Trabalho Remota para os novos servidores do Windows Server 2016.
ms.author: chrimo
ms.date: 11/01/2016
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 62c2cc99277b3cf74f6bde5be59b69569c27a31b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961810"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Migrar sua implantação dos Serviços de Área de Trabalho Remota para o Windows Server 2016

Se atualmente você estiver executando Serviços de Área de Trabalho Remota no Windows Server 2012 R2, é possível mudar para o Windows Server 2016 para aproveitar os novos recursos, como o suporte para SQL Azure e os Espaços de Armazenamento Diretos.

Há suporte para uma implantação dos Serviços de Área de Trabalho Remota nos servidores de origem que executam o Windows Server 2016 ou nos servidores de destino que executam o Windows Server 2016. Em outras palavras, não há nenhuma migração in-loco direta do RDS no Windows Server 2012 R2 para o Windows Server 2016. Em vez disso, para a maioria dos componentes do RDS, primeiro é necessário atualizar para o Windows Server 2016 e, em seguida, migrar dados e licenças. Os únicos componentes com uma migração direta são Web da Área de Trabalho Remota, Gateway de Área de Trabalho Remota e o servidor de licenciamento.

Para obter mais informações sobre os requisitos e o processo de upgrade, consulte [Atualizar as implantações dos Serviços de Área de Trabalho Remota para o Windows Server 2016](./upgrade-to-rds.md).

Execute as etapas a seguir para migrar suas implantações de Serviços de Área de Trabalho Remota:

- [Migrar servidores do Agente de Conexão de Área de Trabalho Remota](#migrate-rdconnection-broker-servers)

- [Migrar coleções de sessão](#migrate-session-collections)

- [Migrar coleções de áreas de trabalho virtuais](#migrate-virtual-desktop-collections)

- [Migrar servidores de Acesso via Web da Área de Trabalho Remota](#migrate-rdweb-access-servers)

- [Migrar servidores de Gateway de Área de Trabalho Remota](#migrate-rdgateway-servers)

- [Migrar servidores de Licenciamento da Área de Trabalho Remota](#migrate-rdgateway-servers)

- [Migrar certificados](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Migrar servidores do Agente de Conexão de Área de Trabalho Remota

Essa é a primeira e mais importante etapa para a migração: migrar os Agentes de Conexão de Área de Trabalho Remota para servidores de destino que executam o Windows Server 2016.

> [!IMPORTANT]
> O servidor de destino do Agente de Conexão RD (Agente de Conexão de Área de Trabalho Remota) deve ser configurado para alta disponibilidade para dar suporte à migração. Para mais informações, consulte [Implantar um cluster do Agente de Conexão de Área de Trabalho Remota](./rds-connection-broker-cluster.md).

1. Se você tiver mais de um servidor do Agente de Conexão de Área de Trabalho na configuração de alta disponibilidade, remova todos os servidores do Agente de Conexão de Área de Trabalho, exceto o que estiver ativo no momento.

2. [Atualize](./upgrade-to-rds.md) o servidor restante do Agente de Conexão de Área de Trabalho Remota na implantação para o Windows Server 2016.

3. Adicione os servidores do Agente de Conexão de Área de Trabalho Remota do Windows Server 2016 à implantação de alta disponibilidade.

> [!NOTE]
> Não há suporte para uma configuração de alta disponibilidade mista com o Windows Server 2016 e o Windows Server 2012 R2 para os servidores do Agente de Conexão de Área de Trabalho Remota.
> Um Agente de Conexão de Área de Trabalho Remota executando o Windows Server 2016 pode atender coleções de sessão com servidores Host da Sessão de Área de Trabalho Remota executando o Windows Server 2012 R2 e pode atender coleções de áreas de trabalho virtuais com servidores Host de Virtualização de Área de Trabalho Remota executando o Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Migrar coleções de sessão

Siga estas etapas para migrar uma coleção de sessão no Windows Server 2012 R2 para uma coleção de sessão no Windows Server 2016.

> [!IMPORTANT]
> Migre as coleções de sessão somente após concluir com êxito a etapa anterior, [Migrar servidores do Agente de Conexão de Área de Trabalho Remota](#migrate-rdconnection-broker-servers).

1. [Atualize a coleção de sessão](./upgrade-to-rdsh.md) do Windows Server 2012 R2 para o Windows Server 2016.

2. Adicione o novo servidor Host da Sessão de Área de Trabalho Remota executando o Windows Server 2016 à coleção de sessão.

3. Saia de todas as sessões em servidores de Host de Sessão da Área de Trabalho Remota e remova os servidores que requerem migração da coleção de sessão.

   > [!NOTE]
   > Se o modelo UVHD (UVHD-template.vhdx) estiver habilitado na coleção de sessão e o servidor de arquivos tiver sido migrado para um novo servidor, atualize os Discos de perfil de usuário: Localize a propriedade de coleção com o novo caminho. Os Discos de Perfil de Usuário devem estar disponíveis no mesmo caminho relativo no novo local, assim como estavam no servidor de origem.
   >
   > Não há suporte para uma coleção de sessão de servidores Host da Sessão de Área de Trabalho Remota com uma mistura de servidores que executam o Windows Server 2012 R2 e o Windows Server 2016.

## <a name="migrate-virtual-desktop-collections"></a>Migrar coleções de áreas de trabalho virtuais

Siga estas etapas para migrar uma coleção de áreas de trabalho virtuais de um servidor de origem executando o Windows Server 2012 R2 para um servidor de destino executando o Windows Server 2016.

> [!IMPORTANT]
> Migre a coleção de áreas de trabalho virtuais somente após concluir com êxito a etapa anterior, [Migrar servidores do Agente de Área de Trabalho Remota](#migrate-rdconnection-broker-servers).

1. [Atualize a coleção de áreas de trabalho virtuais](./upgrade-to-rdvh.md) do servidor executando o Windows Server 2012 R2 para o Windows Server 2016.

2. Adicione os novos servidores Host de Virtualização de Área de Trabalho Remota do Windows Server 2016 à coleção de áreas de trabalho virtuais.

3. Migre todas as máquinas virtuais na coleção atual de áreas de trabalho virtuais que estão sendo executadas em servidores de Host de Virtualização de Área de Trabalho Remota para os novos servidores.

4. Remova todos os servidores de Host de Virtualização de Área de Trabalho Remota que exigiam a migração da coleção de áreas de trabalho virtuais no servidor de origem.

> [!NOTE]
> Se o modelo UVHD (UVHD-template.vhdx) estiver habilitado na coleção de sessão e o servidor de arquivos tiver sido migrado para um novo servidor, atualize os Discos de perfil de usuário: Localize a propriedade de coleção com o novo caminho. Os Discos de Perfil de Usuário devem estar disponíveis no mesmo caminho relativo no novo local, assim como estavam no servidor de origem.
>
> Não há suporte para uma coleção de áreas de trabalho virtuais de servidores Host de Virtualização de Área de Trabalho Remota com uma mistura de servidores que executam o Windows Server 2012 R2 e o Windows Server 2016.

## <a name="migrate-rdweb-access-servers"></a>Migrar servidores de Acesso via Web da Área de Trabalho Remota

Siga estas etapas para migrar servidores de Acesso via Web à Área de Trabalho Remota:

- Ingresse os servidores de destino executando o Windows Server 2016 para a implantação dos Serviços de Área de Trabalho Remota e instale a função do Acesso via Web à Área de Trabalho Remota

- Use [ferramenta de Implantação da Web IIS](https://www.iis.net/) para migrar as configurações de sites da Web da Web de Área de Trabalho Remota dos servidores de Acesso via Web à Área de Trabalho Remota para os servidores de destino executando o Windows Server 2016.

- [Migre certificados](#migrate-certificates) para os servidores de destino executando o Windows Server 2016.

- Remova os servidores de origem da implantação dos Serviços de Área de Trabalho Remota.

## <a name="migrate-rdgateway-servers"></a>Migrar servidores de Gateway de Área de Trabalho Remota

Siga estas etapas para migrar servidores de Gateway de Área de Trabalho Remota:

- Ingresse os servidores de destino executando o Windows Server 2016 para a implantação dos Serviços de Área de Trabalho Remota e instale a função do Gateway de Área de Trabalho Remota

- Use [ferramenta de Implantação da Web IIS](https://www.iis.net/) para migrar as configurações de ponto de extremidade do Gateway de Área de Trabalho Remota dos servidores de Gateway de Área de Trabalho Remota atuais para os servidores de destino executando o Windows Server 2016.

- [Migre certificados](#migrate-certificates) para os servidores de destino executando o Windows Server 2016.

- Remova os servidores de origem da implantação dos Serviços de Área de Trabalho Remota.

## <a name="migrate-rdlicensing-servers"></a>Migrar servidores de Licenciamento de Área de Trabalho Remota

Siga estas etapas para migrar um servidor de Licenciamento de Área de Trabalho Remota de um servidor de origem executando o Windows Server 2012 ou o Windows Server 2012 R2 para um servidor de destino executando o Windows Server 2016.

1. [Migre as RDS CALs (Licenças de Acesso para Cliente dos Serviços de Área de Trabalho Remota](migrate-rds-cals.md) do servidor de origem para o servidor de destino.

2. Edite as **Propriedades da Implantação** no **Gerenciador de Servidores** no servidor de gerenciamento de Área de Trabalho Remota (que normalmente está sendo executado no primeiro servidor do Agente de Conexão de Área de Trabalho Remota) para incluir somente os novos servidores de Licenciamento de Área de Trabalho Remota executando o Windows Server 2016.

3. Desative o servidor de Licenciamento de Área de Trabalho Remota de origem: No **Gerenciador de Licenciamento de Área de Trabalho Remota**, clique com botão direito no servidor apropriado, passe o mouse sobre **Avançado** para selecionar **Desativar servidor** e siga as etapas no assistente .

4. Remova os servidores de Licenciamento de Área de Trabalho Remota de origem da implantação no **Gerenciador de Servidores** no servidor de gerenciamento de Área de Trabalho Remota.

## <a name="migrate-certificates"></a>Migrar certificados

A migração de certificado bem-sucedida requer o processo real de migrar certificados e atualiza as informações de certificado nas Propriedades da Implantação dos Serviços de Área de Trabalho Remota.

A migração típica de certificados inclui as seguintes etapas:

- Exportar o certificado para um arquivo PFX com uma chave privada.

- Importar o certificado de um arquivo PFX.

Depois de migrar os certificados apropriados, atualize os certificados necessários para a implantação dos Serviços de Área de Trabalho Remota no Gerenciador de servidores ou PowerShell:

- Agente de Conexão de Área de Trabalho Remota - logon único

- Agente de Conexão de Área de Trabalho Remota - publicação do arquivo RDP

- Gateway de Área de Trabalho Remota - conexão HTTPS

- Acesso via Web à Área de Trabalho Remota - conexão HTTPS e assinatura de conexão de RemoteApp/área de trabalho
