---
title: Configurar ou personalizar o backup do servidor
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5bd36db4f61b990c3864828370607158df4803a3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433043"
---
# <a name="set-up-or-customize-server-backup"></a>Configurar ou personalizar o backup do servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 O backup do servidor não é configurado automaticamente durante a instalação. Você deve proteger seu servidor e seus dados automaticamente com o agendamento de backups diários. É recomendável que você mantenha um plano de backup diário, porque a maioria das organizações não pode perder dados criados ao longo de vários dias.  
  
 Consulte as seções a seguir para configurar ou personalizar o backup do servidor:  
  
-   [Definir ou alterar as configurações de backup do servidor](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Agendamento de backup do servidor](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Unidade de destino de backup](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Itens de backup](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a> Definir ou alterar as configurações de backup do servidor  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Para configurar ou alterar configurações de backup do servidor  
  
1.  Abra o **Painel** e clique na guia **Dispositivos**  
  
2.  Na visualização Lista, clique em seu servidor para selecioná-lo.  
  
3.  No painel de Tarefas, clique em **Configurar o backup do servidor**.  
  
    > [!NOTE]
    >  Se você quiser alterar as configurações de backup existentes, clique em **Personalizar backup para o servidor**.  
  
4.  Siga as instruções no assistente.  
  
    > [!NOTE]
    >  Se você iniciar o assistente antes de anexar o disco rígido externo ao servidor, clique em **Lista de atualização** na página **Selecionar o destino do backup** depois de anexar o disco rígido.  
  
> [!NOTE]
>  Na instalação padrão do Windows Server Essentials, o servidor está configurado para executar automaticamente uma desfragmentação uma vez por semana. Isso pode resultar em backups maiores que o normal se você usar um software de imagem que não for da Microsoft. Se não for necessário desfragmentar o servidor regularmente, você pode seguir estas etapas para desativar o agendamento de desfragmentação:  
> 
> 1. Pressione a tecla Windows + W para abrir a **Pesquisa**.  
>    2. Na caixa de texto Pesquisar, digite **Defragment**.  
>    3. Na seção resultados, clique em **Desfragmentar e otimizar suas unidades**.  
>    4. Na página **Otimizar Unidades**, selecione uma unidade e, em seguida, clique em **Alterar configurações**.  
>    5. Na janela **Agendamento de otimização** , desmarque a caixa de seleção **Executar seguindo um agendamento (recomendado)** e clique em **OK** para salvar a alteração.  
  
##  <a name="BKMK_2"></a> Agendamento de backup do servidor  
 Quando você usa o Assistente Configurar Backup do Servidor ou o Assistente Personalizar o Backup do Servidor, você pode optar por fazer backup de dados do servidor várias vezes durante o dia. Como os assistentes agendam backups baseados em incrementos, os backups são executados rapidamente e o desempenho do servidor não é afetado significativamente. Por padrão, os assistentes agendam um backup para executar diariamente às 12h e às 23h. No entanto, você pode ajustar o cronograma de backup de acordo com as necessidades da sua organização. Ocasionalmente, você deve avaliar a eficácia do seu plano de backup e alterar o plano conforme necessário.  
  
##  <a name="BKMK_Target"></a> Unidade de destino de backup  
 Você pode usar várias unidades de armazenamento externo para backups, e pode alternar as unidades entre locais de armazenamento externo e no local. Isso pode melhorar o planejamento de preparação para desastres, ajudando a recuperar seus dados caso haja danos físicos ao hardware no local.  
  
 Ao escolher uma unidade de armazenamento para o backup do servidor, considere o seguinte:  
  
-   Escolha uma unidade que contém espaço suficiente para armazenar os dados. As unidades de armazenamento devem conter pelo menos 2,5 vezes a capacidade de armazenamento de dados de que você deseja fazer backup. As unidades também devem ser grandes o suficiente para acomodar o crescimento futuro de seus dados de servidor.  
  
-   Ao usar uma unidade de armazenamento externo, certifique-se de que a unidade esteja vazia ou contenha apenas os dados que não são necessários.  
  
    > [!CAUTION]
    >  O Assistente de Configuração de Servidor de Backup formata as unidades de armazenamento quando ele os configura para backup.  
  
-   Se a unidade de destino de backup contiver unidades offline, a configuração de backup não terá êxito. Para concluir a configuração, ao selecionar o destino de backup, desmarque a caixa de seleção para excluir as unidades que estejam offline.  
  
-   Se você escolher uma unidade que contém os backups anteriores como o destino de backup, o assistente permite que você escolha se deseja manter os backups anteriores. Se você mantiver os backups, o assistente não formatará a unidade.  
  
-   Você deve visitar o site do fabricante da unidade armazenamento externo garantir que sua unidade de backup tenha suporte em computadores que executam o Windows Server Essentials.  
  
-   A unidade não pode conter uma partição de sistema Extensible Firmware Interface (EFI). Se houver uma partição EFI em uma unidade USB, presume-se que o disco é um disco de inicialização. Se você tiver certeza de que você não precisa de dados no disco, você pode reformatar o disco e usá-la para backups.  
  
    > [!CAUTION]
    >  Todos os dados serão excluídos quando você reformatar o disco.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Para remover uma partição de sistema EFI de um disco USB  
  
    1.  No Painel de Controle, abra **Sistemas e Segurança**.  
  
    2.  Em **Ferramentas Administrativas**, clique em **Criar e formatar partições de disco rígido**.  
  
    3.  Exclua todos os volumes no disco USB ou apenas exclua a partição EFI. O Assistente Configurar Backup do Servidor irá excluir todos os volumes.  
  
-   A unidade não pode conter as pastas compartilhadas do servidor. Antes de poder usar o disco como uma unidade de destino de backup, você deve interromper o compartilhamento em quaisquer pastas de servidor compartilhado. Você pode interromper o compartilhamento no Painel ou no Explorador de arquivos.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Para interromper o compartilhamento em uma pasta de servidor do Painel  
  
    1.  No Painel, clique em **Armazenamento** e clique em **Pastas do Servidor**.  
  
    2.  Selecione a pasta que você deseja interromper o compartilhamento e, no painel de tarefas, clique em **Parar**.  
  
> [!NOTE]
>  Se um backup falhar porque a unidade de backup tinha espaço insuficiente, a letra da unidade para a unidade de destino de backup é removida do banco de dados do Windows Server Essentials e o painel não exibirá a unidade. Se você deseja usar a unidade em backups futuros, você deve reatribuir a letra de unidade usando uma ferramenta nativa.  
> 
>  **Para reatribuir uma letra de unidade para um volume existente**  
> 
> 1. No Painel de Controle, abra **Sistemas e Segurança**.  
>    2. Em **Ferramentas Administrativas**, clique em **Criar e formatar partições de disco rígido**.  
>    3. Clique com botão direito na unidade e clique em **Alterar a letra da unidade e caminhos**.  
>    4. Clique em **Adicionar**.  
>    5. Na caixa de diálogo Adicionar letra de unidade ou caminho, selecione uma letra de unidade para atribuir. (Você pode reatribuir a mesma letra de unidade). Em seguida, clique em **OK**.  
> 
>    A unidade será exibida no Painel imediatamente.  
  
##  <a name="BKMK_4"></a> Itens de backup  
 Você pode optar por fazer backup de todas as unidades, arquivos e pastas no servidor ou selecionar apenas unidades, arquivos ou pastas individuais para backup.  
  
 Quando você adicionar ou remove uma unidade ou pastas e arquivos compartilhados, você deve verificar a configuração de backup do servidor para certificar-se de que esses itens sejam adicionados ou removidos da configuração de backup. Para adicionar ou remover itens para backup, siga um destes procedimentos:  
  
- Para incluir uma unidade de dados no backup do servidor, marque a caixa de seleção adjacente  
  
- Para excluir uma unidade de dados do backup do servidor, desmarque a caixa de seleção adjacente  
  
  > [!NOTE]
  >  Se deseja excluir o item **Sistema Operacional** do backup, você deve primeiro desmarcar a caixa de seleção **Backup de sistema (recomendado)** .  
  
  Para minimizar a quantidade de armazenamento do servidor que os backups do servidor usam, convém excluir todas as pastas que contenham arquivos que você não considere valiosos ou particularmente importantes.  
  
  Por exemplo, você pode ter uma pasta que contém programas de TV gravados que usam muito espaço em disco rígido. Você pode optar por não fazer backup desses arquivos porque normalmente eles são excluídos após serem assistidos. Ou você pode ter uma pasta que contém os arquivos temporários que você não deseja manter.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
