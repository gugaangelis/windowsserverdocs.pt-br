---
title: Gerenciar servidores com Windows Admin Center
description: Gerenciar servidores com Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 04ade4a14272c7840b5036ca6ad5a3bf3d09bcf9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891147"
---
# <a name="manage-servers-with-windows-admin-center"></a>Gerenciar servidores com Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Gerenciando computadores com Windows Server

Você pode adicionar servidores individuais que executam o Windows Server 2012 ou posterior para Windows Admin Center para gerenciar o servidor com um conjunto abrangente de ferramentas, incluindo certificados, dispositivos, eventos, processos, funções e recursos, atualizações, as máquinas virtuais e muito mais.

![Tela de visão geral de conexão do servidor](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>A adição de um servidor Windows Admin Center

Para adicionar um servidor ao Windows Admin Center:

1. Clique em **+ adicionar** em todas as conexões.
2. Optar por adicionar um **Conexão de servidor**.
3. Digite o nome do servidor e, se solicitado, as credenciais a usar.
4. Clique em **enviar** para concluir.

O servidor será adicionado à sua lista de conexão na página de visão geral. Clique nele para se conectar ao servidor.

> [!NOTE]
> Você também pode adicionar [clusters de failover](manage-failover-clusters.md) ou [clusters hiperconvergente](manage-hyper-converged.md) como uma conexão separada no Windows Admin Center.

## <a name="tools"></a>Ferramentas

As seguintes ferramentas estão disponíveis para conexões de servidor:

| Ferramenta | Descrição |
| ---- | ----------- |
| [Visão geral](#overview) | Exibir detalhes do servidor e controlar o estado do servidor |
| [Backup](#backup) | Exibir e configurar o Backup do Azure |  
| [Certificados](#certificates) | Exibir e modificar os certificados |
| [Contêineres](#containers) | Exibir contêineres |
| [Dispositivos](#devices) | Exibir e modificar os dispositivos |
| [Eventos](#events) | Exibir eventos |
| [Arquivos](#files) | Procurar arquivos e pastas |
| [Firewall](#firewall) | Exibir e modificar as regras de firewall |
| [Aplicativos instalados](#installed-apps) | Exibir e remover aplicativos instalados |
| [Usuários e grupos locais](#local-users-and-groups) | Exibir e modificar usuários e grupos locais |
| [Rede](#network) | Exibir e modificar os dispositivos de rede |
| [PowerShell](#powershell) | Interagir com o servidor por meio do PowerShell |
| [Processos](#processes) | Exibir e modificar os processos em execução |
| [Registro](#registry) | Exibir e modificar as entradas do registro |
| [Área de trabalho remota](#remote-desktop) | Interagir com o servidor por meio da área de trabalho remota |
| [Funções e recursos](#roles-and-features) | Exibir e modificar as funções e recursos |
| [Tarefas agendadas](#scheduled-tasks) | Exibir e modificar tarefas agendadas |
| [Serviços](#services) | Exibir e modificar serviços |
| [Armazenamento](#storage) | Exibir e modificar os dispositivos de armazenamento |
| [Serviço de migração de armazenamento](#storage-migration-service) | Migrar servidores e compartilhamentos de arquivos para o Azure ou o Windows Server 2019 |
| [Réplica de armazenamento](#storage-replica) | Usar a réplica de armazenamento para gerenciar a replicação de armazenamento de servidor para servidor |
| [Informações do sistema](#system-insights) | Proporciona de Insights de sistema que maior percepção o funcionamento do seu servidor. |
| [atualizações](#updates) | Modo de exibição instalado e verificar se há novas atualizações |
| [Máquinas Virtuais](manage-virtual-machines.md) | Exibir e gerenciar máquinas virtuais |
| [Comutadores virtuais](#virtual-switches) | Exibir e gerenciar os comutadores virtuais |

## <a name="overview"></a>Visão geral

**Visão geral** permite que você veja o estado atual de CPU, memória e desempenho de rede, bem como executar operações e modificar as configurações em um servidor ou computador de destino.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte na visão geral do Gerenciador do servidor:

- Exibir detalhes do servidor
- Atividade da CPU do modo de exibição
- Exibir atividade de memória
- Exibir atividade de rede
- Reinicie o servidor
- Desligue o servidor
- Habilitar as métricas de disco no servidor
- Editar ID do computador no servidor

[**Exibir comentários e recursos propostos para visão geral do servidor**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="backup"></a>Backup

**Backup** permite que você proteja seu servidor Windows de corrupções, ataques ou desastres fazendo o backup do servidor diretamente no Microsoft Azure.
[Saiba mais sobre o Backup do Azure.](https://aka.ms/windows-admin-center-backup)

[Fornecer comentários para o backup no Windows Admin Center](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no Backup:

- Visão geral do status do backup do Azure
- Configurar agendamento e itens de backup
- Iniciar ou interromper um trabalho de backup
- Exibir o status e histórico de trabalho de backup
- Exibir pontos de recuperação e recuperar dados
- Excluir dados de backup

## <a name="certificates"></a>Certificados

**Certificados** permite que você gerencie repositórios de certificados em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em certificados:

- Procurar e pesquisar certificados existentes
- Exibir detalhes do certificado
- Exportar certificados
- Renovar certificados
- Solicitar novos certificados
- Excluir certificados

[**Exibir comentários e recursos propostos para certificados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Contêineres

**Contêineres** permite que você exiba os contêineres em um host de contêiner do Windows Server. No caso de um contêiner em execução do Windows Server Core, você pode exibir os logs de eventos e acessar a CLI do contêiner.

[**Exibir comentários e recursos propostos para contêineres**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Dispositivos

**Dispositivos** permite que você gerencie dispositivos conectados em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em dispositivos:

- Dispositivos de navegação e pesquisa
- Exibir detalhes do dispositivo
- Desativar um dispositivo
- Atualização do driver em um dispositivo

[**Exibir comentários e recursos propostos para dispositivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="events"></a>Eventos

**Eventos** permite que você gerencie logs de eventos em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte nos eventos:

- Eventos de navegação e pesquisa
- Exibir detalhes do evento
- Limpar eventos do log
- Eventos de exportação do log

[**Exibir comentários e recursos propostos para eventos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>Arquivos

**Arquivos** permite que você gerencie os arquivos e pastas em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em arquivos:

- Procurar arquivos e pastas
- Procure um arquivo ou pasta
- Crie uma nova pasta
- Excluir um arquivo ou pasta
- Baixar um arquivo ou pasta
- Carregar um arquivo ou pasta
- Renomear um arquivo ou pasta
- Extrair um arquivo zip
- Exibir propriedades do arquivo ou pasta
- Gerenciar o Gerenciador de recursos de servidor de arquivos [cotas](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- Adicionar, editar ou remover compartilhamentos de arquivos
- Modificar permissões de usuário e grupo em compartilhamentos de arquivos

[**Exibir comentários e recursos propostos para arquivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Firewall

**Firewall** permite que você gerencie as configurações de firewall e regras em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no Firewall:

- Visão geral das configurações de firewall
- Exibir regras de firewall de entrada
- Exibir regras de firewall de saída
- Pesquisar regras de firewall
- Exibir detalhes da regra de firewall
- Criar uma nova regra de firewall
- Habilitar ou desabilitar uma regra de firewall
- Excluir uma regra de firewall
- Editar as propriedades de uma regra de firewall

[**Exibir comentários e recursos propostos para o Firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>Aplicativos instalados

**Aplicativos instalados** permite que você liste e desinstalar o aplicativo que estão instalados.

[**Exibir comentários e recursos propostos para aplicativos instalados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Usuários e Grupos Locais

**Os usuários e grupos locais** permite que você gerencie grupos de segurança e os usuários que existem localmente em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em usuários e grupos locais:

- Exibir e pesquisar usuários e grupos
- Criar um novo usuário ou grupo
- Gerenciar a associação de grupo do usuário
- Excluir um usuário ou grupo
- Alterar a senha do usuário
- Editar as propriedades de um usuário ou grupo

[**Exibir comentários e recursos propostos para usuários e grupos locais**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Rede

**Rede** permite que você gerencie dispositivos de rede e configurações em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte na rede:

- Procurar e pesquisar os adaptadores de rede existente
- Exibir detalhes de um adaptador de rede
- Editar propriedades de um adaptador de rede
- Criar um [adaptador de rede do Azure (recurso de visualização)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Exibir comentários e recursos propostos para a rede**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** permite que você interaja com um computador ou servidor por meio de uma sessão do PowerShell.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no PowerShell:

- Criar uma sessão interativa do PowerShell no servidor
- Desconecte a sessão do PowerShell no servidor

[**Exibir comentários e recursos propostos para o PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processos

**Processos** permite que você gerencie os processos em execução em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em processos:

- Navegue e pesquise por processos em execução
- Exibir detalhes do processo
- Iniciar um processo
- Encerrar um processo
- Criar um despejo de processo
- Localize identificadores de processo

[**Exibir comentários e recursos propostos para processos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registro

**Registro** permite que você gerencie as chaves do registro e valores em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no registro:

- Procurar valores e chaves do registro
- Adicionar ou modificar valores do registro
- Excluir valores de registro

[**Exibir comentários e recursos propostos para o registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Área de Trabalho Remota

**Área de trabalho remota** permite que você interaja com um computador ou servidor por meio de uma sessão de área de trabalho interativa.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte na área de trabalho remota:

- Iniciar uma sessão de área de trabalho remota interativa
- Desconectar-se de uma sessão de área de trabalho remota
- Enviar Ctrl + Alt + Del para uma sessão de área de trabalho remota

[**Exibir comentários e recursos propostos para a área de trabalho remota**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Funções e recursos

**Funções e recursos** permite que você gerencie funções e recursos em um servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em funções e recursos:

- Procurar a lista de funções e recursos em um servidor
- Exibir detalhes de função ou recurso
- Instalar uma função ou recurso
- Remover uma função ou recurso

[**Exibir comentários e recursos propostos para funções e recursos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>TAREFAS AGENDADAS

**Tarefas agendadas** permite que você gerencie as tarefas agendadas em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em tarefas agendadas:

- Procurar a biblioteca do Agendador de tarefas
- Editar tarefas agendadas
- Habilitar e desabilitar as tarefas agendadas
- Iniciar e interromper as tarefas agendadas
- Criar tarefas agendadas

[**Exibir comentários e recursos propostos para tarefas agendadas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Serviços

**Serviços** permite que você gerencie serviços em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte nos serviços:

- Serviços de navegação e pesquisa em um servidor
- Exibir detalhes de um serviço
- Iniciar um serviço
- Pausar um serviço
- Editar as propriedades de um serviço

[**Exibir comentários e recursos propostos para serviços**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="storage"></a>Armazenamento

**Armazenamento** permite que você gerencie dispositivos de armazenamento em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte no armazenamento:

- Pesquisa de discos existentes em um servidor
- Exibir detalhes do disco
- Criar um volume
- Inicializar um disco
- Criar, anexar e desanexar um disco rígido virtual (VHD)
- Coloque um disco offline
- Formatar um volume
- Redimensiona um volume
- Editar propriedades de volume
- Excluir um volume
- Instalar o gerenciamento de cotas

[**Exibir comentários e recursos propostos para o armazenamento**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Serviço de Migração do Armazenamento

**Serviço de migração de armazenamento** permite que você migre servidores e compartilhamentos de arquivo para o Azure ou o Windows Server 2019 — sem a necessidade de aplicativos ou usuários alterar nada.
[Obtenha uma visão geral do serviço de migração de armazenamento](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Serviço de migração de armazenamento requer o Windows Server 2019.

## <a name="storage-replica"></a>Réplica de Armazenamento

Use **a réplica de armazenamento** para gerenciar a replicação de armazenamento de servidor para servidor.
[Saiba mais sobre a réplica de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Insights do Sistema

**Insights de sistema** apresenta análise preditiva nativamente em aumentou de Windows Server para ajudar a fornecer informações sobre o funcionamento do seu servidor.
[Obtenha uma visão geral de informações do sistema](http://aka.ms/systeminsights)

>[!NOTE]
>Informações do sistema requer 2019 do Windows Server.

## <a name="updates"></a>Atualizações

**Atualizações** permite que você gerencie a Microsoft e/ou atualizações do Windows em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte nas atualizações:

- Exibir disponível Windows ou o Microsoft Updates
- Exibir uma lista de histórico de atualização
- Instalar atualizações
- Verificar online se há atualizações do Microsoft Update
- Gerencie [gerenciamento de atualizações do Azure](https://docs.microsoft.com/azure/automation/automation-update-management) integração

[**Exibir comentários e recursos propostos para atualizações**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Máquinas virtuais

Consulte [Gerenciando máquinas virtuais com Windows Admin Center](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Comutadores Virtuais

**Comutadores virtuais** permite que você gerencie os comutadores virtuais do Hyper-V em um computador ou servidor.

### <a name="features"></a>Recursos

Os recursos a seguir têm suporte em comutadores virtuais:

- Procurar e pesquisar os comutadores virtuais em um servidor
- Criar um novo comutador Virtual
- Renomear um comutador Virtual
- Excluir um comutador Virtual existente
- Editar as propriedades de um comutador Virtual

[**Exibir comentários e recursos propostos para comutadores virtuais**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
