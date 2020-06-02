---
title: Introdução ao cliente da Área de Trabalho do Windows
description: Informações básicas sobre o cliente da Área de Trabalho do Windows.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 05/26/2020
ms.localizationpriority: medium
ms.openlocfilehash: c229eefbc0cc00ed1af940cd986c89e979873d29
ms.sourcegitcommit: 4fec7d82f0772d03a9e8cac20092a4309b0f796e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84025497"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introdução ao cliente da Área de Trabalho do Windows

>Aplica-se a: Windows 10, Windows 10 IoT Enterprise e Windows 7

É possível usar o cliente da Área de Trabalho Remota para que a Área de Trabalho do Windows acesse áreas de trabalho e aplicativos do Windows usando outro dispositivo Windows.

> [!NOTE]
> - Esta documentação não é para o cliente da Conexão de Área de Trabalho Remota (MSTSC) fornecido com o Windows. É para o novo cliente da Área de Trabalho Remota (MSRDC).
> - No momento, esse cliente só dá suporte ao acesso a aplicativos e áreas de trabalho remotos da [Área de Trabalho Virtual do Windows](https://aka.ms/wvd).
> - Curioso sobre as novas versões para o cliente da Área de Trabalho do Windows? Confira as [Novidades do cliente de Área de Trabalho do Windows](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>Instalar o cliente

Escolha o cliente que corresponde à versão do Windows. O novo cliente de Área de Trabalho Remota (MSRDC) dá suporte a dispositivos Windows 10, Windows 10 IoT Enterprise e Windows 7.

- [Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32 bits](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

É possível instalar o cliente do usuário atual, que não requer direitos de administrador, ou seu administrador pode instalar e configurar o cliente para que todos os usuários no dispositivo possam acessá-lo.

Depois de instalar o cliente, será possível iniciá-lo por meio do menu Iniciar pesquisando por **Área de Trabalho Remota**.

## <a name="update-the-client"></a>Atualizar o cliente

Você será notificado sempre que uma nova versão do cliente estiver disponível, desde que seu administrador não tenha desabilitado as notificações. A notificação será exibida na Central de Conexões ou na Central de Ações do Windows. Para atualizar o cliente, basta selecionar a notificação.

Também é possível pesquisar manualmente novas atualizações para o cliente:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) na barra de comandos na parte superior do cliente.
2. Selecione **Sobre** no menu suspenso.
3. O cliente procura atualizações automaticamente.
4. Se houver uma atualização disponível, toque em **Instalar atualização** para atualizar o cliente.

## <a name="workspaces"></a>Workspaces

Obtenha a lista de recursos gerenciados que você pode acessar, como aplicativos e áreas de trabalho, assinando o Workspace que seu administrador forneceu. Quando você assina, os recursos são disponibilizados no PC local. No momento, o cliente da Área de Trabalho do Windows dá suporte a recursos publicados na Área de Trabalho Virtual do Windows.

### <a name="subscribe-to-a-workspace"></a>Assinar um Workspace

Há duas maneiras de assinar um workspace. O cliente pode tentar descobrir os recursos disponíveis para você por meio da sua conta corporativa ou de estudante, ou, caso ele não consiga encontrá-los, você pode especificar diretamente a URL onde os recursos estão. Depois da assinatura de um workspace, é possível iniciar recursos com um dos seguintes métodos:

- Acesse a Central de Conexão e clique duas vezes em um recurso para iniciá-lo.
- Também é possível acessar o menu Iniciar e procurar uma pasta com o nome do Workspace ou inserir o nome do recurso na barra de pesquisa.

#### <a name="subscribe-with-a-user-account"></a>Assinar com uma conta de usuário

1. Na página principal do cliente, toque em **Assinar**.
2. Entre com sua conta quando solicitado.
3. Os recursos serão exibidos na Central de Conexão agrupados por Workspace.

#### <a name="subscribe-with-url"></a>Assinar com URL

1. Na página principal do cliente, toque em **Assinar com URL**.
2. Insira a URL do workspace ou o endereço de email:
   - Se você usar a **URL do workspace**, use aquela que o administrador lhe forneceu. Se estiver acessando recursos da Área de Trabalho Virtual do Windows, será possível usar uma das seguintes URLs:
     - Área de Trabalho Virtual do Windows no outono de 2019: `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Área de Trabalho Virtual do Windows na primavera de 2020: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Para usar o **email**, insira o seu. Isso instrui o cliente a procurar uma URL associada ao endereço de email caso o administrador tenha configurado a [descoberta de email](../rds-email-discovery.md).
3. Toque em **Avançar**.
4. Entre com sua conta quando solicitado.
5. Os recursos serão exibidos na Central de Conexão agrupados por Workspace.

### <a name="workspace-details"></a>Detalhes do Workspace

Após assinar, será possível exibir outras informações sobre um Workspace no painel Detalhes:

- O nome do Workspace
- A URL e o nome de usuário usados para assinar
- O número de aplicativos e áreas de trabalho
- A data/hora da última atualização
- O status da última atualização

Acessar o painel Detalhes:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do Workspace.
2. Selecione **Detalhes** no menu suspenso.
3. O painel Detalhes é exibido no lado direito do cliente.

Depois de assinar, o Workspace será atualizado automaticamente de maneira regular. Os recursos podem ser adicionados, alterados ou removidos com base nas alterações realizadas pelo seu administrador.

Também é possível procurar manualmente atualizações para os recursos, quando necessário, selecionando **Atualizar** no painel Detalhes.

### <a name="refreshing-a-workspace"></a>Atualizando um Workspace

Você pode atualizar manualmente um Workspace selecionando **Atualizar** no menu de estouro ( **...** ) ao lado do Workspace.

### <a name="unsubscribe-from-a-workspace"></a>Cancelar a assinatura de um Workspace

Esta seção ensinará como cancelar a assinatura de um Workspace. É possível cancelar a assinatura para assinar novamente com uma conta diferente ou remover seus recursos do sistema.

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do Workspace.
2. Selecione **Cancelar assinatura** no menu suspenso.
3. Examine a caixa de diálogo e selecione **Continuar**.

## <a name="managed-desktops"></a>Áreas de trabalho gerenciadas

Os workspaces podem conter vários recursos gerenciados, incluindo áreas de trabalho. Ao acessar uma área de trabalho gerenciada, você tem acesso a todos os aplicativos instalados por seu administrador.

### <a name="desktop-settings"></a>Configurações da área de trabalho

É possível definir algumas das configurações para recursos da área de trabalho a fim de garantir que a experiência atenda às suas necessidades. Para acessar a lista de configurações disponíveis, clique com o botão direito do mouse no recurso da área de trabalho e selecione **Configurações**.

O cliente usará as configurações definidas pelo administrador, a menos que você desative a opção **Usar configurações padrão**. Isso permite que você configure as seguintes opções:

- **Exibir configuração** seleciona quais telas usar para a sessão de área de trabalho e afeta quais configurações adicionais estão disponíveis.
  - **Todas as telas** garante que a sessão sempre use todas as suas telas locais, mesmo quando algumas delas forem adicionadas ou removidas mais tarde.
  - **Tela única** garante que a sessão sempre use uma única tela e permite que você configure suas propriedades.
  - **Selecionar telas** permite que você escolha quais telas usar para a sessão e fornece uma opção para alterar dinamicamente a lista de telas durante a sessão.
- **Selecionar as exibições a serem usadas para a sessão** especifica quais exibições locais usar para a sessão. Todas as exibições selecionadas devem ser adjacentes umas às outras. Essa configuração só está disponível no modo **Selecionar tela**.
- **Maximizar para telas atuais** determina quais telas as sessões usarão ao passar para tela inteira. Quando habilitada, a sessão é exibida em tela inteira nas exibições tocadas pela janela da sessão. Isso permite que você altere as telas durante a sessão. Quando desabilitada, a sessão é exibida em tela inteira na mesma tela em que estava na última vez em que esteve em tela inteira. Essa configuração só está disponível no modo **Selecionar tela**, e será desabilitada em outro caso.
- **Exibição única quando em janela** determina quais telas ficam disponíveis na sessão ao sair da tela inteira. Quando habilitada, a sessão alterna para uma única tela no modo de janela. Quando desabilitada, a sessão retém as mesmas telas no modo de janela, como na tela inteira. Essa configuração só está disponível nos modos **Todas as telas** e **Selecionar tela**, e será desabilitada em outros casos.
- **Iniciar em tela inteira** determina se a sessão será iniciada no modo de tela inteira ou de janela. Essa configuração só está disponível no modo **Tela única** e será desabilitada em outros casos.
- **Ajustar a sessão à janela** determina como a sessão é exibida quando a resolução da área de trabalho remota difere do tamanho da janela local. Quando habilitado, o conteúdo da sessão será redimensionado para se ajustar dentro da janela enquanto preserva a taxa de proporção da sessão. Quando desabilitadas, as barras de rolagem ou as áreas pretas serão mostradas quando a resolução e o tamanho da janela não corresponderem. Essa configuração está disponível em todos os modos.
- **Atualizar a resolução ao redimensionar** faz com que a resolução de área de trabalho remota seja atualizada automaticamente quando você redimensiona a sessão no modo de janela. Quando desabilitada, a sessão sempre permanece na resolução especificada em **Resolução**. Essa configuração só está disponível no modo **Tela única** e será desabilitada em outros casos.
- A **Resolução** permite que você especifique a resolução da área de trabalho remota. A sessão reterá essa resolução por toda a duração. Essa configuração só está disponível no modo **Tela única** e quando **Atualizar a resolução ao redimensionar** está desabilitado.
- **Alterar o tamanho do texto e dos aplicativos** especifica o tamanho do conteúdo da sessão. Essa configuração se aplica somente ao se conectar ao Windows 8.1 e posterior ou ao Windows Server 2012 R2 e posterior. Essa configuração só está disponível no modo **Tela única** e quando **Atualizar a resolução ao redimensionar** está desabilitado.

## <a name="provide-feedback"></a>Fornecer comentários

Tem uma sugestão de recursos ou deseja relatar um problema? Conte-nos usando o [Hub de Comentários](feedback-hub://?tabid=2&contextid=883). Você também pode acessar o Hub de Feedback por meio do cliente:

1. Na Central de Conexões, toque na opção **Enviar comentários** na barra de comandos na parte superior do cliente para abrir o aplicativo Hub de Feedback.
2. Insira as informações necessárias nos campos **Resumo** e **Detalhes**. Quando terminar, toque em **Avançar**.
3. Selecione a opção **Problema** ou **Sugestão**.
4. Verifique se a categoria está em **Aplicativos** > **Área de Trabalho Remota**. Em caso afirmativo, toque em **Avançar**.
5. Examine os tópicos de comentários existentes para ver se outra pessoa relatou o mesmo problema. Caso contrário, selecione **Criar um bug** e, em seguida, toque em **Avançar**.
6. Na próxima página, você poderá nos fornecer mais informações para ajudarmos a resolver seu problema. Você pode escrever informações mais detalhadas, enviar capturas de tela e até mesmo criar uma gravação do problema para nos mostrar o que aconteceu. Para fazer uma gravação, selecione **Iniciar a gravação** e, em seguida, repita o que você fez até o ponto em que o problema ocorreu. Quando terminar, retorne ao Hub de Feedback e selecione **Parar a gravação**.
7. Quando estiver satisfeito com as informações, toque em **Enviar**.
8. Na página “Agradecemos seus comentários!”, toque em **Compartilhar meus comentários** para gerar um link para seus comentários, que poderá ser compartilhado com outras pessoas, conforme necessário.

### <a name="access-client-logs"></a>Acessar logs do cliente

Talvez você precise dos logs do cliente ao investigar um problema.

Para recuperar os logs do cliente:

1. Garanta que nenhuma sessão esteja ativa e que o processo do cliente não esteja em execução em segundo plano clicando com o botão direito do mouse no ícone de **Área de Trabalho Remota** na bandeja do sistema e selecionando **Desconectar todas as sessões**.
2. Abra o **Explorador de Arquivos**.
3. Navegue até a pasta **%temp%\DiagOutputDir\RdClientAutoTrace**.
