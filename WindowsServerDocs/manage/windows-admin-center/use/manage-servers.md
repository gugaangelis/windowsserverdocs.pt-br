---
title: Gerenciar servidores com o centro de administração do Windows
description: Gerenciar servidores com o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c7f436ea9b2baa00294ccef52a5d7a27c7247e4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406787"
---
# <a name="manage-servers-with-windows-admin-center"></a>Gerenciar servidores com o centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Gerenciando computadores Windows Server

Você pode adicionar servidores individuais que executam o Windows Server 2012 ou posterior ao centro de administração do Windows para gerenciar o servidor com um conjunto abrangente de ferramentas, incluindo certificados, dispositivos, eventos, processos, funções e recursos, atualizações, máquinas virtuais e muito mais.

![Tela Visão geral da conexão do servidor](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Adicionando um servidor ao centro de administração do Windows

Para adicionar um servidor ao centro de administração do Windows:

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha Adicionar uma **conexão de servidor**.
3. Digite o nome do servidor e, se solicitado, as credenciais a serem usadas.
4. Clique em **Enviar** para concluir.

O servidor será adicionado à sua lista de conexões na página Visão geral. Clique para se conectar ao servidor.

> [!NOTE]
> Você também pode adicionar [clusters de failover](manage-failover-clusters.md) ou [clusters hiperconvergentes](manage-hyper-converged.md) como uma conexão separada no centro de administração do Windows.

## <a name="tools"></a>Ferramentas

As seguintes ferramentas estão disponíveis para conexões de servidor:

| Ferramenta | Descrição |
| ---- | ----------- |
| [Visão geral](#overview) | Exibir detalhes do servidor e controlar o estado do servidor |
| [Active Directory](#active-directory-preview) | Gerenciar Active Directory |
| [Backup](#backup) | Exibir e configurar o backup do Azure |  
| [Certificados](#certificates) | Exibir e modificar certificados |
| [Recipientes](#containers) | Exibir contêineres |
| [Dispositivos](#devices) | Exibir e modificar dispositivos |
| [DHCP](#dhcp) | Exibir e gerenciar a configuração do servidor DHCP |
| [DNS](#dns) | Exibir e gerenciar a configuração do servidor DNS |
| [Eventos](#events) | Exibir eventos |
| [Arquivos](#files) | Procurar arquivos e pastas |
| [Firewall](#firewall) | Exibir e modificar regras de firewall |
| [Aplicativos instalados](#installed-apps) | Exibir e remover aplicativos instalados |
| [Usuários e grupos locais](#local-users-and-groups) | Exibir e modificar usuários e grupos locais |
| [Network](#network) | Exibir e modificar dispositivos de rede |
| [PowerShell](#powershell) | Interagir com o servidor por meio do PowerShell |
| [Processar](#processes) | Exibir e modificar processos em execução |
| [Registry](#registry) | Exibir e modificar entradas do registro |
| [Área de Trabalho Remota](#remote-desktop) | Interagir com o servidor via Área de Trabalho Remota |
| [Funções e recursos](#roles-and-features) | Exibir e modificar funções e recursos |
| [Tarefas agendadas](#scheduled-tasks) | Exibir e modificar tarefas agendadas |
| [Serviços](#services) | Exibir e modificar serviços |
| [Configurações](#settings) | Exibir e modificar serviços |
| [Armazenamento](#storage) | Exibir e modificar dispositivos de armazenamento |
| [Serviço de migração de armazenamento](#storage-migration-service) | Migrar servidores e compartilhamentos de arquivos para o Azure ou o Windows Server 2019 |
| [Réplica de armazenamento](#storage-replica) | Usar a réplica de armazenamento para gerenciar a replicação de armazenamento de servidor para servidor |
| [Insights do sistema](#system-insights) | O System insights oferece maior insight sobre o funcionamento do seu servidor. |
| [Actualiza](#updates) | Exibir instalado e verificar se há novas atualizações |
| [Máquinas Virtuais](manage-virtual-machines.md) | Exibir e gerenciar máquinas virtuais |
| [Comutadores virtuais](#virtual-switches) | Exibir e gerenciar comutadores virtuais |

## <a name="overview"></a>Visão geral

A **visão geral** permite que você veja o estado atual da CPU, da memória e do desempenho da rede, bem como executar operações e modificar as configurações em um computador ou servidor de destino.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no Gerenciador do Servidor visão geral:

- Exibir detalhes do servidor
- Exibir atividade da CPU
- Exibir atividade de memória
- Exibir atividade de rede
- Reiniciar servidor
- Desligar servidor
- Habilitar métricas de disco no servidor
- Editar ID do computador no servidor
- Exiba o endereço IP do BMC com hiperlink (requer BMC compatível com IPMI).

[**Veja comentários e recursos propostos para visão geral do servidor**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="active-directory-preview"></a>Active Directory (versão prévia)

**Active Directory** é uma visualização antecipada que está disponível no [feed de extensão](../configure/using-extensions.md).

### <a name="features"></a>Recursos

O gerenciamento de Active Directory a seguir está disponível:

- Criar usuário
- Criar grupo
- Procurar por usuários, computadores e grupos
- Painel de detalhes para usuários, computadores e grupos quando selecionado em grade
- Ações de grade global usuários, computadores e grupos (desabilitar/habilitar, remover)
- Redefinir a senha do usuário
- Objetos de usuário: configurar as propriedades básicas & associações de grupo
- Objetos de computador: configurar a delegação para uma única máquina
- Objetos de Grupo: gerenciar associação (Adicionar/remover 1 usuário de cada vez)  

[**Exibir comentários e recursos propostos para Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## <a name="backup"></a>Backup

O **backup** permite proteger o Windows Server contra danos, ataques ou desastres fazendo backup de seu servidor diretamente no Microsoft Azure.
[Saiba mais sobre o backup do Azure.](https://aka.ms/windows-admin-center-backup)

[Fornecer comentários para backup no centro de administração do Windows](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no backup:

- Exibir uma visão geral do status do backup do Azure
- Configurar itens e agenda de backup
- Iniciar ou parar um trabalho de backup
- Exibir o histórico e o status do trabalho de backup
- Exibir pontos de recuperação e recuperar dados
- excluir dados de backup

## <a name="certificates"></a>Certificados

Os **certificados** permitem que você gerencie repositórios de certificados em um computador ou servidor.

### <a name="features"></a>Recursos

Os seguintes recursos têm suporte em certificados:

- Procurar e pesquisar certificados existentes
- Exibir detalhes do certificado
- Exportar certificados
- Renovar certificados
- Solicitar novos certificados
- Excluir certificados

[**Exibir comentários e recursos propostos para certificados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Contêineres

Os **contêineres** permitem exibir os contêineres em um host de contêiner do Windows Server. No caso de um contêiner do Windows Server Core em execução, você pode exibir os logs de eventos e acessar a CLI do contêiner.

[**Exibir comentários e recursos propostos para contêineres**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Dispositivos

Os **dispositivos** permitem que você gerencie dispositivos conectados em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em dispositivos:

- Procurar e Pesquisar dispositivos
- Exibir detalhes do dispositivo
- Desabilitar um dispositivo
- Atualizar o driver em um dispositivo

[**Exibir comentários e recursos propostos para dispositivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="dhcp"></a>DHCP

O **DHCP** permite que você gerencie dispositivos conectados em um computador ou servidor.

### <a name="features"></a>Recursos

- Criar/configurar/exibir escopos IPV4 e IPV6
- Criar exclusões de endereço e configurar o endereço IP inicial e final
- Criar reservas de endereço e configurar endereço MAC do cliente (IPV4), DUID e IAID (IPV6)

[**Exibir comentários e recursos propostos para DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## <a name="dns"></a>DNS

O **DNS** permite que você gerencie dispositivos conectados em um computador ou servidor.

### <a name="features"></a>Recursos

- Exibir detalhes de zonas de pesquisa direta do DNS, zonas de pesquisa inversa e registros DNS
- Criar zonas de pesquisa direta (primária, secundária ou stub) e configurar propriedades da zona de pesquisa direta
- Criar um tipo de host (A ou AAAA), CNAME ou MX de registros DNS
- Configurar propriedades de registros DNS
- Criar zonas de pesquisa inversa de IPV4 e IPV6 (primária, secundária e stub), configurar propriedades da zona de pesquisa inversa
- Criar PTR, tipo CNAME de registros DNS na zona de pesquisa inversa.

[**Exibir comentários e recursos propostos para DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## <a name="events"></a>Events

Os **eventos** permitem que você gerencie logs de eventos em um computador ou servidor.

### <a name="features"></a>Recursos

Os seguintes recursos têm suporte em eventos:

- Procurar e Pesquisar eventos
- Exibir detalhes do evento
- Limpar eventos do log
- Exportar eventos do log

[**Exibir comentários e recursos propostos para eventos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>Arquivos

**Os arquivos** permitem que você gerencie arquivos e pastas em um computador ou servidor.

### <a name="features"></a>Recursos

Os seguintes recursos têm suporte em arquivos:

- Procurar arquivos e pastas
- Pesquisar um arquivo ou uma pasta
- Criar uma nova pasta
- Excluir um arquivo ou pasta
- Baixar um arquivo ou pasta
- Carregar um arquivo ou uma pasta
- Renomear um arquivo ou pasta
- Extrair um arquivo zip
- Exibir Propriedades de arquivo ou pasta
- Adicionar, editar ou Remover compartilhamentos de arquivos
- Modificar permissões de usuário e grupo em compartilhamentos de arquivos

[**Exibir comentários e recursos propostos para arquivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Firewall

O **Firewall** permite que você gerencie as configurações e regras de firewall em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no firewall:

- Exibir uma visão geral das configurações de firewall
- Exibir regras de firewall de entrada
- Exibir regras de firewall de saída
- Pesquisar regras de firewall
- Exibir detalhes da regra de firewall
- Criar uma nova regra de firewall
- Habilitar ou desabilitar uma regra de firewall
- Excluir uma regra de firewall
- Editar as propriedades de uma regra de firewall

[**Exibir comentários e recursos propostos para o firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>Aplicativos instalados

Os **aplicativos instalados** permitem listar e desinstalar o aplicativo que está instalado.

[**Exibir comentários e recursos propostos para aplicativos instalados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Usuários e Grupos Locais

**Usuários e grupos locais** permitem gerenciar grupos de segurança e usuários que existem localmente em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em usuários e grupos locais:

- Exibir e Pesquisar usuários e grupos
- Criar um novo usuário ou grupo
- Gerenciar a associação de grupo de um usuário
- Excluir um usuário ou grupo
- Alterar a senha de um usuário
- Editar as propriedades de um usuário ou grupo

[**Exibir comentários e recursos propostos para usuários e grupos locais**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Rede

A **rede** permite que você gerencie dispositivos e configurações de rede em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte na rede:

- Procurar e Pesquisar adaptadores de rede existentes
- Exibir detalhes de um adaptador de rede
- Editar propriedades de um adaptador de rede
- Criar um [adaptador de rede do Azure (recurso de visualização)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Exibir comentários e recursos propostos para rede**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

O **PowerShell** permite que você interaja com um computador ou servidor por meio de uma sessão do PowerShell.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no PowerShell:

- Criar uma sessão interativa do PowerShell no servidor
- Desconectar da sessão do PowerShell no servidor

[**Exibir comentários e recursos propostos para o PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processos

Os **processos** permitem que você gerencie processos em execução em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em processos:

- Procurar e Pesquisar processos em execução
- Exibir detalhes do processo
- Iniciar um processo
- Encerrar um processo
- Criar um despejo de processo
- Localizar identificadores de processo

[**Exibir comentários e recursos propostos para processos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registro

O **registro** permite que você gerencie valores e chaves do registro em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no registro:

- Procurar valores e chaves do registro
- Adicionar ou modificar valores do registro
- Excluir valores do registro

[**Exibir comentários e recursos propostos para o registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Área de Trabalho Remota

**Área de trabalho remota** permite que você interaja com um computador ou servidor por meio de uma sessão de área de trabalho interativa.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no Área de Trabalho Remota:

- Iniciar uma sessão de área de trabalho remota interativa
- Desconectar-se de uma sessão de área de trabalho remota
- Enviar CTRL + Alt + Del para uma sessão de área de trabalho remota

[**Exibir comentários e recursos propostos para área de trabalho remota**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Funções e recursos

**Funções e recursos** permite que você gerencie funções e recursos em um servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em funções e recursos:

- Procurar lista de funções e recursos em um servidor
- Exibir detalhes da função ou do recurso
- Instalar uma função ou um recurso
- Remover uma função ou recurso

[**Exibir comentários e recursos propostos para funções e recursos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>TAREFAS AGENDADAS

**As tarefas agendadas** permitem que você gerencie tarefas agendadas em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte nas tarefas agendadas:

- Procurar a biblioteca do Agendador de tarefas
- Editar tarefas agendadas
- Habilitar & Desabilitar tarefas agendadas
- Iniciar & interromper as tarefas agendadas
- Criar tarefas agendadas

[**Exibir comentários e recursos propostos para tarefas agendadas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Serviços

Os **Serviços** permitem que você gerencie serviços em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte nos serviços do:

- Procurar e Pesquisar serviços em um servidor
- Exibir detalhes de um serviço
- Iniciar um serviço
- Pausar um serviço
- Editar as propriedades de um serviço

[**Exibir comentários e recursos propostos para serviços**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="settings"></a>Configurações

**Configurações** é um local central para gerenciar as configurações em um computador ou servidor.

### <a name="features"></a>Recursos

- Exibir e modificar variáveis de ambiente do usuário e do sistema
- Exibir a configuração de alertas de monitoramento do [Azure monitor](azure-monitor.md)
- Exibir e modificar a configuração de energia
- Exibir e modificar configurações de Área de Trabalho Remota
- Exibir e modificar as configurações de controle de acesso baseado em função
- Exibir e modificar as configurações de host do Hyper-V, se aplicável

## <a name="storage"></a>Armazenamento

O **armazenamento** permite que você gerencie dispositivos de armazenamento em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no armazenamento:

- Procurar e Pesquisar discos existentes em um servidor
- Exibir detalhes do disco
- Criar um volume
- Inicializar um disco
- Criar, anexar e desanexar um disco rígido virtual (VHD)
- Colocar um disco offline
- Formatar um volume
- Redimensionar um volume
- Editar propriedades do volume
- Excluir um volume
- Instalar gerenciamento de cota
- Gerenciar cotas do Gerenciador de recursos do servidor [de arquivos armazenamento-> criar/atualizar cota](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)

[**Exibir comentários e recursos propostos para armazenamento**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Serviço de Migração do Armazenamento

O **serviço de migração de armazenamento** permite migrar servidores e compartilhamentos de arquivos para o Azure ou o Windows Server 2019, sem exigir que aplicativos ou usuários alterem nada.
[Obtenha uma visão geral do serviço de migração de armazenamento](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>O serviço de migração de armazenamento requer o Windows Server 2019.

## <a name="storage-replica"></a>Réplica de Armazenamento

Use a **réplica de armazenamento** para gerenciar a replicação de armazenamento de servidor para servidor.
[Saiba mais sobre a réplica de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Insights do Sistema

O **System insights** apresenta a análise preditiva nativamente no Windows Server para ajudar a fornecer uma visão mais detalhada do funcionamento do servidor.
[Obtenha uma visão geral do System insights](http://aka.ms/systeminsights)

>[!NOTE]
>O System insights requer o Windows Server 2019.

## <a name="updates"></a>Atualizações

**As atualizações** permitem que você gerencie atualizações do Microsoft e/ou do Windows em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em atualizações:

- Exibir atualizações do Windows ou Microsoft disponíveis
- Exibir uma lista de histórico de atualizações
- Instalar atualizações
- Verificar se há atualizações online do Microsoft Update
- Gerenciar a integração de [Gerenciamento de atualizações do Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Exibir comentários e recursos propostos para atualizações**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Máquinas virtuais

Consulte [Gerenciando máquinas virtuais com o centro de administração do Windows](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Comutadores Virtuais

Os **comutadores virtuais** permitem que você gerencie comutadores virtuais do Hyper-V em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em comutadores virtuais:

- Procurar e Pesquisar comutadores virtuais em um servidor
- Criar um novo comutador virtual
- Renomear um comutador virtual
- Excluir um comutador virtual existente
- Editar as propriedades de um comutador virtual

[**Exibir comentários e recursos propostos para comutadores virtuais**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
