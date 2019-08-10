---
title: Gerenciar o armazenamento de servidor no Windows Server Essentials
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
ms.openlocfilehash: 1637dc6844a0152249fc406a66ef583b9e3a5242
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914657"
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Gerenciar o armazenamento de servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 O Windows Server Essentials permite que você gerencie todo o seu armazenamento de servidor (incluindo unidades de disco rígido e espaços de armazenamento) das páginas **Discos Rígidos** na guia **Armazenamento** do Painel.  
  
 As seções a seguir fornecem informações que ajudarão você a aumentar seu armazenamento de servidor, entender e usar espaços de armazenamento e gerenciar seus discos rígidos:  
  
-   [Gerenciar discos rígidos usando o painel](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aumentar o armazenamento no servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Executar verificações e reparos em discos rígidos](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Formatar discos rígidos](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Adicionar um novo disco rígido](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Visão geral dos espaços de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Criar um espaço de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>Gerenciar discos rígidos usando o painel  
 O Windows Server Essentials permite que você gerencie todos os discos rígidos que estão conectados ao servidor por meio do Painel. Na guia **Armazenamento** do painel, **Discos Rígidos** exibe todos os discos rígidos que estão disponíveis no servidor para armazenar os backups de dados e do servidor. O servidor monitora o espaço disponível em cada disco rígido e exibe um alerta se o espaço em disco rígido se tornar baixo. A guia **Discos Rígidos** exibe as seguintes informações:  
  
- O nome de cada disco rígido  
  
- A capacidade de cada disco rígido  
  
- A quantidade de espaço usado em cada disco rígido  
  
- A quantidade de espaço livre em cada disco rígido  
  
- O status de cada unidade de disco rígido. Um status em branco significa que o disco rígido está funcionando corretamente  
  
- O painel de detalhes, que exibe todas as informações de pilha de armazenamento (para o pool de armazenamento, o espaço de armazenamento e o disco rígido) se o disco rígido selecionado estiver localizado em um espaço de armazenamento (em vez de um disco físico)  
  
  A tabela a seguir lista as tarefas de gerenciamento de disco rígido que estão disponíveis no Painel e suas descrições. Algumas das tarefas são exibidas apenas quando um disco rígido está selecionado.  
  
### <a name="available-hard-drive-management-tasks"></a>Tarefas de gerenciamento de disco rígido disponíveis  
  
|Nome da tarefa|Descrição|  
|---------------|-----------------|  
|**Exibir as propriedades do disco rígido**|Abre a página de**Propriedades** do _disco rígido_ . Essa tarefa é exibida quando o disco rígido é selecionado. A guia **Geral** da página Propriedades do *HardDriveName* inclui as seguintes tarefas adicionais:<br /><br /> -   **Limpeza da unidade**:  Permite que você limpe arquivos não utilizados no disco rígido (essa tarefa só está disponível no Windows Server Essentials).<br />-   **Verificar e reparar**:  Verifica o disco rígido quanto a erros de sistema de arquivos e tenta reparar automaticamente os erros detectados.<br /><br /> A guia **Cópias de Sombra** da página Propriedades do _HardDriveName_ **HardDriveName** permite habilitar cópias de sombra. Essa guia também exibe a próxima vez que as cópias de sombra estão agendadas para serem executadas.|  
|**Gerenciar espaços de armazenamento**|**Observação:** Para o Windows Server Essentials, essa tarefa só é exibida quando há um espaço de armazenamento existente.<br /><br /> Abre o painel de controle **Espaços de Armazenamento**, do qual você pode criar e gerenciar pools de armazenamento e espaços de armazenamento.|  
|**Criar um espaço de armazenamento**|Abre o assistente Criar um Espaço de Armazenamento, que permite que você use um ou mais discos rígidos para aumentar a capacidade de um pool de armazenamento.|  
|**Aumentar a capacidade do pool de armazenamento**|**Observação:** Essa tarefa é visível apenas se o disco rígido selecionado estiver localizado em um espaço de armazenamento.<br /><br /> Abre o assistente Aumentar a Capacidade de um Pool de Armazenamento, que permite que você use um ou mais discos rígidos para aumentar a capacidade de um pool de armazenamento.|  
  
##  <a name="BKMK_2"></a>Aumentar o armazenamento no servidor  
 Para aumentar o armazenamento no servidor, você pode adicionar um disco rígido interno adicional no servidor. Para adicionar o disco rígido interno adicional, você deve desligar o servidor, adicionar o disco rígido interno e reiniciar o servidor. Você não precisa desligar o servidor se o disco rígido estiver conectado ao controlador SCSI. Nesse caso, o disco rígido pode ser conectado enquanto o servidor está em execução.  
  
 Dependendo se o disco rígido a ser adicionado está formatado, siga um destes procedimentos:  
  
-   **Formatado** Se o disco rígido interno estiver formatado com NTFS ou ReFS, o servidor atribui uma letra da unidade a ele e o disco rígido aparece na guia **Discos Rígidos** . Agora você pode criar ou mover pastas do servidor para o novo disco rígido.  
  
-   **Não formatado** Se o disco rígido interno não estiver formatado, o seguinte alerta será exibido: Um ou mais discos rígidos não formatados estão conectados ao servidor. Use o procedimento a seguir para formatar o disco rígido.  
  
#### <a name="to-format-the-hard-disk"></a>Para formatar o disco rígido  
  
1.  Abra o Painel.  
  
2.  No painel de navegação, clique no ícone de alerta para iniciar o **Visualizador de alertas** no Windows Server Essentials ou na guia monitoramento de **integridade** no Windows Server Essentials.  
  
3.  No **Visualizador de Alerta** ou na guia **Monitoramento de Integridade** , clique no alerta e, no painel de tarefas, clique em **Solucionar este problema**.  
  
4.  Siga as instruções para concluir o assistente Adicionar um Novo Disco Rígido.  
  
###  <a name="BKMK_Clean"></a>Para limpar o disco rígido  
  
1.  Abra o Painel.  
  
2.  No painel de navegação, clique em **Armazenamento**e em **Discos Rígidos**.  
  
3.  Na seção **Discos rígidos**, selecione a letra da unidade que foi atribuída ao disco rígido recém-adicionado e, no painel de tarefas, clique em **Exibir as propriedades do disco rígido**.  
  
4.  Em **Propriedades de\> < DriveLetter**, na guia **geral** , clique em **limpeza da unidade**.  
  
##  <a name="BKMK_Check"></a>Executar verificações e reparos em discos rígidos  
 O processo de verificação e reparo do disco rígido verifica a integridade do sistema de arquivos armazenado no discos rígidos. Ele executa um processo **chkdsk** no volume em que os arquivos de backup estão armazenados. O alerta de problema a seguir pode ser resolvido executando uma verificação e reparo nos discos rígidos:  
  
-   Um ou mais discos rígidos no backup do servidor devem ser verificados.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Para verificar e reparar discos rígidos  
  
1.  Abra o Painel.  
  
2.  Clique em **Pastas do Servidor e Discos Rígidos**e em **Discos Rígidos**.  
  
3.  Selecione o disco rígido que está exibindo o erro e selecione **Exibir as propriedades do disco rígido**.  
  
4.  Na guia **Verificar e Reparar**, clique em **Verificar e Reparar**.  
  
##  <a name="BKMK_3"></a>Formatar discos rígidos  
 Quando um disco rígido interno não formatado é detectado no servidor, um alerta de integridade orienta o usuário pelo processo de formatação. O assistente Adicionar um Novo Disco Rígido lhe orienta pela formatação do disco rígido e permite que você o configure de uma das seguintes maneiras:  
  
1.  Formatar o disco rígido e criar automaticamente uma unidade nele. Se você escolher essa opção, quando o assistente for concluído, será criada uma unidade lógica de disco rígido formatada com o sistema de arquivos NTFS.  
  
2.  Formatar o disco rígido e configurá-lo para o Backup do Servidor. Se você escolher essa opção, o assistente Configurar Backup do Servidor é iniciado e lhe orienta pela configuração de backup do servidor.  
  
3.  Se um espaço de armazenamento não existir, use o novo disco rígido para criar um espaço de armazenamento. Você deve ter pelo menos dois discos rígidos para criar um espaço de armazenamento.  
  
4.  Se já existir um espaço de armazenamento, use o novo disco rígido para aumentar a capacidade de um pool de armazenamento. Esta opção é exibida apenas se houver um espaço de armazenamento existente criado no servidor. Se você escolher essa opção, o assistente adicionará esse disco rígido ao pool de armazenamento.  
  
##  <a name="BKMK_4"></a>Adicionar um novo disco rígido  
 Quando você conectar uma nova unidade de disco rígido em um servidor que executa o Windows Server Essentials, você pode:  
  
-   [Usar o novo disco rígido para armazenar pastas do servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Usar o novo disco rígido para armazenar backups do servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Usar o novo disco rígido para aumentar a capacidade do pool de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>Usar o novo disco rígido para armazenar pastas do servidor  
 Para usar o novo disco rígido para armazenar pastas do servidor, você pode adicionar uma nova pasta do servidor no disco rígido ou mover uma pasta do servidor existente para o disco rígido.  
  
##### <a name="to-store-server-folders"></a>Para armazenar pastas do servidor  
  
1. Abra o Painel.  
  
2. Clique na guia **ARMAZENAMENTO** e clique em **Pastas do Servidor**.  
  
3. No painel **Tarefas de Pastas do Servidor** , siga um destes procedimentos:  
  
   1.  Para adicionar uma pasta do servidor, clique em **Adicionar uma pasta**.  
  
   2.  Para mover uma pasta do servidor, selecione a pasta que você deseja mover para o novo disco rígido e clique em **Mover uma pasta**.  
  
   > [!NOTE]
   >  Se você navegar até o disco rígido e selecioná-lo como o local da pasta do servidor sem criar uma pasta, a seguinte mensagem de erro é exibida: **Um diretório raiz (como C:\\, D:\\) não pode ser adicionado como uma pasta de servidor. Crie uma nova pasta ou selecione uma existente no diretório raiz e tente novamente**. Para resolver esse erro, crie uma nova pasta na unidade de disco rígida recém-adicionada e selecione a nova pasta como o local para armazenar pastas do servidor.  
  
4. Siga as instruções para concluir o assistente.  
  
   Para obter mais informações sobre como mover pastas do servidor, consulte [Add or move a server folder](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="BKMK_4b"></a>Usar o novo disco rígido para armazenar backups do servidor  
 Você pode usar o disco rígido recém-adicionado para armazenar backups de servidor.  
  
##### <a name="to-store-server-backups"></a>Para armazenar backups do servidor  
  
1. Abra o Painel.  
  
2. Clique na guia **Dispositivos**, selecione o servidor no painel de lista e, no painel de tarefas, siga um destes procedimentos:  
  
   1. Se o backup do servidor não estiver configurado no servidor, clique em **Configurar Backup do Servidor**.  
  
   2. Se o backup do servidor estiver configurado no servidor, clique em **Personalizar o Backup do Servidor**.  
  
      O assistente Configurar Backup do Servidor é exibido.  
  
3. Na página **Selecionar o Destino do Backup** , selecione o novo disco rígido como o destino do backup.  
  
4. Siga as instruções para concluir o assistente.  
  
###  <a name="BKMK_4c"></a>Usar o novo disco rígido para aumentar a capacidade do pool de armazenamento  
 Quando a capacidade do pool de armazenamento for baixa, você receberá um alerta informando que é possível aumentar a capacidade do pool de armazenamento adicionando um novo disco rígido ao pool de armazenamento usando o assistente Aumentar a Capacidade de um Pool de Armazenamento.  
  
> [!NOTE]
>  Você pode executar este procedimento apenas se criou um pool de armazenamento no servidor.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Para aumentar a capacidade do pool de armazenamento  
  
1.  Abra o Painel.  
  
2.  Clique na guia **Armazenamento** e clique em **Discos Rígidos**.  
  
3.  Selecione a unidade que está mostrando uma baixa capacidade.  
  
4.  No painel de tarefas, selecione **Aumentar capacidade do pool de armazenamento**. O assistente Aumentar a Capacidade de um Pool de Armazenamento é exibido.  
  
5.  Siga as instruções para concluir o assistente.  
  
##  <a name="BKMK_5"></a>Visão geral dos espaços de armazenamento  
 Os espaços de armazenamento permitem que você agrupe discos em um pool de armazenamento. Em seguida, você pode usar a capacidade do pool para criar espaços de armazenamento. Os espaços de armazenamento são unidades virtuais que aparecem na guia **Discos Rígidos** do Painel. Você pode usar espaços de armazenamento como qualquer outra unidade, portanto, é fácil trabalhar com arquivos neles. Quando a capacidade do pool se torna baixa, você pode criar espaços de armazenamento de grandes e adicionar mais unidades ao pool de armazenamento. Se você tiver dois ou mais discos dentro do pool de armazenamento, poderá criar espaços de armazenamento com um espelho bidirecional que não será afetado por uma falha de unidade? ou até mesmo a falha de duas unidades? se você criar um espaço de armazenamento de espelho triplo.  
  
 Para criar um espaço de armazenamento, tudo o que você precisa é uma ou mais unidades adicionais além na unidade em que o Windows está instalado. Essas unidades podem ser discos rígidos internos ou externos ou unidades de estado sólido. Você pode usar uma variedade de tipos de unidades com espaços de armazenamento, inclusive unidades USB, SATA e SAS.  
  
> [!NOTE]
>  Se você configurar espaços de armazenamento em um servidor que executa o Windows Server Essentials, não será possível executar uma redefinição de fábrica com a opção **limpar dados** . A solução alternativa para esse problema é primeiro remover espaços de armazenamento e, em seguida, executar uma redefinição de fábrica com a opção **Limpar dados**.  
  
 Para obter mais informações sobre Espaços de Armazenamento, consulte as [Perguntas Frequentes de Espaços de Armazenamento](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="BKMK_6"></a>Criar um espaço de armazenamento  
 Para começar a trabalhar com espaços de armazenamento no servidor, os requisitos mínimos a seguintes devem ser atendidos:  
  
-   O servidor que executa o Windows Server Essentials deve ser conectado a unidades físicas adicionais (não apenas à unidade de inicialização), as unidades não devem hospedar nenhum volume e devem ter a capacidade mínima de 10 GB. É necessária uma unidade física para criar um pool de armazenamento. É necessário um mínimo de duas unidades físicas para criar um espaço de armazenamento de espelho resiliente.  
  
-   É necessário um mínimo de duas unidades físicas para criar um espaço de armazenamento com resiliência através de paridade ou espelhamento bidirecional.  
  
> [!NOTE]
>  Se você configurar espaços de armazenamento em um servidor que executa o Windows Server Essentials, não será possível executar uma redefinição de fábrica com a opção **limpar dados** . A solução alternativa para esse problema é primeiro remover espaços de armazenamento e, em seguida, executar uma redefinição de fábrica com a opção **Limpar dados**.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para criar um espaço de armazenamento no Windows Server Essentials  
  
1. Adicione ou conecte todas as unidades que você deseja agrupar com espaços de armazenamento no servidor que executa o Windows Server Essentials.  
  
2. No painel, clique em **avançado: Gerenciar espaços**de armazenamento.  
  
3. Clique em **Criar um novo pool e espaço de armazenamento**.  
  
4. Selecione as unidades que você deseja adicionar ao novo espaço de armazenamento e clique em **Criar pool**.  
  
5. Atribua um nome e uma letra à unidade e escolha um layout. **Espelhamento bidirecional**, **Espelho triplo**e **Paridade** podem ajudar a proteger os arquivos no espaço de armazenamento de falha da unidade.  
  
6. Insira o tamanho máximo que o espaço de armazenamento pode atingir e clique em **Criar espaço de armazenamento**.  
  
   No Windows Server Essentials, você pode criar um espaço de armazenamento espelhado bidirecional usando o assistente para criar um espaço de armazenamento no painel.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para criar um espaço de armazenamento no Windows Server Essentials  
  
1. Adicione ou conecte todas as unidades que você deseja agrupar com espaços de armazenamento no servidor que executa o Windows Server Essentials.  
  
2. No Painel, clique em **Gerenciar Espaços de Armazenamento**. O assistente Criar um Espaço de Armazenamento é exibido.  
  
3. Siga as instruções para concluir o assistente.  
  
   Para obter informações sobre como aumentar a capacidade do pool de armazenamento, consulte [usar o novo disco rígido para aumentar a capacidade do pool de armazenamento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar pastas do servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
