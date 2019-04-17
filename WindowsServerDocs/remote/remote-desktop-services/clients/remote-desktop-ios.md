---
title: Começar com a área de trabalho remota no iOS
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
ms.openlocfilehash: aab84dde3ded2a3d3f17f4bb318302321c444606
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297225"
---
# Começar com a área de trabalho remota no iOS

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de área de trabalho remota para iOS para trabalhar com aplicativos Windows, recursos e áreas de trabalho do seu dispositivo iOS (iPhones e iPads).

Use as seguintes informações para começar. Certifique-se de fazer check-out a [perguntas Frequentes](remote-desktop-client-faq.md) , se você tiver dúvidas.

> [!NOTE]
> - Curioso sobre as novas versões do cliente iOS? Confira [Novidades para área de trabalho remota no iOS?](ios-whatsnew.md)
> - O cliente iOS oferece suporte a dispositivos que executam o iOS 6. x e versões mais recentes.

## Obter o cliente de área de trabalho remota e começar a usá-lo

### Baixe o cliente de área de trabalho remota da store iOS
Siga estas etapas para começar com a área de trabalho remota no seu dispositivo iOS:

1. Baixe o cliente de área de trabalho remota Microsoft no [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Adicione uma [conexão de área de trabalho remota](#add-a-remote-desktop-connection) ou um [recurso remoto](#add-a-remote-resource). Usar uma conexão para conectar a um diretamente a um computador Windows e um recurso remoto para usar um programa RemoteApp, com base em sessão de área de trabalho ou uma área de trabalho virtual publicados no local usando o RemoteApp e conexões de área de trabalho. Esse recurso está normalmente disponível em ambientes corporativos.

### Baixe o cliente de Beta do iOS de área de trabalho remota
Em seu dispositivo iOS, siga [estas instruções](https://aka.ms/rdiosbeta) para baixar o cliente de Beta do iOS de área de trabalho remota.

### Adicionar uma conexão de área de trabalho remota

Para criar uma conexão de área de trabalho remota: 
1. No toque Conexão Center **+** e, em seguida, toque em **Adicionar computador ou servidor**.
2. Insira as seguintes informações para a conexão de área de trabalho remota:
  - **Nome do computador** – o nome do computador. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome de computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Nome de usuário** – o nome de usuário para usar para acessar o computador remoto. Você pode usar os seguintes formatos: *nome_do_usuário*, *domínio \ nome_de_usuário*ou *user_name@domain.com*. Você também pode especificar se deseja solicitar um nome de usuário e senha.
3. Você também pode definir as seguintes opções adicionais:
  - **Nome amigável (opcional)** – um nome para o computador que você está se conectando fácil de lembrar. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador é exibido.
  - **Gateway (opcional)** – gateway a área de trabalho remota que você deseja usar para conectar a áreas de trabalho virtuais, programas do RemoteApp e áreas de trabalho da sessão com base em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
  - **Som** – selecione o dispositivo a ser usado para áudio durante a sessão remota. Você pode optar por reproduzir um som em dispositivos locais, o dispositivo remoto, ou não em todos.
  - **Troca de botões do mouse** – sempre que um gesto de mouse envia um comando com o botão esquerdo do mouse, ele o envia o mesmo comando com o botão direito do mouse em vez disso. Isso é necessário se o computador remoto estiver configurado para o modo de mouse à esquerda.
  - **Modo administrador** - se conectar a uma sessão de administração em um servidor executando o Windows Server 2003 ou posterior.
4. Toque em **Salvar**.

Você precisa editar essas configurações? Pressione e segure a área de trabalho que você deseja editar e, em seguida, toque no ícone de configurações. 

### Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho com base em sessão e áreas de trabalho virtuais publicadas usando conexões de RemoteApp e área de trabalho.

- A URL exibe o link para o servidor de acesso via Web que fornece acesso a RemoteApp e conexões de área de trabalho.
- O configurado RemoteApp e conexões de área de trabalho são listados.

Para adicionar um recurso remoto:

1. Na tela do Centro de Conexão, toque em **+** e, em seguida, toque em **Adicionar recursos remotos**. 
2. Insira as informações para o recurso remoto:
   - **URL do feed** - a URL do servidor de acesso via Web. Você também pode inserir sua conta de email corporativas nesse campo – isso informa ao cliente para pesquisar o servidor de acesso via Web associado com seu endereço de email.
   - **Nome de usuário** - o nome de usuário para usar para o servidor de acesso via Web que você está se conectando.
   - **Senha** - a senha para usar para o servidor de acesso via Web que você está se conectando.
3. Toque em **Salvar**.

Os recursos remotos serão exibidos no centro da Conexão.


## Conectar a um Gateway de área de trabalho remota para acessar ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de área de trabalho remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **Configurações gt _ Gateways**. 
2. Toque **gateway Adicionar área de trabalho remota**.
3. Insira as seguintes informações:
  - **Nome do servidor** – o nome do computador que deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
  - **Nome de usuário** - o nome de usuário e senha a ser usada para o gateway de área de trabalho remota, que você está se conectando. Você também pode selecionar **usar credenciais de conexão** para usar o mesmo nome de usuário e senha, como aqueles usados para a conexão de área de trabalho remota.


## Gerenciar suas contas de usuário 

Quando você se conecta aos recursos de um desktop ou controle remoto, você pode salvar as contas de usuário de novamente. Você pode gerenciar suas contas de usuário usando o cliente de área de trabalho remota.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, toque em **configurações**e, em seguida, toque em **Nomes de usuário**.
2. Toque em **Adicionar conta de usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** - o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_do_usuário, domínio \ nome_de_usuário, ou user_name@domain.com.
   - **Senha** - a senha para o usuário especificado. Cada conta de usuário que você deseja salvar para usar para conexões remotas precisa ter uma senha associada a ela.
4. Toque em **Salvar**e, em seguida, toque em **configurações**.
5. Toque em **concluído** para salvar a nova configuração.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **Configurações gt _ nomes de usuário**.
2. Passe o dedo na linha da direita para a esquerda para selecionar o usuário.
3. Toque em **Excluir**.



## Navegue a sessão de área de trabalho remota
Quando você inicia uma sessão de área de trabalho remota, há ferramentas disponíveis que podem ser usados para navegar a sessão.

### Iniciar uma Conexão de área de trabalho remota

1. Toque a conexão de área de trabalho remota para iniciar a sessão de área de trabalho remota. 
2. Se você for solicitado a verificar o certificado para a área de trabalho remota, toque em **Aceitar**. Você pode optar por aceitar sempre deslizando o botão de alternância **não perguntar novamente para conexões com este computador** como **ON**. 

### Barra de Conexão

O fornece de barra de conexão que acesso a controles adicionais de navegação. 

- **Controle de panorâmica**: O controle de panorâmica permite que a tela seja ampliado e removidos. Observe que o controle panorâmico só está disponível usando toque direto.
   - Habilitar / desabilitar o controle de Panorâmica: Toque no ícone de movimento panorâmico na barra de conexão para exibir o controle de Panorâmica e zoom na tela. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela à resolução original.
   - Usar o controle de Panorâmica: tocar e segurar panorâmica de controle e, em seguida, arraste na direção em que você deseja mover a tela.
   - Mover o controle Panorâmica: Toque duas vezes e mantenha o controle de movimento panorâmico para mover o controle na tela.
- **Nome da Conexão**: O nome da conexão atual é exibido. Toque no nome da conexão para exibir a barra de seleção de sessão.
- **Teclado**: Toque no ícone de teclado para exibir ou ocultar o teclado. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.
- **Mover a barra de conexão**: tocar e segurar a barra de conexão e, em seguida, arrastar e soltar para um novo local na parte superior da tela.

### Seleção de sessão
Você pode ter várias conexões abrir para computadores diferentes ao mesmo tempo. Toque na barra de conexão para exibir a barra de seleção de sessão no lado esquerdo da tela. A barra de seleção de sessão permite que você exiba suas conexões abertas e alternar entre eles. 

- Alternar entre aplicativos em uma sessão remota de recurso aberto.

    Quando você está conectado a recursos remotos, você pode alternar entre aplicativos abertos dessa sessão tocando no menu de expansão e escolhendo na lista de itens disponíveis.
- Iniciar uma nova sessão

  Você pode iniciar novos aplicativos ou sessões de área de trabalho de dentro de sua conexão atual: toque **Iniciar nova**e, em seguida, escolha na lista de itens disponíveis.

- Desconexão de uma sessão

  Para desconectar um toque de sessão X no lado esquerdo do bloco sessão.

### Barra de comandos

A barra de comandos substituído o utilitário desde a versão 8.0.1 da barra. Você pode alternar entre os modos de mouse e retornar para o Centro de conexão da barra de comandos.

## Use gestos de toque e modos de mouse em uma sessão remota

O cliente usa gestos de toque padrão. Você também pode usar gestos de toque para replicar as ações do mouse na área de trabalho remota. Os modos de mouse disponíveis são definidos na tabela a seguir.

> [!NOTE]
> Interagir com o Windows 8 ou mais recente os gestos de toque nativos têm suporte no modo de toque direto. Para obter mais informações sobre o Windows 8 Consulte gestos [Touch: passar o dedo, toque ou mais](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Modo de mouse    | Operação de mouse      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Toque direto  | Clique esquerdo           | toque com 1 dedos                                               |
| Toque direto  | Clique com o botão direito do mouse          | 1 dedo tocar e segurar                                      |
| Ponteiro do mouse | Clique esquerdo           | toque com 1 dedos                                               |
| Ponteiro do mouse | Clicar com o botão esquerdo e arrastar  | toque duplo 1 dedo e segurar, arraste                    |
| Ponteiro do mouse | Clique com o botão direito do mouse          | toque com dedos 2                                               |
| Ponteiro do mouse | Clicar com o botão direito e arrastar | 2 toque duplo dedo e segurar, arraste                    |
| Ponteiro do mouse | Roda do mouse          | 2 dedo toque pressionado e arraste para cima ou para baixo                |
| Ponteiro do mouse | Zoom                 | Pinçar 2 dedos para ampliar ou se espalhar 2 dedos para reduzir o zoom |

## Dispositivos de entrada com suporte

O [cliente de beta do iOS de área de trabalho remota](https://aka.ms/rdiosbeta) oferece suporte as mouses físicos Swiftpoint GT e ProPoint. Swiftpoint está oferecendo um [desconto exclusivo](https://www.swiftpoint.com/microsoft/) sobre o GT para usuários do cliente iOS beta.

O cliente iOS atualmente suporta apenas Swiftpoint mouses. Consulte a página de [Novidades no cliente do iOS](ios-whatsnew.md) e o [Repositório de aplicativo iOS](https://aka.ms/rdios) para notícias sobre o suporte para outros dispositivos no futuro.

## Usar um teclado em uma sessão remota

Você pode usar na tela teclado ou teclado físico na sessão remota.

Na tela teclados, use o botão na borda direita da barra acima do teclado para alternar entre o teclado padrão e adicional.

Se o Bluetooth está habilitado para seu dispositivo iOS, o cliente detecta automaticamente o teclado Bluetooth.

Lembre-se de que, devido a limitações no sistema operacional, teclas especiais, como opção, Ctrl e função não funcionarão conforme esperado com um teclado Bluetooth. As seguintes chaves funcionam:

- Teclas alfanuméricas
- Teclas de cursor
- Guia: Funciona de guia, mas Shift + Tab não funciona
- Home / Pos1: Alt + à esquerda = Home
- Final: Alt + seta para direita = final
- Page Up: Alt + seta para cima = Page Up
- Page Down: Alt + seta para baixo = Page Down
- Selecionar tudo: Comando + A = Ctrl + A (Selecionar tudo na maioria dos programas)
- Recortar: Comando + X = Ctrl + X (Recortar na maioria dos programas)
- Cópia: Comando + C = Ctrl + C (copiar na maioria dos programas)
- Colar: Comando + V = Ctrl + V (colar na maioria dos programas)
- Símbolos: Teclas Alt + Alphanumeric produzirá símbolos diferentes dependendo do idioma configurado

> [!TIP]
> Perguntas e comentários sempre são boas-vindas. No entanto, não envie uma solicitação de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum do cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo segmento. Tem uma sugestão de recurso? Conte-no [Fórum de voz de usuário do cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

