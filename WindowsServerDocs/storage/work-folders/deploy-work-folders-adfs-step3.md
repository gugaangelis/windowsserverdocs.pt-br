---
title: Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web - Etapa 3, Configurar Pastas de Trabalho
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: 433d981d558cc49cd860a5c05a8d76996d9cdd30
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965738"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 3, Configurar Pastas de Trabalho

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve a terceira etapa da implantação das Pastas de Trabalho com o AD FS (Serviços de Federação do Active Directory) e o Proxy de aplicativo Web. Você encontrará as outras etapas desse processo nestes tópicos:  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: visão geral](deploy-work-folders-adfs-overview.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 1, Configurar o AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 2, Trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 4, Configurar o Proxy de aplicativo Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 5, Configurar clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   As instruções abordadas nesta seção são para um ambiente do Windows Server 2019 ou do Windows Server 2016. Se você estiver usando o Windows Server 2012 R2, siga as [instruções do Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn747208(v=ws.11)).

Para configurar Pastas de Trabalho, use os procedimentos a seguir.  
  
## <a name="pre-installment-work"></a>Trabalho de pré-instalação  
Para instalar Pastas de trabalho, você precisará de um servidor que tenha ingressado no domínio e esteja executando o Windows Server 2016. O servidor deve ter uma configuração de rede válida.  
  
Como exemplo de teste, ingresse o computador que executará Pastas de Trabalho no domínio Contoso e configure a interface da rede, conforme descrito nas seções a seguir. 

### <a name="set-the-server-ip-address"></a>Defina o endereço IP do servidor  
Altere o endereço IP do servidor para um endereço IP estático. Como exemplo de teste, use a classe IP A, que é 192.168.0.170 / máscara de sub-rede: 255.255.0.0 / Gateway Padrão: 192.168.0.1 / DNS Preferencial: 192.168.0.150 (o endereço IP do controlador de domínio). 
  
### <a name="create-the-cname-record-for-work-folders"></a>Criar o registro CNAME para Pastas de Trabalho  
Para criar o registro CNAME para Pastas de Trabalho, siga estas etapas:  
  
1.  No controlador de domínio, abra o **Gerenciador DNS**.  
  
2.  Expanda a pasta Zonas de Pesquisa Direta, clique com botão direito do mouse no domínio e clique em **Novo Alias (CNAME)**.  
  
3.  Na janela **Novo Registro de Recursos**, no campo **Nome do alias**, insira o alias de Pastas de Trabalho. No exemplo de teste, o alias é **workfolders**.  
  
4.  No campo **Nome de domínio totalmente qualificado**, o valor deve ser **workfolders.contoso.com**.  
  
5.  No campo **Fully qualified domain name for target host**, insira o FQDN do servidor de Pastas de Trabalho. No exemplo de teste, esse FQDN é **2016-WF.contoso.com**.  
  
6.  Clique em **OK**.  
  
Para realizar as etapas equivalente por meio do Windows PowerShell, use o comando a seguir. O comando deve ser executado no controlador de domínio.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com   
```  
  
### <a name="install-the-ad-fs-certificate"></a>Instalar o certificado do AD FS  
Instale o certificado do AD FS criado durante a configuração do AD FS no repositório de certificados do computador local, executando estas etapas:  
  
1.  Clique em **Iniciar**e em **Executar**.  
  
2.  Digite **MMC**.  
  
3.  No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
4.  Na lista **Snap-ins disponíveis**, selecione **Certificados** e clique em **Adicionar**. O Assistente de Snap-in de Certificados é iniciado.  
  
5.  Selecione **Conta de computador** e clique em **Avançar**.  
  
6.  Selecione **Computador local: (o computador no qual este console está sendo executado)** e clique em **Concluir**.  
  
7.  Clique em **OK**.  
  
8.  Expanda o **console de pasta Root\Certificates \( computador local) \Personal\Certificates**.  
  
9. Clique com o botão direito do mouse em **Certificados**, clique em **Todas as Tarefas** e clique em **Importar**.  
  
10. Navegue até a pasta que contém o certificado do AD FS, siga as instruções no assistente para importar o arquivo e coloque-o no repositório de certificados.

11. Expanda o **console de pasta Root\Certificates \( local Computer) \Trusted Root Certification Authorities\Certificates**.  
  
12. Clique com o botão direito do mouse em **Certificados**, clique em **Todas as Tarefas** e clique em **Importar**.  
  
13. Navegue até a pasta que contém o certificado do AD FS, siga as instruções no assistente para importar o arquivo e coloque-o no repositório Autoridades de Certificação Confiáveis.  
  
### <a name="create-the-work-folders-self-signed-certificate"></a>Criar o certificado autoassinado de Pastas de Trabalho  
Para criar o certificado auto-assinado de Pastas de Trabalho, siga estas etapas:  
  
1.  Baixe os scripts fornecidos na postagem de blog [Implantando Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap/ba-p/425318) e copie o arquivo makecert.ps1 para o computador de Pastas de Trabalho.  
  
2.  Abra um janela do Windows PowerShell com privilégios de administrador.  
  
3.  Defina a política de execução para irrestrita:  
  
    ```powershell  
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted   
    ```  
  
4.  Vá para o diretório no qual copiou o script.  
  
5.  Execute o script makeCert:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  Quando você for solicitado a alterar o certificado da entidade, insira o novo valor da entidade. Neste exemplo, o valor é **workfolders.contoso.com**.  
  
7.  Quando você for solicitado a inserir nomes SAN (nome alternativo da entidade), pressione S e insira os nomes SAN, um de cada vez.  
  
    Neste exemplo, digite **workfolders.contoso.com** e pressione Enter. Em seguida, digite **2016-WF.contoso.com** e pressione Enter.  
  
    Quando todos os nomes SAN forem inseridos, pressione Enter em uma linha vazia.  
  
8.  Quando você for solicitado a instalar os certificados no repositório Autoridade de Certificação Raiz Confiável, pressione S.  
  
O certificado de Pastas de Trabalho deve ser um certificado SAN com os seguintes valores:  
  
-   **workfolders**.**domínio**  
  
-   **nome do computador**.**domínio**  
  
No exemplo de teste, os valores são:  
  
-   **workfolders.contoso.com**  
  
-   **2016-WF.contoso.com**  
  
## <a name="install-work-folders"></a>Instalar Pastas de Trabalho  
Para instalar a função de Pastas de Trabalho, siga estas etapas:  
  
1.  Abra **Gerenciador do Servidor**, clique em **Adicionar funções e recursos** e clique em **Avançar**.  
  
2.  Na página **Tipo de Instalação**, selecione **Instalação baseada em função ou recurso** e clique em **Avançar**.  
  
3.  Na página **Seleção de Servidor**, selecione o servidor atual e clique em **Avançar**.  
  
4.  Na página **Funções de Servidor**, expanda **Serviços de Arquivo e Armazenamento**, expanda **Serviços de Arquivo e iSCSI** e selecione **Pastas de Trabalho**.  
  
5.  Na página **Assistente Adicionar Função e Recurso** , clique em **Adicionar Recursos** e em **Avançar**.  
  
6.  Na página **recursos** , clique em **Avançar**.  
  
7.  Na página **Confirmação**, clique em **Instalar**.  
  
## <a name="configure-work-folders"></a>Configurar pastas de trabalho  
Para configurar Pastas de Trabalho, siga estas etapas:  
  
1.  Abra o **Server Manager**.  
  
2.  Selecione **Serviços de Arquivo e Armazenamento** e, em seguida, selecione **Pastas de Trabalho**.  
  
3.  Na página **Pastas de Trabalho**, inicie o **Assistente de Novo Compartilhamento de Sincronização** e clique em **Avançar**.  
  
4.  Na página **Servidor e Caminho**, selecione o servidor em que o compartilhamento de sincronização será criado, insira um caminho local no qual os dados de Pastas de Trabalho serão armazenados e clique em **Avançar**.  
  
    Se o caminho não existir, você será solicitado a criá-lo. Clique em **OK**.  
  
5.  Na página **Usar Estrutura de Pasta**, selecione **Alias de usuário** e clique em **Avançar**.  
  
6.  Na página **Nome de Compartilhamento de Sincronização**, insira o nome para o compartilhamento de sincronização. No exemplo de teste, esse nome é **WorkFolders**. Clique em **Próximo**.  
  
7.  Na página **Acesso à Sincronização**, adicione os usuários ou os grupos que terão acesso ao novo compartilhamento de sincronização. No exemplo de teste, conceda acesso a todos os usuários do domínio. Clique em **Próximo**.  
  
8.  Na página **Políticas de Segurança do Computador**, selecione **Criptografar pastas de trabalho** e **Automatically lock page and require a password**. Clique em **Próximo**.  
  
9. Na página **Confirmação**, clique em **Criar** para concluir o processo de configuração.  
  
## <a name="work-folders-post-configuration-work"></a>Trabalho de pós-configuração de Pastas de Trabalho  
Para concluir a configuração de Pastas de Trabalho, conclua estas etapas adicionais:  
  
-   Associar o certificado de Pastas de Trabalho à porta SSL  
  
-   Configurar Pastas de Trabalho para usar a autenticação do AD FS  
  
-   Exportar o certificado de Pastas de Trabalho (se você estiver usando um certificado autoassinado)  
  
### <a name="bind-the-certificate"></a>Associar o certificado  
Pastas de Trabalho se comunica apenas via SSL e deve ter o certificado autoassinado criado anteriormente (ou emitido pela autoridade de certificação) associado à porta.  
  
Existem dois métodos que você pode usar para associar o certificado à porta via Windows PowerShell: netsh e cmdlets do IIS.  
  
#### <a name="bind-the-certificate-by-using-netsh"></a>Associar o certificado usando o netsh  
Para usar o utilitário de script de linha de comando netsh no Windows PowerShell, você deve redirecionar o comando para netsh. O script de exemplo a seguir localiza o certificado com a entidade **workfolders.contoso.com** e a associa à porta 443 usando o netsh:  
  
```powershell  
$subject = "workfolders.contoso.com"   
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
$Command = "http add sslcert ipport=0.0.0.0:443 certhash=$thumbprint appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY"  
$Command | netsh  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>Associar o certificado usando os cmdlets do IIS  
Você também pode associar o certificado à porta usando os cmdlets de gerenciamento do IIS, que estarão disponíveis se você tiver instalado os scripts e as ferramentas de gerenciamento do IIS.  
  
> [!NOTE]  
> A instalação das ferramentas de gerenciamento do IIS não habilita a versão completa do IIS (Serviços de Informações da Internet) no computador de Pastas de Trabalho; ela habilita somente os cmdlets de gerenciamento. Essa configuração pode proporcionar algumas vantagens. Por exemplo, se você estiver procurando cmdlets que forneçam a funcionalidade obtida com o netsh. Quando o certificado é associado à porta por meio do cmdlet New-WebBinding, a associação não depende do IIS. Após a associação, você pode até mesmo remover o recurso Web-Mgmt-Console, e o certificado continuará associado à porta. Você pode verificar a associação via netsh digitando **netsh http show sslcert**.  
  
O exemplo a seguir usa o cmdlet New-WebBinding para localizar o certificado com a entidade **workfolders.contoso.com** e associá-lo à porta 443:  
  
```powershell  
$subject = "workfolders.contoso.com"  
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert =Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject } | sort $_.NotAfter -Descending | select -first 1   
$thumbprint = $cert.Thumbprint  
New-WebBinding -Name "Default Web Site" -IP * -Port 443 -Protocol https  
#The default IIS website name must be used for the binding. Because Work Folders uses Hostable Web Core and its own configuration file, its website name, 'ECSsite', will not work with the cmdlet. The workaround is to use the default IIS website name, even though IIS is not enabled, because the NewWebBinding cmdlet looks for a site in the default IIS configuration file.   
Push-Location IIS:\SslBindings  
Get-Item cert:\LocalMachine\MY\$thumbprint | new-item *!443  
Pop-Location  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
### <a name="set-up-ad-fs-authentication"></a>Configurar a autenticação do AD FS  
Para configurar Pastas de Trabalho para usar a autenticação do AD FS, siga estas etapas:  
  
1.  Abra o **Server Manager**.  
  
2.  Clique em **Servidores**e selecione o servidor de Pastas de Trabalho na lista.  
  
3.  Clique com o botão direito do mouse no servidor e clique em **Configurações de Pastas de Trabalho**.  
  
4.  Na janela **Configurações de Pasta de Trabalho**, selecione **Serviços de Federação do Active Directory** e digite a URL do serviço de federação. Clique em **Aplicar**.  
  
    No exemplo de teste, a URL é **https://blueadfs.contoso.com** .  
  
O cmdlet para realizar a mesma tarefa por meio do Windows PowerShell é:  
  
```powershell  
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"   
```  
  
Se você estiver configurando o AD FS com certificados autoassinados, poderá receber uma mensagem de erro informando que a URL do serviço de federação está incorreta, inacessível ou um objeto de confiança de terceira parte confiável não foi configurado.  
  
Esse erro também poderá ocorrer se o certificado do AD FS não tiver sido instalado no servidor de Pastas de Trabalho ou se o CNAME do AD FS não tiver sido configurado corretamente. Você deve corrigir esses problemas antes de continuar.  
  
### <a name="export-the-work-folders-certificate"></a>Exportar o certificado de Pastas de Trabalho  
O certificado autoassinado de Pastas de Trabalho deve ser exportado para que você possa instalá-lo mais tarde nos seguintes computadores do ambiente de teste:  
  
-   O servidor usado para Proxy de aplicativo Web  
  
-   O cliente Windows ingressado no domínio  
  
-   O cliente Windows não ingressado no domínio  
  
Para exportar o certificado, siga as mesmas etapas usadas para exportar o certificado do AD FS anteriormente, conforme descrito em [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 2, Trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md), Exportar o certificado do AD FS.  
  
Próxima etapa: [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 4, Configurar o Proxy de aplicativo Web](deploy-work-folders-adfs-step4.md)  
  
## <a name="see-also"></a>Consulte Também  
[Visão geral de Pastas de Trabalho](Work-Folders-Overview.md)  
  
