---
title: Comparação das edições Standard e Datacenter do Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-2932-80cab33fe914
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: ba7487a7e063775219182645a273d49c473f52e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854127"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2019"></a>Comparação das edições Standard e Datacenter do Windows Server 2019

> Aplica-se a: Windows Server 2019
  
## <a name="locks-and-limits"></a>Bloqueios e limites
|Bloqueios e limites|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|Número máximo de usuários|Com base em CALs|Com base em CALs|
|Máximo de conexões SMB|16777216|16777216|
|Máximo de conexões RRAS|ilimitado|ilimitado|
|Máximo de conexões IAS|2147483647|2147483647|
|Máximo de conexões RDS|65535|65535|
|Número máximo de soquetes 64 bits|64|64|
|Número máximo de núcleos|ilimitado|ilimitado|
|RAM Máxima|24 TB|24 TB|
|Pode ser usado como convidado para virtualização|Sim; 2 máquinas virtuais, além de um host do Hyper-V por licença|Sim; máquinas virtuais ilimitadas, além de um host do Hyper-V por licença|
|Servidor pode ingressar em um domínio|sim|sim|
|Proteção/firewall da rede de borda|não|não|
|DirectAccess|sim|sim|
|Codecs DLNA e streaming de mídia Web|Sim, se instalado como servidor com Experiência Desktop|Sim, se instalado como servidor com Experiência Desktop|

## <a name="server-roles"></a>Funções de servidor
|Funções do Windows Server disponíveis|Serviços de função|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
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
|MultiPoint Services| |Sim|Sim|
|Controlador de rede| |Não|Sim|
|Network Policy and Access Services| |Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Serviços de impressão e documentos| |Sim|Sim|
|Acesso remoto| |Sim|Sim|
|Serviços da Área de Trabalho Remota| |Sim|Sim|
|Serviços de Ativação por Volume| |Sim|Sim|
|Web Services (IIS)| |Sim|Sim|
|Serviços de Implantação do Windows| |Sim*|Sim*|
|Experiência do Windows Server Essentials| |Sim|Sim|
|Windows Server Update Services| |Sim|Sim|

* Servidor de transporte do WDS é novo para instalações do Server Core no Windows Server 2019 (também no canal semestral começando com o Windows Server, versão 1803)


## <a name="features"></a>Recursos

|Recursos do Windows Server instaláveis com Gerenciador de Servidores (ou PowerShell)|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|Sim|Sim|
|.NET Framework 4.6|Sim|Sim|
|Serviço de Transferência Inteligente em Segundo Plano (BITS)|Sim|Sim|
|Criptografia de Unidade de Disco BitLocker|Sim|Sim|
|Desbloqueio pela rede do BitLocker|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|BranchCache|Sim|Sim|
|Cliente NFS|Sim|Sim|
|Contêineres|Sim (contêineres do Windows ilimitados; Hyper-V contêineres até 2)|Sim (todos os tipos de contêineres ilimitados)|
|Data Center Bridging|Sim|Sim|
|Direct Play|Sim, quando instalado como servidor com Experiência Desktop|Sim, quando instalado como servidor com Experiência Desktop|
|Armazenamento Avançado|Sim|Sim|
|Clustering de failover|Sim|Sim|
|Gerenciamento de Política de Grupo|Sim|Sim|
|Suporte do Hyper-V ao Guardião de Host|Não|Sim|
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
|Réplica de Armazenamento|Não|Sim|
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

|Recursos disponíveis em geral|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|Analisador de Práticas Recomendadas|Sim|Sim|
|Réplica de armazenamento restrita|Sim, (1 parceria e 1 grupo de recursos com volume único de 2TB)|Sim, ilimitado|
|Direct Access|Sim|Sim|
|Memória Dinâmica (em virtualização)|Sim|Sim|
|Adicionar/Substituir RAM a quente|Sim|Sim|
|Console de Gerenciamento Microsoft|Sim|Sim|
|Interface Mínima do Servidor|Sim|Sim|
|Balanceamento de Carga de Rede|Sim|Sim|
|Windows PowerShell|Sim|Sim|
|Opção de instalação do Server Core|Sim|Sim|
|Opção de instalação do Nano Server|Sim|Sim|
|Gerenciador do Servidor|Sim|Sim|
|SMB Direct e SMB sobre RDMA|Sim|Sim|
|Rede definida por software|Não|Sim|
|Serviço de Gerenciamento de Armazenamento|Sim|Sim|
|Espaços de Armazenamento|Sim|Sim|
|Espaços de Armazenamento Diretos|Não|Sim|
|Serviços de Ativação por Volume|Sim|Sim|
|Integração VSS (Serviço de Cópias de Sombra de Volume)|Sim|Sim|
|Windows Server Update Services|Sim|Sim|
|Gerenciador de Recursos de Sistema do Windows|Sim|Sim|
|Log de Licença de Servidor|Sim|Sim|
|Ativação herdada|Como convidado, se hospedado no Datacenter|Pode ser host ou convidado|
|Pastas de trabalho|Sim|Sim|

