---
title: Atualização das suas implantações de Serviços de Área de Trabalho Remota para o Windows Server 2016
description: Este artigo descreve como atualizar as implantações existentes de Serviços de Área de Trabalho Remota para o Windows Server 2016.
ms.author: spatnaik
ms.date: 03/20/2018
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
ms.openlocfilehash: a56511873c020172af0bb0813c2ee31f82ee5a2b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971503"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>Atualização das suas implantações de Serviços de Área de Trabalho Remota para o Windows Server 2016

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Atualizações de sistema operacional compatíveis com a função RDS instalada
Os upgrades para o Windows Server 2016 são compatíveis somente do Windows Server 2012 R2 e do Windows Server 2016 em diante.

## <a name="flow-for-deployment-upgrades"></a>Fluxo para atualizações de implantação
Para manter o tempo de inatividade ao mínimo, siga as etapas abaixo:

1. **Servidores do Agente de Conexão de Área de Trabalho Remota** devem ser os primeiros a serem atualizados. Se houver configuração ativa/ativa na implantação, remova todas, exceto um servidor da implantação e execute uma atualização in-loco. Execute atualizações nos servidores restantes do Agente de Conexão de Área de Trabalho Remota offline e, em seguida, adicione-os novamente à implantação. A implantação não estará disponível durante a atualização de servidores do Agente de Conexão de Área de Trabalho Remota.

   > [!NOTE]
   > É obrigatório atualizar os servidores do Agente de Conexão de Área de Trabalho Remota. Não damos suporte a servidores do Agente de Conexão de Área de Trabalho Remota do Windows Server 2012 R2 em uma implantação mista com servidores do Windows Server 2016. Depois que os servidores do Agente de Conexão de Área de Trabalho Remota estiverem executando o Windows Server 2016, a implantação será funcional, mesmo que o restante dos servidores na implantação ainda estiverem executando o Windows Server 2012 R2.

2. **Servidores de licenças RD** devem ser atualizados antes de atualizar seus servidores de Host da Sessão RD.
   > [!NOTE]
   > Servidores de licença de Área de Trabalho Remota do Windows Server 2012 e 2012 R2 funcionarão com implantações do Windows Server 2016, mas eles só podem processar CALs do Windows Server 2012 R2 e anteriores. Eles não podem usar CALs do Windows Server 2016. Confira [Licença de sua implantação do RDS com licenças de acesso para cliente (CALs)](rds-client-access-license.md) para obter mais informações sobre servidores de licenças RD.

3. **Servidores de Host da Sessão RD** podem ser atualizados em seguida. Para evitar o tempo de inatividade durante a atualização, o administrador pode dividir os servidores a serem atualizados em duas etapas, conforme detalhado abaixo. Tudo estará funcional após a atualização. Para atualizar, use as etapas descritas em [Atualizando servidores de Host da Sessão da Área de Trabalho Remota para o Windows Server 2016](upgrade-to-rdsh.md).

4. Os **servidores de Host de Virtualização de Área de Trabalho Remota** podem ser atualizados em seguida. Para atualizar, use as etapas descritas em [Atualizando servidores de Host de Virtualização de Área de Trabalho Remota para o Windows Server 2016](upgrade-to-rdvh.md).

5. Os **servidores de Acesso via Web da Área de Trabalho Remota** podem ser atualizados a qualquer momento.
   > [!NOTE]
   > A atualização via Web de Área de Trabalho Remota pode redefinir as propriedades do IIS (como arquivos de configuração). Para não perder as alterações, faças anotações ou cópias de personalizações feitas para o site da Web de Área de Trabalho Remota no IIS.

   > [!NOTE]
   > O Windows Server 2012 e os servidores de Acesso via Web da Área de Trabalho Remota 2012 R2 funcionarão com implantações do Windows Server 2016.

6. **Servidores de Gateway de Área de Trabalho Remota** podem ser atualizados a qualquer momento.
   > [!NOTE]
   > O Windows Server 2016 não inclui as políticas NAP (Proteção de Acesso à Rede) – elas terão de ser removidas. A maneira mais fácil de remover as políticas corretas é executando o Assistente de atualização. Se houver alguma política NAP que você deve excluir, a atualização será bloqueada e criará um arquivo de texto na área de trabalho que inclui as políticas específicas. Para gerenciar políticas NAP, abra a ferramenta de Servidor de Políticas de Rede. Depois de excluí-las, clique em **Atualizar** na ferramenta de instalação para continuar com o processo de atualização.

   > [!NOTE]
   > O Windows Server 2012 e os servidores de Gateway de Área de Trabalho Remota 2012 R2 funcionarão com implantações do Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Implantação de VDI – atualização de sistemas operacionais convidados compatíveis
Os administradores terão as seguintes opções para atualizar coleções de VM:

### <a name="upgrade-managed-shared-vm-collections"></a>Atualizar coleções de VM compartilhadas gerenciadas
Os administradores precisarão criar modelos de VM com a versão desejada do sistema operacional e usá-la para aplicar patch em todas as VMs no pool.

Damos suporte aos cenários de aplicação de patches a seguir:
- Windows 7 SP1 pode ser corrigido para o Windows 8 ou Windows 8.1
- Windows 8 pode ser corrigido para Windows 8.1
- Windows 8.1 pode ser corrigido para Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Atualizar coleções de VMs compartilhadas não gerenciadas
Os usuários finais não podem atualizar suas áreas de trabalho pessoas. Os administradores devem executar a atualização. As etapas exatas ainda serão determinadas.