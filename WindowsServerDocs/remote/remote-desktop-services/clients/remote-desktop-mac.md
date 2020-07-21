---
title: Introdução ao cliente para macOS
description: Saiba como configurar o cliente da Área de Trabalho Remota para Mac
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 7be6b7542ca77c80a638df6404aefe77d8d7d19c
ms.sourcegitcommit: b363d8ceed863c8fd5a464bc8afdc4ef1af9a6f8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86445700"
---
# <a name="get-started-with-the-macos-client"></a>Introdução ao cliente para macOS

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Você pode usar o cliente da Área de Trabalho Remota para Mac para trabalhar com aplicativos, recursos e áreas de trabalho do Windows usando seu computador Mac. Use as seguintes informações para começar – e confira as [Perguntas frequentes](remote-desktop-client-faq.md) se tiver dúvidas.

>[!NOTE]
> - Curioso sobre as novas versões para o cliente macOS? Confira as [Novidades da Área de Trabalho Remota no Mac?](mac-whatsnew.md)
> - O cliente Mac é executado em computadores que executam o macOS 10.10 e versões mais recentes.
> - As informações neste artigo se aplicam principalmente à versão completa do cliente Mac, a versão disponível na Mac AppStore. Teste os novos recursos baixando o aplicativo de teste aqui: [notas de versão do cliente beta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Obtenha o cliente de Área de Trabalho Remota

Siga estas etapas para começar a usar a Área de Trabalho Remota em seu dispositivo Mac:

1. Baixar o cliente da Área de Trabalho Remota da Microsoft da [Mac App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configure seu computador para aceitar conexões remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). Se você ignorar esta etapa, não poderá se conectar ao seu computador.
3. Adicione uma conexão de Área de Trabalho Remota ou um recurso remoto. Você usa uma conexão para se conectar diretamente a um computador Windows e um recurso remoto para usar um programa RemoteApp, uma área de trabalho baseada em sessão ou uma área de trabalho virtual publicada localmente usando conexões de RemoteApp e área de trabalho. Normalmente, esse recurso está disponível em ambientes corporativos.

## <a name="what-about-the-mac-beta-client"></a>E quanto ao cliente beta do Mac?

Vamos testar novos recursos em nosso canal de visualização no App Center. Quer conhecer? Acesse [Área de Trabalho Remota da Microsoft para Mac](https://aka.ms/rdmacbeta) e selecione **Baixar**. Você não precisa criar uma conta nem entrar no App Center para baixar o cliente beta.

Se você já tiver o cliente, poderá verificar as atualizações para garantir que tenha a versão mais recente. No cliente beta, selecione **Área de Trabalho Remota Beta da Microsoft** na parte superior e depois selecione **Verificar atualizações**. 

## <a name="add-a-workspace"></a>Adicionar um workspace

Assine o feed que o administrador lhe deu para obter a lista de recursos gerenciados disponíveis para você em seu dispositivo macOS.

Para assinar um feed:

1. Selecione **Adicionar feed** na página principal para se conectar ao serviço e recuperar seus recursos.
2. Insira a URL do Feed. Ela pode ser uma URL ou endereço de email:
   - Essa URL é geralmente uma URL da Área de Trabalho Virtual do Windows. A que você usa depende de qual versão da Área de Trabalho Virtual do Windows que você está usando.
      - Para a versão Fall 2019, use `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`.
      - Para a versão Spring 2020, use `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`.
   - Para usar email, insira seu endereço de email. Isso informa ao cliente para pesquisar uma URL associada ao seu endereço de email, caso o administrador tenha configurado o servidor dessa maneira.
3. Selecione **Inscrever-se**.
4. Entre com sua conta quando solicitado.

Depois de entrar, você deverá ver uma lista de recursos disponíveis.

Após a assinatura de um feed, o conteúdo dele será atualizado automaticamente de maneira regular. Os recursos podem ser adicionados, alterados ou removidos com base nas alterações realizadas pelo seu administrador.

### <a name="export-and-import-connections"></a>Exporte e importe conexões

Você pode exportar uma definição de conexão de área de trabalho remota e usá-la em um dispositivo diferente. Áreas de trabalho remotas são salvas em arquivos RDP separados.

Para exportar um arquivo RDP:

1. No Centro de Conexão, clique com botão direito do mouse na área de trabalho remota.
2. Selecione **Exportar**.
3. Procure o local em que você deseja salvar o arquivo RDP de área de trabalho remota.
4. Selecione **OK**.

Para importar um arquivo RDP:

1. Na barra de menus, selecione **Arquivo** > **Importar**.
2. Procure o arquivo RDP.
3. Selecione **Abrir**.

## <a name="add-a-remote-resource"></a>Adicionar um recurso remoto

Os recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas usando conexões RemoteApp e de Área de Trabalho.

- A URL exibe o link para o servidor de Acesso via Web à Área de Trabalho Remota, que fornece acesso a conexões de RemoteApp e Área de Trabalho.
- As Conexões de RemoteApp e Área de Trabalho são listadas.

Para adicionar um recurso remoto:

1. Na Central de Conexão, selecione **+** e **Adicionar Recursos Remotos**.
2. Insira informações para o recurso remoto:
   - **URL do Feed** – a URL do servidor de Acesso via Web à Área de Trabalho Remota. Você também pode inserir sua conta de email corporativo nesse campo – isso instrui o cliente a pesquisar pelo servidor de Acesso via Web à Área de Trabalho Remota associado com seu endereço de email.
   - **Nome de usuário** – o nome de usuário a ser usado para o servidor de Acesso via Web à Área de Trabalho Remota ao qual você está se conectando.
   - **Senha** – a senha a ser usada para o servidor de Acesso via Web à Área de Trabalho Remota ao qual você está se conectando.
3. Selecione **Salvar**.

Os recursos remotos serão exibidos na Central de Conexão.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de Área de Trabalho Remota para acessar os ativos internos

Um Gateway de Área de Trabalho Remota permite que você se conecte a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways nas preferências do aplicativo ou ao configurar uma nova conexão de área de trabalho.

Para configurar um novo gateway nas preferências:

1. Na Central de Conexão, selecione **Preferências > Gateways**. 
2. Selecione o botão **+** na parte inferior da tabela e insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Nome de usuário** – o nome de usuário e a senha a serem usados para o Gateway de Área de Trabalho Remota ao qual você está se conectando. Você também pode selecionar **Usar credenciais de conexão** para usar o mesmo nome de usuário e senha usados para a conexão à área de trabalho remota.

## <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conecta a uma área de trabalho ou a recursos remotos, você pode salvar as contas de usuário para selecioná-las novamente. Você pode gerenciar suas contas de usuário usando o cliente da Área de Trabalho Remota.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, selecione **Configurações** > **Contas**.
2. Selecione **Adicionar Conta de Usuário**.
3. Insira as seguintes informações:
   - **Nome de usuário** – o nome do usuário a salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_de_usuário, domínio\nome_de_usuário ou user_name@domain.com.
   - **Senha** – a senha para o usuário que você especificou. Todas as contas de usuário que você deseja salvar para uso com conexões remotas precisam ter uma senha associada a elas.
   - **Nome Amigável** – se você estiver usando a mesma conta de usuário com senhas diferentes, defina um nome amigável para distinguir essas contas.
4. Selecione **Salvar** e depois **Configurações**.

## <a name="customize-your-display-resolution"></a>Personalizar a resolução de vídeo

Você pode especificar a resolução de vídeo para a sessão de área de trabalho remota.

1. Na Central de Conexão, selecione **Preferências**.
2. Selecione **Resolução**.
3. Selecione **+** .
4. Insira altura e largura da resolução e selecione **OK.**

Para excluir a resolução, selecione-a e, sem seguida, selecione **-** .

## <a name="displays-have-separate-spaces"></a>Monitores têm espaços separados

Se estiver executando o Mac OS X 10.9 e tiver desabilitado **Monitores têm espaços separados** no Mavericks (**Preferências do Sistema > Controle de Missão**), você precisará definir essa configuração no cliente de Área de Trabalho Remota usando a mesma opção.

### <a name="drive-redirection-for-remote-resources"></a>Redirecionamento de unidade para recursos remotos

O redirecionamento de unidade tem suporte para recursos remotos, para que você possa salvar os arquivos criados com um aplicativo remoto localmente no seu Mac. A pasta redirecionada é sempre seu diretório inicial exibido como uma unidade de rede na sessão remota.

> [!NOTE]
> Para usar esse recurso, o administrador precisa definir as configurações apropriadas no servidor.

## <a name="use-a-keyboard-in-a-remote-session"></a>Usar um teclado em uma sessão remota

Os layouts de teclado do Mac são diferentes dos layouts de teclado do Windows.

- A tecla Command no teclado do Mac é igual à tecla Windows.
- Para executar ações que usam o botão Command no Mac, você precisará usar o botão Ctrl no Windows (por exemplo, Copiar = Ctrl+C).
- As teclas de função podem ser ativadas na sessão pressionando a tecla FN (por exemplo, FN+F1).
- A tecla Alt à direita da barra de espaço no teclado Mac é igual à tecla Alt Gr/Alt à direita no Windows.

Por padrão, a sessão remota usará a mesma localidade do teclado do sistema operacional no qual você estiver executando o cliente. (Caso seu Mac esteja executando um sistema operacional em inglês dos EUA, o idioma também será usado nas sessões remotas.) Caso a localidade de teclado do sistema operacional não seja usada, verifique a configuração de teclado no computador remoto e altere-a manualmente. Confira as [Perguntas Frequentes de Cliente de Área de Trabalho Remota](remote-desktop-client-faq.md) para obter mais informações sobre teclados e localidades.

## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Suporte para autorização e autenticação conectável do gateway de Área de Trabalho Remota

O Windows Server 2012 R2 apresentou o suporte para um novo método de autenticação, a autenticação conectável do Gateway de Área de Trabalho Remota e autorização, que oferece mais flexibilidade para rotinas de autenticação personalizada. Agora, você pode experimentar esse modelo de autenticação com o cliente Mac.

> [!IMPORTANT]
> Os modelos personalizados de autenticação e autorização antes do Windows 8.1 não têm suporte, embora o artigo acima fale sobre eles.

Para saber mais sobre esse recurso, confira [https://aka.ms/paa-sample](https://aka.ms/paa-sample).

> [!TIP]
> Perguntas e comentários são sempre bem-vindos. No entanto, NÃO publique uma solicitação de ajuda com solução de problemas usando o recurso de comentários no final deste artigo. Em vez disso, vá para o [Fórum de cliente da Área de Trabalho Remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicie uma nova conversa. Tem alguma sugestão de recurso? Conte-nos no [Fórum de voz do usuário cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
