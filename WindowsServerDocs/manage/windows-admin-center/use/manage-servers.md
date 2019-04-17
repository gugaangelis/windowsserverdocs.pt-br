---
title: Gerenciar servidores com o Windows Admin Center
description: Gerenciar servidores com o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93a40345d05a6230596832b2d455d36eee2401b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296678"
---
# Gerenciar servidores com o Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## Gerenciamento de máquinas do Windows Server

Você pode adicionar servidores individuais que executam o Windows Server 2012 ou posterior para Windows Admin Center para gerenciar o servidor com um conjunto abrangente de ferramentas, incluindo certificados, dispositivos, eventos, processos, funções e recursos, atualizações, as máquinas virtuais e muito mais.

![Tela de visão geral de conexão do servidor](../media/manage-servers/server-overview.png)

## Adicionar um servidor ao Windows Admin Center

Para adicionar um servidor ao Windows Admin Center:

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha esta opção Adicionar uma **Conexão de servidor**.
3. Digite o nome do servidor e, se solicitado, as credenciais para usar.
4. Clique em **Enviar** para concluir.

O servidor será adicionado à sua lista de conexão na página de visão geral. Clique nele para se conectar ao servidor.

> [!NOTE]
> Você também pode adicionar os [clusters de failover](manage-failover-clusters.md) ou [clusters hiperconvergentes](manage-hyper-converged.md) como uma conexão separada no Windows Admin Center.

## Ferramentas

As seguintes ferramentas estão disponíveis para conexões de servidor:

| Ferramenta | Descrição |
| ---- | ----------- |
| [Visão geral](#overview) | Exibir detalhes de servidor e controlar o estado do servidor |
| [Active Directory](#active-directory-preview) | Gerenciar o Active Directory |
| [Backup](#backup) | Exibir e configurar o Backup do Azure |  
| [Certificados](#certificates) | Exiba e modifique certificados |
| [Contêineres](#containers) | Contêineres de exibição |
| [Dispositivos](#devices) | Exiba e modifique dispositivos |
| [DHCP](#dhcp) | Exibir e gerenciar a configuração do servidor DHCP |
| [DNS](#dns) | Exibir e gerenciar a configuração do servidor DNS |
| [Eventos](#events) | Exibir eventos |
| [Arquivos](#files) | Procurar arquivos e pastas |
| [Firewall](#firewall) | Exiba e modifique as regras de firewall |
| [Aplicativos instalados](#installed-apps) | Exibir e remover aplicativos instalados |
| [Usuários e grupos locais](#local-users-and-groups) | Exiba e modifique usuários e grupos locais |
| [Rede](#network) | Exiba e modifique a dispositivos de rede |
| [PowerShell](#powershell) | Interagir com o servidor por meio do PowerShell |
| [Processos](#processes) | Exiba e modifique processos em execução |
| [Registro](#registry) | Exiba e modifique as entradas do registro |
| [Área de Trabalho Remota](#remote-desktop) | Interagir com o servidor por meio da área de trabalho remota |
| [Funções e recursos](#roles-and-features) | Exiba e modifique as funções e recursos |
| [Tarefas agendadas](#scheduled-tasks) | Exiba e modifique tarefas agendadas |
| [Serviços](#services) | Exiba e modifique serviços |
| [Configurações](#settings) | Exiba e modifique serviços |
| [Armazenamento](#storage) | Exiba e modifique a dispositivos de armazenamento |
| [Serviço de migração do armazenamento](#storage-migration-service) | Migrar servidores e compartilhamentos de arquivos para o Azure ou Windows Server 2019 |
| [Réplica de armazenamento](#storage-replica) | Usar a réplica de armazenamento para gerenciar a replicação de armazenamento de servidor para servidor |
| [Insights do Sistema](#system-insights) | Proporciona Insights do sistema que maior insight o funcionamento do servidor. |
| [Atualizações](#updates) | Modo de exibição instalado e verifique se há novas atualizações |
| [Máquinas virtuais](manage-virtual-machines.md) | Exibir e gerenciar máquinas virtuais |
| [Switches virtuais](#virtual-switches) | Exibir e gerenciar switches virtuais |

## Visão geral

**Visão geral** permite ver o estado atual de CPU, memória e desempenho de rede, bem como realizar operações e modificar as configurações em um computador de destino ou servidor.

### Recursos

Os seguintes recursos têm suporte na visão geral do Gerenciador do servidor:

- Exibir detalhes do servidor
- Atividade de modo de exibição da CPU
- Atividade de memória do modo de exibição
- Exibir atividade de rede
- Reinicie o servidor
- Servidor de desligamento
- Habilitar métricas de disco no servidor
- Editar a ID de computador no servidor
- Exibir o endereço IP do BMC com hiperlink (requer compatível com IPMI BMC).

[**Comentários do modo de exibição e recursos propostos de visão geral do servidor**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## Active Directory (visualização)

**Active Directory** é uma visualização antecipada que está disponível no [feed de extensão](../configure/using-extensions.md).

### Recursos

O gerenciamento de Active Directory a seguir estão disponíveis:

- Criar usuário
- Criar um grupo
- Pesquisa de usuários, computadores e grupos
- Painel de detalhes para os usuários, computadores e grupos quando selecionado na grade
- Global Grid ações os usuários, computadores e grupos (Ativar/desativar, remova)
- Redefinir a senha do usuário
- Objetos de usuário: definir associações de grupo & propriedades básicas
- Objetos de computador: configurar a delegação para um único computador
- Objetos de grupo: gerenciar a associação (Adicionar/remover 1 ao usuário uma vez)  

[**Comentários do modo de exibição e propostos recursos do Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## Backup

**Backup** permite que você proteja o servidor do Windows de danos, ataques ou desastres, fazendo backup do servidor diretamente em Microsoft Azure.
[Saiba mais sobre o Backup do Azure.](https://aka.ms/windows-admin-center-backup)

[Fornecer comentários para backup no Windows Admin Center](https://aka.ms/backup-wac-feedback)

### Recursos

Os recursos a seguir são suportados no Backup:

- Visão geral do status de backup do Azure
- Configurar o agendamento e itens de backup
- Iniciar ou parar um trabalho de backup
- Exibir histórico do trabalho de backup e status
- Exibir pontos de recuperação e recuperar dados
- Excluir dados de backup

## Certificados

**Certificados** permite que você gerencie repositórios de certificados em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em certificados:

- Navegar e procurar certificados existentes
- Exibir detalhes de certificados
- Exportar certificados
- Renovar certificados
- Solicitar novos certificados
- Excluir certificados

[**Comentários do modo de exibição e os recursos propostos para certificados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## Contêineres

**Contêineres** permite que você exiba os contêineres em um host de contêiner do Windows Server. No caso de um contêiner do Server Core do Windows em execução, você pode exibir os logs de eventos e acessar a CLI do contêiner.

[**Comentários do modo de exibição e os recursos propostos para contêineres**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## Dispositivos

**Dispositivos** permite que você gerencie dispositivos conectados em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em dispositivos:

- Dispositivos de navegação e pesquisa
- Exibir detalhes do dispositivo
- Desativar um dispositivo
- Atualização do driver em um dispositivo

[**Comentários do modo de exibição e os recursos propostos para dispositivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## DHCP

**DHCP** permite que você gerencie dispositivos conectados em um computador ou servidor.

### Recursos

- Escopos criar/configurar/exibição IPV4 e IPV6
- Criar exclusões de endereço e configurar o endereço IP de início e término
- Criar reservas de endereço e configurar o cliente endereço MAC (IPV4), DUID e IAID (IPV6)

[**Comentários do modo de exibição e os recursos propostos para DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## DNS

**DNS** permite que você gerencie dispositivos conectados em um computador ou servidor.

### Recursos

- Exibir detalhes de zonas de pesquisa direta do DNS, zonas de pesquisa inversa e registros DNS
- Criar zonas de pesquisa direta (primária, secundária ou stub) e configurar as propriedades de zona de pesquisa direta
- Criar Host (A ou AAAA), CNAME ou MX tipo de registros DNS
- Configurar as propriedades de registros DNS
- Criar zonas de pesquisa inversa IPV6 IPV4 (principal, secundária e stub), configurar as propriedades de zona de pesquisa inversa
- Criar PTR, registra tipo CNAME do DNS na zona de pesquisa inversa.

[**Comentários do modo de exibição e os recursos propostos para DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## Eventos

**Eventos** permite que você gerencie os logs de eventos em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em eventos de:

- Eventos de navegação e pesquisa
- Exibir detalhes do evento
- Limpar eventos do log de
- Eventos de exportação do log

[**Comentários do modo de exibição e os recursos propostos para eventos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## Arquivos

**Arquivos** permite que você gerencie arquivos e pastas em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em arquivos:

- Procurar arquivos e pastas
- Procurar um arquivo ou pasta
- Crie uma nova pasta
- Excluir um arquivo ou pasta
- Baixar um arquivo ou pasta
- Carregar um arquivo ou pasta
- Renomear um arquivo ou pasta
- Extrair um arquivo zip
- Exibir as propriedades de arquivo ou pasta
- Gerenciar [cotas](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management) de Gerenciador de recursos de servidor de arquivos
- Adicionar, editar ou remover compartilhamentos de arquivos
- Modificar permissões de usuário e grupo em compartilhamentos de arquivos

[**Comentários do modo de exibição e os recursos propostos para arquivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## Firewall

**Firewall** permite que você gerencie as configurações de firewall e regras em um computador ou servidor.

### Recursos

Os recursos a seguir são suportados no Firewall:

- Visão geral das configurações de firewall
- Exibir regras de firewall de entrada
- As regras de firewall de saída do modo de exibição
- Regras de firewall de pesquisa
- Exibir detalhes da regra de firewall
- Criar uma nova regra de firewall
- Habilitar ou desabilitar uma regra de firewall
- Excluir uma regra de firewall
- Editar as propriedades de uma regra de firewall

[**Exibir comentários e recursos propostos para o Firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## Aplicativos instalados

**Aplicativos instalados** permite que você listar e desinstalar o aplicativo que são instalados.

[**Comentários do modo de exibição e os recursos propostos para aplicativos instalados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## Usuários e grupos locais

**Usuários e grupos locais** permite que você gerencie grupos de segurança e os usuários que existem localmente em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em usuários e grupos locais:

- Modo de exibição e pesquisa de usuários e grupos
- Criar um novo usuário ou grupo
- Gerenciar a associação de grupo do usuário
- Excluir um usuário ou grupo
- Alterar a senha do usuário
- Editar as propriedades de um usuário ou grupo

[**Exibir comentários e recursos propostos para usuários e grupos locais**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## Rede

**Rede** permite que você gerencie dispositivos de rede e configurações em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em rede:

- Navegar e procurar existente adaptadores de rede
- Exibir detalhes de um adaptador de rede
- Editar propriedades de um adaptador de rede
- Criar um [Adaptador de rede do Azure (recurso de visualização)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Exibir comentários e proposta de recursos de rede**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## PowerShell

**PowerShell** permite que você interaja com um computador ou servidor por meio de uma sessão do PowerShell.

### Recursos

Os recursos a seguir são suportados no PowerShell:

- Criar uma sessão interativa do PowerShell no servidor
- Desconecte a sessão do PowerShell no servidor

[**Exibir comentários e recursos propostos para o PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## Processos

**Processos** permite que você gerencie processos em execução em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em processos:

- Navegar e procurar processos em execução
- Exibir detalhes do processo
- Iniciar um processo
- Encerrar um processo
- Criar um despejo de processo
- Encontre os identificadores de processo

[**Comentários do modo de exibição e os recursos propostos para processos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## Registro

**Registro** permite que você gerencie chaves do registro e valores em um computador ou servidor.

### Recursos

Os recursos a seguir são suportados no registro:

- Procurar valores e chaves do registro
- Adicionar ou modificar os valores do registro
- Excluir os valores do registro

[**Exibir comentários e recursos propostos para o registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## Área de Trabalho Remota

**Área de trabalho remota** permite que você interaja com um computador ou servidor por meio de uma sessão de área de trabalho interativa.

### Recursos

Os seguintes recursos têm suporte na área de trabalho remota:

- Iniciar uma sessão interativa de área de trabalho remota
- Desconectar-se de uma sessão de área de trabalho remota
- Enviar Ctrl + Alt + Del para uma sessão de área de trabalho remota

[**Comentários do modo de exibição e os recursos propostos para área de trabalho remota**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## Funções e recursos

**Funções e recursos** permite que você gerencie funções e recursos em um servidor.

### Recursos

Os recursos a seguir são suportados nas funções e recursos:

- Navegar na lista de funções e recursos em um servidor
- Exibir detalhes de função ou recurso
- Instalar uma função ou recurso
- Remover uma função ou recurso

[**Comentários do modo de exibição e os recursos propostos para funções e recursos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## Tarefas agendadas

**Tarefas agendadas** permite que você gerencie tarefas agendadas em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte em tarefas agendadas:

- Procurar a biblioteca do Agendador de tarefas
- Editar tarefas agendadas
- Habilitar & desabilitar agendada tarefas
- Iniciar & parada agendada tarefas
- Crie tarefas agendadas

[**Comentários do modo de exibição e propostos recursos para tarefas agendadas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## Serviços

**Serviços** permite que você gerencie serviços em um computador ou servidor.

### Recursos

Os seguintes recursos têm suporte nos serviços:

- Navegar e procurar serviços em um servidor
- Exibir detalhes de um serviço
- Iniciar um serviço
- Pausar um serviço
- Editar as propriedades de um serviço

[**Recursos propostos para serviços e comentários de modo de exibição**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## Configurações

**Configurações** é um local central para gerenciar as configurações em um computador ou servidor.

### Recursos

- Exiba e modifique as variáveis de ambiente do usuário e do sistema
- Exibir a configuração de monitoramento de alertas do [Monitor do Azure](azure-monitor.md)
- Exiba e modifique a configuração de energia
- Exiba e modifique as configurações de área de trabalho remota
- Exiba e modifique as configurações de controle de acesso baseado em função
- Exibir e modificar as configurações de host do Hyper-V, se aplicável

## Armazenamento

**Armazenamento** permite que você gerencie dispositivos de armazenamento em um computador ou servidor.

### Recursos

Os recursos a seguir são suportados no armazenamento:

- Navegar e procurar discos existentes em um servidor
- Exibir detalhes do disco
- Crie um volume
- Inicializar um disco
- Criar, anexar e desanexar um disco rígido virtual (VHD)
- Colocar um disco offline
- Formatar um volume
- Redimensionar um volume
- Editar propriedades de volume
- Excluir um volume
- Instalar o gerenciamento de cota

[**Exibir comentários e proposta de recursos de armazenamento**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## Serviço de migração do armazenamento

**Serviço de migração de armazenamento** permite que você migre servidores e compartilhamentos de arquivo para o Azure ou Windows Server 2019 — sem a necessidade de aplicativos ou usuários alterar nada.
[Obtenha uma visão geral do serviço de migração do armazenamento](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Serviço de migração do armazenamento requer o Windows Server 2019.

## Réplica de armazenamento

Use **A réplica de armazenamento** para gerenciar a replicação de armazenamento de servidor para servidor.
[Saiba mais sobre a Qualidade de serviço de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## Insights do Sistema

**Insights do sistema** apresenta a análise de previsão nativamente no Windows Server para ajudar a oferecer a você maior insight o funcionamento do servidor.
[Obter uma visão geral de Insights do sistema](http://aka.ms/systeminsights)

>[!NOTE]
>Insights do sistema requer o Windows Server 2019.

## Atualizações

**Atualizações** permite que você gerencie a Microsoft e/ou atualizações do Windows em um computador ou servidor.

### Recursos

Os recursos a seguir são suportados nas atualizações:

- Exibir disponíveis do Windows ou Microsoft Updates
- Exibir uma lista de histórico de atualizações
- Instalar atualizações
- Verificar online se há atualizações do Microsoft Update
- Gerenciar a integração de [Gerenciamento de atualização do Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Exibir comentários e recursos propostos para atualizações**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## Máquinas virtuais

Consulte [gerenciamento de máquinas virtuais com o Windows Admin Center](manage-virtual-machines.md)

## Switches virtuais

**Switches virtuais** permite que você gerencie comutadores virtuais Hyper-V em um computador ou servidor.

### Recursos

Os recursos a seguir são compatíveis em Switches virtuais:

- Navegar e procurar Switches virtuais em um servidor
- Criar um novo comutador Virtual
- Renomear um comutador Virtual
- Excluir um comutador Virtual existente
- Editar as propriedades de um comutador Virtual

[**Exibir comentários e recursos propostos de Switches virtuais**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
