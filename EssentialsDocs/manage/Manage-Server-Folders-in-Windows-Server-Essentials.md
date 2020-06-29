---
title: Gerenciar Pastas do Servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8dba2bb0b282751fbbe584b64dbc92bbe3702979
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470561"
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Gerenciar Pastas do Servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Como administrador de servidor, você pode gerenciar o acesso a qualquer pasta de servidor (conhecida como pastas compartilhadas quando acessada no Launchpad, Acesso via Web remoto, meu aplicativo de servidor para Windows Phone ou meu aplicativo de servidor para Windows 8) no servidor usando as tarefas na guia **pastas de servidor** do painel, permitindo que os usuários tenham vários níveis de acesso a uma variedade de arquivos.

 Os tópicos a seguir fornecem informações que o ajudarão a compreender, criar e gerenciar as pastas do servidor:

-   [Gerenciar pastas do servidor usando o Painel](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)

-   [Gerenciar o acesso a pastas do servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)

-   [Adicionar ou mover uma pasta de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)

-   [Adicionar uma pasta de servidor ausente](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)

-   [Noções básicas das pastas compartilhadas](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)

-   [Noções básicas das cópias de sombra](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)

##  <a name="manage-server-folders-using-the-dashboard"></a><a name="BKMK_2"></a>Gerenciar pastas do servidor usando o painel
 O Windows Server Essentials possibilita realizar tarefas administrativas comuns usando o Painel. A página **Pastas do Servidor** do Painel oferece o seguinte:

- Uma lista de pastas do servidor, que exibe:

  -   O nome da pasta

  -   Uma descrição da pasta

  -   O local da pasta

  -   A quantidade de espaço livre disponível no local da pasta

  -   Breves informações de status sobre as tarefas que estão sendo executadas na pasta; o campo **Status** fica em branco se a pasta estiver íntegra e não houver tarefas em execução

- Um painel de detalhes que pode fornecer informações adicionais sobre uma pasta selecionada

- Um painel de tarefas que inclui um conjunto de tarefas administrativas relacionadas à pasta

  A tabela a seguir descreve as diversas tarefas de pasta do servidor que estão disponíveis no Painel do Windows Server Essentials. A maioria das tarefas são específicas da pasta e são visíveis apenas quando você seleciona uma pasta da lista.

### <a name="server-folder-tasks-on-the-dashboard"></a>Tarefas de pasta do servidor no Painel

|Nome da tarefa|Descrição|
|---------------|-----------------|
|Abrir a pasta|Exibe o conteúdo da pasta selecionada no Explorador de Arquivos (chamado Windows Explorer em versões anteriores do Windows).|
|Excluir a pasta|Permite que você exclua uma pasta criada pelo usuário. Esta tarefa não está disponível para pastas padrão criadas pela instalação do servidor.|
|Mover a pasta|Abre um assistente que ajuda você a mover uma pasta de servidor para um novo local.|
|Interromper o compartilhamento de pasta|Interrompe o compartilhamento da pasta selecionada, mas não a exclui. Quando a pasta não está mais compartilhada, ele não aparece no Painel. Esta tarefa não está disponível para pastas padrão criadas pela instalação do servidor.|
|Exibir as propriedades de pasta|Exibe as propriedades de uma pasta selecionada e permite que você:<br /><br /> -Alterar o nome das pastas criadas pelo usuário.<br /><br /> -Alterar a descrição de uma pasta selecionada.<br /><br /> -Exiba o tamanho da pasta.<br /><br /> -Abra a pasta selecionada no explorador de arquivos.<br /><br /> -Especificar permissões de acesso à conta de usuário para uma pasta selecionada.<br /><br /> -Ocultar uma pasta selecionada de Acesso via Web remota e aplicativos de serviço Web.<br /><br /> -Especifique a cota da pasta.|
|Adicionar uma pasta|Ajuda a criar uma nova pasta no servidor e a atribuir o nível de acesso permitido para cada conta de usuário.|
|Noções básicas sobre pastas do servidor|Abre um tópico da Ajuda na Internet que descreve o uso e a funcionalidade de pastas do servidor.|

##  <a name="manage-access-to-server-folders"></a><a name="BKMK_1"></a>Gerenciar o acesso às pastas do servidor
 Windows Server Essentials permite que você armazene arquivos que estão localizados nos computadores cliente para um local central usando pastas do servidor. Armazenar os arquivos em pastas do servidor garante que os arquivos estejam em um local que seja sempre acessível de uma maneira segura a partir de cada cliente.

 Usar pastas do servidor para armazenar os arquivos permite que você:

- Faça backup da pasta de servidor usando o Servidor de backup e restauração para ajudar na proteção contra falha total do servidor.

- Acesse os arquivos que estão armazenados na pasta do servidor a partir de qualquer local usando um navegador da Internet por meio do Acesso via Web remoto ou por meio de aplicativos My Server para Windows Phone e Windows 8.

- Acesse a nova pasta no servidor a partir de qualquer computador cliente.

  Você pode gerenciar o acesso às pastas de qualquer servidor no servidor usando as tarefas da guia **Pastas do servidor** no Painel. A tabela a seguir lista as pastas do servidor que são criadas por padrão quando você instala o Windows Server Essentials ou ativa o streaming de mídia no seu servidor.

|Nome de pasta do servidor|Descrição|
|------------------------|-----------------|
|Backups do computador cliente|Por padrão, o Windows Server Essentials cria backups de computadores cliente que são armazenados nessa pasta. As configurações de Backups do computador cliente podem ser modificadas pelo administrador da rede.|
|Empresa|Usado para armazenar e acessar documentos relacionados à sua organização pelos usuários da rede.|
|Backups do histórico de arquivos|Por padrão, o Windows Server Essentials usa o histórico de arquivos para criar backups de arquivos que estão armazenados nesta pasta. Essas configurações do histórico de arquivos podem ser modificadas por administradores de rede.|
|Redirecionamento de pasta|Usado para armazenar e acessar pastas que estão configuradas para redirecionamento de pasta pelos usuários da rede.|
|Usuários|Usada para armazenar e acessar arquivos por usuários da rede. Uma pasta específica do usuário é gerada automaticamente na pasta **Usuários** do servidor para cada conta de usuário de rede que você criar.|
|Música|Usado para armazenar e acessar arquivos de música pelos usuários da rede. Esta pasta está disponível quando você ativa o compartilhamento de mídia.|
|Imagens|Usado para armazenar e acessar arquivos de imagem pelos usuários da rede. Esta pasta está disponível quando você ativa o compartilhamento de mídia.|
|TV gravada|Usada para armazenar e acessar programas de TV gravados pelos usuários da rede. Esta pasta está disponível quando você ativa o compartilhamento de mídia.|
|vídeos|Usado para armazenar e acessar arquivos de vídeo pelos usuários da rede. Esta pasta está disponível quando você ativa o compartilhamento de mídia.|

 Para ocultar ou definir permissões para pastas de servidor ou para modificar as propriedades da pasta do servidor, consulte os procedimentos a seguir:

-   [Ocultar pastas do servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)

-   [Definir permissões para pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)

-   [Exibir ou modificar as propriedades de pasta do servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)

###  <a name="hide-server-folders"></a><a name="BKMK_Hide"></a>Ocultar pastas do servidor
 Como administrador de rede, você pode optar por ocultar qualquer uma dessas pastas de servidor e impedir que elas sejam exibidas no site de Acesso Remoto via Web ou nos aplicativos de serviços Web (como o My Server).

> [!NOTE]
>  Você deve ser um administrador de rede para executar este procedimento.

##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Para ocultar pastas do servidor para que não sejam exibidas no Acesso Remoto via Web

1.  Abra o Painel do Windows Server Essentials.

2.  Clique em **ARMAZENAMENTO** e clique em **Pastas do servidor**.

3.  Na exibição de lista, selecione a pasta do servidor cujas propriedades você deseja exibir ou modificar.

4.  No painel ** \> tarefas do<ServerFolder** , clique em **Exibir Propriedades da pasta**.

5.  Nas ** \> Propriedades<nome_da_pasta**, clique **em compartilhamento**, selecione **ocultar esta pasta de acesso via Web remoto e aplicativos de serviço Web**e, em seguida, clique em **aplicar**.

###  <a name="set-permissions-to-server-folders"></a><a name="BKMK_Perms"></a>Definir permissões para pastas do servidor
 Para quaisquer pastas de servidor adicionais que adicionar ao servidor usando o Painel, você pode escolher três configurações diferentes de acesso:

-   **Leitura/gravação**

     Escolha essa configuração se quiser permitir que a pessoa crie, altere ou exclua quaisquer arquivos na pasta do servidor.

-   **Somente leitura**

     Escolha essa configuração se quiser permitir apenas que a pessoa leia os arquivos na pasta do servidor. Usuários com acesso somente leitura não podem criar, alterar ou excluir os arquivos na pasta do servidor.

-   **Sem acesso**

     Escolha esta opção se você não quiser que a pessoa acesse os arquivos na pasta do servidor.

> [!IMPORTANT]
>  As permissões que são exibidas nas propriedades da pasta representam apenas os usuários que são gerenciados pelo Painel. Elas não incluem permissões de usuário como contas de serviço ou grupos, não incluem permissões que podem ser definidas na pasta usando outras ferramentas nativas, nem incluem usuários que não foram adicionados por meio do Painel.

> [!NOTE]
>  Você deve ser um administrador de rede para executar este procedimento.

##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Para definir permissões para pastas de servidor no servidor

1.  Abra o Painel do Windows Server Essentials.

2.  Clique em **ARMAZENAMENTO** e clique em **Pastas do servidor**.

3.  Na exibição de lista, selecione a pasta do servidor cujas propriedades você deseja exibir ou modificar.

4.  No painel ** \> tarefas do<ServerFolder** , clique em **Exibir Propriedades da pasta**.

5.  Nas ** \> Propriedades<nome_da_pasta**, clique em **compartilhamento**e selecione o nível de acesso do usuário apropriado para as contas de usuário listadas e clique em **aplicar**.

> [!NOTE]
>  Por padrão, quando você adiciona uma conta de usuário à rede, uma subpasta é criada para o usuário na pasta **Usuários** no servidor. A subpasta pode ser acessada a partir de um computador da rede, somente pelo usuário ou administrador. As permissões são definidas para cada subpasta em **Usuários**, portanto, não há permissões de acesso geral para a pasta de nível superior **Usuários**.

> [!NOTE]
>  Não é possível modificar as permissões de compartilhamento das pastas **Backups do histórico de arquivos**, **Redirecionamento de pasta** e **Usuários** do servidor. Assim, as propriedades dessas pastas de servidor não incluem uma guia **Compartilhamento**.

###  <a name="view-or-modify-server-folder-properties"></a><a name="BKMK_10"></a>Exibir ou modificar propriedades da pasta do servidor
 Você pode modificar o nome da pasta do servidor, sua descrição e definir quais contas de usuário têm acesso a uma pasta no servidor por meio da tarefa **Exibir as propriedades da pasta** na guia **Pastas do servidor** do Painel.

> [!NOTE]
>  No Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, você também pode modificar a cota da pasta.

##### <a name="to-view-or-modify-folder-properties"></a>Para exibir ou modificar as propriedades de pasta

1.  Abra o Painel do Windows Server Essentials.

2.  Clique em **ARMAZENAMENTO** e clique em **Pastas do servidor**.

3.  Na exibição de lista, selecione a pasta do servidor cujas propriedades você deseja exibir ou modificar.

4.  No painel ** \> tarefas do<ServerFolder** , clique em **Exibir Propriedades da pasta**.

5.  Nas ** \> Propriedades<nome_da_pasta**, na guia **geral** , exiba ou modifique o nome e a descrição da pasta do servidor.

    > [!NOTE]
    >  No Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, você também pode modificar a cota da pasta que fornece uma mensagem de aviso quando uma pasta do servidor atinge seu tamanho especificado.

##  <a name="add-or-move-a-server-folder"></a><a name="BKMK_5"></a>Adicionar ou mover uma pasta do servidor
 Você pode **Adicionar mais pastas do servidor** para armazenar os arquivos no servidor, além de pastas do servidor padrão que são criadas durante a instalação. Você pode adicionar pastas do servidor no servidor primário ou em um servidor membro que executa o Windows Server Essentials.

 Você pode **Mover uma pasta de servidor** que está localizada no servidor primário que executa o Windows Server Essentials e é exibida na guia **Pastas do servidor** do Painel para outra unidade de disco rígido, quando necessário, usando o Assistente de Adição de Pasta. Você pode mover uma pasta de servidor para outro endereço local de unidade de disco rígido se:

- A unidade de disco rígido de dados não tiver espaço suficiente para armazenar dados.

- Você quiser alterar o local de armazenamento padrão. Para um movimento mais rápido, considere a possibilidade de mover a pasta do servidor enquanto ele não inclui nenhum dado.

- Você deseja remover o disco rígido existente sem perder as pastas do servidor que estão localizadas nele.

  Antes de mover a pasta, verifique o seguinte:

- Certifique-se de que você fez backup do servidor.

- Certifique-se de que todos os backups do cliente foram interrompidos e não estão em andamento, se você planeja migrar a pasta de Backup do computador cliente. Ao mover a pasta de Backup do computador cliente, o servidor não poderá fazer backup de nenhum dos computadores cliente até que a movimentação de pasta esteja concluída.

- Certifique-se de que o servidor não está executando operações críticas do sistema. É recomendável que você conclua todas as atualizações ou backups que estiverem em andamento antes de uma movimentação de pasta, ou o processo pode levar mais tempo para ser concluído.

- Nenhum dos arquivos na pasta a ser movida estão em uso. Não será possível acessar a pasta do servidor enquanto ele é movido.

  Não é possível mover uma pasta do NTFS para ReFS se os arquivos nas pastas do servidor implementam as tecnologias a seguir:

- Fluxos de dados alternados

- Identificadores de objeto

- Nomes curtos (nomes 8.3)

- Compactação

- Criptografia EFS

- NTFS Transacional, TxF (introduzido com o Windows Vista)

- Arquivos esparsos

- Links físicos

- Atributos estendidos

- Cotas

###  <a name="where-to-add-or-move-a-server-folder"></a><a name="BKMK_6"></a>Onde adicionar ou mover uma pasta de servidor
 Normalmente, você deve adicionar ou mover as pastas de servidor para unidades de disco rígido com a quantidade máxima de espaço livre. Se possível, evite adicionar ou mover uma pasta compartilhada para a unidade do sistema (por exemplo, c:), pois isso pode ocupar imediatamente o espaço em disco necessário para o sistema operacional e suas atualizações. Além disso, evite adicionar ou mover pastas do servidor para um disco rígido externo, porque eles podem ser facilmente desconectados e, como resultado, você não poderá acessar seus arquivos. Em vez disso, é recomendável que você crie a pasta em uma unidade interna.

 Uma pasta no servidor não pode ser adicionada ou movida para os seguintes locais, e ocorrerá em um erro se algum desses locais estiver marcado para acréscimos ou movimentações:

-   Um disco rígido que não está formatado com o sistema de arquivos NTFS ou ReFS

-   A pasta % windir %

-   Uma unidade de rede mapeada

-   Uma pasta que contenha uma pasta compartilhada

-   Um disco rígido que esteja localizado em um **Dispositivo com armazenamento removível**

-   Um diretório raiz de uma unidade de disco rígido (como C: \\ , D: \\ , e: \\ )

-   Uma subpasta de uma pasta compartilhada existente

-   Um servidor membro executando o Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada

### <a name="steps-to-add-or-move-a-server-folder"></a>Etapas para adicionar ou mover uma pasta de servidor

> [!NOTE]
>  Você deve ser um administrador de servidor para executar estes procedimentos.

##### <a name="to-add-a-server-folder"></a>Para adicionar uma pasta de servidor

1. Abra o Painel.

2. Clique em **ARMAZENAMENTO** e clique em **Pastas do servidor**.

3. Em **Tarefas de pasta do servidor**, clique em **Adicionar uma pasta**. Isso inicia o Assistente de Adição de Pasta.

4. Siga as instruções para concluir o assistente.

   > [!NOTE]
   > - Se você navegar para uma pasta específica usando o botão Procurar para especificar o local da pasta de servidor, a pasta para a qual você navegou será adicionada como uma pasta no servidor.
   >   -   Você pode definir quais pastas de servidor podem ser acessadas por meio do Acesso Remoto via Web. Para obter mais informações, consulte [Gerenciar o acesso a pastas do servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).

##### <a name="to-move-a-server-folder"></a>Para mover uma pasta de servidor

1.  Abra o Painel.

2.  Clique em **ARMAZENAMENTO** e clique em **Pastas do servidor**.

3.  Na lista de pastas do servidor, selecione a pasta que você deseja mover.

4.  No painel Tarefas, clique em **Mover a pasta**.

5.  Siga as instruções para concluir o assistente.

##  <a name="add-a-missing-server-folder"></a><a name="BKMK_9"></a>Adicionar uma pasta de servidor ausente
 Quando o servidor detecta que uma pasta de servidor predefinida? Empresa, usuários, backups de computadores cliente, backup de histórico de arquivos ou redirecionamento de pasta? não é mais compartilhado (por algum motivo ou outro), um alerta é gerado para orientar o usuário a resolver esse problema. É recomendável que você tente restaurar a pasta do backup do servidor. No entanto, se o backup do servidor não tiver sido feito, selecione a pasta ausente e, em seguida, clique em **Recriar a pasta ausente** para reconfigurar o local da pasta do servidor.

> [!NOTE]
>  Somente pastas predefinidas? Empresa, usuários, backups de computadores cliente, backup de histórico de arquivos ou redirecionamento de pasta? podem ser recriados. Pastas do servidor criadas pelo usuário e pastas do servidor de mídia não podem ser recriadas.

 Após a pasta ausente ser recriada ou restaurada, ele não deverá estar listada como **Ausente**.

 Para obter informações sobre como restaurar arquivos de backups de servidor, consulte a seção saiba mais sobre como restaurar arquivos e pastas no tópico [gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).

##  <a name="understand-shared-folders"></a><a name="BKMK_11"></a>Entender pastas compartilhadas
 Há várias maneiras diferentes de acessar as pastas compartilhadas no Windows Server Essentials a partir de um dispositivo conectado ao servidor. Para obter mais informações, consulte o tópico [usar pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).

##  <a name="understand-shadow-copies"></a><a name="BKMK_Shadow"></a>Entender as cópias de sombra
 Com cópias de sombra do servidor, os usuários podem exibir arquivos e pastas compartilhadas como eram no passado. Acessar versões anteriores de arquivos ou cópias de sombra é útil porque os usuários podem:

1. **Recuperar arquivos que foram excluídos acidentalmente**. Se excluir um arquivo acidentalmente, você pode abrir uma versão anterior e copiá-lo para um local seguro.

2. **Recuperar a substituição acidental de um arquivo**. Se substituir um arquivo acidentalmente, você pode recuperar uma versão anterior do arquivo. (O número de versões depende quantos instantâneos você criou.)

3. **Comparar versões de um arquivo durante o trabalho**. Você pode usar versões anteriores quando quiser verificar o que foi alterado entre as versões de um arquivo.

   Para usar cópias de sombra, de um computador cliente, clique em uma pasta compartilhada do servidor e selecione **Restaurar versão anterior**.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar o armazenamento do servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md)

-   [Usar pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
