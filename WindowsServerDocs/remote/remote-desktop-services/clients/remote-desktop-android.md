---
title: Comece com a área de trabalho remota no Android
description: Basic etapas de configuração para o cliente de área de trabalho remota para Android.
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446743"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Comece com a área de trabalho remota no Android

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de área de trabalho remota para o Android para trabalhar com áreas de trabalho e os aplicativos do Windows diretamente do seu dispositivo Android.

Use as seguintes informações para começar a usar. Não se esqueça de conferir o [perguntas frequentes sobre](remote-desktop-client-faq.md) se você tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre novas versões para o cliente do Android? Fazer check-out [o que há de novo para a área de trabalho remota no Android?](android-whatsnew.md)
> Você pode executar o cliente no Android 4.1 e dispositivos mais recentes, bem como os Chromebooks com 53 ChromeOS instalado. Saiba mais sobre aplicativos Android no Chrome [aqui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Obter o cliente de área de trabalho remota e começar a usá-lo

Siga estas etapas para começar com a área de trabalho remota em seu dispositivo Android:

1. Baixe o cliente de área de trabalho remota do [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de área de trabalho remota ou um recurso remoto. Usar uma conexão para se conectar diretamente a um computador de Windows e um recurso remoto para usar um programa RemoteApp, a área de trabalho baseada em sessão ou área de trabalho virtual publicado no local. 
4. Crie um widget para que você possa começar rapidamente a área de trabalho remota.

> [!NOTE]
> Se você gostaria de novos recursos do flight anteriormente, é recomendável baixar nosso [Beta de área de trabalho remota Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) aplicativo da Google Play store. 

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de área de trabalho remota

Para criar uma conexão de área de trabalho remota:

1. No tap Centro de Conexão **+** e, em seguida, toque em **Desktop**.
2. Insira as seguintes informações para o computador que você deseja se conectar:
   - **Nome do PC** – o nome do computador. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode anexar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nome de usuário** – o nome de usuário a ser usada para acessar o computador remoto. Você pode usar os seguintes formatos: *user_name*, *domain\user_name*, ou <em>user_name@domain.com</em>. Você também pode especificar se é solicitar um nome de usuário e senha.
3. Você também pode definir as seguintes opções adicionais:
   - **Nome amigável** – um nome fácil de lembrar para o PC que você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador é exibido.
   - **Gateway** – gateway de área de trabalho remota a que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
    Você precisa configurar um Gateway de área de trabalho remota?
   - **Som** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, o dispositivo remoto, ou não de forma alguma.
   - **Personalizar a resolução de vídeo** -definir uma resolução personalizada para uma conexão ao habilitar essa configuração. Quando desativar a resolução é aplicado que você definiu nas configurações globais do aplicativo.
   - **Trocar os botões do mouse** – Use essa opção, a troca de funções do botão esquerdo do mouse para o botão direito do mouse. (Isso é especialmente útil se o computador remoto está configurado para um usuário canhoto, mas usar um mouse destro.)
   - **Conectar-se à sessão de administrador** -Use essa opção para se conectar a uma sessão de console para administrar um servidor Windows.
   - **Redirecionar para o armazenamento local** – seu armazenamento local é montado como um sistema de arquivos remoto no PC remoto.
4. Toque **salvar**.

É necessário editar essas configurações? Toque no menu de estouro ( **...** ) ao lado do nome da área de trabalho e, em seguida, toque **editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro ( **...** ) e, em seguida, toque **remover**.

>[!TIP]
> Se você receber o erro 0xf07 uma senha incorreta ("não foi possível conectar ao PC remoto porque a senha associada com a conta de usuário expirou"), alterar sua senha e tente novamente.

### <a name="add-a-remote-resource"></a>Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas usando conexões de RemoteApp e área de trabalho.

Para adicionar um recurso remoto:

1. Na tela do Centro de Conexão, toque **+** e, em seguida, toque em **Feed de recursos remotos**. 
2. Insira informações para o recurso remoto:
   - **URL ou email** -a URL do servidor de acesso via Web RD. Você também pode inserir sua conta de email corporativo nesse campo – isso informa ao cliente para procurar o servidor de acesso da Web de área de trabalho remota associado com seu endereço de email.
   - **Nome de usuário** -o nome de usuário a ser usado para o servidor de acesso via Web RD que você está se conectando.
   - **Senha** -a senha a ser usado para o servidor de acesso via Web RD que você está se conectando.
3. Toque **salvar**.

Os recursos remotos serão exibidos no centro da Conexão.


Para excluir os recursos remotos:

1. No Centro de Conexão, toque no menu de estouro ( **...** ) ao lado do recurso remoto.
2. Toque **remover**.
3. Confirme a exclusão.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Widgets – uma área de trabalho salva em sua página inicial de Pin

Os aplicativos de área de trabalho remota dão suporte à fixação conexões com sua tela inicial usando o recurso de widget Android. A maneira que você adicionar um widget depende do tipo de dispositivo Android que você está usando e seu sistema operacional. Aqui está a maneira mais comum para adicionar um widget: 

1. Toque **aplicativos** para iniciar o menu de aplicativos.
2. Toque **widgets**.
3. Passe o dedo por meio de widgets e procure o ícone de área de trabalho remota com a descrição, "Pin área de trabalho remota."
4. Pressionado widget Área de trabalho remota e mova-o para a tela inicial.
5. Quando você soltar o ícone, você verá as áreas de trabalho remotas salvas. Escolha a conexão que você deseja salvar em sua tela inicial.

Agora você pode iniciar a conexão de área de trabalho remota diretamente da sua tela inicial, tocando-lo.

> [!NOTE]
> Se você renomear a conexão de área de trabalho no aplicativo de área de trabalho remota, o rótulo dessa área de trabalho remota fixado não será atualizada.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de área de trabalho remota para acessar os ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de área de trabalho remota.

Para configurar um novo gateway:

1. No Centro de Conexão, toque em **Configurações > Gateways**. Toque **+** para adicionar um novo gateway.
2. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** -o nome de usuário e a senha a ser usada para o Gateway de área de trabalho remota estiver se conectando a. Você também pode selecionar **usar conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de área de trabalho remota.

## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conectar a um recurso de área de trabalho ou remoto, você pode salvar as contas de usuário para selecionar de novamente. Você também pode definir contas de usuário no próprio cliente, em vez de salvar os dados de usuário quando você se conecta a uma área de trabalho.

Para criar uma nova conta de usuário:

1. No Centro de Conexão, toque em **as configurações**e, em seguida, toque em **contas de usuário**.
2. Toque **+** para adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** -o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: domínio \ nome_de_usuário, user_name ou user_name@domain.com.
   - **Senha** -a senha para o usuário especificado. Cada conta de usuário que você deseja salvar para usar para conexões remotas precisa ter uma senha associada a ele.
4. Toque **salvar**.

Para excluir uma conta de usuário:

1. No Centro de Conexão, toque em **Configurações > contas de usuário**.
2. Toque e segure a uma conta de usuário na lista para selecioná-lo. Você pode selecionar vários usuários.
3. Toque na Lixeira para excluir o usuário selecionado.

## <a name="navigate-the-remote-desktop-session"></a>Navegue a sessão de área de trabalho remota
Quando você inicia uma conexão de área de trabalho remota, existem ferramentas disponíveis que você pode usar para navegar a sessão.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma conexão de área de trabalho remota

1. Toque a conexão de área de trabalho remota para iniciar a sessão. 
2. Se você for solicitado a verificar o certificado para a área de trabalho remota, toque em **Connect**. Você também pode selecionar **não me pergunte novamente sobre conexões com este computador** por sempre aceitar o certificado.

### <a name="manage-global-app-settings"></a>Gerenciar configurações de aplicativo global

Você pode definir as seguintes configurações globais no seu cliente do Android:

- **Mostrar área de trabalho visualizações** -permite que você veja uma visualização de uma área de trabalho no centro da Conexão antes de se conectar a ele. Por padrão, isso é definido como **em**.
- **Aperte zoom** -permite que você use gestos de pinçar para ampliar. Se o aplicativo que você está usando, por meio da área de trabalho remota oferece suporte a multitoque (introduzido no Windows 8), desative essa configuração **desativar**.
- **Ajudam a melhorar a área de trabalho remota** -envia dados anônimos à Microsoft. Usamos esses dados para melhorar o cliente. Você pode saber mais sobre como tratamos esses dados anônimos, privados, consulte o [declaração de privacidade de cliente de área de trabalho remota](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Por padrão, essa configuração é **em**.
- **Exibir** -há duas configurações globais para a exibição:
  - **Orientação** -define a orientação preferida (paisagem ou retrato) para a sua sessão. 
    >[!NOTE]
    > Se você se conectar a um computador executando o Windows 8 ou uma versão anterior do Windows, a sessão não dimensionadas corretamente. Sua melhor aposta é desconectar do computador e, em seguida, reconectar-se na orientação que você deseja usar. Uma opção ainda melhor é atualizar o PC pelo menos Windows 8.1.

  - **Resolução** -define a resolução que você deseja usar para conexões de área de trabalho global. Se você já tiver definido uma resolução personalizada para um aplicativo individual ou a conexão, essa configuração não mudará que.
    >[!NOTE]
    >Quando você altera uma das configurações de exibição, elas se aplicam apenas a novas conexões desse ponto em. Para ver a alteração em uma sessão já está conectado para desconectar e, em seguida, conectar-se novamente.

### <a name="connection-bar"></a>Barra de Conexão

A conexão barra lhe dê que acesso para controles de navegação adicionais. Por padrão, a barra de conexão é colocada no meio na parte superior da tela. Toque duas vezes e arrastar a barra para a esquerda ou direita para movê-lo.

- **Controle de panorâmica**: O controle de panorâmica permite que a tela ser ampliados e movidos. Observe que o controle panorâmico só está disponível usando o toque direto.
   - Habilitar / desabilitar o controle de Panorâmica: Toque no ícone de bandeja na barra de conexão para exibir o controle de Panorâmica e zoom a tela. Toque no ícone de bandeja na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Use o controle de Panorâmica: Toque e mantenha o controle de Panorâmica e, em seguida, arraste na direção em que você deseja mover a tela.
   - Mova o controle de Panorâmica: Duplo toque e segure o controle panorâmico para mover o controle na tela.
- **Opções adicionais**: Toque no ícone de opções adicionais para exibir a seleção de sessão da barra e comando da barra (veja abaixo).
- **Teclado**: Toque no ícone de teclado para exibir ou ocultar o teclado. O controle panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mova a barra de conexão**: Toque e mantenha a barra de conexão e, em seguida, arraste e solte para um novo local na parte superior da tela.


### <a name="command-bar"></a>Barra de comandos

Toque na barra de conexão para exibir a barra de comandos no lado direito da tela. Você pode alternar entre os modos de mouse (toque direto e ponteiro do Mouse). Use o botão página inicial para retornar para o Centro de conexão na barra de comandos. Como alternativa, você pode usar o botão Voltar para a mesma ação. Sua sessão ativa não será desconectada. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso direto de toque gestos e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> Interação com o Windows 8 ou mais recente de gestos de toque nativos têm suporte no modo de toque direto. 

| Modo de mouse    | Operação de mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Com o botão esquerdo           | toque de 1 dedo                                                          |
| Toque direto  | Clique com o botão direito do mouse          | 1 de toque de dedo e manter pressionado                                                 |
| Ponteiro do mouse | Zoom                 | Use os 2 dedos e pinçar para ampliar ou mover entre os dedos para diminuir o zoom. |
| Ponteiro do mouse | Com o botão esquerdo           | toque de 1 dedo                                                          |
| Ponteiro do mouse | Clicar com o botão esquerdo e arrastar  | toque duplo de 1 dedo e manter pressionado, arraste                               |
| Ponteiro do mouse | Clique com o botão direito do mouse          | toque de dedo 2                                                          |
| Ponteiro do mouse | Clicar com o botão direito e arrastar | 2 toque duplo de dedo e manter pressionado, arraste                               |
| Ponteiro do mouse | Roda do mouse          | 2 dedo toque e mantenha pressionado e arrastar para cima ou para baixo                           |

> [!TIP]
> Perguntas e comentários são sempre bem-vindas. No entanto, não poste uma solicitação de ajuda de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum de cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo thread. Tem alguma sugestão de recurso? Conte-na [Fórum de voz do usuário cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
