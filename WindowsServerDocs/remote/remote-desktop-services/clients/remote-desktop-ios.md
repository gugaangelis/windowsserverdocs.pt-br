---
title: Comece com a área de trabalho remota no iOS
description: Saiba como configurar o cliente de área de trabalho remota para iOS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: 71fe969de4d21f7fa3c134b0f80fc7f69e5b2da8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446700"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>Comece com a área de trabalho remota no iOS

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de área de trabalho remota para iOS para trabalhar com aplicativos, recursos e áreas de trabalho do Windows do seu dispositivo iOS (iPhones e iPads).

Use as seguintes informações para começar a usar. Não se esqueça de conferir o [perguntas frequentes sobre](remote-desktop-client-faq.md) se você tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre novas versões para o cliente do iOS? Fazer check-out [o que há de novo para a área de trabalho remota no iOS?](ios-whatsnew.md)
> - O cliente do iOS dá suporte a dispositivos que executam o iOS 6. x e mais recente.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obter o cliente de área de trabalho remota e começar a usá-lo

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Baixe o cliente de área de trabalho remota da loja do iOS
Siga estas etapas para começar com a área de trabalho remota em seu dispositivo iOS:

1. Baixe o cliente de área de trabalho remota Microsoft do [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Adicionar um [conexão de área de trabalho remota](#add-a-remote-desktop-connection) ou um [recurso remoto](#add-a-remote-resource). Usar uma conexão para conectar a uma diretamente a um computador do Windows e um recurso remoto para usar um programa RemoteApp, com base em sessão de área de trabalho ou uma área de trabalho virtual publicado no local usando conexões de RemoteApp e área de trabalho. Esse recurso está disponível geralmente em ambientes corporativos.

### <a name="download-the-remote-desktop-ios-beta-client"></a>Baixe o cliente de versão Beta do iOS de área de trabalho remota
Em seu dispositivo iOS, siga [estas instruções](https://aka.ms/rdiosbeta) para baixar o cliente de versão Beta do iOS de área de trabalho remota.

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de área de trabalho remota

Para criar uma conexão de área de trabalho remota: 
1. No tap Centro de Conexão **+** e, em seguida, toque em **adicionar PC ou servidor**.
2. Insira as seguintes informações para a conexão de área de trabalho remota:
   - **Nome do PC** – o nome do computador. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode anexar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nome de usuário** – o nome de usuário a ser usada para acessar o computador remoto. Você pode usar os seguintes formatos: *user_name*, *domain\user_name*, ou <em>user_name@domain.com</em>. Você também pode especificar se é solicitar um nome de usuário e senha.
3. Você também pode definir as seguintes opções adicionais:
   - **Nome amigável (opcional)** – um nome fácil de lembrar para o PC que você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador é exibido.
   - **(Opcional) do gateway** – gateway de área de trabalho remota a que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
   - **Som** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, o dispositivo remoto, ou não de forma alguma.
   - **Trocar os botões do mouse** – sempre que um gesto de mouse enviaria um comando com o botão esquerdo do mouse, ele envia o mesmo comando com o botão direito do mouse em vez disso. Isso é necessário se o computador remoto está configurado para o modo de mouse canhoto.
   - **Modo de administrador** -conectar-se a uma sessão de administração em um servidor executando o Windows Server 2003 ou posterior.
4. Toque **salvar**.

É necessário editar essas configurações? Pressione e mantenha a área de trabalho que você deseja editar e, em seguida, toque no ícone de configurações. 

### <a name="add-a-remote-resource"></a>Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas usando conexões de RemoteApp e área de trabalho.

- A URL exibe o link para o servidor de acesso via Web RD que fornece acesso a conexões de RemoteApp e área de trabalho.
- O configuradas conexões de RemoteApp e área de trabalho são listadas.

Para adicionar um recurso remoto:

1. Na tela do Centro de Conexão, toque **+** e, em seguida, toque em **adicionar recursos remotos**. 
2. Insira informações para o recurso remoto:
   - **URL do feed** -a URL do servidor de acesso via Web RD. Você também pode inserir sua conta de email corporativo nesse campo – isso informa ao cliente para procurar o servidor de acesso da Web de área de trabalho remota associado com seu endereço de email.
   - **Nome de usuário** -o nome de usuário a ser usado para o servidor de acesso via Web RD que você está se conectando.
   - **Senha** -a senha a ser usado para o servidor de acesso via Web RD que você está se conectando.
3. Toque **salvar**.

Os recursos remotos serão exibidos no centro da Conexão.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de área de trabalho remota para acessar os ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de área de trabalho remota.

Para configurar um novo gateway:

1. No Centro de Conexão, toque em **Configurações > Gateways**. 
2. Toque **gateway de área de trabalho remota adicionar**.
3. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** -o nome de usuário e a senha a ser usada para o gateway de área de trabalho remota que você está se conectando. Você também pode selecionar **usar credenciais de conexão** para usar o mesmo nome de usuário e senha usadas para a conexão de área de trabalho remota.


## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário 

Quando você se conectar a um recurso de área de trabalho ou remoto, você pode salvar as contas de usuário para selecionar de novamente. Você pode gerenciar suas contas de usuário usando o cliente de área de trabalho remota.

Para criar uma nova conta de usuário:

1. No Centro de Conexão, toque em **as configurações**e, em seguida, toque em **nomes de usuário**.
2. Toque **adicionar a conta de usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** -o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: domínio \ nome_de_usuário, user_name ou user_name@domain.com.
   - **Senha** -a senha para o usuário especificado. Cada conta de usuário que você deseja salvar para usar para conexões remotas precisa ter uma senha associada a ele.
4. Toque **salve**e, em seguida, toque em **configurações**.
5. Toque **feito** para salvar a nova configuração.

Para excluir uma conta de usuário:

1. No Centro de Conexão, toque em **Configurações > nomes de usuário**.
2. Passe o dedo para a linha da direita para esquerda para selecionar o usuário.
3. Toque **excluir**.



## <a name="navigate-the-remote-desktop-session"></a>Navegue a sessão de área de trabalho remota
Quando você inicia uma sessão de área de trabalho remota, existem ferramentas disponíveis que você pode usar para navegar a sessão.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma Conexão de área de trabalho remota

1. Toque a conexão de área de trabalho remota para iniciar a sessão de área de trabalho remota. 
2. Se você for solicitado a verificar o certificado para a área de trabalho remota, toque em **Accept**. Você pode optar por sempre aceitar deslizando a **não me pergunte novamente sobre conexões com este computador** alternar para o **ON**. 

### <a name="connection-bar"></a>Barra de Conexão

A conexão barra lhe dê que acesso para controles de navegação adicionais. 

- **Controle de panorâmica**: O controle de panorâmica permite que a tela ser ampliados e movidos. Observe que o controle panorâmico só está disponível usando o toque direto.
   - Habilitar / desabilitar o controle de Panorâmica: Toque no ícone de bandeja na barra de conexão para exibir o controle de Panorâmica e zoom a tela. Toque no ícone de bandeja na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Use o controle de Panorâmica: Toque e mantenha o controle de Panorâmica e, em seguida, arraste na direção em que você deseja mover a tela.
   - Mova o controle de Panorâmica: Duplo toque e segure o controle panorâmico para mover o controle na tela.
- **Nome da Conexão**: O nome da conexão atual é exibido. Toque no nome de conexão para exibir a barra de seleção de sessão.
- **Teclado**: Toque no ícone de teclado para exibir ou ocultar o teclado. O controle panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mova a barra de conexão**: Toque e mantenha a barra de conexão e, em seguida, arraste e solte para um novo local na parte superior da tela.

### <a name="session-selection"></a>Seleção de sessão
Você pode ter várias conexões abrir em computadores diferentes ao mesmo tempo. Toque na barra de conexão para exibir a barra da sessão de seleção no lado esquerdo da tela. Barra da sessão de seleção permite que você exibir suas conexões abertas e alternar entre eles. 

- Alternar entre aplicativos em uma sessão aberta de recurso remoto.

    Quando você está conectado aos recursos remotos, você pode alternar entre aplicativos abertos dentro dessa sessão tocando no menu do expansor e escolhendo na lista de itens disponíveis.
- Inicie uma nova sessão

  Você pode iniciar novos aplicativos ou sessões da área de trabalho de dentro de sua conexão atual: toque **Iniciar novo**e, em seguida, escolha na lista de itens disponíveis.

- Desconexão de uma sessão

  Para desconectar um toque de sessão X do lado esquerdo do bloco de sessão.

### <a name="command-bar"></a>Barra de comandos

Barra de comandos substituído o utilitário barra a partir da versão 8.0.1. Você pode alternar entre os modos de mouse e retornar para o Centro de conexão na barra de comandos.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Use gestos de toque e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> Interação com o Windows 8 ou mais recente de gestos de toque nativos têm suporte no modo de toque direto. Para obter mais informações sobre o Windows 8 gestos consulte [de toque: Passe o dedo, toque e muito mais](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Modo de mouse    | Operação de mouse      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque direto  | Com o botão esquerdo           | toque de 1 dedo                                               |
| Toque direto  | Clique com o botão direito do mouse          | 1 de toque de dedo e manter pressionado                                      |
| Ponteiro do mouse | Com o botão esquerdo           | toque de 1 dedo                                               |
| Ponteiro do mouse | Clicar com o botão esquerdo e arrastar  | toque duplo de 1 dedo e manter pressionado, arraste                    |
| Ponteiro do mouse | Clique com o botão direito do mouse          | toque de dedo 2                                               |
| Ponteiro do mouse | Clicar com o botão direito e arrastar | 2 toque duplo de dedo e manter pressionado, arraste                    |
| Ponteiro do mouse | Roda do mouse          | 2 dedo toque e mantenha pressionado e arrastar para cima ou para baixo                |
| Ponteiro do mouse | Zoom                 | Aperto 2 dedos para ampliar ou distribuídos 2 dedos para zoom |

## <a name="supported-input-devices"></a>Suporte para dispositivos de entrada

O [cliente de versão beta do iOS de área de trabalho remota](https://aka.ms/rdiosbeta) dá suporte os mouses Swiftpoint GT e ProPoint físicos. Swiftpoint está oferecendo uma [desconto exclusivo](https://www.swiftpoint.com/microsoft/) sobre o GT para usuários de cliente do iOS beta.

O cliente do iOS atualmente suporta apenas Swiftpoint mouses. Consulte a [o que há de novo no cliente do iOS](ios-whatsnew.md) página e o [iOS App Store](https://aka.ms/rdios) para ler notícias sobre o suporte para outros dispositivos no futuro.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usar um teclado em uma sessão remota

Você pode usar tanto na tela teclado ou físico do teclado em sua sessão remota.

Na tela teclados, use o botão na borda direita da barra de acima do teclado para alternar entre o teclado padrão e adicional.

Se o Bluetooth está habilitado para seu dispositivo iOS, o cliente detecta automaticamente o teclado Bluetooth.

Lembre-se de que, devido a limitações no sistema operacional, chaves especiais, como Ctrl, opção e a função não funcionará conforme o esperado com um teclado Bluetooth. As chaves a seguir funcionam:

- Chaves alfanuméricas
- Teclas de cursor
- Guia: Guia funciona, mas não funciona Shift + Tab
- Página inicial / Pos1: Alt + Left = início
- Fim: ALT + seta para direita = término
- Page Up: ALT + seta para cima = Page Up
- Page Down: ALT + seta para baixo = Page Down
- Selecione tudo: Command + A = Ctrl + A (Selecionar tudo na maioria dos programas)
- Recortar: Command + X = Ctrl + X (Recortar na maioria dos programas)
- Copiar: Command + C = Ctrl + C (copiar na maioria dos programas)
- Colar: Command + V = Ctrl + V (colar na maioria dos programas)
- Símbolos: Teclas ALT + alfanumérica produzirá símbolos diferentes, dependendo do idioma configurado

> [!TIP]
> Perguntas e comentários são sempre bem-vindas. No entanto, não poste uma solicitação de ajuda de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum de cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo thread. Tem alguma sugestão de recurso? Conte-na [Fórum de voz do usuário cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

