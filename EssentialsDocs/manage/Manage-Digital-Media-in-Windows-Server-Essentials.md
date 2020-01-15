---
title: Gerenciar mídia digital no Windows Server Essentials
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
ms.openlocfilehash: 8d7c4f0432e1f840717776ee5df00d64c39b18fd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947484"
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>Gerenciar mídia digital no Windows Server Essentials

>Aplica-se a: Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Os tópicos a seguir abordam as recursos do servidor de streaming de mídia e explicam como configurar e usar streaming de mídia na sua rede.  
  
> [!NOTE]
>  Por padrão, a funcionalidade de streaming de mídia não está disponível no Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada. Para adicionar a função de streaming de mídia nessas versões, [baixe e instale o Windows Server Essentials Media Pack](https://www.microsoft.com/download/details.aspx?id=40837) do Centro de Download da Microsoft.  
  
-   [Visão geral de mídia digital](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Gerenciar o servidor de mídia usando o painel](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Como funciona o streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ativar ou desativar streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Adicionar arquivos de mídia digital ao servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Permitir ou restringir o acesso a uma biblioteca de mídia no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Renomear a biblioteca de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Interromper o compartilhamento de mídia digital](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Habilitar dispositivos de mídia que usam o protocolo SMB para acessar arquivos compartilhados no servidor](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Processadores comuns e os perfis de vídeo aos quais eles dão suporte](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [Problemas conhecidos com tipos de arquivo de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>Visão geral de mídia digital  
 Mídia digital se refere ao áudio, vídeo e conteúdo de fotos que tenha sido codificado (compactado digitalmente). Codificação conteúdo envolve a conversão de entrada de áudio e vídeo para um arquivo de mídia digital, como um arquivo do Windows Media. Depois da mídia digital ser codificada, pode ser facilmente manipulada, distribuída e executada por computadores e facilmente são transmitidos por redes de computador.  
  
 Exemplos dos tipos de mídia digital incluem: Windows Media Audio (WMA), Windows Media Video (WMV), MP3, JPEG e AVI. Para obter informações sobre os tipos de mídia digital com suporte no Windows Media Player, consulte [Tipos de arquivo com suporte pelo Windows Media Player](https://support.microsoft.com/kb/316992).  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>Por que eu quero fazer transmissão de mídia digital?  
 Muitas pessoas armazenam música, vídeo e imagens em pastas compartilhadas no Windows Server Essentials. Pode haver momentos em que você deseja fazer o seguinte:  
  
-   **Assistir vídeos**. O servidor pode ser usado para armazenar e transmitir grandes coleções de vídeos e TV gravada mostrada aos computadores ou outra reprodução de dispositivos na sua rede. Você pode transmitir vídeos para um Xbox 360 ou para um computador usando o Windows Media Player.  
  
-   **Reproduzir música**. Quando você ativa o compartilhamento de mídia para a pasta compartilhada\ **música** , você poderá acessar seus arquivos de música de dispositivos que oferecem suporte a conexão de mídia do Windows. Não é necessário habilitar ou configurar as contas de usuário para transmitir da pasta compartilhad **música** depois que o compartilhamento estiver ativado.  
  
-   **Exibir apresentações de fotos**. Você pode armazenar fotos digitais na pasta compartilhada **fotos** no servidor e acessá-las a partir de qualquer computador ou um Xbox 360 conectado a uma TV em casa ou no escritório. Você pode assistir a apresentações de fotos, transformando sua TV em um grande quadro de imagens.  
  
###  <a name="BKMK_1.5"></a>Compartilhamento de mídia protegida por cópia  
  O Windows Server Essentials não oferece suporte ao compartilhamento de mídia protegida por cópia. Isso inclui músicas adquiridas por meio de um repositório de música online.  
  
 Mídia protegida contra cópia pode ser executada novamente somente no computador ou dispositivo que você usou para adquiri-lo. Proteção contra cópia impede a execução da mídia em mais de um computador ou dispositivo, mesmo que você copie a mídia para o servidor e executá-lo a partir dele. No entanto, você pode armazenar a mídia protegida por cópia no Windows Server Essentials e continuar a reproduzir a mídia no computador ou dispositivo que você usou para comprá-la.  
  
##  <a name="BKMK_2"></a>Gerenciar o servidor de mídia usando o painel  
  O Windows Server Essentials torna possível executar tarefas administrativas comuns usando o painel do Windows Server Essentials. O guia **mídia** do servidor **configurações** na página do Painel fornece o seguinte:  
  
|Seção|Funcionalidade|  
|-------------|-------------------|  
|Servidor de mídia|O botão **ativar / desativar** permite ativar ou desativar o streaming de mídia.|  
|Definir a qualidade de streaming de vídeo|A seta suspensa permite que você escolha a qualidade dos vídeos que são executados a partir do servidor de streaming de vídeo.|  
|Biblioteca de mídia|Exibe o nome da biblioteca de mídia. O nome padrão da biblioteca é chamado **Biblioteca de Mídia Digital**, que é criado quando você ativa o streaming de mídia. Para alterar o nome da biblioteca de mídia, consulte [Rename the media library](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8). Você pode clicar em **Personalizar** para Personalizar as pastas compartilhadas para a biblioteca de mídia.|  
  
 Para obter mais informações, consulte [Allow or restrict access to a media library on the server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6) e [Sharing copy-protected media](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5).  
  
##  <a name="BKMK_3"></a>Como funciona o streaming de mídia  
 O recurso de streaming de mídia no Windows Server Essentials possibilita que computadores em rede e alguns dispositivos de mídia digital em rede reproduzam arquivos de mídia digital armazenados no servidor.  
  
 Quando você ativa o servidor de mídia, o conteúdo que você compartilha nas bibliotecas de mídia estará disponível para execução em dispositivos na sua rede quesão capazes de receber streaming de mídia do seu servidor. Você pode transmitir a maioria dos tipos de arquivos de mídia digital. Alguns dos tipos mais comuns de arquivos que você pode transmitir incluem:  
  
- Formatos do Windows Media (. ASF,. wma,. wmv,. wm)  
  
- Audio Visual Interleave (. avi)  
  
- Moving Pictures Experts Group (.mpeg, .mpg, .mp3)  
  
- Áudio para Windows (.wav)  
  
- Faixa de CD de áudio (. cda)  
  
  Para executar um arquivo, basta localizar uma música, vídeo ou imagem em uma pasta compartilhada, clicar duas vezes no arquivo e o conteúdo será transmitido do servidor para o computador e executado. Para obter informações sobre como localizar e reproduzir os arquivos de mídia digital armazenados no servidor, consulte [reproduzir mídia digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
  Para transmitir sua mídia, você precisa do seguinte hardware:  
  
- Uma rede privada com ou sem fio  
  
- Qualquer outro computador na sua rede ou um dispositivo conhecido como um receptor de mídia digital (às vezes chamado de um reprodutor de mídia digital em rede). Os receptores de mídia digital são dispositivos de hardware conectados à sua rede com ou sem fio que você pode controlar usando o computador? mesmo que o computador esteja em outra sala.  
  
  Para obter mais informações, consulte [Ativar ou desativar streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4).  
  
##  <a name="BKMK_4"></a>Ativar ou desativar streaming de mídia  
 Você pode compartilhar música, vídeos e imagens do Windows Server Essentials transmitindo arquivos para qualquer receptor de mídia digital (DMR) com suporte, como computadores, celulares, televisões, receptores de mídia digital, extensores para o Windows Media Center (incluindo o Xbox 360) e outros dispositivos eletrônicos pessoais.  
  
 Para obter uma lista atual de dispositivos de mídia digital compatíveis com o Windows Server Essentials, consulte o [centro de compatibilidade do Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
### <a name="enabling-media-sharing"></a>Ativar o compartilhamento de mídia  
 Para compartilhar a mídia armazenada no Windows Server Essentials, você precisa ativar o streaming de mídia. O Streaming de mídia é desativado por configuração padrão.  
  
####  <a name="BKMK_2.5"></a>Para ativar ou desativar o streaming de mídia  
  
1. Abra o painel do Windows Server Essentials.  
  
2. Clique em **Configurações**, clique em **mídia**, e siga um procedimentos  
  
   -   Clique em **Ativar** para iniciar o compartilhamento de todos os arquivos que estão armazenados na biblioteca de mídia do servidor.  
  
   -   Clique em **Desativar** para interromper o compartilhamento de todos os arquivos que estão armazenados na biblioteca de mídia do servidor.  
  
3. Se você quiser compartilhar pastas adicionais na biblioteca de mídia, clique em **Personalizar**e selecione **Sim** para cada pasta compartilhada que você deseja incluir na biblioteca de mídia.  
  
4. Clique em **OK** para salvar suas alterações.  
  
   Para obter informações sobre os tipos de mídia digital com suporte no Windows Media Player, consulte [Tipos de arquivos com suporte pelo Windows Media Player](https://support.microsoft.com/kb/316992).  
  
   Para obter mais informações, consulte [Allow or restrict access to a media library on the server](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6).  
  
##  <a name="BKMK_5"></a>Adicionar arquivos de mídia digital ao servidor  
 O administrador do servidor pode adicionar mídia digital a pastas compartilhadas na biblioteca de mídia acessando o servidor diretamente ou usando o site de Acesso via Web remoto para entrar no painel. Outros usuários podem adicionar arquivos de mídia ao servidor usando a conexão **pastas compartilhadas** no Launchpad, usando o site de acesso via Web remoto ou usando o aplicativo meu servidor para Windows Phone. Para obter informações sobre como reproduzir mídia, consulte [reproduzir mídia digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Você também pode carregar arquivos de mídia para o servidor usando o aplicativo My Server para Windows Phone. Você pode baixar o aplicativo My Server da [Loja do Windows Phone](https://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a). Para obter mais informações sobre o aplicativo do meu servidor para Windows Phone, consulte a postagem no blog [meu aplicativo de telefone do servidor para Windows Server Essentials](https://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx).  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>Para adicionar arquivos de mídia digital para pastas compartilhadas no servidor  
  
1.  Use um dos métodos a seguir para entrar no servidor:  
  
    1.  Para obter informações sobre como fazer logon no Acesso Remoto via Web, consulte [Log on to Remote Web Access](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1).  
  
    2.  Para obter informações sobre como entrar com a Launchpad, consulte [visão geral da Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md).  
  
2.  Pesquisar e clicar na pasta para a mídia que você está adicionando.  
  
3.  Copiar e colar ou arrastar e soltar os arquivos de mídia que você deseja adicionar à pasta compartilhada apropriada no servidor.  
  
##  <a name="BKMK_6"></a>Permitir ou restringir o acesso a uma biblioteca de mídia no servidor  
  
-   Quando você ativa o compartilhamento de mídia, ele cria quatro pastas predefinidas: Música, imagens, vídeos e TV gravada. Se houver qualquer uma das pastas no servidor, a pasta existente será reutilizada como uma pasta compartilhada para compartilhamento de mídia. Todas as permissões de conteúdo de mídia e de usuário da pasta existente são preservadas e são compartilhadas com todos os usuários da rede.  
  
-   Antes de ativar o compartilhamento de biblioteca de mídia para uma pasta compartilhada, você deve saber que o compartilhamento de biblioteca de mídia ignora qualquer tipo de acesso à conta de usuário que você definir para a pasta compartilhada. Por exemplo, digamos que você ativa o compartilhamento de biblioteca de mídia para a pasta compartilhada **Fotos** e você definir a pasta compartilhada **Fotos** como **Sem Acesso** para uma conta de usuário chamada Bobby. Bobby ainda pode transmitir qualquer mídia digital a partir da pasta compartilhada **vídeos** para qualquer reprodutor de mídia digital compatível ou DMR. Se houver mídia digital que você não deseja transmitir dessa maneira, armazene os arquivos em uma pasta que não tenha o compartilhamento de biblioteca de mídia ativado.  
  
-   Se você ativar o compartilhamento de biblioteca de mídia para uma pasta compartilhada, qualquer player de mídia digital com suporte ou DMR que possa acessar sua rede do Windows Server Essentials também poderá acessar sua mídia digital nessa pasta compartilhada. Por exemplo, se você tiver uma rede sem fio e ela não estiver protegida, qualquer pessoa dentro do alcance de sua rede sem fio potencialmente pode acessar sua mídia digital nessa pasta. Antes de ativar o compartilhamento de biblioteca de mídia, certifique-se de proteger sua rede sem fio. Para obter mais informações, veja a documentação do ponto de acesso sem fio.  
  
##  <a name="BKMK_8"></a>Renomear a biblioteca de mídia  
 O nome padrão da biblioteca de mídia é **servidor de mídia Digital**. Ele é criado quando você ativa o streaming de mídia no Windows Server Essentials. Para obter mais informações sobre como ativar o streaming de mídia, consulte [para ativar ou desativar o streaming de mídia](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5). Você pode modificar o nome da biblioteca de mídia a qualquer momento usando o painel do servidor.  
  
#### <a name="to-rename-the-media-library"></a>Para renomear a biblioteca de mídia  
  
1.  Abra o Painel do servidor. Para abrir o Painel da barra inicial, clique em **Painel**, digite a senha do servidor e, em seguida, clique na seta para entrar.  
  
2.  NoPainel do servidor, clique em **configurações**.  
  
3.  Na página **configurações** , clique em **mídia** .  
  
4.  Na seção **biblioteca de mídia** da página **configurações de mídia** , clique no nome da biblioteca de mídia.  
  
5.  Na caixa de diálogo **alterar o nome da biblioteca de mídia** , digite um novo nome para a biblioteca de mídia e, em seguida, clique em **OK**.  
  
##  <a name="BKMK_9"></a>Interromper o compartilhamento de mídia digital  
 O administrador do servidor pode interromper o compartilhamento de mídia digital armazenada em pastas compartilhadas em um servidor que executa o Windows Server Essentials.  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>Para interromper o compartilhamento de mídia em pastas compartilhadas  
  
1.  Abra o Painel do servidor.  
  
2.  Na página **Início** do Painel, clique em **Configuração**, clique em **Configurar o servidor de mídia**e clique em **Clique para configurar o servidor de mídia**.  
  
3.  Na página de configurações **Mídia** , você pode escolher uma das seguintes opções:  
  
    -   Clicar em **Desativar** para interromper o compartilhamento de todos os arquivos que são armazenados no servidor.  
  
    -   Clicar em **Personalizar**e selecione **não** para pastas específicas que você deseja interromper o compartilhamento.  
  
4.  Clicar em **Aplicar** ou **OK** para salvar suas alterações.  
  
##  <a name="BKMK_10"></a>Habilitar dispositivos de mídia que usam o protocolo SMB para acessar arquivos compartilhados no servidor  
 Dispositivos que usam protocolo SMB para acesso a arquivos e compartilhamento de rede em vez de DLNA (para streaming de mídia) exigem que a conta Guest seja ativada. Isso permite que qualquer dispositivo ou usuário da rede veja o conteúdo das pastas compartilhadas sem autenticação.  
  
> [!CAUTION]
>  Quando você habilitar a conta de Convidado, qualquer pessoa pode acessar os recursos compartilhados no servidor por padrão.  
  
##  <a name="BKMK_CommonProcessors"></a>Processadores comuns e os perfis de vídeo aos quais eles dão suporte  
 Para transmitir mídia de seu servidor do Windows Server Essentials, você pode usar um computador que esteja executando o sistema operacional Windows 7 ou Windows 8 ou outros dispositivos em rede (como players de mídia digital) ou extensores do Media Center (como o Xbox 360). Quando você estiver usando sua rede, use o Player de Mídia de acesso remoto da Web para executar arquivos que são armazenados no servidor.  
  
 Você precisa de uma taxa de transferência de dados entre 200 KBps e 10 MBps. Você precisa usar formatos de mídia que seu computador e dispositivos podem reconhecer e reproduzir. Nem todos os dispositivos oferecem suporte a formatos de mídia, deve haver uma forma de seu computador e dispositivos para executar os arquivos de mídia que você tem.  
  
  O Windows Server Essentials contém suporte para transcodificação que determina a capacidade do computador ou dispositivo e converte dinamicamente um arquivo de vídeo sem suporte em um com suporte. Em geral, se o Windows Media Player 12 pode executar o conteúdo em um computador que esteja executando o Windows 7 ou Windows 8, o conteúdo no servidor funcionará no dispositivo conectado à rede.  
  
 O formato e média de bites escolhidos para transcodificação dependem totalmente do desempenho do processador do servidor. O desempenho do processador é identificado como parte do Índice de experiência do Windows. Para determinar a pontuação de desempenho do servidor, siga este procedimento:  
  
- Em um computador de rede que executa o Windows 7 ou o Windows 8 que tem o mesmo processador que o servidor, vá para o **painel de controle**, clique em **informações e ferramentas de desempenho**e examine as informações na **taxa e melhore a página desempenho do computador** .  
  
- Entre em contato com o fabricante do processador.  
  
  Para a melhor experiência de usuário, escolha um qualidade de resolução que seja apropriada para o processador do servidor de streaming de vídeo. O servidor será ajustar automaticamente à média de bits para uma destas configurações:  
  
- **Baixo** se a pontuação do processador for menor que 3.6.  
  
- **Médio** se a pontuação do processador for maior que 3,6 e menor que 4,2.  
  
- **Alta** se a pontuação do processador for maior que 4.2 e inferiores 6.0.  
  
- **Melhor** se a pontuação do processador for maior que 6.0.  
  
  Se você escolher um resolução que requer a capacidade de processamento maior que o servidor de streaming de vídeo, você pode enfrentar buffers e interromper o streaming de mídia do servidor.  
  
> [!NOTE]
>  Para fluxo de vídeo de alta definição através do Acesso Remoto via Web, é necessário um processador com pontuação de pelo menos 6.0.  
  
##  <a name="BKMK_KnownIssues"></a>Problemas conhecidos com tipos de arquivo de mídia  
 O recurso de streaming de mídia no Acesso Remoto via Web usa o Windows Media Player 12 Network Sharing Service. Streaming de mídia de Acesso Remoto via Web oferece suporte a tipos de arquivo de imagem, áudio e vídeo com suporte no Windows Media Player 12 e no Silverlight 4.  
  
 A tabela a seguir lista os tipos de arquivo (formatos) que têm suporte no streaming de mídia do Acesso via Web Remoto. Se houver mídia de tipos de arquivos no servidor que não estão incluídos na tabela, não é permitido transmiti-los através de streaming de mídia do Acesso Remoto via Web.  
  
|Tipo de arquivo|Extensão de nome de arquivo|  
|---------------|-------------------------|  
|Arquivos 3GPP|.3GP, .3gpp, .3g2 e .3gp2|  
|Arquivos de áudio (ADTS)Audio Data Transport Stream|.adts e .adt|  
|Arquivos de áudio AU|.au e .snd|  
|Arquivos de áudio Audio Interchange File Format (AIFF)|.aif, .aifc, e .aiff|  
|Arquivos AVCHD|.m2ts, .m2t e .mts|  
|CD de áudio|.cda|  
|Disco de DVD-Video|.vob|  
|Arquivos de imagem JPEG|.jpg|  
|Arquivos de áudio MP3|.mp3 e .m3u|  
|Arquivos de vídeo MPEG|.mpeg, .mpg, .m1v, .mpa, .mpe, .mp4, .mp4v, .m4v, .m4a, e .mov|  
|Arquivos de áudio e vídeo do Windows|.avi e .wav|  
|Arquivos de áudio e vídeo do Windows Media|.asf, .asx, .wax, .wm, .wma, .wmd, .wmp, .wmv, .wmx, .wpl, e .wvx|  
  
> [!NOTE]
>  Se você não pode usar um tipo de arquivo listado nesta tabela, o arquivo também pode ser codificado com um codec sem suporte no Windows Media Player.  
  
 Para obter informações adicionais sobre formatos de arquivo com suporte, consulte [Tipos de arquivo com suporte pelo Windows Media Player](https://go.microsoft.com/fwlink/p/?LinkID=196118) e [Formatos de mídia, protocolos e campos de log com suporte](https://go.microsoft.com/fwlink/p/?LinkId=203339) para Silverlight.  
  
## <a name="see-also"></a>Veja também  
  
-   [Reproduzir mídia digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
