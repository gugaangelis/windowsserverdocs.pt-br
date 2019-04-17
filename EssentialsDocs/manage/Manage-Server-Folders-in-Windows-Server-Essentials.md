---
title: Gerenciar pastas de servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1f3640f21b95acafa850b2204cd52f9c0f324e5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Gerenciar pastas de servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Como um administrador de servidor, você pode gerenciar o acesso aos quais as pastas de servidor (conhecido como pastas compartilhadas quando acessado da barra inicial, acesso via Web remoto, aplicativo meu servidor para Windows Phone ou aplicativo meu servidor para Windows 8) no servidor, usando as tarefas no **pastas de servidor** guia do painel, permitindo que os usuários vários níveis de acesso a uma variedade de arquivos.  
  
 Os tópicos a seguir fornecem informações que ajudarão você a entender, criar e gerenciar pastas de servidor:  
  
-   [Gerenciar pastas de servidor usando o painel](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gerenciar o acesso a pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Adicionar ou mover uma pasta de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Adicionar uma pasta ausente do servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Entender as pastas compartilhadas](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Entender as cópias de sombra](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>Gerenciar pastas de servidor usando o painel  
 Windows Server Essentials torna possível realizar tarefas administrativas comuns usando o Dashboard. O **pastas de servidor** página do painel fornece os seguintes:  
  
-   Uma lista de pastas de servidor, que exibe:  
  
    -   O nome da pasta  
  
    -   Uma descrição da pasta  
  
    -   O local da pasta  
  
    -   A quantidade de espaço livre que está disponível no local de pasta  
  
    -   Informações de status breve sobre todas as tarefas que estão sendo executadas na pasta; o **Status** campo fica em branco se a pasta está saudável e se nenhuma tarefa estiver executando  
  
-   Um painel de detalhes que pode fornecer informações adicionais sobre uma pasta selecionada  
  
-   Um painel de tarefas que inclui um conjunto de tarefas administrativas relacionadas à pasta  
  
 A tabela a seguir descreve as várias tarefas de pasta de servidor que estão disponíveis no Windows Server Essentials painel. A maioria das tarefas é específicas da pasta, e eles só ficam visíveis quando você selecionar uma pasta na lista.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Tarefas de pasta de servidor no painel  
  
|Nome da tarefa|Descrição|  
|---------------|-----------------|  
|Abra a pasta|Exibe o conteúdo da pasta selecionada no Explorador de arquivos (chamado de Windows Explorer em versões anteriores do Windows).|  
|Exclua a pasta|Permite que você exclua uma pasta criados pelo usuário. Essa tarefa não está disponível para as pastas padrão que cria de instalação do servidor.|  
|Mova a pasta|Abre um assistente que ajuda você a migrar uma pasta no servidor para um novo local.|  
|Parar de compartilhar a pasta|Parar de compartilhar a pasta selecionada, mas não a exclui. Quando a pasta não está mais compartilhada, ele não aparece no painel. Essa tarefa não está disponível para as pastas padrão que cria de instalação do servidor.|  
|Exibir as propriedades da pasta|Exibe as propriedades de uma pasta selecionada e permite que você:<br /><br /> -Altere o nome das pastas criadas pelo usuário.<br /><br /> -Altere a descrição de uma pasta selecionada.<br /><br /> -Exiba o tamanho da pasta.<br /><br /> – Abra a pasta selecionada no Explorador de arquivos.<br /><br /> – Especifique as permissões de acesso de conta de usuário para uma pasta selecionada.<br /><br /> -Oculte uma pasta selecionada de aplicativos de acesso via Web remoto e o serviço Web.<br /><br /> – Especifique a cota de pasta.|  
|Adicionar uma pasta|Ajuda você a criar uma nova pasta no servidor e atribuir o nível de acesso permitido para cada conta de usuário.|  
|Noções básicas sobre pastas de servidor|Abre um tópico da Ajuda na Internet que descreve o uso e a funcionalidade de pastas de servidor.|  
  
##  <a name="BKMK_1"></a>Gerenciar o acesso a pastas de servidor  
 Windows Server Essentials permite armazenar arquivos que estão localizados em seus computadores cliente para um local central usando pastas do servidor. Armazenar seus arquivos em pastas de servidor garante que seus arquivos estejam em um local que está sempre acessível de maneira segura de cada cliente.  
  
 Usar pastas do servidor para armazenar seus arquivos permite que você:  
  
-   Fazer backup da pasta de servidor usando o servidor de Backup e restauração para ajudar a proteger contra falha total do servidor.  
  
-   Acessar arquivos armazenados na pasta de servidor de qualquer local usando um navegador da Internet por meio de acesso via Web remoto ou por meio dos aplicativos de meu servidor para Windows Phone e Windows 8.  
  
-   Acesse a nova pasta no servidor de qualquer computador cliente.  
  
 Você pode gerenciar o acesso aos quais as pastas de servidor no servidor, usando as tarefas no **pastas de servidor** guia do painel. A tabela a seguir lista as pastas de servidor que são criadas por padrão quando você instala o Windows Server Essentials ou ativar streaming de mídia em seu servidor.  
  
|Nome da pasta de servidor|Descrição|  
|------------------------|-----------------|  
|Backups do computador cliente|Por padrão, o Windows Server Essentials cria cliente backups de computador que são armazenados nessa pasta. As configurações para Backups de computador cliente podem ser modificadas pelo administrador da rede.|  
|Empresa|Usado para armazenar e acessar documentos relacionados à sua organização pelos usuários da rede.|  
|Backups do histórico de arquivos|Por padrão, o Windows Server Essentials usa histórico de arquivos para criar backups de arquivos que são armazenados nessa pasta. Essas configurações de histórico de arquivos podem ser modificadas por administradores de rede.|  
|Redirecionamento de pasta|Usado para armazenar e acessar as pastas que estão configuradas para redirecionamento de pasta pelos usuários da rede.|  
|Usuários|Usado para armazenar e acessar arquivos por usuários de rede. Uma pasta específica do usuário é gerada automaticamente no **usuários** pasta de servidor para cada conta de usuário de rede que você criar.|  
|Música|Usado para armazenar e acessar arquivos de música pelos usuários da rede. Esta pasta está disponível quando você ativar o compartilhamento de mídia.|  
|Imagens|Usado para armazenar e acessar arquivos de imagem por usuários da rede. Esta pasta está disponível quando você ativar o compartilhamento de mídia.|  
|Programas de TV gravados|Usado para armazenar e acessar programas de TV gravados pelos usuários da rede. Esta pasta está disponível quando você ativar o compartilhamento de mídia.|  
|Vídeos|Usado para armazenar e acessar arquivos de vídeo por usuários da rede. Esta pasta está disponível quando você ativar o compartilhamento de mídia.|  
  
 Para ocultar ou definir permissões para pastas de servidor ou modificar propriedades da pasta de servidor, consulte os procedimentos a seguir:  
  
-   [Ocultar pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Definir permissões para pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Exibir ou modificar propriedades da pasta de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>Ocultar pastas de servidor  
 Como um administrador de rede, você pode optar por ocultar qualquer uma dessas pastas de servidor e impedir que está sendo exibido no site de acesso via Web remoto ou aplicativos de serviços Web (por exemplo, meu servidor).  
  
> [!NOTE]
>  Você deve ser um administrador de rede para executar este procedimento.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Para ocultar as pastas de servidor de sendo exibido no acesso via Web remoto  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Na exibição de lista, selecione a pasta de servidor cujas propriedades você deseja exibir ou modificar.  
  
4.  No **< ServerFolder\ > tarefas** painel, clique em **exibir propriedades da pasta**.  
  
5.  Em **< FolderName\ > propriedades**, clique em **compartilhamento**, selecione **ocultar essa pasta de aplicativos de acesso via Web remoto e o serviço Web**e clique em **aplicar**.  
  
###  <a name="BKMK_Perms"></a>Definir permissões para pastas de servidor  
 Para qualquer pasta de servidor adicionais que você adiciona no servidor usando o painel, você pode escolher três configurações diferentes de acesso para ele:  
  
-   **Leitura/gravação**  
  
     Escolha essa configuração se você quiser permitir que esta pessoa criar, alterar e excluir todos os arquivos na pasta do servidor.  
  
-   **Somente leitura**  
  
     Escolha essa configuração se você quiser permitir que esta pessoa apenas ler os arquivos na pasta do servidor. Os usuários com acesso somente leitura não podem criar, alterar ou excluir todos os arquivos na pasta do servidor.  
  
-   **Sem acesso**  
  
     Escolha essa configuração se você não quiser que essa pessoa para acessar todos os arquivos na pasta do servidor.  
  
> [!IMPORTANT]
>  As permissões que são exibidas nas propriedades da pasta representam apenas os usuários que são gerenciados pelo painel. Elas não incluem as permissões de usuário, como grupos ou contas de serviço ou incluir nenhuma permissão que pode ser definida na pasta usando outras ferramentas nativas ou incluem usuários que não foram adicionados por meio do painel.  
  
> [!NOTE]
>  Você deve ser um administrador de rede para executar este procedimento.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Para definir as permissões para pastas de servidor no servidor  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Na exibição de lista, selecione a pasta de servidor cujas propriedades você deseja exibir ou modificar.  
  
4.  No **< ServerFolder\ > tarefas** painel, clique em **exibir propriedades da pasta**.  
  
5.  Em **< FolderName\ > propriedades**, clique em **compartilhamento**, selecione o nível de acesso do usuário apropriado para as contas de usuário listada e, em seguida, clique em **aplicar**.  
  
> [!NOTE]
>  Por padrão, quando você adiciona uma conta de usuário à sua rede, uma subpasta é criada para o usuário sob o **usuários** pasta no servidor. A subpasta pode ser acessada de um computador de rede, somente o usuário ou o administrador. As permissões são definidas para cada subpasta sob **usuários**, portanto, não há nenhuma permissão de acesso geral para o nível superior **usuários** pasta.  
  
> [!NOTE]
>  Você não pode modificar as permissões de compartilhamento para o **Backups de histórico de arquivos**, **redirecionamento de pasta**, e **usuários** pastas do servidor. Portanto, as propriedades da pasta dessas pastas de servidor não incluem um **compartilhamento** guia.  
  
###  <a name="BKMK_10"></a>Exibir ou modificar propriedades da pasta de servidor  
 Você pode modificar o nome da pasta do servidor, sua descrição e definir quais contas de usuário têm acesso a uma pasta de servidor por meio do **exibir as propriedades da pasta** de tarefas no **pastas de servidor** guia do painel.  
  
> [!NOTE]
>  No Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, você também pode modificar a cota de pasta.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Para exibir ou modificar propriedades da pasta  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Na exibição de lista, selecione a pasta de servidor cujas propriedades você deseja exibir ou modificar.  
  
4.  No **< ServerFolder\ > tarefas** painel, clique em **exibir propriedades da pasta**.  
  
5.  Em **< Foldername\ > propriedades**diante do **geral** guia, visualizar ou modificar o nome e a descrição da pasta do servidor.  
  
    > [!NOTE]
    >  No Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, você também pode modificar a cota de pasta que oferece uma mensagem de aviso quando uma pasta de servidor atinge o tamanho especificado.  
  
##  <a name="BKMK_5"></a>Adicionar ou mover uma pasta de servidor  
 Você pode **adicionar pastas de servidor mais** para armazenar seus arquivos no servidor além as pastas de servidor padrão que são criados durante a instalação. Você pode adicionar pastas de servidor no servidor principal ou em um servidor membro executando o Windows Server Essentials.  
  
 Você pode **mover uma pasta de servidor** que está localizado no servidor principal executando o Windows Server Essentials e é exibido no **pastas de servidor** guia do painel para outro disco rígido quando necessário usando a movimentação de um Assistente de pasta. Você pode mover uma pasta de servidor para outro endereço de local do disco rígido se:  
  
-   O disco rígido de dados não tem espaço suficiente para armazenar dados.  
  
-   Você deseja alterar o local de armazenamento padrão. Para um movimento mais rápido, considere mover a pasta de servidor enquanto ele não inclui todos os dados.  
  
-   Você deseja remover o disco rígido existente sem perder as pastas de servidor que estão localizadas no-lo.  
  
 Antes de se mover a pasta, verifique o seguinte:  
  
-   Certifique-se de que você tiver feito backup do servidor.  
  
-   Certifique-se de que todos os backups de cliente são interrompidos e não está em andamento se você planeja migrar a pasta de Backup do computador cliente. Ao mover a pasta de Backup do computador cliente, o servidor não poderão fazer backup de todos os computadores cliente até que o movimento da pasta seja concluído.  
  
-   Certifique-se de que o servidor não está realizando operações críticas do sistema. É recomendável que você concluir as atualizações ou mova backups que estão em andamento, antes de começar a uma pasta ou o processo pode levar mais tempo para concluir.  
  
-   Nenhum dos arquivos na pasta seja movido estão em uso. Não será possível acessar a pasta de servidor enquanto ele está sendo movido.  
  
 Mover uma pasta do NTFS para ReFS não é suportado se os arquivos das pastas de servidor implementam as seguintes tecnologias:  
  
-   Fluxos de dados alternativos  
  
-   IDs de objeto  
  
-   Nomes curtos (8.3 nomes)  
  
-   Compactação  
  
-   Criptografia de EFS  
  
-   NTFS Transacional, TxF (introduzido com o Windows Vista)  
  
-   Arquivos esparsos  
  
-   Links físicos  
  
-   Atributos estendidos  
  
-   Cotas  
  
###  <a name="BKMK_6"></a>Onde adicionar ou mover uma pasta de servidor  
 Normalmente, você deve adicionar ou mover as pastas de servidor em discos rígidos que têm a quantidade máxima de espaço livre. Se possível, evite adicionar ou mover uma pasta compartilhada para a unidade do sistema (por exemplo, c:.) que pode levar imediatamente o espaço em disco necessário que é necessário para o sistema operacional e suas atualizações. Além disso, evite adicionando ou movendo pastas do servidor para um disco rígido externo, porque eles podem ser facilmente desconectados, e como resultado, você não poderá acessar seus arquivos. Em vez disso, recomendamos que você crie a pasta em uma unidade interna.  
  
 Uma pasta de servidor não pode ser adicionada ou movida para os seguintes locais e resultará em um erro se qualquer um desses locais será selecionado para adições ou movimentos:  
  
-   Um disco rígido não está formatado com o sistema de arquivos NTFS ou ReFS  
  
-   A pasta % windir %  
  
-   Uma unidade de rede mapeadas  
  
-   Uma pasta que contém uma pasta compartilhada  
  
-   Um disco rígido que está localizado em **dispositivo com o armazenamento removível**  
  
-   Um diretório raiz de um disco rígido (por exemplo, C:\\, D:\\, E:\\)  
  
-   Uma subpasta de uma pasta compartilhada existente  
  
-   Um servidor membro executando o Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Etapas para adicionar ou mover uma pasta de servidor  
  
> [!NOTE]
>  Você deve ser um administrador de servidor para concluir esses procedimentos.  
  
##### <a name="to-add-a-server-folder"></a>Para adicionar uma pasta de servidor  
  
1.  Abra o painel.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Em **servidor pasta tarefas**, clique em **adicionar uma pasta**. Isso inicia o Assistente adicionar um pasta.  
  
4.  Siga as instruções para concluir o assistente.  
  
    > [!NOTE]
    >  -   Se você navegar para uma pasta específica usando o botão Procurar para especificar o local da pasta de servidor, a pasta que você navegou é adicionada como uma pasta no servidor.  
    > -   Você pode definir quais pastas do servidor podem ser acessadas por meio do acesso via Web remoto. Para obter mais informações, consulte [gerenciar o acesso a pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Para mover uma pasta de servidor  
  
1.  Abra o painel.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Na lista de pastas de servidor, selecione a pasta que você deseja mover.  
  
4.  No painel de tarefas, clique em **mover a pasta**.  
  
5.  Siga as instruções para concluir o assistente.  
  
##  <a name="BKMK_9"></a>Adicionar uma pasta ausente do servidor  
 Quando o servidor detecta que uma pasta de servidor predefinidos? Empresa, os usuários, Backups de computador cliente, Backup do histórico de arquivos ou redirecionamento de pasta? não está mais compartilhado (por algum motivo ou outro), um alerta é gerado para orientar o usuário para resolver esse problema. É recomendável que você tente e restaurar a pasta de backup do servidor. No entanto, se o servidor não foi feito, selecione a pasta ausente e, em seguida, clique em **recriar a pasta ausente** reconfigurar o local da pasta do servidor.  
  
> [!NOTE]
>  Somente pastas predefinidas? Empresa, os usuários, Backups de computador cliente, Backup do histórico de arquivos ou redirecionamento de pasta? podem ser recriados. Pastas de servidor criados pelo usuário e as pastas de servidor de mídia não podem ser recriadas.  
  
 Depois de restaurar ou recriar a pasta ausente, ele não deve estar listado como **ausente**.  
  
 Para obter informações sobre como restaurar arquivos de backups de servidor, consulte a seção Saiba mais sobre como restaurar arquivos e pastas no tópico [Gerenciar Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_11"></a>Entender as pastas compartilhadas  
 Há várias maneiras diferentes que você pode acessar suas pastas compartilhadas no Windows Server Essentials de um dispositivo que está conectado ao servidor. Para obter mais informações, consulte o tópico [usar pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Shadow"></a>Entender as cópias de sombra  
 Com servidor cópias de sombra, os usuários podem exibir arquivos e pastas compartilhados que existia em pontos de tempo no passado. Acessando versões anteriores de arquivos ou cópias de sombra, é útil porque os usuários podem:  
  
1.  **Recuperar arquivos que foram acidentalmente excluídos**. Se você acidentalmente excluir um arquivo, você pode abrir uma versão anterior e copiá-lo para um local seguro.  
  
2.  **Recuperar um arquivo substituído acidentalmente**. Se você substituir um arquivo acidentalmente, você pode recuperar uma versão anterior do arquivo. (O número de versões depende quantidade de instantâneos que você criou.)  
  
3.  **Compare versões de um arquivo enquanto trabalha**. Você pode usar versões anteriores quando quiser verificar as mudanças entre versões de um arquivo.  
  
 Para usar cópias de sombra, de um computador cliente, clique com botão direito uma pasta compartilhada do servidor e selecione **restaurar a versão anterior**.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerencie o armazenamento de servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Use pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
