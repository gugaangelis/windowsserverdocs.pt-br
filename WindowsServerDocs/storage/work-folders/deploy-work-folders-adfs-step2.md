---
title: "Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web - Etapa 2, Trabalho de pós-configuração do AD FS"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 2a07f31e3040f63edfb8c73d454b6301aad46583
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 2, Trabalho de pós-configuração do AD FS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a segunda etapa da implantação das Pastas de Trabalho com o AD FS (Serviços de Federação do Active Directory) e o Proxy de aplicativo Web. Você encontrará as outras etapas desse processo nestes tópicos:  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: visão geral](deploy-work-folders-adfs-overview.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 1, Configurar o AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 3, Configurar Pastas de Trabalho](deploy-work-folders-adfs-step3.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 4, Configurar o Proxy de aplicativo Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 5, Configurar clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   As instruções abordadas nesta seção destinam-se a um ambiente do Server 2016. Se você estiver usando o Windows Server 2012 R2, siga as [instruções do Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Na etapa 1, você instalou e configurou o AD FS. Agora, você precisa executar as seguintes etapas de pós-configuração do AD FS.  
  
## <a name="configure-dns-entries"></a>Configurar entradas DNS  
Você deve criar duas entradas DNS para o AD FS. Essas são as mesmas duas entradas que foram usadas nas etapas de pré-instalação quando você criou o certificado do SAN (nome alternativo da entidade).  
  
As entradas DNS estão no formulário:  
  
-   Nome do serviço AD FS.domínio  
  
-   enterpriseregistration.domínio  
  
-   Nome do serviço AD FS.domínio (a entrada DNS já deve existir. Por exemplo, 2016-ADFS.contoso.com)
  
No exemplo de teste, os valores são:  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>Criar os registros A e CNAME para o AD FS  
Para criar registros A e CNAME para o AD FS, siga estas etapas:  
  
1.  No controlador de domínio, abra o Gerenciador DNS.  
  
2.  Expanda a pasta Zonas de Pesquisa Direta, clique com botão direito do mouse no domínio e selecione **Novo Host (A)**.  
  
3.  A janela **Novo Host** é aberta. No campo **Nome**, insira o alias do nome do serviço AD FS. No exemplo de teste, o alias é **blueadfs**.  
  
    O alias deve ser igual à entidade no certificado usado para o AD FS. Por exemplo, se a entidade for adfs.contoso.com, o alias inserido aqui será **adfs**.  
  
    > [!IMPORTANT]  
    > Ao configurar o AD FS usando a interface do usuário do Windows Server, em vez do Windows PowerShell, você deve criar um registro A, em vez de um registro CNAME, para o AD FS. Isso deve ser feito porque a SPN (nome da entidade de serviço) criada por meio da interface do usuário contém apenas o alias usado para configurar o serviço AD FS como host.  
    >   
4.  Em **Endereço IP**, insira o endereço IP do servidor AD FS. No exemplo de teste, o endereço IP é **192.168.0.160**. Clique em **Adicionar Host**.  
  
5.  Na pasta Zonas de Pesquisa Direta, clique com botão direito do mouse no domínio novamente e selecione **Novo Alias (CNAME)**.  
  
6.  Na janela **Novo Registro de Recursos**, adicione o nome de alias **enterpriseregistration** e insira o FQDN do servidor AD FS. Esse alias é usado para Adição de Dispositivo e deve ser chamado **enterpriseregistration**.
  
7.  Clique em **OK**.  
  
Para realizar as etapas equivalente por meio do Windows PowerShell, use o comando a seguir. O comando deve ser executado no controlador de domínio.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com   
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>Configurar o objeto de confiança de terceira parte confiável do AD FS para Pastas de Trabalho  
Você pode instalar e configurar o objeto de confiança de terceira parte confiável, mesmo que Pastas de Trabalho ainda não tenham sido configuradas. O objeto de confiança de terceira parte confiável deve ser configurado para habilitar Pastas de Trabalho a usar o AD FS. Como você está no processo de configuração do AD FS, é recomendável realizar esta etapa agora.  
  
Para configurar o objeto de confiança de terceira parte confiável:  
  
1.  Abra **Gerenciador do Servidor**, no menu **Ferramentas**, selecione **Gerenciamento do AD FS**.  
  
2.  No painel direito, em **Ações**, clique em **Adicionar Confiança da Terceira Parte Confiável**.  
  
3.  Na página **Bem-vindo**, selecione **Com reconhecimento de declaração** e clique em **Iniciar**.  
  
4.  Na página **Selecionar Fonte de Dados**, selecione **Inserir dados sobre a terceira parte confiável manualmente** e clique em **Avançar**.  
  
5.  No campo **Nome para exibição**, insira **WorkFolders** e clique em **Avançar**.  
  
6.  Na página **Configurar Certificado**, clique em **Avançar**. Os certificados de criptografia de tokens são opcionais e não são necessários à configuração de teste.  
  
7.  Na página **Configurar URL**, clique em **Avançar**.  
  
8. Na página **Configurar Identificadores**, adicione o seguinte identificador: **https://windows-server-work-folders/V1**. Esse identificador é um valor embutido em código usado pelas Pastas de Trabalho e enviado pelo serviço Pastas de Trabalho quando ele está se comunicando com o AD FS. Clique em **Avançar**.  
  
9. Na página Escolher Política de Controle de Acesso, selecione **Permitir Todos** e clique em **Avançar**.  
  
10. Na página **Pronto para Adicionar Confiança**, clique em **Avançar**.  
  
11. Depois que a configuração for concluída, a última página do assistente informará que a configuração foi bem-sucedida. Marque a caixa de seleção para editar as regras de declarações e clique em **Fechar**.  
  
12. No snap-in do AD FS, selecione o objeto de terceira parte confiável **WorkFolders** e clique em **Editar Política de Emissão de Declaração** em Ações.

13. A janela **Editar Política de Emissão de Declaração para WorkFolders** é aberta. Clique em **Adicionar Regra**.  
  
14. Na lista suspensa **Modelo de regra de declaração**, selecione **Enviar Atributos LDAP como Declarações** e clique em **Avançar**.  
  
15. Na página **Configurar Regra de Declaração**, no campo **Nome da regra de declaração**, insira **WorkFolders**.  
  
16. Na lista suspensa **Repositório de atributos**, selecione **Active Directory**.  
  
17. Na tabela de mapeamento, insira estes valores:  
  
    -   Nome Principal do Usuário: UPN  
  
    -   Nome para Exibição: Nome  
  
    -   Sobrenome: Sobrenome  
  
    -   Nome Fornecido: Nome Fornecido  
  
18. Clique em **Concluir**. Você verá a regra WorkFolders listada na guia Regras de Transformação de Emissão e clicará em **OK**.  
  
### <a name="set-relying-part-trust-options"></a>Definir opções de objeto de confiança de terceira parte confiável  
Depois que o objeto de confiança de terceira parte confiável tiver sido configurado para o AD FS, você deverá concluir a configuração executando cinco comandos no Windows PowerShell. Esses comandos definem as opções necessárias para que as Pastas de Trabalho se comuniquem com êxito com o AD FS; elas não podem ser definidas por meio da interface do usuário. Estas são as opções:  
  
-   Habilitar o uso dos JWTs (tokens web JSON)  
  
-   Desabilitar declarações criptografadas  
  
-   Habilitar atualização automática  
  
-   Defina a emissão de tokens de atualização do Oauth para Todos os Dispositivos.  

-   Conceder aos clientes acesso ao objeto de confiança de terceira parte confiável

Para definir essas opções, use os seguintes comandos:  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://Windows-Server-Work-Folders/V1" -AllowAllRegisteredClients  
```  
  
## <a name="enable-workplace-join"></a>Habilitar Workplace Join  
A habilitação do Workplace Join é opcional, mas pode ser útil quando você quiser que os usuários usem dispositivos pessoais para acessar os recursos do local de trabalho.  
  
Para habilitar o registro de dispositivo do Workplace Join, execute os seguintes comandos do Windows PowerShell, que configurará o registro do dispositivo e definirá a política de autenticação global:  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>Habilitar certificado do AD FS  
Em seguida, exporte o certificado autoassinado do AD FS para que ele possa ser instalado nos seguintes computadores no ambiente de teste:  
  
-   O servidor usado para Pastas de Trabalho  
  
-   O servidor usado para Proxy de aplicativo Web  
  
-   O cliente Windows ingressado no domínio  
  
-   O cliente Windows não ingressado no domínio  
  
Para exportar o certificado, siga estas etapas:  
  
1.  Clique em **Iniciar** e em **Executar**.  
  
2.  Digite **MMC**.  
  
3.  No menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
4.  Na lista **Snap-ins disponíveis**, selecione **Certificados** e clique em **Adicionar**. O Assistente de Snap-in de Certificados é iniciado.  
  
5.  Selecione **Conta de computador** e clique em **Avançar**.  
  
6.  Selecione **Computador local: (o computador no qual este console está sendo executado)** e clique em **Concluir**.  
  
7.  Clique em **OK**.  
  
8.  Expanda a pasta **Console Root\Certificates\(Local Computer)\Personal\Certificates**.  
  
9.  Clique com o botão direito do mouse em **Certificado AD FS**, clique em **Todas as Tarefas** e clique em **Exportar...**.  
  
10. O Assistente para Exportação de Certificados é aberto. Selecione **Sim, exportar a chave privada**.  
  
11. Na página **Formato do Arquivo de Exportação**, deixe as opções padrão selecionadas e clique em **Avançar**.  
  
12. Crie uma senha para o certificado. Esta é a senha que você usará mais tarde quando importar o certificado para outros dispositivos. Clique em **Avançar**.  
  
13. Insira um local e o nome para o certificado e clique em **Concluir**.  
  
A instalação do certificado será abordada posteriormente no procedimento de implantação.  
  
## <a name="manage-the-private-key-setting"></a>Gerenciar configuração de chave privada  
Você deve dar permissão à conta de serviço do AD FS para acessar a chave privada do novo certificado. Você precisará conceder essa permissão novamente ao substituir o certificado de comunicação depois que ele expirar. Para conceder permissão, siga estas etapas:  
  
1.  Clique em **Iniciar** e em **Executar**.  
  
2.  Digite **MMC**.  
  
3.  No menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
4.  Na lista **Snap-ins disponíveis**, selecione **Certificados** e clique em **Adicionar**. O Assistente de Snap-in de Certificados é iniciado.  
  
5.  Selecione **Conta de computador** e clique em **Avançar**.  
  
6.  Selecione **Computador local: (o computador no qual este console está sendo executado)** e clique em **Concluir**.  
  
7.  Clique em **OK**.  
  
8.  Expanda a pasta **Console Root\Certificates\(Local Computer)\Personal\Certificates**.  
  
9.  Clique com o botão direito do mouse em **Certificado AD FS**, clique em **Todas as Tarefas** e clique em **Gerenciar Chaves Particulares**.  
  
10. Na janela **Permissões**, clique em **Adicionar**.  
  
11. Na janela **Tipos de Objeto**, selecione **Contas de Serviço** e clique em **OK**.  
  
12. Digite o nome da conta que está executando o AD FS. No exemplo de teste, essa conta é ADFSService. Clique em **OK**.  
  
13. Na janela **Permissões**, atribua pelo menos permissões de leitura à conta e clique em **OK**.  
  
Se você não tiver a opção de gerenciar chaves privadas, talvez seja necessário executar o comando a seguir: `certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>Verificar se AD FS está operacional  
Para verificar se o AD FS está funcionando, abra uma janela de navegador e acesse https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml 
  
A janela do navegador exibirá os metadados do servidor de federação sem formatação. Se você conseguir ver os dados sem erros ou avisos de SSL, o servidor de federação está operacional.  
  
Próxima etapa: [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 3, Configurar Pastas de Trabalho](deploy-work-folders-adfs-step3.md)  
  
## <a name="see-also"></a>Consulte também  
[Visão geral de Pastas de Trabalho](Work-Folders-Overview.md)  
  

