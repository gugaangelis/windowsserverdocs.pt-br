---
title: Etapas de solução de problemas comuns do Windows Admin Center
description: Etapas de solução de problemas comuns do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/12/2019
ms.openlocfilehash: a91a8dcf6f05ef0ef66dee603851150b2145d559
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296865"
---
# Solução de problemas do Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

> [!Important]
> Este guia ajudará você a diagnosticar e resolver problemas que impedem o uso do Windows Admin Center. Se tiver problemas com uma ferramenta específica, verifique se você encontrou um [problema conhecido.](http://aka.ms/wacknownissues)

<a id="toc"></a>

## Links rápidos

* [O instalador falha com a mensagem: ** _o módulo 'Microsoft.PowerShell.LocalAccounts' não pôde ser carregado._**](#psmodulepath)

* Recebo um erro **Esta página/site não pode ser acessado** em meu navegador (selecione o tipo de implantação)
    * [O Windows Admin Center está instalado como um aplicativo no Windows 10](#whitescreenw10)
    * [O Windows Admin Center está instalado como um Gateway no Windows Server](#whitescreenws)
    * [O Windows Admin Center está instalado como um Gateway em uma VM do Azure](#whitescreenazvm)

* [Windows Admin Center página inicial é carregada, mas fico preso no painel Adicionar Conexão ou não é possível conectar-se a qualquer computador.](#winvercompat)

* [Recebo a mensagem: "Erro ao carregar o módulo. RPC: tentativas expiradas de 'Ping'."](#winvercompat)

* [Recebo a mensagem: "não é possível se conectar com segurança a essa página. Isso pode ser porque o site usa as configurações de segurança TLS desatualizadas ou não seguras."](#tls)

* [Estou tendo problemas com as ferramentas de área de trabalho remota, eventos e PowerShell.](#websockets)

* [Estou tendo problemas usando recursos do Azure no Edge](#azlogin)

* [Posso me conectar somente a alguns servidores](#connectionissues)

* [Estou usando o Windows Admin Center em um **grupo de trabalho**](#workgroup)

* [Posso anteriormente tinha o Windows Admin Center está instalado, e agora nada mais pode usar a mesma porta TCP/IP](#urlacl)

* [Meu problema não está listado aqui ou as etapas nesta página não resolveram o problema.](#filebug)

<a id="psmodulepath"></a>

## Instalador falha com a mensagem: ** _o módulo 'Microsoft.PowerShell.LocalAccounts' não pôde ser carregado._**

Isso pode acontecer se o seu caminho de módulo de PowerShell padrão foi modificado ou removido. Para resolver o problema, verifique se ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` é o **primeiro** item na sua variável de ambiente PSModulePath. Você pode obter isso com a seguinte linha do PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## Recebo um erro **Essa página/site não pode ser acessado** em meu navegador da Web

<a id="whitescreenw10"></a>

### Se você instalou o Windows Admin Center como um **Aplicativo no Windows 10**

* Verifique para certificar-se de que o Windows Admin Center está em execução. Procure o ícone do Windows Admin Center ![](../media/trayIcon.PNG) na bandeja do sistema ou **área de trabalho do Windows Admin Center / SmeDesktop.exe** no Gerenciador de tarefas. Caso contrário, inicie o **Windows Admin Center** no Menu Iniciar.

> [!NOTE] 
> Após a reinicialização, você deve iniciar o Windows Admin Center no Menu Iniciar do Windows.  

* [Verificar a versão do Windows](#winvercompat)

* Verifique se que você está usando o Microsoft Edge ou o Google Chrome como seu navegador da Web.

* Você selecionou o certificado correto na [primeira inicialização?](../use/get-started.md#selecting-a-client-certificate)

  * Tente abrir o navegador em uma sessão privada. Se isso funcionar, você deverá limpar o cache.

* Você recentemente atualizaram Windows 10 para uma nova compilação ou versão?

  * Isso pode ter desmarcado suas configurações de hosts confiáveis. [Siga estas instruções para atualizar suas configurações de hosts confiáveis.](#configure-trustedhosts) 

[[voltar ao início]](#toc)

<a id="whitescreenws"></a>

### Se você instalou o Windows Admin Center como um **Gateway no Windows Server**

* Você upgrade de uma versão anterior do Windows Admin Center? Verifique se que a regra de firewall não foi excluída devido a [esse problema conhecido](known-issues.md#upgrade). Use o comando do PowerShell abaixo para determinar se a regra existe. Caso contrário, siga [estas instruções](known-issues.md#upgrade) para recriá-la.
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Verifique a versão do Windows](#winvercompat) do cliente e servidor.

* Verifique se que você está usando o Microsoft Edge ou o Google Chrome como seu navegador da Web.

* No servidor, abra o Gerenciador de tarefas gt _ serviços e verifique se **ServerManagementGateway / Windows Admin Center** está em execução.
![](../media/Service-TaskMan.PNG)

* Teste a conexão de rede com o Gateway (substituir \<values> pelas informações da implantação)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[voltar ao início]](#toc)

<a id="whitescreenazvm"></a>  

### Se você instalou o Windows Admin Center em uma VM do Azure Windows Server

* [Verifique a versão do Windows](#winvercompat)
* Você adicionou uma regra de porta de entrada para HTTPS? 
* [Saiba mais sobre como instalar o Windows Admin Center em uma VM do Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[voltar ao início]](#toc)

<a id="winvercompat"></a>

### Verificar a versão do Windows

* Abra a caixa de diálogo Executar (tecla do Windows + R) e o inicie ```winver```.

* Se estiver usando o Windows 10 versão 1703 ou anterior, o Windows Admin Center não é suportado em sua versão do Microsoft Edge. Faça upgrade para uma versão recente do Windows 10 ou use o Chrome.

* Se você estiver usando uma versão de visualização de insider do Windows 10 ou Server com uma versão de compilação entre 17134 e 17637, o Windows tinha um bug que causou o Windows Admin Center falha. Use uma versão atual com suporte do Windows.

### Verifique se que o serviço Windows Remote Management (WinRM) está em execução no computador de gateway e no nó gerenciado

* Abra a caixa de diálogo Executar com a tecla do Windows + R
* Tipo ```services.msc``` e pressione enter
* Na janela que é aberto, procure o Windows Remote Management (WinRM), verifique se ele está em execução e definido para ser iniciado automaticamente

### Você atualizar seu servidor do 2016 para 2019?

* Isso pode ter desmarcado suas configurações de hosts confiáveis. [Siga estas instruções para atualizar suas configurações de hosts confiáveis.](#configure-trustedhosts) 

[[voltar ao início]](#toc)

<a id="tls"></a>

## Recebo a mensagem: "não é possível se conectar com segurança a essa página. Isso pode ser porque o site usa as configurações de segurança TLS desatualizadas ou não seguras.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
Seu computador está restrito a conexões HTTP/2. Windows Admin Center usa a autenticação integrada do Windows, que não é compatível com HTTP/2. Adicione os seguintes valores do duas registro sob o ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` chave no **computador executando o navegador** para remover a restrição de HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[voltar ao início]](#toc)

<a id="websockets"></a> 

## Estou tendo problemas com as ferramentas de área de trabalho remota, eventos e PowerShell.

Essas três ferramentas exigem o protocolo websocket, que normalmente é bloqueado por firewalls e servidores proxy. Se você estiver usando o Google Chrome, há um [problema conhecido](known-issues.md#google-chrome) com websockets e a autenticação NTLM.

[[voltar ao início]](#toc)


<a id="connectionissues"></a> 

## Posso me conectar a alguns servidores, mas não a outros
* Entre no computador de gateway localmente e tente ```Enter-PSSession <machine name>``` no PowerShell, substituindo \<machine name> pelo nome do computador que você está tentando gerenciar no Windows Admin Center. 

* Se o ambiente usa um grupo de trabalho em vez de um domínio, consulte [usar o Windows Admin Center em um grupo de trabalho](#workgroup).

* **Usar contas de administrador local:** se estiver usando uma conta de usuário local que não é a conta de administrador interno, você deverá habilitar a política na máquina de destino executando o seguinte comando no PowerShell ou em um Prompt de comando como administrador no computador de destino:

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[voltar ao início]](#toc)

<a id="workgroup"></a>

## Usar o Windows Admin Center em um grupo de trabalho 

### Qual conta está usando?
Verifique se as credenciais que você está usando são parte do grupo de administradores locais do servidor de destino. Em alguns casos, o WinRM também requer a participação no grupo de Usuários de gerenciamento remotos. Se estiver usando uma conta de usuário local que **não é a conta de administrador interno**, você deverá habilitar a diretiva no computador de destino, executando o seguinte comando no PowerShell ou em um Prompt de comando como administrador no computador de destino:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### Você está se conectando a uma máquina de grupo de trabalho em uma sub-rede diferente?

Para conectar-se a uma máquina de grupo de trabalho que não esteja na mesma sub-rede que o gateway, verifique se a porta do firewall para WinRM (TCP 5985) permite que o tráfego de entrada no computador de destino. Você pode executar o seguinte comando no PowerShell ou em um Prompt de comando como administrador no computador de destino para criar esta regra de firewall:

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### Configurar TrustedHosts

Ao instalar o Windows Admin Center, você terá a opção de permitir que o Windows Admin Center gerencie configurações de TrustedHosts do gateway. Isso é necessário em um ambiente de grupo de trabalho ou ao usar credenciais de administrador local em um domínio. Caso opte por ignorar essa configuração, você deve configurar TrustedHosts manualmente.

**Para modificar TrustedHosts usando comandos do PowerShell:**

1. Abra uma sessão do PowerShell do administrador.
2. Veja sua configuração TrustedHosts atual:

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

    > [!WARNING]
    > Se a configuração atual da sua TrustedHosts não estiver vazia, os comandos a seguir substituirá a configuração. Recomendamos que você salve a configuração atual para um arquivo de texto com o seguinte comando para poder restaurá-lo se necessário:

    > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Defina TrustedHosts para o NetBIOS, IP ou FQDN das máquinas que você pretende gerenciar:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

    > [!TIP] 
    >Para uma maneira fácil de definir todas as TrustedHosts ao mesmo tempo, você pode usar um curinga.

    >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Quando terminar o teste, você pode emitir o seguinte comando a partir de uma sessão do PowerShell com privilégios elevados para limpar sua configuração de TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Se você tinha anteriormente exportou as configurações, abra o arquivo, copie os valores e use este comando:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

[[voltar ao início]](#toc)

<a id="urlacl"></a>

## Posso anteriormente tinha o Windows Admin Center está instalado, e agora nada mais pode usar a mesma porta TCP/IP

Execute manualmente esses dois comandos em um prompt de comando com privilégios elevados:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[voltar ao início]](#toc)

<a id="azlogin"></a>

## Estou tendo problemas usando recursos do Azure no Edge

Edge tem [problemas conhecidos](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) relacionados a zonas de segurança que afetam o logon do Azure no Centro de administração do Windows. Se você estiver tendo problemas usando os recursos do Azure ao usar o Edge, tente adicionar https://login.microsoftonline.com, https://login.live.com e a URL do seu gateway como sites confiáveis e para sites permitidos para configurações de pop-ups Edge seu navegador no lado do cliente. 

Para fazer isso:
1. Pesquisa de **Opções da Internet** no Menu Iniciar do Windows
2. Vá para a guia **segurança**
3. Sob a opção de **Sites confiáveis** , clique no botão **sites** e adicione as URLs na caixa de diálogo é aberta. Você precisará adicionar a URL do seu gateway, bem como https://login.microsoftonline.com e https://login.live.com.
4. Vá para a guia **privacidade**
5. Na seção **Bloqueador de pop-up** , clique no botão **configurações** e adicione as URLs na caixa de diálogo é aberta. Você precisará adicionar a URL do seu gateway, bem como https://login.microsoftonline.com e https://login.live.com.


[[voltar ao início]](#toc)

<a id="azissue"></a>

## Tendo um problema com um recurso de Azure?

Envie um email em wacFeedbackAzure@microsoft.com com as seguintes informações:
* Informações gerais do problema a [perguntas listados abaixo](#filebug). 
* Descrever o problema e as etapas seguidas para reproduzir o problema. 
* Você anteriormente registrar seu gateway para Azure usando o script baixável New-AadApp.ps1 e, em seguida, atualizar para a versão 1807? Ou você se registrar seu gateway para Azure usando a interface do usuário de gateway gt _ configurações do Azure?
* É sua conta do Azure associada com vários diretórios/locatários?
    * Em caso afirmativo: ao registrar o aplicativo do Azure AD para o Windows Admin Center, foi o diretório que você usou seu diretório padrão no Azure? 
* Sua conta do Azure tem acesso a várias assinaturas?
* A assinatura que você estava usando tem anexada a cobrança?
* Você efetuou login a várias contas do Azure quando você encontrou o problema?
* Sua conta do Azure exige a autenticação multifator?
* É o computador que você está tentando gerenciar uma VM do Azure?
* Windows Admin Center está instalado em uma VM do Azure?

[[voltar ao início]](#toc)

<a id="filebug"></a>

## Ainda não funcionando ou seu problema não apresentado aqui? [perguntas comuns de solução de problemas]

Vá para o Visualizador de Eventos > Aplicativos e serviços > Microsoft-ServerManagementExperience e procure por erros ou avisos.

Registre um bug no nosso [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) que descreve seu problema.

Inclua qualquer erro ou aviso encontrado no log de eventos, assim como as seguintes informações: 

* Plataforma onde o Windows Admin Center está **instalado** (Windows 10 ou Windows Server):
    * Se instalado no servidor, o que é a [versão](#winvercompat) do Windows da **máquina executando o navegador** para acessar o Windows Admin Center: 
    * Você está usando o certificado autoassinado criado pelo instalador?
    * Se estiver usando seu próprio certificado, o nome do assunto corresponde ao computador?
    * Se estiver usando seu próprio certificado, ele especifica um nome de entidade alternativo?
* Você instalou com a configuração de porta padrão?
    * Caso contrário, qual porta você especificou?
* A máquina onde o Windows Admin Center está **instalado** está unida a um domínio?
* Windows [versão](#winvercompat) onde o Windows Admin Center está **instalado**:
* O computador que você está **tentando gerenciar** está associado a um domínio?
* Windows [versão](#winvercompat) do computador que você está **tentando gerenciar**:
* Qual navegador você está usando?
    * Se estiver usando o Google Chrome, qual é a versão? (Ajuda > Sobre o Google Chrome)

[[voltar ao início]](#toc)