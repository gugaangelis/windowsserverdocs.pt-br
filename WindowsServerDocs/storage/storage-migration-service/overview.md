---
Title: Visão geral do serviço de migração de armazenamento
description: Uma breve descrição do tópico para resultados da pesquisa
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 09/24/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: edc82c996f6877f770454fc6e27ccf5205a7d540
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843857"
---
# <a name="storage-migration-service-overview"></a>Visão geral do serviço de migração de armazenamento

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

### <a name="security-requirements"></a>Requisitos de segurança

- Uma conta de migração que seja um administrador nos computadores de origem.
- Uma conta de migração que seja um administrador nos computadores de destino.
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

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

### <a name="requirements-for-destination-servers"></a>Requisitos para servidores de destino

O servidor de destino deve executar um dos seguintes sistemas operacionais:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Servidores de destino que executam o Windows Server 2019 têm o dobro do desempenho de transferência de versões anteriores do Windows Server. Esse aumento de desempenho é devido à inclusão de um serviço de proxy de serviço de migração de armazenamento interno, que também abre o firewall necessárias portas abertas se eles não ainda.

## <a name="see-also"></a>Consulte também

- [Migrar um servidor de arquivos usando o serviço de migração de armazenamento](migrate-data.md)
- [Serviços de migração de armazenamento, perguntas frequentes (FAQ)](faq.md)
- [Problemas conhecidos do serviço de migração de armazenamento](known-issues.md)