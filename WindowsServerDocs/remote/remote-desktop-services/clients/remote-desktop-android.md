---
title: Introdução a área de trabalho remota no Android
description: Basic configurar etapas para o cliente de área de trabalho remota para Android.
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
ms.openlocfilehash: 42b4b4ffb73bd9d5d1397d32bd36c41d7e404dd7
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297425"
---
# Introdução a área de trabalho remota no Android

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de área de trabalho remota para Android para trabalhar com áreas de trabalho e aplicativos do Windows diretamente do seu dispositivo Android.

Use as seguintes informações para começar. Certifique-se de fazer check-out a [perguntas Frequentes](remote-desktop-client-faq.md) , se você tiver dúvidas.

> [!NOTE]
> - Curioso sobre as novas versões do cliente Android? Confira [Novidades para área de trabalho remota no Android?](android-whatsnew.md)
> Você pode executar o cliente em Android 4.1 e dispositivos mais recentes, bem como em. Chromebooks com 53 ChromeOS instalado. Saiba mais sobre aplicativos Android no Chrome [aqui](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## Obter o cliente de área de trabalho remota e começar a usá-lo

Siga estas etapas para começar com a área de trabalho remota no seu dispositivo Android:

1. Baixe o cliente de área de trabalho remota no [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de área de trabalho remota ou um recurso remoto. Você usar uma conexão para se conectar diretamente a um computador com Windows e um recurso remoto para usar um programa RemoteApp, com base em sessão de área de trabalho, ou área de trabalho virtual publicados no local. 
4. Crie um widget para que você possa ficar rapidamente para a área de trabalho remota.

> [!NOTE]
> Se você gostaria de novos recursos de versão de pré-lançamento anteriormente é recomendável baixar nosso aplicativo [Beta de área de trabalho remota da Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) do armazenamento do Google Play. 

### Adicionar uma conexão de área de trabalho remota

Para criar uma conexão de área de trabalho remota:

1. No toque Conexão Center **+** e, em seguida, toque em **Desktop**.
2. Insira as seguintes informações para o computador que você deseja se conectar:
  - **Nome do computador** – o nome do computador. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome de computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Nome de usuário** – o nome de usuário para usar para acessar o computador remoto. Você pode usar os seguintes formatos: *nome_do_usuário*, *domínio \ nome_de_usuário*ou *user_name@domain.com*. Você também pode especificar se deseja solicitar um nome de usuário e senha.
3. Você também pode definir as seguintes opções adicionais:
  - **Nome amigável** – um nome para o computador que você está se conectando fácil de lembrar. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador é exibido.
  - **Gateway** – gateway a área de trabalho remota que você deseja usar para conectar a áreas de trabalho virtuais, programas do RemoteApp e áreas de trabalho da sessão com base em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
    Você precisa configurar um Gateway de área de trabalho remota?
  - **Som** – selecione o dispositivo a ser usado para áudio durante a sessão remota. Você pode optar por reproduzir um som em dispositivos locais, o dispositivo remoto, ou não em todos.
  - **Personalizar a resolução de exibição** - definir uma resolução personalizada para uma conexão ao habilitar essa configuração. Quando desativado a resolução é aplicado que você definiu nas configurações globais do aplicativo.
  - **Botões do mouse troca** – Use esta opção para a troca de funções de botão esquerdo do mouse para o botão direito do mouse. (Isso é especialmente útil se você usar um mouse à direita, mas o computador remoto está configurado para um usuário à esquerda.)
  - **Conectar-se a sessão do administrador** - Use essa opção para se conectar a uma sessão de console para administrar um servidor do Windows.
  - **Redirecionar para armazenamento local** – monta o armazenamento local como um sistema de arquivos remoto no computador remoto.
4. Toque em **Salvar**.

Você precisa editar essas configurações? Toque no menu de estouro (**...**) ao lado do nome da área de trabalho e, em **Editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro (**...**) e, em seguida, toque em **Remover**.

>[!TIP]
> Se você receber o erro 0xf07 uma senha incorreta ("nós não pôde se conectar ao computador remoto porque a senha associada à conta de usuário expirou"), alterar sua senha e tente novamente.

### Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho com base em sessão e áreas de trabalho virtuais publicadas usando conexões de RemoteApp e área de trabalho.

Para adicionar um recurso remoto:

1. Na tela do Centro de Conexão, toque em **+** e, em seguida, toque em **Feed de recursos remotos**. 
2. Insira as informações para o recurso remoto:
   - **Email ou URL** - a URL do servidor de acesso via Web. Você também pode inserir sua conta de email corporativas nesse campo – isso informa ao cliente para pesquisar o servidor de acesso via Web associado com seu endereço de email.
   - **Nome de usuário** - o nome de usuário para usar para o servidor de acesso via Web que você está se conectando.
   - **Senha** - a senha para usar para o servidor de acesso via Web que você está se conectando.
3. Toque em **Salvar**.

Os recursos remotos serão exibidos no centro da Conexão.


Para excluir recursos remotos:

1. Na Central de Conexão, toque no menu de estouro (**...**) ao lado do recurso remoto.
2. Toque em **Remover**.
3. Confirme a exclusão.

### Widgets – Fixar uma área de trabalho salva à tela inicial

Os aplicativos de área de trabalho remota suportam a anexação conexões à tela inicial usando o recurso de widget Android. A maneira que você adicione um widget depende do tipo de dispositivo Android, que você está usando e seu sistema operacional. Aqui está a maneira mais comum para adicionar um widget: 

1. Toque em **aplicativos** para iniciar o menu de aplicativos.
2. Toque em **widgets**.
3. Passe o dedo por meio de widgets e procure o ícone da área de trabalho remota com a descrição, "Fixar área de trabalho remota."
4. Toque, mantenha esse widget de área de trabalho remota e movê-lo para a tela inicial.
5. Quando você soltar o ícone, você verá as áreas de trabalho remotas salvas. Escolha a conexão que você deseja salvar a sua tela inicial.

Agora você pode iniciar a conexão de área de trabalho remota diretamente da sua tela inicial tocando-lo.

> [!NOTE]
> Se você renomear a conexão de área de trabalho no aplicativo de área de trabalho remota, o rótulo dessa área de trabalho remota fixado não serão atualizados.

## Conectar a um Gateway de área de trabalho remota para acessar ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de área de trabalho remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações gt _ Gateways**. Toque em **+** adicionar um novo gateway.
2. Insira as seguintes informações:
  - **Nome do servidor** – o nome do computador que deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
  - **Nome de usuário** - o nome de usuário e senha a ser usada para o Gateway de área de trabalho remota você está se conectando. Você também pode selecionar **usar a conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de área de trabalho remota.

## Gerenciar suas contas de usuário

Quando você se conecta aos recursos de um desktop ou controle remoto, você pode salvar as contas de usuário de novamente. Você também pode definir contas de usuário no cliente em si, em vez de salvar os dados do usuário quando você se conectar a uma área de trabalho.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, toque em **configurações**e, em seguida, toque em **contas de usuário**.
2. Toque em **+** adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** - o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_do_usuário, domínio \ nome_de_usuário, ou user_name@domain.com.
   - **Senha** - a senha para o usuário especificado. Cada conta de usuário que você deseja salvar para usar para conexões remotas precisa ter uma senha associada a ela.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **contas de usuário gt _ configurações**.
2. Tocar e segurar uma conta de usuário na lista para selecioná-lo. Você pode selecionar vários usuários.
3. Toque a Lixeira para excluir o usuário selecionado.

## Navegue a sessão de área de trabalho remota
Quando você inicia uma conexão de área de trabalho remota, há ferramentas disponíveis que podem ser usados para navegar a sessão.

### Iniciar uma conexão de área de trabalho remota

1. Toque a conexão de área de trabalho remota para iniciar a sessão. 
2. Se você for solicitado a verificar o certificado para a área de trabalho remota, toque em **Conectar**. Você também pode selecionar **não perguntar novamente para conexões com este computador** sempre aceite o certificado.

### Gerenciar configurações de aplicativo global

Você pode definir as seguintes configurações globais no seu cliente Android:

- **Mostrar visualizações de área de trabalho** - permite que você veja uma visualização de uma área de trabalho no centro da Conexão antes de se conectar a ele. Por padrão, isso é definido como **no**.
- **Pinçar para Zoom** - permite que você use gestos de pinçar para aplicar zoom. Se o aplicativo que você está usando por meio da área de trabalho remota suporta multitoque (introduzido no Windows 8), ativar essa configuração **desativada**.
- **Ajudar a melhorar a área de trabalho remota** - envia dados anônimos à Microsoft. Nós usamos esses dados para melhorar o cliente. Você pode saber mais sobre como podemos tratar esses dados anônimos, particulares, consulte a [Declaração de privacidade de cliente de área de trabalho remota](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Por padrão, essa configuração é **no**.
- **Exibir** - há duas configurações globais para sua exibição:
   - **Orientação** - define a orientação preferencial (paisagem ou retrato) para sua sessão. 
   >[!NOTE]
   > Se você se conectar a um computador executando o Windows 8 ou uma versão mais antiga do Windows, a sessão não sejam dimensionadas corretamente. A melhor opção é desconectar do computador e, em seguida, reconecte na orientação que deseja usar. Uma opção ainda melhor é atualizar o computador para pelo menos Windows 8.1.

   - **Resolução** - define a resolução em que você deseja usar para conexões de área de trabalho globalmente. Se você já tiver definido uma resolução personalizada para um aplicativo individual ou uma conexão, essa configuração não altera isso.
   >[!NOTE]
   >Quando você alterar uma das configurações de exibição, eles só se aplicam ao novas conexões desse ponto em. Para ver a alteração em uma sessão, que você já está conectado para desconectar e, em seguida, conecte-se novamente.

### Barra de Conexão

O fornece de barra de conexão que acesso a controles adicionais de navegação. Por padrão, a barra de conexão é colocada no meio na parte superior da tela. Toque duas vezes e arrastar a barra para a esquerda ou direita para movê-lo.

- **Controle de panorâmica**: O controle de panorâmica permite que a tela seja ampliado e removidos. Observe que o controle panorâmico só está disponível usando toque direto.
   - Habilitar / desabilitar o controle de Panorâmica: Toque no ícone de movimento panorâmico na barra de conexão para exibir o controle de Panorâmica e zoom na tela. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela à resolução original.
   - Usar o controle de Panorâmica: tocar e segurar panorâmica de controle e, em seguida, arraste na direção em que você deseja mover a tela.
   - Mover o controle Panorâmica: Toque duas vezes e mantenha o controle de movimento panorâmico para mover o controle na tela.
- **Opções adicionais**: Toque no ícone de opções adicionais para exibir a seleção de sessão da barra e comando da barra (veja abaixo).
- **Teclado**: Toque no ícone de teclado para exibir ou ocultar o teclado. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mover a barra de conexão**: tocar e segurar a barra de conexão e, em seguida, arrastar e soltar para um novo local na parte superior da tela.


### Barra de comandos

Toque na barra de conexão para exibir a barra de comandos no lado direito da tela. Você pode alternar entre os modos de mouse (toque direto e ponteiro do Mouse). Use o botão página inicial para retornar para o Centro de conexão da barra de comandos. Como alternativa, você pode usar o botão Voltar para a mesma ação. Sua sessão ativa não será desconectada. 


### Use diretos touch gestos e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> Interagir com o Windows 8 ou mais recente os gestos de toque nativos têm suporte no modo de toque direto. 

| Modo de mouse    | Operação de mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Clique esquerdo           | toque com 1 dedos                                                          |
| Toque direto  | Clique com o botão direito do mouse          | 1 dedo tocar e segurar                                                 |
| Ponteiro do mouse | Zoom                 | Use os 2 dedos e pinçar para ampliar ou mover distanciando dedos para reduzir o zoom. |
| Ponteiro do mouse | Clique esquerdo           | toque com 1 dedos                                                          |
| Ponteiro do mouse | Clicar com o botão esquerdo e arrastar  | toque duplo 1 dedo e segurar, arraste                               |
| Ponteiro do mouse | Clique com o botão direito do mouse          | toque com dedos 2                                                          |
| Ponteiro do mouse | Clicar com o botão direito e arrastar | 2 toque duplo dedo e segurar, arraste                               |
| Ponteiro do mouse | Roda do mouse          | 2 dedo toque pressionado e arraste para cima ou para baixo                           |

> [!TIP]
> Perguntas e comentários sempre são boas-vindas. No entanto, não envie uma solicitação de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum do cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo segmento. Tem uma sugestão de recurso? Conte-no [Fórum de voz de usuário do cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
