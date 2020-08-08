---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurar um computador para a função de proxy do servidor de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 06bccb135963d5b5bc754e895843a4d75153c120
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963117"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurar um computador para a função de proxy do servidor de federação

Depois de configurar um computador com os certificados necessários e ter instalado o serviço de função Proxy do Serviço de Federação, você estará pronto para configurar o computador para se tornar um proxy de servidor de Federação. Use o procedimento a seguir para que o computador atue na função proxy de servidor de federação.

> [!IMPORTANT]
> Antes de usar este procedimento para configurar o computador proxy do servidor de Federação, certifique-se de ter seguido todas as etapas na lista de [verificação: configurar um proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md) na ordem em que estão listados. Verifique se pelo menos um servidor de federação foi implantado e se todas as credenciais necessárias para autorização de uma configuração proxy de servidor de federação foram implementadas. Você também deve configurar protocolo SSL \( \) associações SSL no site padrão ou o assistente não será iniciado. Todas essas tarefas devem ser concluídas antes do funcionamento desse servidor de federação.

Depois que você terminar de configurar o computador, verifique se o proxy de servidor de federação está funcionando como esperado. Para mais informações, consulte [Verificar se um Proxy do Servidor de Federação está operacional](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Para configurar um computador para a função de proxy do servidor de federação

1.  Ha duas maneiras para iniciar o Assistente de Configuração do Servidor de Federação AD FS. Para iniciar o assistente, tome uma das seguintes ações:

    -   Na tela **Iniciar** , digite**AD FS assistente de configuração de proxy do servidor de Federação**e pressione Enter.

    -   A qualquer momento depois que o assistente de instalação for concluído, abra o Windows Explorer, navegue até a pasta **C: \\ Windows \\ ADFS** e clique duas vezes \- em **FspConfigWizard.exe**.

2.  Usando qualquer método, Iniciar o assistente, e em seguida, na página **Bem-vindo**, clique em **Próximo**.

3.  Na página **Especificar o Nome de Serviço de Federação**, em **Nome de Serviço de Federação**, digite o nome que representa o Serviço de Federação para o qual este computador atua na função de proxy.

4.  Com base em seus requisitos de rede específicos, determine se você precisará usar um servidor proxy HTTP para encaminhar solicitações ao Serviço de Federação. Se for assim, marque a caixa de seleção **Use um servidor proxy HTTP ao enviar solicitações para este Serviço de Federação**, em **Endereço de servidor proxy HTTP** digite o endereço no servidor proxy, clique em **Testar a Conexão** para verificar a conectividade, e em seguida clique em **Próximo**.

5.  Quando for solicitado, especifique as credenciais necessárias para estabelecer uma relação de confiança entre o proxy do servidor de federação e o Serviço de Federação.

    Por padrão, somente a conta de serviço usada pelo Serviço de Federação ou um membro do grupo local de \\ Administradores internos pode autorizar um proxy de servidor de Federação.

6.  Na página **Pronto para Aplicar as Configurações**, verificar os detalhes. Se as configurações parecem estar certas, clique em **Próximo** para começar configurar este computador com estas configurações de proxy.

7.  Na página **Resultados de Configuração**, analise os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.

    Não há nenhum snap-in do MMC do console de gerenciamento Microsoft \( \) \- a ser usado para administrar os proxies do servidor de Federação. Para definir as configurações para cada um dos proxies do servidor de Federação em sua organização, use os cmdlets do Windows PowerShell.

## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configurando uma \/ porta alternativa de IP TCP para operações de proxy
Por padrão, o serviço proxy do servidor de Federação é configurado para usar a porta TCP 443 para tráfego HTTPS e a porta 80 para tráfego HTTP para comunicação com o servidor de Federação. Para configurar portas diferentes, como a porta TCP 444 para HTTPS e a porta 81 para HTTP, as seguintes tarefas devem ser concluídas.

> [!NOTE]
> Se pretender implantar inicialmente AD FS para operar em portas TCP alternativas de \/ IP, você deve primeiro modificar as portas nas associações de protocolo do IIS para http e HTTPS no servidor de Federação e nos computadores proxy do servidor de Federação. Isso deve ocorrer antes de executar os assistentes de configuração do AD FS para configuração inicial. Se você configurar Serviços de Informações da Internet \( \) primeiro o IIS, suas configurações de porta IP de TCP alternativas \/ serão descobertas quando \- a configuração baseada em assistente ocorrer no AD FS e o procedimento a seguir não for necessário. Para alterar as configurações de porta depois, atualize as associações do protocolo IIS primeiro e, depois, use o seguinte procedimento para atualizar as configurações da porta de forma adequada. Para obter mais informações sobre como editar associações do IIS, consulte o [artigo 149605](https://go.microsoft.com/fwlink/?LinkId=190275) na base de dados de conhecimento Microsoft.

#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Para configurar portas alternativas de \/ IP TCP para o proxy do servidor de Federação a ser usado

1.  Configure o servidor de federação para usar as portas não padrão.

    Para fazer isso, especifique o número da porta não padrão, incluindo-o com as opções *HttpsPort* e *HttpPort* como parte do cmdlet **set \- adfsproperties** . Por exemplo, para configurar essas portas, use os seguintes comandos na sessão do Windows PowerShell no computador do servidor de Federação:

    ```
    Set-ADFSProperties -HttpsPort 444
    Set-ADFSProperties -HttpPort 81
    ```

2.  Configure o proxy do servidor de Federação para usar a porta não padrão.

    Para fazer isso, especifique o número da porta não padrão, incluindo-o com as opções *HttpsPort* e *HttpPort* como parte do cmdlet **set \- ADFSProxyProperties** . Por exemplo, para configurar essas portas, use os seguintes comandos na sessão do Windows PowerShell no computador do servidor de Federação:

    ```
    Set-ADFSProxyProperties -HttpsPort 444
    Set-ADFSProxyProperties -HttpPort 81
    ```

    > [!NOTE]
    > As URLs de ponto de extremidade não são habilitadas por padrão para o serviço proxy de servidor de Federação. Se você estiver configurando uma nova instalação de servidor de Federação, deverá habilitar primeiro os pontos de extremidade do serviço proxy do servidor de Federação. Por exemplo, supõe-se que para todos os pontos de extremidade que o exemplo neste procedimento se refere a você os habilitou para proxy, selecionando-os no snap-in de gerenciamento de AD FS \- e, em seguida, selecionando **habilitar no proxy**.

3.  Atualize a instalação do IIS no proxy do servidor de Federação para que Security Assertion Markup Language \( \) os pontos de extremidade de confiança SAML e WS \- sejam configurados para refletir o número da porta atualizada. Para fazer isso, você pode usar o bloco de notas para modificar o seguinte no arquivo Web.config, que está localizado em systemdrive% \\ Inetpub \\ ADFS \\ ls \\ no computador proxy do servidor de Federação. Por exemplo, supondo que você tenha um servidor de Federação chamado sts1.contoso.com e o novo número da porta seja 444, navegue até e abra o arquivo de Web.config no bloco de notas no computador proxy do servidor de Federação, localize a seção a seguir, modifique o número da porta como realçado abaixo e, em seguida, salve e saia do bloco de notas.

    ```
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />
    ```

4.  Adicione a conta de usuário do serviço de proxy do servidor de Federação à ACL da lista de controle \( \) de acesso para as URLs de ponto de extremidade relacionadas. Por exemplo, se o número da porta for 1234 e a conta de usuário usada para executar o serviço proxy de servidor do AD FSfederation em for a \- conta de serviço de rede interna, digite o seguinte comando no prompt de comando:

    ```
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"

    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"
    ```

    Os comandos anteriores devem ser executados no servidor de Federação e nos computadores proxy do servidor de Federação.

## <a name="additional-references"></a>Referências adicionais
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)


