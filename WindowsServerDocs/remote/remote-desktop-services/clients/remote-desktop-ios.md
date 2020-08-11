---
title: Introdução ao cliente para iOS
description: Saiba como configurar o cliente da Área de Trabalho Remota para iOS
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: Heidilohr
manager: lizross
ms.author: helohr
ms.date: 07/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 723fa40e1c2d446381b333eee1289a25adefd5d8
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997371"
---
# <a name="get-started-with-the-ios-client"></a>Introdução ao cliente para iOS

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente da Área de Trabalho Remota para iOS para trabalhar com aplicativos, recursos e áreas de trabalho do Windows usando seu dispositivo iOS (iPhones e iPads).

Use as informações a seguir para começar. Não se esqueça de conferir as [Perguntas Frequentes](remote-desktop-client-faq.md) se tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre as novas versões para o cliente iOS? Confira as [Novidades da Área de Trabalho Remota no iOS](ios-whatsnew.md).
> - O cliente iOS dá suporte a dispositivos que executam o iOS 6.x e versões posteriores.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obter o cliente da Área de Trabalho Remota e começar a usá-lo

Esta seção descreverá como baixar e configurar o cliente da Área de Trabalho Remota para iOS.

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Baixar o cliente da Área de Trabalho Remota da loja do iOS

Primeiro, você precisará baixar o cliente e configurar seu computador para se conectar aos recursos remotos.

Para baixar o cliente:

1. Baixar o cliente da Área de Trabalho Remota da Microsoft da [App Store do iOS](https://aka.ms/rdios) ou do [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configure seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).

### <a name="add-a-pc"></a>Adicionar um PC

Depois que você baixar o cliente e configurar seu computador para aceitar conexões remotas, será o momento de realmente adicionar um computador.

Para adicionar um PC:

1. Na Central de Conexão, toque em **+** e em **Adicionar Computador**.
2. Insira as seguintes informações:
   - **Nome do PC** – o nome do computador. O nome do computador pode ser um nome de computador Windows, um nome de domínio da Internet ou um endereço IP. Você também pode acrescentar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nome de usuário**: o nome de usuário a ser usado para acessar o computador remoto. Você pode usar os seguintes formatos: *nome_de_usuário*, *domínio\nome_de_usuário* ou `user_name@domain.com`. Você também pode selecionar **Perguntar quando necessário** para ser solicitado a fornecer um nome de usuário e uma senha quando necessário.
3. Você também pode definir as seguintes opções adicionais:
   - **Nome amigável (opcional)** – um nome fácil de lembrar para o PC ao qual você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador será exibido.
   - **Gateway (opcional)** – o Gateway de Área de Trabalho Remota que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
   - **Som** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, no dispositivo remoto ou por não reproduzir som.
   - **Alternar botões do mouse** – sempre que um gesto de mouse enviaria um comando referente ao botão esquerdo do mouse, ele envia o mesmo comando usando o botão direito do mouse. A troca de botões do mouse é necessária quando o computador remoto está configurado para o modo de mouse para canhotos.
   - **Modo Admin** – conecte-se a uma sessão de administração em um servidor executando o Windows Server 2003 ou posterior.
   - **Área de transferência** – escolha se deseja redirecionar texto e imagens na área de transferência para seu computador.
   - **Armazenamento** – escolha se deseja redirecionar o armazenamento para seu computador.
4. Toque em **Salvar**.

É necessário editar essas configurações? Selecione e segure a área de trabalho que deseja editar e toque no ícone de configurações.

### <a name="add-a-workspace"></a>Adicionar um workspace

Para obter uma lista de recursos gerenciados que você pode acessar no seu iOS, adicione um workspace assinando o feed fornecido pelo administrador.

Para adicionar um workspace:

1. Na tela da Central de Conexão, toque em **+** e em **Adicionar workspace**.
2. No campo URL do Feed, insira a URL do feed que você deseja adicionar. Essa URL pode ser uma URL ou um endereço de email.
   - Se você usar uma URL, use a que o administrador lhe forneceu.
      - Essa URL é geralmente uma URL da Área de Trabalho Virtual do Windows. A que você usa depende de qual versão da Área de Trabalho Virtual do Windows que você está usando.
        - Para a versão Fall 2019, use `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`.
        - Para a versão Spring 2020, use `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`.
   - Se você usar um endereço de email, insira seu endereço de email. A inserção do seu endereço de email instrui o cliente a pesquisar uma URL associada ao seu endereço de email, caso o administrador tenha configurado o servidor dessa maneira.
3. Toque em **Avançar**.
4. Forneça suas credenciais quando solicitado.
   - Em **Nome de usuário**, forneça o nome de usuário de uma conta com permissão para acessar recursos.
   - Em **Senha**, forneça a senha da conta.
   - Você também pode ser solicitado a fornecer informações adicionais, dependendo das configurações com as quais seu administrador configurou a autenticação.
5. Toque em **Salvar**.

Depois que você terminar, a Central de Conexão deverá exibir os recursos remotos.

Após a assinatura de um feed, o conteúdo dele será atualizado automaticamente de maneira regular. Os recursos podem ser adicionados, alterados ou removidos com base nas alterações realizadas pelo seu administrador.

## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Ao se conectar a um PC ou workspace, você pode salvar as contas de usuário para selecioná-las novamente.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de Usuário**.
2. Toque em **Adicionar Conta de Usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** – o nome do usuário a salvar para uso com uma conexão remota. Insira o nome de usuário em um dos seguintes formatos: `user_name`, `domain\user_name` ou `user_name@domain.com`.
   - **Senha** – a senha para o usuário que você especificou.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de Usuário**.
2. Selecione a conta que você deseja excluir.
3. Toque em **Excluir**.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de Área de Trabalho Remota para acessar os ativos internos

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de Área de Trabalho Remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações** > **Gateways**.
2. Toque em **Adicionar gateway**.
3. Insira as seguintes informações:
   - **Nome do gateway** – o nome do computador que você deseja usar como um gateway. O nome do gateway pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo, **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** – o nome de usuário e a senha a serem usados para o Gateway de Área de Trabalho Remota ao qual você está se conectando. Selecione também a opção **Usar credenciais de conexão** para usar o mesmo nome de usuário e a senha usados para a Conexão de Área de Trabalho Remota.

## <a name="navigate-the-remote-desktop-session"></a>Navegar pela sessão de Área de Trabalho Remota

Esta seção descreve as ferramentas que você pode usar para ajudar a navegar na sessão da Área de Trabalho Remota.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma conexão de Área de Trabalho Remota

1. Toque na conexão de área de trabalho remota para iniciar a sessão.
2. Se precisar confirmar o certificado para a Área de Trabalho Remota, toque em **Aceitar**. Para aceitar essa opção por padrão, defina a configuração **Não me perguntar novamente sobre conexões com este computador** como **Ativado**.

### <a name="connection-bar"></a>Barra de conexão

A barra de conexão lhe dá acesso a controles de navegação adicionais.

- **Controle de movimento panorâmico**: o controle de movimento panorâmico permite que a tela seja ampliada e movida. O controle de movimento panorâmico só está disponível por meio do toque direto.
   - Para habilitar ou desabilitar o controle de movimento panorâmico, toque no ícone de movimento panorâmico na barra de conexão para exibir o controle de movimento panorâmico. A tela será ampliada enquanto o controle de movimento panorâmico estiver ativo. o ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Para usar o controle panorâmico, toque nele e segure-o. Enquanto o segura, arraste os dedos na direção em que deseja mover a tela.
   - Para mover o controle de movimento panorâmico, dê um toque duplo no controle de movimento panorâmico e segure-o para mover o controle na tela.
- **Nome da conexão**: o nome da conexão atual é exibido. Toque no nome da conexão para exibir a barra de seleção de sessão.
- **Teclado**: toque no ícone de teclado para exibir ou ocultar o teclado. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mover a barra de conexão**: Toque na barra de conexão e segure-a. Ao segurar a barra, arraste-a para a nova localização. Solte a barra para colocá-la na nova localização.

### <a name="session-selection"></a>Seleção de sessão

Você pode ter várias conexões abertas em computadores diferentes ao mesmo tempo. Toque na barra de conexão para exibir a barra de seleção de sessão no lado esquerdo da tela. A barra de seleção de sessão permite exibir suas conexões abertas e alternar entre elas.

Veja o que você pode fazer com a barra de seleção de sessão:

- Para alternar entre aplicativos em uma sessão aberta de recurso remoto, toque no menu do expansor e escolha um aplicativo na lista.
- Toque em **Iniciar Novo** para iniciar uma nova sessão e escolha uma sessão na lista de sessões disponíveis.
- Toque no ícone **X** no lado esquerdo do bloco da sessão para se desconectar da sessão.

### <a name="command-bar"></a>Barra de comandos

A barra de comandos substituiu a barra de Utilitário da versão 8.0.1 em diante. Use a barra de comandos para alternar entre os modos de mouse e voltar à Central de Conexão.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usar gestos de toque e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> No Windows 8 ou posterior, há suporte para gestos de toque nativos no modo de Toque Direto. Para obter mais informações sobre os gestos do Windows 8, confira [Toque: deslizar o dedo, tocar e muito mais](https://windows.microsoft.com/windows-8/touch-swipe-tap-beyond).

| Modo do mouse    | Operação do mouse      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque direto  | Clique com o botão esquerdo do mouse           | Toque com um dedo                                               |
| Toque direto  | Clique com o botão direito em          | Tocar e segurar com um dedo                                      |
| Ponteiro do mouse | Clique com o botão esquerdo do mouse           | Toque com um dedo                                               |
| Ponteiro do mouse | Clique com o botão esquerdo do mouse e arraste  | Tocar e segurar com um dedo e arrastar                   |
| Ponteiro do mouse | Clique com o botão direito em          | Tocar com dois dedos                                              |
| Ponteiro do mouse | Clicar com o botão direito do mouse e arrastar | Tocar duas vezes e segurar com dois dedos e arrastar                    |
| Ponteiro do mouse | Botão de rolagem do mouse          | Dar um toque duplo e segurar com dois dedos e arrastar para cima ou para baixo                |
| Ponteiro do mouse | Zoom                 | Com dois dedos, pinçar para reduzir e abrir os dedos para ampliar |

## <a name="supported-input-devices"></a>Dispositivos de entrada com suporte

O cliente tem [suporte ao mouse Bluetooth](https://support.apple.com/HT210546) no iOS 13 e no iPadOS como um recurso de acessibilidade. Use as opções de mouse Swiftpoint GT ou ProPoint para uma integração de mouse mais profunda. O cliente também dá suporte a teclados externos compatíveis com o iOS e o iPadOS.

Para obter mais informações sobre o suporte a dispositivos, confira [Novidades do cliente iOS](ios-whatsnew.md) e a [App Store do iOS](https://aka.ms/rdios).

> [!TIP]
> A Swiftpoint está oferecendo um [desconto exclusivo no mouse ProPoint](https://www.swiftpoint.com/microsoft) para os usuários do cliente do iOS.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usar um teclado em uma sessão remota

Você pode usar um teclado virtual ou um teclado físico em sua sessão remota.

Para teclados virtuais, use o botão na extremidade direita da barra acima do teclado para alternar entre o teclado padrão e o adicional.

Se o Bluetooth estiver habilitado para seu dispositivo iOS, o cliente detectará automaticamente o teclado Bluetooth.

Embora algumas combinações de teclas possam não funcionar como esperado em uma sessão remota, muitas das combinações comuns de teclas Windows, como CTRL + C, CTRL + V e ALT + TAB, funcionarão.

> [!TIP]
> Perguntas e comentários são sempre bem-vindos. No entanto, se você postar solicitações de suporte ou comentários sobre o produto na seção de comentários deste artigo, não poderemos responder aos seus comentários. Caso precise de ajuda ou deseje solucionar problemas do seu cliente, recomendamos expressamente que você acesse o [fórum de clientes da Área de Trabalho Remota](/answers/topics/windows-remote-desktop-client.html) e inicie uma nova conversa. Caso tenha uma sugestão de recurso, conte-nos usando o [fórum de clientes UserVoice](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).