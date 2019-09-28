---
title: Migrar um servidor de federação AD FS autônomo ou um farm de AD FS de nó único
description: Fornece informações sobre como migrar um servidor autônomo ou de nó único AD FS 2,0 para o Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b8029d67a9f21e5189322692b8f1316306542c96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359377"
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Migrar um servidor de federação AD FS autônomo ou um farm de AD FS de nó único  
Este documento fornece informações detalhadas sobre como migrar um servidor autônomo AD FS 2,0 para o Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Migrar um servidor AD FS autônomo 2,0

Use o procedimento a seguir para migrar o servidor AD FS 2,0 para o Windows Server 2012.
  
1.  Examine e execute os procedimentos em [preparar para migrar um servidor de federação AD FS autônomo ou um farm de AD FS de nó único](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Execute uma atualização in-loco do sistema operacional no servidor do Windows Server 2008 R2 ou do Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor do Windows Server 2012 AD FS está instalada, mas não está configurada. Você deverá criar manualmente a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.  
  
3. Criar a configuração original do AD FS. Você pode criar a configuração original do AD FS usando um dos métodos a seguir:  
  
-   Use o **Assistente de configuração de servidor de federação do AD FS** para criar um novo servidor de federação. Para saber mais informações, confira [Criar o primeiro servidor de federação em um farm de servidores de federação](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Ao avançar no assistente, use as informações coletadas durante a preparação da migração para migrar o servidor de federação do AD FS como mostrado a seguir:  
  
 |**Opção de entrada do assistente de configuração do servidor de Federação**|**Use o seguinte valor**| 
|-----|-----| 
|**Certificado SSL** na página **Especifique o nome do serviço de federação**|Escolha o certificado SSL com o nome de assunto e miniatura definidos por você ao preparar a migração do servidor de federação do AD FS.|  
|**Conta de serviço** e **Senha** na página **Especifique a conta de serviço**|Insira as informações da conta de serviço que você registrou ao se preparar para a migração do AD FS servidor de Federação. **Observação:**  Se você tiver escolhido o servidor de federação autônomo na segunda página do assistente, SERVIÇO DE REDE será usado automaticamente como conta de serviço.|  
  
> [!IMPORTANT] 
> Você pode usar este método somente se usar o WID (Banco de Dados Interno do Windows) para armazenar o banco de dados de configuração do AD FS para o servidor de federação autônomo ou farm de AD FS de nó único.  
>
>  Se você usar o SQL Server para armazenar o banco de dados de configuração do AD FS para o farm de AD FS de nó único, deverá usar o Windows PowerShell para criar a configuração original do AD FS no servidor de federação.  
  
-   Use o Windows PowerShell  
  
> [!IMPORTANT]
>  Você deve usar o Windows PowerShell se usar o SQL Server para armazenar o banco de dados de configuração do AD FS para o servidor de federação autônomo ou farm de AD FS de nó único.  
  
Veja a seguir um exemplo de como usar o Windows PowerShell para criar a configuração original do AD FS em um servidor de federação no farm de nó único do SQL Server.  Abra o módulo do Windows PowerShell e execute o seguinte comando: `$fscredential = Get-Credential`. Digite o nome e a senha da conta de serviço definida ao preparar a migração do farm do SQL Server. Em seguida, execute o seguinte comando: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` onde `Data Source` é o valor da fonte de dados no valor da cadeia de conexão do repositório de políticas no seguinte arquivo: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4. Restaurar as configurações de serviço e relações de confiança restantes do AD FS. Esta é uma etapa manual na qual você poderá usar os arquivos exportados e os valores coletados durante a preparação da migração do AD FS. Para ver instruções detalhadas, confira Restaurar a configuração restante do farm do AD FS.  
  
> [!NOTE]
>  Esta etapa somente será pedida se você estiver migrando um servidor de federação autônomo ou um farm de WID de nó único.  Se o servidor de federação usar um banco de dados do SQL Server como armazenamento da configuração, as configurações do serviço e relações de confiança serão preservadas no banco de dados.  
  
5. Atualizar suas páginas da web do AD FS. Esta é uma etapa manual. Se você tiver feito backup de suas páginas da Web AD FS personalizadas ao se preparar para a migração, use seus dados de backup para substituir as páginas da Web do AD FS padrão que foram criadas por padrão no diretório **%systemdrive%\inetpub\adfs\ls** como resultado da AD FS configuração no Windows Server 2012.  
  
6. Restaure as personalizações restantes do AD FS, tais como armazenamento de atributos personalizados.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Restaurando as configurações restantes do farm do AD FS  
  
-   Restaure as configurações a seguir do serviço do AD FS para um farm de WID com nó único ou serviço de federação autônomo da seguinte maneira:  
  
    -   No console de gerenciamento do AD FS, escolha **Serviço** e clique em **Editar serviço de federação…** . Verifique as configurações do serviço de federação comparando cada um com os valores que você exportou para o arquivo properties.txt durante a preparação para a migração:  
  
    
|**Serviço de Federação nome da propriedade conforme relatado por Get-ADFSproperties**|**Serviço de Federação nome da propriedade no console de gerenciamento AD FS**|  
|-----|-----|
|DisplayName|Nome de exibição do Serviço de Federação|  
|HostName|Nome do Serviço de Federação|  
|Identificador|Identificador do Serviço de Federação|  
  
-   No console de gerenciamento do AD FS, selecione **Certificados**. Verifique os certificados de descriptografia de token, autenticação de token e comunicações de serviço, comparando cada um com os valores que você exportou para o arquivo certificates.txt durante a preparação da migração.  
  
Para alterar os certificados de descriptografia de token e de autenticação de token dos certificados autoassinados padrão para certificados externos, é preciso primeiro desabilitar o recurso de sobreposição automática de certificados, que fica habilitado por padrão.  Para fazer isso, é possível usar o seguinte comando do Windows PowerShell: `PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   No console de gerenciamento do AD FS, selecione **Pontos de extremidade**. Compare os pontos de extremidade do AD FS habilitados com a lista de pontos de extremidade do AD FS habilitados que você exportou para um arquivo durante a preparação para a migração do AD FS.  
  
-   No console de gerenciamento do AD FS, selecione **Descrições de declarações**. Compare a lista de descrições de declarações do AD FS com a lista de descrições de declarações que você exportou para um arquivo durante a preparação para a migração do AD FS. Adicione todas as descrições de declarações personalizadas de seu arquivo que não tenham sido incluídas na lista padrão do AD FS.  Observe que o identificador de declarações no console de gerenciamento mapeia para ClaimType no arquivo.  Para saber mais sobre como adicionar descrições de declarações, confira [Adicionar uma descrição de declaração](../operations/add-a-claim-description.md). Para saber mais, confira a seção "Etapa 1 - Exportar configurações do serviço" em Preparar para migrar o servidor de federação do AD FS 2.0.  
  
-   No console de gerenciamento do AD FS, escolha **Relações de confiança do provedor de declarações**. Você deverá criar novamente cada relação de provedor de declaração manualmente usando o **Assistente de adição de relação de confiança de provedor de declarações**.  Use a lista de relações de confiança de provedor de declarações exportada e registrada durante a preparação para a migração do AD FS. Você pode ignorar a relação de confiança do provedor de declaração com o identificador "AD AUTHORITY" no arquivo, pois esta é a relação de confiança de provedor de declarações do "Active Directory", que faz parte da configuração padrão do AD FS.  Contudo, verifique se há alguma outra regra de declaração personalizada que pode ter sido adicionada às relações de confiança do Active Directory antes da migração. Para saber mais sobre como criar relações de confiança de provedor de declarações, confira [Criar uma relação de confiança do provedor de declarações usando metadados de federação](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) ou [Criar manualmente uma relação de confiança do provedor de declarações](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   No console de gerenciamento do AD FS, **escolha Objetos de confiança de terceira parte confiável**. Você deverá criar novamente a terceira parte confiável manualmente usando o **Assistente de adição de relação de confiança de terceira parte confiável**. Use a lista de terceira parte confiável exportada e registrada durante a preparação para a migração do AD FS. Para saber mais sobre a criação de objetos de confiança de terceira parte confiável, confira [Criar um objeto de confiança de terceira parte confiável usando metadados de federação](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) ou [Criar manualmente um objeto de confiança de terceira parte confiável](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o proxy do servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de federação AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar o proxy do servidor de federação AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)




-   
-    