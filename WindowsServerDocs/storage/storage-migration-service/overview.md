---
Title: Visão geral do serviço de migração de armazenamento
description: Serviço de migração de armazenamento facilita a migração de servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário de dados em servidores e, em seguida, transfere os dados e a configuração para servidores mais recentes — tudo sem aplicativos ou usuários que têm alterar nada.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 05/21/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: fd99058036a5b8041e4c65ca120c6a7e68b2df8d
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976655"
---
# <a name="storage-migration-service-overview"></a>Visão geral do serviço de migração de armazenamento

>Aplica-se a: 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canal semestral) do Windows Server

Serviço de migração de armazenamento facilita a migração de servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário de dados em servidores e, em seguida, transfere os dados e a configuração para servidores mais recentes — tudo sem aplicativos ou usuários que têm alterar nada.

Este tópico discute por que você iria querer usar o serviço de migração de armazenamento, como funciona o processo de migração e quais são os requisitos para servidores de origem e destino.

## <a name="why-use-storage-migration-service"></a>Por que usar o serviço de migração de armazenamento

Use o serviço de migração de armazenamento porque você tem um servidor (ou muitos servidores) que você deseja migrar para o hardware ou máquinas virtuais mais recentes. Serviço de migração de armazenamento é projetado para ajudá-lo fazendo o seguinte:

- Vários servidores e seus dados de inventário
- Transferência rápida de arquivos, compartilhamentos de arquivos e configuração de segurança de servidores de origem
- Opcionalmente, assumir a identidade dos servidores de origem (também conhecido como cutting sobre) para que os usuários e aplicativos não precisam alterar nada para acessar os dados existentes
- Gerenciar uma ou várias migrações da interface do usuário Windows Admin Center

![Diagrama mostrando a migração de arquivos do serviço de migração de armazenamento & configuração de servidores de origem para servidores de destino, as VMs do Azure ou a sincronização de arquivos do Azure.](media\overview\storage-migration-service-diagram.png)

**Figura 1: Fontes de serviço de migração de armazenamento e destinos**

## <a name="how-the-migration-process-works"></a>Como funciona o processo de migração

A migração é um processo de três etapas:

1. **Inventário de servidores** para coletar informações sobre seus arquivos e configuração (mostrada na Figura 2).
2. **Transferência de dados (cópia)** dos servidores de origem para os servidores de destino.
3. **Transferir para os novos servidores** (opcional).<br>Os servidores de destino supor que a fonte de identidades antiga de servidores para que aplicativos e usuários não precisem alterar nada. <br>Os servidores de origem entram em um estado de manutenção em que eles ainda contêm os mesmos arquivos que eles sempre têm (nós nunca remover arquivos dos servidores de origem), mas não estão disponíveis para usuários e aplicativos. Em seguida, você pode encerrar os servidores em sua conveniência.

![Captura de tela mostrando um servidor pronto para ser examinado](media/migrate/inventory.png)
**Figura 2: Inventário de servidores de serviço de migração de armazenamento**

## <a name="requirements"></a>Requisitos

Para usar o serviço de migração de armazenamento, você precisa do seguinte:

- Um **servidor de origem** para migrar arquivos e dados de
- Um **servidor de destino** executando o Windows Server 2019 para migrar para o – Windows Server 2016 e Windows Server 2012 R2 funcionar bem, mas são cerca de 50% mais lento
- Uma **servidor orchestrator** executando o Windows Server 2019 para gerenciar a migração  <br>Se você estiver migrando apenas alguns servidores e um dos servidores estiver executando o Windows Server 2019, você pode usar que como o orchestrator. Se você estiver migrando mais servidores, é recomendável usar um servidor separado do orchestrator.
- Um **PC ou servidor que executa [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)**  para executar a interface do usuário do serviço de migração de armazenamento, a menos que você prefere usar o PowerShell para gerenciar a migração. A versão do Windows Admin Center e Windows Server 2019 deve ser pelo menos versão 1809.

É altamente recomendável que o orchestrator e o destino computadores têm pelo menos dois núcleos ou duas vCPUs e pelo menos 2 GB de memória. Operações de inventário e de transferência são significativamente mais rápidas com mais processadores e memória.

### <a name="security-requirements"></a>Requisitos de segurança

- Uma conta de migração que seja um administrador nos computadores de origem e o computador do orchestrator.
- Uma conta de migração que seja um administrador nos computadores de destino e o computador do orchestrator.
- O computador do orchestrator deve ter a regra de firewall (SMB-entrada) de compartilhamento de arquivo e impressora habilitada *entrada*.
- Os computadores de origem e de destino devem ter as seguintes regras de firewall habilitadas *entrada* (embora talvez você já tenha habilitado):
  - Compartilhamento de arquivos e de impressora (SMB-Entrada)
  - Serviço Netlogon (NP-In)
  - Instrumentação de gerenciamento do Windows (DCOM-In)
  - Instrumentação de Gerenciamento do Windows (WMI-In)
  
  > [!TIP]
  > Instalar o serviço de Proxy de serviço de migração de armazenamento em um computador Windows Server 2019 automaticamente abre as portas de firewall necessárias nesse computador.
- Se os computadores pertencem a um domínio do Active Directory Domain Services, eles devem todas pertencem à mesma floresta. O servidor de destino também deve ser no mesmo domínio que o servidor de origem, se você quiser transferir o nome de domínio da origem para o destino ao recortar sobre. Substituição tecnicamente funciona entre domínios, mas o nome de domínio totalmente qualificado do destino será diferente da origem...

### <a name="requirements-for-source-servers"></a>Requisitos para servidores de origem

O servidor de origem deve executar um dos seguintes sistemas operacionais:

- Windows Server, o canal semestral
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

Se o orquestrador executa o Windows Server, versão 1903 ou posterior, você pode migrar os seguintes tipos de origem adicionais:

- Clusters de failover
- Servidores Linux que usam o Samba. Testamos o seguinte:
    - RedHat Enterprise Linux 7.6, CentOS 7, Debian 8, Ubuntu 16.04 and 12.04.5, SUSE Linux Enterprise Server (SLES) 11 SP4
    - Samba 4.x, 3.6.x

### <a name="requirements-for-destination-servers"></a>Requisitos para servidores de destino

O servidor de destino deve executar um dos seguintes sistemas operacionais:

- Windows Server, o canal semestral
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Servidores de destino executando o Windows Server 2019 ou o Windows Server, canal semestral versão 1809 ou posterior têm o dobro do desempenho de transferência de versões anteriores do Windows Server. Esse aumento de desempenho é devido à inclusão de um serviço de proxy de serviço de migração de armazenamento interno, que também abre o firewall necessárias portas abertas se eles não ainda.

## <a name="whats-new-in-storage-migration-service"></a>O que há de novo no serviço de migração de armazenamento

Windows Server, versão 1903 adiciona os seguintes recursos novos, quando executados no servidor do orchestrator:

- Migrar usuários e grupos locais para o novo servidor
- Migrar o armazenamento de clusters de failover
- Migrar o armazenamento de um servidor Linux que usa o Samba
- Mais facilmente migrados compartilhamentos de sincronização no Azure usando a sincronização de arquivos do Azure
- Migrar para novas redes, como o Azure

## <a name="see-also"></a>Consulte também

- [Migrar um servidor de arquivos usando o serviço de migração de armazenamento](migrate-data.md)
- [Serviços de migração de armazenamento, perguntas frequentes (FAQ)](faq.md)
- [Problemas conhecidos do serviço de migração de armazenamento](known-issues.md)
