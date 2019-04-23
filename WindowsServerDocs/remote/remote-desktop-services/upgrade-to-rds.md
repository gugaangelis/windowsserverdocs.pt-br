---
title: Atualizando suas implantações dos serviços de área de trabalho remota para o Windows Server 2016
description: Este artigo descreve como atualizar as implantações existentes do serviços de área de trabalho remota para Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 03/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
notes: https://social.technet.microsoft.com/wiki/contents/articles/22069.remote-desktop-services-upgrade-guidelines-for-windows-server-2012-r2.aspx
ms.openlocfilehash: f683a7d9346494e7f1fb6faf716ca9c90cfef8d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875747"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>Atualizando suas implantações dos serviços de área de trabalho remota para o Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Suporte para atualizações de sistema operacional com a função RDS instalada
Atualizações para o Windows Server 2016 têm suporte apenas no Windows Server 2012 R2 e Windows Server 2016.

## <a name="flow-for-deployment-upgrades"></a>Fluxo para atualizações de implantação
Para impedir que o tempo de inatividade mínimo, é melhor seguir as etapas abaixo:

1. **Servidores do agente de Conexão de área de trabalho remota** deve ser o primeiro a ser atualizado. Se não houver configuração ativo/ativo na implantação, remova todos, exceto um servidor da implantação e realizar uma atualização in-loco. Execute atualizações nos servidores restantes do agente de Conexão de área de trabalho offline e, em seguida, adicioná-los novamente para a implantação. A implantação não estará disponível durante a atualização de servidores do agente de Conexão de área de trabalho.

   > [!NOTE] 
   > É obrigatório para atualizar os servidores do agente de Conexão de área de trabalho remota. Não oferecemos suporte a servidores do agente de Conexão de área de trabalho do Windows Server 2012 R2 em uma implantação mista com servidores do Windows Server 2016. Depois que os servidores do agente de Conexão de área de trabalho estiver executando o Windows Server 2016 a implantação será ser funcional, mesmo se o restante dos servidores na implantação ainda estiverem executando o Windows Server 2012 R2.

2. **Servidores de licenças de área de trabalho remota** deve ser atualizado antes de atualizar seus servidores de Host de sessão de área de trabalho remota.
   > [!NOTE] 
   > Servidores de licença do Windows Server 2012 e 2012 R2 RD funcionará com implantações do Windows Server 2016, mas eles só podem processar CALs do Windows Server 2012 R2 e anteriores. Eles não podem usar CALs do Windows Server 2016. Ver [licença de sua implantação do RDS com licenças de acesso para cliente (CALs)](rds-client-access-license.md) para obter mais informações sobre servidores de licenças de área de trabalho remota.

3. **Servidores de Host de sessão de área de trabalho remota** podem ser atualizados em seguida. Para evitar o tempo durante a atualização, o administrador pode dividir os servidores a ser atualizado em 2 etapas, conforme detalhado abaixo. Tudo estará funcional após a atualização. Para atualizar, use as etapas descritas em [servidores de atualização de Host da sessão da área de trabalho remota para o Windows Server 2016](upgrade-to-rdsh.md).

4. **Servidores de Host de virtualização de área de trabalho remota** podem ser atualizados em seguida. Para atualizar, use as etapas descritas em [servidores de atualização de Host de virtualização de área de trabalho remota para o Windows Server 2016](upgrade-to-rdvh.md).

5. **Servidores de acesso via Web RD** podem ser atualizados a qualquer momento.
   > [!NOTE]
   > Atualizando a Web da área de trabalho remota pode redefinir as propriedades IIS (como arquivos de configuração). Para não perder as alterações, fazer observações ou cópias de personalizações feitas para o site da Web da área de trabalho remota no IIS.

   > [!NOTE] 
   > Windows Server 2012 e servidores de acesso via Web RD R2 2012 funcionará com implantações do Windows Server 2016.

6. **Servidores de Gateway de área de trabalho remota** podem ser atualizados a qualquer momento.
   > [!NOTE]
   > Windows Server 2016 não inclui as políticas de proteção de acesso de rede (NAP) – eles terão de ser removido. É a maneira mais fácil de remover as políticas corretas, executando o Assistente de atualização. Se houver quaisquer políticas NAP, que você deve excluir, a atualização bloqueará e crie um arquivo de texto na área de trabalho que inclui as políticas específicas. Para gerenciar políticas NAP, abra a ferramenta de servidor de políticas de rede. Depois de excluí-los, clique em **Refresh** na ferramenta de instalação para continuar com o processo de atualização. 

   > [!NOTE] 
   > Windows Server 2012 e servidores de Gateway de área de trabalho remota R2 2012 funcionará com implantações do Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Implantação de VDI – atualização de SO de convidado com suporte
Os administradores terão as seguintes opções para atualizar de coleções de VM:

### <a name="upgrade-managed-shared-vm-collections"></a>Atualizar coleções VM compartilhado gerenciado 
Os administradores precisarão criar modelos de VM com a versão desejada do sistema operacional e usá-lo para todas as VMs no pool de patch. 

Damos suporte a cenários de aplicação de patches a seguir:
- Windows 7 SP1 pode ser corrigida para o Windows 8 ou Windows 8.1
- Windows 8 pode ser corrigida para Windows 8.1
- Windows 8.1 pode ser corrigida para Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Atualizar compartilhados conjuntos não gerenciados de VM 
Os usuários finais não pode atualizar suas áreas de trabalho pessoas. Os administradores devem executar a atualização. As etapas exatas são ainda deve ser determinado.