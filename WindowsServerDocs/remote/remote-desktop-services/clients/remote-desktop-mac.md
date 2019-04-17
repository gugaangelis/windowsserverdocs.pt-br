---
title: Introdução a área de trabalho remota no Mac
description: Saiba como configurar o cliente de área de trabalho remota para Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: e8c5da1960d0e3129b5520e65c2d5ecf45eef778
ms.sourcegitcommit: 6dc14d4315793132488494b5543ee83e3f562f09
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4555713"
---
# Introdução a área de trabalho remota no Mac

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Você pode usar o cliente de área de trabalho remota para Mac para trabalhar com aplicativos, recursos e áreas de trabalho do Windows em seu computador Mac. Use as informações a seguir para começar e check-out a [Perguntas frequentes sobre](remote-desktop-client-faq.md) se você tiver dúvidas.

>[!Note]
> - Curioso sobre os novos lançamentos para o cliente macOS? Confira [que há de novo para área de trabalho remota no Mac?](mac-whatsnew.md)
> - O cliente de Mac é executado em computadores que executam macOS 10.10 e mais recente.
> - As informações neste artigo aplica-se principalmente para a versão completa do cliente Mac - a versão disponível no AppStore Mac. Testar novos recursos, baixando o nosso aplicativo de visualização aqui: [notas de versão de cliente beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## Obter o cliente de área de trabalho remota
Siga estas etapas para começar a usar a área de trabalho remota no seu Mac:

1. Baixe o cliente de área de trabalho remota Microsoft da [Loja de aplicativos do Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Caso você ignore esta etapa, você não pode conectar ao seu computador).
3. Adicione uma conexão de área de trabalho remota ou um recurso remoto. Use uma conexão para se conectar diretamente a um computador com Windows e um recurso remoto para usar um programa RemoteApp, com base em sessão de área de trabalho ou uma área de trabalho virtual publicados no local usando RemoteApp e conexões de área de trabalho. Este recurso está normalmente disponível em ambientes corporativos.

## E o cliente do Mac beta?
Estamos estiver testando novos recursos em nosso canal de visualização em HockeyApp. Quer fazer check-out? Vá para a [Área de trabalho remota para Mac Microsoft](https://go.microsoft.com/fwlink/?LinkID=619698) e clique em **Baixar**. Você não precisa criar uma conta ou a entrada em HockeyApp para baixar o cliente beta.

Se você já tiver o cliente, você pode verificar atualizações garantir que você tenha a versão mais recente. No cliente beta, clique **Beta de área de trabalho remota da Microsoft** na parte superior e, em seguida, clique em **Procurar atualizações**. 

## Adicionar uma conexão de área de trabalho remota
Para criar uma conexão de área de trabalho remota:

1. No Centro de Conexão, clique em **+** e, em seguida, clique em **Desktop**.
2. Insira as seguintes informações:
   - **Nome do computador** - o nome do computador.
      - Isso pode ser um nome de computador do Windows (encontrada nas configurações do **sistema** ), um nome de domínio ou um endereço IP.
      - Você também pode adicionar informações de porta ao final desse nome, como *MyDesktop:3389*.
   - **Conta de usuário** - adicionar a conta de usuário que você usa para acessar o computador remoto.
      - Para ingressados no Active Directory (AD), computadores ou contas locais, use um dos seguintes formatos: *nome_do_usuário*, *domínio \ nome_de_usuário*ou *user_name@domain.com*.
      - Para o Azure Active Directory (AAD) ingressar computadores, use um desses formatos: *AzureAD\user_name* ou *azuread \ user_name@domain.com*.
      - Você também pode escolher se deseja exigir uma senha.
      - Ao gerenciar várias contas de usuário com o mesmo nome de usuário, defina um nome amigável para diferenciar as contas.
      - Gerencie suas contas de usuário que foram salvos nas preferências do aplicativo. 

3. Você também pode definir essas configurações opcionais para a conexão:
   - Definir um nome amigável 
   - Adicionar um Gateway
   - Defina a saída de som
   - Troca de botões do mouse
   - Habilitar o modo de administrador
   - Redirecionamento de pastas locais em uma sessão remota
   - Impressoras locais para a frente
   - Cartões inteligentes para a frente
4. Clique em **Salvar**.

Para iniciar a conexão, apenas duas vezes nele. O mesmo é verdadeiro para recursos remotos.

### Exportar e importar conexões
Você pode exportar uma definição de conexão de área de trabalho remota e usá-lo em um dispositivo diferente. Áreas de trabalho remotas são salvas em separado. Arquivos RDP.

1. No Centro de Conexão, clique com botão direito a área de trabalho remota.
2. Clique em **Exportar**.
3. Navegue até o local onde deseja salvar a área de trabalho remota. Arquivo RDP.
4. Clique em **OK**.

Use as seguintes etapas para importar uma área de trabalho remota. Arquivo RDP.

1. Na barra de menus, clique em **arquivo > Importar**.
2. Navegue até o. Arquivo RDP.
3. Clique em **Abrir**.

## Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho com base em sessão e áreas de trabalho virtuais publicadas usando RemoteApp e conexões de área de trabalho.

- A URL exibe o link para o servidor de acesso via Web que fornece acesso a RemoteApp e conexões de área de trabalho.
- As conexões de área de trabalho e RemoteApp configurado são listados.

Para adicionar um recurso remoto:

1. No clique Conexão Center **+** e, em seguida, clique em **Adicionar recursos remotos**. 
2. Insira as informações para o recurso remoto:
   - **URL do feed** - a URL do servidor de acesso via Web. Você também pode inserir sua conta de email corporativas nesse campo – isso informa ao cliente para procurar o servidor de acesso via Web associado com seu endereço de email.
   - **Nome de usuário** - o nome de usuário para usar para o servidor de acesso via Web que você está se conectando.
   - **Senha** - a senha para o servidor de acesso via Web que você está se conectando.
3. Clique em **Salvar**.


Os recursos remotos serão exibidos no Centro de Conexão.


## Conectar a um Gateway de área de trabalho remota para acessar ativos internos

Um Gateway de área de trabalho remota (Gateway de área de trabalho remota) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways nas preferências do aplicativo ou ao configurar uma nova conexão de área de trabalho.

Para configurar um novo gateway nas preferências:

1. Na Central de Conexão, clique **Preferências > Gateways**. 
2. Clique no **+** botão na parte inferior da tabela insira as seguintes informações:
  - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
  - **Nome de usuário** - o nome de usuário e senha para ser usado para o gateway de área de trabalho remota, que você está se conectando. Você também pode selecionar **usar credenciais de conexão** para usar o mesmo nome de usuário e a senha como os usados para a conexão de área de trabalho remota.


## Gerenciar suas contas de usuário

Quando você se conectar a um desktop ou remoto recursos, você pode salvar as contas de usuário de novamente. Você pode gerenciar suas contas de usuário usando o cliente de área de trabalho remota.

Para criar uma nova conta de usuário:

1. No Centro de Conexão, clique em **configurações** > **contas**.
2. Clique em **Adicionar conta de usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** - o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_do_usuário, domínio \ nome_de_usuário, ou user_name@domain.com.
   - **Senha** - a senha para o usuário especificado. Cada conta de usuário que você deseja salvar para usar para conexões remotas precisa ter uma senha associada a ela.
   - **Nome amigável** - se você estiver usando a mesma conta de usuário com senhas diferentes, defina um nome amigável para distinguir as contas de usuário.
4. Toque em **Salvar**e, em seguida, toque em **configurações**.

## Personalizar a resolução de vídeo
Você pode especificar a resolução de vídeo para a sessão de área de trabalho remota.

1. No Centro de Conexão, clique em **Preferências**.
2. Clique em **resolução**. 
3. Clique em **+**.
4. Insira uma altura de resolução e a largura e, em seguida, clique em **Okey.**

Para excluir a resolução, selecione-o e, em seguida, clique em **-**.

**Exibe ter espaços separados** Se você estiver executando o Mac OS X 10,9 e desativado **monitores têm espaços separados** no Mavericks (**Preferências do sistema > controle de missão**), você precisará definir essa configuração no cliente da área de trabalho remoto usando a mesma opção.

### Redirecionamento de unidade para recursos remotos
Redirecionamento de unidade tem suporte para recursos remotos, para que você pode salvar arquivos criados com um aplicativo remoto localmente para o seu Mac. A pasta redirecionada sempre é o diretório exibido como uma unidade de rede na sessão remota.

> [!NOTE]
> Para usar esse recurso, o administrador precisa definir as configurações apropriadas no servidor.


## Usar um teclado em uma sessão remota

Layouts de teclado do Mac diferem os layouts de teclado do Windows. 

- A tecla de comando no teclado Mac é igual a tecla do Windows.
- Para realizar ações que usam o botão de comando no Mac, você precisará usar o botão de controle no Windows (por exemplo: cópia = Ctrl + C).
- As teclas de função podem ser ativadas na sessão, além disso pressionando a tecla FN (por exemplo: FN + F1).
- A tecla Alt à direita da barra de espaço no teclado Mac é igual a tecla Alt Alt Gr/direita no Windows.

Por padrão, a sessão remota usará o mesmo idioma do teclado como o sistema operacional esteja o cliente. (Se o seu Mac está executando uma en-us do sistema operacional, que será usado para as sessões remotas também. Se o idioma do teclado do sistema operacional não for usado, verifique se o teclado configuração no computador remoto e alterando a configuração manualmente. Consulte as [Perguntas frequentes sobre o cliente de área de trabalho remota](remote-desktop-client-faq.md) para obter mais informações sobre teclados e localidades.


## Suporte para área de trabalho remota gateway conectável autenticação e autorização

Windows Server 2012 R2 introduziu o suporte para um novo método de autenticação, autenticação conectáveis Gateway de área de trabalho remota e autorização, que fornece mais flexibilidade para rotinas de autenticação personalizada. Agora você pode esse modelo de autenticação com o cliente do Mac. 

> [!IMPORTANT]
> Modelos personalizados de autenticação e autorização antes do Windows 8.1 não são suportados, embora o artigo acima aborda-los.

Para saber mais sobre esse recurso, confira [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Perguntas e comentários sempre são boas-vindas. No entanto, não envie uma solicitação de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum do cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo segmento. Tem uma sugestão de recurso? Conte-no [Fórum de voz de usuário do cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

