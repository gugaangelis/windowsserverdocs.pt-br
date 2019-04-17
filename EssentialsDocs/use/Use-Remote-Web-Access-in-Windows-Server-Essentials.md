---
title: Use o acesso via Web remoto no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac384b545fe61b4a832debdc8aedcc81a75c669a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Use o acesso via Web remoto no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
  Acesso via Web remoto ajuda você a ficar conectado à sua rede do Windows Server Essentials quando estiver ausente. Quando você fizer logon no acesso via Web remoto, você pode se conectar aos computadores em sua rede do Windows Server Essentials, abrir o painel para gerenciar sua rede do Windows Server Essentials e acessar todos os arquivos de mídia e pastas compartilhados no servidor.  
  
 Este tópico inclui as seções a seguir:  
  

-   [Conectar ao acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Compartilhar arquivos e pastas](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Conectar-se de um dispositivo móvel](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Conectar ao acesso via Web remoto  
  
-   [Fazer logon em acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Acessar remotamente seu computador](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [Conectar ao acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Compartilhar arquivos e pastas](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [Conectar-se de um dispositivo móvel](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>Conectar ao acesso via Web remoto  
  
-   [Fazer logon em acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Acessar remotamente seu computador](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>Fazer logon em acesso via Web remoto  
 Quando você fizer logon no acesso via Web remoto em um computador local ou remoto, você pode acessar recursos do servidor que executa o Windows Server Essentials e computadores em sua rede.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>Para fazer logon no acesso via Web remoto em um computador de rede  
  
1.  Abra um navegador da Web, tipo **https://***< YourServerName\ >***/remoto** na barra de endereços e pressione Enter.  
  
    > [!NOTE]
    >  Certifique-se de que você inclua o s no https.  
  
2.  Na página de logon de acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e, em seguida, clique na seta.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>Para fazer logon no acesso via Web remoto de um computador remoto  
  
1.  Abra um navegador da Web, tipo **https://***< YourDomainName\ >***/remoto** na barra de endereços e pressione Enter.  
  
    > [!NOTE]
    >  Você pode obter as informações de nome de domínio ao administrador de rede. Certifique-se de que você inclua o s no https.  
  
2.  Na página de logon de acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e, em seguida, clique na seta.  
  
###  <a name="BKMK_1.5"></a>Acessar remotamente seu computador  
 Quando você estiver fora do escritório, você pode usar seu navegador da Web para fazer logon no site de acesso via Web remoto para acessar remotamente seu painel do Windows Server Essentials, pastas compartilhadas e computadores em sua rede.  
  
 Quando você se conecta ao painel, você pode gerenciar o Windows Server Essentials como faria se estivesse no escritório. Você pode executar todas as tarefas administrativas comuns, como a adição de contas de usuário, adicionando pastas compartilhadas, configuração acesso à pasta compartilhada e assim por diante. Quando você se conectar a computadores em sua rede, você pode acessar suas áreas de trabalho como se você estivesse sentado em frente-los no escritório.  
  
 O **Status** coluna mostra se você pode se conectar a um computador em sua rede e pode incluir os seguintes valores:  
  
-   **Disponível**  
  
     O computador está ativado e está disponível para uma conexão remota. Mesmo se você vir esse status, você ainda pode não ser capaz de se conectar a este computador, se um firewall de terceiros bloqueia a conexão.  
  
-   **Offline ou dormindo**  
  
     O computador é desligado ou está em modo de suspensão ou hibernação. Se um computador está offline ou no modo de suspensão, o status é atualizado em tempo real para que você saiba quando o computador estiver disponível.  
  
-   **Sistema operacional sem suporte**  
  
     O sistema operacional no computador não oferece suporte a área de trabalho remota. Ele pode levar até 6 horas para esse status ser atualizado no servidor, se houver uma alteração.  
  
-   **Conexão está desabilitado**  
  
     A conexão do computador está bloqueado por um firewall ou a área de trabalho remota está desabilitada no computador ou pela política de grupo. Ele pode levar até 6 horas para esse status ser atualizado no servidor, se houver uma alteração.  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>Para se conectar a um computador em sua rede  
 Sobre o **dispositivos** guia, clique no nome do computador. Você pode selecionar apenas os computadores com um **disponível** status.  
  
#### <a name="to-connect-to-the-server-dashboard"></a>Para se conectar ao servidor painel  
 Sobre o **dispositivos** guia, clique no nome do seu servidor. Você pode selecionar apenas os computadores com um **disponível** status. Você deve ser capaz de fornecer uma conta de usuário administrador e uma senha no seu servidor para usar o painel.  
  
##  <a name="BKMK_SharedFolders"></a>Compartilhar arquivos e pastas  
  

-   [Carregar e baixar arquivos no acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Criar, renomear, mover, excluir ou copiar arquivos e pastas no acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Carregar e baixar arquivos no acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [Criar, renomear, mover, excluir ou copiar arquivos e pastas no acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>Carregar e baixar arquivos no acesso via Web remoto  
 Sobre o acesso via Web remoto **pastas compartilhadas** guia, você pode fazer o seguinte:  
  
-   Carregue arquivos (envio) do computador para o Windows Server Essentials.  
  
    > [!NOTE]
    >  Você pode carregar somente arquivos e pastas não para acesso via Web remoto. Se você quer ter a mesma hierarquia de arquivos e pastas **pastas compartilhadas** no servidor em seu computador, você deve criar as pastas no servidor de acesso via Web remoto e, em seguida, carregue os arquivos para a pasta que você criou. Para obter informações sobre a criação de pastas de servidor, consulte [adicionar ou mover uma pasta de servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
-   Baixar (receber) arquivos e pastas do Windows Server Essentials para seu computador.  
  
-   Crie uma pasta dentro de uma pasta compartilhada no Windows Server Essentials.  
  

-   Mover, excluir e renomear arquivos e pastas no Windows Server Essentials. Para obter mais informações, consulte [criar, renomear, mover, excluir, ou copiar arquivos e pastas no acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

-   Mover, excluir e renomear arquivos e pastas no Windows Server Essentials. Para obter mais informações, consulte [criar, renomear, mover, excluir, ou copiar arquivos e pastas no acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2).  

  
#### <a name="upload-files"></a>Fazer upload de arquivos  
  
###### <a name="to-upload-files"></a>Fazer upload de arquivos  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Na lista pasta compartilhada de arquivos e pastas, clique na pasta onde você deseja carregar o arquivo e clique em **carregar**.  
  
3.  Se a ferramenta de carregamento padrão já não é carregada, clique em **Use o método de carregamento padrão**.  
  
4.  Clique em **procurar** para encontrar um arquivo no computador.  
  
5.  Navegue por meio de pastas em seu computador para encontrar o arquivo que você deseja carregar e, em seguida, clique em **abrir**.  
  
6.  Repita as etapas 2 e 3 para cada arquivo que você deseja carregar.  
  
7.  Quando você tiver adicionado todos os arquivos que você deseja carregar, clique em **carregar**.  
  
 A ferramenta de carregamento de arquivo fácil simplifica o processo de carregamento de arquivos no servidor que executa o Windows Server Essentials. Você pode adicionar quantos arquivos que você deseja a ferramenta transferência fácil de arquivos usando o recurso de arrastar e soltar e, em seguida, carregá-los para as pastas compartilhadas no servidor.  
  
> [!NOTE]
>  Carregamento de vários arquivos seja nativamente compatível em navegadores da web que são compatíveis com HTML5. Essa ferramenta só é necessária quando o navegador da web não dá suporte a HTML5.  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>Para carregar arquivos usando a ferramenta transferência fácil de arquivos  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Na lista pasta compartilhada de arquivos e pastas, clique na pasta onde você deseja carregar os arquivos e, em seguida, clique em **carregar**. Se a pasta que você deseja carregar para não existir, clique em **nova pasta**, digite o nome da nova pasta na caixa de diálogo e, em seguida, clique em **Okey**.  
  
3.  Talvez seja necessário executar o complemento do Windows Server Solutions. Em caso afirmativo, clique na faixa amarela na parte superior da tela, clique em executar **complemento**e clique em **executar** na caixa de diálogo.  
  
4.  Se a ferramenta transferência fácil de arquivos já não é carregada, clique em **usar a ferramenta de carregamento de arquivo fácil**.  
  
5.  Você pode arrastar e soltar arquivos no Windows Explorer para a ferramenta de carregamento de arquivo simples ou clique em **Procurar para selecionar arquivos**.  
  
6.  Quando você termina de adicionar os arquivos que você deseja carregar na pasta selecionada, clique em **carregar**.  
  
7.  Quando os arquivos forem carregados com êxito, clique em **fechar**.  
  
#### <a name="download-files-or-folders"></a>Baixar arquivos ou pastas  
  
###### <a name="to-download-a-single-file"></a>Para baixar um arquivo único  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Da lista de arquivos da pasta compartilhada, clique na caixa de seleção ao lado do arquivo que você deseja baixar para seu computador doméstico.  
  
3.  Clique em **baixar** para iniciar o download.  
  
4.  Sobre o **Download do arquivo** caixa de diálogo, clique em **salvar** para salvar o arquivo em seu computador.  
  
5.  No **Salvar como** caixa de diálogo, selecione o local para salvar o arquivo e, em seguida, clique em **salvar**. Um único arquivo não é compactado antes de baixá-los.  
  
 Há duas opções de download de vários arquivos ou pastas. Escolha a opção que atenda às suas necessidades:  
  
> [!NOTE]
>  Essas opções estão disponíveis apenas quando você estiver baixando vários arquivos ou pastas para seu computador.  
  
-   **Arquivos executáveis autoextraíveis (.exe)**  
  
    > [!NOTE]
    >   Esta seção se aplica a um servidor que executa o Windows Server Essentials.  
  
     Um arquivo executável autoextraíveis é um arquivo que você pode baixar que combina o programa de descompactação (executável) com os arquivos compactados. Quando você executa o programa executável, ele descompacta automaticamente os arquivos compactados (extração automática). Isso é uma maneira comum de distribuir dados compactados sem se preocupar se o destinatário tem o utilitário de descompactação certo.  
  
    > [!NOTE]
    >  Essa opção dá suporte a caracteres Unicode.  
  
-   **Pasta compactada do Windows (. zip)**  
  
     Ao compactar um arquivo cria uma versão compactada do arquivo que é menor do que o arquivo original. A versão compactada do arquivo tem uma extensão de nome de arquivo. zip. Tipos de arquivos que são reduziu a maior compactação são tipos de arquivo orientados a texto, como. txt,. doc,. xls, e arquivos de elementos gráficos que tipos de arquivo não compactado de uso como BMP. Alguns arquivos gráficos, como arquivos. jpg e. gif, já usam compactação e o tamanho do arquivo é reduzido pouquíssima por compactados. Além disso, um documento do Word que contém uma série de elementos gráficos não é reduzido quanto um documento que é basicamente texto.  
  
    > [!NOTE]
    >  Essa opção fornece suporte limitado para nomes de arquivo internacionais no Windows Server Essentials.  
  
 Antes de começa o download real, o arquivo. exe ou zip é criado. Dependendo do número de arquivos e o tamanho total dos arquivos sejam baixados, isso pode levar vários minutos. Depois que o download de arquivo é criado, baixando o arquivo ocorre em segundo plano. Isso permite que você continue trabalhando enquanto o processo de download for concluído.  
  
###### <a name="to-download-multiple-files-or-folders"></a>Para baixar vários arquivos ou pastas  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Da lista de arquivos da pasta compartilhada, clique na caixa de seleção ao lado de arquivos ou pastas que você deseja baixar para seu computador doméstico.  
  
3.  Clique em **baixar** para iniciar o download.  
  
4.  No **escolher um formato baixar** caixa de diálogo, clique para selecionar a opção de formato de download que você preferir e clique em **Okey**. O arquivo compactado é preparado na opção de formato que você selecionou.  
  
5.  No **Download do arquivo** caixa de diálogo, clique em **salvar** para salvar o arquivo em seu computador.  
  
6.  No **Salvar como** caixa de diálogo, selecione o local para salvar o arquivo e, em seguida, clique em **salvar**.  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>Recuperar arquivos compactados baixados para seu computador  
  
> [!NOTE]
>   Esta seção se aplica a um servidor que executa o Windows Server Essentials.  
  
 Se você selecionar vários arquivos ou pastas para baixar, você pode receber um arquivo de executável compactado autoextraíveis (.exe) ou um arquivo compactado (. zip).  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>Para recuperar um arquivo do arquivo compactado (.exe)  
  
1.  Em seu computador, clique duas vezes no arquivo compactado para abri-lo.  
  
2.  Siga as instruções para extrair os arquivos para uma pasta no seu computador.  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>Para recuperar um arquivo do arquivo compactado (. zip)  
  
1.  Em seu computador, clique duas vezes no arquivo compactado para abri-lo.  
  
2.  Selecione os arquivos que você deseja recuperar e arraste os arquivos para uma pasta no computador onde deseja armazená-los.  
  
    > [!NOTE]
    >  Se você usar um programa de compactação de arquivos de terceiros, siga os procedimentos para esse programa extrair os arquivos do arquivo compactado.  
  
###  <a name="BKMK_2"></a>Criar, renomear, mover, excluir ou copiar arquivos e pastas no acesso via Web remoto  
 Você pode usar o acesso via Web remoto para criar novas pastas em uma pasta compartilhada existente, renomear arquivos e pastas, mover e copiar arquivos e pastas e excluir arquivos e pastas em seu servidor.  
  
> [!NOTE]
>  Para adicionar novas pastas compartilhadas em um servidor que está executando o Windows Server Essentials, você deve usar o painel. Para conectar-se para o console do servidor de acesso via Web remoto, o **computadores** guia, clique no nome do servidor, clique em **conectar**e siga as instruções para fazer logon no servidor. Para obter informações sobre como criar pastas compartilhadas, consulte [adicionar ou mover uma pasta de servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
##### <a name="to-create-a-new-folder"></a>Para criar uma nova pasta  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Na barra de tarefas, clique em **nova pasta**.  
  
3.  Digite um nome para a pasta e clique em **Okey**.  
  
##### <a name="to-rename-a-file-or-folder"></a>Para renomear um arquivo ou pasta  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Clique com botão direito no arquivo ou pasta que você deseja renomear e, em seguida, clique em **Renomear**.  
  
3.  Digite um novo nome na caixa de texto e, em seguida, clique em **Okey**.  
  
##### <a name="to-move-files-or-folders"></a>Para mover arquivos ou pastas  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Marque a caixa de seleção ao lado de arquivos ou pastas que você deseja mover, clique com botão direito uma das pastas ou arquivos selecionados e, em seguida, clique em **Recortar**.  
  
3.  Clique com botão direito na pasta que você deseja mover os arquivos ou pastas para e clique em **colar**.  
  
##### <a name="to-delete-a-file-or-folder"></a>Para excluir um arquivo ou pasta  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Marque a caixa de seleção ao lado de arquivos ou pastas que você deseja excluir, clique com botão direito uma das pastas ou arquivos selecionados e, em seguida, clique em **excluir**.  
  
3.  Para confirmar que você deseja excluir os arquivos e pastas selecionados, clique em **Sim**.  
  
##### <a name="to-copy-files-or-folders"></a>Para copiar os arquivos ou pastas  
  
1.  No acesso via Web remoto, clique no **pastas compartilhadas** guia e clique em um link para a pasta compartilhada. Será exibida uma lista dos arquivos e pastas na pasta compartilhada.  
  
2.  Marque a caixa de seleção ao lado de arquivos ou pastas que você deseja copiar, clique com botão direito uma das pastas ou arquivos selecionados e, em seguida, clique em **cópia**.  
  
3.  Clique com botão direito na pasta que você deseja copiar os arquivos ou pastas para e clique em **colar**.  
  
##  <a name="BKMK_ConnectMobile"></a>Conectar-se de um dispositivo móvel  
  

-   [Acesso via Web remoto do uso de um dispositivo móvel](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Suporte para navegadores da Web para dispositivos móveis](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Acesso via Web remoto do uso de um dispositivo móvel](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Suporte para navegadores da Web para dispositivos móveis](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>Acesso via Web remoto do uso de um dispositivo móvel  
 Você pode fazer logon para acesso via Web remoto do seu telefone inteligente para exibir os arquivos e pastas nas pastas compartilhadas no servidor.  
  
> [!NOTE]
>  Você também pode baixar e usar o aplicativo meu servidor para Windows Server Essentials do [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) para acessar seus arquivos de mídia e pastas compartilhados armazenados no servidor.  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>Fazer logon acesso via Web remoto em um dispositivo móvel  
  
1.  Abra um navegador da Web e digite **https://***< YourDomainName\ >***/remoto** na barra de endereços.  Certifique-se de que você inclua o s no https.  
  
2.  Na página de logon de acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e, em seguida, clique na seta. Você está conectado à versão móvel do acesso via Web remoto.  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>Para alternar para a versão da área de trabalho do acesso via Web remoto  
  
1.  Abra um navegador da Web e digite **https://***< YourDomainName\ >***/remoto** na barra de endereços.  Certifique-se de que você inclua o s no https.  
  
2.  Na página de logon de acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto, clique em **versão de área de trabalho do modo de exibição**e, em seguida, clique na seta. Você está conectado à versão da área de trabalho do acesso via Web remoto.  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>Para retornar à versão para celular do acesso via Web remoto  
  
1.  Faça logoff.  
  
2.  Abra um navegador da Web e digite **https://***< YourDomainName\ >***/remoto/m** na barra de endereços. Certifique-se de que você inclua o s no https.  
  
3.  A versão móvel do acesso remoto via Web é exibida. Na página de logon de acesso via Web remoto, digite seu nome de usuário e senha nas caixas de texto e, em seguida, clique na seta. Você está conectado à versão móvel do acesso via Web remoto.  
  
 Você pode procurar arquivos e pastas nas pastas compartilhadas no servidor.  
  
###  <a name="BKMK_9"></a>Suporte para navegadores da Web para dispositivos móveis  
 Navegadores da web com suporte para dispositivos móveis incluem:  
  
-   O Internet Explorer Mobile 6.0 ou posterior  
  
-   Safari  
  
-   BlackBerry  
  
-   Symbian 6.0 ou posterior  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o acesso via Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

