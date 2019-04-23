---
title: Funções, serviços de função e recursos incluídos no Windows Server - Server Core
description: Quais funções e recursos estão incluídos na opção de instalação Server Core do Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859287"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Funções, serviços de função e recursos incluídos no Windows Server - Server Core

> Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

Em geral, podemos falar sobre [o que do *não* no Server Core](server-core-removed-roles.md) : agora, vamos tentar uma abordagem diferente e lhe dizer da *incluídos* e se algo é *instalados por padrão*. As seguintes funções, serviços de função e recursos são *em* a opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para o seu ambiente. Como esta é uma lista grande, considere procurando específica função ou recurso que você está interessado - se que a pesquisa não retornar o que você está procurando, ele não está incluído no Server Core.

Por exemplo, se você procurar por "Host de sessão de área de trabalho remota -" você não o encontrará nesta página. Isso ocorre porque o Host de sessão de área de trabalho remota não está incluído na imagem do Server Core.

Lembre-se de que você pode [empre fico](server-core-removed-roles.md) em quais do *não* incluídos. Isso é apenas uma maneira diferente para examinar as coisas.

## <a name="roles-included-in-server-core"></a>Funções incluídas no Server Core
A opção de instalação Server Core inclui as seguintes funções de servidor.

| Função                                            | Nome                           | Instalado por padrão? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Serviços de Certificados do Active Directory           | AD-Certificate                 | N                     |
| Active Directory Domain Services                | AD-Domain-Services             | N                     |
| Serviços de Federação do Active Directory (AD FS)            | ADFS-Federation                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Atestado de integridade de dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Serviços de Arquivo e Armazenamento                       | FileAndStorage-Services        | Y                     |
| Serviço Guardião de Host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Serviços de impressão e documentos                     | Serviços de impressão                 | N                     |
| Acesso remoto                                   | RemoteAccess                   | N                     |
| Serviços da Área de Trabalho Remota                         | Remote-Desktop-Services        | N                     |
| Serviços de Ativação por Volume                      | VolumeActivation               | N                     |
| Servidor Web IIS                                  | Web-Server                     | N                     |
| Experiência do Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Serviços de função incluídos no Server Core
A opção de instalação Server Core inclui os seguintes serviços de função.

| Função                                  | Serviço de função                                                   | Nome                    | Instalado por padrão? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Serviços de Certificados do Active Directory | Autoridade de certificação                                        | ADCS-Cert-Authority     | N                     |
|                                       | Serviço Web de Política de Registro de Certificado                      | ADCS-Enroll-Web-Pol     | N                     |
|                                       | Serviço Web de Registro de Certificado                             | ADCS-Enroll-Web-Svc     | N                     |
|                                       | Registro na Web de Autoridade de Certificação                         | ADCS-Web-Enrollment     | N                     |
|                                       | Serviço de Inscrição do Dispositivo de Rede                              | ADCS-Device-Enrollment  | N                     |
|                                       | Respondente Online                                               | ADCS-Online-Cert        | N                     |
| Gerenciamento de direitos do Active Directory    | Servidor de Gerenciamento de Direitos do Active Directory                      | Servidor ADRMS            | N                     |
|                                       | Suporte à Federação de Identidade                                    | ADRMS-Identity          | N                     |
| Serviços de Arquivo e Armazenamento             | Serviços de Arquivo e iSCSI                                        | Serviços de arquivo           | N                     |
|                                       | Servidor de arquivos                                                    | FS-FileServer           | N                     |
|                                       | BranchCache para arquivos de rede                                  | FS-BranchCache          | N                     |
|                                       | Eliminação de Duplicação de Dados                                             | FS-Data-Deduplication   | N                     |
|                                       | Namespaces DFS                                                 | FS-DFS-Namespace        | N                     |
|                                       | Replicação do DFS                                                | FS-DFS-Replication      | N                     |
|                                       | Gerenciador de Recursos de Servidor de Arquivos                                   | FS-Resource-Manager     | N                     |
|                                       | Serviço de Agente VSS de Servidor de Arquivos                                  | FS-VSS-Agent            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget-Server      | N                     |
|                                       | o provedor de armazenamento de destino (provedores de hardware VDS e VSS) iSCSI | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | FS-NFS-Service          | N                     |
|                                       | Pastas de trabalho                                                   | FS-SyncShareService     | N                     |
|                                       | Serviços de armazenamento                                               | Serviços de armazenamento        | Y                     |
| Serviços de impressão e documentos           | Servidor de Impressão                                                   | Servidor de impressão            | N                     |
|                                       | Serviço LPD                                                    | Print-LPD-Service       | N                     |
| Acesso remoto                         | DirectAccess e VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Roteamento                                                        | Roteamento                 | N                     |
|                                       | Proxy de aplicativo Web                                          | Proxy de aplicativo Web   | N                     |
| Serviços da Área de Trabalho Remota               | Agente de Conexão de Área de Trabalho Remota                               | RDS-Connection-Broker   | N                     |
|                                       | Licenciamento de Área de Trabalho Remota                                       | RDS-Licensing           | N                     |
|                                       | Host de Virtualização de área de trabalho remota                             | RDS-Virtualization      | N                     |
| Servidor Web (IIS)                      | Servidor Web                                                     | Web-WebServer           | N                     |
|                                       | Recursos HTTP comuns                                           | Web-Common-Http         | N                     |
|                                       | Documento padrão                                               | Web-Default-Doc         | N                     |
|                                       | Pesquisa no diretório                                             | Web-Dir-Browsing        | N                     |
|                                       | Erros de HTTP                                                    | Web-Http-Errors         | N                     |
|                                       | Conteúdo estático                                                 | Web-Static-Content      | N                     |
|                                       | Redirecionamento de HTTP                                               | Web-Http-Redirect       | N                     |
|                                       | Publicação WebDAV                                              | Publicação de Web DAV      | N                     |
|                                       | Integridade e diagnóstico                                         | Web-Health              | N                     |
|                                       | Log HTTP                                                   | Web-Http-Logging        | N                     |
|                                       | Registro em log personalizado                                                 | Web-personalizado-registro em log      | N                     |
|                                       | Ferramentas de log                                                  | Web-Log-Libraries       | N                     |
|                                       | Log de ODBC                                                   | Web-ODBC-Logging        | N                     |
|                                       | Monitor de solicitação                                              | Web-Request-Monitor     | N                     |
|                                       | Rastreamento                                                        | Web-Http-Tracing        | N                     |
|                                       | Desempenho                                                    | Desempenho na Web         | N                     |
|                                       | Compactação de conteúdo estático                                     | Web-Stat-Compression    | N                     |
|                                       | Compactação de conteúdo dinâmico                                    | Compactação de Dyn da Web     | N                     |
|                                       | Segurança                                                       | Segurança da Web            | N                     |
|                                       | Request Filtering                                              | Filtragem da Web           | N                     |
|                                       | Autenticação Básica                                           | Web-Basic-Auth          | N                     |
|                                       | Suporte a certificados SSL centralizado                            | Web-CertProvider        | N                     |
|                                       | Autenticação de mapeamento de certificado de cliente                      | Web-Client-Auth         | N                     |
|                                       | Autenticação Digest                                          | Web-Digest-Auth         | N                     |
|                                       | Autenticação de mapeamento de certificado de cliente do IIS                  | Web-Cert-Auth           | N                     |
|                                       | Restrições de IP e domínio                                     | Segurança de IP da Web         | N                     |
|                                       | Autorização de URL                                              | Web-Url-Auth            | N                     |
|                                       | Autenticação do Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Desenvolvimento de aplicativo                                        | Web-App-Dev             | N                     |
|                                       | Extensibilidade .NET 3.5                                         | Web-Net-Ext             | N                     |
|                                       | Extensibilidade .NET 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | Inicialização de Aplicativos                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensões ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-Filter        | N                     |
|                                       | Server Side Includes                                           | Inclui a Web            | N                     |
|                                       | Protocolo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Web-Ftp-Server          | N                     |
|                                       | Serviço de FTP                                                    | Web-Ftp-Service         | N                     |
|                                       | Extensibilidade de FTP                                              | Web-Ftp-Ext             | N                     |
|                                       | Ferramentas de gerenciamento                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilidade de Gerenciamento do IIS 6                                 | Web-Mgmt-Compat         | N                     |
|                                       | Compatibilidade de Metabase do IIS 6                                   | Web-Metabase            | N                     |
|                                       | Ferramentas de script 6 do IIS                                          | Web Lgcy Scripting      | N                     |
|                                       | Compatibilidade de WMI do IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts e Ferramentas de Gerenciamento do IIS                               | Ferramentas de script da Web     | N                     |
|                                       | Serviço de Gerenciamento                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Conectividade WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Serviços do WSUS                                                  | UpdateServices-Services | N                     |
|                                       | Conectividade do SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Recursos incluídos no Server Core
A opção de instalação Server Core inclui os seguintes recursos.

| Recurso                                                | Nome                               | Instalado por padrão? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Recursos do .NET framework 3.5                            | Recursos do NET Framework             | N                     |
| .NET framework 3.5 (inclui .NET 2.0 e 3.0)       | NET-Framework-Core                 | (removido)             |
| Ativação HTTP                                        | NET-HTTP-Activation                | N                     |
| Ativação não HTTP                                    | NET-não-HTTP-ativas                 | N                     |
| Recursos do .NET framework 4.6                            | NET-Framework-45-Features          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| Serviços do WCF                                           | NET-WCF-Services45                 | Y                     |
| Ativação HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Ativação de enfileiramento de mensagens (MSMQ)                      | NET-WCF MSMQ Activation45          | N                     |
| Ativação de Pipe nomeado                                  | NET-WCF-Pipe-Activation45          | N                     |
| Ativação TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Compartilhamento de porta TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Serviço de Transferência Inteligente em Segundo Plano (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-Server                | N                     |
| Criptografia de Unidade de Disco BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente NFS                                         | NFS-Client                         | N                     |
| Contêineres                                             | Contêineres                         | N                     |
| Data Center Bridging                                   | Data-Center-Bridging               | N                     |
| Armazenamento Avançado                                       | EnhancedStorage                    | N                     |
| Clustering de failover                                    | Clustering de failover                | N                     |
| Gerenciamento de Política de Grupo                                | GPMC                               | N                     |
| Qualidade de Serviço de E/S                                 | QoS DiskIo                         | N                     |
| Núcleo da Web Hospedável do IIS                                  | Web-WHC                            | N                     |
| Servidor de Gerenciamento de Endereço IP (IPAM)                    | IPAM                               | N                     |
| Serviço do iSNS Server                                    | ISNS                               | N                     |
| Extensão do IIS do Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-Media-Foundation            | N                     |
| Enfileiramento de Mensagens                                        | MSMQ                               | N                     |
| Serviços de enfileiramento de mensagens                               | MSMQ-Services                      | N                     |
| Servidor de enfileiramento de mensagens                                 | Servidor do MSMQ                        | N                     |
| Integração de Serviços de Diretório                          | Diretório do MSMQ                     | N                     |
| Suporte a HTTP                                           | Suporte para HTTP MSMQ                  | N                     |
| Disparadores de enfileiramento de mensagens                               | Disparadores do MSMQ                      | N                     |
| Serviço de roteamento                                        | Roteamento de MSMQ                       | N                     |
| Proxy DCOM do Serviço de Enfileiramento de Mensagens                             | MSMQ-DCOM                          | N                     |
| Multipath I/O                                          | E/s de vários caminhos                       | N                     |
| MultiPoint Connector                                   | Conector do multiPoint               | N                     |
| Conector do multiPoint Services                          | MultiPoint-Connector-Services      | N                     |
| MultiPoint Manager e o MultiPoint Dashboard            | Ferramentas do multiPoint                   | N                     |
| Balanceamento de Carga de Rede                                 | NLB                                | N                     |
| Protocolo PNRP                          | PNRP                               | N                     |
| Quality Windows Audio-Video Experience                 | qWave                              | N                     |
| Compactação Diferencial Remota                        | RDC                                | N                     |
| Ferramentas de Administração de Servidor Remoto                     | RSAT                               | N                     |
| Ferramentas de administração do recurso                           | RSAT-Feature-Tools                 | N                     |
| Utilitários de administração de criptografia de unidade de disco BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Ferramentas LLDP DataCenterBridging                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Ferramentas de cluster de failover                              | RSAT-Clustering                    | N                     |
| Módulo de Cluster de failover para o Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Servidor de automação de Cluster de failover                     | RSAT-Clustering-AutomationServer   | N                     |
| Interface de comando do Cluster de failover                     | RSAT-Clustering-CmdInterface       | N                     |
| Cliente IPAM (gerenciamento) do endereço IP                    | IPAM-Client-Feature                | N                     |
| Ferramentas de VM blindada                                      | RSAT---ferramentas de VM Blindada             | N                     |
| Módulo de réplica de armazenamento para o Windows PowerShell          | RSAT-Storage-Replica               | N                     |
| Ferramentas de administração de funções                              | RSAT-Role-Tools                    | N                     |
| Ferramentas AD DS e AD LDS                                 | RSAT-AD-Tools                      | N                     |
| Módulo do Active Directory para o Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Ferramentas de AD DS                                            | RSAT-ADDS                          | N                     |
| Centro Administrativo do Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Snap-ins AD DS e Ferrramentas de linha de comando                  | RSAT-ADDS-Tools                    | N                     |
| Ferramentas de linha de comando e Snap-Ins do AD LDS                 | RSAT-ADLDS                         | N                     |
| Ferramentas de Gerenciamento do Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Módulo Hyper-V para o Windows PowerShell                  | PowerShell do Hyper-V                 | N                     |
| Ferramentas do Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| Cmdlets do PowerShell e API                             | UpdateServices-API                 | N                     |
| Ferramentas do servidor DHCP                                      | RSAT-DHCP                          | N                     |
| Ferramentas do servidor DNS                                       | RSAT-DNS-Server                    | N                     |
| Ferramentas de gerenciamento de acesso remoto                         | RSAT-RemoteAccess                  | N                     |
| Módulo de Acesso Remoto para o Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| RPC sobre Proxy HTTP                                    | RPC-over-HTTP-Proxy                | N                     |
| Coleta de Eventos de Instalação e Inicialização                        | Instalação-e-Boot--coleta de eventos    | N                     |
| Serviços TCP/IP Simples                                 | Simple-TCPIP                       | N                     |
| Suporte para Compartilhamento de Arquivos SMB 1.0/ CIFS                      | FS-SMB1                            | Y                     |
| Limite de Largura de Banda do SMB                                    | FS-SMBBW                           | N                     |
| Serviço SNMP                                           | Serviço SNMP                       | N                     |
| Provedor de SNMP WMI                                      | SNMP-WMI-Provider                  | N                     |
| Cliente Telnet                                          | Telnet-Client                      | N                     |
| Ferramentas de Blindagem de VM para Gerenciamento de Malha               | FabricShieldedTools                | N                     |
| Recursos do Windows Defender                              | Windows-Defender-Features          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Banco de Dados Interno do Windows                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Mecanismo do Windows PowerShell 2.0                          | PowerShell-V2                      | (removido)             |
| Windows PowerShell Desired State Configuration do serviço | Serviço de DSC                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Serviço de Ativação de Processos do Windows                     | WAS                                | N                     |
| Modelo de Processo                                          | WAS-Process-Model                  | N                     |
| Ambiente .NET 3.5                                   | FOI-NET-Environment                | N                     |
| APIs de Configuração                                     | WAS-Config-APIs                    | N                     |
| Backup do Windows Server                                  | Windows-Server-Backup              | N                     |
| Ferramentas de Migração do Windows Server                         | Migração                          | N                     |
| Gerenciamento de Armazenamento com base nos padrões do Windows             | WindowsStorageManagementService    | N                     |
| Extensão IIS WinRM                                    | WinRM-IIS-Ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Suporte WoW64                                          | WoW64-Support                      | Y                     |
|                                                        |                                    |                       |
