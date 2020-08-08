---
title: Etapa 2 configurar o servidor DirectAccess básico
description: Este tópico faz parte do guia implantar um único servidor DirectAccess usando o assistente de Introdução para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c70e68d6bdf96cd8493720936121b1ea90a9e971
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995907"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>Etapa 2 configurar o servidor DirectAccess básico

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve como definir as configurações de cliente e servidor necessárias para uma implantação básica do DirectAccess. Antes de iniciar as etapas de implantação, verifique se você concluiu as etapas de planejamento descritas em [planejar uma implantação básica do DirectAccess](Plan-a-Basic-DirectAccess-Deployment.md).

|Tarefa|Descrição|
|----|--------|
|Instalar a função Acesso Remoto|Instalar a função Acesso Remoto.|
|Configurar o DirectAccess usando o Assistente do Guia de Introdução|O novo Assistente do Guia de Introdução apresenta uma experiência de configuração incrivelmente simplificada. O assistente mascara a complexidade do DirectAccess e permite a configuração automatizada em apenas algumas etapas simples. O assistente proporciona uma experiência ininterrupta para o administrador por meio da configuração automática do proxy Kerberos para eliminar a necessidade de implantação de PKI interna.|
|Atualizar clientes com configuração do DirectAccess|Para receber as configurações do DirectAccess, os clientes devem atualizar a política de grupo enquanto estão conectados à Intranet.|

> [!NOTE]
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, confira [Usando os Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="install-the-remote-access-role"></a><a name="BKMK_Role"></a>Instalar a função de acesso remoto
Para implantar o Acesso Remoto, você deverá instalar a função Acesso Remoto em um servidor na sua organização que agirá como servidor de Acesso Remoto.

#### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.

1.  No servidor de acesso remoto, no console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.

2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.

3.  Na caixa de diálogo **Selecionar funções do servidor**, selecione **Acesso Remoto** e clique em **Avançar**.

4.  Na caixa de diálogo **Selecionar recursos**, clique em **Avançar**.

5.  Clique em **Avançar**e, na caixa de diálogo **selecionar serviços de função** , clique em **DirectAccess e VPN (RAS)** .

6.  Clique em **Adicionar recursos**, clique em **Avançar**e em **instalar**.

7.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.

![](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***

Os seguintes cmdlets ou cmdlets do Windows PowerShell instalam a função de acesso remoto:

1. Abra o PowerShell como administrador.

2. Instalar o recurso de acesso remoto:

   ```
   Install-WindowsFeature RemoteAccess
   ```

3. Reinicie o computador:

   ```
   Restart-Computer
   ```

4. Instalar o PowerShell de acesso remoto:

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```




## <a name="configure-directaccess-with-the-getting-started-wizard"></a>Configurar DirectAccess com o Assistente do Guia de Introdução

#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>Para configurar o DirectAccess usando o Assistente do Guia de Introdução

1.  No Gerenciador do Servidor, clique em **Ferramentas** e clique em **Gerenciamento de Acesso Remoto**.

2.  No console de gerenciamento de acesso remoto, selecione o serviço de função a ser configurado no painel de navegação esquerdo e clique em **executar o assistente de introdução**.

3.  Clique em **Implantar somente DirectAccess**.

4.  Selecione a topologia da configuração de rede e digite o nome público ao qual os clientes de acesso remoto irão se conectar. Clique em **Próximo**.

    > [!NOTE]
    > Por padrão, o Assistente do Guia de Introdução implanta o DirectAccess em todos os laptops e notebooks no domínio por meio da aplicação de um filtro WMI ao GOP de configurações do cliente.

5.  Clique em **Concluir**.

6.  Como não há nenhuma KPI usada nessa implantação de testes, se não forem encontrados certificados, o assistente provisionará certificados autoassinados automaticamente para IP-HTTPS e o Servidor de Local de Rede e habilitará o proxy Kerberos automaticamente. O assistente também habilitará NAT64 e DNS64 para conversão de protocolos no ambiente somente IPv4. Depois que o assistente concluir a aplicação da configuração com êxito, clique em **Fechar**.

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

## <a name="previous-step"></a><a name="BKMK_Links"></a>Etapa anterior

-   [Etapa 1: Configurar a infraestrutura do DirectAccess](./da-basic-configure-s1-infrastructure.md)

## <a name="next-step"></a>Próxima etapa

-   [Etapa 3 verificar as implantações básicas do DirectAccess](da-basic-configure-s3-verify.md)