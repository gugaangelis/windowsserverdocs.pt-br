---
title: Introdução ao cliente para Microsoft Store
description: Etapas básicas de configuração para o cliente de Área de Trabalho Remota para Microsoft Store.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 64362c6f6da77ee15a95ddcbaf33c6cb9ecd5cf4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861429"
---
# <a name="get-started-with-the-windows-store-client"></a>Introdução ao cliente para Microsoft Store

>Aplica-se a: Windows 10

Você pode usar o cliente de Área de Trabalho Remota para o Windows para trabalhar remotamente com áreas de trabalho e aplicativos do Windows usando outro dispositivo Windows.

Use as informações a seguir para começar. Não se esqueça de conferir as [Perguntas Frequentes](remote-desktop-client-faq.md) se tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre as novas versões para o cliente da Windows Store? Confira as [Novidades do cliente da Windows Store](windows-whatsnew.md)
> - É possível executar o cliente em qualquer versão do Windows 10 com suporte.

## <a name="get-the-rd-client-and-start-using-it"></a>Obter o cliente de Área de Trabalho Remota e começar a usá-lo

Siga estas etapas para começar a usar a Área de Trabalho Remota em seu dispositivo Windows 10:

1. Baixar o cliente da Área de Trabalho Remota da [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configure seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de Área de Trabalho Remota ou um recurso remoto. Você usa uma conexão para se conectar diretamente a um computador Windows e um recurso remoto para usar um programa RemoteApp, área de trabalho baseada em sessão ou área de trabalho virtual publicada por seu administrador. 
4. Fixe os itens para que você possa acessar a Área de Trabalho Remota rapidamente.

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de Área de Trabalho Remota

Para criar uma conexão de Área de Trabalho Remota:

1. Na Central de Conexão, toque em **+Adicionar** e, em seguida, toque em **Área de Trabalho**.
2. Insira as informações a seguir para o computador com o qual você deseja se conectar:
   - **Nome do PC** – o nome do computador. Pode ser um nome do computador Windows, um nome de domínio da Internet ou um endereço IP. Você também pode acrescentar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Conta de usuário** – a conta de usuário a ser usada para acessar o computador remoto. Toque em **+** para adicionar uma nova conta ou selecionar uma conta existente. Você pode usar os seguintes formatos para o nome de usuário: *nome_de_usuário*, *domínio\nome_de_usuário* ou <em>user_name@domain.com</em>. Você também pode especificar se deseja solicitar um nome de usuário e senha durante a conexão selecionando **Perguntar sempre**.
3. Também é possível definir opções adicionais tocando em **Mostrar mais**:
   - **Nome de exibição** – um nome fácil de lembrar para o PC ao qual você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador será exibido.
   - **Grupo** – especifique um grupo para que seja mais fácil encontrar suas conexões mais tarde. Você pode adicionar um novo grupo tocando em **+** ou pode selecionar um na lista.
   - **Gateway** – o Gateway de Área de Trabalho Remota que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
   - **Conectar-se à sessão de administrador** – use essa opção para se conectar a uma sessão de console para administrar um servidor Windows.
   - **Trocar os botões do mouse** – use essa opção para trocar as funções dos botões esquerdo do mouse por aquelas do botão direito do mouse. (Isso é especialmente útil se o computador remoto está configurado para um usuário canhoto, mas você usa um mouse para destros.)
   - **Definir a resolução da sessão remota como:** – selecione a resolução que deseja usar na sessão. **Escolha para mim** definirá a resolução com base no tamanho do cliente.
   - **Altere o tamanho da exibição:** – ao selecionar uma alta resolução estática para a sessão, você tem a opção de fazer com que itens na tela pareçam maiores para melhorar a legibilidade. Observação: isso se aplica somente ao conectar-se ao Windows 8.1 ou superior.
   - **Atualizar a resolução da sessão remota no redimensionamento** – quando habilitado, o cliente atualizará a resolução da sessão dinamicamente com base no tamanho do cliente. Observação: isso se aplica somente ao conectar-se ao Windows 8.1 ou superior.
   - **Área de transferência** – quando habilitada, permite copiar texto e imagens para/do PC remoto.
   - **Reprodução de áudio** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, no computador remoto ou por não reproduzir de forma nenhuma.
   - **Gravação de áudio** – quando habilitada, permite usar um microfone local com aplicativos no computador remoto.
4. Toque em **Salvar**.

É necessário editar essas configurações? Toque no menu de estouro ( **...** ) ao lado do nome da área de trabalho e, em seguida, toque em **Editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro ( **...** ) e, em seguida, toque em **Remover**.

### <a name="add-a-remote-resource"></a>Adicionar um recurso remoto

Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas por seu administrador usando os Serviços de Área de Trabalho Remota.

Para adicionar um recurso remoto:

1. Na tela da Central de Conexão, toque em **+Adicionar** e, em seguida, toque em **Recursos remotos**.
2. Insira a **URL do Feed** fornecida por seu administrador e toque em **Encontrar feeds**.
3. Quando solicitado, forneça as credenciais a serem usadas para assinar o feed.

Os recursos remotos serão exibidos na Central de Conexão.

Para excluir os recursos remotos:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do recurso remoto.
2. Toque em **Remover**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Fixar uma área de trabalho salva ao menu Iniciar

Para fixar uma conexão ao menu Iniciar, toque no menu de estouro ( **...** ) ao lado do nome da área de trabalho e, em seguida, toque em **Fixar na Tela Inicial**.

Agora, você pode iniciar a conexão de área de trabalho remota tocando nela diretamente no menu Iniciar.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de Área de Trabalho Remota para acessar os ativos internos

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de Área de Trabalho Remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações**.
2. Ao lado de Gateway, toque em **+** para adicionar um novo gateway. Observação: também é possível adicionar um gateway ao adicionar uma nova conexão.
3. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Conta de usuário** – selecione ou adicione uma conta de usuário a ser usada com o Gateway de Área de Trabalho Remota a que está se conectando. Você também pode selecionar **Usar conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de área de trabalho remota.
4. Toque em **Salvar**.  

## <a name="global-app-settings"></a>Configurações globais do aplicativo

Você pode definir as seguintes configurações globais em seu cliente tocando em **Configurações**:

ITENS GERENCIADOS

- **Conta de usuário** – permite Adicionar, editar e excluir contas de usuário salvas no cliente. É uma boa maneira de atualizar a senha de uma conta após ela ter sido alterada.
- **Gateway** – permite Adicionar, editar e excluir servidores de gateway salvos no cliente.
- **Grupo** – permite Adicionar, editar e excluir grupos salvos no cliente. Permite agrupar conexões facilmente.

CONFIGURAÇÕES DE SESSÃO

- **Iniciar conexões em tela inteira** – quando habilitado, sempre que uma conexão é iniciada, o cliente usa toda a tela do monitor atual.
- **Iniciar cada conexão em uma nova janela** – quando habilitado, cada conexão é iniciada em uma janela separada, permitindo que você as coloque em monitores diferentes e alterne entre elas usando a barra de tarefas.
- **Ao redimensionar o aplicativo:** – permite controlar o que acontece quando a janela do cliente é redimensionada. O padrão é **Alongar o conteúdo, preservando a taxa de proporção**.
- **Usar comandos de teclado com:** – permite especificar onde comandos de teclado, como *WIN* ou *ALT + TAB*, são usados. O padrão é enviá-los à sessão apenas quando a conexão está em tela inteira.
- **Impedir que a tela atinja o tempo limite** – impede que a tela atinja o tempo limite quando uma sessão está ativa. É útil quando a conexão não exige interação por longos períodos.

CONFIGURAÇÕES DE APLICATIVO

- **Mostrar versão prévia da área de trabalho** – permite que você veja uma versão prévia de uma área de trabalho na Central de Conexão antes de se conectar a ela. Por padrão, isso é definido como **ativado**.
- **Ajudar a melhorar a Área de Trabalho Remota** – envia dados anônimos à Microsoft. Usamos esses dados para melhorar o cliente. Para saber mais sobre como tratamos esses dados anônimos e privados consultando a [Política de Privacidade da Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Por padrão, essa configuração fica **ativada**.

### <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conecta a uma área de trabalho ou a recursos remotos, você pode salvar as contas de usuário para selecioná-las novamente. Você também pode definir contas de usuário no cliente propriamente dito, em vez de salvar os dados de usuário quando você se conecta a uma área de trabalho.

Para criar uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações**.
2. Ao lado de Conta de usuário, toque em **+** para adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** – o nome do usuário a salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_de_usuário, domínio\nome_de_usuário ou user_name@domain.com.
   - **Senha** – a senha para o usuário que você especificou. Pode ser deixado em branco para solicitar uma senha durante a conexão.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações**.
2. Selecione a conta a ser excluída na lista sob Conta de usuário.
3. Ao lado de Conta de usuário, toque no ícone de edição.
4. Toque em **Remover esta conta** na parte inferior para excluir a conta de usuário.
5. Você também pode editar a conta de usuário e tocar em **Salvar**.

## <a name="navigate-the-remote-desktop-session"></a>Navegar pela sessão de Área de Trabalho Remota
Quando você inicia uma conexão de Área de Trabalho Remota, existem ferramentas disponíveis que você pode usar para navegar pela sessão.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma conexão de Área de Trabalho Remota

1. Toque na conexão de Área de Trabalho Remota para iniciar a sessão.
2. Se você ainda não salvou as credenciais para a conexão, será solicitado que forneça um **Nome de usuário** e uma **Senha**.
3. Se for solicitado que você verifique o certificado da área de trabalho remota, examine as informações para garantir que se trata de um computador confiável antes de tocar em **Conectar**. Também é possível selecionar **Não pergunte sobre o certificado novamente** para sempre aceitar este certificado.

### <a name="connection-bar"></a>Barra de Conexão

A barra de conexão lhe dá acesso a controles de navegação adicionais. Por padrão, a barra de conexão é colocada no meio da parte superior da tela. Toque na barra e arraste-a para a esquerda ou a direita para movê-la.

- **Controle de movimento panorâmico** – o controle de movimento panorâmico permite que a tela seja ampliada e movida. Observe que o controle de movimento panorâmico só está disponível em dispositivos sensíveis ao toque e usando o modo de toque direto.
   - Habilitar/desabilitar o controle de movimento panorâmico: toque no ícone de bandeja na barra de conexão para exibir o controle de movimento panorâmico e ampliar a tela. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Usar o controle de movimento panorâmico – toque no controle de movimento panorâmico e segure, depois arraste na direção em que você deseja mover a tela.
   - Mover o controle de movimento panorâmico – dê um toque duplo no controle de movimento panorâmico e segure para mover o controle na tela.
- **Opções adicionais** – toque no ícone de opções adicionais para exibir a barra de seleção de sessão e a barra de comandos (veja abaixo).
- **Teclado** – toque no ícone de teclado para exibir ou ocultar o teclado virtual. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.

### <a name="command-bar"></a>Barra de comandos

Toque em **...** na barra de conexão para exibir a barra de comandos no lado direito da tela.

- **Início** – use o botão de Início para retornar à Central de Conexão da barra de comandos.
  - Como alternativa, você pode usar o botão voltar para realizar a mesma ação.
  - Sua sessão ativa não será desconectada.
  - Isso permite iniciar conexões adicionais.
- **Desconectar** – use o botão Desconectar para encerrar a conexão.
  - Seus aplicativos permanecerão ativos até que a sessão seja encerrada no computador remoto.
- **Tela inteira** – entra ou sai do modo de tela inteira.
- **Toque/Mouse** – você pode alternar entre os modos de mouse (toque direto e ponteiro do mouse).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Usar gestos de toque direto e modos de mouse em uma sessão remota

Dois modos de mouse estão disponíveis para interagir com a sessão.

- **Toque direto**: passa todos os contatos de toque para a sessão para que sejam interpretados remotamente.
  - Usado da mesma forma que você usaria o Windows com uma tela sensível ao toque.
- **Ponteiro do mouse**: transforma sua tela sensível ao toque local em um grande touchpad, permitindo mover um ponteiro do mouse na sessão.
  - Usado da mesma forma que você usaria o Windows com um touchpad.

> [!NOTE]
> Ao interagir com o Windows 8 ou mais recente, gestos de toque nativos são compatíveis no modo de toque direto.

| Modo do mouse    | Operação do mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Clicar com o botão esquerdo do mouse           | Tocar com um dedo                                                          |
| Toque direto  | Clicar com o botão direito do mouse          | Tocar e segurar com um dedo                                                 |
| Ponteiro do mouse | Clicar com o botão esquerdo do mouse           | Tocar com um dedo                                                          |
| Ponteiro do mouse | Clicar com o botão do mouse esquerdo e arrastar  | Dar um toque duplo com um dedo e segurar, depois arrastar                               |
| Ponteiro do mouse | Clicar com o botão direito do mouse          | Tocar com dois dedos                                                          |
| Ponteiro do mouse | Clicar com o botão direito do mouse e arrastar | Dar um toque duplo com dois dedos e segurar, depois arrastar                               |
| Ponteiro do mouse | Botão de rolagem do mouse          | Tocar com dois dedos e segurar, depois arrastar para cima ou baixo                           |
| Ponteiro do mouse | Zoom                 | Usar dois dedos e fazer movimento de pinça para aplicar zoom ou aumentar o espaço entre os dedos para reduzir o zoom. |

> [!TIP]
> Perguntas e comentários são sempre bem-vindos. No entanto, NÃO publique uma solicitação de ajuda com solução de problemas usando o recurso de comentários no final deste artigo. Em vez disso, vá para o [Fórum de cliente da Área de Trabalho Remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicie uma nova conversa. Tem alguma sugestão de recurso? Conte-nos usando o [Hub de Comentários](feedback-hub://?tabid=2&contextid=605).
