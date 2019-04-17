---
title: Introdução a área de trabalho remota no Windows
description: Basic configurar etapas para o cliente de área de trabalho remota para o Windows.
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
ms.openlocfilehash: 1c06e2eca725a825ac0fa4c7b617a26d89c898ff
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297451"
---
# Introdução a área de trabalho remota no Windows

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar o cliente de área de trabalho remota para o Windows para trabalhar com aplicativos do Windows e áreas de trabalho remotamente de um dispositivo diferente do Windows.

Use as seguintes informações para começar. Certifique-se de fazer check-out a [perguntas Frequentes](remote-desktop-client-faq.md) , se você tiver dúvidas.

> [!NOTE]
> - Curioso sobre as novas versões de cliente do Windows? Confira [Novidades para área de trabalho remota no Windows?](windows-whatsnew.md)
> - Você pode executar o cliente em qualquer versão do Windows 10.

## Obter o cliente de área de trabalho remota e começar a usá-lo

Siga estas etapas para começar com a área de trabalho remota no seu dispositivo Windows 10:

1. Baixe o cliente de área de trabalho remota da [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurar seu computador para aceitar conexões remotas](remote-desktop-allow-access.md).
3. Adicione uma conexão de área de trabalho remota ou um recurso remoto. Você usa uma conexão para se conectar diretamente a um computador com Windows e um recurso remoto para usar um programa RemoteApp, com base em sessão de área de trabalho ou área de trabalho virtual publicados por seu administrador. 
4. Fixar itens, portanto, você pode obter rapidamente a área de trabalho remota.

### Adicionar uma conexão de área de trabalho remota

Para criar uma conexão de área de trabalho remota:

1. No Centro de Conexão toque **+ Adicionar**e, em **área de trabalho**.
2. Insira as seguintes informações para o computador que você deseja se conectar:
  - **Nome do computador** – o nome do computador. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome de computador (por exemplo, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Conta de usuário** – a conta de usuário para usar para acessar o computador remoto. Toque em **+** para adicionar uma nova conta ou selecionar uma conta existente. Você pode usar os formatos a seguir para o nome de usuário: *user_name@domain.com*, *domínio \ nome_do_usuário*ou *nome_do_usuário*. Você também pode especificar se deseja solicitar um nome de usuário e senha durante a conexão selecionando **Sempre perguntar**.
3. Você também pode definir opções adicionais tocar em **Mostrar mais**:
  - **Nome de exibição** – um nome para o computador que você está se conectando fácil de lembrar. Você pode usar qualquer cadeia de caracteres, mas se você não especificar um nome amigável, o nome do computador é exibido.
  - **Grupo** – especifique um grupo para tornar mais fácil encontrar as conexões mais tarde. Você pode adicionar um novo grupo, tocando em **+** ou selecione um da lista.
  - **Gateway** – gateway a área de trabalho remota que você deseja usar para conectar a áreas de trabalho virtuais, programas do RemoteApp e áreas de trabalho da sessão com base em uma rede corporativa interna. Obtenha as informações sobre o gateway do administrador do sistema.
  - **Conectar-se a sessão do administrador** - Use essa opção para se conectar a uma sessão de console para administrar um servidor do Windows.
  - **Botões do mouse troca** – Use esta opção para a troca de funções de botão esquerdo do mouse para o botão direito do mouse. (Isso é especialmente útil se você usar um mouse à direita, mas o computador remoto está configurado para um usuário à esquerda.)
  - **Definir a resolução da sessão remota:** – selecione a resolução que você deseja usar na sessão. **Escolha para mim** definirá a resolução com base no tamanho do cliente.
  - **Alterar o tamanho do vídeo:** – ao selecionar uma alta resolução estática para a sessão, você tem a opção de fazer itens na tela pareçam maiores para melhorar a legibilidade. Observação: Isso se aplica somente ao se conectar ao Windows 8.1 ou superior.
  - **Atualizar a sessão remota resolução em Redimensionar** – quando habilitado, o cliente dinamicamente atualizará a resolução de sessão com base no tamanho do cliente. Observação: Isso se aplica somente ao se conectar ao Windows 8.1 ou superior.
  - **Área de transferência** – quando habilitado, permite que você copie o texto e imagens de/para o computador remoto.
  - **Reprodução de áudio** – selecione o dispositivo a ser usado para áudio durante a sessão remota. Você pode optar por reproduzir um som em dispositivos locais, o computador remoto, ou não em todos.
  - **Gravação de áudio** – quando habilitado, permite que você use um microfone local com aplicativos no computador remoto.
4. Toque em **Salvar**.

Você precisa editar essas configurações? Toque no menu de estouro (**...**) ao lado do nome da área de trabalho e, em **Editar**.

Deseja excluir a conexão? Novamente, toque no menu de estouro (**...**) e, em seguida, toque em **Remover**.

### Adicionar um recurso remoto
Recursos remotos são programas RemoteApp, áreas de trabalho com base em sessão e áreas de trabalho virtuais publicadas por seu administrador usando os serviços de área de trabalho remota.

Para adicionar um recurso remoto:

1. Na tela de Conexão central, toque **+ Adicionar**e, em **recursos remotos**. 
2. Insira a **URL do Feed** fornecida pelo seu administrador e toque em **Localizar feeds**.
3. Quando solicitado, forneça as credenciais para usar para assinar o feed.

Os recursos remotos serão exibidos no centro da Conexão.


Para excluir recursos remotos:

1. Na Central de Conexão, toque no menu de estouro (**...**) ao lado do recurso remoto.
2. Toque em **Remover**.

### Fixar uma área de trabalho salva ao menu Iniciar

Para fixar uma conexão ao menu Iniciar, toque no menu de estouro (**...**) ao lado do nome da área de trabalho e, em seguida, toque em **Fixar em Iniciar**.

Agora você pode iniciar a conexão de área de trabalho remota diretamente do menu Iniciar tocando-lo.

## Conectar a um Gateway de área de trabalho remota para acessar ativos internos

Um Gateway de área de trabalho remota (Gateway RD) permite que você se conectar a um computador remoto em uma rede corporativa de qualquer lugar na Internet. Você pode criar e gerenciar seus gateways usando o cliente de área de trabalho remota.

Para configurar um novo gateway:

1. Na Central de Conexão, toque em **configurações**.
2. Ao lado de Gateway, toque em **+** adicionar um novo gateway. Observação: Um gateway também pode ser adicionado ao adicionar uma nova conexão.
3. Insira as seguintes informações:
  - **Nome do servidor** – o nome do computador que deseja usar como um gateway. Isso pode ser um nome de computador do Windows, um nome de domínio de Internet ou um endereço IP. Você também pode adicionar informações de porta para o nome do servidor (por exemplo: **RDGateway:443** ou **10.0.0.1:443**).
  - **Conta de usuário** - selecionar ou adicionar uma conta de usuário para usar com o Gateway de área de trabalho remota você está se conectando. Você também pode selecionar **usar a conta de usuário da área de trabalho** para usar as mesmas credenciais usadas para a conexão de área de trabalho remota.
4. Toque em **Salvar**.  

## Configurações de aplicativo global

Você pode definir as seguintes configurações globais no seu cliente tocando **configurações**:

GERENCIADO ITENS
- **Conta de usuário** - permite que você adicionar, editar e excluir contas de usuário salvas no cliente. Isso é uma boa maneira de atualizar a senha de uma conta depois que ele foi alterado.
- **Gateway** – permite que você adicionar, editar e excluir os servidores de gateway salvos no cliente.
- **Grupo** - permite que você adicionar, editar e excluir grupos salvos no cliente. Elas permitem que você facilmente conexões do grupo.

CONFIGURAÇÕES DE SESSÃO
- **Iniciar conexões em tela inteira** , quando ativado, sempre que uma conexão é iniciado, o cliente usará toda a tela do monitor atual.
- **Iniciar cada conexão em uma nova janela** , quando habilitado, cada conexão é iniciado em uma janela separada, permitindo que você colocá-los em diferentes monitores e alternar entre eles usando a barra de tarefas.
- **Ao redimensionar o aplicativo:** -permite que você controle sobre o que acontece quando a janela do cliente é redimensionada. Padrões para **ampliar o conteúdo, mantendo a taxa de proporção**.
- **Usar comandos de teclado com:** -vamos você especificar onde os comandos de teclado como *GANHANDO* ou *ALT + TAB* são usados. O padrão é somente enviá-los para a sessão quando a conexão está em completa de serviços.
- **Impedir que a tela do tempo limite** - permite manter a tela do tempo limite quando uma sessão estiver ativa. Isso é útil quando a conexão não exige qualquer interação por longos períodos de tempo.

CONFIGURAÇÕES DO APLICATIVO
- **Mostrar visualizações de área de trabalho** - permite que você veja uma visualização de uma área de trabalho no centro da Conexão antes de se conectar a ele. Por padrão, isso é definido como **no**.
- **Ajudar a melhorar a área de trabalho remota** - envia dados anônimos à Microsoft. Nós usamos esses dados para melhorar o cliente. Para saber mais sobre como podemos tratar esses dados anônimos, particulares, consulte a [Declaração de privacidade da Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Por padrão, essa configuração é **no**.

### Gerenciar suas contas de usuário

Quando você se conecta aos recursos de um desktop ou controle remoto, você pode salvar as contas de usuário de novamente. Você também pode definir contas de usuário no cliente em si, em vez de salvar os dados do usuário quando você se conectar a uma área de trabalho.

Para criar uma nova conta de usuário:

1. Na Central de Conexão, toque em **configurações**.
2. Ao lado da conta de usuário, toque em **+** adicionar uma nova conta de usuário.
3. Insira as seguintes informações:
   - **Nome de usuário** - o nome do usuário para salvar para uso com uma conexão remota. Você pode inserir o nome de usuário em qualquer um dos seguintes formatos: nome_do_usuário, domínio \ nome_de_usuário, ou user_name@domain.com.
   - **Senha** - a senha para o usuário especificado. Isso pode ser deixado em branco para ser solicitado a fornecer uma senha durante a conexão.
4. Toque em **Salvar**.

Para excluir uma conta de usuário:

1. Na Central de Conexão, toque em **configurações**.
2. Selecione a conta para excluir da lista na conta de usuário.
3. Ao lado da conta de usuário, toque no ícone de edição.
4. Toque em **remover essa conta** na parte inferior para excluir a conta de usuário.
5. Você também pode editar a conta de usuário e toque em **Salvar**.

## Navegue a sessão de área de trabalho remota
Quando você inicia uma conexão de área de trabalho remota, há ferramentas disponíveis que podem ser usados para navegar a sessão.

### Iniciar uma conexão de área de trabalho remota

1. Toque a conexão de área de trabalho remota para iniciar a sessão.
2. Se você ainda não salvou as credenciais para a conexão, você será solicitado a fornecer um **nome de usuário** e **senha**.
3. Se você for solicitado a verificar o certificado para a área de trabalho remota, examine as informações e garantir que isso seja um computador confiável antes de tocar **Connect**. Você também pode selecionar **não perguntar sobre esse certificado novamente** sempre aceitar o certificado.

### Barra de Conexão

O fornece de barra de conexão que acesso a controles adicionais de navegação. Por padrão, a barra de conexão é colocada no meio na parte superior da tela. Tocar e arrastar a barra para a esquerda ou direita para movê-lo.

- **Panorâmica controle** - o controle de panorâmica permite que a tela seja ampliado e removidos. Observe que o controle panorâmico só está disponível em dispositivos habilitados para toque e usando o modo de toque direto.
   - Habilitar / desabilitar o controle de Panorâmica: Toque no ícone de movimento panorâmico na barra de conexão para exibir o controle de Panorâmica e zoom na tela. Toque no ícone de movimento panorâmico na barra de conexão novamente para ocultar o controle e retornar a tela à resolução original.
   - Use o controle de panorâmica - toque, mantenha o controle de movimento panorâmico e, em seguida, arraste na direção em que você deseja mover a tela.
   - Mova o controle pantoque duplo e mantenha o controle de movimento panorâmico para mover o controle na tela.
- **Opções adicionais** - toque no ícone de opções adicionais para exibir a seleção de sessão da barra e comando da barra (veja abaixo).
- **Teclado** - toque no ícone de teclado para exibir ou ocultar o teclado virtual. O controle de movimento panorâmico é exibido automaticamente quando o teclado é exibido.

### Barra de comandos

Toca o Stores **** na barra de conexão para exibir a barra de comandos no lado direito da tela.

- **Home** - Use o botão Home para retornar para o Centro de conexão da barra de comandos.
  - Como alternativa, você pode usar o botão Voltar para a mesma ação.
  - Sua sessão ativa não será desconectada.
  - Isso permite que você iniciar conexões adicionais.
- **Desconectar** - Use o botão Desconectar encerrar a conexão.
  - Seus aplicativos permanecerá ativos enquanto a sessão não é encerrada no computador remoto.
- **Tela inteira** - entra ou sai do modo de tela inteira.
- **Toque / Mouse** - você pode alternar entre os modos de mouse (toque direto e ponteiro do Mouse).

### Use diretos touch gestos e modos de mouse em uma sessão remota

Dois modos de mouse estão disponíveis para interagir com a sessão.
- **Toque direto**: passa em todos os contatos por toque para a sessão deve ser interpretado remotamente.
  - Usado da mesma maneira que você usaria o Windows com uma tela sensível ao toque.
- **Ponteiro do mouse**: transforma sua tela sensível ao toque local em um grande touchpad permitindo mover um ponteiro do mouse na sessão.
  - Usado da mesma maneira que você usaria são do Windows com um touchpad.

> [!NOTE]
> Interagir com o Windows 8 ou mais recente os gestos de toque nativos têm suporte no modo de toque direto. 

| Modo de mouse    | Operação de mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque direto  | Clique esquerdo           | toque com 1 dedos                                                          |
| Toque direto  | Clique com o botão direito do mouse          | 1 dedo tocar e segurar                                                 |
| Ponteiro do mouse | Clique esquerdo           | toque com 1 dedos                                                          |
| Ponteiro do mouse | Clicar com o botão esquerdo e arrastar  | toque duplo 1 dedo e segurar, arraste                               |
| Ponteiro do mouse | Clique com o botão direito do mouse          | toque com dedos 2                                                          |
| Ponteiro do mouse | Clicar com o botão direito e arrastar | 2 toque duplo dedo e segurar, arraste                               |
| Ponteiro do mouse | Roda do mouse          | 2 dedo toque pressionado e arraste para cima ou para baixo                           |
| Ponteiro do mouse | Zoom                 | Use os 2 dedos e pinçar para ampliar ou mover distanciando dedos para reduzir o zoom. |

> [!TIP]
> Perguntas e comentários sempre são boas-vindas. No entanto, não envie uma solicitação de solução de problemas usando o recurso de comentário no final deste artigo. Em vez disso, vá para o [Fórum do cliente de área de trabalho remota](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar um novo segmento. Tem uma sugestão de recurso? Conte-nos usando o [Hub de Feedback](feedback-hub://?tabid=2&contextid=605).
