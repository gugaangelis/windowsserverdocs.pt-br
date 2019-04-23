---
title: Serviço de hospedagem da área de trabalho
description: Descreve os componentes de um serviço de hospedagem de área de trabalho.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: adbb9fd69bc61d2e877cadb0484a4e42093f262a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840467"
---
# <a name="desktop-hosting-service"></a>Serviço de hospedagem da área de trabalho

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este artigo mostrará a você mais sobre a área de trabalho que hospeda componentes do serviço.

## <a name="tenant-environment"></a>Ambiente de locatário

Conforme descrito em [funções de serviço de área de trabalho remota](rds-roles.md), cada função desempenha um papel distinto no ambiente de locatário.

O serviço de hospedagem da área de trabalho do provedor é implementado como um conjunto de ambientes de locatários isoladas. Ambiente de cada locatário consiste em um contêiner de armazenamento, um conjunto de máquinas virtuais e uma combinação de serviços do Azure, todos os comunicando-se ao longo de uma rede virtual isolada. Cada máquina virtual contém um ou mais dos componentes que compõem o ambiente de área de trabalho hospedado do locatário. As subseções a seguir descrevem os componentes que compõem o ambiente de área de trabalho hospedado de cada locatário.

## <a name="active-directory-domain-services"></a>Active Directory Domain Services

Os serviços de domínio do Active Directory (AD DS) fornece as informações de domínio e floresta, de modo que os usuários do locatário podem entrar para desktops e aplicativos para executar suas cargas de trabalho. Isso também permite que você configurar ou se conectar a compartilhamentos de arquivos necessários e bancos de dados que podem ser necessários para aplicativos do Windows.

Floresta do locatário não exige qualquer relação de confiança com a floresta de gerenciamento do provedor. Uma conta de administrador de domínio pode ser configurada para permitir que equipe de suporte técnico do provedor para executar tarefas administrativas no ambiente do locatário (como monitorar o status do sistema e aplicação de atualizações de software) e para ajudá-lo no domínio do locatário solução de problemas e configuração.

Há várias maneiras de implantar o AD DS:

1. Habilite serviços de domínio do Active Directory do Azure no ambiente de rede virtual do locatário. Isso criará uma instância gerenciada do AD DS para o locatário com base em como os usuários e grupos que existem no AD do Azure.
2. Configure um servidor autônomo do AD DS no ambiente de rede virtual do locatário. Isso fornece todo o controle completo da instância do AD DS em execução em máquinas virtuais.
3. Crie uma conexão de VPN site a site para um servidor do AD DS localizado em instalações do locatário. Isso permite que o locatário para se conectar a sua instância de AD DS existente e reduzir a duplicação de usuários, grupos, unidades organizacionais e assim por diante.

Para obter mais informações, consulte os seguintes artigos:

* [Documentação de serviços de domínio do Active Directory do Azure](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Guia de arquitetura de referência do Windows Server 2012 R2 de hospedagem de área de trabalho](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Criar uma conexão site a site no portal do Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>Banco de dados SQL

Um banco de dados altamente disponível do SQL é usado pelo agente de Conexão de área de trabalho remota para armazenar informações de implantação, como o mapeamento de conexões de usuários atuais para os servidores de host.

Há várias maneiras de implantar um banco de dados SQL:

1. Crie um banco de dados do SQL Azure no ambiente do locatário. Isso fornece a funcionalidade de um banco de dados redundante do SQL sem a necessidade de gerenciar os servidores propriamente ditos. Isso também permite que você pague pelo que consumir em vez de investir na infraestrutura.
2. Crie um cluster AlwaysOn do SQL Server. Isso permite que você aproveite a infraestrutura existente do SQL Server e lhe dá controle total sobre as instâncias do SQL Server.

Para obter mais informações sobre como configurar uma infraestrutura altamente disponível do banco de dados SQL, consulte os artigos a seguir:

* [O que é o serviço de banco de dados SQL?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Criação e configuração de grupos de disponibilidade (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Adicione o servidor do agente de Conexão de área de trabalho remota para a implantação e configurar a alta disponibilidade](rds-connection-broker-cluster.md).

## <a name="file-server"></a>Servidor de arquivos

O servidor de arquivos usa o protocolo SMB Server Message Block () 3.0 para fornecer a pastas compartilhadas. Essas pastas compartilhadas são usadas para criar e armazenar arquivos de disco de perfil do usuário (. vhdx) para fazer backup de dados e permite que usuários compartilhem dados entre si no serviço de nuvem do locatário.

A máquina virtual que implanta o servidor de arquivos deve ter um disco de dados do Azure conectada e configurada com pastas compartilhadas. Os discos de dados do Azure usam cache de gravação, garantindo que as gravações no disco não serão apagadas sempre que a máquina virtual é reiniciada.

Locatários pequenos podem reduzir os custos ao combinar o servidor de arquivos e [a função de licenciamento de área de trabalho remota](rds-roles.md#remote-desktop-licensing) em uma única máquina virtual no ambiente do locatário.

Para obter mais informações, consulte os seguintes artigos:

* [Armazenamento no Windows Server](../../storage/storage.md)
* [Como anexar um disco de dados gerenciado a uma VM Windows no portal do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Discos de perfil do usuário

Discos de perfil de usuário permitem que os usuários salvem arquivos e configurações pessoais quando estão conectados a uma sessão em um servidor de Host de sessão de área de trabalho remota em uma coleção, em seguida, acessar as mesmas configurações e arquivos ao entrar em um diferente [Host de sessão de área de trabalho remota](rds-roles.md#remote-desktop-session-host) servidor na coleção. Quando o usuário entra pela primeira vez, o servidor de arquivos do locatário cria um disco de perfil do usuário que obtém montado no servidor de Host de sessão de área de trabalho remota que o usuário está conectado no momento. Para cada subsequente entrar, o disco de perfil do usuário está montado para o servidor de host de sessão de área de trabalho remota apropriado e ele for desmontado com cada saída. Somente o usuário pode acessar o conteúdo do disco de perfil.