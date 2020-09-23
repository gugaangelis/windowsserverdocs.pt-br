---
title: Introdução ao cliente para Microsoft Store
description: Instruções básicas de configuração para o cliente de Área de Trabalho Remota da Microsoft Store.
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 09/17/2020
ms.localizationpriority: medium
ms.openlocfilehash: 07e778fef6a944107ab9cb66f06c8c5d980f3675
ms.sourcegitcommit: 877d6db73d9520e3a23738d6528016235493cff3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90779250"
---
# <a name="get-started-with-the-microsoft-store-client"></a>Introdução ao cliente para Microsoft Store

>Aplica-se a: Windows 10

Você pode usar o cliente de Área de Trabalho Remota para o Windows para trabalhar remotamente com PCs e aplicativos do Windows usando outro dispositivo Windows.

Use as informações a seguir para começar. Não se esqueça de conferir as [Perguntas Frequentes](remote-desktop-client-faq.md) se tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre as novas versões para o Cliente da Microsoft Store? Confira as [Novidades do Cliente da Microsoft Store](windows-whatsnew.md)
> - É possível executar o cliente em qualquer versão do Windows 10 com suporte.

## <a name="get-the-rd-client-and-start-using-it"></a>Obter o cliente de Área de Trabalho Remota e começar a usá-lo

Siga estas etapas para começar a usar a Área de Trabalho Remota em seu dispositivo Windows 10:

1. Baixar o cliente da Área de Trabalho Remota da [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps).
2. [Configure seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de PC remoto ou um workspace. Você usa uma conexão para se conectar diretamente a um PC Windows e um workspace para usar um programa RemoteApp, a área de trabalho baseada em sessão ou a área de trabalho virtual publicada por seu administrador.
4. Fixe os itens para que você possa acessar a Área de Trabalho Remota rapidamente.

### <a name="add-a-remote-pc-connection"></a>Adicionar uma conexão de PC remoto

Para criar uma conexão de PC remoto:

1. Na Central de Conexão, toque em **+ Adicionar** e em **PCs**.
2. Insira as informações a seguir para o computador com o qual você deseja se conectar:
   - **Nome do PC** – o nome do computador. O nome do computador pode ser um nome de computador Windows, um nome de domínio da Internet ou um endereço IP. Você também pode acrescentar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Conta de usuário** – a conta de usuário a ser usada para acessar o computador remoto. Toque em **+** para adicionar uma nova conta ou selecionar uma conta existente. Você pode usar os seguintes formatos para o nome de usuário: *nome_de_usuário*, *domínio\nome_de_usuário* ou <em>user_name@domain.com</em>. Especifique também se deseja solicitar as credenciais durante a conexão selecionando **Sempre perguntar**.
3. Também é possível definir opções adicionais tocando em **Mostrar mais**:
   - **Nome de exibição**: um nome fácil de lembrar para o computador ao qual você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se não especificar um nome amigável, o nome do computador será exibido.
   - **Grupo** – especifique um grupo para que seja mais fácil encontrar suas conexões mais tarde. Você pode adicionar um novo grupo tocando em **+** ou pode selecionar um na lista.
   - **Gateway** – o gateway de PC Remoto que você deseja usar para se conectar a PCs, programas RemoteApp e PCs baseados em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
   - **Conectar-se à sessão de administrador** – use essa opção para se conectar a uma sessão de console para administrar um servidor Windows.
   - **Trocar os botões do mouse** – use essa opção para trocar as funções dos botões esquerdo do mouse por aquelas do botão direito do mouse. A troca de botões do mouse é necessária quando você usa um computador configurado para um usuário canhoto, mas tem apenas um mouse para destros.
   - **Definir a resolução da sessão remota como:** – selecione a resolução que deseja usar na sessão. **Escolha para mim** definirá a resolução com base no tamanho do cliente.
   - **Altere o tamanho da exibição:** ao selecionar uma alta resolução estática para a sessão, use essa configuração para fazer com que os itens na tela pareçam maiores a fim de aprimorar a legibilidade. Essa configuração se aplica somente à conexão com o Windows 8.1 ou superior.
   - **Atualizar a resolução da sessão remota no redimensionamento** – quando habilitado, o cliente atualizará a resolução da sessão dinamicamente com base no tamanho do cliente. Essa configuração se aplica somente à conexão com o Windows 8.1 ou superior.
   - **Área de transferência** – quando habilitada, permite copiar texto e imagens para/do PC remoto.
   - **Reprodução de áudio** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, no computador remoto ou por não reproduzir de forma nenhuma.
   - **Gravação de áudio** – quando habilitada, permite usar um microfone local com aplicativos no computador remoto.
4. Toque em **Salvar**.

É necessário editar essas configurações? Toque no menu de estouro ( **...** ) ao lado do nome do PC e, em seguida, toque em **Editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro ( **...** ) e, em seguida, toque em **Remover**.

### <a name="add-a-workspace"></a>Adicionar um workspace

Workspaces são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas por seu administrador usando os Serviços de Área de Trabalho Remota.

Para adicionar um workspace:

1. Na tela da Central de Conexão, toque em **+ Adicionar** e, em seguida, toque em **Workspaces**.
2. Insira a **URL do Feed** fornecida por seu administrador e toque em **Encontrar feeds**.
3. Quando precisar, forneça as credenciais para assinar o feed.

Os workspaces serão exibidos na Central de Conexão.

Para excluir workspaces:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do workspace.
2. Toque em **Remover**.

### <a name="pin-a-saved-pc-to-your-start-menu"></a>Fixar um PC salvo no menu Iniciar

Para fixar uma conexão no menu Iniciar, toque no menu de estouro ( **...** ) ao lado do nome do PC e, em seguida, toque em **Fixar em Iniciar**.

Agora, você pode iniciar a conexão de PC tocando nela diretamente no menu Iniciar.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de Área de Trabalho Remota para acessar os ativos internos

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de Área de Trabalho Remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações**.
2. Ao lado de Gateway, toque em **+** para adicionar um novo gateway.

      >[!NOTE]
      >Adicione também um gateway ao adicionar uma nova conexão.

3. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. O nome do servidor pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Conta de usuário**: selecione ou adicione uma conta de usuário a ser usada com o Gateway de PC Remoto ao qual você está se conectando. Selecione também a opção **Usar conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de PC Remoto.
4. Toque em **Salvar**.

## <a name="global-app-settings"></a>Configurações globais do aplicativo

Você pode definir as seguintes configurações globais em seu cliente tocando em **Configurações**:

### <a name="managed-items"></a>Itens gerenciados

- **Conta de usuário**: permite adicionar, editar e excluir as contas de usuário salvas no cliente. Atualize também a senha para uma conta após a alteração dela.
- **Gateway**: permite adicionar, editar e excluir os servidores de gateway salvos no cliente.
- **Grupo**: permite adicionar, editar e excluir os grupos salvos no cliente. Agrupe também as conexões aqui.

### <a name="session-settings"></a>Configurações da sessão

- **Iniciar conexões em tela inteira** – quando habilitado, sempre que uma conexão é iniciada, o cliente usa toda a tela do monitor atual.
- **Iniciar cada conexão em uma nova janela** – quando habilitado, cada conexão é iniciada em uma janela separada, permitindo que você as coloque em monitores diferentes e alterne entre elas usando a barra de tarefas.
- **Ao redimensionar o aplicativo:** – permite controlar o que acontece quando a janela do cliente é redimensionada. O padrão é **Alongar o conteúdo, preservando a taxa de proporção**.
- **Usar comandos de teclado com:** permite especificar o local em que os comandos de teclado, como *WIN* ou *ALT + TAB*, são usados. O padrão é enviá-los à sessão apenas quando a conexão está em tela inteira.
- **Impedir que a tela atinja o tempo limite** – impede que a tela atinja o tempo limite quando uma sessão está ativa. Prevenir o tempo limite é útil quando a conexão não precisa de interação por longos períodos.

### <a name="app-settings"></a>Configurações de aplicativo

- **Mostrar versões prévias do PC**: permite que você veja uma versão prévia de um PC na Central de Conexão antes de se conectar a ela. Esta configuração está ativada por padrão.
- **Ajudar a melhorar a Área de Trabalho Remota** – envia dados anônimos à Microsoft. Usamos esses dados para melhorar o cliente. Para saber mais sobre como tratamos esses dados anônimos e particulares, confira a [Política de Privacidade da Microsoft](https://privacy.microsoft.com/privacystatement). Esta configuração está ativada por padrão.

### <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Ao se conectar a um PC ou a um workspace, você pode salvar as informações da conta para se conectar a ela mais tarde. Defina também contas de usuário no cliente, em vez de salvar os dados de usuário ao se conectar a um PC.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, toque em **Configurações**.
2. Ao lado de Conta de usuário, toque em **+** para adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** – o nome do usuário a salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_de_usuário, domínio\nome_de_usuário ou user_name@domain.com.
   - **Senha** – a senha para o usuário que você especificou. Esse campo pode ser deixado em branco para solicitar uma senha durante a conexão.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações**.
2. Selecione a conta a ser excluída na lista sob Conta de usuário.
3. Ao lado de Conta de usuário, toque no ícone de edição.
4. Toque em **Remover esta conta** na parte inferior para excluir a conta de usuário.
5. Você também pode editar a conta de usuário e tocar em **Salvar**.

## <a name="navigate-your-remote-session"></a>Navegar em sua sessão remota

Esta seção descreve as ferramentas disponíveis para ajudar você a navegar em sua sessão remota depois de se conectar ao serviço.

### <a name="start-a-remote-session"></a>Iniciar uma sessão remota

1. Toque no nome da conexão que você deseja usar para iniciar a sessão.
2. Se você não salvar as credenciais da conexão, precisará fornecer um **Nome de usuário** e uma **Senha**.
3. Se precisar confirmar o certificado do seu workspace ou PC, examine as informações e verifique se você confia nesse PC antes de tocar em **Conectar**. Também é possível selecionar **Não pergunte sobre o certificado novamente** para sempre aceitar este certificado.

### <a name="connection-bar"></a>Barra de conexão

A barra de conexão lhe dá acesso a controles de navegação adicionais. Por padrão, a barra de conexão é colocada no meio da parte superior da tela. Toque na barra e arraste-a para a esquerda ou a direita para movê-la.

- **Controle de movimento panorâmico** – o controle de movimento panorâmico permite que a tela seja ampliada e movida. Observe que o controle de movimento panorâmico só está disponível em dispositivos habilitados para toque e que usam o modo de toque direto.
   - Para habilitar ou desabilitar o controle de movimento panorâmico, toque no ícone de movimento panorâmico na barra de conexão para exibir o controle de movimento panorâmico. A tela será ampliada enquanto o controle de movimento panorâmico estiver ativo. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Para usar o controle de movimento panorâmico, toque no controle de movimento panorâmico, segure-o e arraste-o na direção em que deseja mover a tela.
   - Para mover o controle de movimento panorâmico, dê um toque duplo no controle de movimento panorâmico e segure-o para mover o controle na tela.
- **Opções adicionais**: toque no ícone de opções adicionais para exibir a barra de seleção de sessão e a barra de comandos.
- **Teclado** – toque no ícone de teclado para exibir ou ocultar o teclado virtual. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.

### <a name="command-bar"></a>Barra de comandos

Toque nas **...** na barra de conexão para exibir a barra de comandos no lado direito da tela.

- **Início** – use o botão de Início para retornar à Central de Conexão da barra de comandos.
  - Use também o botão Voltar para realizar a mesma ação. Se você usar o botão Voltar, a sessão ativa não será desconectada, permitindo que você inicie conexões adicionais.
- **Desconectar**: use o botão Desconectar para se desconectar da sessão. Seus aplicativos permanecerão ativos, desde que a sessão ainda esteja ativa no computador remoto.
- **Tela inteira** – entra ou sai do modo de tela inteira.
- **Toque ou Mouse**: alterne entre os modos de mouse (toque direto e ponteiro do mouse).

### <a name="use-direct-touch-gestures-and-mouse-modes"></a>Usar gestos de toque direto e modos de mouse

Você pode interagir com sua sessão com dois modos de mouse disponíveis:

- **Toque direto**: passa todos os contatos de toque para a sessão para que sejam interpretados remotamente.
  - Usado da mesma forma que você usaria o Windows com uma tela sensível ao toque.
- **Ponteiro do mouse**: transforma a tela touch local em um grande touchpad, permitindo mover um ponteiro do mouse na sessão.
  - Usado da mesma forma que você usaria o Windows com um touchpad.

> [!NOTE]
> No Windows 8 ou posterior, há suporte para gestos de toque nativos no modo de Toque Direto.

| Modo do mouse    | Operação do mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Clique com o botão esquerdo do mouse           | Toque com um dedo                                                          |
| Toque direto  | Clique com o botão direito em          | Tocar e segurar com um dedo                                                |
| Ponteiro do mouse | Clique com o botão esquerdo do mouse           | Toque com um dedo                                                          |
| Ponteiro do mouse | Clique com o botão esquerdo do mouse e arraste  | Tocar duas vezes e segurar com um dedo e arrastar                               |
| Ponteiro do mouse | Clique com o botão direito em          | Tocar com dois dedos                                                          |
| Ponteiro do mouse | Clicar com o botão direito do mouse e arrastar | Tocar duas vezes e segurar com dois dedos e arrastar                              |
| Ponteiro do mouse | Botão de rolagem do mouse          | Fechar e abrir dedos indicador e polegar e manter com dois dedos e arrastar para cima ou para baixo                          |
| Ponteiro do mouse | Zoom                 | Com dois dedos, pinçar para reduzir a tela e abrir os dedos para ampliá-la |

## <a name="give-us-feedback"></a>Fornecer comentários

Tem uma sugestão de recursos ou deseja relatar um problema? Conte-nos usando o [Hub de Comentários](https://aka.ms/rdstorefeedback).

Você também pode nos fornecer comentários selecionando o botão de reticências ( **...** ) no aplicativo cliente, depois selecionando **Comentários**, conforme mostrado na imagem a seguir.

> [!div class="mx-imgBorder"]
> ![Uma captura de tela mostrando o botão de reticências realçado em vermelho. Um menu suspenso foi aberto sob o botão, e a opção "Comentários" também está realçada em vermelho.](../media/ellipsis-icon.png)

>[!NOTE]
>Para que possamos ajudar você da melhor maneira, precisamos que nos forneça informações com o máximo possível de detalhes sobre o problema. Por exemplo, você pode incluir capturas de tela ou uma gravação das ações que você realizou antes de encontrar o problema. Para obter mais dicas sobre como fornecer comentários úteis, confira [Comentários](/windows-insider/at-home/feedback#add-new-feedback).
