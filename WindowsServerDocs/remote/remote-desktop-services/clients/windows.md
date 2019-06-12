---
title: Comece com a área de trabalho remota no Windows
description: Basic etapas de configuração para o cliente de área de trabalho remota para Windows.
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
ms.date: 05/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: b2141a6a57629ccbb585b2f74c581eba5ba2b1b1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446679"
---
# <a name="get-started-with-remote-desktop-on-windows"></a>Comece com a área de trabalho remota no Windows

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de área de trabalho remota para Windows para trabalhar com aplicativos do Windows e áreas de trabalho remotamente de um dispositivo diferente do Windows.

Use as seguintes informações para começar a usar. Não se esqueça de conferir o [perguntas frequentes sobre](remote-desktop-client-faq.md) se você tiver alguma dúvida.

> [!NOTE]
> - Curioso sobre novas versões para o cliente do Windows? Fazer check-out [o que há de novo para a área de trabalho remota no Windows?](windows-whatsnew.md)
> - Você pode executar o cliente em qualquer versão do Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obter o cliente de área de trabalho remota e começar a usá-lo

Siga estas etapas para começar com a área de trabalho remota em seu dispositivo Windows 10:

1. Baixe o cliente de área de trabalho remota do [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de área de trabalho remota ou um recurso remoto. Usar uma conexão para se conectar diretamente a um computador de Windows e um recurso remoto para usar um programa RemoteApp, com base em sessão de área de trabalho ou a área de trabalho virtual é publicado pelo seu administrador. 
4. Fixar itens para que você possa obter rapidamente para a área de trabalho remota.

### <a name="add-a-remote-desktop-connection"></a>Adicionar uma conexão de área de trabalho remota

Para criar uma conexão de área de trabalho remota:

1. No tap Centro de Conexão **+ adicionar**e, em seguida, toque em **área de trabalho**.
2. Insira as seguintes informações para o computador que você deseja se conectar:
   - **Nome do PC** – o nome do computador. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode anexar informações de porta ao nome do computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Conta de usuário** – a conta de usuário a ser usada para acessar o computador remoto. Toque **+** para adicionar uma nova conta ou selecione uma conta existente. Você pode usar os seguintes formatos para o nome de usuário: *user_name*, *domain\user_name*, ou <em>user_name@domain.com</em>. Você também pode especificar se deseja solicitar um nome de usuário e senha durante a conexão, selecionando **Perguntar sempre**.
3. Você também pode definir opções adicionais tocando no **Mostrar mais**:
   - **Nome de exibição** – um nome fácil de lembrar para o PC que você está se conectando. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador é exibido.
   - **Grupo** – especificar um grupo para torná-lo mais fácil de encontrar suas conexões mais tarde. Você pode adicionar um novo grupo tocando **+** ou selecione um na lista.
   - **Gateway** – gateway de área de trabalho remota a que você deseja usar para se conectar a áreas de trabalho virtuais, programas RemoteApp e áreas de trabalho baseadas em sessão em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
   - **Conectar-se à sessão de administrador** -Use essa opção para se conectar a uma sessão de console para administrar um servidor Windows.
   - **Trocar os botões do mouse** – Use essa opção, a troca de funções do botão esquerdo do mouse para o botão direito do mouse. (Isso é especialmente útil se o computador remoto está configurado para um usuário canhoto, mas usar um mouse destro.)
   - **Defina a resolução da sessão remota:** – selecione a resolução que você deseja usar na sessão. **Escolha para mim** definirá a resolução com base no tamanho do cliente.
   - **Alterar o tamanho da tela:** – ao selecionar uma resolução alta de estática para a sessão, você tem a opção para exibir itens na tela maiores para melhorar a legibilidade. Observação: Isso se aplica somente ao conectar-se ao Windows 8.1 ou superior.
   - **Atualizar a resolução de sessão remota no redimensionamento** – quando habilitado, o cliente atualizar dinamicamente a resolução de sessão com base no tamanho do cliente. Observação: Isso se aplica somente ao conectar-se ao Windows 8.1 ou superior.
   - **Área de transferência** – quando habilitada, permite que você copie o texto e imagens para/do PC remoto.
   - **Reprodução de áudio** – selecione o dispositivo a ser usado para áudio durante sua sessão remota. Você pode optar por reproduzir som nos dispositivos locais, o computador remoto, ou não de forma alguma.
   - **Gravação de áudio** – quando habilitada, permite que você use um microfone local com aplicativos no PC remoto.
4. Toque **salvar**.

É necessário editar essas configurações? Toque no menu de estouro ( **...** ) ao lado do nome da área de trabalho e, em seguida, toque **editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro ( **...** ) e, em seguida, toque **remover**.

### <a name="add-a-remote-resource"></a>Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho baseadas em sessão e áreas de trabalho virtuais publicadas pelo seu administrador usando os serviços de área de trabalho remota.

Para adicionar um recurso remoto:

1. Na tela do Centro de Conexão, toque **+ adicionar**e, em seguida, toque em **recursos remotos**. 
2. Insira o **URL do Feed** fornecidas pelo seu administrador e toque **encontrar feeds**.
3. Quando solicitado, forneça as credenciais a usar para assinar o feed.

Os recursos remotos serão exibidos no centro da Conexão.


Para excluir os recursos remotos:

1. No Centro de Conexão, toque no menu de estouro ( **...** ) ao lado do recurso remoto.
2. Toque **remover**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Fixar uma área de trabalho salva ao menu Iniciar

Para fixar uma conexão ao menu Iniciar, toque no menu de estouro ( **...** ) ao lado do nome da área de trabalho e, em seguida, toque **Fixar na tela inicial**.

Agora você pode iniciar a conexão de área de trabalho remota diretamente do seu menu Iniciar tocando-lo.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectar-se a um Gateway de área de trabalho remota para acessar os ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de área de trabalho remota.

Para configurar um novo gateway:

1. No Centro de Conexão, toque em **configurações**.
2. Ao lado de Gateway, toque em **+** para adicionar um novo gateway. Observação: Um gateway também pode ser adicionado ao adicionar uma nova conexão.
3. Insira as seguintes informações:
   - **Nome do servidor** – o nome do computador que você deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio da Internet ou um endereço IP. Você também pode adicionar informações de porta ao nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
   - **Conta de usuário** - selecionar ou adicionar uma conta de usuário para usar com o Gateway de área de trabalho remota estiver se conectando a. Você também pode selecionar **usar conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de área de trabalho remota.
4. Toque **salvar**.  

## <a name="global-app-settings"></a>Configurações de aplicativo global

Você pode definir as seguintes configurações globais no seu cliente tocando **configurações**:

GERENCIADO ITENS
- **Conta de usuário** - lhe permite adicionar, editar e excluir contas de usuário salvas no cliente. Isso é uma boa maneira de atualizar a senha para uma conta após ele ter sido alterado.
- **Gateway** - lhe permite adicionar, editar e excluir servidores de gateway salvos no cliente.
- **Grupo** - lhe permite adicionar, editar e excluir grupos salvos no cliente. Eles permitem que você facilmente conexões do grupo.

CONFIGURAÇÕES DE SESSÃO
- **Iniciar conexões em tela inteira** -quando habilitada, sempre que uma conexão é iniciado, o cliente usará toda a tela do monitor atual.
- **Iniciar cada conexão em uma nova janela** – quando habilitado, cada conexão é iniciado em uma janela separada, permitindo que você coloque em monitores diferentes e alternar entre eles usando a barra de tarefas.
- **Ao redimensionar o aplicativo:** -permite que você controle sobre o que acontece quando a janela do cliente é redimensionada. O padrão é **ampliar o conteúdo, preservando a taxa de proporção**.
- **Usar comandos de teclado com:** -permite especificar onde os comandos de teclado, como *WIN* ou *ALT + TAB* são usados. O padrão é enviá-las apenas a sessão quando a conexão está em tela inteira.
- **Impedir que a tela atingir o tempo limite** -permite que você mantenha a tela de atingir o tempo limite quando uma sessão estiver ativa. Isso é útil quando a conexão não exige qualquer interação por longos períodos de tempo.

CONFIGURAÇÕES DO APLICATIVO
- **Mostrar área de trabalho visualizações** -permite que você veja uma visualização de uma área de trabalho no centro da Conexão antes de se conectar a ele. Por padrão, isso é definido como **em**.
- **Ajude a aprimorar a área de trabalho remota** -envia dados anônimos à Microsoft. Usamos esses dados para melhorar o cliente. Para saber mais sobre como tratamos esses dados anônimos, privados, consulte o [declaração de privacidade do Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Por padrão, essa configuração é **em**.

### <a name="manage-your-user-accounts"></a>Gerenciar suas contas de usuário

Quando você se conectar a um recurso de área de trabalho ou remoto, você pode salvar as contas de usuário para selecionar de novamente. Você também pode definir contas de usuário no próprio cliente, em vez de salvar os dados de usuário quando você se conecta a uma área de trabalho.

Para criar uma nova conta de usuário:

1. No Centro de Conexão, toque em **configurações**.
2. Ao lado da conta de usuário, toque em **+** para adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** -o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: domínio \ nome_de_usuário, user_name ou user_name@domain.com.
   - **Senha** -a senha para o usuário especificado. Isso pode ser deixado em branco para ser solicitado a fornecer uma senha durante a conexão.
4. Toque **salvar**.

Para excluir uma conta de usuário:

1. No Centro de Conexão, toque em **configurações**.
2. Selecione a conta a ser excluída da lista sob a conta de usuário.
3. Ao lado da conta de usuário, toque no ícone de edição.
4. Toque **remover esta conta** na parte inferior para excluir a conta de usuário.
5. Você também pode editar a conta de usuário e toque **salvar**.

## <a name="navigate-the-remote-desktop-session"></a>Navegue a sessão de área de trabalho remota
Quando você inicia uma conexão de área de trabalho remota, existem ferramentas disponíveis que você pode usar para navegar a sessão.

### <a name="start-a-remote-desktop-connection"></a>Iniciar uma conexão de área de trabalho remota

1. Toque a conexão de área de trabalho remota para iniciar a sessão.
2. Se você ainda não salvou as credenciais para a conexão, você será solicitado a fornecer um **nome de usuário** e **senha**.
3. Se você for solicitado a verificar o certificado para a área de trabalho remota, examine as informações e garantir que este é um PC confiável antes de tocar **Connect**. Você também pode selecionar **não pergunte novamente sobre o certificado** por sempre aceitar este certificado.

### <a name="connection-bar"></a>Barra de Conexão

A conexão barra lhe dê que acesso para controles de navegação adicionais. Por padrão, a barra de conexão é colocada no meio na parte superior da tela. Toque e arraste a barra para a esquerda ou direita para movê-lo.

- **Controle de panorâmica** -o controle de panorâmica permite que a tela ser ampliados e movidos. Observe que o controle panorâmico só está disponível em dispositivos sensíveis ao toque e usando o modo de toque direto.
   - Habilitar / desabilitar o controle de Panorâmica: Toque no ícone de bandeja na barra de conexão para exibir o controle de Panorâmica e zoom a tela. Toque no ícone de bandeja na barra de conexão novamente para ocultar o controle e retornar a tela para a resolução original.
   - Usar o controle panorâmico - Tap, mantenha o controle de Panorâmica e, em seguida, arraste na direção em que você deseja mover a tela.
   - Mova o controle panorâmico - toque duplo e mantenha o controle panorâmico para mover o controle na tela.
- **Opções adicionais** -toque no ícone de opções adicionais para exibir a seleção de sessão da barra e comando da barra (veja abaixo).
- **Teclado** -toque no ícone de teclado para exibir ou ocultar o teclado na tela. O controle panorâmico é exibido automaticamente quando o teclado é exibido.

### <a name="command-bar"></a>Barra de comandos

Toque o **...**  na barra de conexão para exibir a barra de comandos no lado direito da tela.

- **Página inicial** -Use o botão Home para retornar para o Centro de conexão na barra de comandos.
  - Como alternativa, você pode usar o botão Voltar para a mesma ação.
  - Sua sessão ativa não será desconectada.
  - Isso permite que você inicie conexões adicionais.
- **Desconectar** -usar o botão Desconectar para encerrar a conexão.
  - Seus aplicativos permanecerá ativos desde que a sessão não será encerrada no PC remoto.
- **Tela inteira** - entra ou sai do modo de tela inteira.
- **Tocar / Mouse** -você pode alternar entre os modos de mouse (toque direto e ponteiro do Mouse).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso direto de toque gestos e modos de mouse em uma sessão remota

Dois modos de mouse estão disponíveis para interagir com a sessão.
- **Direcionar o toque**: Passa todos os contatos de toque para a sessão deve ser interpretado remotamente.
  - Usado da mesma forma que você usaria o Windows com uma tela sensível ao toque.
- **Ponteiro do mouse**: Transforma sua tela sensível ao toque local em uma grande touchpad permitindo para mover o ponteiro do mouse na sessão.
  - Usado da mesma forma que você usaria o Windows com um touchpad.

> [!NOTE]
> Interação com o Windows 8 ou mais recente de gestos de toque nativos têm suporte no modo de toque direto. 

| Modo de mouse    | Operação de mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Com o botão esquerdo           | toque de 1 dedo                                                          |
| Toque direto  | Clique com o botão direito do mouse          | 1 de toque de dedo e manter pressionado                                                 |
| Ponteiro do mouse | Com o botão esquerdo           | toque de 1 dedo                                                          |
| Ponteiro do mouse | Clicar com o botão esquerdo e arrastar  | toque duplo de 1 dedo e manter pressionado, arraste                               |
| Ponteiro do mouse | Clique com o botão direito do mouse          | toque de dedo 2                                                          |
| Ponteiro do mouse | Clicar com o botão direito e arrastar | 2 toque duplo de dedo e manter pressionado, arraste                               |
| Ponteiro do mouse | Roda do mouse          | 2 dedo toque e mantenha pressionado e arrastar para cima ou para baixo                           |
| Ponteiro do mouse | Zoom                 | Use os 2 dedos e pinçar para ampliar ou mover entre os dedos para diminuir o zoom. |

> [!TIP]
> Perguntas e comentários são sempre bem-vindas. No entanto, não poste uma solicitação de ajuda de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum de cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo thread. Tem alguma sugestão de recurso? Conte-nos usando a [Hub de comentários](feedback-hub://?tabid=2&contextid=605).
