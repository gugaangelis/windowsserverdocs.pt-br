---
title: Comece com a área de trabalho remota no Mac
description: Saiba como configurar o cliente de área de trabalho remota para Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: a54ffd3d5596ba8c71deab668e4952da445ca12e
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804944"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>Comece com a área de trabalho remota no Mac

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Você pode usar o cliente de área de trabalho remota para Mac para trabalhar com aplicativos, recursos e áreas de trabalho do Windows no computador Mac. Use as seguintes informações para começar – e fazer check-out a [perguntas frequentes sobre](remote-desktop-client-faq.md) se você tiver dúvidas.

>[!NOTE]
> - Curioso sobre novas versões para o cliente do macOS? Fazer check-out [o que há de novo para a área de trabalho remota no Mac?](mac-whatsnew.md)
> - O cliente Mac é executado em computadores que executam o macOS 10.10 e versões mais recentes.
> - As informações neste artigo aplica-se principalmente para a versão completa do cliente Mac - a versão disponível na loja de aplicativos do Mac. Test drive de novos recursos ao baixar nosso aplicativo de visualização aqui: [notas de versão do cliente beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Obter o cliente de área de trabalho remota
Siga estas etapas para começar com a área de trabalho remota em seu Mac:

1. Baixe o cliente de área de trabalho remota Microsoft do [Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Se você ignorar essa etapa, você não pode se conectar a seu computador.)
3. Adicione uma conexão de área de trabalho remota ou um recurso remoto. Usar uma conexão para se conectar diretamente a um computador de Windows e um recurso remoto para usar um programa RemoteApp, a área de trabalho baseada em sessão ou um desktop virtual publicado no local usando conexões de RemoteApp e área de trabalho. Esse recurso está disponível geralmente em ambientes corporativos.

## <a name="what-about-the-mac-beta-client"></a>O que o cliente do Mac beta?
Vamos testar novos recursos no nosso canal de visualização no HockeyApp. Deseja fazer check-out? Vá para [Microsoft Remote Desktop para Mac](https://go.microsoft.com/fwlink/?LinkID=619698) e clique em **baixar**. Você não precisa criar uma conta ou entrar no HockeyApp para baixar o cliente da versão beta.

Se você já tiver o cliente, você pode verificar as atualizações para garantir que você tenha a versão mais recente. No cliente beta, clique em **Microsoft Beta de área de trabalho remota** na parte superior e depois clique em **verificar atualizações**. 

## <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de área de trabalho remota
Para criar uma conexão de área de trabalho remota:

1. No Centro de Conexão, clique em **+** e, em seguida, clique em **Desktop**.
2. Insira as seguintes informações:
   - **Nome do PC** -o nome do computador.
      - Isso pode ser um nome de computador do Windows (encontrada na **sistema** configurações), um nome de domínio ou um endereço IP.
      - Você também pode adicionar informações de porta ao final desse nome, como *MyDesktop:3389*.
   - **Conta de usuário** -adicionar a conta de usuário que você usa para acessar o computador remoto.
     - Para ingressados no Active Directory (AD), computadores ou contas locais, use um destes formatos: *user_name*, *domain\user_name*, ou <em>user_name@domain.com</em>.
     - Para o Azure Active Directory (AAD) computadores Unidos, use um destes formatos: *AzureAD\user_name* ou <em>AzureAD\user_name@domain.com</em>.
     - Você também pode escolher se deseja exigir uma senha.
     - Ao gerenciar várias contas de usuário com o mesmo nome de usuário, defina um nome amigável para diferenciar as contas.
     - Gerencie suas contas de usuário que foram salvos nas preferências do aplicativo. 

3. Você também pode definir essas configurações opcionais para a conexão:
   - Definir um nome amigável 
   - Adicionar um Gateway
   - Definir a saída de áudio
   - Trocar os botões do mouse
   - Habilitar o modo de administrador
   - Redirecionamento de pastas locais em uma sessão remota
   - Encaminhamento impressoras locais
   - Encaminhar os cartões inteligentes
4. Clique em **Salvar**.

Para iniciar a conexão, apenas clique duas vezes nele. O mesmo é verdadeiro para recursos remotos.

### <a name="export-and-import-connections"></a>Exportar e importar conexões
Você pode exportar uma definição de conexão de área de trabalho remota e usá-lo em um dispositivo diferente. Áreas de trabalho remotas são salvas em separado. Arquivos RDP.

1. No Centro de Conexão, clique com botão direito a área de trabalho remota.
2. Clique em **Exportar**.
3. Navegue até o local onde você deseja salvar a área de trabalho remota. Arquivo RDP.
4. Clique em **OK**.

Use as etapas a seguir para importar uma área de trabalho remota. Arquivo RDP.

1. Na barra de menus, clique em **arquivo** > **importação**.
2. Navegue até o. Arquivo RDP.
3. Clique em **Abrir**.

## <a name="add-a-remote-resource"></a>Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas usando conexões de RemoteApp e área de trabalho.

- A URL exibe o link para o servidor de acesso via Web RD que fornece acesso a conexões de RemoteApp e área de trabalho.
- O configuradas conexões de RemoteApp e área de trabalho são listadas.

Para adicionar um recurso remoto:

1. No, clique no Centro de Conexão **+** e, em seguida, clique em **adicionar recursos remotos**. 
2. Insira informações para o recurso remoto:
   - **URL do feed** -a URL do servidor de acesso via Web RD. Você também pode inserir sua conta de email corporativo nesse campo – isso informa ao cliente para procurar o servidor de acesso da Web de área de trabalho remota associado com seu endereço de email.
   - **Nome de usuário** -o nome de usuário a ser usado para o servidor de acesso via Web RD que você está se conectando.
   - **Senha** -a senha a ser usado para o servidor de acesso via Web RD que você está se conectando.
3. Clique em **Salvar**.


Os recursos remotos serão exibidos no centro da Conexão.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de área de trabalho remota para acessar os ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways nas preferências do aplicativo ou ao configurar uma nova conexão de área de trabalho.

Para configurar um novo gateway nas preferências:

1. No Centro de Conexão, clique em **Preferências > Gateways**. 
2. Clique o **+** botão na parte inferior da tabela insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** -o nome de usuário e a senha a ser usada para o gateway de área de trabalho remota que você está se conectando. Você também pode selecionar **usar credenciais de conexão** para usar o mesmo nome de usuário e senha usadas para a conexão de área de trabalho remota.


## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conectar a um recurso de área de trabalho ou remoto, você pode salvar as contas de usuário para selecionar de novamente. Você pode gerenciar suas contas de usuário usando o cliente de área de trabalho remota.

Para criar uma nova conta de usuário:

1. No Centro de Conexão, clique em **as configurações** > **contas**.
2. Clique em **adicionar a conta de usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** -o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: domínio \ nome_de_usuário, user_name ou user_name@domain.com.
   - **Senha** -a senha para o usuário especificado. Cada conta de usuário que você deseja salvar para usar para conexões remotas precisa ter uma senha associada a ele.
   - **Nome amigável** – se você estiver usando a mesma conta de usuário com senhas diferentes, defina um nome amigável para distinguir as contas de usuário.
4. Toque **salve**e, em seguida, toque em **configurações**.

## <a name="customize-your-display-resolution"></a>Personalizar a resolução de vídeo
Você pode especificar a resolução de vídeo para a sessão de área de trabalho remota.

1. No Centro de Conexão, clique em **preferências**.
2. Clique em **resolução**. 
3. Clique em **+** .
4. Insira uma resolução de altura e largura e, em seguida, clique em **Okey.**

Para excluir a resolução, selecioná-lo e, em seguida, clique em **-** .

**Monitores têm espaços separados** se você estiver executando o Mac OS X 10.9 e desabilitada **monitores têm espaços separados** em Mavericks (**preferências do sistema > controle de missão**), você precisa configurar Essa configuração no cliente de área de trabalho remota usando a mesma opção.

### <a name="drive-redirection-for-remote-resources"></a>Redirecionamento de unidade para recursos remotos
Redirecionamento de unidade há suporte para recursos remotos, para que você possa salvar os arquivos criados com um aplicativo remoto localmente no seu Mac. A pasta redirecionada é sempre seu diretório inicial exibido como uma unidade de rede na sessão remota.

> [!NOTE]
> Para usar esse recurso, o administrador precisa definir as configurações apropriadas no servidor.


## <a name="use-a-keyboard-in-a-remote-session"></a>Usar um teclado em uma sessão remota

Layouts de teclado do Mac diferem os layouts de teclado do Windows. 

- A chave de comando do teclado do Mac é igual a chave do Windows.
- Para executar ações que usam o botão de comando no Mac, você precisará usar o botão de controle no Windows (por exemplo: Copiar = Ctrl + C).
- As teclas de função podem ser ativadas na sessão, além disso pressionando a tecla FN (por exemplo: FN + F1).
- A tecla Alt para a direita da barra de espaço no teclado Mac é igual a tecla Alt da direita/Gr Alt no Windows.

Por padrão, a sessão remota usará a mesma localidade do teclado como o sistema operacional que você estiver executando o cliente em. (Se seu Mac estiver executando uma en-us do sistema operacional, que será usado para sessões remotas. Se a localidade de teclado do sistema operacional não for usada, verifique se o teclado definindo no PC remoto e alterando a configuração manualmente. Consulte a [perguntas frequentes sobre o cliente da área de trabalho remota](remote-desktop-client-faq.md) para obter mais informações sobre teclados e localidades.


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Suporte para autorização e autenticação conectável do gateway de área de trabalho remota

Windows Server 2012 R2 introduziu o suporte para um novo método de autenticação, autenticação conectável do Gateway de área de trabalho remota e autorização, que oferece mais flexibilidade para rotinas de autenticação personalizada. Agora você pode esse modelo de autenticação com o cliente Mac. 

> [!IMPORTANT]
> Modelos personalizados de autenticação e autorização antes do Windows 8.1 não têm suporte, embora o artigo acima discute-los.

Para saber mais sobre esse recurso, fazer check-out [ http://aka.ms/paa-sample ](http://aka.ms/paa-sample).


> [!TIP]
> Perguntas e comentários são sempre bem-vindas. No entanto, não poste uma solicitação de ajuda de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum de cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo thread. Tem alguma sugestão de recurso? Conte-na [Fórum de voz do usuário cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

