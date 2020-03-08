---
title: Introdução ao cliente para Android
description: Informações gerais sobre o cliente Android.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 12/02/2019
ms.localizationpriority: medium
ms.openlocfilehash: cb0fa7009c4a8fe53810f8b5f058e659c2391dc1
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370663"
---
# <a name="get-started-with-the-android-client"></a>Introdução ao cliente para Android

>Aplica-se a: Android 4.1 e posterior

É possível usar o cliente de Área de Trabalho Remota para o Android para trabalhar com áreas de trabalho e aplicativos do Windows diretamente do seu dispositivo Android ou do Chromebook que dá suporte à Google Play Store.

Use as informações a seguir para começar. Não se esqueça de conferir as [Perguntas Frequentes](remote-desktop-client-faq.md) se tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre novas versões para o cliente Android? Confira [Novidades do cliente Android](android-whatsnew.md).
> - O cliente Android dá suporte a dispositivos que executam o Android 4.1 e posterior, bem como Chromebooks com ChromeOS 53 e posterior. Saiba mais sobre aplicativos Android no Chrome [aqui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="set-up-the-remote-desktop-client-for-android"></a>Configurar o cliente de Área de Trabalho Remota para Android

### <a name="download-the-remote-desktop-client-from-the-google-play-store"></a>Baixe o cliente da Área de Trabalho Remota da Google Play Store

Veja como configurar o cliente da Área de Trabalho Remota no dispositivo Android:

1. Baixe o cliente da Área de Trabalho Remota da Microsoft do [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android).
2. Inicie o **cliente da RD** na sua lista de aplicativos.
3. Adicione uma [conexão de Área de Trabalho Remota](#add-a-remote-desktop-connection) ou [recursos remotos](#add-remote-resources). Use uma conexão para se conectar diretamente a um computador Windows e a recursos remotos para acessar aplicativos e áreas de trabalho publicados para você por um administrador.

> [!NOTE]
> Se você deseja testar novos recursos antes de eles serem lançados, é recomendável baixar o cliente [Beta da Área de Trabalho Remota da Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) da Google Play Store.

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de Área de Trabalho Remota

Caso ainda não tenha feito, [configure seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).

Para criar uma conexão de Área de Trabalho Remota:

1. Na Central de Conexão, toque em **+** e, em seguida, em **Área de Trabalho**.
2. Insira o nome do computador remoto em **Nome do computador**. Pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Também é possível acrescentar informações de porta ao nome do computador (por exemplo, MyDesktop:3389 ou 10.0.0.1:3389). Esse é o único campo obrigatório.
3. Selecione o **Nome de usuário** usado para acessar o computador remoto.
   - Selecione **Entrar toda vez** para que o cliente solicite suas credenciais sempre que você se conectar ao computador remoto.
   - Selecione **Adicionar conta de usuário** para salvar uma conta que você usa com frequência para que você não precise inserir as credenciais sempre que entrar. Confira [gerencie suas contas de usuário](#manage-your-user-accounts) para obter mais detalhes.
4. Você também pode tocar em **Mostrar opções adicionais** para definir os seguintes parâmetros opcionais:
   - Em **Nome amigável**, você pode inserir um nome fácil de lembrar para o computador ao qual você está se conectando. Se você não especificar um nome amigável, o nome do computador será exibido.
   - **Gateway** é o gateway de Área de Trabalho Remota que você usará para se conectar a um computador por meio de uma rede externa. Para obter mais informações, entre em contato com o administrador do sistema.
   - **Som** seleciona o dispositivo que sua sessão remota usa para áudio. Você pode optar por reproduzir som no dispositivo local, no dispositivo remoto ou por não reproduzir som.
   - **Personalizar resolução de exibição** define a resolução da sessão remota. Quando desativada, a resolução especificada nas configurações globais é usada.
   - **Trocar os botões do mouse** alterna os comandos enviados pelos gestos direito e esquerdo do mouse. Ideal para usuários canhotos.
   - **Conectar-se à sessão de administrador** permite que você se conecte a uma sessão de administrador no computador remoto.
   - **Redirecionar armazenamento local** permite o redirecionamento de armazenamento local. Por padrão, essa configuração é desabilitada.
5. Quando terminar, toque em **Salvar**.

É necessário editar essas configurações? Toque no menu **Mais opções** ( **…** ) ao lado do nome da área de trabalho e, em seguida, toque em **Editar**.

Deseja remover a conexão? Novamente, toque no menu **Mais opções** ( **…** ) e, em seguida, toque em **Remover**.

>[!TIP]
> Se você receber o erro 0xf07 por uma senha incorreta ("Não foi possível conectar ao PC remoto porque a senha associada com a conta de usuário expirou"), altere sua senha e tente novamente.

### <a name="add-remote-resources"></a>Adicionar recursos remotos

Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas por seu administrador. O cliente Android dá suporte a recursos publicados de implantações de **Serviços de Área de Trabalho Remota** e **Área de Trabalho Virtual do Windows**. Para adicionar recursos remotos:

1. Na Central de Conexão, toque em **+** e, em seguida, toque em **Feed de Recursos Remotos**.
2. Insira a **URL do Feed**. Ela pode ser uma URL ou endereço de email:
   - A **URL** é o servidor de Acesso via Web da RD fornecida a você por seu administrador. Se estiver acessando recursos da Área de Trabalho Virtual do Windows, você poderá usar `https://rdweb.wvd.microsoft.com`.
   - Se você planeja usar **Email**, insira seu endereço de email nesse campo. Isso instrui o cliente a procurar um servidor de Acesso via Web da Área de Trabalho Remota associado ao endereço de email se ele foi configurado pelo administrador.
3. Toque em **Avançar**.
4. Forneça suas informações de entrada quando solicitado. Isso pode variar com base na implantação e pode incluir:
   - O **Nome de usuário** que tem permissão para acessar os recursos.
   - A **Senha** associada ao nome de usuário.
   - **Fator adicional**, que poderá ser solicitado se a autenticação tiver sido configurada dessa forma pelo administrador.
5. Quando terminar, toque em **Salvar**.

Os recursos remotos serão exibidos na Central de Conexão.

Para remover os recursos remotos:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do recurso remoto.
2. Toque em **Remover**.
3. Confirme a remoção.

### <a name="use-a-widget-to-pin-a-saved-desktop-to-your-home-screen"></a>Use um widget para fixar uma área de trabalho salva em sua tela inicial

O cliente da Área de Trabalho Remota dá suporte à fixação de conexões em sua tela inicial usando o recurso de widget do Android. A maneira em que você adiciona um widget depende do tipo de dispositivo Android que você está usando e do seu sistema operacional. Aqui está a maneira mais comum para adicionar um widget:

1. Toque em **Aplicativos** para iniciar o menu de aplicativos.
2. Toque em **Widgets**.
3. Passe o dedo pelos widgets e procure o ícone de Área de Trabalho Remota com a descrição: "Fixar Área de trabalho Remota".
4. Toque nesse widget de Área de Trabalho Remota e segure, movendo-o em seguida para a tela inicial.
5. Quando soltar o ícone, você verá as áreas de trabalho remotas salvas. Escolha a conexão que você deseja salvar em sua tela inicial.

Agora você pode iniciar a conexão de área de trabalho remota tocando nela diretamente da sua tela inicial.

> [!NOTE]
> Se você renomear a conexão de área de trabalho no cliente da Área de Trabalho Remota, seu rótulo fixado não será atualizado.

## <a name="manage-general-app-settings"></a>Gerenciar configurações gerais do aplicativo

Para alterar as configurações gerais do aplicativo, acesse Central de Conexão, toque em **Configurações** e então toque em **Geral**.

É possível definir as seguintes configurações gerais:

- **Mostrar versões prévias da área de trabalho** permite que você veja uma versão prévia de uma área de trabalho na Central de Conexão antes de se conectar a ela. Essa configuração é habilitada por padrão.
- **Pinçar para aplicar zoom na seção remota** permite que você use gestos de pinçar para aplicar zoom. Se o aplicativo que você estiver usando por meio da Área de Trabalho Remota der suporte a multitoque (introduzido no Windows 8), desabilite esse recurso.
- Habilite **Usar entrada de código de verificação quando disponível** se seu aplicativo remoto não responder devidamente à entrada do teclado enviada como código de verificação. A entrada é enviada como Unicode quando desabilitada.
- **Ajude a melhorar Área de Trabalho Remota** envia dados anônimos sobre como você usa a Área de Trabalho Remota para Android para a Microsoft. Usamos esses dados para melhorar o cliente. Para saber mais sobre nossa política de privacidade e os tipos de dados que coletamos, confira a [Declaração de Privacidade da Microsoft](https://privacy.microsoft.com/privacystatement). Essa configuração é habilitada por padrão.

## <a name="manage-display-settings"></a>Gerenciar configurações de exibição

Para alterar as configurações de exibição, toque em **Configurações** e, em seguida, toque em **Exibir** na Central de Conexão.

É possível definir as seguintes configurações de exibição:

- **Orientação** define a orientação preferida (paisagem ou retrato) para a sua sessão.
  
  >[!NOTE]
  > Se você se conectar a um computador executando o Windows 8 ou anterior, a sessão não será dimensionada corretamente se a orientação do dispositivo for alterada. Para que o cliente seja dimensionado corretamente, desconecte-se do computador e reconecte-se na orientação que você deseja usar. Você também pode garantir a escala correta usando um computador com o Windows 10.

- **Resolução** define a resolução remota que você deseja usar para conexões de área de trabalho globalmente. Se você já tiver definido uma resolução personalizada para uma conexão individual, essa configuração não a mudará.
  
  >[!NOTE]
  >Qualquer alteração às configurações de exibição aplicam-se somente a novas conexões. Para aplicar suas alterações à sessão à qual você está conectado no momento, atualize sua sessão desconectando e reconectando.

## <a name="manage-your-rd-gateways"></a>Gerenciar seus Gateways de RD

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede privada de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de Área de Trabalho Remota.

Para configurar um novo Gateway de Área de Trabalho Remota:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Gateways**.
2. Toque em **+** para adicionar um novo gateway.
3. Insira as seguintes informações:
   - Insira o nome do computador que você deseja usar como um gateway em **Nome do servidor**. Pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: RDGateway:443 ou 10.0.0.1:443).
   - Selecione a **Conta de Usuário** que você usará para acessar o Gateway de Área de Trabalho Remota.
     - Selecione **Usar a conta de usuário de área de trabalho** para usar as mesmas credenciais especificadas para o computador remoto.
     - Selecione **Adicionar conta de usuário** para salvar uma conta que você usa com frequência para que você não precise inserir as credenciais sempre que entrar. Para obter mais informações, confira [Gerenciar suas contas de usuário](#manage-your-user-accounts).

Para excluir um Gateway de Área de Trabalho Remota:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Gateways**.
2. Toque em um gateway na lista e segure para selecioná-la. É possível selecionar vários gateways de uma vez.
3. Toque na lixeira para excluir o gateway selecionado.

## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Você pode salvar contas de usuário para usar sempre que você se conectar a uma área de trabalho remota ou a recursos remotos.

Para salvar uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de usuário**.
2. Toque em **+** para adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - O **Nome de usuário** a salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_de_usuário, domínio\nome_de_usuário ou user_name@domain.com.
   - A **Senha** para o usuário que você especificou. Todas as contas de usuário que você deseja salvar para uso com conexões remotas precisam ter uma senha associada a elas.
4. Quando terminar, toque em **Salvar**.

Para excluir uma conta de usuário salva:

1. Na Central de Conexão, toque em **Configurações** e, em seguida, toque em **Contas de usuário**.
2. Toque em uma conta de usuário na lista e segure para selecioná-la. Você pode selecionar vários usuários ao mesmo tempo.
3. Toque na lixeira para excluir o usuário selecionado.

## <a name="navigate-the-remote-desktop-session"></a>Navegar pela sessão de Área de Trabalho Remota

Aqui está uma breve introdução a como abrir e navegar na sessão de Área de Trabalho Remota.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma conexão de Área de Trabalho Remota

1. Toque **no nome da conexão de Área de Trabalho Remota** para iniciar a sessão.
2. Se for solicitado que você verifique o certificado da área de trabalho remota, toque em **Conectar**. Também é possível selecionar **Não me pergunte novamente para conexões com este computador** para aceitar o certificado sempre por padrão.

### <a name="connection-bar"></a>Barra de conexão

A barra de conexão lhe dá acesso a controles de navegação adicionais. Por padrão, a barra de conexão é colocada no meio da parte superior da tela. Arraste a barra para a esquerda ou para a direita para movê-la.

- **Controle de movimento panorâmico**: o controle de movimento panorâmico permite que a tela seja ampliada e movida. O controle de movimento panorâmico só está disponível para toque direto.
  - Para mostrar o controle de movimento panorâmico, toque no ícone de movimento panorâmico na barra de conexão para exibir o controle de movimento panorâmico e aplicar zoom na tela. Toque no ícone de movimento panorâmico novamente para ocultar o controle e retornar a tela para seu tamanho original.
  - Para usar o controle de movimento panorâmico, toque e segure-o e arraste-o na direção desejada para mover a tela.
  - Para mover o controle de movimento panorâmico, toque duas vezes e segure-o para mover o controle ao redor da tela.
- **Opções adicionais**: toque no ícone de opções adicionais para exibir a barra de seleção de sessão e a barra de comandos.
- **Teclado**: toque no ícone de teclado para exibir ou ocultar o teclado. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.

### <a name="session-selection-bar"></a>Barra de seleção de sessão

Você pode ter várias conexões abertas em computadores diferentes ao mesmo tempo. Toque na barra de conexão para exibir a barra de seleção de sessão no lado esquerdo da tela. A barra de seleção de sessão permite exibir suas conexões abertas e alternar entre elas.

Quando está conectado a recursos remotos, você pode alternar entre os aplicativos dentro dessa sessão tocando no menu do expansor ( **>** ) e escolhendo na lista de itens disponíveis.

Para iniciar uma nova sessão em sua conexão atual, toque em **Iniciar Novo** e escolha na lista de itens disponíveis.

Para desconectar uma sessão, toque em **X** no lado direito do bloco da sessão.

### <a name="command-bar"></a>Barra de comandos

Toque na barra de conexão para exibir a barra de comandos no lado direito da tela. Na barra de comandos, é possível alterar entre modos de mouse (toque direto e ponteiro do mouse) ou tocar no botão Página Inicial para retornar à Central de Conexão. Também é possível tocar no botão Voltar para retornar à Central de Conexão. Retornar à Central de Conexão não desconectará sua sessão ativa.

### <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usar gestos de toque e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. A tabela a seguir explica quais gestos correspondem a quais ações do mouse em cada modo de mouse.

> [!NOTE]
> Há suporte para gestos de toque nativos no modo Toque Direto no Windows 8 ou posterior.

| Modo do mouse    | Ação do mouse         | Gesto                                                                 |
|---------------|----------------------|-------------------------------------------------------------------------|
| Toque direto  | Clique com o botão esquerdo do mouse           | Toque com um dedo                                                     |
| Toque direto  | Clique com o botão direito em          | Tocar com um dedo e segurar e soltar                              |
| Ponteiro do mouse | Zoom                 | Usar dois dedos e pinçar para reduzir o zoom ou aumentar o espaço entre os dedos para ampliar. |
| Ponteiro do mouse | Clique com o botão esquerdo do mouse           | Toque com um dedo                                                     |
| Ponteiro do mouse | Clique com o botão esquerdo do mouse e arraste  | Tocar duas vezes e segurar com um dedo e arrastar                          |
| Ponteiro do mouse | Clique com o botão direito em          | Tocar com dois dedos                                                    |
| Ponteiro do mouse | Clicar com o botão direito do mouse e arrastar | Tocar duas vezes e segurar com dois dedos e arrastar                         |
| Ponteiro do mouse | Botão de rolagem do mouse          | Fechar e abrir dedos indicador e polegar e manter com dois dedos e arrastar para cima ou para baixo                     |
