---
title: Gerenciamento de Servidor de Políticas de Rede com Ferramentas de Administração
description: Você pode usar este tópico para saber mais sobre as ferramentas que você pode usar para gerenciar o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa1767e49b16f4a55f36e052d4354aaead540f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856877"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gerenciamento de Servidor de Políticas de Rede com Ferramentas de Administração

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as ferramentas que você pode usar para gerenciar seu NPSs.

Depois de instalar o NPS, você pode administrar NPSs:

- Localmente, usando o Console de gerenciamento Microsoft NPS \(MMC\) snap-in, o console do NPS estático em Ferramentas administrativas, comandos do Windows PowerShell ou o Shell de rede \(Netsh\) comandos do NPS.
- De um NPS remoto, usando o snap-in NPS MMC, os comandos Netsh para NPS, os comandos do Windows PowerShell para o NPS ou a Conexão de área de trabalho remota.
- De uma estação de trabalho remota, usando a Conexão de área de trabalho remota em combinação com outras ferramentas, como o NPS MMC ou o Windows PowerShell.

>[!NOTE]
>No Windows Server 2016, você pode gerenciar o NPS local usando o console do NPS. Para gerenciar NPSs locais e remotos, você deve usar o snap do MMC do NPS\-no.

As seções a seguir fornecem instruções sobre como gerenciar seu NPSs locais e remotos.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurar o NPS Local usando o Console do NPS

Depois de instalar o NPS, você pode usar este procedimento para gerenciar o NPS local usando o MMC do NPS.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Para configurar o NPS local usando o console do NPS

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.

2. No console do NPS, clique em NPS \(Local\). No painel de detalhes, escolha **configuração padrão** ou **Advanced Configuration**, e, em seguida, faça o seguinte com base em sua seleção:
    - Se você escolher **configuração padrão**, selecione um cenário na lista e, em seguida, siga as instruções para iniciar um Assistente de configuração.
    - Se você escolher **Advanced Configuration**, clique na seta para expandir **opções de configuração avançada**e, em seguida, analise e defina as opções disponíveis com base na funcionalidade NPS que você deseja - Servidor RADIUS, proxy RADIUS ou ambos.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Gerenciar vários NPSs, usando o NPS MMC Snap\-em

Você pode usar este procedimento para gerenciar o NPS local e várias NPSs remotos usando o NPS MMC snap\-no.

Antes de executar o procedimento a seguir, você deve instalar o NPS no computador local e em computadores remotos.

Dependendo das condições da rede e o número de NPSs gerenciar usando o NPS MMC snap\-na resposta de snap do MMC\-em pode ser lenta. Além disso, o tráfego de configuração do NPS é enviado pela rede durante uma sessão de administração remota usando o snap NPS\-no. Certifique-se de que sua rede está fisicamente segura e que usuários mal-intencionados não têm acesso a esse tráfego de rede.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Para gerenciar vários NPSs usando o snap NPS\-em

1. Para abrir o MMC, execute o Windows PowerShell como administrador. No Windows PowerShell, digite **mmc**, e pressione ENTER. O Console de Gerenciamento Microsoft será aberto.
2. No MMC, sobre o **arquivo** menu, clique em **Adicionar/Remover Snap\-em**. O **adicionar ou Remover Snap\-ins** caixa de diálogo é aberta.
3. Na **adicionar ou Remover Snap\-ins**, na **snap disponível\-ins**, role para baixo na lista, clique em **Network Policy Server**e, em seguida, clique em  **Adicionar**. A caixa de diálogo **Selecionar Computador** será aberta.
4. Na **Selecionar computador**, verifique **computador Local \(o computador no qual este console está sendo\)**  está selecionado e, em seguida, clique em **Okey**. O snap\-para o NPS local é adicionado à lista na **selecionado snap\-ins**.
5. Na **adicionar ou Remover Snap\-ins**, na **snap disponível\-ins**, certifique-se de que **Network Policy Server** ainda está selecionado e, em seguida, clique em  **Adicionar**. O **Selecionar computador** caixa de diálogo será aberta novamente.
6. Na **Selecionar computador**, clique em **outro computador**e, em seguida, digite o endereço IP ou nome de domínio totalmente qualificado \(FQDN\) de NPS remoto que você deseja gerenciar usando o NPS Encaixar\-no. Opcionalmente, você pode clicar em **procurar** para examinar o diretório para o computador que você deseja adicionar. Clique em **OK**.
7. Repita as etapas 5 e 6 para adicionar mais NPSs para o NPS snap\-no. Quando você tiver adicionado todos os NPSs de que você deseja gerenciar, clique em **Okey**.
8. Para salvar o snap-in do NPS para uso posterior, clique em **arquivo**e, em seguida, clique em **salvar**. No **Salvar como** caixa de diálogo, navegue até o local do disco rígido em que você deseja salvar o arquivo, digite um nome para o Console de gerenciamento Microsoft \(. msc\) de arquivo e, em seguida, clique em **salvar**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Gerenciar um NPS usando a Conexão de área de trabalho remota

Você pode usar este procedimento para gerenciar um NPS remoto usando a Conexão de área de trabalho remota.

Usando a Conexão de área de trabalho remota, você pode gerenciar remotamente seu NPSs executando o Windows Server 2016. Você pode gerenciar o NPSs também remotamente de um computador executando o Windows 10 ou sistemas de operacionais de cliente Windows anteriores.

Você pode usar a conexão de área de trabalho remota para gerenciar vários NPSs usando um dos dois métodos.

1. Crie uma conexão de área de trabalho remota para cada um dos seus NPSs individualmente.
2. Use a área de trabalho remota para conectar-se para um NPS e, em seguida, usar o NPS MMC nesse servidor para gerenciar outros servidores remotos. Para obter mais informações, consulte a seção anterior **gerenciar vários NPSs usando o MMC do NPS Snap\-em**.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores no NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Para gerenciar um NPS usando a Conexão de área de trabalho remota

1. Em cada NPS que você deseja gerenciar remotamente, no Gerenciador do servidor, selecione **servidor Local**. No painel de detalhes do Gerenciador do servidor, exibir o **área de trabalho remota** configuração e faça o seguinte. 
    1. Se o valor da **área de trabalho remota** configuração é **habilitado**, você não precisa executar algumas das etapas neste procedimento. Pular para a etapa 4 para começar a configurar as permissões de usuário de área de trabalho remota.
    2. Se o **área de trabalho remota** configuração é **desabilitado**, clique na palavra **desabilitado**. O **propriedades do sistema** caixa de diálogo é aberta na **remoto** guia.
2. Na **área de trabalho remota**, clique em **permitir conexões remotas com este computador**. O **Conexão de área de trabalho remota** caixa de diálogo é aberta. Siga um destes procedimentos.
    1. Para personalizar as conexões de rede que são permitidas, clique em **Firewall do Windows com segurança avançada**e, em seguida, defina as configurações que você deseja permitir. 
    2. Para habilitar a Conexão de área de trabalho remota para todas as conexões no computador de rede, clique em **Okey**.
3. Na **propriedades do sistema**, na **área de trabalho remota**, decida se deseja habilitar **permitir conexões somente de computadores que executam a área de trabalho remota com autenticação no nível da rede**e fazer sua seleção.
4. Clique em **Selecionar Usuários**. O **Remote Desktop Users** caixa de diálogo é aberta.
5. Na **Remote Desktop Users**, para conceder permissão a um usuário para se conectar remotamente ao NPS, clique em **Add**e, em seguida, digite o nome de usuário para a conta do usuário. Clique em **OK**.
6. Repita a etapa 5 para cada usuário para quem você deseja conceder permissão de acesso remoto para o NPS. Quando você terminar a adição de usuários, clique em **Okey** para fechar o **Remote Desktop Users** caixa de diálogo e **Okey** novamente para fechar o **propriedades do sistema**caixa de diálogo.
7. Para se conectar a um NPS remoto que você tenha configurado usando as etapas anteriores, clique em **inicie**, role para baixo a lista em ordem alfabética e, em seguida, clique em **Acessórios do Windows**e clique em **remoto Conexão de área de trabalho**. O **Conexão de área de trabalho remota** caixa de diálogo é aberta.
8. No **Conexão de área de trabalho remota** na caixa **computador**, digite o nome NPS ou o endereço IP. Se você preferir, clique em **opções**, configure as opções de conexão adicionais e, em seguida, clique em **salvar** para salvar a conexão para uso repetido.
9. Clique em **Connect**e, quando solicitado forneça credenciais de conta de usuário para uma conta que tenha permissões para fazer logon no e configurar o NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Use os comandos de Netsh NPS para gerenciar um NPS

Você pode usar comandos no contexto de Netsh NPS para mostrar e definir a configuração da autenticação, autorização, estatísticas e auditoria de banco de dados usado pelo NPS e o serviço de acesso remoto. Use os comandos no contexto de Netsh NPS para:

- Configurar ou reconfigurar um NPS, incluindo todos os aspectos do NPS que também estão disponível para a configuração usando o console do NPS na interface do Windows.
- Exporte a configuração de um NPS (servidor de origem,), incluindo as chaves do registro e a configuração do NPS armazenar, como um script Netsh.
- Importe a configuração para outro NPS usando um script de Netsh e o arquivo de configuração exportados da fonte de NPS.

Você pode executar esses comandos no Prompt de comando do Windows Server 2016 ou o Windows PowerShell. Você também pode executar comandos do netsh nps em scripts e arquivos em lotes.

**Credenciais administrativas** 

Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Para inserir o contexto de Netsh NPS em um NPS

1. Abra o Prompt de comando ou o Windows PowerShell.
2. Tipo de **netsh**, e pressione ENTER.
3. Tipo de **nps**, e pressione ENTER.
4. Para exibir uma lista dos comandos disponíveis, digite um ponto de interrogação \(?\) e pressione ENTER.


Para obter mais informações sobre comandos Netsh NPS, consulte [comandos Netsh para servidor de políticas de rede no Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), ou baixar todo o [referência técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) da Galeria do TechNet. Este download é o total rede Shell técnica referência para o Windows Server 2008 e Windows Server 2008 R2. O formato é Windows ajudar \(chm\) em um arquivo zip. Esses comandos ainda estão presentes no Windows Server 2016 e Windows 10, portanto, você pode usar netsh nesses ambientes, embora, é recomendável usar o Windows PowerShell.

## <a name="use-windows-powershell-to-manage-npss"></a>Usar o Windows PowerShell para gerenciar NPSs

Você pode usar comandos do Windows PowerShell para gerenciar NPSs. Para obter mais informações, consulte os seguintes tópicos de referência de comando do Windows PowerShell.

- [Cmdlets NPS (servidor) de diretiva no Windows PowerShell de rede](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Você pode usar esses comandos netsh no Windows Server 2012 R2 ou sistemas operacionais posteriores.
- [Módulo NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Você pode usar esses comandos netsh no Windows Server 2016.

Para obter mais informações sobre a administração do NPS, consulte [servidor de diretivas de rede (NPS) gerenciar](nps-manage-top.md).
