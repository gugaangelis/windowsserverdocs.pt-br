---
title: Introdução ao cliente para iOS
description: Saiba como configurar o cliente da Área de Trabalho Remota para iOS
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: Heidilohr
manager: lizross
ms.author: helohr
date: 02/11/2020
ms.localizationpriority: medium
ms.openlocfilehash: ef13227a9f7b83f01786bbb11498da912c86581b
ms.sourcegitcommit: 32211610ad9a24d282b35ed8c0aaa179497c63bf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77780815"
---
# <a name="get-started-with-the-ios-client"></a>Introdução ao cliente para iOS

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente da Área de Trabalho Remota para iOS para trabalhar com aplicativos, recursos e áreas de trabalho do Windows usando seu dispositivo iOS (iPhones e iPads).

Use as informações a seguir para começar. Não se esqueça de conferir as [Perguntas Frequentes](remote-desktop-client-faq.md) se tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre as novas versões para o cliente iOS? Confira as [Novidades da Área de Trabalho Remota no iOS.](ios-whatsnew.md)
> - O cliente iOS dá suporte a dispositivos que executam o iOS 6.x e versões posteriores.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obter o cliente da Área de Trabalho Remota e começar a usá-lo

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Baixar o cliente da Área de Trabalho Remota da loja do iOS

Siga estas etapas para começar a usar a Área de Trabalho Remota em seu dispositivo iOS:

1. Baixar o cliente da Área de Trabalho Remota da Microsoft da [App Store do iOS](https://aka.ms/rdios) ou do [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configure seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Adicione uma [conexão de Área de Trabalho Remota](#add-a-remote-desktop-connection) ou um [recurso remoto](#add-a-remote-resource). Você usa uma conexão para se conectar diretamente a um computador Windows e um recurso remoto para usar um programa RemoteApp, uma área de trabalho baseada em sessão ou uma área de trabalho virtual publicada localmente usando conexões de RemoteApp e área de trabalho. Normalmente, esse recurso está disponível em ambientes corporativos.

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de Área de Trabalho Remota

Para criar uma conexão de área de trabalho remota:

1. Na Central de Conexão, toque em **+** e, em seguida, toque em **Adicionar PC ou Servidor**.
2. Insira as seguintes informações para a conexão de área de trabalho remota:
   - **Nome do PC** – o nome do computador. Pode ser um nome do computador Windows, um nome de domínio da Internet ou um endereço IP. Você também pode acrescentar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nome de usuário** – o nome de usuário a ser usado para acessar o computador remoto. Você pode usar os seguintes formatos: *nome_de_usuário*, *domínio\nome_de_usuário* ou `user_name@domain.com`. Você também pode especificar se um nome de usuário e senha devem ser solicitados.
3. Você também pode definir as seguintes opções adicionais:
   - **Nome amigável (opcional)** – um nome fácil de lembrar para o PC ao qual você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador será exibido.
   - **Gateway (opcional)** – o Gateway de Área de Trabalho Remota que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
   - **Som** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, no dispositivo remoto ou por não reproduzir som.
   - **Alternar botões do mouse** – sempre que um gesto de mouse enviaria um comando referente ao botão esquerdo do mouse, ele envia o mesmo comando usando o botão direito do mouse. Isso é necessário quando o computador remoto está configurado para o modo de mouse para canhotos.
   - **Modo Admin** – conecte-se a uma sessão de administração em um servidor executando o Windows Server 2003 ou posterior.
4. Toque em **Salvar**.

É necessário editar essas configurações? Pressione e mantenha pressionada a área de trabalho que deseja editar e, em seguida, toque no ícone de configurações.

### <a name="add-a-remote-resource"></a>Adicionar um recurso remoto

Os recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas usando conexões RemoteApp e de Área de Trabalho.

- A URL exibe o link para o servidor de Acesso via Web à Área de Trabalho Remota, que fornece acesso a conexões de RemoteApp e Área de Trabalho.
- As Conexões de RemoteApp e Área de Trabalho são listadas.

Para adicionar um recurso remoto:

1. Na tela da Central de Conexão, toque em **+** e, em seguida, toque em **Adicionar Recursos Remotos**.
2. Insira informações para o recurso remoto:
   - **URL do Feed** – a URL do servidor de Acesso via Web à Área de Trabalho Remota. Você também pode inserir sua conta de email corporativo nesse campo – isso instrui o cliente a pesquisar pelo servidor de Acesso via Web à Área de Trabalho Remota associado com seu endereço de email.
   - **Nome de usuário** – o nome de usuário a ser usado para o servidor de Acesso via Web à Área de Trabalho Remota ao qual você está se conectando.
   - **Senha** – a senha a ser usada para o servidor de Acesso via Web da Área de Trabalho Remota ao qual você está se conectando.
3. Toque em **Salvar**.

Os recursos remotos serão exibidos na Central de Conexão.

## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conecta a uma área de trabalho ou a recursos remotos, você pode salvar as contas de usuário para selecioná-las novamente.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de Usuário**.
2. Toque em **Adicionar Conta de Usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** – o nome do usuário a salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_de_usuário, domínio\nome_de_usuário ou user_name@domain.com.
   - **Senha** – a senha para o usuário que você especificou. Todas as contas de usuário que você deseja salvar para uso com conexões remotas precisam ter uma senha associada a elas.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de Usuário**.
2. Selecione a conta que você deseja excluir.
3. Toque em **Excluir**.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de Área de Trabalho Remota para acessar os ativos internos

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de Área de Trabalho Remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações** > **Gateways**.
2. Toque em **Adicionar Gateway de Área de Trabalho Remota**.
3. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo, **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** – o nome de usuário e a senha a serem usados para o Gateway de Área de Trabalho Remota ao qual você está se conectando. Você também pode selecionar **Usar credenciais de conexão** para usar o mesmo nome de usuário e senha usados para a conexão à área de trabalho remota.

## <a name="navigate-the-remote-desktop-session"></a>Navegar pela sessão de Área de Trabalho Remota

Quando você inicia uma sessão de Área de Trabalho Remota, existem ferramentas disponíveis que você pode usar para navegar pela sessão.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma Conexão de Área de Trabalho Remota

1. Toque na conexão de área de trabalho remota para iniciar a sessão.
2. Se for solicitado a você que verifique o certificado para a Área de Trabalho Remota, toque em **Aceitar**. É possível optar por sempre aceitar deslizando o botão de alternância **Não me pergunte novamente para conexões com este computador** para **ATIVADO**.

### <a name="connection-bar"></a>Barra de Conexão

A barra de conexão lhe dá acesso a controles de navegação adicionais.

- **Controle de movimento panorâmico**: o controle de movimento panorâmico permite que a tela seja ampliada e movida. Observe que o controle de movimento panorâmico só está disponível usando o toque direto.
   - Habilitar/desabilitar o controle de movimento panorâmico: toque no ícone de bandeja na barra de conexão para exibir o controle de movimento panorâmico e ampliar a tela. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Usar o controle de movimento panorâmico: toque no controle de movimento panorâmico e segure, depois arraste na direção em que você deseja mover a tela.
   - Mover o controle de movimento panorâmico: dê um toque duplo no controle de movimento panorâmico e segure para mover o controle na tela.
- **Nome da conexão**: o nome da conexão atual é exibido. Toque no nome da conexão para exibir a barra de seleção de sessão.
- **Teclado**: toque no ícone de teclado para exibir ou ocultar o teclado. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mover a barra de conexão**: toque na barra de conexão e segure e, em seguida, arraste e solte em uma nova localização na parte superior da tela.

### <a name="session-selection"></a>Seleção de sessão

Você pode ter várias conexões abertas em computadores diferentes ao mesmo tempo. Toque na barra de conexão para exibir a barra de seleção de sessão no lado esquerdo da tela. A barra de seleção de sessão permite exibir suas conexões abertas e alternar entre elas.

- Alternar entre aplicativos em uma sessão aberta do recurso remoto.

    Quando está conectado a recursos remotos, você pode alternar entre os aplicativos abertos dentro dessa sessão tocando no menu do expansor e escolhendo na lista de itens disponíveis.
- Iniciar uma nova sessão

  Você pode iniciar novos aplicativos ou sessões da área de trabalho de dentro da conexão atual: toque em **Iniciar Novo** e, em seguida, escolha na lista de itens disponíveis.

- Desconectar uma sessão

  Para desconectar uma sessão, toque no X no lado esquerdo do bloco de sessão.

### <a name="command-bar"></a>Barra de comandos

A barra de comandos substituiu a barra de Utilitário da versão 8.0.1 em diante. Você pode alternar entre os modos de mouse e voltar para a central de conexão usando a barra de comandos.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usar gestos de toque e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> Ao interagir com o Windows 8 ou mais recente, gestos de toque nativos são compatíveis no modo de toque direto. Para obter mais informações sobre gestos no Windows 8, confira [Toque: deslizar o dedo, tocar e muito mais](https://windows.microsoft.com/windows-8/touch-swipe-tap-beyond).

| Modo do mouse    | Operação do mouse      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque direto  | Clicar com o botão esquerdo do mouse           | Tocar com um dedo                                               |
| Toque direto  | Clicar com o botão direito do mouse          | Tocar e segurar com um dedo                                      |
| Ponteiro do mouse | Clicar com o botão esquerdo do mouse           | Tocar com um dedo                                               |
| Ponteiro do mouse | Clicar com o botão do mouse esquerdo e arrastar  | Dar um toque duplo com um dedo e segurar, depois arrastar                    |
| Ponteiro do mouse | Clicar com o botão direito do mouse          | Tocar com dois dedos                                               |
| Ponteiro do mouse | Clicar com o botão direito do mouse e arrastar | Dar um toque duplo com dois dedos e segurar, depois arrastar                    |
| Ponteiro do mouse | Botão de rolagem do mouse          | Tocar com dois dedos e segurar, depois arrastar para cima ou baixo                |
| Ponteiro do mouse | Zoom                 | Pince com dois dedos para ampliar ou afaste dois dedos para reduzir |

## <a name="supported-input-devices"></a>Dispositivos de entrada com suporte

O [suporte básico ao mouse Bluetooth](https://support.apple.com/HT210546) está disponível no iOS 13 e no iPadOS como um recurso de acessibilidade. A integração mais profunda de mouse no Cliente de Área de Trabalho Remota está disponível nos modelos de mouse Swiftpoint GT e ProPoint. Além disso, também há suporte para os teclados externos que são compatíveis com o iOS e o iPadOS.

Para obter mais informações sobre o suporte a dispositivos, confira [Novidades do cliente iOS](ios-whatsnew.md) e a [App Store do iOS](https://aka.ms/rdios).

> [!TIP]
> A Swiftpoint está oferecendo um [desconto exclusivo no mouse ProPoint](https://www.swiftpoint.com/microsoft) para os usuários do cliente do iOS.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usar um teclado em uma sessão remota

Você pode usar um teclado virtual ou um teclado físico em sua sessão remota.

Para teclados virtuais, use o botão na extremidade direita da barra acima do teclado para alternar entre o teclado padrão e o adicional.

Se o Bluetooth estiver habilitado para seu dispositivo iOS, o cliente detectará automaticamente o teclado Bluetooth.

Embora algumas combinações de teclas possam não funcionar como esperado em uma sessão remota, muitas das combinações comuns de teclas Windows, como CTRL + C, CTRL + V e ALT + TAB, funcionarão.

> [!IMPORTANT]
> Perguntas e comentários são sempre bem-vindos. No entanto, NÃO publique uma solicitação de ajuda com solução de problemas usando o recurso de comentários no final deste artigo. Em vez disso, vá para o [Fórum de cliente da Área de Trabalho Remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicie uma nova conversa. Tem alguma sugestão de recurso? Conte-nos no [Fórum de voz do usuário cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
