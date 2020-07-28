---
title: Utilizar o acesso remoto via Web no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 276e39cc3e17ce74f7fee43c512cc726ff631a5a
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87179562"
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Utilizar o acesso remoto via Web no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

  O Acesso via Web remoto é um recurso do Windows Servers Essentials que permite que você acesse arquivos/pastas e computadores na rede por meio de um navegador da Web de qualquer lugar com conectividade com a Internet.

  O Acesso via Web remoto  ajuda você a permanecer conectado à sua rede do Windows Server Essentials quando estiver ausente. Ao fazer logon no Acesso via Web remoto, você pode se conectar aos computadores na rede do Windows Server Essentials, abrir o painel para gerenciar sua rede do Windows Server Essentials e acessar todas as pastas compartilhadas e arquivos de mídia no servidor.

 Este tópico inclui as seções a seguir:


-   [Conectar-se ao Acesso via Web Remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)

-   [Compartilhar arquivos e pastas](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)

-   [Conectar-se de um dispositivo móvel](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)

##  <a name="connect-to-remote-web-access"></a><a name="BKMK_Connect"></a>Conectar-se ao Acesso via Web remoto

-   [Fazer logon no Acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)

-   [Acessar remotamente o computador](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)

-   [Conectar-se ao Acesso via Web Remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)

-   [Compartilhar arquivos e pastas](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)

-   [Conectar-se de um dispositivo móvel](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)

##  <a name="connect-to-remote-web-access"></a><a name="BKMK_Connect"></a>Conectar-se ao Acesso via Web remoto

-   [Fazer logon no Acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)

-   [Acessar remotamente o computador](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)


###  <a name="log-on-to-remote-web-access"></a><a name="BKMK_1"></a>Fazer logon no Acesso via Web remoto
 Ao fazer logon no Acesso via Web remoto de um computador local ou remoto, você pode acessar recursos em seu servidor que executa o Windows Server Essentials e computadores na sua rede.

##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Para fazer logon no Acesso via Web Remoto de um computador da rede

1.  Abra um navegador da Web, digite **https://** _<\> nomedoservidor_**/Remote** na barra de endereços e pressione Enter.

    > [!NOTE]
    >  Certifique-se de incluir os s em https.

2.  Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e clique na seta.

##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Para fazer logon no Acesso via Web Remoto de um computador remoto

1.  Abra um navegador da Web, digite **https://** _<\> YourDomainName_**/Remote** na barra de endereços e pressione Enter.

    > [!NOTE]
    >  Você pode obter as informações de nome de domínio do administrador da rede. Certifique-se de incluir os s em https.

2.  Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e clique na seta.

###  <a name="remotely-access-your-computer"></a><a name="BKMK_1.5"></a>Acessar remotamente seu computador
 Quando estiver fora do seu escritório, você poderá usar seu navegador da Web para fazer logon no site de Acesso via Web remoto para acessar remotamente seu painel do Windows Server Essentials, pastas compartilhadas e computadores em sua rede.

 Quando você se conecta ao painel, você pode gerenciar o Windows Server Essentials exatamente como você faria se estivesse no office. Você pode executar todas as tarefas administrativas comuns, como a adição de contas de usuário, a adição de pastas compartilhadas, definindo o acesso à pasta compartilhada e assim por diante. Quando você se conectar a computadores na sua rede, você pode acessar suas áreas de trabalho como se estivesse na frente no office.

 A coluna **Status** mostra se você pode se conectar a um computador na sua rede e pode incluir os seguintes valores:

-   **Disponível**

     O computador está ligado e está disponível para uma conexão remota. Mesmo se status aparecer, você ainda pode não será capaz de se conectar a esse computador se um firewall de terceiros bloquear a conexão.

-   **Offline ou em espera**

     O computador está desligado ou no modo de suspensão ou de hibernação. Se um computador estiver offline ou em espera, o status é atualizado em tempo real para que você possa saber quando o computador estará disponível.

-   **Sistema operacional sem suporte**

     O sistema operacional no computador não oferece suporte a área de trabalho remota. Pode levar até 6 horas para esse status ser atualizado no servidor, se houver uma alteração.

-   **A conexão está desabilitada**

     A conexão de computador está bloqueada por um firewall ou a área de trabalho remota está desabilitada no computador ou pela política de grupo. Pode levar até 6 horas para esse status ser atualizado no servidor, se houver uma alteração.

#### <a name="to-connect-to-a-computer-on-your-network"></a>Para se conectar a um computador na sua rede
 Sobre a guia **dispositivos**, clique no nome do computador. Você pode selecionar somente os computadores com o status **disponível** .

#### <a name="to-connect-to-the-server-dashboard"></a>Para se conectar ao painel do servidor
 Sobre a guia **dispositivos**, clique no nome do seu servidor. Você pode selecionar somente os computadores com o status **disponível** . Você deve ser capaz de fornecer uma conta de usuário administrador e senha em seu servidor para usar o Painel.

##  <a name="share-files-and-folders"></a><a name="BKMK_SharedFolders"></a>Compartilhar arquivos e pastas


-   [Fazer upload e download de arquivos no acesso via Web Remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)

-   [Criar, renomear, mover, excluir ou copiar arquivos e pastas no acesso via Web Remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)


###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_UploadRWA"></a>Carregar e baixar arquivos no Acesso via Web remoto
 Sobre o acesso remoto via Web **pastas compartilhadas** guia, você pode fazer o seguinte:

-   Carrega arquivos (envio) do seu computador para o Windows Server Essentials.

    > [!NOTE]
    >  Você pode carregar somente arquivos, e não pastas, para o Acesso Remoto via Web. Se você deseja ter a mesma hierarquia de arquivo e pasta no **pastas compartilhadas** no servidor em seu computador, você deve criar as pastas no servidor de acesso remoto via Web e carregar os arquivos para a pasta que você criou. Para obter informações sobre a criação de pastas do servidor, veja [adicionar ou mover uma pasta no servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).

-   Baixar (receber) arquivos e pastas do Windows Server Essentials para o computador.

-   Crie uma pasta dentro de uma pasta compartilhada no Windows Server Essentials.


-   Mover, excluir e renomear arquivos e pastas no Windows Server Essentials. Para obter mais informações, consulte [criar, renomear, mover, excluir ou copiar arquivos e pastas no acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).

-   Mover, excluir e renomear arquivos e pastas no Windows Server Essentials. Para obter mais informações, consulte [criar, renomear, mover, excluir ou copiar arquivos e pastas no acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).


#### <a name="upload-files"></a>Carregar arquivos

###### <a name="to-upload-files"></a>Carregar arquivos

1. No acesso remoto via Web , clique na guia **pastas compartilhadas** e, em seguida, clique no link da pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2. Na lista de pasta compartilhada de arquivos e pastas, clique na pasta onde você deseja carregar o arquivo e clique em **carregar**.

3. Se a ferramenta de carregamento padrão ainda não estiver carregada, clique em **usar o método de carregamento padrão**.

4. Clicar em **Procurar**  para encontrar um arquivo no seu computador.

5. Navegue nas pastas do seu computador para localizar o arquivo que você deseja carregar e, em seguida, clique em **abrir**.

6. Repita as etapas 2 e 3 para cada arquivo que você deseja carregar.

7. Quando você tiver adicionado todos os arquivos que você deseja carregar, clique em **carregar**.

   A ferramenta de carregamento fácil de arquivo simplifica o processo de carregamento de arquivos no servidor que executa o Windows Server Essentials. Você pode adicionar quantos arquivos desejar à ferramenta de carregamento fácil de arquivo usando o recurso de arrastar e soltar e carregá-los para as pastas compartilhadas no servidor.

> [!NOTE]
>  Upload de vários arquivos tem suporte nativo em navegadores da Web compatíveis com HTML5. Essa ferramenta só é necessária quando o navegador da Web não oferece suporte a HTML5.

###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Para carregar arquivos usando a ferramenta de carregamento fácil de arquivo

1.  No acesso remoto via Web , clique na guia **pastas compartilhadas** e, em seguida, clique no link da pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Na lista pasta compartilhada de arquivos e pastas, clique na pasta onde você deseja carregar os arquivos e, em seguida, clique em **carregar**. Se a pasta que você deseja carregar não existir, clique em **nova pasta**, digite o nome da nova pasta na caixa de diálogo e, em seguida, clique em **OK**.

3.  Talvez seja necessário executar o complemento de soluções do Windows Server. Nesse caso, clique na faixa amarela na parte superior da tela, clique em executar **complemento**e clique em **executar** na caixa de diálogo.

4.  Se a ferramenta de carregamento fácil de arquivo ainda não foi carregada, clique em **usar a ferramenta de carregamento fácil de arquivo**.

5.  Você pode arrastar e soltar arquivos no Windows Explorer para a ferramenta de carregamento fácil de arquivo, ou clique **Procurar para selecionar arquivos**.

6.  Quando você terminar de adicionar os arquivos que você deseja carregar na pasta selecionada, clique em **carregar**.

7.  Quando os arquivos forem carregados com êxito, clique em **fechar**.

#### <a name="download-files-or-folders"></a>Baixe arquivos ou pastas

###### <a name="to-download-a-single-file"></a>Para baixar um único arquivo

1. No acesso remoto via Web , clique na guia **pastas compartilhadas** e, em seguida, clique no link da pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2. Da lista de arquivos da pasta compartilhada, clique na caixa de seleção ao lado do arquivo que você deseja baixar no seu computador doméstico.

3. Clique em **baixar** para iniciar o download.

4. Na caixa de diálogo **Download de arquivo** , clique em **salvar** para salvar o arquivo em seu computador.

5. Na caixa de diálogo **Salvar como**, selecione o local para salvar o arquivo e clique em **salvar**. Nenhum arquivo não é compactado antes até o serem baixados.

   Há duas opções para o download de vários arquivos ou pastas. Escolha a opção que atenda às suas necessidades:

> [!NOTE]
>  Essas opções estão disponíveis somente quando você baixar vários arquivos ou pastas para o computador.

- **Arquivo executável auto-extraível (.exe)**

  > [!NOTE]
  >   Esta seção se aplica a um servidor que executa o Windows Server Essentials.

   Um arquivo executável auto-extraível é um arquivo permitido para baixar e que combina o programa de (executável) descompactação com os arquivos compactados. Quando você executar o programa executável, ele descompacta automaticamente os arquivos compactados (auto-extraível). Isso é uma forma comum de distribuir dados compactados sem se preocupar se o destinatário tem o utilitário de descompactação certo.

  > [!NOTE]
  >  Essa opção oferece suporte a caracteres Unicode.

- **Pasta compactada Windows (. zip)**

   Ao compactar um arquivo é criada uma versão compactada do arquivo que for menor do que o arquivo original. A versão compactada do arquivo tem uma extensão de nome de arquivo .zip. Tipos de arquivos que são reduzidos em mais compactados são tipos de arquivo de texto, .txt, .doc, .xls e arquivos de elementos gráficos que usam tipos de arquivo não compactado, como .bmp. Alguns arquivos gráficos, como arquivos. jpg e. gif, já usam compressão, o tamanho do arquivo reduz muito pouco ao ser compactado. Além disso, um documento do Word que contém muitos elementos gráficos não é reduzido quanto um documento é de texto.

  > [!NOTE]
  >  Essa opção fornece suporte limitado para nomes de arquivo internacionais no Windows Server Essentials.

  Antes de inicia o download, o arquivo exe ou zip é criado. Dependendo do número de arquivos e o tamanho total dos arquivos a serem baixados, isso pode levar vários minutos. Depois que o arquivo de download é criado, o download do arquivo ocorre em segundo plano. Isso permite que você continue trabalhando enquanto o processo de download é concluído.

###### <a name="to-download-multiple-files-or-folders"></a>Para baixar vários arquivos ou pastas

1.  No acesso remoto via Web , clique na guia **pastas compartilhadas** e, em seguida, clique no link da pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Da lista de arquivos da pasta compartilhada, clique na caixa de seleção ao lado de arquivos ou pastas que deseja baixar no seu computador doméstico.

3.  Clique em **baixar** para iniciar o download.

4.  Na caixa de diálogo **Escolher um formato de download** , clique para selecionar a opção de formato de download que você preferir e clique em **OK**. O arquivo compactado é preparado na opção de formato selecionada.

5.  Na caixa de diálogo **Download de Arquivo** , clique em **Salvar** para salvar o arquivo em seu computador.

6.  Na caixa de diálogo **Salvar como**, selecione o local para salvar o arquivo e clique em **salvar**.

#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Recuperar arquivos compactados baixados para o computador

> [!NOTE]
>   Esta seção se aplica a um servidor que executa o Windows Server Essentials.

 Se você selecionar vários arquivos ou pastas para baixar, você poderá receber um arquivo executável compactado auto-extraível (.exe) ou um arquivo compactado (. zip).

###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Para recuperar um arquivo do arquivo compactado (.exe)

1.  No computador, clique duas vezes no arquivo compactado para abri-lo.

2.  Siga as instruções para extrair os arquivos para uma pasta no computador.

###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Para recuperar um arquivo do arquivo compactado (. zip)

1.  No computador, clique duas vezes no arquivo compactado para abri-lo.

2.  Selecione os arquivos que você deseja recuperar e arraste os arquivos para uma pasta no computador onde você deseja armazená-los.

    > [!NOTE]
    >  Se você usar um programa de compactação de terceiros, siga os procedimentos para o programa extrair os arquivos do arquivo compactado.

###  <a name="create-rename-move-delete-or-copy-files-and-folders-in-remote-web-access"></a><a name="BKMK_2"></a>Criar, renomear, mover, excluir ou copiar arquivos e pastas no Acesso via Web remoto
 Você pode usar o acesso remoto via Web para criar novas pastas em uma pasta compartilhada existente para renomear os arquivos e pastas, mover e copiar arquivos e pastas e excluir arquivos e pastas no servidor.

> [!NOTE]
<<<<<<< HEAD para adicionar novas pastas compartilhadas em um servidor que executa o Windows Server Essentials, você deve usar o painel. Para se conectar ao console do servidor de acesso remoto via Web, em **computadores**, clique no nome do servidor, clique em **conectar**e, em seguida, siga as instruções para fazer logon no servidor. Para obter informações sobre como criar pastas compartilhadas, veja [adicionar ou mover uma pasta no servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).

##### <a name="to-create-a-new-folder"></a>Para criar uma nova pasta

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Na barra de tarefas, clique em **nova pasta**.

3.  Digite um nome para a pasta e, em seguida, clique em **OK**.

##### <a name="to-rename-a-file-or-folder"></a>Renomear um arquivo ou pasta

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Com o botão direito no arquivo ou pasta que você deseja renomear e, em seguida, clique em **Renomear**.

3.  Digite um novo nome na caixa de texto e, em seguida, clique em **OK**.

##### <a name="to-move-files-or-folders"></a>Para mover arquivos ou pastas

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Marque a caixa de seleção de arquivos ou pastas que você deseja mover, clique em um dos arquivos selecionados ou pastas e, em seguida, clique em **Recortar**.

3.  Clique na pasta que você deseja mover os arquivos ou pastas e, em seguida, clique em **colar**.

##### <a name="to-delete-a-file-or-folder"></a>Para excluir um arquivo ou pasta

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Marque a caixa de seleção de arquivos ou pastas que você deseja excluir, clique em um dos arquivos selecionados ou pastas e, em seguida, clique em **excluir**.

3.  Para confirmar que você deseja excluir os arquivos e pastas selecionados, clique em **Sim**.

##### <a name="to-copy-files-or-folders"></a>Para copiar os arquivos ou pastas

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Marque a caixa de seleção de arquivos ou pastas que deseja copiar, clique em um dos arquivos selecionados ou pastas e, em seguida, clique em **cópia**.

3.  Clique na pasta que você deseja copiar os arquivos ou pastas e, em seguida, clique em **colar**.

##  <a name="connect-from-a-mobile-device"></a><a name="BKMK_ConnectMobile"></a>Conectar-se de um dispositivo móvel


-   [Usar o Acesso Remoto via Web de um dispositivo móvel](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)

-   [Compatível com navegadores Web para dispositivos móveis](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)

-   [Usar o Acesso Remoto via Web de um dispositivo móvel](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)

-   [Compatível com navegadores Web para dispositivos móveis](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)


###  <a name="use-remote-web-access-from-a-mobile-device"></a><a name="BKMK_8"></a>Usar o Acesso via Web remoto de um dispositivo móvel
 Você pode fazer logon no Acesso via Web Remoto de seu smartphone para exibir os arquivos e pastas nas pastas compartilhadas no servidor.

> [!NOTE]
>  Você também pode baixar e usar o aplicativo My Server para Windows Server Essentials do [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) para acessar seus arquivos de mídia e pastas compartilhados que são armazenados no servidor.

##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Para fazer logon no Acesso via Web Remoto de um dispositivo móvel

1.  Abra um navegador da Web e digite **https://** _<\> YourDomainName_**/Remote** na barra de endereços.  Certifique-se de incluir os s em https.

2.  Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e clique na seta. Você está conectado à versão móvel do Acesso Remoto via Web .

##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Alternar para a versão da área de trabalho do acesso via Web Remoto

1.  Abra um navegador da Web e digite **https://** _<\> YourDomainName_**/Remote** na barra de endereços.  Certifique-se de incluir os s em https.

2.  Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto, clique em **Exibir versão da área de trabalho**e clique na seta. Você está conectado à versão da área de trabalho do acesso remoto via Web .

##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Para voltar para a versão móvel do acesso via Web Remoto

1. Dê logoff.

2. Abra um navegador da Web e digite **https://** _<\> YourDomainName_**/Remote/m** na barra de endereços. Certifique-se de incluir os s em https.

3. A versão móvel do Acesso via Web remoto é exibida. Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e clique na seta. Você está conectado à versão móvel do Acesso via Web remoto.

   Você pode procurar arquivos e pastas em pastas compartilhadas no servidor.

###  <a name="supported-web-browsers-for-mobile-devices"></a><a name="BKMK_9"></a>Navegadores da Web com suporte para dispositivos móveis
 Os navegadores web compatíveis com dispositivos móveis incluem:

-   Internet Explorer Mobile 6.0 ou posterior

-   Safari

-   BlackBerry

-   Symbian 6.0 ou posterior

-   Android

-   Google Chrome

-   Firefox

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar o Acesso via Web Remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)


-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](../use/Work-Remotely-in-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

=======
>  Para adicionar novas pastas compartilhadas em um servidor que esteja executando o Windows Server Essentials, você deve usar o Painel geral. Para se conectar ao console do servidor de acesso remoto via Web, em **computadores**, clique no nome do servidor, clique em **conectar**e, em seguida, siga as instruções para fazer logon no servidor. Para obter informações sobre como criar pastas compartilhadas, veja [adicionar ou mover uma pasta no servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).

##### <a name="to-create-a-new-folder"></a>Para criar uma nova pasta

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Na barra de tarefas, clique em **nova pasta**.

3.  Digite um nome para a pasta e, em seguida, clique em **OK**.

##### <a name="to-rename-a-file-or-folder"></a>Renomear um arquivo ou pasta

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Com o botão direito no arquivo ou pasta que você deseja renomear e, em seguida, clique em **Renomear**.

3.  Digite um novo nome na caixa de texto e, em seguida, clique em **OK**.

##### <a name="to-move-files-or-folders"></a>Para mover arquivos ou pastas

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Marque a caixa de seleção de arquivos ou pastas que você deseja mover, clique em um dos arquivos selecionados ou pastas e, em seguida, clique em **Recortar**.

3.  Clique na pasta que você deseja mover os arquivos ou pastas e, em seguida, clique em **colar**.

##### <a name="to-delete-a-file-or-folder"></a>Para excluir um arquivo ou pasta

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Marque a caixa de seleção de arquivos ou pastas que você deseja excluir, clique em um dos arquivos selecionados ou pastas e, em seguida, clique em **excluir**.

3.  Para confirmar que você deseja excluir os arquivos e pastas selecionados, clique em **Sim**.

##### <a name="to-copy-files-or-folders"></a>Para copiar os arquivos ou pastas

1.  No Acesso Remoto via Web, clique na guia **pastas compartilhadas** e, em seguida, clique no link para a pasta compartilhada. Uma lista de arquivos e pastas da pasta compartilhada é exibida.

2.  Marque a caixa de seleção de arquivos ou pastas que deseja copiar, clique em um dos arquivos selecionados ou pastas e, em seguida, clique em **cópia**.

3.  Clique na pasta que você deseja copiar os arquivos ou pastas e, em seguida, clique em **colar**.

##  <a name="connect-from-a-mobile-device"></a><a name="BKMK_ConnectMobile"></a>Conectar-se de um dispositivo móvel


-   [Usar o Acesso Remoto via Web de um dispositivo móvel](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)

-   [Compatível com navegadores Web para dispositivos móveis](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)


###  <a name="use-remote-web-access-from-a-mobile-device"></a><a name="BKMK_8"></a>Usar o Acesso via Web remoto de um dispositivo móvel
 Você pode fazer logon no Acesso via Web Remoto de seu smartphone para exibir os arquivos e pastas nas pastas compartilhadas no servidor.

> [!NOTE]
>  Você também pode baixar e usar o aplicativo My Server para Windows Server Essentials do [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) para acessar seus arquivos de mídia e pastas compartilhados que são armazenados no servidor.

##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Para fazer logon no Acesso via Web Remoto de um dispositivo móvel

1.  Abra um navegador da Web e digite **https://** _<\> YourDomainName_**/Remote** na barra de endereços.  Certifique-se de incluir os s em https.

2.  Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e clique na seta. Você está conectado à versão móvel do Acesso Remoto via Web .

##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Alternar para a versão da área de trabalho do acesso via Web Remoto

1.  Abra um navegador da Web e digite **https://** _<\> YourDomainName_**/Remote** na barra de endereços.  Certifique-se de incluir os s em https.

2.  Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto, clique em **Exibir versão da área de trabalho**e clique na seta. Você está conectado à versão da área de trabalho do acesso remoto via Web .

##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Para voltar para a versão móvel do acesso via Web Remoto

1. Dê logoff.

2. Abra um navegador da Web e digite **https://** _<\> YourDomainName_**/Remote/m** na barra de endereços. Certifique-se de incluir os s em https.

3. A versão móvel do Acesso via Web remoto é exibida. Na página logon Acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e clique na seta. Você está conectado à versão móvel do Acesso via Web remoto.

   Você pode procurar arquivos e pastas em pastas compartilhadas no servidor.

###  <a name="supported-web-browsers-for-mobile-devices"></a><a name="BKMK_9"></a>Navegadores da Web com suporte para dispositivos móveis
 Os navegadores web compatíveis com dispositivos móveis incluem:

-   Internet Explorer Mobile 6.0 ou posterior

-   Safari

-   BlackBerry

-   Symbian 6.0 ou posterior

-   Android

-   Google Chrome

-   Firefox

## <a name="see-also"></a>Confira também

-   [Gerenciar o Acesso via Web Remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](Use-Windows-Server-Essentials.md)

>>>>>>> 97724df67237ac603cf9eb996732230bdb7c0b88
