---
title: Etapa 2 configurar o servidor de acesso remoto
description: Este tópico faz parte do guia de clientes do DirectAccess de gerenciar remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60373a24663c73c537e747d5993e60fa2a38972
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282814"
---
# <a name="step-2-configure-the-remote-access-server"></a>Etapa 2 configurar o servidor de acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como definir as configurações de cliente e servidor que são necessárias para o gerenciamento remoto de clientes DirectAccess. Antes de começar as etapas de implantação, certifique-se de que você tenha concluído as etapas de planejamento descritas em [etapa 2 planejar a implantação do acesso remoto](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|Instalar a função Acesso Remoto|Instalar a função Acesso Remoto.|  
|Configurar o tipo de implantação|Configurar o tipo de implantação como DirectAccess e VPN, somente DirectAccess ou somente VPN.|  
|Configurar os clientes de DirectAccess|Configurar o servidor de Acesso Remoto com os grupos de segurança contendo os clientes do DirectAccess.|  
|Configurar o servidor de Acesso Remoto|Defina as configurações do servidor de acesso remoto.|  
|Configurar os servidores de infraestrutura|Configurar os servidores de infraestrutura usados na organização.|  
|Configurar os servidores de aplicativos|Configure os servidores de aplicativo para exigir a autenticação e criptografia.|  
|Resumo de configuração e GPOs alternativos|Ver o resumo de configuração de Acesso Remoto e modificar os GPOs, se desejado.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Instalar a função de acesso remoto  
Você deve instalar a função acesso remoto em um servidor em sua organização que atuará como o servidor de acesso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Para instalar a função de acesso remoto nos servidores DirectAccess  
  
1.  No servidor do DirectAccess, no console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Sobre o **selecionar funções do servidor** caixa de diálogo, selecione **acesso remoto**e, em seguida, clique em **próxima**.  
  
4.  Clique em **próxima** três vezes.  
  
5.  Sobre o **selecionar serviços de função** caixa de diálogo, selecione **DirectAccess e VPN (RAS)** e, em seguida, clique em **adicionar recursos**.  
  
6.  Selecione **roteamento**, selecione **Proxy de aplicativo Web**, clique em **adicionar recursos**e, em seguida, clique em **Avançar**.  
  
7. Clique em **Avançar**e, em seguida, clique em **Instalar**.  
  
8.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>Configurar o tipo de implantação  
Há três opções que você pode usar para implantar o acesso remoto do console de gerenciamento de acesso remoto:  
  
-   DirectAccess e VPN  
  
-   Somente DirectAccess  
  
-   Somente VPN  
  
> [!NOTE]  
> Este guia usa o DirectAccess único método de implantação nos procedimentos de exemplo.  
  
#### <a name="to-configure-the-deployment-type"></a>Para configurar o tipo de implantação  
  
1.  No servidor de Acesso Remoto, abra o console de Gerenciamento de Acesso Remoto: Sobre o **iniciar** tela, o tipo, o tipo **Console de gerenciamento de acesso remoto**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de Gerenciamento de Acesso Remoto, no painel do meio, clique em **Executar o assistente de configuração de acesso remoto**.  
  
3.  No **configurar o acesso remoto** caixa de diálogo, selecione o DirectAccess e VPN, somente DirectAccess ou somente VPN.  
  
## <a name="BKMK_Clients"></a>Configurar clientes DirectAccess  
Para que um computador cliente possa ser provisionado para usar o DirectAccess, ele deverá pertencer ao grupo de segurança selecionado. Depois de configurado o DirectAccess, os computadores de clientes no grupo de segurança são provisionados para receber os objetos de diretiva de grupo (GPOs) DirectAccess para gerenciamento remoto.  
  
#### <a name="to-configure-directaccess-clients"></a>Para configurar os clientes do DirectAccess  
  
1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 1: Clientes Remotos**, clique em **Configurar**.  
  
2.  No Assistente de instalação do cliente do DirectAccess, sobre o **cenário de implantação** , clique em **implantar DirectAccess somente para gerenciamento remoto**e, em seguida, clique em **próxima**.  
  
3.  Na página **Selecionar Grupos**, clique em **Adicionar**.  
  
4.  No **selecionar grupos** caixa de diálogo, selecione os grupos de segurança que contêm os computadores cliente DirectAccess e, em seguida, clique em **próxima**.  
  
5.  Na página **Assistente de conectividade de rede**:  
  
    -   Na tabela, adicione os recursos que serão usados para determinar a conectividade à rede interna. Uma sonda de web padrão é criada automaticamente se nenhum outro recurso estiver configurado. Ao configurar os locais de investigação da web para determinar a conectividade com a rede corporativa, certifique-se de que você tenha pelo menos um teste baseado em HTTP configurado. Configurar somente uma investigação de ping não é suficiente, e isso pode levar a uma determinação imprecisa do status de conectividade. Isso ocorre porque o ping é isento de IPsec. Como resultado, ping não garante que os túneis de IPsec sejam estabelecidos adequadamente.  
  
    -   Adicione um endereço de email de assistência técnica para permitir que os usuários enviem informações se enfrentarem problemas de conectividade.  
  
    -   Forneça um nome amigável para a conexão do DirectAccess.  
  
    -   Marque a caixa de seleção **Permitir que clientes do DirectAccess usem resolução de nome local**, se necessário.  
  
        > [!NOTE]  
        > Quando a resolução de nomes locais é habilitada, os usuários que estão executando o NCA podem resolver nomes usando servidores DNS configurados no computador cliente DirectAccess.  
  
6.  Clique em **concluir**.  
  
## <a name="BKMK_Server"></a>Configurar o servidor de acesso remoto  
Para implantar o acesso remoto, você precisa configurar o servidor que agirá como servidor de acesso remoto com o seguinte:  
  
1.  Adaptadores de rede corretos  
  
2.  Uma URL pública para o servidor de acesso remoto ao qual cliente computadores podem se conectar (o endereço ConnectTo)  
  
3.  Um certificado IP-HTTPS com um assunto que corresponda ao endereço ConnectTo  
  
4.  Configurações de IPv6  
  
5.  Autenticação do computador cliente  
  
#### <a name="to-configure-the-remote-access-server"></a>Para configurar o servidor de Acesso Remoto  
  
1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 2: Servidor de Acesso Remoto**, clique em **Configurar**.  
  
2.  No Assistente de configuração do servidor de Acesso Remoto, na página **Topologia de Rede**, clique na topologia de implantação que será usada na sua organização. Em **Digitar o nome público ou endereço IPv4 usado pelos clientes para se conectar ao servidor de acesso remoto**, digite o nome público da implantação (esse nome corresponde ao nome do assunto do certificado IP-HTTPS, por exemplo, edge1.contoso.com) e clique em **Avançar**.  
  
3.  Sobre o **adaptadores de rede** página, o assistente detecta automaticamente:  
  
    -   Adaptadores de rede para as redes em sua implantação. Se o assistente não detectar os adaptadores de rede corretos, selecione-os manualmente.  
  
    -   Certificado IP-HTTPS. Isso se baseia no nome público para a implantação que você definiu na etapa anterior do assistente. Se o assistente não detectar o certificado IP-HTTPS correto, clique em **procurar** para selecionar manualmente o certificado correto.  
  
4.  Clique em **Avançar**.  
  
5.  Sobre o **configuração de prefixo** página (essa página só é visível se o IPv6 é detectado na rede interna), o assistente detecta automaticamente as configurações de IPv6 que são usadas na rede interna. Se sua implantação necessitar de prefixos adicionais, configure os prefixos IPv6 para a rede interna, um prefixo IPv6 para atribuir a computadores cliente do DirectAccess e um para atribuir a computadores cliente do VPN.  
  
6.  Na página **Autenticação**:  
  
    -   Para implantações multissite e com autenticação de dois fatores, você deverá usar uma autenticação de certificado de computador. Selecione o **usar certificados de computador** caixa de seleção para usar a autenticação de certificado de computador e selecione o certificado raiz IPsec.  
  
    -   Para habilitar os computadores cliente que executam o Windows 7 para se conectar por meio do DirectAccess, selecione a **computadores cliente de habilitar o Windows 7 para se conectarem via DirectAccess** caixa de seleção. Você também deverá usar uma autenticação de certificado de computador para este tipo de implantação.  
  
7.  Clique em **concluir**.  
  
## <a name="BKMK_Infra"></a>Configurar os servidores de infraestrutura  
Para configurar os servidores de infraestrutura em uma implantação de acesso remoto, você deve configurar o seguinte:  
  
-   Servidor de local da rede  
  
-   Lista de pesquisa de sufixo de configurações de DNS, incluindo o DNS  
  
-   Quaisquer servidores de gerenciamento que não são detectados automaticamente pelo acesso remoto  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Para configurar os servidores de infraestrutura  
  
1.  No painel central do Console de Gerenciamento de Acesso Remoto, na área **Etapa 3: Servidores de Infraestrutura**, clique em **Configurar**.  
  
2.  No Assistente de configuração de servidor de infraestrutura, na página **Servidor do local de rede**, clique na opção correspondente ao local do servidor de local de rede na sua implantação.  
  
    -   Se o servidor de local de rede estiver em um servidor web remoto, insira a URL e, em seguida, clique em **validar** antes de continuar.  
  
    -   Se o servidor de local de rede estiver no servidor de Acesso Remoto, clique em **Navegar** para localizar o certificado apropriado e depois em **Avançar**.  
  
3.  Sobre o **DNS** página, na tabela, insira os sufixos de nome adicionais que serão aplicados como isenções de nome resolução diretiva NRPT (tabela). Selecione a opção de resolução de nome local e clique em **Avançar**.  
  
4.  Sobre o **lista de pesquisa de sufixos DNS** página, o servidor de acesso remoto detecta automaticamente os sufixos de domínio na implantação. Use o **Add** e **remover** botões para criar a lista de sufixos de domínio que você deseja usar. Para adicionar um novo sufixo de domínio, em **Novo sufixo**, digite o sufixo e clique em **Adicionar**. Clique em **Avançar**.  
  
5.  Sobre o **Management** página, adicione os servidores de gerenciamento que não são detectados automaticamente e, em seguida, clique em **próxima**. O Acesso Remoto adiciona automaticamente os controladores de domínio e os servidores do System Center Configuration Manager.  
  
6.  Clique em **concluir**.  
  
## <a name="BKMK_App"></a>Configurar servidores de aplicativos  
Em uma implantação completa do acesso remoto, a configuração de servidores de aplicativo é uma tarefa opcional. Neste cenário para o gerenciamento remoto de clientes DirectAccess, servidores de aplicativos não são utilizados e essa etapa é esmaecida para indicar que não está ativa. Clique em **concluir** para aplicar a configuração.  
  
## <a name="BKMK_GPO"></a>Configuração resumo e GPOs alternativos  
Uma vez concluída a configuração do Acesso Remoto, a **Revisão de Acesso Remoto** será exibida. Aqui, você pode revisar todas as configurações previamente selecionadas, incluindo:  
  
-   **Configurações de GPO**  
  
    O nome do GPO do servidor DirectAccess e o nome do GPO do cliente são listados. Você pode clicar no **alteração** link ao lado de **configurações de GPO** título para modificar as configurações de GPO.  
  
-   **Clientes remotos**  
  
    A configuração de cliente do DirectAccess é exibida, incluindo o grupo de segurança, verificadores de conectividade e o nome de conexão do DirectAccess.  
  
-   **Servidor de acesso remoto**  
  
    A configuração do DirectAccess é exibida, incluindo o nome público e endereço, configuração do adaptador de rede e informações de certificado.  
  
-   **Servidores de infraestrutura**  
  
    Esta lista inclui a URL do servidor de local de rede, os sufixos de DNS usados pelos clientes do DirectAccess e as informações do servidor de gerenciamento.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 3: Verificar a implantação](Step-3-Verify-the-Deployment_2.md)  
  
  


