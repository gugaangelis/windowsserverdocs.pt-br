---
title: Etapa 2 configurar o servidor de DirectAccess básico
description: Este tópico faz parte do guia de implantar um único servidor DirectAccess usando o Introdução ao Assistente para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9983fb475143109d191f3b6d69afef48d109472a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446955"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>Etapa 2 configurar o servidor de DirectAccess básico

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como definir as configurações de cliente e servidor necessárias para uma implantação básica do DirectAccess. Antes de começar as etapas de implantação, certifique-se de que você tenha concluído as etapas de planejamento descritas em [planejar uma implantação básica do DirectAccess](Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|Instalar a função Acesso Remoto|Instalar a função Acesso Remoto.|  
|Configurar o DirectAccess usando o Assistente do Guia de Introdução|O novo Assistente do Guia de Introdução apresenta uma experiência de configuração incrivelmente simplificada. O assistente mascara a complexidade do DirectAccess e permite a configuração automatizada em apenas algumas etapas simples. O assistente proporciona uma experiência ininterrupta para o administrador por meio da configuração automática do proxy Kerberos para eliminar a necessidade de implantação de PKI interna.|  
|Atualizar clientes com configuração do DirectAccess|Para receber as configurações do DirectAccess, os clientes devem atualizar a política de grupo enquanto estão conectados à Intranet.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Instalar a função de acesso remoto  
Para implantar o Acesso Remoto, você deverá instalar a função Acesso Remoto em um servidor na sua organização que agirá como servidor de Acesso Remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.  
  
1.  No servidor de acesso remoto, no console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na caixa de diálogo **Selecionar funções de servidor** , selecione **Acesso Remoto**e clique em **Avançar**.  
  
4.  Na caixa de diálogo **Selecionar recursos**, clique em **Avançar**.  
  
5.  Clique em **próxima**e, em seguida, no **selecionar serviços de função** caixa de diálogo, clique o **DirectAccess e VPN (RAS)** caixa de seleção.  
  
6.  Clique em **adicionar recursos**, clique em **próxima**e, em seguida, clique em **instalar**.  
  
7.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
![Windows PowerShell](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet do Windows PowerShell ou os cmdlets instalar a função acesso remoto: 

1. Abra o PowerShell como administrador.

2. Instale o recurso de acesso remoto:

   ```  
   Install-WindowsFeature RemoteAccess   
   ```  

3. Reinicie o computador:

   ```
   Restart-Computer
   ```
   
4. Instale o PowerShell de acesso remoto:

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```



  
## <a name="configure-directaccess-with-the-getting-started-wizard"></a>Configurar DirectAccess com o Assistente do Guia de Introdução  
  
#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>Para configurar o DirectAccess usando o Assistente do Guia de Introdução  
  
1.  No Gerenciador do Servidor, clique em **Ferramentas** e clique em **Gerenciamento de Acesso Remoto**.  
  
2.  No console do gerenciamento de acesso remoto, selecione o serviço de função para configurar no painel de navegação à esquerda e, em seguida, clique em **execute o Assistente para introdução**.  
  
3.  Clique em **Implantar somente DirectAccess**.  
  
4.  Selecione a topologia da configuração de rede e digite o nome público ao qual os clientes de acesso remoto irão se conectar. Clique em **Avançar**.  
  
    > [!NOTE]  
    > Por padrão, o Assistente do Guia de Introdução implanta o DirectAccess em todos os laptops e notebooks no domínio por meio da aplicação de um filtro WMI ao GOP de configurações do cliente.  
  
5.  Clique em **concluir**.  
  
6.  Como não há nenhuma KPI usada nessa implantação de testes, se não forem encontrados certificados, o assistente provisionará certificados autoassinados automaticamente para IP-HTTPS e o Servidor de Local de Rede e habilitará o proxy Kerberos automaticamente. O assistente também habilitará NAT64 e DNS64 para conversão de protocolos no ambiente somente IPv4. Depois que o assistente concluir com sucesso aplicando a configuração, clique em **fechar**.  
  
7.  Na árvore do console de Gerenciamento de Acesso remoto, selecione **Status das Operações**. Aguarde até que o status de todos os monitores seja exibido como "Processando". No painel Tatera, em Monitoramento, clique em **Atualizar** constantemente para atualizar a exibição.  
  
## <a name="update-clients-with-the-directaccess-configuration"></a>Atualizar clientes com configuração do DirectAccess  
  
#### <a name="to-update-directaccess-clients"></a>Para atualizar clientes do DirectAccess  
  
1.  Abra o PowerShell como administrador.  
  
2.  Na janela do PowerShell, digite **gpupdate** e pressione **ENTER**.  
  
3.  Aguarde até que a atualização da política do computador seja concluída com êxito.  
  
4.  Digite **Get-DnsClientNrptPolicy** e pressione **ENTER**  
  
    As entradas da Tabela de Políticas de Resolução de Nomes (NRPT) para o DirectAccess são exibidas. Observe que a isenção do servidor NLS é exibida. O Assistente do Guia de Introdução criou essa entrada DNS automaticamente para o servidor DirectAccess e provisionou um certificado autoassinado associado, de modo que o servidor DirectAccess possa funcionar como Servidor de Local de Rede.  
  
5.  Digite **Get-NCSIPolicyConfiguration** e pressione **ENTER**. As configurações do indicador de status de conectividade de rede implantadas pelo assistente são exibidas. Observe o valor de DomainLocationDeterminationURL. Sempre que este URL de servidor de local de rede estiver acessível, o cliente determinará que está dentro da rede corporativa e as configurações de NRPT não serão aplicadas.  
  
6.  Digite **Get-DAConnectionStatus** e pressione **ENTER**. Como o cliente pode alcançar o URL do servidor de local de rede, o status será exibido como **ConnectedLocally**.  
  
## <a name="BKMK_Links"></a>Etapa anterior  
  
-   [Etapa 1: Configurar a infraestrutura do DirectAccess](Step-1-Configure-the-DirectAccess-Infrastructure.md)  
  
## <a name="next-step"></a>Próximas etapas  
  
-   [Etapa 3 verificar as implantações do DirectAccess básico](da-basic-configure-s3-verify.md)  
  


