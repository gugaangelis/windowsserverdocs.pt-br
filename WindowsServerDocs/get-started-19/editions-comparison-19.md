---
title: Comparação das edições Standard e Datacenter do Windows Server 2019
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-2932-80cab33fe914
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 2cb368b5ff4723c1afb53c5a9e787d9f8a39bef3
ms.sourcegitcommit: 40e4ba214954d198936341c4d6ce1916dc891169
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690454"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2019"></a>Comparação das edições Standard e Datacenter do Windows Server 2019

> Aplica-se a: Windows Server 2019
  
## <a name="locks-and-limits"></a>Bloqueios e limites

| Bloqueios e limites                 | Windows Server 2019 Standard | Windows Server 2019 Datacenter |  
| -------------------              | ---------------------------  | --------------------------- |  
| Número máximo de usuários          | Com base em CALs                | Com base em CALs |
| Máximo de conexões SMB          | 16.777.216                   | 16.777.216 |
| Máximo de conexões RRAS         | ilimitado                    | ilimitado |
| Máximo de conexões IAS          | 2\.147.483.647                | 2\.147.483.647 |
| Máximo de conexões RDS          | 65.535                       | 65.535 |
| Número máximo de soquetes 64 bits | 64                           | 64 |
| Número máximo de núcleos          | ilimitado                    | ilimitado |
| RAM Máxima                      | 24 TB                        | 24 TB |
| Pode ser usado como convidado para virtualização | Sim; 2 máquinas virtuais, além de um host do Hyper-V por licença|Sim; <strong>máquinas virtuais ilimitadas</strong>, além de um host do Hyper-V por licença |
| Servidor pode ingressar em um domínio        | sim                           | sim |
| Proteção/firewall da rede de borda| não                            | não    |
| DirectAccess                    | sim                           | sim |
| Codecs DLNA e streaming de mídia Web | Sim, se instalado como servidor com Experiência Desktop | Sim, se instalado como servidor com Experiência Desktop |

## <a name="server-roles"></a>Funções de servidor

|Funções do Windows Server disponíveis|Serviços de função|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|----------|---------------------------|  
|Serviços de Certificados do Active Directory| |Sim|Sim|
|Active Directory Domain Services| |Sim|Sim|
|Serviços de Federação do Active Directory (AD FS)| |Sim|Sim|
|AD Lightweight Directory Services| |Sim|Sim|
|AD Rights Management Services| |Sim|Sim|
|Atestado de integridade de dispositivo| |Sim|Sim|
|Servidor DHCP| |Sim|Sim|
|Servidor DNS| |Sim|Sim|
|Servidor de Fax| |Sim|Sim|
|Serviços de Arquivo e Armazenamento|Servidor de arquivos|Sim|Sim|
|Serviços de Arquivo e Armazenamento|BranchCache para arquivos de rede|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Eliminação de Duplicação de Dados|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Namespaces DFS|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Replicação do DFS|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Gerenciador de Recursos de Servidor de Arquivos|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Serviço de Agente VSS de Servidor de Arquivos|Sim|Sim|
|Serviços de Arquivo e Armazenamento|iSCSI Target Server|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Provedor de Armazenamento do destino iSCSI|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Servidor para NFS|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Pastas de trabalho|Sim|Sim|
|Serviços de Arquivo e Armazenamento|Serviços de armazenamento|Sim|Sim|
|Serviço Guardião de Host| |Sim|Sim|
|Hyper-V| |Sim|Sim; inclusive máquinas virtuais blindadas|
|Controlador de rede| |Não| <strong>Sim</strong> |
|Network Policy and Access Services| |Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Serviços de impressão e documentos| |Sim|Sim|
|Acesso remoto| |Sim|Sim|
|Serviços da Área de Trabalho Remota| |Sim|Sim|
|Serviços de Ativação por Volume| |Sim|Sim|
|Web Services (IIS)| |Sim|Sim|
|Serviços de Implantação do Windows| |Sim*|Sim*|
|Experiência do Windows Server Essentials| |Não | Não |
|Windows Server Update Services| |Sim|Sim|

*O Servidor de Transporte WDS é novo para instalações do Server Core no Windows Server 2019 (também no canal semestral a partir do Windows Server, versão 1803)


## <a name="features"></a>Recursos

|Recursos do Windows Server instaláveis com Gerenciador de Servidores (ou PowerShell)|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5 |Sim|Sim|
|.NET Framework 4.7 |Sim|Sim|
|Serviço de Transferência Inteligente em Segundo Plano (BITS)|Sim|Sim|
|Criptografia de Unidade de Disco BitLocker|Sim|Sim|
|Desbloqueio pela rede do BitLocker|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|BranchCache|Sim|Sim|
|Cliente NFS|Sim|Sim|
|Contêineres|Sim (contêineres do Windows ilimitados; até dois contêineres do Hyper-V)|Sim (<strong>contêineres do Windows ilimitados e do Hyper-V</strong>) |
|Data Center Bridging|Sim|Sim|
|Direct Play|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Armazenamento Avançado|Sim|Sim|
|Clustering de failover|Sim|Sim|
|Gerenciamento de Política de Grupo|Sim|Sim|
|Suporte do Hyper-V ao Guardião de Host|Não| <strong>Sim</strong> |
|Qualidade de Serviço de E/S|Sim|Sim|
|Núcleo da Web Hospedável do IIS|Sim|Sim|
|Cliente de Impressão via Internet|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Servidor IPAM|Sim|Sim|
|Serviço do iSNS Server|Sim|Sim|
|Monitor de Porta LPR|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Extensão do IIS do Management OData|Sim|Sim|
|Media Foundation|Sim|Sim|
|Message Queueing|Sim|Sim|
|Multipath I/O|Sim|Sim|
|MultiPoint Connector|Sim|Sim|
|Balanceamento de Carga de Rede|Sim|Sim|
|Protocolo PNRP|Sim|Sim|
|Quality Windows Audio-Video Experience|Sim|Sim|
|Kit de Administração do Gerenciador de Conexões|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Assistência Remota|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Compactação Diferencial Remota|Sim|Sim|
|RSAT|Sim|Sim|
|RPC sobre Proxy HTTP|Sim|Sim|
|Coleta de Eventos de Instalação e Inicialização|Sim|Sim|
|Serviços TCP/IP Simples|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Suporte para Compartilhamento de Arquivos SMB 1.0/ CIFS|Instalado|Instalado|
|Limite de Largura de Banda do SMB|Sim|Sim|
|Servidor SMTP|Sim|Sim|
|Serviço SNMP|Sim|Sim|
|Balanceador de Carga de Software|Sim|Sim|
|Réplica de Armazenamento|Sim|Sim|
|Cliente Telnet|Sim|Sim|
|Cliente TFTP|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Ferramentas de Blindagem de VM para Gerenciamento de Malha|Sim|Sim|
|Redirecionador WebDAV|Sim|Sim|
|Windows Biometric Framework|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Recursos do Windows Defender|Instalado|Instalado|
|Windows Identity Foundation 3.5|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Banco de Dados Interno do Windows|Sim|Sim|
|Windows PowerShell|Instalado|Instalado|
|Serviço de Ativação de Processos do Windows|Sim|Sim|
|Serviço Windows Search|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Backup do Windows Server|Sim|Sim|
|Ferramentas de Migração do Windows Server|Sim|Sim|
|Gerenciamento de Armazenamento com base nos padrões do Windows|Sim|Sim|
|Windows TIFF IFilter|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Extensão IIS WinRM|Sim|Sim|
|Servidor WINS|Sim|Sim|
|Serviço de LAN sem fio|Sim|Sim|
|Suporte a WoW64|Instalado|Instalado|
|Visualizador XPS|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|

|Recursos disponíveis em geral|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|Analisador de Práticas Recomendadas|Sim|Sim|
|Direct Access|Sim|Sim|
|Memória Dinâmica (em virtualização)|Sim|Sim|
|Adicionar/Substituir RAM a quente|Sim|Sim|
|Console de Gerenciamento Microsoft|Sim|Sim|
|Interface Mínima do Servidor|Sim|Sim|
|Balanceamento de Carga de Rede|Sim|Sim|
|Windows PowerShell|Sim|Sim|
|Opção de instalação do Server Core|Sim|Sim|
|Gerenciador do Servidor|Sim|Sim|
|SMB Direct e SMB sobre RDMA|Sim|Sim|
|Redes definidas por software|Não| <strong>Sim</strong> |
|Serviço de Migração de Armazenamento|Sim|Sim|
| Réplica de Armazenamento         | Sim, (1 parceria e 1 grupo de recursos com volume único de 2 TB)    | Sim, <strong>ilimitado</strong> |
|Espaços de Armazenamento|Sim|Sim|
|Espaços de Armazenamento Diretos|Não| <strong>Sim</strong> |
|Serviços de Ativação por Volume|Sim|Sim|
|Integração VSS (Serviço de Cópias de Sombra de Volume)|Sim|Sim|
|Windows Server Update Services|Sim|Sim|
|Gerenciador de Recursos de Sistema do Windows|Sim|Sim|
|Log de Licença de Servidor|Sim|Sim|
|Ativação herdada|Como convidado, se hospedado no Datacenter| <strong>Pode ser um host ou um convidado</strong> |
|Pastas de trabalho|Sim|Sim|
