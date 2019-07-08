---
title: Introdução à Área de Trabalho Remota no Android
description: Etapas básicas de configuração para o cliente de Área de Trabalho Remota para Android.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b4b188eb8148b2f4e5c6672b07884af8fdcd0c60
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446743"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Introdução à Área de Trabalho Remota no Android

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de Área de Trabalho Remota para o Android para trabalhar com áreas de trabalho e aplicativos do Windows diretamente do seu dispositivo Android.

Use as informações a seguir para começar. Não se esqueça de conferir as [Perguntas Frequentes](remote-desktop-client-faq.md) se tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre novas versões para o cliente Android? Confira as [Novidades da Área de Trabalho Remota no Android.](android-whatsnew.md)
> Você pode executar o cliente no Android 4.1 e dispositivos mais recentes, bem como em Chromebooks com ChromeOS 53 instalado. Saiba mais sobre aplicativos Android no Chrome [aqui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Obter o cliente de Área de Trabalho Remota e começar a usá-lo

Siga estas etapas para começar com a Área de Trabalho Remota em seu dispositivo Android:

1. Baixe o cliente de Área de Trabalho Remota do [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configure seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de Área de Trabalho Remota ou um recurso remoto. Você usa uma conexão para se conectar diretamente a um computador Windows e um recurso remoto para usar um programa RemoteApp, área de trabalho baseada em sessão ou área de trabalho virtual publicada localmente. 
4. Crie um widget para que você acessar a Área de Trabalho Remota rapidamente.

> [!NOTE]
> Se você deseja disponibilizar uma versão piloto de novos recursos mais cedo, é recomendável baixar nosso aplicativo [Beta da Área de Trabalho Remota da Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) da Google Play Store. 

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de Área de Trabalho Remota

Para criar uma conexão de Área de Trabalho Remota:

1. Na Central de Conexão, toque em **+** e, em seguida, toque em **Área de Trabalho**.
2. Insira as informações a seguir para o computador com o qual você deseja se conectar:
   - **Nome do PC** – o nome do computador. Pode ser um nome do computador Windows, um nome de domínio da Internet ou um endereço IP. Você também pode acrescentar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nome de usuário** – o nome de usuário a ser usado para acessar o computador remoto. Você pode usar os seguintes formatos: *nome_de_usuário*, *domínio\nome_de_usuário* ou <em>user_name@domain.com</em>. Você também pode especificar se um nome de usuário e senha devem ser solicitados.
3. Você também pode definir as seguintes opções adicionais:
   - **Nome amigável** – um nome fácil de lembrar para o PC ao qual você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador será exibido.
   - **Gateway** – o Gateway de Área de Trabalho Remota que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
    Precisa configurar um Gateway de Área de Trabalho Remota?
   - **Som** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, no dispositivo remoto ou por não reproduzir som.
   - **Personalizar a resolução de vídeo** – defina uma resolução personalizada para uma conexão habilitando essa configuração. Quando desativado, é aplicada a resolução que você definiu nas configurações globais do aplicativo.
   - **Trocar os botões do mouse** – use essa opção para trocar as funções dos botões esquerdo do mouse por aquelas do botão direito do mouse. (Isso é especialmente útil se o computador remoto está configurado para um usuário canhoto, mas você usa um mouse para destros.)
   - **Conectar-se à sessão de administrador** – use essa opção para se conectar a uma sessão de console para administrar um servidor Windows.
   - **Redirecionar para o armazenamento local** – monta seu armazenamento local como um sistema de arquivos remoto no PC remoto.
4. Toque em **Salvar**.

É necessário editar essas configurações? Toque no menu de estouro ( **...** ) ao lado do nome da área de trabalho e, em seguida, toque em **editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro ( **...** ) e, em seguida, toque em **remover**.

>[!TIP]
> Se você receber o erro 0xf07 por uma senha incorreta ("Não foi possível conectar ao PC remoto porque a senha associada com a conta de usuário expirou"), altere sua senha e tente novamente.

### <a name="add-a-remote-resource"></a>Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas usando conexões RemoteApp e de Área de Trabalho.

Para adicionar um recurso remoto:

1. Na tela da Central de Conexão, toque em **+** e, em seguida, toque em **Feed de Recursos Remotos**. 
2. Insira informações para o recurso remoto:
   - **Email ou URL** –a URL do servidor de Acesso via Web da Área de Trabalho Remota. Você também pode inserir sua conta de email corporativo nesse campo – isso informa ao cliente para pesquisar pelo servidor de Acesso via Web da Área de Trabalho Remota associado com seu endereço de email.
   - **Nome de usuário** – o nome de usuário a ser usado para o servidor de Acesso via Web da Área de Trabalho Remota ao qual você está se conectando.
   - **Senha** – a senha a ser usada para o servidor de Acesso via Web da Área de Trabalho Remota ao qual você está se conectando.
3. Toque em **Salvar**.

Os recursos remotos serão exibidos na Central de Conexão.


Para excluir os recursos remotos:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do recurso remoto.
2. Toque em **Remover**.
3. Confirme a exclusão.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Widgets – fixe uma área de trabalho salva em sua tela inicial

Os aplicativos de Área de Trabalho Remota dão suporte à fixação de conexões em sua tela inicial usando o recurso de widget do Android. A maneira em que você adiciona um widget depende do tipo de dispositivo Android que você está usando e do seu sistema operacional. Aqui está a maneira mais comum para adicionar um widget: 

1. Toque em **aplicativos** para iniciar o menu de aplicativos.
2. Toque em **widgets**.
3. Passe o dedo pelos widgets e procure o ícone de Área de Trabalho Remota com a descrição "Fixar Área de Trabalho Remota".
4. Toque nesse widget de Área de Trabalho Remota e segure, movendo-o em seguida para a tela inicial.
5. Quando soltar o ícone, você verá as áreas de trabalho remotas salvas. Escolha a conexão que você deseja salvar em sua tela inicial.

Agora você pode iniciar a conexão de área de trabalho remota tocando nela diretamente da sua tela inicial.

> [!NOTE]
> Se você renomear a conexão de área de trabalho no aplicativo de Área de Trabalho Remota, o rótulo dessa Área de Trabalho Remota fixada não será atualizado.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de Área de Trabalho Remota para acessar os ativos internos

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de Área de Trabalho Remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações > Gateways**. Toque em **+** para adicionar um novo gateway.
2. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Pode ser um nome do computador Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** – o nome de usuário e a senha a serem usados para o Gateway de Área de Trabalho Remota ao qual você está se conectando. Você também pode selecionar **Usar conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de área de trabalho remota.

## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conecta a uma área de trabalho ou a recursos remotos, você pode salvar as contas de usuário para selecioná-las novamente. Você também pode definir contas de usuário no cliente propriamente dito, em vez de salvar os dados de usuário quando você se conecta a uma área de trabalho.

Para criar uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de usuário**.
2. Toque em **+** para adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** – o nome do usuário a salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_de_usuário, domínio\nome_de_usuário ou user_name@domain.com.
   - **Senha** – a senha para o usuário que você especificou. Todas as contas de usuário que você deseja salvar para uso com conexões remotas precisam ter uma senha associada a elas.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações > Contas de usuário**.
2. Toque em uma conta de usuário na lista e segure para selecioná-la. Vários usuários podem ser selecionados.
3. Toque na lixeira para excluir o usuário selecionado.

## <a name="navigate-the-remote-desktop-session"></a>Navegar pela sessão de Área de Trabalho Remota
Quando você inicia uma conexão de Área de Trabalho Remota, existem ferramentas disponíveis que você pode usar para navegar pela sessão.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma conexão de Área de Trabalho Remota

1. Toque na conexão de Área de Trabalho Remota para iniciar a sessão. 
2. Se for solicitado a você que verifique o certificado para a Área de Trabalho Remota, toque em **Conectar**. Você também pode selecionar **Não me pergunte novamente para conexões com este computador** para aceitar o certificado sempre.

### <a name="manage-global-app-settings"></a>Gerenciar configurações globais do aplicativo

Você pode definir as seguintes configurações globais no seu cliente Android:

- **Mostrar versão prévia da área de trabalho** – permite que você veja uma versão prévia de uma área de trabalho na Central de Conexão antes de se conectar a ela. Por padrão, isso é definido como **ativado**.
- **Pinçar Para Aplicar Zoom** – permite que você use gestos de pinçar para aplicar zoom. Se o aplicativo que você estiver usando por meio da Área de Trabalho Remota der suporte a multitoque (introduzido no Windows 8), **desative** essa configuração.
- **Ajudar a melhorar a Área de Trabalho Remota** – envia dados anônimos à Microsoft. Usamos esses dados para melhorar o cliente. Você pode saber mais sobre como tratamos esses dados anônimos e privados consultando a [Política de privacidade do cliente da Área de Trabalho Remota](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Por padrão, essa configuração fica **ativada**.
- **Tela** – há duas configurações globais para a tela:
  - **Orientação** – define a orientação preferida (paisagem ou retrato) para a sua sessão. 
    >[!NOTE]
    > Se você se conectar a um computador executando o Windows 8 ou uma versão anterior do Windows, a sessão não será dimensionada corretamente. Sua melhor saída é desconectar-se do computador e, em seguida, reconectar-se na orientação que você deseja usar. Uma opção ainda melhor é atualizar o PC pelo menos para o Windows 8.1.

  - **Resolução** – define a resolução que você deseja usar para conexões de área de trabalho globalmente. Se você já tiver definido uma resolução personalizada para um aplicativo ou conexão individual, esta configuração não a mudará.
    >[!NOTE]
    >Quando você altera uma das configurações de exibição, elas se aplicam apenas a novas conexões desse ponto em diante. Para ver a alteração em uma sessão à qual você já está conectado, desconecte-se e conecte-se novamente.

### <a name="connection-bar"></a>Barra de Conexão

A barra de conexão lhe dá acesso a controles de navegação adicionais. Por padrão, a barra de conexão é colocada no meio da parte superior da tela. Toque duas vezes na barra e arraste-a para a esquerda ou a direita para movê-la.

- **Controle de movimento panorâmico**: o controle de movimento panorâmico permite que a tela seja ampliada e movida. Observe que o controle de movimento panorâmico só está disponível usando o toque direto.
   - Habilitar/desabilitar o controle de movimento panorâmico: toque no ícone de bandeja na barra de conexão para exibir o controle de movimento panorâmico e ampliar a tela. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Usar o controle de movimento panorâmico: toque no controle de movimento panorâmico e segure, depois arraste na direção em que você deseja mover a tela.
   - Mover o controle de movimento panorâmico: dê um toque duplo no controle de movimento panorâmico e segure para mover o controle na tela.
- **Opções adicionais**: toque no ícone de opções adicionais para exibir a barra de seleção de sessão e a barra de comandos (veja abaixo).
- **Teclado**: toque no ícone de teclado para exibir ou ocultar o teclado. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mover a barra de conexão**: toque na barra de conexão e segure e, em seguida, arraste e solte em uma nova localização na parte superior da tela.


### <a name="command-bar"></a>Barra de comandos

Toque na barra de conexão para exibir a barra de comandos no lado direito da tela. Você pode alternar entre os modos de mouse (toque direto e ponteiro do mouse). Use o botão de tela inicial para retornar à Central de Conexão da barra de comandos. Como alternativa, você pode usar o botão voltar para realizar a mesma ação. Sua sessão ativa não será desconectada. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usar gestos de toque direto e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> Ao interagir com o Windows 8 ou mais recente, gestos de toque nativos são compatíveis no modo de toque direto. 

| Modo de mouse    | Operação do mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Clicar com o botão esquerdo do mouse           | Tocar com um dedo                                                          |
| Toque direto  | Clicar com o botão direito do mouse          | Tocar e segurar com um dedo                                                 |
| Ponteiro do mouse | Zoom                 | Usar dois dedos e fazer movimento de pinça para aplicar zoom ou aumentar o espaço entre os dedos para reduzir o zoom. |
| Ponteiro do mouse | Clicar com o botão esquerdo do mouse           | Tocar com um dedo                                                          |
| Ponteiro do mouse | Clicar com o botão do mouse esquerdo e arrastar  | Dar um toque duplo com um dedo e segurar, depois arrastar                               |
| Ponteiro do mouse | Clicar com o botão direito do mouse          | Tocar com dois dedos                                                          |
| Ponteiro do mouse | Clicar com o botão direito do mouse e arrastar | Dar um toque duplo com dois dedos e segurar, depois arrastar                               |
| Ponteiro do mouse | Botão de rolagem do mouse          | Tocar com dois dedos e segurar, depois arrastar para cima ou baixo                           |

> [!TIP]
> Perguntas e comentários são sempre bem-vindos. No entanto, NÃO poste uma solicitação de ajuda com solução de problemas usando o recurso de comentários no final deste artigo. Em vez disso, vá para o [Fórum de cliente da Área de Trabalho Remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicie um novo thread. Tem alguma sugestão de recurso? Conte-nos no [Fórum de voz do usuário cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
