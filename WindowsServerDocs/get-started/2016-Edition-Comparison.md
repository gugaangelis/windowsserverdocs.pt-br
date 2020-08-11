---
title: Produtos e edições do Windows Server 2016
description: Explica as diferenças nas edições Windows Server Standard e Windows Server Datacenter
ms.date: 10/04/2019
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-a932-80cab33f419e
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 50d0c603e5134c716c50e3aa8286cb33578ee06e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959904"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2016"></a>Comparação das edições Standard e Datacenter do Windows Server 2016

> Aplica-se a: Windows Server 2016

## <a name="locks-and-limits"></a>Bloqueios e limites

| Bloqueios e limites | Windows Server 2016 Standard | Windows Server 2016 Datacenter |
| ------------------- |---------- | --------------------------- |
| Número máximo de usuários | Com base em CALs   | Com base em CALs     |
| Máximo de conexões SMB | 16.777.216      | 16.777.216          |
| Máximo de conexões RRAS| ilimitado       | ilimitado         |
| Máximo de conexões IAS | 2\.147.483.647   | 2\.147.483.647        |
| Máximo de conexões RDS | 65535           | 65535             |
| Número máximo de soquetes 64 bits | 64     | 64                |
| Número máximo de núcleos | ilimitado       | ilimitado      |
| RAM Máxima             | 24 TB           | 24 TB             |
| Pode ser usado como convidado para virtualização | Sim; 2 máquinas virtuais, além de um host do Hyper-V por licença | Sim; <strong>máquinas virtuais ilimitadas</strong>, além de um host do Hyper-V por licença |
| Servidor pode ingressar em um domínio | sim            | sim                |
| Proteção/firewall da rede de borda | não     | não                 |
| DirectAccess            | sim             | sim                |
| Codecs DLNA e streaming de mídia Web | Sim, se instalado como servidor com Experiência Desktop | Sim, se instalado como servidor com Experiência Desktop |

## <a name="server-roles"></a>Funções de servidor

| Funções do Windows Server disponíveis     | Serviços de função | Windows Server 2016 Standard | Windows Server 2016 Datacenter |
| -------------------                | ----------    | ----------                   | ---------------------------    |
| Serviços de Certificados do Active Directory|              | Sim                          | Sim                            |
| Active Directory Domain Services    |               | Sim                          | Sim                            |
| Serviços de Federação do Active Directory|               | Sim                          | Sim                            |
| AD Lightweight Directory Services| |Sim|Sim|
| AD Rights Management Services| |Sim|Sim|
| Atestado de Integridade do Dispositivo| |Sim|Sim|
| Servidor DHCP| |Sim|Sim|
| Servidor DNS| |Sim|Sim|
| Servidor de Fax| |Sim|Sim|
| Serviços de Arquivo e Armazenamento|Servidor de arquivos|Sim|Sim|
| Serviços de Arquivo e Armazenamento|BranchCache para arquivos de rede|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Eliminação de duplicação de dados|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Namespaces DFS|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Replicação do DFS|Sim|Sim|
| Serviços de Arquivo e Armazenamento|File Server Resource Manager|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Serviço de Agente VSS de Servidor de Arquivos|Sim|Sim|
| Serviços de Arquivo e Armazenamento|iSCSI Target Server|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Provedor de Armazenamento do destino iSCSI|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Servidor para NFS|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Pastas de trabalho|Sim|Sim|
| Serviços de Arquivo e Armazenamento|Serviços de armazenamento|Sim|Sim|
| Serviço Guardião de Host| |Sim|Sim|
| Hyper-V| |Sim|Sim; <strong>inclusive máquinas virtuais blindadas</strong>|
| MultiPoint Services| |Sim|Sim|
| Controlador de rede| |Não| <strong>Sim</strong> |
| Network Policy and Access Services| |Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
| Serviços de impressão e documentos| |Sim|Sim|
| Acesso remoto| |Sim|Sim|
| Serviços da área de trabalho Remota| |Sim|Sim|
| Serviços de Ativação por Volume| |Sim|Sim|
| Web Services (IIS)| |Sim|Sim|
| Windows Deployment Services| |Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
| Experiência do Windows Server Essentials| |Sim|Sim|
| Windows Server Update Services| |Sim|Sim|

## <a name="features"></a>Recursos

|Recursos do Windows Server instaláveis com Gerenciador de Servidores (ou PowerShell)|Windows Server 2016 Standard|Windows Server 2016 Datacenter|
|-------------------|----------|---------------------------|
|.NET Framework 3.5|Sim|Sim|
|.NET Framework 4.6|Sim|Sim|
|BITS|Sim|Sim|
|Criptografia de Unidade de Disco BitLocker|Sim|Sim|
|Desbloqueio de Rede do BitLocker|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|BranchCache|Sim|Sim|
|Cliente NFS|Sim|Sim|
|Contêineres|Sim (contêineres do Windows ilimitados; até 2 contêineres do Hyper-V)|Sim (todos os tipos de contêineres ilimitados)|
|Ponte de Data Center|Sim|Sim|
|Direct Play|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Armazenamento Avançado|Sim|Sim|
|Clustering de failover|Sim|Sim|
|Gerenciamento de Política de Grupo|Sim|Sim|
|Suporte do Hyper-V ao Guardião de Host|Não|<strong>Sim</strong> |
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
|Network Load Balancing|Sim|Sim|
|Protocolo PNRP|Sim|Sim|
|Quality Windows Audio-Video Experience|Sim|Sim|
|Kit de Administração do Gerenciador de Conexões|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Assistência Remota|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Compactação Diferencial Remota|Sim|Sim|
|RSAT|Sim|Sim|
|RPC sobre Proxy HTTP|Sim|Sim|
|Coleta de Eventos de Instalação e Inicialização|Sim|Sim|
|Serviços TCP/IP Simples|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Suporte para Compartilhamento de Arquivos SMB 1.0/ CIFS|Instalada|Instalada|
|Limite de Largura de Banda do SMB|Sim|Sim|
|Servidor SMTP|Sim|Sim|
|Serviço SNMP|Sim|Sim|
|Balanceador de Carga de Software|Não| <strong>Sim</strong> |
|Réplica de Armazenamento|Não|Sim|
|Cliente Telnet|Sim|Sim|
|Cliente TFTP|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Ferramentas de Blindagem de VM para Gerenciamento de Malha|Sim|Sim|
|Redirecionador WebDAV|Sim|Sim|
|Windows Biometric Framework|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Recursos do Windows Defender|Instalada|Instalada|
|Windows Identity Foundation 3.5|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Banco de Dados Interno do Windows|Sim|Sim|
|Usando o Windows PowerShell|Instalada|Instalada|
|Serviço de Ativação de Processos do Windows|Sim|Sim|
|Serviço Windows Search|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Backup do Windows Server|Sim|Sim|
|Ferramentas de Migração do Windows Server|Sim|Sim|
|Gerenciamento de Armazenamento Baseado em Padrões do Windows|Sim|Sim|
|Windows TIFF IFilter|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Extensão IIS WinRM|Sim|Sim|
|Servidor WINS|Sim|Sim|
|Serviço de LAN sem fio|Sim|Sim|
|Suporte a WoW64|Instalada|Instalada|
|Visualizador XPS|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|

|Recursos disponíveis em geral|Windows Server 2016 Standard|Windows Server 2016 Datacenter|
|-------------------|----------|---------------------------|
|Analisador de práticas recomendadas|Sim|Sim|
|Direct Access|Sim|Sim|
|Memória Dinâmica (em virtualização)|Sim|Sim|
|Adicionar/Substituir RAM a quente|Sim|Sim|
|Console de Gerenciamento Microsoft|Sim|Sim|
|Interface Mínima do Servidor|Sim|Sim|
|Network Load Balancing|Sim|Sim|
|Usando o Windows PowerShell|Sim|Sim|
|Opção de instalação do Server Core|Sim|Sim|
|Opção de instalação do Nano Server|Sim|Sim|
|Gerenciador do Servidor|Sim|Sim|
|SMB Direct e SMB sobre RDMA|Sim|Sim|
| Redes definidas por software | Não | <strong>Sim</strong> |
|Réplica de Armazenamento | Não | <strong>Sim</strong> |
|Espaços de Armazenamento|Sim|Sim|
|Espaços de Armazenamento Direct|Não| <strong>Sim</strong> |
|Serviços de Ativação por Volume|Sim|Sim|
|Integração VSS (Serviço de Cópias de Sombra de Volume)|Sim|Sim|
|Windows Server Update Services|Sim|Sim|
|Gerenciador de recursos de sistema do Windows|Sim|Sim|
|Log de Licença de Servidor|Sim|Sim|
|Ativação herdada|Como convidado, se hospedado no Datacenter| <strong>Pode ser host ou convidado</strong> |
|Pastas de trabalho|Sim|Sim|

