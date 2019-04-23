---
title: Configurar o cliente da web de Área de Trabalho Remota para seus usuários
description: Descreve como um administrador pode configurar o cliente de web da área de trabalho remota.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865967"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurar o cliente da web de Área de Trabalho Remota para seus usuários

O cliente da web de área de trabalho remota permite que os usuários a acessar a infraestrutura de área de trabalho remota da sua organização por meio de um navegador da web compatível com. Ele poderá interagir com aplicativos remotos ou áreas de trabalho, como faria com um computador local, independentemente de onde estiverem. Depois que você configurar seu cliente de web da área de trabalho remota, todos os usuários precisam para começar é a URL onde poderão acessar o cliente, suas credenciais e um navegador da web com suporte.

>[!IMPORTANT]
>O cliente da web não oferece suporte a usar o Proxy de aplicativo do Azure e não oferece suporte a Proxy de aplicativo Web em todos os. Ver [usando o RDS com serviços de proxy de aplicativo](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) para obter detalhes.

## <a name="what-youll-need-to-set-up-the-web-client"></a>O que você precisará configurar o cliente da web

Antes de começar, tenha o seguinte em mente:

* Certifique-se de sua [implantação de área de trabalho remota](../rds-deploy-infrastructure.md) tem um Gateway de área de trabalho remota, um agente de Conexão de área de trabalho remota e acesso via Web RD em execução no Windows Server 2016 ou de 2019.
* Verifique se sua implantação é configurada para [licenças de acesso para cliente por usuário](../rds-client-access-license.md) (CALs) em vez de por dispositivo, caso contrário, todas as licenças serão consumidas.
* Instalar o [atualização do Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) no Gateway de área de trabalho remota. Mais tarde as atualizações cumulativas talvez já contém este KB.
* Certifique-se de certificados confiáveis públicos são configurados para as funções de Gateway de área de trabalho remota e acesso via Web RD.
* Verifique se todos os seus usuários se conectarão os computadores estão executando uma das seguintes versões do sistema operacional:
  * Windows 10
  * Windows Server 2008 R2 ou posterior

Os usuários verão um melhor desempenho se conectar ao Windows Server 2016 (ou posterior) e Windows 10 (versão 1611 ou posterior).

>[!IMPORTANT]
>Se você usou o cliente da web durante o período de visualização e instalada uma versão anterior à 1.0.0, você deve primeiro desinstalar o cliente antigo antes de passar para a nova versão. Se você receber um erro que diz "o cliente da web foi instalado usando uma versão mais antiga do RDWebClientManagement e deve ser removido antes de implantar a nova versão", siga estas etapas:
>
>1. Abra um prompt elevado do PowerShell.
>2. Execute **RDWebClientManagement Uninstall-Module** para desinstalar o novo módulo.
>3. Feche e reabra o prompt do PowerShell com privilégios elevados.
>4. Execute **RDWebClientManagement Install-Module - RequiredVersion \<versão antiga > para instalar o módulo antigo.**
>5. Execute **RDWebClient desinstalação** para desinstalar o cliente web antigo.
>6. Execute **RDWebClientManagement Uninstall-Module** para desinstalar o módulo antigo.
>7. Feche e reabra o prompt do PowerShell com privilégios elevados.
>8. Continue com as etapas de instalação normal da seguinte maneira.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Como publicar o cliente da web de área de trabalho remota

Para instalar o cliente web pela primeira vez, siga estas etapas:

1. No servidor do agente de Conexão de área de trabalho remota, obtenha o certificado usado para conexões de área de trabalho remota e exportá-lo como um arquivo. cer. Copie o arquivo. cer do agente de Conexão de área de trabalho remota para o servidor que executa a função Web da área de trabalho remota.
2. No servidor de acesso via Web RD, abra um prompt elevado do PowerShell.
3. No Windows Server 2016, atualize o módulo PowerShellGet, pois a versão da caixa de entrada não dá suporte a instalar o módulo de gerenciamento de cliente da web. Para atualizar o PowerShellGet, execute o seguinte cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Você precisará reiniciar o PowerShell antes da atualização entre em vigor, caso contrário, que o módulo pode não funcionar.

4. Instale o módulo do PowerShell do gerenciamento de cliente da web de área de trabalho remota da Galeria do PowerShell com este cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Depois disso, execute o seguinte cmdlet para baixar a versão mais recente do cliente da web da área de trabalho remota:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Em seguida, execute este cmdlet com o valor entre colchetes substituído com o caminho do arquivo. cer que você copiou do agente de área de trabalho remota:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Por fim, execute este cmdlet para publicar o cliente da web de área de trabalho remota:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Verifique se você pode acessar o cliente da web na URL de cliente da web com o nome do servidor, formatado como <https://server_FQDN/RDWeb/webclient/index.html>. É importante usar o nome do servidor que corresponde ao certificado público de acesso via Web RD na URL (normalmente o FQDN do servidor).

    >[!NOTE]
    >Ao executar o **RDWebClientPackage publicar** cmdlet, você poderá ver um aviso que diz CALs por dispositivo sem suporte, mesmo se sua implantação estiver configurada para CALs por usuário. Se sua implantação usar CALs por usuário, você pode ignorar este aviso. Vamos exibi-lo para se certificar de que você esteja ciente de que a limitação de configuração.
8. Quando você estiver pronto para os usuários acessarem o cliente da web, basta envie a URL do cliente web criado por você.

>[!NOTE]
>Para ver uma lista de todos os cmdlets com suporte para o módulo RDWebClientManagement, execute o seguinte cmdlet do PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Como atualizar o cliente da web de área de trabalho remota

Quando uma nova versão do cliente da web da área de trabalho remota estiver disponível, siga estas etapas para atualizar a implantação com o novo cliente:

1. Abra um prompt elevado do PowerShell no servidor de acesso via Web RD e execute o seguinte cmdlet para baixar a versão mais recente do cliente da web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Opcionalmente, você pode publicar o cliente para teste antes do lançamento oficial executando este cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    O cliente deve aparecer na URL do teste que corresponde à URL do seu cliente web (por exemplo, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publica o cliente para usuários executando o seguinte cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Isso substituirá o cliente para todos os usuários quando eles reiniciar a página da web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Como desinstalar o cliente de web da área de trabalho remota

Para remover todos os vestígios do cliente da web, siga estas etapas:

1. No servidor de acesso via Web RD, abra um prompt elevado do PowerShell.
2. Cancelar a publicação dos clientes de teste e produção, desinstale todos os pacotes de locais e remover as configurações do cliente da web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Desinstale o módulo do PowerShell do gerenciamento de cliente da web de área de trabalho remota:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Como instalar o cliente da web de área de trabalho remota sem uma conexão de internet

Siga estas etapas para implantar o cliente da web em um servidor de acesso via Web RD que não tem uma conexão de internet.

> [!NOTE]
> Instalando sem uma conexão de internet está disponível na versão 1.0.1 e posterior do módulo do RDWebClientManagement PowerShell.

> [!NOTE]
> Você ainda precisa de um PC do administrador com acesso à internet para baixar os arquivos necessários antes de transferi-los para o servidor offline.

> [!NOTE]
> O PC do usuário final precisa de uma conexão de internet por enquanto. Isso será corrigido em uma versão futura do cliente para fornecer um cenário offline completo.

### <a name="from-a-device-with-internet-access"></a>De um dispositivo com acesso à internet

1. Abra um prompt do PowerShell.

2. Importe o módulo do PowerShell do gerenciamento de cliente da web de área de trabalho remota da Galeria do PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Baixe a versão mais recente do cliente de web de área de trabalho remota para instalação em um dispositivo diferente:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Baixe a versão mais recente do módulo do RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copie o conteúdo de "C:\WebClient\" para o servidor de acesso via Web RD.

### <a name="from-the-rd-web-access-server"></a>O servidor de acesso de Web de área de trabalho remota

Siga as instruções em [como publicar o cliente da web de área de trabalho remota](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), substituindo as etapas 4 e 5 com o seguinte.

4. Importe o módulo do PowerShell do gerenciamento de cliente da web de área de trabalho remota na pasta local:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Implante a versão mais recente do cliente web da área de trabalho remota na pasta local (Substituir pelo arquivo zip apropriado):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Conectar-se ao agente de área de trabalho sem o Gateway de área de trabalho remota no Windows Server de 2019
Esta seção descreve como habilitar uma conexão de cliente da web com um agente de área de trabalho remota sem um Gateway de área de trabalho remota no Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Como configurar o servidor do agente de área de trabalho remota

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Siga estas etapas se não houver nenhum certificado associado ao servidor do agente de área de trabalho remota

1. Abra **Gerenciador de servidores** > **dos serviços de área de trabalho remota**.

2. Na **visão geral da implantação** seção, selecione a **tarefas** menu suspenso.

3. Selecione **editar propriedades de implantação**, uma nova janela intitulada **propriedades de implantação** será aberta.

4. No **propriedades de implantação** janela, selecione **certificados** no menu à esquerda.

5. Na lista de níveis de certificado, selecione **agente de Conexão de área de trabalho - habilitar logon único**. Você tem duas opções: (1) crie um novo certificado ou (2) um certificado existente.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Siga estas etapas se houver um certificado associado anteriormente ao servidor do agente de área de trabalho remota

1. Abra o certificado associado ao agente e copie a **impressão digital** valor.

2. Para associar esse certificado para a porta segura 3392, abra uma janela do PowerShell com privilégios elevados e execute o seguinte comando, substituindo **"< impressão digital >"** com o valor copiado na etapa anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para verificar se o certificado foi associado corretamente, execute o seguinte comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Na lista de associações do certificado SSL, certifique-se de que o certificado correto está associado à porta 3392.

3. Abra o registro do Windows (regedit) e nagivate para ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e localize a chave **WebSocketURI**. O valor deve ser definido como **https://+:3392/rdp/**.

### <a name="setting-up-the-rd-session-host"></a>Como configurar o Host de sessão de área de trabalho remota
Se o servidor de Host de sessão de área de trabalho remota é diferente do servidor do agente de área de trabalho remota, siga estas etapas:

1. Criar um certificado para a máquina de Host de sessão de área de trabalho remota, abri-lo e copiar o **impressão digital** valor.

2. Para associar esse certificado para a porta segura 3392, abra uma janela do PowerShell com privilégios elevados e execute o seguinte comando, substituindo **"< impressão digital >"** com o valor copiado na etapa anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para verificar se o certificado foi associado corretamente, execute o seguinte comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Na lista de associações do certificado SSL, certifique-se de que o certificado correto está associado à porta 3392.

3. Abra o registro do Windows (regedit) e nagivate para ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e localize a chave **WebSocketURI**. O valor deve ser definido como **https://+:3392/rdp/**.

### <a name="general-observations"></a>Observações gerais

* Certifique-se de que o do Host de sessão de área de trabalho remota e o agente de área de trabalho remota no servidor esteja executando Windows Server 2019.

* Certifique-se de que pública confiável certificados são configurados do Host de sessão de área de trabalho remota e o agente de área de trabalho remota no servidor.
    > [!NOTE]
    > Se o Host de sessão de área de trabalho remota e o servidor do agente de área de trabalho remota compartilham o mesmo computador, defina o certificado do servidor de agente de área de trabalho remota apenas. Se o servidor Host de sessão de área de trabalho remota e área de trabalho remota Broker usar computadores diferentes, ambos devem ser configuradas com certificados exclusivos.

* O **nome alternativo da entidade (SAN)** para cada certificado deve ser definido para a máquina **totalmente o nome de domínio qualificado (FQDN)**. O **CN (nome comum)** deve coincidir com a SAN para cada certificado.

## <a name="troubleshooting"></a>Solução de problemas

Se um usuário relata qualquer um dos seguintes problemas ao abrir o cliente web pela primeira vez, as seções a seguir informará o que fazer para corrigi-los.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>O que fazer se o navegador do usuário mostra um aviso de segurança quando tentarem acessar o cliente da web

A função acesso via Web RD talvez não estejam usando um certificado confiável. Verifique se que a função acesso via Web RD é configurada com um certificado confiável publicamente.

Se isso não funcionar, o nome do servidor da web URL do cliente pode não corresponder ao nome fornecido pelo certificado da Web da área de trabalho remota. Verifique se que a URL usa o FQDN do servidor que hospeda a função Web da área de trabalho remota.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>O que fazer se o usuário não pode se conectar a um recurso com o cliente da web, embora eles podem ver os itens em todos os recursos

Se o usuário relata que eles não é possível conectar com o cliente da web, mesmo que eles podem ver os recursos listados, verifique os seguintes itens:

* A função do Gateway de área de trabalho remota está configurada corretamente para usar um certificado público?
* O servidor de Gateway de área de trabalho remota tem as atualizações necessárias instaladas? Certifique-se de que o servidor tem [a atualização KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) instalado.

Se o usuário recebe um erro "o certificado de autenticação de servidor inesperado foi recebido" ao tentar conectar-se, em seguida, a mensagem mostrará a impressão digital do certificado da mensagem. Pesquise Gerenciador de certificados do servidor do agente de área de trabalho remota usando essa impressão digital para localizar o certificado correto. Verifique se o certificado é configurado para ser usado para a função de agente de área de trabalho remota na página de propriedades de implantação de área de trabalho remota. Depois de verificar se o certificado ainda não tiver expirado, copie o certificado no formato de arquivo. cer para o servidor de acesso via Web RD e execute o seguinte comando no servidor de acesso via Web RD com o valor entre colchetes substituído pelo caminho do arquivo do certificado:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnosticar problemas com o log de console

Se você não conseguir resolver o problema com base nas instruções para solução de problemas neste artigo, você pode tentar diagnosticar a origem do problema observando o console de log no navegador. O cliente da web fornece um método para registrar a atividade de log de console do navegador ao usar o cliente da web para ajudar a diagnosticar problemas.

* Selecione as reticências no canto superior direito e navegue até a **sobre** página no menu suspenso.
* Sob **informações de suporte de captura** selecionar a **inicie a gravação** botão.
* Execute as operações no cliente web que produziu o problema que você está tentando diagnosticar.
* Navegue até a **sobre** página e selecione **Parar gravação**.
* O navegador baixará automaticamente um arquivo. txt intitulado **logs de Console da área de trabalho remota**. Esse arquivo conterá a atividade de log de console completo gerada ao reproduzir o problema de destino.

O console também pode ser acessado diretamente por meio de seu navegador. O console geralmente está localizado em ferramentas de desenvolvedor. Por exemplo, você pode acessar o log no Microsoft Edge pressionando a **F12** chave ou selecionando as reticências, navegando para **mais ferramentas** > **Ferramentasdedesenvolvedor**.

## <a name="get-help-with-the-web-client"></a>Obtenha ajuda com o cliente da web

Se você tiver encontrado um problema que não pode ser resolvido com as informações neste artigo, você pode [-em um email](mailto:rdwbclnt@microsoft.com) para relatá-lo. Você também pode solicitar ou votar em novos recursos no nosso [caixa de sugestões](https://aka.ms/rdwebfbk).