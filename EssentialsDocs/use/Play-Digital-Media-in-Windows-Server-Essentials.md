---
title: Gerenciar mídia digital no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ce7359929cb1658cc84418231fb7d65efce8692
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87838215"
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Gerenciar mídia digital no Windows Server Essentials

>Aplica-se a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mídia digital se refere ao áudio, vídeo e conteúdo de fotos que foram compactados digitalmente.  O Windows Server Essentials possibilita que computadores em rede e alguns dispositivos de mídia digital em rede reproduzam arquivos de mídia digital armazenados no servidor.

 Os tópicos a seguir fornecem informações sobre como acessar e reproduzir arquivos de mídia digital armazenados no Windows Server Essentials:


-   [Visão geral de mídia digital](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)

-   [Executar e compartilhar mídia digital](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)

-   [Executar arquivos de mídia digital compartilhada de um local remoto](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)

-   [Adicionar arquivos de mídia digital para o servidor](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)

-   [Opções de formatos de download](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)

-   [Ferramenta de upload de arquivo fácil](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)

-   [Exibir e procurar mídia digital compartilhada](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)


##  <a name="digital-media-overview"></a><a name="BKMK_1"></a>Visão geral de mídia digital
 Mídia digital se refere ao áudio, vídeo e conteúdo de fotos que tenha sido codificado (compactado digitalmente). Codificação conteúdo envolve a conversão de entrada de áudio e vídeo para um arquivo de mídia digital, como um arquivo do Windows Media. Depois da mídia digital ser codificada, pode ser facilmente manipulada, distribuída e executada por computadores e facilmente são transmitidos por redes de computador.

 Exemplos dos tipos de mídia digital incluem: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Para obter informações sobre os tipos de mídia digital com suporte no Windows Media Player, consulte [Tipos de arquivo com suporte pelo Windows Media Player](https://support.microsoft.com/kb/316992).

### <a name="why-would-i-want-to-stream-my-digital-media"></a>Por que eu quero fazer transmissão de mídia digital?
 Muitas pessoas armazenam música, vídeo e imagens em pastas compartilhadas no Windows Server Essentials. Pode haver momentos que você desejará fazer o seguinte:

-   **Assistir vídeos**. O servidor pode ser usado para armazenar e transmitir grandes coleções de vídeos e TV gravada mostrada aos computadores ou outra reprodução de dispositivos na sua rede. Você pode transmitir vídeos para um Xbox 360 ou para um computador usando o Windows Media Player.

-   **Reproduzir música**. Quando você ativa o compartilhamento de mídia para a pasta compartilhada\**música**, você poderá acessar seus arquivos de música de dispositivos que oferecem suporte a conexão de mídia do Windows. Não é necessário habilitar ou configurar as contas de usuário para transmitir da pasta compartilhad**música** depois que o compartilhamento estiver ativado.

-   **Exibir apresentações de fotos**. Você pode armazenar fotos digitais na pasta compartilhada **fotos** no servidor e acessá-las a partir de qualquer computador ou um Xbox 360 conectado a uma TV em casa ou no escritório. Você pode assistir a apresentações de fotos, transformando sua TV em um grande quadro de imagens.

### <a name="sharing-copy-protected-media"></a>Compartilhamento de mídia protegido contra cópia
  O Windows Server Essentials não oferece suporte ao compartilhamento de mídia protegida por cópia. Isso inclui músicas adquiridas por meio de um repositório de música online.

 Mídia protegida contra cópia pode ser executada novamente somente no computador ou dispositivo que você usou para adquiri-lo. Proteção contra cópia impede a execução da mídia em mais de um computador ou dispositivo, mesmo que você copie a mídia para o servidor e executá-lo a partir dele. No entanto, você pode armazenar a mídia protegida por cópia no Windows Server Essentials e continuar a reproduzir a mídia no computador ou dispositivo que você usou para comprá-la.

##  <a name="play-and-share-digital-media"></a><a name="BKMK_2"></a>Reproduzir e compartilhar mídia digital
 Depois de configurar a rede e se conectar com êxito seus computadores e dispositivos de mídia com a rede do servidor, você pode procurar os arquivos de mídia digital que armazena e compartilha no servidor.

> [!NOTE]
>  Para obter uma lista atual de dispositivos de recebimento e player de mídia digital que são compatíveis com o Windows Server Essentials, consulte o [centro de compatibilidade do Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).

 Você pode usar qualquer uma das seguintes maneiras para pesquisar e executar arquivos de mídia digital armazenados no servidor:

-   [Pesquisar e reproduzir arquivos de mídia no Windows Server Essentials de um computador ou player de mídia digital na rede](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)

-   [Enviar arquivos de mídia no Windows Server Essentials para o Windows Media Player, Xbox 360 ou para um player de mídia digital em rede](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)


###  <a name="search-for-and-play-media-files-on-windows-server-essentials-from-a-computer-or-digital-media-player-on-the-network"></a><a name="BKMK_2.1"></a>Pesquisar e reproduzir arquivos de mídia no Windows Server Essentials de um computador ou player de mídia digital na rede
 Quando o dispositivo estiver ingressado na rede do Windows Server Essentials, você poderá pesquisar e reproduzir arquivos de mídia digital de qualquer uma das seguintes maneiras:


-   [Pesquisar e executar arquivos de mídia de um computador que esteja executando o Windows Media Center](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)

-   [Pesquisar e executar arquivos de mídia de um computador que esteja executando o Windows usando o Windows Media Player](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)

-   [Pesquisar e executar arquivos de mídia usando o Xbox 360](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)

-   [Pesquisar e reproduzir arquivos de mídia usando outros media players digitais ou receptores que são compatíveis com o Windows Server Essentials](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)

-   [Pesquisar e executar arquivos de mídia usando o recurso de pastas compartilhadas da Barra Inicial](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)

-   [Procurar e reproduzir mídia compartilhada usando o acesso via Web Remoto](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)


####  <a name="search-for-and-play-media-files-from-a-computer-that-is-running-windows-media-center"></a><a name="BKMK_WMC"></a>Pesquisar e reproduzir arquivos de mídia de um computador que esteja executando o Windows Media Center

1.  Clique em **Iniciar**, clique em **Todos os Programas** e clique em **Windows Update**.

2.  Na página **Windows Media Center**, role até o tipo de mídia que você está procurando e clique na biblioteca de mídia.

3.  Procurar manualmente o arquivo que você está interessado, ou clique em **pesquisa** e digite o nome do arquivo que deseja localizar.

4.  Clique na imagem de arquivos de mídia para exibir ou executar o arquivo.

####  <a name="search-for-and-play-media-files-from-a-computer-that-is-running-windows-by-using-windows-media-player"></a><a name="BKMK_MWP"></a>Pesquisar e reproduzir arquivos de mídia de um computador que esteja executando o Windows usando o Windows Media Player

-   Do computador ou dispositivo de mídia, abra **Windows Media Player** e procure sua biblioteca de mídia.

    > [!NOTE]
    >  As etapas de pesquisa variam dependendo da versão do Windows Media Player que você está usando. Para obter informações detalhadas, consulte a Ajuda para a sua versão.

####  <a name="search-for-and-play-media-files-by-using-xbox-360"></a><a name="BKMK_Xbox"></a>Pesquisar e reproduzir arquivos de mídia usando o Xbox 360

1.  Conecte seu console Xbox 360 à sua rede doméstica usando uma conexão com ou sem fio.

2.  No computador que está executando o Windows Server Essentials, ative o compartilhamento de mídia. Para obter mais informações, consulte o tópico [Ativar ou desativar o streaming de mídia](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).

3.  Para executar arquivos de mídia digital usando o console Xbox 360:

    1.  Vá para **Meu Xbox**e selecione **biblioteca de vídeos**, **biblioteca de música**, ou **biblioteca de imagens**, dependendo do tipo de mídia que você deseja exibir ou executar.

    2.  Selecione o nome do seu servidor.

        > [!NOTE]
        >  Se o nome do seu servidor não estiver listado, selecione **computador**e clique em **Testar conexão**.

    3.  Procurar a lista de arquivos e selecione o item que você deseja executar.

####  <a name="search-for-and-play-media-files-by-using-other-digital-media-players-or-receivers-that-are-compatible-with-windows-server-essentials"></a><a name="BKMK_Other"></a>Pesquisar e reproduzir arquivos de mídia usando outros media players digitais ou receptores que são compatíveis com o Windows Server Essentials

1.  Vá para o [Centro de Compatibilidade do Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) e certifique-se de que seu reprodutor ou receptor de mídia digital aparece na lista de dispositivos compatíveis.

2.  Como as etapas de pesquisa variam dependendo do reprodutor de mídia digital que você está usando, consulte a Ajuda para o seu dispositivo para obter instruções detalhadas.

####  <a name="search-for-and-play-media-files-by-using-the-shared-folders-feature-of-the-launchpad"></a><a name="BKMK_SharedFolders"></a>Pesquisar e reproduzir arquivos de mídia usando o recurso pastas compartilhadas do Launchpad

1.  Entre no Launchpad do Windows Server Essentials.

2.  A partir da barra inicial, clique em **pastas compartilhadas**. Uma janela do Windows Explorer é aberta e exibe as pastas compartilhadas no servidor.

3.  Na caixa **Pesquisa**, digite o nome de um arquivo de mídia. Os resultados da busca da pesquisa serão exibidos.

    > [!NOTE]
    >  Como opção, você pode também clique duas vezes em uma pasta compartilhada para procurar o conteúdo da pasta.

####  <a name="search-for-and-play-shared-media-by-using-remote-web-access"></a><a name="BKMK_RWA2"></a>Pesquisar e reproduzir mídia compartilhada usando o Acesso via Web remoto

1.  Faça logon no Acesso Remoto via Web.

2.  Clique em **Pastas Compartilhadas**. A seção **pastas compartilhadas** da página da Web exibe uma lista de pastas compartilhadas no servidor.

3.  Clique duas vezes em uma pasta para exibir o conteúdo da pasta.

###  <a name="send-media-files-on-windows-server-essentials-to-windows-media-player-xbox-360-or-to-a-networked-digital-media-player-in-the-network"></a><a name="BKMK_SendToDevice"></a>Enviar arquivos de mídia no Windows Server Essentials para o Windows Media Player, Xbox 360 ou para um player de mídia digital em rede
 Use **Windows Media Player** para procurar o arquivo de mídia que você deseja. Com o botão direito no arquivo de mídia e, em seguida, clique em **reproduzir em** para enviar o arquivo de mídia para um dispositivo de mídia em rede.

##  <a name="play-shared-digital-media-files-from-a-remote-location"></a><a name="BKMK_3"></a>Reproduzir arquivos de mídia digital compartilhados de um local remoto
 Você pode reproduzir seus arquivos de mídia quando estiver fora da rede do Windows Server Essentials usando o Acesso via Web remoto. Você pode usar um telefone, um computador remoto ou um reprodutor de mídia digital para pesquisar e executar os arquivos de mídia compartilhada armazenada no servidor.

#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>Para executar arquivos de mídia compartilhada quando está fora da rede

1. Abra um navegador da Internet.

2. Vá para o site de Acesso Remoto via Web . Digite **https://<YourDomainName \> /Remote** na barra de endereços do navegador da Internet e pressione Enter.

   > [!NOTE]
   >  *<YourDomainName \> * é um espaço reservado. Ele será um nome exclusivo para o seu servidor, portanto, o endereço que você digitar terá a seguinte aparência **https://contoso.com/remote** . Se você não souber o nome do seu domínio, peça ao administrador que escolheu o nome de domínio quando a função Acesso remoto estiver configurada no servidor. Para obter mais informações, veja [ativar o acesso remoto via Web](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).

3. Sobre a acesso remoto via Web da página de entrada, digite o nome de conta de usuário e senha e, em seguida, clique na seta.

4. Use qualquer método que você desejar para pesquisar o arquivo de mídia que você deseja executar.

   > [!NOTE]
   >
   >  Para obter informações sobre os vários métodos de pesquisa, consulte [Pesquisar e reproduzir arquivos de mídia no Windows Server Essentials de um computador ou player de mídia digital na rede](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).
   >
   >  Para obter informações sobre os vários métodos de pesquisa, consulte [Pesquisar e reproduzir arquivos de mídia no Windows Server Essentials de um computador ou player de mídia digital na rede](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1).


5. Quando o nome do arquivo de mídia for exibida, clique no nome do arquivo para executar a mídia.

##  <a name="add-digital-media-files-to-the-server"></a><a name="BKMK_4"></a>Adicionar arquivos de mídia digital ao servidor

 O administrador do servidor pode adicionar mídia digital a pastas compartilhadas na biblioteca de mídia acessando o servidor diretamente ou usando o site de Acesso via Web remoto para entrar no painel. Outros usuários podem adicionar arquivos de mídia ao servidor usando a conexão **pastas compartilhadas** no Launchpad, usando o site de acesso via Web remoto ou usando o aplicativo meu servidor para Windows Phone. Para obter informações sobre a execução de mídia, veja[reproduzir e compartilhar mídia digital](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).

 O administrador do servidor pode adicionar mídia digital a pastas compartilhadas na biblioteca de mídia acessando o servidor diretamente ou usando o site de Acesso via Web remoto para entrar no painel. Outros usuários podem adicionar arquivos de mídia ao servidor usando a conexão **pastas compartilhadas** no Launchpad, usando o site de acesso via Web remoto ou usando o aplicativo meu servidor para Windows Phone. Para obter informações sobre a execução de mídia, veja[reproduzir e compartilhar mídia digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2).


> [!NOTE]
>  Você também pode carregar arquivos de mídia para o servidor usando o aplicativo My Server para Windows Phone. Você pode baixar o aplicativo My Server da [Loja do Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Para obter mais informações sobre o aplicativo de servidor para Windows Phone, consulte a postagem de blog [meu aplicativo de telefone do servidor para Windows Server Essentials](/archive/blogs/sbs/my-server-phone-app-for-windows-server-2012-essentials).

#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Para adicionar arquivos de mídia digital para pastas compartilhadas no servidor

1.  Use um dos métodos a seguir para entrar no servidor:


    1.  Para obter informações sobre como fazer logon em Acesso via Web remotos, consulte [usar o acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).

    1.  Para obter informações sobre como fazer logon em Acesso via Web remotos, consulte [usar o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).


    2.  Para obter informações sobre como entrar com a Launchpad, consulte [visão geral da Launchpad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).

2.  Pesquisar e clicar na pasta para a mídia que você está adicionando.

3.  Copiar e colar ou opção de arrastar e soltar, a mídia de arquivos que você deseja adicionar ao apropriado compartilhados pasta no servidor.

##  <a name="download-format-options"></a><a name="BKMK_5"></a>Opções de formato de download
 Há duas opções para download de arquivos. Essas opções estão disponíveis somente quando baixar uma pasta ou vários arquivos para um computador em Windows.

 Escolha a opção a seguir que atenda às suas necessidades de downloads:

- **Arquivo ZIP compactado (.zip)**

   Ao compactar um arquivo é criada uma versão compactada do arquivo que for menor do que o arquivo original. A versão compactada do arquivo tem uma extensão de nome de arquivo .zip. Tipos de arquivos que são reduzidos em mais compactados são tipos de arquivo de texto (por exemplo, .txt, .doc e .xls) e arquivos de elementos gráficos que usa tipos de arquivo não compactados (como .bmp). Alguns arquivos gráficos, como arquivos. jpg e. gif, já usam compressão, o tamanho do arquivo reduz muito pouco ao ser compactado. Além disso, um documento do Word que contém muitos elementos gráficos não é reduzido quanto um documento é de texto.

  > [!NOTE]
  >  Essa opção fornece suporte limitado para nomes de arquivo internacionais.

- **Arquivo executável auto-extraível (.exe)**

   Um arquivo executável auto-extraível é um arquivo permitido para baixar e que combina o programa de (executável) descompactação com os arquivos compactados. Quando você executar o programa executável, ele descompacta automaticamente os arquivos compactados. Isso é uma forma comum de distribuir dados compactados sem se preocupar se o destinatário tem o utilitário de descompactação certo.

  > [!NOTE]
  >  Essa opção oferece suporte a caracteres Unicode.

  Antes de iniciar o download, o arquivo .exe ou .zip é criado. Dependendo do número de arquivos e o tamanho total dos arquivos a serem baixados, isso pode levar vários minutos. Depois que o arquivo de download é criado, o download do arquivo ocorre em segundo plano. Isso permite que você continue trabalhando enquanto o processo de download é concluído.

##  <a name="easy-file-upload-tool"></a><a name="BKMK_6"></a>Ferramenta de upload de arquivo fácil
 A ferramenta de upload de arquivo fácil simplifica o processo de carregamento de arquivos no servidor do Windows Server Essentials. Você pode adicionar quantos arquivos desejar à ferramenta de upload de arquivo fácil e, em seguida, carregá-los nas pastas compartilhadas no servidor do Windows Server Essentials em um único lote. Para obter mais informações, consulte a postagem de blog [Understanding Remote Web Access File Sharing (Entendendo o compartilhamento de arquivos do Acesso Remoto via Web)](/archive/blogs/sbs/understanding-remote-web-access-file-sharing).

##  <a name="view-and-browse-shared-digital-media"></a><a name="BKMK_7"></a>Exibir e procurar mídia digital compartilhada
 Você pode exibir ou procurar recursos usando o Painel, a barra inicial, o site de Acesso Remoto via Web ou o aplicativo My Server para Windows Phone.

#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>Para exibir e procurar mídia compartilhada do Painel

1.  Abra o Painel do servidor.

2.  Na barra de navegação principal, clique em **armazenamento**.

3.  Clique na guia **pastas do servidor** . Uma lista de pastas de servidor é exibida.

4.  Clique duas vezes em uma pasta para exibir o conteúdo da pasta.

#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>Para exibir e procurar mídia compartilhada usando a barra inicial de um computador de rede

1.  Ir para a barra inicial.

2.  Clique em **Pastas Compartilhadas**. O Windows Explorer é aberta e exibe uma lista de pastas compartilhadas no servidor.

3.  Clique duas vezes em uma pasta para exibir o conteúdo da pasta.

#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>Para exibir e procurar mídia compartilhada usando o Acesso Remoto via Web

1.  Faça logon no Acesso Remoto via Web.

2.  Clique em **Pastas Compartilhadas**. A seção **pastas compartilhadas** da página da Web exibe uma lista de pastas compartilhadas no servidor.

3.  Clique duas vezes em uma pasta para exibir o conteúdo da pasta.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar mídia digital](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)

-   [Conecte-se](Get-Connected-in-Windows-Server-Essentials.md)

-   [Usar pastas compartilhadas](Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)