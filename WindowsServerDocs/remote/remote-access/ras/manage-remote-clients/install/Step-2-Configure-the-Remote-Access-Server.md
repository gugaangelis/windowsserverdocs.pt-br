---
title: Etapa 2 configurar o servidor de acesso remoto
description: Este tópico faz parte do guia gerenciar clientes DirectAccess remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fa8cd203304b477761e9cfa0742efc8c9d8a5443
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308141"
---
# <a name="step-2-configure-the-remote-access-server"></a>Etapa 2 configurar o servidor de acesso remoto

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como definir as configurações de cliente e servidor que são necessárias para o gerenciamento remoto de clientes DirectAccess. Antes de iniciar as etapas de implantação, verifique se você concluiu as etapas de planejamento descritas na [etapa 2 planejar a implantação de acesso remoto](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|Instalar a função de Acesso Remoto|Instalar a função Acesso Remoto.|  
|Configurar o tipo de implantação|Configurar o tipo de implantação como DirectAccess e VPN, somente DirectAccess ou somente VPN.|  
|Configurar os clientes de DirectAccess|Configurar o servidor de Acesso Remoto com os grupos de segurança contendo os clientes do DirectAccess.|  
|Configurar o servidor de Acesso Remoto|Defina as configurações do servidor de acesso remoto.|  
|Configurar os servidores de infraestrutura|Configurar os servidores de infraestrutura usados na organização.|  
|Configurar os servidores de aplicativos|Configure os servidores de aplicativos para exigir autenticação e criptografia.|  
|Resumo de configuração e GPOs alternativos|Ver o resumo de configuração de Acesso Remoto e modificar os GPOs, se desejado.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets de exemplo do Windows PowerShell que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="install-the-remote-access-role"></a><a name="BKMK_Role"></a>Instalar a função de acesso remoto  
Você deve instalar a função de acesso remoto em um servidor em sua organização que atuará como o servidor de acesso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Para instalar a função de acesso remoto em servidores DirectAccess  
  
1.  No servidor DirectAccess, no console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na caixa de diálogo **selecionar funções de servidor** , selecione **acesso remoto**e clique em **Avançar**.  
  
4.  Clique em **Avançar** três vezes.  
  
5.  Na caixa de diálogo **selecionar serviços de função** , selecione **DirectAccess e VPN (RAS)** e clique em **Adicionar recursos**.  
  
6.  Selecione **Roteamento**, selecione **proxy de aplicativo Web**, clique em **Adicionar recursos**e, em seguida, clique em **Avançar**.  
  
7. Clique em **Avançar** e, em seguida, clique em **Instalar**.  
  
8.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem sucedida e, em seguida, clique em **Fechar**.  
  
![](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>Configurar o tipo de implantação  
Há três opções que você pode usar para implantar o acesso remoto no console de gerenciamento de acesso remoto:  
  
-   DirectAccess e VPN  
  
-   Somente DirectAccess  
  
-   Somente VPN  
  
> [!NOTE]  
> Este guia usa o método somente DirectAccess de implantação nos procedimentos de exemplo.  
  
#### <a name="to-configure-the-deployment-type"></a>Para configurar o tipo de implantação  
  
1.  No servidor de acesso remoto, abra o console de gerenciamento de acesso remoto: na tela **Iniciar** , digite, digite **console de gerenciamento de acesso remoto**e pressione Enter. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
2.  No Console de Gerenciamento de Acesso Remoto, no painel do meio, clique em **Executar o assistente de configuração de acesso remoto**.  
  
3.  Na caixa de diálogo **Configurar acesso remoto** , selecione DirectAccess e VPN, somente DirectAccess ou somente VPN.  
  
## <a name="configure-directaccess-clients"></a><a name="BKMK_Clients"></a>Configurar clientes DirectAccess  
Para que um computador cliente possa ser provisionado para usar o DirectAccess, ele deverá pertencer ao grupo de segurança selecionado. Após a configuração do DirectAccess, os computadores cliente no grupo de segurança são provisionados para receber os GPOs (objetos de Política de Grupo do DirectAccess) para gerenciamento remoto.  
  
#### <a name="to-configure-directaccess-clients"></a>Para configurar os clientes do DirectAccess  
  
1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 1: Clientes Remotos**, clique em **Configurar**.  
  
2.  No assistente de instalação do cliente DirectAccess, na página **cenário de implantação** , clique em **implantar DirectAccess somente para gerenciamento remoto**e, em seguida, clique em **Avançar**.  
  
3.  Na página **Selecionar Grupos**, clique em **Adicionar**.  
  
4.  Na caixa de diálogo **Selecionar grupos** , selecione os grupos de segurança que contêm os computadores cliente do DirectAccess e clique em **Avançar**.  
  
5.  Na página **Assistente de conectividade de rede**:  
  
    -   Na tabela, adicione os recursos que serão usados para determinar a conectividade com a rede interna. Uma sonda de web padrão é criada automaticamente se nenhum outro recurso estiver configurado. Ao configurar os locais de investigação da Web para determinar a conectividade com a rede corporativa, verifique se você tem pelo menos uma investigação baseada em HTTP configurada. Configurar apenas uma investigação de ping não é suficiente e pode levar a uma determinação imprecisa do status de conectividade. Isso ocorre porque o ping é isento do IPsec. Como resultado, o ping não garante que os túneis IPsec sejam estabelecidos corretamente.  
  
    -   Adicione um endereço de email de assistência técnica para permitir que os usuários enviem informações se enfrentarem problemas de conectividade.  
  
    -   Forneça um nome amigável para a conexão do DirectAccess.  
  
    -   Marque a caixa de seleção **Permitir que clientes do DirectAccess usem resolução de nome local**, se necessário.  
  
        > [!NOTE]  
        > Quando a resolução de nomes locais está habilitada, os usuários que executam o NCA podem resolver nomes usando servidores DNS configurados no computador cliente do DirectAccess.  
  
6.  Clique em **Concluir**.  
  
## <a name="configure-the-remote-access-server"></a><a name="BKMK_Server"></a>Configurar o servidor de acesso remoto  
Para implantar o acesso remoto, você precisa configurar o servidor que atuará como o servidor de acesso remoto com o seguinte:  
  
1.  Adaptadores de rede corretos  
  
2.  Uma URL pública para o servidor de acesso remoto ao qual os computadores cliente podem se conectar (o endereço connectto)  
  
3.  Um certificado IP-HTTPS com um assunto que corresponde ao endereço connectto  
  
4.  Configurações de IPv6  
  
5.  Autenticação do computador cliente  
  
#### <a name="to-configure-the-remote-access-server"></a>Para configurar o servidor de Acesso Remoto  
  
1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 2: Servidor de Acesso Remoto**, clique em **Configurar**.  
  
2.  No Assistente de configuração do servidor de Acesso Remoto, na página **Topologia de Rede**, clique na topologia de implantação que será usada na sua organização. Em **Digitar o nome público ou endereço IPv4 usado pelos clientes para se conectar ao servidor de acesso remoto**, digite o nome público da implantação (esse nome corresponde ao nome do assunto do certificado IP-HTTPS, por exemplo, edge1.contoso.com) e clique em **Avançar**.  
  
3.  Na página **adaptadores de rede** , o assistente detecta automaticamente:  
  
    -   Adaptadores de rede para as redes em sua implantação. Se o assistente não detectar os adaptadores de rede corretos, selecione-os manualmente.  
  
    -   Certificado IP-HTTPS. Isso se baseia no nome público da implantação que você definiu durante a etapa anterior do assistente. Se o assistente não detectar o certificado IP-HTTPS correto, clique em **procurar** para selecionar manualmente o certificado correto.  
  
4.  Clique em **Avançar**.  
  
5.  Na página **configuração de prefixo** (essa página só ficará visível se o IPv6 for detectado na rede interna), o assistente detectará automaticamente as configurações de IPv6 que são usadas na rede interna. Se sua implantação necessitar de prefixos adicionais, configure os prefixos IPv6 para a rede interna, um prefixo IPv6 para atribuir a computadores cliente do DirectAccess e um para atribuir a computadores cliente do VPN.  
  
6.  Na página **Autenticação**:  
  
    -   Para implantações multissite e com autenticação de dois fatores, você deverá usar uma autenticação de certificado de computador. Marque a caixa de seleção **usar certificados de computador** para usar a autenticação de certificado de computador e selecione o certificado raiz IPSec.  
  
    -   Para permitir que computadores cliente que executam o Windows 7 se conectem via DirectAccess, marque a caixa de seleção **habilitar computadores cliente do Windows 7 para se conectar via DirectAccess** . Você também deverá usar uma autenticação de certificado de computador para este tipo de implantação.  
  
7.  Clique em **Concluir**.  
  
## <a name="configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>Configurar os servidores de infraestrutura  
Para configurar os servidores de infraestrutura em uma implantação de acesso remoto, você deve configurar o seguinte:  
  
-   Servidor de local da rede  
  
-   Configurações de DNS, incluindo a lista de pesquisa de sufixo DNS  
  
-   Qualquer servidor de gerenciamento que não seja detectado automaticamente pelo acesso remoto  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Para configurar os servidores de infraestrutura  
  
1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 3: Servidores de Infraestrutura**, clique em **Configurar**.  
  
2.  No Assistente de configuração de servidor de infraestrutura, na página **Servidor do local de rede**, clique na opção correspondente ao local do servidor de local de rede na sua implantação.  
  
    -   Se o servidor de local de rede estiver em um servidor Web remoto, insira a URL e clique em **validar** antes de continuar.  
  
    -   Se o servidor de local de rede estiver no servidor de Acesso Remoto, clique em **Navegar** para localizar o certificado apropriado e depois em **Avançar**.  
  
3.  Na página **DNS** , na tabela, insira os sufixos de nome adicionais que serão aplicados como isenções de tabela de políticas de resolução de nomes (NRPT). Selecione a opção de resolução de nome local e clique em **Avançar**.  
  
4.  Na página **lista de pesquisa de sufixo DNS** , o servidor de acesso remoto automaticamente detecta sufixos de domínio na implantação. Use os botões **Adicionar** e **remover** para criar a lista de sufixos de domínio que você deseja usar. Para adicionar um novo sufixo de domínio, em **Novo sufixo**, digite o sufixo e clique em **Adicionar**. Clique em **Avançar**.  
  
5.  Na página **Gerenciamento** , adicione servidores de gerenciamento que não são detectados automaticamente e clique em **Avançar**. O acesso remoto adiciona automaticamente os controladores de domínio e os servidores de Configuration Manager.  
  
6.  Clique em **Concluir**.  
  
## <a name="configure-application-servers"></a><a name="BKMK_App"></a>Configurar servidores de aplicativos  
Em uma implantação de acesso remoto completa, a configuração de servidores de aplicativos é uma tarefa opcional. Nesse cenário para o gerenciamento remoto de clientes DirectAccess, os servidores de aplicativos não são utilizados e essa etapa fica esmaecida para indicar que ele não está ativo. Clique em **concluir** para aplicar a configuração.  
  
## <a name="configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>Resumo de configuração e GPOs alternativos  
Uma vez concluída a configuração do Acesso Remoto, a **Revisão de Acesso Remoto** será exibida. Aqui, você pode revisar todas as configurações previamente selecionadas, incluindo:  
  
-   **Configurações de GPO**  
  
    O nome do GPO do servidor DirectAccess e o nome do GPO do cliente são listados. Você pode clicar no link **alterar** ao lado do cabeçalho **configurações de GPO** para modificar as configurações de GPO.  
  
-   **Clientes remotos**  
  
    A configuração do cliente do DirectAccess é exibida, incluindo o grupo de segurança, verificadores de conectividade e o nome da conexão do DirectAccess.  
  
-   **Servidor de acesso remoto**  
  
    A configuração do DirectAccess é exibida, incluindo o nome público e o endereço, a configuração do adaptador de rede e as informações de certificado.  
  
-   **Servidores de infraestrutura**  
  
    Esta lista inclui a URL do servidor de local de rede, os sufixos de DNS usados pelos clientes do DirectAccess e as informações do servidor de gerenciamento.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 3: verificar a implantação](Step-3-Verify-the-Deployment_2.md)  
  
  


