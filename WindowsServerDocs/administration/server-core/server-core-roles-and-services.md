---
title: Funções, serviços de função e recursos incluídos no Windows Server-Server Core
description: Quais funções e recursos estão incluídos na opção de instalação do Server Core do Windows Server?
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 7b5d5d5ad38b1b03e409c26485860f43799f1322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383334"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Funções, serviços de função e recursos incluídos no Windows Server-Server Core

> Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

Em geral, falamos sobre [o que *não* está no Server Core](server-core-removed-roles.md) . agora, vamos tentar uma abordagem diferente e informar o que está *incluído* e se algo está *instalado por padrão*. As funções, os serviços de função e os recursos a seguir estão *na* opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para seu ambiente. Como esta é uma lista grande, considere procurar a função ou o recurso específico no qual você está interessado – se essa pesquisa não retornar o que você está procurando, ela não estará incluída no Server Core.

Por exemplo, se você pesquisar por "Host da Sessão da Área de Trabalho Remota", não o encontrará nesta página. Isso ocorre porque o Host da Sessão RD não está incluído na imagem do Server Core.

Lembre-se de que você [sempre pode examinar](server-core-removed-roles.md) o que *não* está incluído. Essa é apenas uma maneira diferente de examinar as coisas.

## <a name="roles-included-in-server-core"></a>Funções incluídas no Server Core
A opção de instalação Server Core inclui as seguintes funções de servidor.

| Role                                            | Nome                           | Instalado por padrão? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Serviços de Certificados do Active Directory           | AD-certificado                 | N                     |
| Active Directory Domain Services                | AD-Domain-Services             | N                     |
| Serviços de Federação do Active Directory (AD FS)            | ADFS-Federação                | N                     |
| Active Directory Lightweight Directory Services | LDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Atestado de integridade de dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Serviços de Arquivo e Armazenamento                       | FileAndStorage-serviços        | S                     |
| Serviço Guardião de Host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Serviços de impressão e documentos                     | Serviços de impressão                 | N                     |
| Acesso remoto                                   | RemoteAccess                   | N                     |
| Serviços da Área de Trabalho Remota                         | Serviços de área de trabalho remota        | N                     |
| Serviços de Ativação por Volume                      | VolumeActivation               | N                     |
| Servidor Web IIS                                  | Servidor Web                     | N                     |
| Experiência do Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Serviços de função incluídos no Server Core
A opção de instalação Server Core inclui os seguintes serviços de função.

| Role                                  | Serviço de função                                                   | Nome                    | Instalado por padrão? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Serviços de Certificados do Active Directory | Autoridade de certificação                                        | ADCS-CERT-Authority     | N                     |
|                                       | Serviço Web de Política de Registro de Certificado                      | ADCS-registrar-Web-pol     | N                     |
|                                       | Serviço Web de Registro de Certificado                             | ADCS-inscrever-Web-svc     | N                     |
|                                       | Registro na Web de Autoridade de Certificação                         | ADCS-registro da Web     | N                     |
|                                       | Serviço de Inscrição do Dispositivo de Rede                              | ADCS-registro de dispositivo  | N                     |
|                                       | Respondente Online                                               | ADCS-online-CERT        | N                     |
| Active Directory Rights Management    | Servidor de Gerenciamento de Direitos do Active Directory                      | ADRMS-servidor            | N                     |
|                                       | Suporte à Federação de Identidade                                    | ADRMS-identidade          | N                     |
| Serviços de Arquivo e Armazenamento             | Serviços de Arquivo e iSCSI                                        | Serviços de arquivos           | N                     |
|                                       | Servidor de arquivos                                                    | FS-arquivos de arquivos           | N                     |
|                                       | BranchCache para arquivos de rede                                  | FS-BranchCache          | N                     |
|                                       | Eliminação de Duplicação de Dados                                             | FS-eliminação de duplicação de dados   | N                     |
|                                       | Namespaces DFS                                                 | FS-DFS-namespace        | N                     |
|                                       | Replicação do DFS                                                | FS-Replicação DFS      | N                     |
|                                       | Gerenciador de Recursos de Servidor de Arquivos                                   | FS-Gerenciador de recursos     | N                     |
|                                       | Serviço de Agente VSS de Servidor de Arquivos                                  | FS-VSS-Agent            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget-servidor      | N                     |
|                                       | Provedor de armazenamento de destino iSCSI (provedores de hardware VDS e VSS) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | Serviço FS-NFS          | N                     |
|                                       | Pastas de trabalho                                                   | FS-SyncShareService     | N                     |
|                                       | Serviços de armazenamento                                               | Serviços de armazenamento        | S                     |
| Serviços de impressão e documentos           | Servidor de Impressão                                                   | Servidor de impressão            | N                     |
|                                       | Serviço LPD                                                    | Print-LPD-Service       | N                     |
| Acesso remoto                         | DirectAccess e VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Roteamento                                                        | Roteamento                 | N                     |
|                                       | Proxy de aplicativo Web                                          | Aplicativo Web-proxy   | N                     |
| Serviços da Área de Trabalho Remota               | Agente de Conexão de Área de Trabalho Remota                               | RDS-Connection-Broker   | N                     |
|                                       | Licenciamento de Área de Trabalho Remota                                       | Licenciamento RDS           | N                     |
|                                       | Host de Virtualização de área de trabalho remota                             | RDS-virtualização      | N                     |
| Servidor Web (IIS)                      | Servidor Web                                                     | Web-webserver           | N                     |
|                                       | Recursos HTTP comuns                                           | Web-Common-http         | N                     |
|                                       | Documento padrão                                               | Web-padrão-doc         | N                     |
|                                       | Pesquisa no diretório                                             | Web-dir-navegando        | N                     |
|                                       | Erros de HTTP                                                    | Web-http-erros         | N                     |
|                                       | Conteúdo estático                                                 | Web-estático-conteúdo      | N                     |
|                                       | Redirecionamento de HTTP                                               | Web-http-Redirect       | N                     |
|                                       | Publicação de WebDAV                                              | Publicação na Web-DAV      | N                     |
|                                       | Integridade e diagnóstico                                         | Web-integridade              | N                     |
|                                       | Log HTTP                                                   | Log Web-http        | N                     |
|                                       | Log personalizado                                                 | Log personalizado da Web      | N                     |
|                                       | Ferramentas de log                                                  | Bibliotecas de log da Web       | N                     |
|                                       | Log de ODBC                                                   | Log Web-ODBC        | N                     |
|                                       | Monitor de solicitações                                              | Monitor de solicitação da Web     | N                     |
|                                       | Rastreamento                                                        | Rastreamento de http Web        | N                     |
|                                       | Desempenho                                                    | Desempenho da Web         | N                     |
|                                       | Compactação de conteúdo estático                                     | Web-Stat-Compression    | N                     |
|                                       | Compactação de conteúdo dinâmico                                    | Web-Dyn-Compression     | N                     |
|                                       | Segurança                                                       | Segurança da Web            | N                     |
|                                       | Request Filtering                                              | Filtragem da Web           | N                     |
|                                       | Autenticação Básica                                           | Web-básico-autenticação          | N                     |
|                                       | Suporte a certificados SSL centralizados                            | Web-Certprovider        | N                     |
|                                       | Autenticação de mapeamento de certificado de cliente                      | Web-cliente-autenticação         | N                     |
|                                       | Autenticação Digest                                          | Web-Digest-auth         | N                     |
|                                       | Autenticação de mapeamento de certificado de cliente IIS                  | Web-CERT-auth           | N                     |
|                                       | Restrições de IP e domínio                                     | Segurança de IP da Web         | N                     |
|                                       | Autorização de URL                                              | Web-URL-auth            | N                     |
|                                       | Autenticação do Windows                                         | Web-Windows-auth        | N                     |
|                                       | Desenvolvimento de aplicativo                                        | Aplicativo Web-desenvolvimento             | N                     |
|                                       | Extensibilidade do .NET 3,5                                         | Web-net-ext             | N                     |
|                                       | Extensibilidade do .NET 4,6                                         | Web-net-Ext45           | N                     |
|                                       | Inicialização de Aplicativos                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3,5                                                    | Web-ASP-net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-ASP-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensões ISAPI                                               | Web-ISAPI-ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-Filter        | N                     |
|                                       | Server Side Includes                                           | Web-inclui            | N                     |
|                                       | Protocolo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Servidor Web-FTP          | N                     |
|                                       | Serviço FTP                                                    | Web-FTP-Service         | N                     |
|                                       | Extensibilidade de FTP                                              | Web-FTP-ext             | N                     |
|                                       | Ferramentas de gerenciamento                                               | Web-MGMT-Tools          | N                     |
|                                       | Compatibilidade de Gerenciamento do IIS 6                                 | Web-MGMT-compat         | N                     |
|                                       | Compatibilidade de Metabase do IIS 6                                   | Web-metabase            | N                     |
|                                       | Ferramentas de script do IIS 6                                          | Web-LGCY-scripting      | N                     |
|                                       | Compatibilidade de WMI do IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts e Ferramentas de Gerenciamento do IIS                               | Web-Scripting-ferramentas     | N                     |
|                                       | Serviço de Gerenciamento                                             | Web-mgmt-serviço        | N                     |
| Windows Server Update Services        | Conectividade de WID                                               | Updateservices-WidDB    | N                     |
|                                       | Serviços do WSUS                                                  | Updateservices-Services | N                     |
|                                       | Conectividade de SQL Server                                        | Updateservices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Recursos incluídos no Server Core
A opção de instalação Server Core inclui os seguintes recursos.

| Recurso                                                | Nome                               | Instalado por padrão? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Recursos do .NET Framework 3,5                            | NET-Framework-recursos             | N                     |
| .NET Framework 3,5 (inclui o .NET 2,0 e 3,0)       | NET-Framework-Core                 | removido             |
| Ativação HTTP                                        | Ativação de NET-HTTP                | N                     |
| Ativação não HTTP                                    | NET-non-HTTP-ATI                 | N                     |
| Recursos do .NET Framework 4,6                            | NET-Framework-45-recursos          | S                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | S                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| Serviços WCF                                           | NET-WCF-Services45                 | S                     |
| Ativação HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Ativação do MSMQ (enfileiramento de mensagens)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Ativação de pipe nomeado                                  | NET-WCF-pipe-Activation45          | N                     |
| Ativação de TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Compartilhamento de porta TCP                                       | NET-WCF-TCP-PortSharing45          | S                     |
| Serviço de Transferência Inteligente em Segundo Plano (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-Server                | N                     |
| Criptografia de Unidade de Disco BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente NFS                                         | NFS-cliente                         | N                     |
| Contêineres                                             | Contêineres                         | N                     |
| Data Center Bridging                                   | Data Center-ponte               | N                     |
| Armazenamento Avançado                                       | EnhancedStorage                    | N                     |
| Clustering de failover                                    | Failover-clustering                | N                     |
| Gerenciamento de Política de Grupo                                | GPMC                               | N                     |
| Qualidade de Serviço de E/S                                 | DiskIo-QoS                         | N                     |
| Núcleo da Web Hospedável do IIS                                  | Web-WHC                            | N                     |
| Servidor de Gerenciamento de Endereço IP (IPAM)                    | IPAM                               | N                     |
| Serviço do iSNS Server                                    | ISNS                               | N                     |
| Extensão do IIS do Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-Media-Foundation            | N                     |
| Enfileiramento de Mensagens                                        | MSMQ                               | N                     |
| Serviços de enfileiramento de mensagens                               | Serviços MSMQ                      | N                     |
| Servidor de enfileiramento de mensagens                                 | Servidor MSMQ                        | N                     |
| Integração de Serviços de Diretório                          | Diretório MSMQ                     | N                     |
| Suporte a HTTP                                           | MSMQ-HTTP-support                  | N                     |
| Gatilhos de enfileiramento de mensagens                               | Gatilhos MSMQ                      | N                     |
| Serviço de roteamento                                        | Roteamento do MSMQ                       | N                     |
| Proxy DCOM do Serviço de Enfileiramento de Mensagens                             | MSMQ-DCOM                          | N                     |
| Multipath I/O                                          | Vários caminhos-e/s                       | N                     |
| MultiPoint Connector                                   | Conector do MultiPoint               | N                     |
| Serviços do conector do MultiPoint                          | MultiPoint-Connector-Services      | N                     |
| MultiPoint Manager e MultiPoint Dashboard            | MultiPoint-Tools                   | N                     |
| Balanceamento de Carga de Rede                                 | NLB                                | N                     |
| Protocolo PNRP                          | PNRP                               | N                     |
| Quality Windows Audio-Video Experience                 | qWave                              | N                     |
| Compactação Diferencial Remota                        | RDC                                | N                     |
| Ferramentas de Administração de Servidor Remoto                     | RSAT                               | N                     |
| Ferramentas de administração de recursos                           | RSAT-recurso-ferramentas                 | N                     |
| Utilitários de administração de Criptografia de Unidade de Disco BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Ferramentas LLDP DataCenterBridging                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Ferramentas de cluster de failover                              | RSAT-clustering                    | N                     |
| Módulo de cluster de failover para Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Servidor de automação de cluster de failover                     | RSAT-clustering-AutomationServer   | N                     |
| Interface de comando de cluster de failover                     | RSAT-clustering-CmdInterface       | N                     |
| Cliente IPAM (gerenciamento de endereços IP)                    | IPAM-cliente-recurso                | N                     |
| Ferramentas de VM blindadas                                      | RSAT-blindado-VM-Tools             | N                     |
| Módulo de réplica de armazenamento para Windows PowerShell          | RSAT-Storage-Replica               | N                     |
| Ferramentas de administração de funções                              | RSAT-função-ferramentas                    | N                     |
| Ferramentas AD DS e AD LDS                                 | RSAT-AD-Tools                      | N                     |
| Módulo do Active Directory para o Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Ferramentas de AD DS                                            | RSAT – ADICIONA                          | N                     |
| Centro Administrativo do Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Snap-ins AD DS e Ferrramentas de linha de comando                  | RSAT-ADDS – ferramentas                    | N                     |
| AD LDS snap-ins e ferramentas de linha de comando                 | RSAT-ADLDS                         | N                     |
| Ferramentas de Gerenciamento do Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Módulo Hyper-V para o Windows PowerShell                  | PowerShell do Hyper-V                 | N                     |
| Ferramentas de Windows Server Update Services                   | Updateservices-RSAT                | N                     |
| Cmdlets do PowerShell e API                             | Updateservices-API                 | N                     |
| Ferramentas de servidor DHCP                                      | RSAT-DHCP                          | N                     |
| Ferramentas de servidor DNS                                       | RSAT-DNS-Server                    | N                     |
| Ferramentas de gerenciamento de acesso remoto                         | RSAT-RemoteAccess                  | N                     |
| Módulo de Acesso Remoto para o Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| RPC sobre Proxy HTTP                                    | RPC via HTTP-proxy                | N                     |
| Coleta de Eventos de Instalação e Inicialização                        | Configuração-e-boot-Event-Collection    | N                     |
| Serviços TCP/IP Simples                                 | Simples-TCPIP                       | N                     |
| Suporte para Compartilhamento de Arquivos SMB 1.0/ CIFS                      | FS-SMB1                            | S                     |
| Limite de Largura de Banda do SMB                                    | FS-SMBBW                           | N                     |
| Serviço SNMP                                           | Serviço SNMP                       | N                     |
| Provedor WMI SNMP                                      | SNMP-WMI-provedor                  | N                     |
| Cliente Telnet                                          | Telnet-cliente                      | N                     |
| Ferramentas de Blindagem de VM para Gerenciamento de Malha               | FabricShieldedTools                | N                     |
| Recursos do Windows Defender                              | Windows-Defender-recursos          | S                     |
| Windows Defender                                       | Windows-Defender                   | S                     |
| Banco de Dados Interno do Windows                              | Windows-Internal-Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | S                     |
| Windows PowerShell 5,1                                 | PowerShell                         | S                     |
| Mecanismo 2,0 do Windows PowerShell                          | PowerShell-v2                      | removido             |
| Serviço de configuração de estado desejado do Windows PowerShell | Serviço DSC                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Serviço de Ativação de Processos do Windows                     | DEVERIA                                | N                     |
| Modelo de Processo                                          | WAS-processo-modelo                  | N                     |
| Ambiente .NET 3,5                                   | ERA NET-environment                | N                     |
| APIs de Configuração                                     | WAS-config-APIs                    | N                     |
| Backup do Windows Server                                  | Windows-Server-Backup              | N                     |
| Ferramentas de Migração do Windows Server                         | Migração                          | N                     |
| Gerenciamento de Armazenamento com base nos padrões do Windows             | WindowsStorageManagementService    | N                     |
| Extensão IIS WinRM                                    | WinRM-IIS-Ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Suporte a WoW64                                          | WoW64 – suporte                      | S                     |
|                                                        |                                    |                       |
