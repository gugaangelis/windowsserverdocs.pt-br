---
title: "Gerenciar mídia Digital no Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6906c1dafc6d4131e07c008b9db47ebebe9b7770
ms.sourcegitcommit: 5c8d8acc59d61756377c9c4e459a47a9ab555b39
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2018
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Gerenciar mídia Digital no Windows Server Essentials

>Aplica-se a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Os tópicos a seguir abordam as recursos do seu servidor de streaming de mídia e explicam como configurar e usar o streaming de mídia em sua rede.  
  
> [!NOTE]
>  Por padrão, a funcionalidade de streaming de mídia não está disponível no Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada. Para adicionar a funcionalidade nessas versões de streaming de mídia [baixar e instalar o pacote de mídia do Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40837) da Microsoft Download Center.  
  
-   [Visão geral de mídia digital](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Gerenciar o servidor de mídia usando o painel](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Como funciona o streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ativar ou desativar de streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Adicionar arquivos de mídia digital no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Permitir ou restringir o acesso a uma biblioteca de mídia no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Renomeie a biblioteca de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Parar de compartilhar mídia digital](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Permitir que dispositivos de mídia que usam o protocolo SMB Server Message Block () para acessar arquivos compartilhados no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Processadores comuns e os perfis de vídeo compatíveis](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problemas conhecidos com tipos de arquivo de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>Visão geral de mídia digital  
 Mídia digital se refere ao áudio, vídeo e conteúdo de foto que tenha sido codificado (digitalmente compactados). Conteúdo de codificação envolve a conversão de entrada de áudio e vídeo em um arquivo de mídia digital, como um arquivo de mídia do Windows. Depois de mídia digital é codificada, ele pode ser facilmente manipulado, distribuído e executado pelos computadores e facilmente são transmitidos por redes de computador.  
  
 Exemplos dos tipos de mídia digital incluem: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Para saber mais sobre os tipos de mídia digital são suportados pelo Windows Media Player, veja [suportados pelo Windows Media Player de tipos de arquivo](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Por que eu quiser transmitir mídia digital?  
 Muitas pessoas armazenam fotos, vídeo e música em pastas compartilhadas no Windows Server Essentials. Pode haver momentos em que você queira fazer o seguinte:  
  
-   **Assista a vídeos**. O servidor pode ser usado para armazenar e transmitir grandes coleções de vídeos e programas de TV gravados mostra seus computadores ou reprodução de outra dispositivos em sua rede. Você pode transmitir vídeos para um Xbox 360 ou para um computador usando o Windows Media Player.  
  
-   **Reproduzir música**. Quando você ativar o compartilhamento de mídia para o **música** pasta compartilhada, você pode acessar suas músicas de dispositivos que dão suporte a conexão de mídia do Windows. Você não precisa ativar ou configurar as contas de usuário para transmitir do **música** pasta compartilhada depois que o compartilhamento está ativado.  
  
-   **Apresentar apresentações de slides de fotos**. Você pode armazenar suas fotos digitais no **fotos** pasta no servidor compartilhada e, em seguida, acessá-los de qualquer computador ou de um Xbox 360 estiver conectado a uma TV em casa ou no escritório. Você pode assistir a apresentações de slides de fotos, que é como transformar seus programas de TV em um quadro de imagem grande.  
  
###  <a name="BKMK_1.5"></a>Compartilhamento de mídia protegidos contra cópia  
  Windows Server Essentials não oferece suporte a compartilhamento de mídia protegidos contra cópia. Isso inclui a música que está adquirida por meio de uma loja de música online.  
  
 Mídia Copy-protected pode ser reproduzida apenas no computador ou dispositivo que você usou para comprá-lo. Proteção contra cópia impede a reprodução de mídia em mais de um computador ou dispositivo, mesmo se você copiar a mídia ao seu servidor e reproduzi-lo a partir daí. No entanto, você pode armazenar a mídia protegidos contra cópia no Windows Server Essentials e continuar a reproduzir a mídia no computador ou dispositivo que você usou para comprá-lo.  
  
##  <a name="BKMK_2"></a>Gerenciar o servidor de mídia usando o painel  
  Windows Server Essentials torna possível realizar tarefas administrativas comuns usando o painel do Windows Server Essentials. O **mídia** guia do servidor **configurações** página do painel fornece os seguintes:  
  
|Seção|Funcionalidade|  
|-------------|-------------------|  
|Servidor de mídia|O **ativar / desativar** botão permite que você ative ou desative de streaming de mídia.|  
|Qualidade de streaming de vídeo|A seta suspensa permite que você escolher o qualidade dos vídeos que são reproduzidos do servidor de streaming de vídeo.|  
|Biblioteca de mídia|Exibe o nome da biblioteca de mídia. O nome padrão da biblioteca é chamado **biblioteca de mídia Digital**, que é criado quando você ativar streaming de mídia. Para alterar o nome da biblioteca de mídia, consulte [renomeie a biblioteca de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). Você pode clicar em **personalizar** para personalizar as pastas que são compartilhadas para a biblioteca de mídia.|  
  
 Para obter mais informações, consulte [permitir ou restringir o acesso a uma biblioteca de mídia no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) e [compartilhamento de mídia protegidos contra cópia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="BKMK_3"></a>Como funciona o streaming de mídia  
 A mídia de streaming de recurso no Windows Server Essentials torna possível para computadores em rede e alguns dispositivos de mídia digital em rede para reproduzir arquivos de mídia digital armazenados no servidor.  
  
 Quando você ativa o servidor de mídia, o conteúdo que você compartilhar as bibliotecas de mídia estará disponível para reproduzir em dispositivos em sua rede que são capazes de receber streaming de mídia do seu servidor. Você pode transmitir a maioria dos tipos de arquivos de mídia digital. Alguns dos tipos mais comuns de arquivos que você pode transmitir incluem:  
  
-   Formatos de mídia do Windows (.asf,. wma,. wmv,. wm)  
  
-   Audiovisual intercalação (.avi)  
  
-   Grupo de especialistas de imagens em movimento (.mpeg,. mpg,. mp3)  
  
-   Áudio para Windows (.wav)  
  
-   CD faixa de áudio (.cda)  
  
 Para reproduzir um arquivo, basta localizar uma música, vídeo ou foto em uma pasta compartilhada, duas vezes no arquivo e o conteúdo será transmitir do servidor para seu computador e reproduzir. Para obter informações sobre como localizar e reproduzir arquivos de mídia digital armazenados no servidor, consulte [reproduzir mídia Digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
 Para transmitir sua mídia, você precisa do seguinte hardware:  
  
-   Uma rede privada com ou sem fio  
  
-   Outro computador na rede ou um dispositivo conhecido como um receptor de mídia digital (às vezes chamado de um player de mídia digital em rede). Receptores de mídia digital são dispositivos de hardware conectados à rede com ou sem fio que você pode controlar usando seu computador? mesmo se o computador estiver em outro cômodo.  
  
 Para obter mais informações, consulte [ativar ou desativar de streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="BKMK_4"></a>Ativar ou desativar de streaming de mídia  
 Você pode compartilhar músicas, vídeos e imagens do Windows Server Essentials por streaming arquivos para qualquer receptor de mídia digital compatíveis (DMR), como computadores, telefones celulares, televisões, receptores de mídia digital, extenders para o Windows Media Center (incluindo o Xbox 360) e outros dispositivos eletrônicos pessoais.  
  
 Para obter uma lista atual de dispositivos de mídia digital são compatíveis com o Windows Server Essentials, consulte o [Centro de compatibilidade do Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Ativar o compartilhamento de mídia  
 Para compartilhar a mídia que é armazenada no Windows Server Essentials, você precisa ativar streaming de mídia. Streaming de mídia está desativado por padrão.  
  
####  <a name="BKMK_2.5"></a>Para ativar ou desativar de streaming de mídia  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **configurações**, clique em **mídia**, e siga um destes procedimentos:  
  
    -   Clique em **ativar** para começar a compartilhar todos os arquivos que são armazenados na biblioteca de mídia do servidor.  
  
    -   Clique em **desativar** parar de compartilhar todos os arquivos que são armazenados na biblioteca de mídia do servidor.  
  
3.  Se você quiser compartilhar pastas adicionais na biblioteca de mídia, clique em **personalizar**e, em seguida, selecione **Sim** para cada pasta compartilhada que você deseja incluir na biblioteca de mídia.  
  
4.  Clique em **Okey** para salvar as alterações.  
  
 Para obter informações sobre os tipos de mídia digital compatíveis com o Windows Media Player, consulte [suportados pelo Windows Media Player de tipos de arquivo](https://support.microsoft.com/kb/316992).  
  
 Para obter mais informações, consulte [permitir ou restringir o acesso a uma biblioteca de mídia no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="BKMK_5"></a>Adicionar arquivos de mídia digital no servidor  
 O administrador do servidor pode adicionar mídia digital em pastas compartilhadas na biblioteca de mídia acessando o servidor diretamente ou usando o site de acesso via Web remoto para efetuar login no painel. Outros usuários podem adicionar arquivos de mídia para o servidor usando o **pastas compartilhadas** conexão na barra inicial, usando o site de acesso via Web remoto ou usando o aplicativo meu servidor para Windows Phone. Para saber mais sobre a reprodução de mídia, veja [reproduzir mídia Digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Você também pode carregar arquivos de mídia para o servidor usando o aplicativo meu servidor para Windows Phone. Você pode baixar o aplicativo meu servidor a partir do [loja do Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Para obter mais informações sobre o aplicativo meu servidor para Windows phone, consulte o blog postar [aplicativo de telefone de meu servidor para Windows Server Essentials](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Para adicionar arquivos de mídia digital em pastas compartilhadas no servidor  
  
1.  Use um dos seguintes métodos para fazer logon servidor:  
  
    1.  Para obter informações sobre como fazer logon nos acesso via Web remoto, consulte [logon acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Para obter informações sobre o registro em log com barra inicial, consulte [visão geral da barra inicial](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Procure e clique na pasta para a mídia que você está adicionando.  
  
3.  Copiar e colar ou arrastar e soltar os arquivos de mídia que você deseja adicionar à pasta compartilhada apropriada no servidor.  
  
##  <a name="BKMK_6"></a>Permitir ou restringir o acesso a uma biblioteca de mídia no servidor  
  
-   Quando você ativa o compartilhamento de mídia, ele cria quatro pastas predefinidas: músicas, fotos, vídeos e TV gravada. Se qualquer uma dessas pastas preexists no servidor, a pasta existente é reutilizada como uma pasta compartilhada para o compartilhamento de mídia. Todas as permissões de pasta s mídia conteúdo e do usuário são preservadas, e eles são compartilhados com todos os usuários da rede.  
  
-   Antes de você ativar o compartilhamento de biblioteca de mídia para uma pasta compartilhada, você deve saber que o compartilhamento de biblioteca de mídia ignora qualquer tipo de acesso à conta de usuário que você definiu para a pasta compartilhada. Por exemplo, digamos que você ativar o compartilhamento de biblioteca de mídia para o **fotos** pasta compartilhada e definir o **fotos** pasta compartilhada para **sem acesso** para um usuário conta denominado Bobby. Bobby ainda pode transmitir qualquer mídia digital o **vídeos** pasta compartilhada para qualquer player de mídia digital compatível ou DMR. Se você tiver uma mídia digital que você não deseja transmitir dessa maneira, armazene os arquivos em uma pasta que não tenha o compartilhamento de biblioteca de mídia ativado.  
  
-   Se você ativar o compartilhamento de biblioteca de mídia para uma pasta compartilhada, qualquer player de mídia digital compatível ou DMR que pode acessar sua rede do Windows Server Essentials também pode acessar sua mídia digital na pasta compartilhada. Por exemplo, se você tiver uma rede sem fio e ele não têm protegido, qualquer pessoa dentro do alcance de sua rede sem fio pode acessar sua mídia digital nessa pasta. Antes de você ativar o compartilhamento de biblioteca de mídia, certifique-se de que você proteja sua rede sem fio. Para obter mais informações, consulte a documentação do ponto de acesso sem fio.  
  
##  <a name="BKMK_8"></a>Renomeie a biblioteca de mídia  
 O nome padrão da biblioteca de mídia é **servidor de mídia Digital**. Ele é criado quando você ativa a mídia de streaming no Windows Server Essentials. Para obter mais informações sobre como ativar streaming de mídia, consulte [para ativar ou desativar de streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). Você pode modificar o nome da biblioteca de mídia a qualquer momento usando o painel do servidor.  
  
#### <a name="to-rename-the-media-library"></a>Para renomear a biblioteca de mídia  
  
1.  Abra o painel do servidor. Para abrir o painel da barra inicial, clique em **painel**, digite a senha do servidor e, em seguida, clique na seta para fazer logon.  
  
2.  No painel do servidor, clique em **configurações**.  
  
3.  Sobre o **configurações** página, clique no **mídia** guia.  
  
4.  No **biblioteca de mídia** seção o **configurações Media** página, clique no nome da sua biblioteca de mídia.  
  
5.  No **alterar o nome da biblioteca de mídia** caixa de diálogo, digite um novo nome para a biblioteca de mídia e, em seguida, clique em **Okey**.  
  
##  <a name="BKMK_9"></a>Parar de compartilhar mídia digital  
 O administrador do servidor pode parar de compartilhar mídia digital armazenados em pastas compartilhadas em um servidor que executa o Windows Server Essentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Para interromper o compartilhamento de mídia em pastas compartilhadas  
  
1.  Abra o painel do servidor.  
  
2.  No painel **Home** página, clique em **instalação**, clique em **configurar o servidor de mídia**e clique em **clique em configurar o servidor de mídia**.  
  
3.  Sobre o **mídia** página de configurações, você pode escolher uma das seguintes opções:  
  
    -   Clique em **desativar** parar de compartilhar todos os arquivos que são armazenados no servidor.  
  
    -   Clique em **personalizar**e, em seguida, selecione **não** para pastas específicas que você deseja parar de compartilhar.  
  
4.  Clique em **aplicar** ou **Okey** para salvar as alterações.  
  
##  <a name="BKMK_10"></a>Permitir que dispositivos de mídia que usam o protocolo SMB Server Message Block () para acessar arquivos compartilhados no servidor  
 Dispositivos que usam SMB Server Message Block () para acesso a arquivos e compartilhamento de rede em vez de DLNA (para streaming de mídia) exigem que a conta Convidado seja ativada. Isso permite que qualquer dispositivo ou usuário na rede para exibir o conteúdo de pastas compartilhadas sem autenticação.  
  
> [!CAUTION]
>  Quando você habilitar a conta convidado, qualquer pessoa pode acessar os recursos compartilhados no servidor por padrão.  
  
##  <a name="BKMK_CommonProcessors"></a>Processadores comuns e os perfis de vídeo compatíveis  
 Para transmitir mídia do seu servidor Windows Server Essentials, você pode usar um computador que esteja executando o sistema operacional Windows 7 ou Windows 8 ou outros dispositivos em rede (como players de mídia digital) ou o Media Center extenders (por exemplo, o Xbox 360). Quando você estiver longe de sua rede, use remoto da Web acesso Media Player para reproduzir arquivos armazenados em seu servidor.  
  
 Você precisa de uma taxa de transferência de dados entre 200 KBps e 10 MBps. Você precisa usar formatos de mídia que seu computador e dispositivos podem reconhecer e reproduzir. Nem todos os dispositivos oferecem suporte a formatos de mídia, deve haver uma maneira para seu computador e dispositivos de reproduzir os arquivos de mídia que você tem.  
  
  Windows Server Essentials contém suporte de transcodificação que determina a capacidade do computador ou do dispositivo e, em seguida, dinamicamente converte um arquivo de vídeo sem suporte em uma conta com suporte. Em geral, se o Windows Media Player 12 pode reproduzir o conteúdo em um computador que executa o Windows 7 ou Windows 8, o conteúdo no servidor será reproduzido no dispositivo conectado à rede.  
  
 A taxa de bits e de formato escolhida para transcodificação depende muito o desempenho do processador do servidor. O desempenho do processador é identificado como parte do índice de experiência do Windows. Para determinar a pontuação de desempenho do seu servidor, siga um destes procedimentos:  
  
-   Em um computador executando o Windows 7 ou Windows 8 que tem o mesmo processador do seu servidor da rede, vá para o **painel de controle**, clique em **informações de desempenho e ferramentas**e, em seguida, examine as informações sobre o **taxa e melhorar o desempenho do seu computador s** página.  
  
-   Contate o fabricante do processador.  
  
 Para a melhor experiência de usuário, escolha um qualidade de resolução que seja adequada para o processador de servidor de streaming de vídeo. O servidor será ajustada automaticamente a taxa de bits para uma destas configurações:  
  
-   **Baixo** se a pontuação do processador é menor que 3.6.  
  
-   **Médio** se a pontuação do processador for maior que 3.6 e menos de 4.2.  
  
-   **Alto** se a pontuação do processador for maior que 4.2 e menos de 6.0.  
  
-   **Melhor** se a pontuação do processador for maior que 6.0.  
  
 Se você escolher um resolução que requer mais energia de processamento que o servidor de streaming de vídeo, você pode enfrentar buffers e paradas durante o streaming de mídia do servidor.  
  
> [!NOTE]
>  Para transmitir alta definição vídeo por meio do acesso via Web remoto, você precisa de um processador com uma pontuação de pelo menos 6.0.  
  
##  <a name="BKMK_KnownIssues"></a>Problemas conhecidos com tipos de arquivo de mídia  
 O recurso de streaming de mídia no acesso via Web remoto usa o Windows Media Player 12 compartilhamento de serviço de rede. Streaming de mídia de acesso via Web remoto dá suporte a áudio, vídeo e tipos de arquivo de imagem que são suportados pelo Windows Media Player 12 e Silverlight 4.  
  
 A tabela a seguir lista os tipos de arquivo (formatos) que são aceitos por streaming de mídia de acesso via Web remoto. Se houver tipos que não estão incluídos na tabela em seu servidor de arquivos de mídia, você não pode transmiti-los por meio de streaming de mídia de acesso via Web remoto.  
  
|Tipo de arquivo|Extensão de nome de arquivo|  
|---------------|-------------------------|  
|Arquivos 3GPP|. 3GP,. 3GPP,. 3G2 e. 3gp2|  
|Arquivos de áudio Audio Data Transport Stream ADTS)|. ADTS e. ADT|  
|Arquivos de áudio AU|. au e. snd|  
|Arquivos de áudio Audio Interchange arquivo formato AIFF)|. AIF,. aifc e. aiff|  
|Arquivos AVCHD|. m2ts,. m2t e. MTS|  
|Disco de áudio de CD|cda|  
|Disco de DVD-Video|. VOB|  
|Arquivos de imagem JPEG|. jpg|  
|Arquivos de áudio MP3|. mp3 e. m3u|  
|Arquivos de vídeo MPEG|. MPEG,. mpg,. m1v,. MPA,. mpe,. mp4,. MP4V,. M4V,. M4A e. mov|  
|Arquivos de áudio e vídeos do Windows|. avi e. wav|  
|Arquivos de áudio e vídeos do Windows Media|. ASF,. asx,. wax,. WM,. wma,. wmd,. WMP,. wmv, WMX,. wpl e. wvx|  
  
> [!NOTE]
>  Se você não pode usar um tipo de arquivo que esteja listado nesta tabela, o arquivo também pode ser codificado com um codec que não é compatível com o Windows Media Player.  
  
 Para obter informações adicionais sobre formatos de arquivo com suporte, consulte [suportados pelo Windows Media Player de tipos de arquivo](https://go.microsoft.com/fwlink/p/?LinkID=196118) e [suporte para formatos de mídia, protocolos e campos de log](https://go.microsoft.com/fwlink/p/?LinkId=203339) para o Silverlight.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Reproduzir mídia Digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
