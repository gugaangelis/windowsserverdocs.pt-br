---
title: Gerenciamento de servidor de política de rede com ferramentas de administração
description: Você pode usar este tópico para saber mais sobre as ferramentas que você pode usar para gerenciar o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdb82c5bc1c541f19b1b2f4c8db837af4a812f81
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-management-with-administration-tools"></a>Gerenciamento de servidor de política de rede com ferramentas de administração

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre as ferramentas que você pode usar para gerenciar seus servidores NPS.

Depois de instalar o NPS, você pode administrar servidores NPS:

- Localmente, usando o snap-in do Console de gerenciamento Microsoft NPS \(MMC\), o console NPS estático em Ferramentas administrativas, comandos do Windows PowerShell ou os comandos de \(Netsh\) Shell de rede para NPS.
- Do servidor NPS remoto, usando o snap-in NPS MMC, os comandos Netsh para NPS, os comandos do Windows PowerShell para NPS ou Conexão de área de trabalho remota.
- De uma estação de trabalho remota, usando a Conexão de área de trabalho remota em combinação com outras ferramentas, como o NPS MMC ou o Windows PowerShell.

>[!NOTE]
>No Windows Server 2016, você pode gerenciar o servidor NPS local usando o console NPS. Para gerenciar servidores NPS locais e remotos, você deve usar o NPS MMC snap\-in.

As seções a seguir fornecem instruções sobre como gerenciar seus servidores NPS locais e remotos.

## <a name="configure-the-local-nps-server-by-using-the-nps-console"></a>Configurar o servidor NPS Local usando o Console NPS

Depois que você instalou NPS, você pode usar esse procedimento para gerenciar o servidor NPS local usando o MMC NPS.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-configure-the-local-nps-server-by-using-the-nps-console"></a>Para configurar o servidor NPS local usando o console NPS

1. No Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.

2. No console do NPS, clique em \(Local\) NPS. No painel de detalhes, escolha **configuração padrão** ou **Advanced Configuration**, e siga um destes procedimentos com base na sua seleção:
    - Se você escolher **configuração padrão**, selecione um cenário na lista e, em seguida, siga as instruções para iniciar um Assistente de configuração.
    - Se você escolher **Advanced Configuration**, clique na seta para expandir **opções de configuração avançada**e, em seguida, analisar e configurar as opções disponíveis com base em relação à funcionalidade NPS que você deseja - servidor RADIUS, proxy RADIUS ou ambos.

## <a name="manage-multiple-nps-servers-by-using-the-nps-mmc-snap-in"></a>Gerenciar vários servidores NPS usando Snap\-in do MMC do NPS

Você pode usar esse procedimento para gerenciar o servidor NPS local e vários servidores NPS remotos usando o MMC NPS snap\-in.

Antes de executar o procedimento a seguir, você deve instalar o NPS no computador local e em computadores remotos.

Dependendo de condições de rede e o número de servidores NPS que gerenciar usando o MMC NPS snap\-in, a resposta do MMC snap\-in pode estar lenta. Além disso, o tráfego de configuração de servidor NPS é enviado pela rede durante uma sessão de administração remota usando o NPS snap\-in. Certifique-se de que sua rede é fisicamente segura e que usuários mal-intencionados não têm acesso a esse tráfego de rede.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores.

### <a name="to-manage-multiple-nps-servers-by-using-the-nps-snap-in"></a>Gerenciar vários servidores NPS usando o NPS snap\-in

1. Para abrir o MMC, execute o Windows PowerShell como administrador. No Windows PowerShell, digite **mmc**, e pressione ENTER. Abre o Console de gerenciamento Microsoft.
2. No MMC, no **arquivo** menu, clique em **Adicionar/remover Snap\-in**. O **adicionar ou remover Snap\-ins** caixa de diálogo é aberta.
3. Em **adicionar ou remover Snap\-ins**, na **snap\-ins disponíveis**, role para baixo na lista, clique em **NPS**e clique em **adicionar**. O **Selecionar computador** caixa de diálogo é aberta.
4. Em **Selecionar computador**, verifique **computador Local \ (o computador no qual este console está running\)** está selecionado e clique em **Okey**. O snap\-in para o servidor NPS local é adicionado à lista na **selecionado snap\-ins**.
5. Em **adicionar ou remover Snap\-ins**, na **snap\-ins disponíveis**, certifique-se de que **NPS** é ainda selecionada e clique **adicionar**. O **Selecionar computador** caixa de diálogo abrir novamente.
6. Em **Selecionar computador**, clique em **outro computador**e digite o endereço IP ou domínio totalmente qualificado nomear \(FQDN\) do servidor NPS remoto que você deseja gerenciar usando o NPS snap\-in. Opcionalmente, você pode clicar em **procurar** examinar o diretório do computador que você deseja adicionar. Clique em **Okey**.
7. Repita as etapas 5 e 6 para adicionar mais servidores NPS ao NPS snap\-in. Quando você tiver adicionado todos os servidores NPS que você deseja gerenciar, clique em **Okey**.
8. Para salvar o snap-in NPS para uso posterior, clique em **arquivo**e clique em **salvar**. No **Salvar como** caixa de diálogo, navegue até o local do disco rígido em que você deseja salvar o arquivo, digite um nome para seu arquivo de \(.msc\) do Console de gerenciamento Microsoft e, em seguida, clique em **salvar**. 

## <a name="manage-an-nps-server-by-using-remote-desktop-connection"></a>Gerenciar um servidor NPS usando a Conexão de área de trabalho remota

Você pode usar esse procedimento para gerenciar um servidor NPS remoto usando a Conexão de área de trabalho remota.

Usando a Conexão de área de trabalho remota, você pode gerenciar remotamente servidores NPS que executam o Windows Server 2016. Você pode gerenciar servidores NPS também remotamente em um computador executando o Windows 10 ou sistemas operacionais de cliente de Windows anteriores.

Você pode usar a conexão de área de trabalho remota para gerenciar vários servidores NPS usando um dos dois métodos.

1. Crie uma conexão de área de trabalho remota para cada um dos servidores NPS individualmente.
2. Use a área de trabalho remota para se conectar a um servidor NPS e, em seguida, use o NPS MMC no servidor para gerenciar outros servidores remotos. Para obter mais informações, consulte a seção anterior **gerenciar vários servidores NPS usando o MMC NPS Snap\-in**.

**Credenciais administrativas** 

Para concluir este procedimento, você deve ser um membro do grupo Administradores no servidor NPS.

### <a name="to-manage-an-nps-server-by-using-remote-desktop-connection"></a>Gerenciar um servidor NPS usando a Conexão de área de trabalho remota

1. Em cada servidor NPS que você deseja gerenciar remotamente, no Gerenciador do servidor, selecione **servidor Local**. No painel de detalhes do Gerenciador do servidor, exiba a **área de trabalho remota** configuração e siga um destes procedimentos. 
    1. Se o valor da **área de trabalho remota** configuração é **Enabled**, você não precisa executar algumas das etapas neste procedimento. Vá até a etapa 4 para iniciar a configuração de permissões de usuário de área de trabalho remota.
    2. Se o **área de trabalho remota** configuração é **desabilitada**, clique na palavra **desabilitada**. O **propriedades do sistema** caixa de diálogo é aberta no **remoto** guia.
2. Em **área de trabalho remota**, clique em **permitir conexões remotas com este computador**. O **Conexão de área de trabalho remota** caixa de diálogo é aberta. Siga um destes procedimentos:
    1. Para personalizar as conexões de rede são permitidas, clique em **Firewall do Windows com segurança avançada**e, em seguida, defina as configurações que você deseja permitir. 
    2. Para habilitar a Conexão de área de trabalho remota para todas as conexões no computador de rede, clique em **Okey**.
3. Em **propriedades do sistema**, na **área de trabalho remota**, decida se deseja habilitar **permitir conexões somente de computadores que executam a área de trabalho remota com autenticação no nível da rede**e faça sua seleção.
4. Clique em **Selecione usuários**. O **usuários da área de trabalho remota** caixa de diálogo é aberta.
5. Em **usuários da área de trabalho remota**, para conceder permissão ao usuário para se conectar remotamente ao servidor NPS, clique em **adicionar**e, em seguida, digite o nome de usuário para a conta do usuário. Clique em **Okey**.
6. Repita a etapa 5 para cada usuário para o qual você deseja conceder permissão de acesso remoto no servidor NPS. Quando terminar de adicionar usuários, clique em **Okey** para fechar o **usuários da área de trabalho remota** caixa de diálogo e **Okey** novamente para fechar o **propriedades do sistema** caixa de diálogo.
7. Para conectar a um servidor NPS remoto que você tenha configurado usando as etapas anteriores, clique em **iniciar**, role para baixo a lista em ordem alfabética e, em seguida, clique em **Acessórios do Windows**e clique em **Conexão de área de trabalho remota**. O **Conexão de área de trabalho remota** caixa de diálogo é aberta.
8. No **Conexão de área de trabalho remota** na caixa **computador**, digite o endereço IP ou nome do servidor NPS. Se você preferir, clique em **opções**, configurar as opções de conexão adicionais e, em seguida, clique em **salvar** para salvar a conexão para uso repetido.
9. Clique em **conectar**e quando solicitado fornecer as credenciais de conta de usuário para uma conta que tenha permissão para fazer logon e configurar o servidor NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps-server"></a>Use os comandos Netsh NPS para gerenciar um servidor NPS

Você pode usar comandos no contexto Netsh NPS para mostrar e definir a configuração de autenticação, autorização, estatísticas e auditoria de banco de dados usado pelo NPS e o serviço de acesso remoto. Use os comandos no contexto Netsh NPS para:

- Configurar ou reconfigurar um servidor NPS, incluindo todos os aspectos do NPS que também estão disponível para a configuração usando o console NPS na interface do Windows.
- Exporte a configuração de um servidor NPS (servidor de origem,), incluindo chaves do registro e o repositório de configuração do NPS, como um script Netsh.
- Importe a configuração para outro servidor NPS usando um script Netsh e o arquivo de configuração exportada do servidor NPS de origem.

Você pode executar esses comandos do Prompt de comando do Windows Server 2016 ou do Windows PowerShell. Você também pode executar os comandos netsh nps em scripts e arquivos em lotes.

**Credenciais administrativas** 

Para executar este procedimento, você deve ser um membro do grupo Administradores no computador local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps-server"></a>Para inserir o contexto de Netsh NPS em um servidor NPS

1. Abra o Prompt de comando ou o Windows PowerShell.
2. Tipo **netsh**, e pressione ENTER.
3. Tipo **nps**, e pressione ENTER.
4. Para ver uma lista de comandos disponíveis, digite um ponto de interrogação \(?\) e pressione ENTER.


Para obter mais informações sobre os comandos Netsh NPS, consulte [comandos Netsh para o servidor de política de rede no Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), ou baixar todo o [referência técnica do Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) da Galeria do TechNet. Este download é a rede Shell Technical referência para o Windows Server 2008 e Windows Server 2008 R2 completo. O formato é \(*.chm\) ajuda do Windows em um arquivo zip. Esses comandos ainda estão presentes no Windows Server 2016 e Windows 10, para que você possa usar netsh nesses ambientes, embora usando o Windows PowerShell é recomendado.

## <a name="use-windows-powershell-to-manage-nps-servers"></a>Usar o Windows PowerShell para gerenciar servidores NPS

Você pode usar comandos do Windows PowerShell para gerenciar servidores NPS. Para obter mais informações, consulte os seguintes tópicos de referência de comando do Windows PowerShell.

- [Cmdlets da diretiva de NPS (servidor) no Windows PowerShell de rede](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Você pode usar esses comandos netsh no Windows Server 2012 R2 ou sistemas operacionais posteriores.
- [Módulo NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Você pode usar esses comandos netsh no Windows Server 2016.

Para obter mais informações sobre a administração de NPS, consulte [servidor de política de rede (NPS) gerenciar](nps-manage-top.md).
