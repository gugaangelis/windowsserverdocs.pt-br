---
title: Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web - Etapa 1, Configurar o AD FS
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 10/18/2018
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 4f4119e893b215bd9f6d713bc5a17218b751c3d3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812688"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 1, a configuração do AD FS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a primeira etapa da implantação das Pastas de Trabalho com o AD FS (Serviços de Federação do Active Directory) e o Proxy de aplicativo Web. Você encontrará as outras etapas desse processo nestes tópicos:  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Visão geral](deploy-work-folders-adfs-overview.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 2, o trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 3, configurar as pastas de trabalho](deploy-work-folders-adfs-step3.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 4, configurar o Proxy de aplicativo Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 5, configurar os clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   As instruções apresentadas nesta seção são para um ambiente de 2019 do Windows Server ou Windows Server 2016. Se você estiver usando o Windows Server 2012 R2, siga as [instruções do Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Para configurar o AD FS para uso com Pastas de Trabalho, execute os procedimentos a seguir.  
  
## <a name="pre-installment-work"></a>Trabalho de pré-instalação  
Se você pretende converter o ambiente de teste que está configurando com essas instruções em ambiente de produção, há dois procedimentos recomendáveis antes de começar:  
  
-   Configure uma conta de administrador do domínio do Active Directory a ser usada na execução do serviço do AD FS.
  
-   Obtenha um certificado SAN (nome alternativo da entidade) SSL para autenticação do servidor. No exemplo de teste, você usará um certificado autoassinado, mas para produção; você deve usar um certificado confiável publicamente.  
  
A obtenção desses itens pode levar algum tempo, dependendo das políticas da empresa; por isso, é recomendável iniciar o processo de solicitação dos itens antes de começar a criar o ambiente de teste.  
  
Há várias ACs (autoridades de certificação) nas quais você pode adquirir o certificado. Você pode encontrar uma lista das ACs consideradas confiáveis pela Microsoft no [artigo da base de dados 931125](https://support.microsoft.com/kb/931125). Outra alternativa é obter um certificado da AC corporativa da empresa.  
  
No ambiente de teste, você usará um certificado autoassinado criado por um dos scripts fornecidos.  
  
> [!NOTE]  
> O AD FS não oferece suporte a certificados de criptografia CNG (Cryptography Next Generation), que significa que você não pode criar o certificado autoassinado usando o cmdlet New-SelfSignedCertificate do Windows PowerShell. No entanto, você pode usar o script makecert.ps1 incluído na postagem de blog [Implantando Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap). Esse script cria um certificado autoassinado que funciona com o AD FS e solicita os nomes SAN necessários para criar o certificado.  
  
Em seguida, realize o trabalho adicional de pré-instalação descrito nas seções a seguir.  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>Criar um certificado autoassinado do AD FS  
Para criar um certificado autoassinado do AD FS, siga estas etapas:  
  
1.  Baixe os scripts fornecidos na postagem de blog [Implantando Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap) e copie o arquivo makecert.ps1 para o computador do AD FS.  
  
2.  Abra um janela do Windows PowerShell com privilégios de administrador.  
  
3.  Defina a política de execução para irrestrita:  
  
    ```powershell  
    Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  Vá para o diretório no qual copiou o script.  
  
5.  Execute o script makecert:  
  
    ```powershell  
    .\makecert.ps1  
    ```  
  
6.  Quando você for solicitado a alterar o certificado da entidade, insira o novo valor da entidade. Neste exemplo, o valor é **blueadfs.contoso.com**.  
  
7.  Quando você for solicitado a inserir nomes SAN, pressione S e insira os nomes SAN, um de cada vez.  
  
    Neste exemplo, digite **blueadfs.contoso.com** e pressione Enter. Em seguida, digite **2016-adfs.contoso.com** e pressione Enter. Por fim, digite **enterpriseregistration.contoso.com** e pressione Enter.  
  
    Quando todos os nomes SAN forem inseridos, pressione Enter em uma linha vazia.  
  
8.  Quando você for solicitado a instalar os certificados no repositório Autoridade de Certificação Raiz Confiável, pressione S.  
  
O certificado do AD FS deve ser um certificado SAN com os seguintes valores:  

-   Nome do serviço AD FS.domínio

-   enterpriseregistration.domínio

-   Nome do servidor do AD FS.domínio

No exemplo de teste, os valores são:  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
O SAN enterpriseregistration é necessário para Workplace Join.  
  
### <a name="set-the-server-ip-address"></a>Defina o endereço IP do servidor  
Altere o endereço IP do servidor para um endereço IP estático. Por exemplo, teste, use o IP classe a, que é 192.168.0.160 / máscara de sub-rede: 255.255.0.0 / Gateway padrão: 192.168.0.1 / preferencial DNS: 192.168.0.150 (o endereço IP do seu controlador de domínio\).  
  
## <a name="install-the-ad-fs-role-service"></a>Instalar o serviço de função do AD FS  
Para instalar o AD FS, siga estas etapas:  
  
1.  Faça logon no computador físico ou virtual no qual você pretende instalar o AD FS, abra **Gerenciador do Servidor** e inicie o Assistente de Adição de Funções e Recursos.  
  
2.  Na página **Funções de Servidor**, selecione a função **Serviços de Federação do Active Directory** e clique em **Avançar**.  
  
3.  Na página **Serviços de Federação do Active Directory (AD FS)** , você verá uma mensagem informando que a função Proxy de aplicativo Web não pode ser instalada no mesmo computador do AD FS. Clique em **Avançar**.  
  
4.  Clique em **Instalar** na página de confirmação.  
  
Para realizar a instalação equivalente do AD FS por meio do Windows PowerShell, use estes comandos:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>Configurar o AD FS  
Em seguida, configure o AD FS usando o Gerenciador do Servidor ou o Windows PowerShell.  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>Configurar o AD FS usando o Gerenciador do Servidor  
Para configurar o AD FS usando o Gerenciador do Servidor, siga estas etapas:  
  
1.  Abra o Gerenciador do Servidor.  
  
2.  Clique no sinalizador **Notificações** na parte superior da janela Gerenciador do Servidor e clique em **Configure o serviço de federação neste servidor**.  
  
3.  O Assistente de Configuração dos Serviços de Federação do Active Directory é iniciado. Na página **Conectar ao AD DS**, insira a conta de administrador do domínio que você deseja usar como conta do AD FS e clique em **Avançar**.  
  
4.  Na página **Especificar Propriedades do Serviço**, insira o nome da entidade do certificado SSL a ser usado na comunicação do AD FS. No exemplo de teste, esse serviço é **blueadfs.contoso.com**.  
  
5.  Insira o nome do serviço de federação. No exemplo de teste, esse serviço é **blueadfs.contoso.com**. Clique em **Avançar**.  
  
    > [!NOTE]  
    > O nome do serviço de federação não deve usar o nome de um servidor existente no ambiente. Se você usar o nome de um servidor existente, a instalação do AD FS falhará e deverá ser reiniciada.  
  
6.  Na página **Especificar Conta de Serviço**, insira o nome que você deseja atribuir à conta de serviço gerenciado. No exemplo de teste, selecione **Criar uma Conta de Serviço Gerenciado de Grupo** ,e em **Nome da Conta**, insira **ADFSService**. Clique em **Avançar**.  
  
7.  Na página **Especificar Banco de Dados de Configuração**, selecione **Crie um banco de dados neste servidor que utiliza o Banco de Dados Interno do Windows** e clique em **Avançar**.  
  
8.  A página **Examinar Opções** mostra uma visão geral das opções que você selecionou. Clique em **Avançar**.  
  
9. A página **Verificações de Pré-requisitos** indica se todas as verificações de pré-requisitos passaram no teste. Se houver problemas, clique em **Configurar**.  
  
    > [!NOTE]  
    > Se você usou o nome do servidor do AD FS ou de qualquer outro computador existente para o nome do serviço de federação, será exibida uma mensagem de erro. Você deve iniciar a instalação novamente e escolher um nome que não seja o nome de um computador existente.  
  
10. Quando a configuração for concluída com êxito, a página **Resultados** confirma que o AD FS foi configurado com êxito.  
  
### <a name="configure-ad-fs-by-using-powershell"></a>Configurar o AD FS usando o PowerShell  
Para realizar a configuração equivalente do AD FS por meio do Windows PowerShell, use os comandos a seguir.  
  
Para instalar o AD FS:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
Para criar a conta de serviço gerenciada:  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
Após configurar o AD FS, você deve configurar um farm AD FS usando a conta de serviço gerenciado criada na etapa anterior e o certificado criado nas etapas a pré-configuração.  
  
Para configurar um farm do AD FS:  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
Próxima etapa: [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 2, o trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md)  
  
## <a name="see-also"></a>Consulte também  
[Visão geral de pastas de trabalho](Work-Folders-Overview.md)  
  

