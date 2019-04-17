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
ms.openlocfilehash: 727dd74e4bddc52f735969f216914b9d76d1f413
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="set-up-or-customize-server-backup"></a>Configurar ou personalizar o backup do servidor

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Backup do servidor não está configurado automaticamente durante a instalação. Você deve proteger seu servidor e seus dados automaticamente agendando backups diários. É recomendável que você mantenha um plano de backup diário porque a maioria das organizações não pode perder os dados que foi criados por vários dias.  
  
 Veja as seções a seguir para configurar ou personalizar o backup do servidor:  
  
-   [Configurar ou alterar configurações de backup do servidor](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Agendamento de backup do servidor](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Unidade de destino do backup](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Itens de backup](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a>Configurar ou alterar configurações de backup do servidor  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Para definir ou alterar configurações de backup do servidor  
  
1.  Abrir o **painel**e, em seguida, clique no **dispositivos** guia.  
  
2.  Na exibição de lista, clique em seu servidor para selecioná-lo.  
  
3.  No painel de tarefas, clique em **configurar o Backup do servidor**.  
  
    > [!NOTE]
    >  Se você quiser alterar as configurações existentes de backup, clique em **personalizar o Backup do servidor**.  
  
4.  Siga as instruções no assistente.  
  
    > [!NOTE]
    >  Se você iniciar o assistente antes de se conectar o disco rígido externo para o servidor, clique em **lista de atualização** sobre o **selecionar o destino do backup** página depois de anexar o disco rígido.  
  
> [!NOTE]
>  Na instalação padrão do Windows Server Essentials, o servidor está configurado para executar automaticamente uma desfragmentação uma vez por semana. Isso pode resultar em maior do que backups normais se você usar o software de geração de imagens não são da Microsoft. Se não for necessária desfragmentar o servidor regularmente, você pode seguir estas etapas para desativar o agendamento de desfragmentação:  
>   
>  1.  Pressione a tecla Windows + W para abrir **pesquisa**.  
> 2.  Na caixa de texto de pesquisa, digite **desfragmentar**.  
> 3.  Na seção de resultados, clique em **desfragmentar e otimizar suas unidades**.  
> 4.  No **otimizar unidades** página, selecione uma unidade e clique em **alterar configurações**.  
> 5.  No **agendamento de otimização** janela, limpar o **executados em um agendamento (recomendado)** caixa de seleção e, em seguida, clique em **Okey** para salvar a alteração.  
  
##  <a name="BKMK_2"></a>Agendamento de backup do servidor  
 Quando você usa o Assistente de configuração de Backup do servidor ou o Assistente para personalizar o Backup do servidor, você pode optar por fazer backup dos dados do servidor em várias vezes durante o dia. Como os assistentes agendar incrementais com base em backups, os backups são executados rapidamente e desempenho do servidor não é afetado significativamente. Por padrão, os assistentes agendar um backup para execução diariamente em 12:00 PM e 11:00 PM. No entanto, você pode ajustar o agendamento de backup acordo com as necessidades da sua organização. Ocasionalmente, você deve avalia a eficiência do seu plano de backup e altere o plano conforme necessário.  
  
##  <a name="BKMK_Target"></a>Unidade de destino do backup  
 Você pode usar várias unidades de armazenamento externos para backups, e você pode girar as unidades entre os locais de armazenamento externa e no local. Isso pode melhorar seu planejamento ajudando você a recuperar seus dados caso ocorra dano físico para o local de hardware de preparação para desastres.  
  
 Ao escolher uma unidade de armazenamento para o backup do servidor, considere o seguinte:  
  
-   Escolha uma unidade que contém o espaço suficiente para armazenar seus dados. Suas unidades de armazenamento devem conter no mínimo 2,5 vezes a capacidade de armazenamento dos dados que você quer fazer backup. As unidades também devem ser grandes o suficiente para acomodar o crescimento futuro de seus dados de servidor.  
  
-   Ao usar uma unidade de armazenamento externo, certifique-se de que a unidade está vazia ou contém apenas os dados que você não precisa.  
  
    > [!CAUTION]
    >  Assistente de configuração de servidor Backup formata as unidades de armazenamento quando configura-los para backup.  
  
-   Se a unidade de backup de destino contém unidades offline, a configuração de backup não serão bem-sucedidas. Para concluir a configuração, ao selecionar o destino do backup, desmarque a caixa de seleção para excluir as unidades que estão offline.  
  
-   Se você escolher uma unidade que contém os backups anteriores como o destino de backup, o assistente permite que você escolha se você deseja manter os backups anteriores. Se você mantiver os backups, o assistente não formate a unidade.  
  
-   Você deve visitar o site do fabricante do seu armazenamento externo disco para garantir que a unidade de backup tem suporte em computadores que executam o Windows Server Essentials.  
  
-   A unidade não pode conter uma partição de sistema Extensible Firmware Interface (EFI). Se uma partição EFI estiver presente em uma unidade USB, presume-se que o disco é um disco de inicialização. Se você tiver certeza de que você não precisa os dados no disco, você pode reformatar o disco e usá-lo para backups.  
  
    > [!CAUTION]
    >  Todos os dados serão excluídos quando você reformatar o disco.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Para remover uma partição de sistema EFI de um disco USB  
  
    1.  No painel de controle, abra **sistemas e segurança**.  
  
    2.  Em **ferramentas administrativas**, clique em **criar e formatar partições do disco rígido**.  
  
    3.  Excluir todos os volumes no disco USB ou simplesmente exclua a partição EFI. O Assistente de configuração de Backup do servidor excluirá todos os volumes.  
  
-   A unidade não pode conter todas as pastas compartilhadas do servidor. Antes de usar o disco como uma unidade de destino do backup, deverá interromper o compartilhamento de todas as pastas compartilhadas do servidor. Você pode parar de compartilhar no painel ou no Explorador de arquivos.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Para interromper o compartilhamento em uma pasta no servidor do painel  
  
    1.  No painel, clique em **armazenamento**e clique em **pastas de servidor**.  
  
    2.  Selecione a pasta que você deseja parar de compartilhar e, em seguida, no painel de tarefas, clique em **parar**.  
  
> [!NOTE]
>  Se um backup não for bem-sucedida, porque a unidade de backup tinha espaço insuficiente, a letra da unidade para a unidade de destino do backup é removida do banco de dados do Windows Server Essentials e o painel não exibe a unidade. Se você quiser usar a unidade em backups futuros, você deve reatribuir a letra da unidade usando uma ferramenta nativa.  
>   
>  **Para reatribuir uma letra de unidade para um volume existente**  
>   
>  1.  No painel de controle, abra **sistemas e segurança**.  
> 2.  Em **ferramentas administrativas**, clique em **criar e formatar partições do disco rígido**.  
> 3.  Clique com botão direito a unidade e clique em **Alterar letra de unidade e caminhos**.  
> 4.  Clique em **adicionar**.  
> 5.  Na caixa de diálogo Adicionar letra de unidade ou caminho, selecione uma letra de unidade para atribuir. (Você pode reatribuir a mesma letra de unidade.) Clique em **Okey**.  
>   
>      A unidade aparece no painel imediatamente.  
  
##  <a name="BKMK_4"></a>Itens de backup  
 Você pode optar por fazer backup de todas as unidades, arquivos e pastas no servidor, ou selecione apenas unidades individuais, arquivos ou pastas para fazer backup.  
  
 Quando você adicionar ou remove uma unidade, ou adiciona ou remove pastas e arquivos compartilhados, você deve verificar a configuração de backup do servidor para certificar-se de que esses itens são adicionados ou removidos da configuração do backup. Para adicionar ou remover itens para o backup, siga um destes procedimentos:  
  
-   Para incluir uma unidade de dados no backup do servidor, marque a caixa de seleção adjacente  
  
-   Para excluir uma unidade de dados usando o backup do servidor, desmarque a caixa de seleção adjacente  
  
    > [!NOTE]
    >  Se você deseja excluir o **sistema operacional** item usando o backup, primeiro você deve limpar os **Backup de sistema (recomendado)** caixa de seleção.  
  
 Para minimizar a quantidade de armazenamento do servidor que usam seus backups de servidor, você pode querer excluir todas as pastas que contêm arquivos que não considerar importantes ou particularmente importante.  
  
 Por exemplo, você pode ter uma pasta que contém os programas de TV gravados que usa muito espaço em disco rígido. Você pode optar por não fazer backup desses arquivos porque você excluí-los normalmente após exibi-los de qualquer maneira. Ou você pode ter uma pasta que contém os arquivos temporários que não deseja manter.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
