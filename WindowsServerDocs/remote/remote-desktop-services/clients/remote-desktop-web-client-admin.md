---
title: Configurar o cliente Web da Área de Trabalho Remota para seus usuários
description: Descreve como um administrador pode configurar o cliente de Web da Área de Trabalho Remota.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 09/19/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: a8521eae302ade84904e3ba09c001eac21fffd6a
ms.sourcegitcommit: f0fcfee992b76f1ad5dad460d4557f06ee425083
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77125147"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>Configurar o cliente Web da Área de Trabalho Remota para seus usuários

O cliente da Web de Área de Trabalho Remota permite que os usuários acessem a infraestrutura de Área de Trabalho Remota da sua organização por um navegador da Web compatível. Eles poderão interagir com aplicativos remotos ou áreas de trabalho, como fariam com um computador local, independentemente de onde estiverem. Depois que você configurar seu cliente de Web da Área de Trabalho Remota, tudo que seus usuários precisam para começar é a URL pela qual poderão acessar o cliente, as credenciais e um navegador da Web com suporte.

>[!IMPORTANT]
>O cliente da Web não dá suporte a usar ao Proxy de Aplicativo do Azure e não dá suporte a Proxy de Aplicativo Web. Confira [Usando o RDS com serviços de proxy de aplicativo](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) para obter detalhes.

## <a name="what-youll-need-to-set-up-the-web-client"></a>O que você precisará para configurar o cliente Web

Antes de começar, tenha o seguinte em mente:

* Certifique-se de sua [Implantação de Área de Trabalho Remota](../rds-deploy-infrastructure.md) tenha um Gateway de Área de Trabalho Remota, um Agente de Conexão de Área de Trabalho Remota e acesso à Área de Trabalho Remota pela Web em execução no Windows Server 2016 ou 2019.
* Verifique se sua implantação está configurada para [licenças de acesso para cliente por usuário](../rds-client-access-license.md) (CALs) em vez de por dispositivo, caso contrário, todas as licenças serão consumidas.
* Instale a [Atualização do Windows 10 KB4025334](https://support.microsoft.com/help/4025334/windows-10-update-kb4025334) no Gateway de Área de Trabalho Remota. Atualizações cumulativas posteriores já podem conter esta KB.
* Certifique-se de que certificados confiáveis públicos estejam configurados para as funções de Gateway de Área de Trabalho Remota e Acesso pela Web à Área de Trabalho Remota.
* Verifique se todos os computadores usados por seus usuários para se conectar estão executando uma das seguintes versões de sistema operacional:
  * Windows 10
  * Windows Server 2008R2 ou posterior

Os usuários terão um melhor desempenho se conectando ao Windows Server 2016 (ou posterior) e Windows 10 (versão 1611 ou posterior).

>[!IMPORTANT]
>Se você usou o cliente da Web durante o período de teste e instalou uma versão anterior à 1.0.0, será necessário primeiro desinstalar o cliente antigo antes de passar para a nova versão. Caso receba um erro com a mensagem "O cliente da Web foi instalado usando uma verão do RDWebClientManagement e precisa ser removido antes da implantação de uma nova versão", siga estas etapas:
>
>1. Abra um prompt do PowerShell elevado.
>2. Execute **Uninstall-Module RDWebClientManagement** para desinstalar o novo módulo.
>3. Feche e reabra o prompt do PowerShell com privilégios elevados.
>4. Execute **Install-Module RDWebClientManagement -RequiredVersion \<old version> para instalar o módulo antigo.**
>5. Execute **Uninstall-RDWebClient** para desinstalar o cliente da Web antigo.
>6. Execute **Uninstall-Module RDWebClientManagement** para desinstalar o módulo antigo.
>7. Feche e reabra o prompt do PowerShell com privilégios elevados.
>8. Continue com as etapas de instalação normais da seguinte maneira.

## <a name="how-to-publish-the-remote-desktop-web-client"></a>Como publicar o cliente Web de Área de Trabalho Remota

Para instalar o cliente da Web pela primeira vez, siga estas etapas:

1. No servidor do Agente de Conexão de Área de Trabalho Remota, obtenha o certificado usado para conexões de Área de Trabalho Remota e exporte-o como um arquivo .cer. Copie o arquivo. cer do Agente de Conexão de Área de Trabalho Remota para o servidor que executa a função Web da Área de Trabalho Remota.
2. No servidor de acesso via Web RD, abra um prompt elevado do PowerShell.
3. No Windows Server 2016, atualize o módulo PowerShellGet, pois a versão da caixa de entrada não dá suporte à instalação do módulo de gerenciamento de cliente da Web. Para atualizar o PowerShellGet, execute o seguinte cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Você precisará reiniciar o PowerShell para a atualização entrar em vigor, caso contrário, o módulo poderá não funcionar.

4. Instale o módulo do PowerShell do gerenciamento de cliente da Web de Área de Trabalho Remota da galeria do PowerShell com este cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Depois disso, execute o seguinte cmdlet para baixar a versão mais recente do cliente da Web da Área de Trabalho Remota:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Execute este cmdlet com o valor entre colchetes substituído pelo caminho do arquivo .cer que você copiou do Agente de Área de Trabalho Remota:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Por fim, execute este cmdlet para publicar o cliente da Web de Área de Trabalho Remota:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Verifique se você pode acessar o cliente da Web na URL de cliente da Web com o nome do servidor, formatado como <https://server_FQDN/RDWeb/webclient/index.html>. É importante usar o nome do servidor que corresponde ao certificado público de Acesso via Web à Área de Trabalho Remota na URL (normalmente o FQDN do servidor).

    >[!NOTE]
    >Ao executar o cmdlet **Publish-RDWebClientPackage**, você poderá ver um aviso que informa que CALs por dispositivo não são compatíveis, mesmo se a implantação estiver configurada para CALs por usuário. Se sua implantação usa CALs por usuário, você pode ignorar este aviso. Vamos exibi-lo para garantir que você esteja ciente da limitação de configuração.
8. Quando você estiver pronto para que os usuários acessem o cliente da Web, basta enviar para eles a URL do cliente Web criada por você.

>[!NOTE]
>Para ver uma lista de todos os cmdlets com suporte no módulo RDWebClientManagement, execute o seguinte cmdlet do PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>Como atualizar o cliente Web de Área de Trabalho Remota

Quando uma nova versão do cliente da Web da Área de Trabalho Remota estiver disponível, siga estas etapas para atualizar a implantação com o novo cliente:

1. Abra um prompt elevado do PowerShell no servidor de Acesso via Web da Área de Trabalho Remota e execute o seguinte cmdlet para baixar a versão mais recente do cliente da Web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Como alternativa, você pode publicar o cliente para teste antes do lançamento oficial executando este cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    O cliente deve aparecer na URL de teste que corresponde à URL do seu cliente Web (por exemplo, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publique o cliente para usuários executando o seguinte cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Isso substituirá o cliente para todos os usuários quando eles reiniciarem a página da Web.

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>Como desinstalar o cliente Web de Área de Trabalho Remota

Para remover todos os vestígios do cliente da Web, siga estas etapas:

1. No servidor de acesso via Web RD, abra um prompt elevado do PowerShell.
2. Cancele a publicação dos clientes de Teste e Produção, desinstale todos os pacotes locais e remova as configurações do cliente da Web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Desinstale o módulo do PowerShell do gerenciamento de cliente da Web de Área de Trabalho Remota:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>Como instalar o cliente da Web de Área de Trabalho Remota sem uma conexão de internet

Siga estas etapas para implantar o cliente da Web em um servidor de Acesso pela Web à Área de Trabalho Remota que não tem uma conexão de internet.

> [!NOTE]
> A instalação sem uma conexão com a internet está disponível na versão 1.0.1 e posterior do módulo RDWebClientManagement do PowerShell.

> [!NOTE]
> Você ainda precisa de um computador de administrador com acesso à internet para baixar os arquivos necessários antes de transferi-los para o servidor offline.

> [!NOTE]
> O computador do usuário final precisa de uma conexão de internet por enquanto. Isso será corrigido em uma versão futura do cliente para fornecer um cenário offline completo.

### <a name="from-a-device-with-internet-access"></a>De um dispositivo com acesso à internet

1. Abra um prompt do PowerShell.

2. Importe o módulo do PowerShell do gerenciamento de cliente da Web de Área de Trabalho Remota da galeria do PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Baixe a versão mais recente do cliente da Web de Área de Trabalho Remota para instalação em um dispositivo diferente:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Baixe a versão mais recente do módulo RDWebClientManagement do PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copie o conteúdo de "C:\WebClient\" para o servidor de acesso via Web à Área de Trabalho Remota.

### <a name="from-the-rd-web-access-server"></a>No servidor de Acesso via Web da Área de Trabalho Remota

Siga as instruções em [Como publicar o cliente da Web de Área de Trabalho Remota](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), substituindo as etapas 4 e 5 pelo seguinte.

4. Você tem duas opções para recuperar o módulo do PowerShell de gerenciamento de cliente Web mais recente:
    - Importe o módulo do PowerShell de gerenciamento de cliente Web de Área de Trabalho Remota:
      ```PowerShell
      Import-Module -Name RDWebClientManagement
      ```
    - Copie a pasta RDWebClientManagement baixada para uma das pastas locais do módulo do PowerShell em **$env:psmodulePath** ou adicione o caminho para a pasta com os arquivos baixados para o **$env:psmodulePath**.

5. Implante a versão mais recente do cliente da Web da Área de Trabalho Remota pela pasta local (substituir pelo arquivo zip apropriado):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>Conecte-se ao Agente de Área de Trabalho Remota sem o Gateway de Área de Trabalho Remota no Windows Server de 2019
Esta seção descreve como habilitar uma conexão de cliente da Web com um Agente de Área de Trabalho Remota sem um Gateway de Área de Trabalho Remota no Windows Server 2019.

### <a name="setting-up-the-rd-broker-server"></a>Como configurar o servidor do Agente de Área de Trabalho Remota

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>Siga estas etapas se não houver nenhum certificado associado ao servidor do Agente de Área de Trabalho Remota

1. Abra **Gerenciador de Servidores** > **Serviços de Área de Trabalho Remota**.

2. Na seção **Visão Geral da Implantação**, selecione o menu suspenso **Tarefas**.

3. Selecione **Editar Propriedades de Implantação**, uma nova janela intitulada **Propriedades de Implantação** será aberta.

4. Na janela **Propriedades de Implantação**, selecione **Certificados** no menu à esquerda.

5. Na lista de Níveis de Certificado, selecione **Agente de Conexão de Área de Trabalho Remota – Habilitar Logon Único**. Você tem duas opções: (1) crie um novo certificado ou (2) um certificado existente.

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>Siga estas etapas se houver um certificado anteriormente associado ao servidor do Agente de Área de Trabalho Remota

1. Abra o certificado associado ao Agente e copie o valor de **Impressão Digital**.

2. Para associar esse certificado à porta segura 3392, abra uma janela do PowerShell com privilégios elevados e execute o seguinte comando, substituindo **"< thumbprint >"** pelo valor copiado na etapa anterior:

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

3. Abra o registro do Windows (regedit), navegue até ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e localize a chave **WebSocketURI**. O valor deve ser definido como <strong>https://+:3392/rdp/</strong>.

### <a name="setting-up-the-rd-session-host"></a>Como configurar o Host de Sessão de Área de Trabalho Remota
Siga estas etapas se o servidor de Host da Sessão de Área de Trabalho Remota for diferente do servidor do Agente de Área de Trabalho Remota:

1. Crie um certificado para a máquina do Host de Sessão de Área de Trabalho Remota, abra e copie o valor de **Impressão Digital**.

2. Para associar esse certificado à porta segura 3392, abra uma janela do PowerShell com privilégios elevados e execute o seguinte comando, substituindo **"< thumbprint >"** pelo valor copiado na etapa anterior:

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

3. Abra o registro do Windows (regedit), navegue até ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e localize a chave **WebSocketURI**. O valor deve ser definido como <https://+:3392/rdp/>.

### <a name="general-observations"></a>Observações gerais

* Certifique-se de que o Host de Sessão de Área de Trabalho Remota e o servidor do Agente de Área de Trabalho Remota estejam executando o Windows Server 2019.

* Certifique-se de que certificados públicos confiáveis estejam configurados tanto no Host de Sessão de Área de Trabalho Remota quanto no servidor do Agente de Área de Trabalho Remota.
    > [!NOTE]
    > Caso o Host de Sessão de Área de Trabalho Remota e o servidor de Agente Área de Trabalho Remota compartilhem a mesma máquina, defina apenas o certificado do servidor do Agente de Área de Trabalho Remota. Se o Host da Sessão de Área de Trabalho Remota e o servidor do Agente de Área de Trabalho Remota usarem máquinas diferentes, ambos precisam ser configurados com certificados exclusivos.

* O **Nome Alternativo da Entidade (SAN)** para cada certificado deve ser definido para o **Nome de Domínio Totalmente Qualificado (FQDN)** da máquina. O **Nome Comum (CN)** precisa corresponder ao SAN de cada certificado.

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>Como predefinir as configurações para usuários de cliente da Web da Área de Trabalho Remota
Esta seção informa como usar o PowerShell para definir configurações da implantação do cliente da Web da Área de Trabalho Remota. Esses cmdlets do PowerShell controlam a capacidade do usuário de alterar as configurações com base em questões de segurança da organização ou ao fluxo de trabalho pretendido. As configurações a seguir estão todas localizadas no painel lateral **Configurações** cliente da Web.

### <a name="suppress-telemetry"></a>Suprimir a telemetria
Por padrão, os usuários podem optar por habilitar ou desabilitar a coleta de dados de telemetria que são enviados à Microsoft. Para saber mais sobre os dados de telemetria que a Microsoft coleta, confira a Declaração de Privacidade no link no painel lateral **Sobre**.

Como administrador, você pode escolher suprimir a coleta de telemetria para sua implantação usando o seguinte cmdlet do PowerShell:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "SuppressTelemetry" $true
   ```

Por padrão, o usuário pode optar por habilitar ou desabilitar a telemetria. Um valor booliano **$false** corresponderá ao comportamento do cliente padrão. Um valor booliano **$true** desabilita a telemetria e restringe a habilitação da telemetria por parte do usuário.

### <a name="remote-resource-launch-method"></a>Método de inicialização do recurso remoto

>[!NOTE]
>No momento, essa configuração só funciona com o cliente Web RDS, não com o cliente Web da Área de Trabalho Virtual do Windows.

Por padrão, os usuários podem optar por iniciar recursos remotos (1) no navegador ou (2) baixando um arquivo .rdp para lidar com outro cliente instalado em seu computador. Como administrador, você pode optar por restringir o método de inicialização do recurso remoto para sua implantação com o seguinte comando do Powershell:

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser" ($true|$false)
   ```
 Por padrão, o usuário pode selecionar qualquer um dos métodos de inicialização. Um valor booliano **$true** forçará o usuário a iniciar os recursos no navegador. Um valor booliano **$false** forçará o usuário a iniciar os recursos baixando um arquivo .rdp para lidar com um cliente RDP instalado localmente.

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>Redefinir configurações de RDWebClientDeploymentSetting para o padrão

Para redefinir uma configuração de cliente Web no nível de implantação para a configuração padrão, execute o seguinte cmdlet do PowerShell e use o parâmetro -name para especificar a configuração que você deseja redefinir:

   ```PowerShell
    Reset-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser"
    Reset-RDWebClientDeploymentSetting -Name "SuppressTelemetry"
   ```

## <a name="troubleshooting"></a>Solução de problemas

Se um usuário relatar qualquer um dos seguintes problemas ao abrir o cliente Web pela primeira vez, as seções a seguir informarão o que fazer para corrigi-los.

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>O que fazer se o navegador do usuário mostra um aviso de segurança quando ele tenta acessar o cliente da Web

A função Acesso via Web à Área de Trabalho Remota talvez não esteja usando um certificado confiável. Verifique se a função Acesso via Web à Área de Trabalho Remota está configurada com um certificado confiável publicamente.

Se isso não funcionar, o nome do servidor na URL do cliente da Web pode não corresponder ao nome fornecido pelo certificado da Web da Área de Trabalho Remota. Verifique se que a URL usa o FQDN do servidor que hospeda a função Web da Área de Trabalho Remota.

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>O que fazer se o usuário não conseguir se conectar a um recurso com o cliente da Web, mesmo que consigam ver os itens em Todos os Recursos

Se o usuário relata que não consegue se conectar ao cliente da Web, mesmo que consiga ver os recursos listados, verifique os seguintes itens:

* A função do Gateway de Área de Trabalho Remota está configurada corretamente para usar um certificado público confiável?
* O servidor de Gateway de Área de Trabalho Remota tem as atualizações necessárias instaladas? Certifique-se de que o servidor tem [a atualização KB4025334](https://support.microsoft.com/help/4025334/windows-10-update-kb4025334) instalada.

Se o usuário receber um erro "certificado de autenticação de servidor inesperado recebido" ao tentar se conectar, a mensagem mostrará a impressão digital do certificado. Pesquise o gerenciador de certificados do servidor do agente de Área de Trabalho Remota usando essa impressão digital para localizar o certificado correto. Verifique se o certificado está configurado para ser usado para a função de Agente de Área de Trabalho Remota na página de propriedades da implantação de Área de Trabalho Remota. Depois de verificar se o certificado não expirou, copie o certificado no formato de arquivo .cer para o servidor de Acesso via Web à Área de Trabalho Remota e execute o seguinte comando no servidor de Acesso via Web RD à Área de Trabalho Remota com o valor entre colchetes substituído pelo caminho do arquivo do certificado:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>Diagnosticar problemas com o log de console

Se não conseguir resolver o problema com base nas instruções de solução de problemas neste artigo, você pode tentar diagnosticar a origem do problema observando o log do console no navegador. O cliente da Web fornece um método para registrar a atividade de log de console do navegador ao usar o cliente da Web para ajudar a diagnosticar problemas.

* Selecione as reticências no canto superior direito e navegue até a página **Sobre** no menu suspenso.
* Em **Capturar informações de suporte**, selecione o botão **Iniciar gravação**.
* Execute as operações que produziram o problema que você está tentando diagnosticar no cliente Web.
* Navegue até a página **Sobre** e selecione **Parar gravação**.
* O navegador baixará automaticamente um arquivo .txt intitulado **RD Console Logs.txt**. Esse arquivo conterá a atividade de log completa do console gerada ao reproduzir o problema em questão.

O console também pode ser acessado diretamente pelo navegador. O console geralmente está localizado nas ferramentas de desenvolvedor. Por exemplo, você pode acessar o log no Microsoft Edge pressionando a tecla **F12** ou selecionando as reticências e navegando até **Mais ferramentas** > **Ferramentas de Desenvolvedor**.

## <a name="get-help-with-the-web-client"></a>Obter ajuda com o cliente Web

Caso tenha encontrado um problema que não possa ser resolvido com as informações descritas neste artigo, relate-o na [Tech Community](https://aka.ms/wvdtc). Você também pode solicitar ou votar em novos recursos em nossa [caixa de sugestões](https://remotedesktop.uservoice.com/forums/911494-remote-desktop-web-client).
