---
title: Etapas de solução de problemas comuns do Windows Admin Center
description: Etapas de solução de problemas comuns do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 8e718eda7859c5e0b6949829c225b28e882525ad
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811712"
---
# <a name="troubleshooting-windows-admin-center"></a>Solução de problemas do Windows Admin Center

> Aplica-se a: Windows Admin Center, Windows Admin Center Preview

> [!Important]
> Este guia ajudará você a diagnosticar e resolver problemas que impedem o uso do Windows Admin Center. Se tiver problemas com uma ferramenta específica, verifique se você encontrou um [problema conhecido.](http://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-the-module-microsoftpowershelllocalaccounts-could-not-be-loaded"></a>Falha do instalador com a mensagem: **_Não foi possível carregar o módulo 'Microsoft.PowerShell.LocalAccounts'._ **

Isso pode acontecer se o caminho padrão do módulo do PowerShell foi modificado ou removido. Para resolver o problema, certifique-se de que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` é o **primeiro** item na sua variável de ambiente PSModulePath. Você pode obter isso com a seguinte linha do PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Recebo um erro **Essa página/site não pode ser acessado** em meu navegador da Web

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Se você instalou o Windows Admin Center como um **Aplicativo no Windows 10**

* Verifique para certificar-se de que o Windows Admin Center está em execução. Procure o ícone do Windows Admin Center ![](../media/trayIcon.PNG) na bandeja do sistema ou **área de trabalho do Windows Admin Center / SmeDesktop.exe** no Gerenciador de tarefas. Caso contrário, inicie o **Windows Admin Center** no Menu Iniciar.

> [!NOTE] 
> Após a reinicialização, você deve iniciar o Windows Admin Center no Menu Iniciar do Windows.  

* [Verificar a versão do Windows](#check-the-windows-version)

* Verifique se que você está usando o Microsoft Edge ou o Google Chrome como seu navegador da Web.

* Você selecionou o certificado correto na [primeira inicialização?](../use/get-started.md#selecting-a-client-certificate)

  * Tente abrir o navegador em uma sessão privada. Se isso funcionar, você deverá limpar o cache.

* Você recentemente atualizou o Windows 10 para um novo build ou versão?

  * Isso pode ter desmarcado suas configurações de hosts confiáveis. [Siga estas instruções para atualizar as configurações de hosts confiáveis.](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Se você instalou o Windows Admin Center como um **Gateway no Windows Server**

* Você atualizou de uma versão anterior do Windows Admin Center? Verifique se a regra de firewall não foi excluída devido à [esse problema conhecido](known-issues.md#upgrade). Use o comando do PowerShell abaixo para determinar se a regra existe. Caso contrário, siga [estas instruções](known-issues.md#upgrade) para recriá-lo.
    
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Verifique a versão do Windows](#check-the-windows-version) do cliente e servidor.

* Verifique se que você está usando o Microsoft Edge ou o Google Chrome como seu navegador da Web.

* No servidor, abra o Gerenciador de tarefas > Serviços e verifique se **ServerManagementGateway / Windows Admin Center** está em execução.
![](../media/Service-TaskMan.PNG)

* Testar a conexão de rede para o Gateway (substitua \<valores > com as informações da sua implantação)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Se você tiver instalado o Windows Admin Center em uma VM de servidor do Windows Azure

* [Verificar a versão do Windows](#check-the-windows-version)
* Você adicionou uma regra de porta de entrada para HTTPS? 
* [Saiba mais sobre como instalar o Windows Admin Center em uma VM do Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

### <a name="check-the-windows-version"></a>Verificar a versão do Windows

* Abra a caixa de diálogo Executar (tecla do Windows + R) e o inicie ```winver```.

* Se estiver usando o Windows 10 versão 1703 ou anterior, o Windows Admin Center não é suportado em sua versão do Microsoft Edge. Faça upgrade para uma versão recente do Windows 10 ou use o Chrome.

* Se você estiver usando uma versão de visualização do insider do Windows 10 ou Server com uma versão de compilação entre 17134 e 17637, o Windows tinham um bug que causava Windows Admin Center falhe. Use uma versão com suporte atual do Windows.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Verifique se que o serviço Windows Remote Management (WinRM) está em execução no computador do gateway e no nó gerenciado

* Abra a caixa de diálogo Executar com a tecla do Windows + R
* Tipo ```services.msc``` e pressione enter
* Na janela que é aberta, procure o Windows Remote Management (WinRM), verifique se ele está em execução e configurado para iniciar automaticamente

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>Você atualizou seu servidor de 2016 para 2019?

* Isso pode ter desmarcado suas configurações de hosts confiáveis. [Siga estas instruções para atualizar as configurações de hosts confiáveis.](#configure-trustedhosts) 

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>Recebo a mensagem: "Não é possível se conectar com segurança a esta página. Isso pode ocorrer porque o site usa configurações de segurança TLS desatualizadas ou unsafe.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
Seu computador está restrito a conexões de HTTP/2. Windows Admin Center usa autenticação integrada do Windows, que não é compatível com HTTP/2. Adicione os seguintes valores de registro de dois sob o ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` principais no **o computador que executa o navegador** para remover a restrição de HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Estou tendo problemas com as ferramentas de área de trabalho remota, eventos e o PowerShell.

Essas três ferramentas exigem o protocolo websocket, que normalmente é bloqueado por firewalls e servidores proxy. Se você estiver usando o Google Chrome, há uma [problema conhecido](known-issues.md#google-chrome) com autenticação de NTLM e websockets.

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Posso me conectar somente a alguns servidores

* Faça logon no computador do gateway localmente e tente ```Enter-PSSession <machine name>``` no PowerShell, substituindo \<nome do computador > com o nome da máquina que você está tentando gerenciar no Windows Admin Center. 

* Se o ambiente usa um grupo de trabalho em vez de um domínio, consulte [usar o Windows Admin Center em um grupo de trabalho](#using-windows-admin-center-in-a-workgroup).

* **Usando contas de administrador local:** Se você estiver usando uma conta de usuário local que não é a conta de administrador interno, você precisará habilitar a política no computador de destino executando o seguinte comando no PowerShell ou em um Prompt de comando como administrador no computador de destino:

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>Usar o Windows Admin Center em um grupo de trabalho

### <a name="what-account-are-you-using"></a>Qual conta está usando?
Verifique se as credenciais que você está usando são parte do grupo de administradores locais do servidor de destino. Em alguns casos, o WinRM também requer a participação no grupo de Usuários de gerenciamento remotos. Se estiver usando uma conta de usuário local que **não é a conta de administrador interno**, você deverá habilitar a diretiva no computador de destino, executando o seguinte comando no PowerShell ou em um Prompt de comando como administrador no computador de destino:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>Você está se conectando a uma máquina de grupo de trabalho em uma sub-rede diferente?

Para conectar-se a uma máquina de grupo de trabalho que não esteja na mesma sub-rede que o gateway, verifique se a porta do firewall para WinRM (TCP 5985) permite que o tráfego de entrada no computador de destino. Você pode executar o seguinte comando no PowerShell ou em um Prompt de comando como administrador no computador de destino para criar esta regra de firewall:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>Configurar TrustedHosts

Ao instalar o Windows Admin Center, você terá a opção de permitir que o Windows Admin Center gerencie configurações de TrustedHosts do gateway. Isso é necessário em um ambiente de grupo de trabalho ou ao usar credenciais de administrador local em um domínio. Caso opte por ignorar essa configuração, você deve configurar TrustedHosts manualmente.

**Para modificar TrustedHosts usando comandos do PowerShell:**

1. Abra uma sessão do PowerShell do administrador.
2. Veja sua configuração TrustedHosts atual:

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > Se a configuração atual da sua TrustedHosts não estiver vazia, os comandos a seguir substituirá a configuração. Recomendamos que você salve a configuração atual para um arquivo de texto com o seguinte comando para poder restaurá-lo se necessário:
   > 
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Defina TrustedHosts para o NetBIOS, IP ou FQDN das máquinas que você pretende gerenciar:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > Para uma maneira fácil de definir todas as TrustedHosts ao mesmo tempo, você pode usar um curinga.
   > 
   >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Quando terminar o teste, você pode emitir o seguinte comando a partir de uma sessão do PowerShell com privilégios elevados para limpar sua configuração de TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Se você tinha anteriormente exportou as configurações, abra o arquivo, copie os valores e use este comando:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>Anteriormente, eu teria Windows Admin Center instalado, e agora, nada mais pode usar a mesma porta de TCP/IP

Executar manualmente esses dois comandos em um prompt de comando elevado:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>Recursos do Azure não funcionam corretamente no Edge

Borda tem [problemas conhecidos](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relacionadas às zonas de segurança que afetam o logon do Azure no Windows Admin Center. Se você estiver tendo problemas ao usar os recursos do Azure ao usar o Edge, tente adicionar https://login.microsoftonline.com, https://login.live.com e a URL do seu gateway como sites confiáveis e para sites permitidos para as configurações do Bloqueador de pop-up de borda no seu navegador de lado do cliente. 

Para fazer isso:
1. Pesquise **opções da Internet** no Windows no Menu Iniciar
2. Vá para o **segurança** guia
3. Sob o **Sites confiáveis** opção, clique no **sites** botão e adicione as URLs na caixa de diálogo que é aberta. Você precisará adicionar a URL do gateway, bem como https://login.microsoftonline.com e https://login.live.com.
4. Vá para o **privacidade** guia
5. Sob o **Bloqueador de pop-up** seção, clique no **configurações** botão e adicione as URLs na caixa de diálogo que é aberta. Você precisará adicionar a URL do gateway, bem como https://login.microsoftonline.com e https://login.live.com.

## <a name="having-an-issue-with-an-azure-related-feature"></a>Há um problema com um recurso de Azure?

Envie-em um email wacFeedbackAzure@microsoft.com com as seguintes informações:
* Informações gerais do problema dos [perguntas listadas abaixo](#providing-feedback-on-issues).
* Descreva seu problema e as etapas que você seguiu para reproduzir o problema. 
* Você anteriormente registrar seu gateway para o Azure usando o script que pode ser baixado de New-AadApp.ps1 e, em seguida, atualize para a versão 1807? Ou você registrou seu gateway para o Azure usando a interface do usuário do gateway Configurações > Azure?
* É sua conta do Azure associada com vários diretórios/locatários?
    * Se Sim: Ao registrar o aplicativo do Azure AD para Windows Admin Center, era o diretório que você usou seu diretório padrão no Azure? 
* Sua conta do Azure tem acesso a várias assinaturas?
* A assinatura que você estava usando ter anexada a cobrança?
* Foram você conectado a várias contas do Azure quando você encontrou o problema?
* Sua conta do Azure exigem a autenticação multifator?
* É o computador que você está tentando gerenciar uma VM do Azure?
* Windows Admin Center é instalado em uma VM do Azure?

## <a name="providing-feedback-on-issues"></a>Fornecer comentários sobre problemas

Vá para o Visualizador de Eventos > Aplicativos e serviços > Microsoft-ServerManagementExperience e procure por erros ou avisos.

Registre um bug no nosso [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) que descreve seu problema.

Inclua qualquer erro ou aviso encontrado no log de eventos, assim como as seguintes informações: 

* Plataforma onde o Windows Admin Center está **instalado** (Windows 10 ou Windows Server):
    * Se instalado no servidor, o que é o Windows [versão](#check-the-windows-version) dos **o computador que executa o navegador** para acessar a Windows Admin Center: 
    * Você está usando o certificado autoassinado criado pelo instalador?
    * Se estiver usando seu próprio certificado, o nome do assunto corresponde ao computador?
    * Se estiver usando seu próprio certificado, ele especifica um nome de entidade alternativo?
* Você instalou com a configuração de porta padrão?
    * Caso contrário, qual porta você especificou?
* A máquina onde o Windows Admin Center está **instalado** está unida a um domínio?
* Windows [versão](#check-the-windows-version) onde o Windows Admin Center está **instalado**:
* O computador que você está **tentando gerenciar** está associado a um domínio?
* Windows [versão](#check-the-windows-version) do computador que você está **tentando gerenciar**:
* Qual navegador você está usando?
    * Se estiver usando o Google Chrome, qual é a versão? (Ajuda > Sobre o Google Chrome)

