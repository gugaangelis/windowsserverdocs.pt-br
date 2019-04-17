---
title: Funções, serviços de função e recursos incluídos no Windows Server - núcleo do servidor
description: Quais funções e recursos estão incluídos na opção de instalação Server Core do Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 439edac95e1427086268cab469ed03b94db630da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "2217736"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Funções, serviços de função e recursos incluídos no Windows Server - núcleo do servidor

> Aplica-se a: Windows Server (canal delimitadas anual) e Windows Server 2016

Geralmente falamos sobre [quais do *não* no Server Core](server-core-removed-roles.md) - agora vamos tentar uma abordagem diferente e informar o que é *incluído* e se algo é *instalado por padrão*. As seguintes funções, serviços de função e recursos estão *na* opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para seu ambiente. Como esta é uma lista grande, considere a possibilidade de pesquisar a função específica ou o recurso que você está interessado em - se de que a pesquisa não retorna o que você está procurando não está incluído no núcleo do servidor.

Por exemplo, se você procurar por "Host de sessão de área de trabalho remota -" você não encontrará ele nesta página. Isso acontece porque o Host de sessão RD não está incluído na imagem do núcleo do servidor.

Lembre-se de que você pode [Procurar sempre](server-core-removed-roles.md) no que *não* está incluído. Isso é apenas uma maneira diferente a ser analisado coisas.

## <a name="roles-included-in-server-core"></a>Funções incluídas no núcleo do servidor
A opção de instalação Server Core inclui as seguintes funções de servidor.

| Função                                            | Nome                           | Instalado por padrão? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Serviços de Certificados do Active Directory           | Certificado do AD                 | N                     |
| Active Directory Domain Services                | Serviços de domínio do AD             | N                     |
| Serviços de Federação do Active Directory            | Federação do ADFS                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Atestado de integridade do dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Serviços de Arquivo e Armazenamento                       | Serviços de FileAndStorage        | Y                     |
| Serviço Guardião de Host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Serviços de impressão e documentos                     | Serviços de impressão                 | N                     |
| Acesso remoto                                   | Acesso remoto                   | N                     |
| Serviços de Área de Trabalho Remota                         | Serviços de área de trabalho remota        | N                     |
| Serviços de ativação de volume                      | VolumeActivation               | N                     |
| Servidor Web IIS                                  | Servidor Web                     | N                     |
| Experiência do Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Serviços de função incluídos no núcleo do servidor
A opção de instalação Server Core inclui os seguintes serviços de função.

| Função                                  | Serviço de função                                                   | Nome                    | Instalado por padrão? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Serviços de Certificados do Active Directory | Autoridade de certificação                                        | Autoridade de Cert DACS     | N                     |
|                                       | Serviço de Web de diretiva de registro de certificado                      | DACS-registrar-Web-Pol     | N                     |
|                                       | Serviço de Web de registro de certificado                             | DACS-registrar-Web-Svc     | N                     |
|                                       | Registro de Web de autoridade de certificação                         | Registro de Web DACS     | N                     |
|                                       | Serviço de registro do dispositivo de rede                              | Registro do dispositivo de DACS  | N                     |
|                                       | Respondente Online                                               | DACS-Online-Cert.        | N                     |
| Gerenciamento de direitos do Active Directory    | Servidor de Gerenciamento de Direitos do Active Directory                      | ADRMS-servidor            | N                     |
|                                       | Suporte de federação de identidade                                    | ADRMS-Identity          | N                     |
| Serviços de Arquivo e Armazenamento             | Serviços de arquivos e iSCSI                                        | Serviços de arquivo           | N                     |
|                                       | Servidor de arquivos                                                    | FS-servidor de arquivos           | N                     |
|                                       | BranchCache para arquivos de rede                                  | FS-BranchCache          | N                     |
|                                       | Eliminação de Duplicação de Dados                                             | FS-dados-eliminação da duplicação   | N                     |
|                                       | Namespaces DFS                                                 | Namespace de FS-DFS        | N                     |
|                                       | Replicação do DFS                                                | Replicação de FS-DFS      | N                     |
|                                       | Gerenciador de Recursos de Servidor de Arquivos                                   | Gerente de recursos FS     | N                     |
|                                       | Serviço de Agente VSS de Servidor de Arquivos                                  | Agente de FS-VSS            | N                     |
|                                       | iSCSI Target Server                                            | Servidor iSCSITarget      | N                     |
|                                       | o provedor de armazenamento de destino (provedores de hardware VDS e VSS) iSCSI | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | FS-NFS-Service          | N                     |
|                                       | Pastas de trabalho                                                   | FS-SyncShareService     | N                     |
|                                       | Serviços de armazenamento                                               | Serviços de armazenamento        | Y                     |
| Serviços de impressão e documentos           | Servidor de impressão                                                   | Servidor de impressão            | N                     |
|                                       | Serviço LDP                                                    | Impressão-LPD-Service       | N                     |
| Acesso remoto                         | DirectAccess e VPN (RAS)                                     | DirectAccess VPN        | N                     |
|                                       | Roteamento                                                        | Roteamento                 | N                     |
|                                       | Proxy de aplicativo Web                                          | Proxy de aplicativo Web   | N                     |
| Serviços de Área de Trabalho Remota               | Agente de Conexão de área de trabalho remota                               | Agente de Conexão RDS   | N                     |
|                                       | Licenciamento de área de trabalho remota                                       | Licenciamento do RDS           | N                     |
|                                       | Host de virtualização de área de trabalho remota                             | Virtualização do RDS      | N                     |
| Servidor Web (IIS)                      | Servidor Web                                                     | WebServer da Web           | N                     |
|                                       | Recursos HTTP comuns                                           | Comum-Web-Http         | N                     |
|                                       | Documento padrão                                               | Padrão-Web-Doc         | N                     |
|                                       | Pesquisa no diretório                                             | Navegação da Web Dir        | N                     |
|                                       | Erros de HTTP                                                    | Erros de Http de Web         | N                     |
|                                       | Conteúdo estático                                                 | Conteúdo da Web-estático      | N                     |
|                                       | Redirecionamento de HTTP                                               | Redirecionamento de Http de Web       | N                     |
|                                       | Publicação WebDAV                                              | Publicação na Web-DAV      | N                     |
|                                       | Integridade e diagnóstico                                         | Integridade da Web              | N                     |
|                                       | Log HTTP                                                   | Log de Http de Web        | N                     |
|                                       | Log personalizado                                                 | Log do Web personalizado      | N                     |
|                                       | Ferramentas de log                                                  | Bibliotecas de Log da Web       | N                     |
|                                       | Log de ODBC                                                   | Log de ODBC da Web        | N                     |
|                                       | Monitor de solicitações                                              | Monitor de solicitações da Web     | N                     |
|                                       | Rastreamento                                                        | Rastreamento de Http Web        | N                     |
|                                       | Desempenho                                                    | Desempenho da Web         | N                     |
|                                       | Compactação de conteúdo estático                                     | Compactação de Stat Web    | N                     |
|                                       | Compactação de conteúdo dinâmico                                    | Compactação de DIN Web     | N                     |
|                                       | Segurança                                                       | Segurança da Web            | N                     |
|                                       | Filtragem de solicitações                                              | Filtragem de Web           | N                     |
|                                       | Autenticação básica                                           | Basic-Web-autenticação          | N                     |
|                                       | Suporte do certificado SSL centralizado                            | CertProvider da Web        | N                     |
|                                       | Autenticação de mapeamento de certificado de cliente                      | Autenticação de cliente da Web         | N                     |
|                                       | Autenticação Digest                                          | Autenticação de Digest da Web         | N                     |
|                                       | Autenticação de mapeamento de certificado de cliente do IIS                  | Autenticação de certificado de Web           | N                     |
|                                       | Restrições de domínio e de IP                                     | Segurança de IP de Web         | N                     |
|                                       | Autorização de URL                                              | Autenticação de Url da Web            | N                     |
|                                       | Autenticação do Windows                                         | Autenticação do Windows de Web        | N                     |
|                                       | Desenvolvimento de aplicativo                                        | Desenvolvimento de aplicativos Web             | N                     |
|                                       | Extensibilidade do .NET 3.5                                         | Net-Web-Ext             | N                     |
|                                       | Extensibilidade .NET 4.6                                         | Net-Web-Ext45           | N                     |
|                                       | Inicialização de aplicativo                                     | AppInit da Web             | N                     |
|                                       | ASP                                                            | Web ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web Asp-dentro da rede             | N                     |
|                                       | ASP.NET 4.6                                                    | Asp-Web-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensões ISAPI                                               | Extensão ISAPI da Web           | N                     |
|                                       | Filtros ISAPI                                                  | Filtros ISAPI da Web        | N                     |
|                                       | Inclusões do servidor                                           | Inclui a Web            | N                     |
|                                       | Protocolo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Servidor de Web Ftp          | N                     |
|                                       | Serviço FTP                                                    | Serviço Web de Ftp         | N                     |
|                                       | Extensibilidade FTP                                              | Ext-Web-Ftp             | N                     |
|                                       | Ferramentas de gerenciamento                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilidade de Gerenciamento do IIS 6                                 | Web-Mgmt-Compat         | N                     |
|                                       | Compatibilidade de 6 de Metabase do IIS                                   | Metabase da Web            | N                     |
|                                       | Ferramentas de script 6 do IIS                                          | Web Lgcy Scripting      | N                     |
|                                       | Compatibilidade do IIS 6 WMI                                        | Web-WMI                 | N                     |
|                                       | Ferramentas e Scripts de gerenciamento do IIS                               | Ferramentas de script da Web     | N                     |
|                                       | Serviço de gerenciamento                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Conectividade WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Serviços do WSUS                                                  | Serviços de UpdateServices | N                     |
|                                       | Conectividade do SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Recursos incluídos no núcleo do servidor
A opção de instalação Server Core inclui os seguintes recursos.

| Recurso                                                | Nome                               | Instalado por padrão? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Recursos do .NET framework 3.5                            | Recursos do NET Framework             | N                     |
| .NET framework 3.5 (inclui .NET 2.0 e 3.0)       | NET-Framework-Core                 | (removido)             |
| Ativação de HTTP                                        | NET-ativação de HTTP                | N                     |
| Ativação de não-HTTP                                    | NET-não-HTTP-ativar                 | N                     |
| Recursos do .NET framework 4.6                            | NET Framework-45 recursos          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4.6                                            | NET Framework-45 ASPNET            | N                     |
| Serviços WCF                                           | NET-WCF-Services45                 | Y                     |
| Ativação de HTTP                                        | NET-WCF HTTP Activation45          | N                     |
| Ativação de enfileiramento de mensagens (MSMQ) de mensagem                      | NET-WCF MSMQ Activation45          | N                     |
| Ativação de Pipe nomeado                                  | NET-WCF Pipe Activation45          | N                     |
| Ativação de TCP                                         | NET-WCF TCP Activation45           | N                     |
| Compartilhamento de porta TCP                                       | NET-WCF TCP PortSharing45          | Y                     |
| Serviço de Transferência Inteligente em Segundo Plano (BITS)         | BITS                               | N                     |
| Compact Server                                         | Servidor de CD de BITS                | N                     |
| Criptografia de Unidade de Disco BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente NFS                                         | Cliente NFS                         | N                     |
| Contêineres                                             | Contêineres                         | N                     |
| Ponte de Data Center                                   | Ponte de centro de dados               | N                     |
| Armazenamento Avançado                                       | EnhancedStorage                    | N                     |
| Clustering de failover                                    | Cluster de failover                | N                     |
| Gerenciamento de Política de Grupo                                | GPMC                               | N                     |
| Qualidade de Serviço de E/S                                 | DiskIo-QoS                         | N                     |
| Núcleo da Web Hospedável do IIS                                  | WHC da Web                            | N                     |
| Servidor de gerenciamento (IPAM) de endereço IP                    | IPAM                               | N                     |
| Serviço do iSNS Server                                    | ISNS                               | N                     |
| Extensão do IIS do Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Núcleo de mídia do servidor            | N                     |
| Enfileiramento de mensagens                                        | MSMQ                               | N                     |
| Serviços de enfileiramento de mensagens                               | Serviços do MSMQ                      | N                     |
| Servidor de enfileiramento de mensagens                                 | MSMQ-Server                        | N                     |
| Integração de serviços de diretório                          | MSMQ-Directory                     | N                     |
| Suporte de HTTP                                           | Suporte a MSMQ-HTTP                  | N                     |
| Disparadores de enfileiramento de mensagem                               | Disparadores do MSMQ                      | N                     |
| Serviço de roteamento                                        | Roteamento de MSMQ                       | N                     |
| Proxy DCOM enfileiramento de mensagem                             | DCOM MSMQ                          | N                     |
| Multipath I/O                                          | E/s de vários caminhos                       | N                     |
| MultiPoint Connector                                   | Conector de multiponto               | N                     |
| Serviços do conector de multiponto                          | Serviços do conector de multiponto      | N                     |
| Gerente de multiponto e painel de controle de multipontos            | Ferramentas de multiponto                   | N                     |
| Balanceamento de Carga de Rede                                 | NLB                                | N                     |
| Protocolo PNRP                          | PNRP                               | N                     |
| Quality Windows Audio-Video Experience                 | qWave                              | N                     |
| Compactação Diferencial Remota                        | RDC                                | N                     |
| Ferramentas de Administração de Servidor Remoto                     | RSAT                               | N                     |
| Ferramentas de administração do recurso                           | Ferramentas de recurso RSAT                 | N                     |
| Utilitários de administração de criptografia de unidade de disco BitLocker  | Recurso RSAT-BitLocker ferramentas       | N                     |
| Ferramentas de DataCenterBridging LLDP                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Ferramentas de cluster de failover                              | Agrupamento de RSAT                    | N                     |
| Módulo de Cluster de failover do Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Servidor de automação do Cluster de failover                     | AutomationServer Clustering RSAT   | N                     |
| Interface de comando do Cluster de failover                     | CmdInterface Clustering RSAT       | N                     |
| Cliente de gerenciamento (IPAM) de endereço IP                    | Recurso do IPAM-cliente                | N                     |
| Ferramentas de blindado VM                                      | RSAT-livre-VM-Tools             | N                     |
| Módulo de réplica de armazenamento para Windows PowerShell          | RSAT-armazenamento de réplica               | N                     |
| Ferramentas de administração de função                              | Ferramentas de função RSAT                    | N                     |
| Ferramentas de AD LDS e AD DS                                 | Ferramentas de AD RSAT                      | N                     |
| Módulo do Active Directory para o Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Ferramentas do AD DS                                            | ADICIONA O RSAT                          | N                     |
| Centro Administrativo do Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Snap-Ins do AD DS e ferramentas de linha de comando                  | Ferramentas ADICIONA RSAT                    | N                     |
| Ferramentas de linha de comando e Snap-Ins do AD LDS                 | RSAT-ADLDS                         | N                     |
| Ferramentas de gerenciamento do Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Módulo do Hyper-V para o Windows PowerShell                  | PowerShell do Hyper-V                 | N                     |
| Ferramentas do Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| Cmdlets do PowerShell e de API                             | API de UpdateServices                 | N                     |
| Ferramentas do servidor DHCP                                      | RSAT-DHCP                          | N                     |
| Ferramentas do servidor DNS                                       | Servidor de DNS RSAT                    | N                     |
| Ferramentas de gerenciamento de acesso remoto                         | RSAT-acesso remoto                  | N                     |
| Módulo de acesso remoto para Windows PowerShell            | Acesso remoto-RSAT-PowerShell       | N                     |
| RPC sobre Proxy HTTP                                    | Proxy RPC-over-HTTP                | N                     |
| Coleta de Eventos de Instalação e Inicialização                        | Instalação e-inicialização-evento-conjunto    | N                     |
| Serviços TCP/IP Simples                                 | Simples-TCPIP                       | N                     |
| Suporte para Compartilhamento de Arquivos SMB 1.0/ CIFS                      | FS-SMB1                            | Y                     |
| Limite de Largura de Banda do SMB                                    | FS-SMBBW                           | N                     |
| Serviço SNMP                                           | Serviço SNMP                       | N                     |
| Provedor de WMI do SNMP                                      | Provedor de WMI do SNMP                  | N                     |
| Cliente Telnet                                          | Cliente Telnet                      | N                     |
| Ferramentas de Blindagem de VM para Gerenciamento de Malha               | FabricShieldedTools                | N                     |
| Recursos do Windows Defender                              | Recursos do Windows Defender          | Y                     |
| Windows Defender                                       | O Windows Defender                   | Y                     |
| Banco de Dados Interno do Windows                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| O Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Mecanismo do Windows PowerShell 2.0                          | PowerShell V2                      | (removido)             |
| Serviço de estado de configuração de desejado do Windows PowerShell | DSC-Service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Serviço de Ativação de Processos do Windows                     | FOI                                | N                     |
| Modelo de processo                                          | FOI-modelo de processo                  | N                     |
| Ambiente .NET 3.5                                   | FOI-NET-Environment                | N                     |
| APIs de configuração                                     | FOI-Config-APIs                    | N                     |
| Backup do Windows Server                                  | Backup do Windows Server              | N                     |
| Ferramentas de Migração do Windows Server                         | Migração                          | N                     |
| Gerenciamento de Armazenamento com base nos padrões do Windows             | WindowsStorageManagementService    | N                     |
| Extensão IIS WinRM                                    | WinRM-IIS-Ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Suporte de WoW64                                          | Suporte a WoW64                      | Y                     |
|                                                        |                                    |                       |
