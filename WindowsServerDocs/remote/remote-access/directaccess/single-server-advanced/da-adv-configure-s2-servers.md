---
title: Etapa 2 configurar servidores DirectAccess avançados
description: Este tópico faz parte do guia implantar um único servidor DirectAccess com as configurações avançadas do Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f1d4be029ffac40486482ff8ff56528b5f662ba9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955263"
---
# <a name="step-2-configure-advanced-directaccess-servers"></a>Etapa 2 configurar servidores DirectAccess avançados

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve como configurar as definições do cliente e do servidor requeridas para uma implantação de Acesso Remoto avançado usando um único servidor de Acesso Remoto em um ambiente misto de IPv4 e IPv6. Antes de começar as etapas de implantação, verifique se você concluiu as etapas de planejamento descritas em [planejar uma implantação avançada do DirectAccess](Plan-an-Advanced-DirectAccess-Deployment.md).

|Tarefa|Descrição|
|----|--------|
|2.1. Instalar a função Acesso Remoto|Instalar a função Acesso Remoto.|
|2.2. Configurar o tipo de implantação|Configurar o tipo de implantação como DirectAccess e VPN, somente DirectAccess ou somente VPN.|
|[Planejar uma implantação do DirectAccess Avançado](Plan-an-Advanced-DirectAccess-Deployment.md)|Configurar o servidor de Acesso Remoto com os grupos de segurança contendo os clientes do DirectAccess.|
|2.4. Configurar o servidor de Acesso Remoto|Definir as configurações do servidor de Acesso Remoto.|
|2.5. Configurar os servidores de infraestrutura|Configurar os servidores de infraestrutura usados na organização.|
|2.6. Configurar os servidores de aplicativos|Configurar os servidores de aplicativos para que exijam autenticação e criptografia.|
|2.7. Resumo de configuração e GPOs alternativos|Ver o resumo de configuração de Acesso Remoto e modificar os GPOs, se desejado.|
|2.8. Como configurar o servidor de Acesso Remoto usando o Windows PowerShell|Configurar o acesso remoto usando o Windows PowerShell.|

> [!NOTE]
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, confira [Usando os Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="21-install-the-remote-access-role"></a><a name="BKMK_Role"></a>2.1. Instalar a função Acesso Remoto
Para implantar o Acesso Remoto, você deverá instalar a função Acesso Remoto em um servidor na sua organização que agirá como servidor de Acesso Remoto.

#### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.

1.  No servidor de acesso remoto, no console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.

2.  Clique em **Avançar** três vezes para chegar à tela **Selecionar funções do servidor**.

3.  Na página **Selecionar funções do servidor**, selecione **Acesso Remoto**, clique em **Adicionar recursos** e em **Avançar**.

4.  Clique em **Avançar** cinco vezes.

5.  Na página **Confirmar seleções de instalação**, clique em **Instalar**.

6.  Na página **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.

![Êxito no progresso da instalação](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

```
Install-WindowsFeature RemoteAccess -IncludeManagementTools
```

## <a name="22-configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>2.2. Configurar o tipo de implantação
É possível implantar o Acesso Remoto com o console de Gerenciamento de Acesso Remoto de três maneiras:

-   DirectAccess e VPN

-   Somente DirectAccess

-   Somente VPN

Este guia usa uma implantação somente DirectAccess nos procedimentos de exemplo.

#### <a name="to-configure-the-deployment-type"></a>Para configurar o tipo de implantação

1.  No servidor de acesso remoto, abra o console de gerenciamento de acesso remoto: na tela **Iniciar** , digite**RAMgmtUI.exe**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.

2.  No Console de Gerenciamento de Acesso Remoto, no painel do meio, clique em **Executar o assistente de configuração de acesso remoto**.

3.  Na caixa de diálogo **Configurar o Acesso Remoto**, clique na opção desejada para implantar DirectAccess e VPN, somente DirectAccess ou somente VPN.

## <a name="23-configure-directaccess-clients"></a><a name="BKMK_Clients"></a>2.3. Configurar os clientes de DirectAccess
Para que um computador cliente possa ser provisionado para usar o DirectAccess, ele deverá pertencer ao grupo de segurança selecionado. Depois de configurado o DirectAccess, os computadores cliente do grupo de segurança são provisionados para receber a GPO (Política de Grupo de Objeto) do DirectAccess. Você também pode configurar o cenário de implantação, que permite configurar o DirectAccess para acesso ao cliente e gerenciamento remoto, ou somente para gerenciamento remoto.

#### <a name="to-configure-directaccess-clients"></a>Para configurar os clientes do DirectAccess

1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 1: Clientes Remotos**, clique em **Configurar**.

2.  No Assistente de configuração de cliente do DirectAccess, na página **Cenário de implantação**, clique no cenário de implantação que você deseja usar na sua organização (**DirectAccess completo** ou **Somente gerenciamento remoto**) e clique em **Avançar**.

3.  Na página **Selecionar Grupos**, clique em **Adicionar**.

4.  Na caixa de diálogo **Selecionar Grupos**, selecione os grupos de segurança que contêm computadores cliente do DirectAccess.

    > [!NOTE]
    > Se o grupo de segurança estiver localizado em uma floresta diferente do servidor de acesso remoto, depois de concluir o assistente de instalação de acesso remoto, clique em **Atualizar servidores de gerenciamento** no painel **tarefas** para descobrir os controladores de domínio e os servidores Configuration Manager na nova floresta.

5.  Marque a caixa de seleção **Habilitar o DirectAccess apenas para computadores móveis** para permitir que somente computadores móveis acessem a rede interna, se necessário.

6.  Marque a caixa de seleção **Usar criação de túneis à força** para rotear todo o tráfego cliente (para a rede interna e para a Internet) pelo servidor de Acesso Remoto, se necessário.

7.  Clique em **Próximo**.

8.  Na página **Assistente de conectividade de rede**:

    -   Na tabela, adicione recursos que serão usados para determinar a conectividade com a rede interna. Uma sonda de web padrão é criada automaticamente se nenhum outro recurso estiver configurado.

        > [!CAUTION]
        > Ao configurar os locais da sonda da web para determinar a conectividade com a rede Corporativa, certifique-se de possuir pelo menos uma sonda HTTP configurada. Configurar somente uma sonda **ping** não é suficiente, podendo levar à determinação imprecisa do status de conectividade. Isso ocorre porque o **ping** é uma exceção do IPsec, não garantindo, assim, que os túneis do IPsec sejam estabelecidos adequadamente.

    -   Adicione um endereço de email de assistência técnica para permitir que os usuários enviem informações se enfrentarem problemas de conectividade.

    -   Forneça um nome amigável para a conexão do DirectAccess. Este nome aparecerá na lista de redes quando os usuários clicarem no ícone de rede na área de notificações.

    -   Marque a caixa de seleção **Permitir que clientes do DirectAccess usem resolução de nome local**, se necessário.

        > [!NOTE]
        > Quando a resolução de nome local é habilitada, os usuários que executarem o Assistente de conectividade de rede podem escolher resolver nomes usando servidores DNS configurados no computador cliente do DirectAccess.

9. Clique em **Concluir**.

## <a name="24-configure-the-remote-access-server"></a><a name="BKMK_Server"></a>2.4. Configurar o servidor de Acesso Remoto
Para implantar o Acesso Remoto, será necessário configurar o servidor de Acesso Remoto com os adaptadores de rede corretos, uma URL pública para o servidor de Acesso Remoto, à qual os computadores cliente poderão se conectar (o endereço ConnectTo) e um certificado IP-HTTPS com o assunto correspondente ao endereço ConnectTo, configurações IPv6 e autenticação no computador cliente.

#### <a name="to-configure-the-remote-access-server"></a>Para configurar o servidor de Acesso Remoto

1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 2: Servidor de Acesso Remoto**, clique em **Configurar**.

2.  No Assistente de configuração do servidor de Acesso Remoto, na página **Topologia de Rede**, clique na topologia de implantação que será usada na sua organização. Em **Digitar o nome público ou endereço IPv4 usado pelos clientes para se conectar ao servidor de acesso remoto**, digite o nome público da implantação (esse nome corresponde ao nome do assunto do certificado IP-HTTPS, por exemplo, edge1.contoso.com) e clique em **Avançar**.

3.  Na página **Adaptadores de rede**, o assistente detecta automaticamente os adaptadores de rede para as redes da sua implantação. Se o assistente não detectar os adaptadores de rede corretos, selecione-os manualmente. O assistente também detecta automaticamente o certificado IP-HTTPS com base no nome público da implantação definido na etapa anterior do assistente. Se o assistente não detectar o certificado IP-HTTPS correto, clique em **Navegar** para selecionar manualmente o certificado correto e clique em **Avançar**.

4.  Na página **Configuração de prefixo** (ela aparecerá somente se o IPv6 for implantado na rede interna), o assistente detecta automaticamente as configurações de IPv6 usadas na rede interna. Se sua implantação necessitar de prefixos adicionais, configure os prefixos IPv6 para a rede interna, um prefixo IPv6 para atribuir a computadores cliente do DirectAccess e um para atribuir a computadores cliente do VPN.

    > [!NOTE]
    > Você pode especificar vários prefixos IPv6 internos usando uma lista delimitada por ponto e vírgula, como, por exemplo, 2001:db8:1::/48;2001:db8:2::/48.

5.  Na página **Autenticação**:

    -   Em **Autenticação do usuário**, clique em **Credenciais do Active Directory**. Para configurar uma implantação usando o autenticador de dois fatores, clique em **Autenticação de dois fatores**. Para mais informações, consulte [Implantar Acesso Remoto com autenticação OTP](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831379(v=ws.11)).

    -   Para implantações multissite e com autenticação de dois fatores, você deverá usar uma autenticação de certificado de computador. Marque a caixa de seleção **Usar certificados de computador** para usar autenticação de certificado de computador e selecione o certificado raiz IPsec.

    -   Para permitir que computadores cliente do Windows 7 se conectem por meio do DirectAccess, marque a caixa de seleção **habilitar computadores cliente do Windows 7 para se conectar via DirectAccess** .

        > [!NOTE]
        > Você também deverá usar uma autenticação de certificado de computador para este tipo de implantação.

6.  Clique em **Concluir**.

## <a name="25-configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>2.5. Configurar os servidores de infraestrutura
Para configurar os servidores de infraestrutura em uma implantação de Acesso Remoto, você deverá configurar o servidor de local de rede, as configurações de DNS (incluindo a lista de pesquisa do sufixo de DNS) e servidores de gerenciamento que não são detectados automaticamente pelo Acesso Remoto.

#### <a name="to-configure-the-infrastructure-servers"></a>Para configurar os servidores de infraestrutura

1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 3: Servidores de Infraestrutura**, clique em **Configurar**.

2.  No Assistente de configuração de servidor de infraestrutura, na página **Servidor do local de rede**, clique na opção correspondente ao local do servidor de local de rede na sua implantação. Se o servidor de local de rede estiver em um servidor da web remoto, digite a URL e clique em **Validar** antes de continuar. Se o servidor de local de rede estiver no servidor de Acesso Remoto, clique em **Navegar** para localizar o certificado apropriado e depois em **Avançar**.

3.  Na página **DNS**, especifique na tabela os sufixos de nome adicionais que serão aplicados como exceções da NRPT (Tabela de Políticas de Resolução de Nomes). Selecione a opção de resolução de nome local e clique em **Avançar**.

4.  Na página **Lista de pesquisa de sufixo de DNS**, o servidor de Acesso Remoto detectará automaticamente qualquer sufixo de domínio na implantação. Use os botões **Adicionar** e **Remover** para adicionar e remover os sufixos de domínio da lista de sufixos de domínio a serem usados. Para adicionar um novo sufixo de domínio, em **Novo sufixo**, digite o sufixo e clique em **Adicionar**. Clique em **Próximo**.

5.  Na página **Gerenciamento**, adicione quaisquer servidores de gerenciamento não detectados automaticamente e clique em **Avançar**. O acesso remoto adiciona automaticamente os controladores de domínio e os servidores de Configuration Manager.

    > [!NOTE]
    > Embora os servidores sejam adicionados automaticamente, eles não aparecem na lista. Depois de aplicar a configuração pela primeira vez, os servidores de Configuration Manager aparecem na lista.

6.  Clique em **Concluir**.

## <a name="26-configure-application-servers"></a><a name="BKMK_App"></a>2,6. Configurar os servidores de aplicativos
Em uma implantação de Acesso Remoto, configurar os servidores de aplicativos é uma tarefa opcional. O Acesso Remoto permite exigir autenticação para os servidores de aplicativos selecionados, o que é determinado por sua inclusão em um grupo de segurança de servidores de aplicativos. Por padrão, o tráfego para os servidores de aplicativos também é criptografado, porém você pode escolher não criptografar o tráfego para os servidores de aplicativos e usar somente autenticação.

> [!NOTE]
> A autenticação sem criptografia tem suporte apenas em servidores de aplicativos que executam o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2.

#### <a name="to-configure-application-servers"></a>Para configurar os servidores de aplicativos

1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 4: Servidores de Aplicativos**, clique em **Configurar**.

2.  No Assistente de configuração do servidor de aplicativos do DirectAccess, para exigir a autenticação a servidores de aplicativos selecionados, clique em **Estender autenticação a servidores de aplicativos selecionados**. Clique em **Adicionar** para selecionar o grupo de segurança de servidores de aplicativos.

3.  Para limitar o acesso a somente os servidores do grupo de segurança de servidores de aplicativos, marque a caixa de seleção **Permitir acesso somente aos servidores incluídos nos grupos de segurança**.

4.  Para usar a autenticação sem criptografia, selecione **não criptografar tráfego. Use** a caixa de seleção somente autenticação.

5.  Clique em **Concluir**.

## <a name="27-configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>2,7. Resumo de configuração e GPOs alternativos
Uma vez concluída a configuração do Acesso Remoto, a **Revisão de Acesso Remoto** será exibida. Aqui, você pode revisar todas as configurações previamente selecionadas, incluindo:

1.  **Configurações de GPO**: o nome do GPO do servidor DirectAccess e o nome de GPO do cliente estão listados. Além disso, você pode clicar no link **Alterar** ao lado do cabeçalho **Configurações de GPO** para modificar as configurações de GPO.

2.  **Clientes Remotos**: a configuração do cliente do DirectAccess é exibida, incluindo o grupo de segurança, o status de criação de túneis à força, os verificadores de conectividade e o nome da conexão do DirectAccess.

3.  **Servidor de Acesso Remoto**: a configuração do DirectAccess é exibida, incluindo endereço/nome público, configuração do adaptador de rede, as informações do certificado e informações de OTP, se configurado.

4.  **Servidores de Infraestrutura**: esta lista inclui a URL do servidor de local de rede, os sufixos de DNS usados pelos clientes do DirectAccess e as informações do servidor de gerenciamento.

5.  **Servidores de Aplicativo**: é exibido o status de gerenciamento remoto do DirectAccess, além do status da autenticação completa de servidores de aplicativos específicos.

## <a name="28-how-to-configure-the-remote-access-server-by-using-windows-powershell"></a><a name="BKMK_PS"></a>2,8. Como configurar o servidor de Acesso Remoto usando o Windows PowerShell
![](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**Comandos equivalentes** do Windows PowerShell

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

Para executar uma instalação completa em uma topologia de borda de acesso remoto para o DirectAccess somente em um domínio com o **Corp.contoso.com** raiz e usando os seguintes parâmetros: GPO do servidor: **configurações do servidor DirectAccess**, GPO do cliente: configurações do cliente DirectAccess, adaptador de rede interno: **corpnet**, adaptador de rede externo: **Internet**, ConnectTto endereço: **EDGE1.contoso.com**e servidor de local de rede: **NLS.Corp.contoso.com**:

```
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'
```

Para configurar o servidor de Acesso Remoto para usar autenticação de certificado de computador, com um certificado raiz IPsec emitido pela autoridade de certificação chamada CORP-APP1-CA:

```
$certs = Get-ChildItem Cert:\LocalMachine\Root
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}
Set-DAServer -IPsecRootCertificate $IPsecRootCert
```

Para adicionar o grupo de segurança que contém os clientes do DirectAccess chamados **DirectAccessClients** e para remover o grupo de segurança Computadores do Domínio:

```
Add-DAClient -SecurityGroupNameList @('corp.contoso.com\DirectAccessClients')
Remove-DAClient -SecurityGroupNameList @('corp.contoso.com\Domain Computers')
```

Para habilitar o acesso remoto para todos os computadores (não apenas para notebooks e laptops) e para habilitar o acesso remoto para clientes do Windows 7:

```
Set-DAClient -OnlyRemoteComputers 'Disabled' -Downlevel 'Enabled'
```

Para configurar a experiência do cliente do DirectAccess, incluindo o nome de conexão amigável e a URL da sonda da web:

```
Set-DAClientExperienceConfiguration -FriendlyName 'Contoso DirectAccess Connection' -PreferLocalNamesAllowed $False -PolicyStore 'corp.contoso.com\DirectAccess Client Settings' -CorporateResources @('HTTP:https://directaccess-WebProbeHost.corp.contoso.com')
```

## <a name="previous-step"></a><a name="BKMK_Links"></a>Etapa anterior

-   [Etapa 1: Configurar a infraestrutura do DirectAccess Avançado](da-adv-configure-s1-infrastructure.md)

## <a name="next-step"></a>Próxima etapa

-   [Etapa 3: Verificar a implantação](Step-3-Verify-the-Deployment.md)

