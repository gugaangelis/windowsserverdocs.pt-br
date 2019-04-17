---
title: Configurar o cliente da web de Área de Trabalho Remota para seus usuários
description: Descreve como um administrador pode configurar o cliente da web de área de trabalho remota.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 491c94b25501732c4a4abda533cc62b8bd278ed2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099179"
---
# Configurar o cliente da web de Área de Trabalho Remota para seus usuários

O cliente da web de área de trabalho remota permite aos usuários acessar infraestrutura de área de trabalho remota da sua organização por meio de um navegador da web compatível. Eles poderá interagir com aplicativos remotos ou áreas de trabalho como fariam com um computador local, não importa onde eles estão. Depois que você configura seu cliente da web de área de trabalho remota, todos os usuários precisam para começar é a URL onde eles podem acessar o cliente, suas credenciais e um navegador da web com suporte.

>[!IMPORTANT]
>O cliente da web atualmente não oferece suporte usando o Proxy de aplicativo do Azure e não suporta Proxy de aplicativo Web. Consulte [Usando RDS com os serviços de proxy de aplicativo](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) para obter detalhes.

## O que você precisará configurar o cliente da web

Antes de começar, tenha o seguinte em mente:

* Verifique se que sua [implantação de área de trabalho remota](../rds-deploy-infrastructure.md) tem um Gateway de área de trabalho remota, um agente de Conexão de área de trabalho remota e acesso via Web em execução no Windows Server 2016 ou 2019.
* Verifique se que sua implantação estiver configurada para [por usuário licenças de acesso para cliente](../rds-client-access-license.md) (CALs) em vez de cada dispositivo, caso contrário, que serão consumidas todas as licenças.
* Instale o [Windows 10 KB4025334 atualizar](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) no Gateway de área de trabalho remota. Mais tarde atualizações cumulativas talvez já contém este KB.
* Certifique-se de certificados confiáveis públicos estão configurados para funções de Gateway de área de trabalho remota e acesso via Web.
* Verifique se todos os usuários se conectam aos computadores estão executando uma das seguintes versões do sistema operacional:
  * Windows 10
  * Windows Server 2008R2 ou posterior

Os usuários verão o melhor desempenho conectando-se para o Windows Server 2016 (ou posterior) e Windows 10 (versão 1611 ou posterior).

>[!IMPORTANT]
>Se você usou o cliente web durante o período de visualização e instalado uma versão antes 1.0.0, você deve primeiro desinstalar o cliente de antigo antes de passar para a nova versão. Si vous recevez une erreur qui dit «le client web a été installé à l’aide d’une version plus ancienne de RDWebClientManagement et doit d’abord être enlevé avant de déployer la nouvelle version», procédez comme suit:
>
>1. Abra um prompt do PowerShell com privilégios elevados.
>2. Execute **RDWebClientManagement módulo de desinstalação** para desinstalar o novo módulo.
>3. Feche e reabra o prompt do PowerShell com privilégios elevados.
>4. Execute **parâmetros-RequiredVersion módulo de instalação RDWebClientManagement \<old version> para instalar o módulo antigo.**
>5. Execute **RDWebClient de desinstalação** para desinstalar o cliente web antigo.
>6. Execute **RDWebClientManagement módulo de desinstalação** para desinstalar o módulo antigo.
>7. Feche e reabra o prompt do PowerShell com privilégios elevados.
>8. Continue com as etapas de instalação normal da seguinte maneira.

## Como publicar o cliente da web de área de trabalho remota

Para instalar o cliente da web pela primeira vez, siga estas etapas:

1. No servidor do agente de Conexão de área de trabalho remota, obtenha o certificado usado para conexões de área de trabalho remota e exportá-lo como um arquivo. cer. Copie o arquivo. cer de agente de Conexão de área de trabalho remota para o servidor que executa a função Web de área de trabalho remota.
2. No servidor de acesso via Web, abra um prompt do PowerShell com privilégios elevados.
3. No Windows Server 2016, atualize o módulo PowerShellGet desde a versão de caixa de entrada não dá suporte ao instalar o módulo de gerenciamento de cliente da web. Para atualizar o PowerShellGet, execute o seguinte cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >Você precisará reiniciar o PowerShell para que a atualização pode entrar em vigor, caso contrário, que o módulo pode não funcionar.

4. Instale o módulo de PowerShell de gerenciamento de cliente do área de trabalho remota da web da Galeria do PowerShell com este cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. Depois disso, execute o seguinte cmdlet para baixar a versão mais recente do cliente da web da área de trabalho remota:
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. Em seguida, execute este cmdlet com o valor entre colchetes substituído com o caminho do arquivo. cer que você copiou de agente de área de trabalho remota:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. Por fim, execute este cmdlet para publicar o cliente da web de área de trabalho remota:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    Verifique se você pode acessar o cliente da web na URL da web cliente com o nome do seu servidor, formatado como <https://server_FQDN/RDWeb/webclient/index.html>. É importante usar o nome do servidor que corresponda ao certificado público acesso via Web na URL (normalmente o FQDN do servidor).

    >[!NOTE]
    >Ao executar o cmdlet **Publicar RDWebClientPackage** , você poderá ver um aviso informando CALs por dispositivo que não têm suporte, mesmo que sua implantação estiver configurada para CALs por usuário. Se sua implantação usa CALs por usuário, você pode ignorar esse aviso. Vamos exibi-lo para garantir que você está ciente de que a limitação de configuração.
8. Quando você estiver pronto para os usuários acessarem o cliente da web, basta envie a URL de cliente da web criado.

>[!NOTE]
>Para ver uma lista de todos os cmdlets com suporte para o módulo RDWebClientManagement, execute o seguinte cmdlet do PowerShell:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## Como atualizar o cliente da web de área de trabalho remota

Quando uma nova versão do cliente da área de trabalho remota da web estiver disponível, siga estas etapas para atualizar a implantação com o novo cliente:

1. Abra um prompt do PowerShell com privilégios elevados no servidor de acesso via Web e execute o seguinte cmdlet para baixar a versão mais recente do cliente da web:
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. Opcionalmente, você pode publicar o cliente para testes antes do lançamento oficial executando este cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    O cliente deve aparecer na URL de teste que corresponde à sua URL de cliente da web (por exemplo, <https://server_FQDN/RDWeb/webclient-test/index.html>).
3. Publica o cliente para usuários executando o seguinte cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    Isso substituirá o cliente para todos os usuários quando eles reiniciar a página da web.

## Como desinstalar o cliente da web de área de trabalho remota

Para remover todos os rastreamentos do cliente da web, siga estas etapas:

1. No servidor de acesso via Web, abra um prompt do PowerShell com privilégios elevados.
2. Cancelar os clientes de teste e produção, desinstale todos os pacotes locais e remover as configurações de cliente da web:

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. Desinstale o módulo de PowerShell de gerenciamento de cliente do área de trabalho remota da web:

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## Como instalar o cliente de área de trabalho remota web sem uma conexão de internet

Siga estas etapas para implantar o cliente da web para um servidor de acesso via Web que não tem uma conexão de internet.

> [!NOTE]
> Instalando sem uma conexão de internet está disponível na versão 1.0.1 e acima do módulo do RDWebClientManagement PowerShell.

> [!NOTE]
> Você ainda precisa de um computador administrador com acesso à internet para baixar os arquivos necessários antes de transferi-los para o servidor offline.

> [!NOTE]
> O computador do usuário final precisa de uma conexão de internet por enquanto. Isso será corrigido em uma versão futura do cliente para fornecer um cenário offline completo.

### Um dispositivo com acesso à internet

1. Abra um prompt do PowerShell.

2. Importe o módulo do PowerShell área de trabalho remota da web cliente gerenciamento da Galeria do PowerShell:
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. Baixe a versão mais recente do cliente da web de área de trabalho remota para instalação em um dispositivo diferente de:
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. Baixe a versão mais recente do módulo do RDWebClientManagement PowerShell:
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. Copie o conteúdo de "C:\WebClient\" para o servidor de acesso via Web.

### O servidor de acesso via Web

Siga as instruções em [como publicar o cliente da web de área de trabalho remota](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client), substituindo as etapas 4 e 5 com o seguinte.

4. Importe o módulo de PowerShell de gerenciamento de cliente do área de trabalho remota da web da pasta local:
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. Implante a versão mais recente do cliente da área de trabalho remota da web da pasta local (substitua usando o arquivo zip apropriado):
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## Conectando-se ao agente sem Gateway da área de trabalho remota no Windows Server 2019
Esta seção descreve como habilitar uma conexão de cliente da web para um agente de área de trabalho remota sem um Gateway de área de trabalho remota no Windows Server 2019.

### Como configurar o servidor do agente de área de trabalho remota

#### Siga estas etapas se não houver nenhum certificado associado para o servidor do agente de área de trabalho remota

1. Abra o **Gerenciador do servidor** > **Serviços de área de trabalho remota**.

2. Na seção de **Visão geral da implantação** , selecione o menu suspenso de **tarefas** .

3. Selecione **Editar propriedades de implantação**, uma nova janela chamada **Propriedades de implantação** será aberto.

4. Na janela **Propriedades de implantação** , selecione **certificados** no menu à esquerda.

5. Na lista de níveis de certificado, selecione o **Agente de Conexão de área de trabalho remota - habilitar logon único**. Você tem duas opções: (1) crie um novo certificado ou (2) um certificado existente.

#### Siga estas etapas se houver um certificado associado anteriormente para o servidor do agente de área de trabalho remota

1. Abra o certificado associado para o agente e copie o valor de **impressão digital** .

2. Para vincular esse certificado à porta segura 3392, abra uma janela do PowerShell com privilégios elevados e execute o seguinte comando, substituindo **"impressão digital < gt _"** com o valor copiado da etapa anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para verificar se o certificado foi vinculado corretamente, execute o seguinte comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Na lista de associações de certificado SSL, certifique-se de que o certificado correto está ligado à porta 3392.

3. Abra o registro do Windows (regedit) e nagivate para ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e localize a chave **WebSocketURI**. O valor deve ser definido como **https://+:3392/rdp/**.

### Como configurar o Host de sessão de área de trabalho remota
Se o servidor de Host de sessão de área de trabalho remota é diferente do servidor de agente, siga estas etapas:

1. Criar um certificado para o computador de Host de sessão de área de trabalho remota, abri-lo e copie o valor de **impressão digital** .

2. Para vincular esse certificado à porta segura 3392, abra uma janela do PowerShell com privilégios elevados e execute o seguinte comando, substituindo **"impressão digital < gt _"** com o valor copiado da etapa anterior:

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > Para verificar se o certificado foi vinculado corretamente, execute o seguinte comando:
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > Na lista de associações de certificado SSL, certifique-se de que o certificado correto está ligado à porta 3392.

3. Abra o registro do Windows (regedit) e nagivate para ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` e localize a chave **WebSocketURI**. O valor deve ser definido como **https://+:3392/rdp/**.

### Observações gerais

* Certifique-se de que o Host de sessão de área de trabalho remota e o agente de servidor estiver executando o Windows Server 2019.

* Certifique-se de que pública confiável certificados são configurados para o Host de sessão de área de trabalho remota e o agente de servidor.
    > [!NOTE]
    > Se o Host de sessão de área de trabalho remota e o servidor do agente de área de trabalho remota compartilhem o mesmo computador, defina o certificado do servidor de agente somente. Se o servidor de Host de sessão de área de trabalho remota e o agente de usar máquinas diferentes, ambos devem ser configurados com certificados exclusivos.

* O **Nome de alternativa de assunto (SAN)** para cada certificado deve ser definido como da máquina **Totalmente o nome de domínio qualificado (FQDN)**. O **Nome comum (CN)** deve coincidir com a SAN para cada certificado.

## Solução de problemas

Se um usuário relatar qualquer um dos seguintes problemas ao abrir o cliente da web pela primeira vez, as seções a seguir informará o que fazer para corrigi-los.

### O que fazer se o navegador do usuário mostra um aviso de segurança quando tentarem acessar o cliente da web

A função de acesso via Web não pode estar usando um certificado confiável. Verifique se que a função de acesso via Web está configurada com um certificado confiável publicamente.

Se isso não funcionar, o nome do seu servidor na web URL do cliente pode não corresponder ao nome fornecido pelo certificado via Web. Certifique-se de que sua URL usa o FQDN do servidor que hospeda a função Web de área de trabalho remota.

### O que fazer se o usuário não pode se conectar a um recurso com o cliente web mesmo que ele possa ver os itens em todos os recursos

Se o usuário relata que eles não podem se conectar com o cliente da web mesmo que ele possa ver os recursos listados, verifique o seguinte:

* A função de Gateway de área de trabalho remota está configurada corretamente para usar um certificado público confiável?
* O servidor de Gateway de área de trabalho remota têm as atualizações necessárias instaladas? Certifique-se de que o servidor tem [o KB4025334 atualizar](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334) instalado.

Se o usuário recebe um erro "certificado de autenticação de servidor inesperado foi recebido" mensagem quando tentarem se conectar, em seguida, a mensagem mostrará a impressão digital do certificado. Pesquise o Gerenciador de certificados do servidor de agente usando essa impressão digital para localizar o certificado correto. Verificar se o certificado está configurado para ser usado para a função de agente na página de propriedades de implantação de área de trabalho remota. Depois, certificando-se o certificado não tiver expirado, copiar o certificado no formato de arquivo. cer para o servidor de acesso via Web e execute o seguinte comando no servidor de acesso via Web com o valor entre colchetes substituído pelo caminho do arquivo do certificado:

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### Diagnosticar problemas com o log de console

Se você não pode resolver o problema com base na solução de problemas as instruções neste artigo, você pode tentar diagnosticar a origem do problema observando o console de fazer logon no navegador. O cliente da web fornece um método para registrar a atividade de log de console do navegador enquanto estiver usando o cliente da web para ajudar a diagnosticar problemas.

* Selecione as reticências no canto superior direito e navegue até a página **sobre** no menu suspenso.
* Em **informações de suporte de captura** , selecione o botão **Iniciar a gravação** .
* Execute as operações no cliente web que gerou o problema que você está tentando diagnosticar.
* Navegue até a página **sobre** e selecione **Parar a gravação**.
* O navegador baixará automaticamente um arquivo. txt intitulado **Logs do Console de área de trabalho remota**. Esse arquivo irá conter a atividade de log de console completo gerada durante a reprodução o problema de destino.

O console também pode ser acessado diretamente por meio de seu navegador. Em geral, o console está localizado nas ferramentas de desenvolvedor. Por exemplo, você pode acessar o log no Microsoft Edge, pressionando a tecla **F12** ou selecionando a reticências e navegar para **mais ferramentas** > **Ferramentas de desenvolvedor**.

## Obter ajuda com o cliente da web

Se você encontrou um problema que não possa ser resolvido as informações neste artigo, você poderá [envie o email](mailto:rdwbclnt@microsoft.com) para relatar. Você também pode solicitar ou vote em nossa [caixa de sugestão](https://aka.ms/rdwebfbk)de novos recursos.