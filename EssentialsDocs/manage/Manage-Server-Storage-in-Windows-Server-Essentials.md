---
title: Gerencie o armazenamento de servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e0f65dfd25afbd584764d33904ba82e4da4c5443
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Gerencie o armazenamento de servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 Windows Server Essentials permite que você gerencie todo o armazenamento do servidor (incluindo discos rígidos e espaços de armazenamento) do **discos rígidos** páginas a **armazenamento** guia do painel.  
  
 As seções a seguir fornecem informações que ajudarão você a aumentar o armazenamento do servidor, entender e usar espaços de armazenamento e gerenciar seus discos rígidos:  
  
-   [Gerenciar discos rígidos usando o painel](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aumentar o armazenamento no servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Executar verificações e reparos em discos rígidos](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Unidades de disco rígido de formato](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Adicionar um novo disco rígido](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Visão geral de espaços de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Criar um espaço de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>Gerenciar discos rígidos usando o painel  
 Windows Server Essentials permite que você gerencie todos os discos rígidos estão conectados ao servidor por meio do painel. No painel **armazenamento** guia, **discos rígidos** exibe todos os discos rígidos que estão disponíveis no servidor para armazenar os backups de dados e servidor. O servidor monitora o espaço disponível em cada unidade de disco rígida e exibe um alerta se ficar com pouco espaço no disco rígido. O **discos rígidos** guia exibe as seguintes informações:  
  
-   O nome de cada disco rígido  
  
-   A capacidade de cada disco rígido  
  
-   A quantidade de espaço usado em cada unidade de disco rígida  
  
-   A quantidade de espaço livre em cada unidade de disco rígida  
  
-   O status de cada unidade de disco rígido. um status em branco significa que o disco rígido está funcionando corretamente  
  
-   O painel de detalhes, que exibe todas as informações de pilha de armazenamento (para o pool de armazenamento, o espaço de armazenamento e o disco rígido), se o disco rígido selecionado está localizado em um espaço de armazenamento (em vez de um disco físico)  
  
 A tabela a seguir lista as tarefas de gerenciamento de disco rígido que estão disponíveis no painel e suas descrições. Algumas das tarefas são exibidas apenas quando um disco rígido é selecionado.  
  
### <a name="available-hard-drive-management-tasks"></a>Tarefas de gerenciamento de disco rígido disponível  
  
|Nome da tarefa|Descrição|  
|---------------|-----------------|  
|**Exibir as propriedades de disco rígido**|Abre o *HardDriveName***propriedades** página. Essa tarefa é exibida quando o disco rígido é selecionado. O **geral** guia a *HardDriveName* página Propriedades inclui as seguintes tarefas adicionais:<br /><br /> -   **Limpeza de disco**: permite que você limpar arquivos não utilizados no disco rígido (essa tarefa está disponível apenas no Windows Server Essentials).<br />-   **Verificar e reparar**: verifica se o disco rígido para erros do sistema de arquivos e tenta corrigir erros detectados automaticamente.<br /><br /> O **cópias de sombra** guia a *HardDriveName***propriedades** página permite que você habilite as cópias de sombra. Esse guia também exibe na próxima vez que as cópias de sombra está agendado para ser executado.|  
|**Gerenciar espaços de armazenamento**|**Observação:** do Windows Server Essentials, essa tarefa é exibida apenas quando há um espaço de armazenamento existente.<br /><br /> Abre o **espaços de armazenamento** painel de controle do qual você pode criar e gerenciar pools de armazenamento e espaços de armazenamento.|  
|**Criar um espaço de armazenamento**|Abre a criar um Assistente de espaço de armazenamento, que permite que você use um ou mais discos rígidos para aumentar a capacidade de um pool de armazenamento.|  
|**Aumentar a capacidade do pool de armazenamento**|**Observação:** essa tarefa ficará visível apenas se o disco rígido selecionado está localizado em um espaço de armazenamento.<br /><br /> Abre a aumentar a capacidade de um Assistente de Pool de armazenamento, que permite que você use um ou mais discos rígidos para aumentar a capacidade de um pool de armazenamento.|  
  
##  <a name="BKMK_2"></a>Aumentar o armazenamento no servidor  
 Para aumentar o armazenamento no servidor, você pode adicionar um disco rígido interno adicional para o servidor. Para adicionar o disco rígido interno adicional, você deve desligar o servidor, adicione o disco rígido interno e, em seguida, reiniciar o servidor. Você não precisa desligar o servidor se o disco rígido está anexado ao controlador SCSI. Nesse caso, o disco rígido pode ser conectado enquanto o servidor está em execução.  
  
 Dependendo se o disco rígido a ser adicionada está formatado, siga um destes procedimentos:  
  
-   **Formatado** se o disco rígido interno é formatado com ReFS ou de NTFS, o servidor atribui a ele uma letra de unidade e o disco rígido aparece no **discos rígidos** guia. Agora você pode criar ou mover as pastas de servidor para a nova unidade de disco rígida.  
  
-   **Não formatado** se o disco rígido interno não está formatado, o seguinte alerta é exibido: um ou mais discos rígidos não formatados estão conectados ao servidor. Use o procedimento a seguir para formatar o disco rígido.  
  
#### <a name="to-format-the-hard-disk"></a>Para formatar o disco rígido  
  
1.  Abra o painel.  
  
2.  No painel de navegação, clique no ícone de alerta para iniciar o **Alert Viewer** no Windows Server Essentials, ou o **monitoramento de integridade** guia no Windows Server Essentials.  
  
3.  No **Alert Viewer** ou o **monitoramento de integridade** guia, clique no alerta e no painel de tarefas, clique em **solucionar esse problema**.  
  
4.  Siga as instruções para concluir o Assistente adicionar um novo disco rígido unidade.  
  
###  <a name="BKMK_Clean"></a>Para limpar o disco rígido  
  
1.  Abra o painel.  
  
2.  No painel de navegação, clique em **armazenamento**e clique em **discos rígidos**.  
  
3.  No **discos rígidos** seção, selecione a letra da unidade que foi atribuída para o disco rígido recém-adicionado e no painel de tarefas, clique em **exibir as propriedades de disco rígido**.  
  
4.  Em **< Driveletter\ > propriedades**diante do **geral** , clique em **limpeza de disco**.  
  
##  <a name="BKMK_Check"></a>Executar verificações e reparos em discos rígidos  
 Os discos rígidos verificar e reparar o processo verifica a integridade do sistema de arquivos armazenado nos discos rígidos. Ele é executado uma **chkdsk** processos no volume que os arquivos de backup são armazenados no. O problema de alerta seguir pode ser resolvido executando uma verificação e reparar em discos rígidos:  
  
-   Um ou mais discos rígidos no Backup do servidor devem ser verificados.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Para verificar e reparar discos rígidos  
  
1.  Abra o painel.  
  
2.  Clique em **discos rígidos e pastas de servidor**e clique em **discos rígidos**.  
  
3.  Selecione o disco rígido que está exibindo o erro e, em seguida, selecione **exibir as propriedades de disco rígido**.  
  
4.  Sobre o **verificar e reparar** , clique em **verificar e reparar**.  
  
##  <a name="BKMK_3"></a>Unidades de disco rígido de formato  
 Quando um disco rígido interno não formatado é detectado no servidor, um alerta de saúde orienta o usuário durante o processo de formatação. Adicionar um novo Assistente de unidade de disco rígido orienta você pelo formatar o disco rígido e permite que você configure o disco rígido em uma das seguintes maneiras:  
  
1.  Formate o disco rígido e criar uma unidade automaticamente nele. Se você escolher essa opção, quando o assistente for concluído, uma unidade de disco rígida lógica formatada com o sistema de arquivos NTFS é criada.  
  
2.  Formate o disco rígido e configurá-lo para o backup do servidor. Se você escolher essa opção, o servidor de Backup Assistente de configuração é iniciado, e ele percorre a configuração de backup do servidor.  
  
3.  Se um armazenamento espaço existe, use a nova unidade de disco rígida para criar um espaço de armazenamento. Você deve ter pelo menos dois discos rígidos para criar um espaço de armazenamento.  
  
4.  Se já existir um espaço de armazenamento, use a nova unidade de disco rígida para aumentar a capacidade de um pool de armazenamento. Essa opção só será exibida se houver um espaço de armazenamento existentes criado no servidor. Se você escolher essa opção, o assistente adicionará esse disco rígido ao pool de armazenamento.  
  
##  <a name="BKMK_4"></a>Adicionar um novo disco rígido  
 Quando você conecta uma nova unidade de disco rígido em um servidor que executa o Windows Server Essentials, você pode:  
  
-   [Use a nova unidade de disco rígida para armazenar as pastas de servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Usar a nova unidade de disco rígida para armazenar os backups de servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Usar a nova unidade de disco rígida para aumentar a capacidade do pool de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>Use a nova unidade de disco rígida para armazenar as pastas de servidor  
 Para usar a nova unidade de disco rígida para armazenar as pastas de servidor, você pode adicionar uma nova pasta no servidor para o disco rígido ou mover uma pasta de servidor existente para o disco rígido.  
  
##### <a name="to-store-server-folders"></a>Para armazenar as pastas de servidor  
  
1.  Abra o painel.  
  
2.  Clique no **armazenamento** guia e, em seguida, clique em **pastas de servidor**.  
  
3.  No **tarefas de pastas de servidor** painel, siga um destes procedimentos:  
  
    1.  Para adicionar uma pasta no servidor, clique em **adicionar uma pasta**.  
  
    2.  Para mover uma pasta no servidor, selecione a pasta que você deseja mover para a nova unidade de disco rígida e, em seguida, clique em **mover uma pasta**.  
  
    > [!NOTE]
    >  Se você navegar para o disco rígido e selecioná-la como o local da pasta de servidor sem criar uma pasta, a seguinte mensagem de erro aparece: **um diretório raiz (como C:\\, D:\\) não podem ser adicionados como uma pasta no servidor. Crie uma nova pasta ou selecione um já existente no diretório raiz e tente novamente**. Para resolver esse erro, crie uma nova pasta no disco rígido recém-adicionado e, em seguida, selecione a nova pasta como o local para armazenar pastas do servidor.  
  
4.  Siga as instruções para concluir o assistente.  
  
 Para saber mais sobre movendo pastas de servidor, veja [adicionar ou mover uma pasta de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="BKMK_4b"></a>Usar a nova unidade de disco rígida para armazenar os backups de servidor  
 Você pode usar o disco rígido recém-adicionado para armazenar os backups de servidor.  
  
##### <a name="to-store-server-backups"></a>Para armazenar os backups de servidor  
  
1.  Abra o painel.  
  
2.  Clique no **dispositivos** guia, selecione o servidor no painel de lista e de painel de tarefas, siga um destes procedimentos:  
  
    1.  Se o Backup do servidor não está configurado no servidor, clique em **configurar o Backup do servidor**.  
  
    2.  Se o Backup do servidor é configurado no servidor, clique em **personalizar o Backup do servidor**.  
  
     O Assistente de configuração de Backup do servidor é exibida.  
  
3.  No **selecionar o destino do Backup** página, selecione o novo disco rígido como o destino do backup.  
  
4.  Siga as instruções para concluir o assistente.  
  
###  <a name="BKMK_4c"></a>Usar a nova unidade de disco rígida para aumentar a capacidade do pool de armazenamento  
 Quando sua capacidade de pool de armazenamento estiver fraca, você recebe um alerta informando que você pode aumentar a capacidade do pool de armazenamento, adicionando um novo disco rígido ao pool de armazenamento usando o aumentar a capacidade de um Assistente de Pool de armazenamento.  
  
> [!NOTE]
>  Você pode executar este procedimento somente se você criou um pool de armazenamento no servidor.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Para aumentar a capacidade do pool de armazenamento  
  
1.  Abra o painel.  
  
2.  Clique no **armazenamento** guia e, em seguida, clique em **discos rígidos**.  
  
3.  Selecione a unidade que está mostrando uma baixa capacidade.  
  
4.  No painel de tarefas, selecione **aumentar a capacidade do pool de armazenamento**. A capacidade de aumentar de um Assistente de Pool de armazenamento é exibida.  
  
5.  Siga as instruções para concluir o assistente.  
  
##  <a name="BKMK_5"></a>Visão geral de espaços de armazenamento  
 Permite que os espaços de armazenamento você agrupar discos juntos em um pool de armazenamento. Você pode usar a capacidade do pool para criar os espaços de armazenamento. Os espaços de armazenamento são discos virtuais que aparecem no **discos rígidos** guia do painel. Você pode usar espaços de armazenamento como qualquer outra unidade, então ele s fácil de trabalhar com arquivos neles. Quando você esgotamento da capacidade do pool, você pode criar espaços de armazenamento grandes e adicionar mais unidades ao pool de armazenamento. Se você tiver dois ou mais discos dentro do pool de armazenamento, você pode criar espaços de armazenamento com um espelhamento bidirecional que não sejam afetado por uma falha do disco? ou até mesmo a falha das duas unidades? se você criar um espaço de armazenamento de espelho triplo.  
  
 Para criar um espaço de armazenamento, tudo o que você precisa é um ou mais unidades extras além para a unidade em que o Windows está instalado. Essas unidades podem ser unidades de disco rígido internas ou externas ou unidades de estado sólido. Você pode usar uma variedade de tipos de unidades com espaços de armazenamento, incluindo unidades USB, SATA e SAS.  
  
> [!NOTE]
>  Se você definir espaços de armazenamento em um servidor que executa o Windows Server Essentials, você não pode executar uma restauração de fábrica com o **dados limpa** opção. A solução alternativa para esse problema é remover primeiro espaços de armazenamento e, em seguida, executar uma restauração de fábrica com o **dados limpa** opção.  
  
 Para saber mais sobre espaços de armazenamento, consulte [armazenamento espaços perguntas frequentes (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="BKMK_6"></a>Criar um espaço de armazenamento  
 Para começar a trabalhar com espaços de armazenamento no servidor, os seguintes requisitos mínimos devem ser atendidos:  
  
-   O servidor que executa o Windows Server Essentials deve ser anexado a outras unidades físicas (não apenas a unidade de inicialização), as unidades não devem hospedar qualquer volumes e devem ter uma capacidade mínima de 10 GB. Uma unidade física é necessária para criar um pool de armazenamento; pelo menos duas unidades físicas é necessária para criar um espaço de armazenamento de espelho resiliente.  
  
-   Pelo menos duas unidades físicas são necessárias para criar um espaço de armazenamento com resiliência por meio de paridade ou espelhamento bidirecional.  
  
> [!NOTE]
>  Se você definir espaços de armazenamento em um servidor que executa o Windows Server Essentials, você não pode executar uma restauração de fábrica com o **dados limpa** opção. A solução alternativa para esse problema é remover primeiro espaços de armazenamento e, em seguida, executar uma restauração de fábrica com o **dados limpa** opção.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para criar um espaço de armazenamento no Windows Server Essentials  
  
1.  Adicione ou conecte todas as unidades que você deseja agrupar com espaços de armazenamento para o servidor que executa o Windows Server Essentials.  
  
2.  No painel, clique em **avançado: gerenciar espaços de armazenamento**.  
  
3.  Clique em **criar um novo pool e espaço de armazenamento**.  
  
4.  Selecione as unidades que você deseja adicionar ao novo espaço de armazenamento e, em seguida, clique em **Criar pool**.  
  
5.  Dê a unidade um nome e uma letra e escolha um layout. **Espelhamento bidirecional**, **espelho triplo**, e **paridade** pode ajudar a proteger os arquivos no espaço de armazenamento contra falhas de unidade.  
  
6.  Insira o tamanho máximo do espaço de armazenamento pode atingir e clique em **criar espaço de armazenamento**.  
  
 No Windows Server Essentials, você pode criar um espaço de armazenamento espelhado bidirecional usando a criar um Assistente de espaço de armazenamento do painel.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para criar um espaço de armazenamento no Windows Server Essentials  
  
1.  Adicione ou conecte todas as unidades que você deseja agrupar com espaços de armazenamento para o servidor que executa o Windows Server Essentials.  
  
2.  No painel, clique em **gerenciar espaços de armazenamento**. Criar um Assistente de espaço de armazenamento é exibida.  
  
3.  Siga as instruções para concluir o assistente.  
  
 Para obter informações sobre como aumentar a capacidade do pool de armazenamento, consulte [usar a nova unidade de disco rígida para aumentar a capacidade do pool de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
