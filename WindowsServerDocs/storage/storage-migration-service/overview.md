---
title: Visão geral do serviço de migração de armazenamento
description: O serviço de migração de armazenamento facilita a migração do armazenamento para o Windows Server ou para o Azure. Ele fornece uma ferramenta gráfica que inventaria dados em servidores Windows e Linux e, em seguida, transfere os dados para servidores mais recentes ou para máquinas virtuais do Azure. O serviço de migração de armazenamento também fornece a opção de transferir a identidade de um servidor para o servidor de destino para que os aplicativos e usuários possam acessar seus dados sem alterar os links ou os caminhos.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 10/25/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 5963035846814f2ec3fd3417e629f71b146cc73e
ms.sourcegitcommit: ac9946deb4fa70203a9b05e0386deb4244b8ca55
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74310378"
---
# <a name="storage-migration-service-overview"></a>Visão geral do serviço de migração de armazenamento

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canal semestral)

O serviço de migração de armazenamento facilita a migração do armazenamento para o Windows Server ou para o Azure. Ele fornece uma ferramenta gráfica que inventaria dados em servidores Windows e Linux e, em seguida, transfere os dados para servidores mais recentes ou para máquinas virtuais do Azure. O serviço de migração de armazenamento também fornece a opção de transferir a identidade de um servidor para o servidor de destino para que os aplicativos e usuários possam acessar seus dados sem alterar os links ou os caminhos.

Este tópico discute por que você desejaria usar o serviço de migração de armazenamento, como funciona o processo de migração e quais são os requisitos para os servidores de origem e de destino.

## <a name="why-use-storage-migration-service"></a>Por que usar o serviço de migração de armazenamento

Use o serviço de migração de armazenamento porque você tem um servidor (ou muitos servidores) que deseja migrar para um hardware ou máquinas virtuais mais recentes. O serviço de migração de armazenamento foi projetado para ajudar fazendo o seguinte:

- Inventariar vários servidores e seus dados
- Transferência rápida de arquivos, compartilhamentos de arquivos e configuração de segurança dos servidores de origem
- Opcionalmente, assuma a identidade dos servidores de origem (também conhecidos como recortar) para que os usuários e aplicativos não precisem alterar nada para acessar os dados existentes
- Gerenciar uma ou várias migrações da interface do usuário do centro de administração do Windows

![Diagrama mostrando o serviço de migração de armazenamento migrando arquivos & configuração de servidores de origem para servidores de destino, VMs do Azure ou Sincronização de Arquivos do Azure.](media/overview/storage-migration-service-diagram.png)

**Figura 1: origens e destinos do serviço de migração de armazenamento**

## <a name="how-the-migration-process-works"></a>Como funciona o processo de migração

A migração é um processo de três etapas:

1. **Servidores de inventário** para coletar informações sobre seus arquivos e a configuração (mostrados na Figura 2).
2. **Transfira (Copie) os dados** dos servidores de origem para os servidores de destino.
3. **Recortar para os novos servidores** (opcional).<br>Os servidores de destino assumem as identidades anteriores dos servidores de origem para que os aplicativos e os usuários não precisem alterar nada. <br>Os servidores de origem entram em um estado de manutenção em que eles ainda contêm os mesmos arquivos que sempre têm (nunca removemos arquivos dos servidores de origem), mas não estão disponíveis para usuários e aplicativos. Em seguida, você pode encerrar os servidores de sua conveniência.

![captura de tela mostrando um servidor pronto para ser examinado](media/migrate/inventory.png)
**Figura 2: servidores de inventário do serviço de migração de armazenamento**

## <a name="requirements"></a>Requisitos

Para usar o serviço de migração de armazenamento, você precisa do seguinte:

- Um **servidor de origem** ou **cluster de failover** para migrar arquivos e dados de
- Um **servidor de destino** que executa o Windows Server 2019 (clusterizado ou autônomo) para migrar para o. O Windows Server 2016 e o Windows Server 2012 R2 funcionam bem, mas estão em cerca de 50% mais lento
- Um **servidor do Orchestrator** que executa o Windows Server 2019 para gerenciar a migração  <br>Se estiver migrando apenas alguns servidores e um dos servidores estiver executando o Windows Server 2019, você poderá usá-lo como o Orchestrator. Se você estiver migrando mais servidores, recomendamos o uso de um servidor Orchestrator separado.
- Um **PC ou servidor que executa o [centro de administração do Windows](../../manage/windows-admin-center/understand/windows-admin-center.md)**  para executar a interface do usuário do serviço de migração de armazenamento, a menos que você prefira usar o PowerShell para gerenciar a migração. O centro de administração do Windows e a versão 2019 do Windows Server devem ter, no mínimo, a versão 1809.

É altamente recomendável que os computadores Orchestrator e de destino tenham pelo menos dois núcleos ou dois vCPUs, e pelo menos 2 GB de memória. As operações de inventário e transferência são significativamente mais rápidas com mais processadores e memória.

### <a name="security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports"></a>Requisitos de segurança, o serviço de proxy de serviço de migração de armazenamento e as portas de firewall

- Uma conta de migração que é um administrador nos computadores de origem e no computador do Orchestrator.
- Uma conta de migração que é um administrador nos computadores de destino e no computador Orchestrator.
- O computador Orchestrator deve ter a regra de firewall de compartilhamento de arquivos e impressoras (SMB-in *) habilitada*.
- Os computadores de origem e de destino devem ter as seguintes regras de firewall habilitadas para *entrada* (embora você já possa tê-las habilitadas):
  - Compartilhamento de arquivos e de impressora (SMB-Entrada)
  - Serviço Netlogon (NP-in)
  - Instrumentação de Gerenciamento do Windows (DCOM-in)
  - Instrumentação de Gerenciamento do Windows (WMI-In)
  
  > [!TIP]
  > A instalação do serviço de proxy de serviço de migração de armazenamento em um computador com Windows Server 2019 abre automaticamente as portas de firewall necessárias nesse computador. Para fazer isso, conecte-se ao servidor de destino no centro de administração do Windows e vá para **Gerenciador do servidor** (no centro de administração do windows) > **funções e recursos**, selecione **proxy de serviço de migração de armazenamento**e, em seguida, selecione **instalar**.


- Se os computadores pertencerem a um domínio Active Directory Domain Services, todos deverão pertencer à mesma floresta. O servidor de destino também deve estar no mesmo domínio que o servidor de origem se você quiser transferir o nome de domínio da origem para o destino ao recortar. A transferência tecnicamente funciona entre domínios, mas o nome de domínio totalmente qualificado do destino será diferente da origem...

### <a name="requirements-for-source-servers"></a>Requisitos para servidores de origem

O servidor de origem deve executar um dos seguintes sistemas operacionais:

- Windows Server, Canal Semestral
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003
- Windows Small Business Server 2003 R2
- Windows Small Business Server 2008
- Windows Small Business Server 2011
- Windows Server 2012 Essentials
- Windows Server 2012 R2 Essentials
- Windows Server 2016 Essentials
- Windows Server 2019 Essentials
- Windows Storage Server 2008
- Windows Storage Server 2008 R2
- Windows Storage Server 2012
- Windows Storage Server 2012 R2
- Windows Storage Server 2016

Observação: o Windows Small Business Server e o Windows Server Essentials são controladores de domínio. O serviço de migração de armazenamento ainda não pode ser reduzido dos controladores de domínio, mas pode inventariar e transferir arquivos deles.   

Você pode migrar os seguintes tipos de fonte adicionais se o Orchestrator estiver executando o Windows Server, versão 1903 ou posterior, ou se o orquestrador estiver executando uma versão anterior do Windows Server com o [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) instalado:

- Clusters de failover
- Servidores Linux que usam o samba. Testamos o seguinte:
    - CentOS 7
    - Debian GNU/Linux 8
    - RedHat Enterprise Linux 7,6
    - SUSE Linux Enterprise Server (SLES) 11 SP4
    - Ubuntu 16, 4 LTS e 12.04.5 LTS
    - Samba 4,8, 4,7, 4,3, 4,2 e 3,6

### <a name="requirements-for-destination-servers"></a>Requisitos para servidores de destino

O servidor de destino deve executar um dos seguintes sistemas operacionais:

- Windows Server, Canal Semestral
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Os servidores de destino que executam o Windows Server 2019 ou o Windows Server, canal semestral ou posterior têm o dobro do desempenho de transferência de versões anteriores do Windows Server. Esse aumento de desempenho ocorre devido à inclusão de um serviço de proxy de serviço de migração de armazenamento interno, que também abre as portas de firewall necessárias, se ainda não estiverem abertas.

## <a name="whats-new-in-storage-migration-service"></a>O que há de novo no serviço de migração de armazenamento

Os novos recursos a seguir estão disponíveis ao executar o Storage Migration Server Orchestrator no Windows Server, versão 1903 ou posterior, ou uma versão anterior do Windows Server com o [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) instalado:

- Migrar usuários e grupos locais para o novo servidor
- Migrar o armazenamento de clusters de failover, migrar para clusters de failover e migrar entre servidores autônomos e clusters de failover
- Migrar o armazenamento de um servidor Linux que usa o Samba
- Sincronização mais fácil de compartilhamentos integrados no Azure usando a Sincronização de Arquivos do Azure
- Migrar para novas redes, como o Azure

## <a name="see-also"></a>Consulte também

- [Migrar um servidor de arquivos usando o serviço de migração de armazenamento](migrate-data.md)
- [Perguntas frequentes sobre serviços de migração de armazenamento](faq.md)
- [Problemas conhecidos do serviço de migração de armazenamento](known-issues.md)
