---
title: Gerenciamento de Servidor de Políticas de Rede com Ferramentas de Administração
description: Você pode usar este tópico para saber mais sobre as ferramentas que você pode usar para gerenciar o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 527fbf52d68f36d198068514476868bcba930a68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396455"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gerenciamento de Servidor de Políticas de Rede com Ferramentas de Administração

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as ferramentas que você pode usar para gerenciar seu NPSs.

Depois de instalar o NPS, você pode administrar o NPSs:

- Localmente, usando o console de gerenciamento do NPS da Microsoft \(snap-in do MMC\), o console NPS estático em ferramentas administrativas, comandos do Windows PowerShell ou o Shell de rede \(comandos netsh\) para NPS.
- De um NPS remoto, usando o snap-in do MMC do NPS, os comandos netsh para NPS, os comandos do Windows PowerShell para NPS ou Conexão de Área de Trabalho Remota.
- De uma estação de trabalho remota, usando Conexão de Área de Trabalho Remota em combinação com outras ferramentas, como o MMC do NPS ou o Windows PowerShell.

>[!NOTE]
>No Windows Server 2016, você pode gerenciar o NPS local usando o console do NPS. Para gerenciar o NPSs remoto e local, você deve usar o snap\-do MMC do NPS no.

As seções a seguir fornecem instruções sobre como gerenciar seus NPSs locais e remotos.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurar o NPS local usando o console do NPS

Depois de instalar o NPS, você pode usar este procedimento para gerenciar o NPS local usando o MMC do NPS.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Para configurar o NPS local usando o console do NPS

1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.

2. No console do NPS, clique em NPS \(local\). No painel de detalhes, escolha **configuração padrão** ou **Configuração avançada**e siga um destes procedimentos com base na sua seleção:
    - Se você escolher **configuração padrão**, selecione um cenário na lista e siga as instruções para iniciar um assistente de configuração.
    - Se você escolher **Configuração avançada**, clique na seta para expandir **as opções de configuração avançadas**e, em seguida, examine e configure as opções disponíveis com base na funcionalidade do NPS que você deseja-servidor RADIUS, proxy RADIUS ou ambos.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Gerenciar vários NPSs usando o snap\-do MMC do NPS em

Você pode usar este procedimento para gerenciar o NPS local e vários NPSs remotos usando o snap\-do MMC do NPS no.

Antes de executar o procedimento a seguir, você deve instalar o NPS no computador local e em computadores remotos.

Dependendo das condições da rede e do número de NPSs que você gerencia usando o snap do MMC do NPS\-no, a resposta do snap\-do MMC pode ser lenta. Além disso, o tráfego de configuração do NPS é enviado pela rede durante uma sessão de administração remota usando o\-snap do NPS no. Verifique se sua rede está fisicamente segura e se os usuários mal-intencionados não têm acesso a esse tráfego de rede.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Para gerenciar vários NPSs usando o snap\-do NPS em

1. Para abrir o MMC, execute o Windows PowerShell como administrador. No Windows PowerShell, digite **MMC**e pressione Enter. O Console de Gerenciamento Microsoft será aberto.
2. No MMC, no menu **arquivo** , clique em **Adicionar/remover snap\-em**. A caixa de diálogo **Adicionar ou Remover Snap\-ins** é aberta.
3. Em **Adicionar ou remover snap\-ins**, no **snap\-ins disponíveis**, role para baixo na lista, clique em **servidor de políticas de rede**e clique em **Adicionar**. A caixa de diálogo **Selecionar Computador** será aberta.
4. Em **Selecionar computador**, verifique se **o computador local \(computador no qual esse console está sendo executado\)** está selecionado e clique em **OK**. O\-de snap no para o NPS local é adicionado à lista em **snap\-ins selecionados**.
5. Em **Adicionar ou remover snap\-ins**, no **snap\-ins disponíveis**, verifique se o **servidor de políticas de rede** ainda está selecionado e clique em **Adicionar**. A caixa de diálogo **Selecionar computador** é aberta novamente.
6. Em **Selecionar computador**, clique em **outro computador**e digite o endereço IP ou o nome de domínio totalmente qualificado \(FQDN\) do NPS remoto que você deseja gerenciar usando o\-snap do NPS no. Opcionalmente, você pode clicar em **procurar** para examinar o diretório do computador que deseja adicionar. Clique em **OK**.
7. Repita as etapas 5 e 6 para adicionar mais NPSs ao snap do NPS\-no. Depois de adicionar todos os NPSs que você deseja gerenciar, clique em **OK**.
8. Para salvar o snap-in do NPS para uso posterior, clique em **arquivo**e em **salvar**. Na caixa de diálogo **salvar como** , navegue até o local do disco rígido onde você deseja salvar o arquivo, digite um nome para o seu console de gerenciamento Microsoft \(arquivo. msc\) e, em seguida, clique em **salvar**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Gerenciar um NPS usando Conexão de Área de Trabalho Remota

Você pode usar este procedimento para gerenciar um NPS remoto usando Conexão de Área de Trabalho Remota.

Usando Conexão de Área de Trabalho Remota, você pode gerenciar remotamente seu NPSs executando o Windows Server 2016. Você também pode gerenciar remotamente o NPSs de um computador que executa sistemas operacionais Windows 10 ou mais antigos do Windows Client.

Você pode usar Área de Trabalho Remota conexão para gerenciar vários NPSs usando um dos dois métodos.

1. Crie uma conexão de Área de Trabalho Remota com cada um dos seus NPSs individualmente.
2. Use Área de Trabalho Remota para se conectar a um NPS e, em seguida, use o MMC do NPS nesse servidor para gerenciar outros servidores remotos. Para obter mais informações, consulte a seção anterior **gerenciar vários NPSs usando o Snap\-do MMC do NPS no**.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores no NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Para gerenciar um NPS usando Conexão de Área de Trabalho Remota

1. Em cada NPS que você deseja gerenciar remotamente, em Gerenciador do Servidor, selecione **servidor local**. No painel detalhes Gerenciador do Servidor, exiba a configuração **área de trabalho remota** e siga um destes procedimentos. 
    1. Se o valor da configuração **área de trabalho remota** estiver **habilitado**, você não precisará executar algumas das etapas neste procedimento. Pule para a etapa 4 para iniciar a configuração de Área de Trabalho Remota permissões de usuário.
    2. Se a configuração de **área de trabalho remota** estiver **desabilitada**, clique na palavra **desabilitada**. A caixa de diálogo **Propriedades do sistema** é aberta na guia **remoto** .
2. Em **área de trabalho remota**, clique em **permitir conexões remotas com este computador**. A caixa de diálogo **conexão de área de trabalho remota** é aberta. Siga um destes procedimentos.
    1. Para personalizar as conexões de rede que são permitidas, clique em **Firewall do Windows com segurança avançada**e defina as configurações que você deseja permitir. 
    2. Para habilitar Conexão de Área de Trabalho Remota para todas as conexões de rede no computador, clique em **OK**.
3. Em **Propriedades do sistema**, em **área de trabalho remota**, decida se deseja habilitar **permitir conexões somente de computadores que executam o área de trabalho remota com autenticação no nível da rede**e faça sua seleção.
4. Clique em **Selecionar Usuários**. A caixa de diálogo **área de trabalho remota usuários** é aberta.
5. Em **área de trabalho remota usuários**, para conceder permissão a um usuário para se conectar remotamente ao NPS, clique em **Adicionar**e digite o nome de usuário da conta do usuário. Clique em **OK**.
6. Repita a etapa 5 para cada usuário para quem você deseja conceder permissão de acesso remoto ao NPS. Quando você terminar de adicionar usuários, clique em **OK** para fechar a caixa de diálogo **área de trabalho remota usuários** e **OK** novamente para fechar a caixa de diálogo **Propriedades do sistema** .
7. Para se conectar a um NPS remoto que você configurou usando as etapas anteriores, clique em **Iniciar**, role para baixo na lista alfabética e clique em **acessórios do Windows**e clique em **conexão de área de trabalho remota**. A caixa de diálogo **conexão de área de trabalho remota** é aberta.
8. Na caixa de diálogo **conexão de área de trabalho remota** , em **computador**, digite o nome do NPS ou o endereço IP. Se preferir, clique em **Opções**, configure opções de conexão adicionais e, em seguida, clique em **salvar** para salvar a conexão para uso repetido.
9. Clique em **conectar**e, quando solicitado, forneça as credenciais de conta de usuário para uma conta que tenha permissões para fazer logon no e configurar o NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Usar comandos netsh NPS para gerenciar um NPS

Você pode usar comandos no contexto netsh NPS para mostrar e definir a configuração do banco de dados de autenticação, autorização, contabilidade e auditoria usado pelo NPS e pelo serviço de acesso remoto. Use comandos no contexto netsh NPS para:

- Configure ou RECONFIGURE um NPS, incluindo todos os aspectos do NPS que também estão disponíveis para configuração usando o console do NPS na interface do Windows.
- Exporte a configuração de um NPS (o servidor de origem), incluindo chaves do registro e o repositório de configuração do NPS, como um script netsh.
- Importe a configuração para outro NPS usando um script netsh e o arquivo de configuração exportado do NPS de origem.

Você pode executar esses comandos no prompt de comando do Windows Server 2016 ou no Windows PowerShell. Você também pode executar comandos netsh nps em scripts e arquivos em lotes.

**Credenciais administrativas** 

Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Para inserir o contexto netsh NPS em um NPS

1. Abra o prompt de comando ou o Windows PowerShell.
2. Digite **netsh**e pressione Enter.
3. Digite **NPS**e pressione Enter.
4. Para exibir uma lista de comandos disponíveis, digite um ponto de interrogação \(?\) e pressione ENTER.


Para obter mais informações sobre comandos netsh NPS, consulte [comandos netsh para o servidor de políticas de rede no Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), ou baixe toda a [referência técnica do netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) na galeria do TechNet. Este download é a referência técnica completa do Shell de rede para o Windows Server 2008 e o Windows Server 2008 R2. O formato é a ajuda do Windows \(*. chm\) em um arquivo zip. Esses comandos ainda estão presentes no Windows Server 2016 e no Windows 10, para que você possa usar o netsh nesses ambientes, embora o uso do Windows PowerShell seja recomendado.

## <a name="use-windows-powershell-to-manage-npss"></a>Usar o Windows PowerShell para gerenciar o NPSs

Você pode usar comandos do Windows PowerShell para gerenciar o NPSs. Para obter mais informações, consulte os seguintes tópicos de referência de comando do Windows PowerShell.

- [Cmdlets do servidor de diretivas de rede (NPS) no Windows PowerShell](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Você pode usar esses comandos netsh no Windows Server 2012 R2 ou em sistemas operacionais posteriores.
- [Módulo do NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Você pode usar esses comandos netsh no Windows Server 2016.

Para obter mais informações sobre a administração do NPS, consulte [gerenciar o servidor de políticas de rede (NPS)](nps-manage-top.md).
