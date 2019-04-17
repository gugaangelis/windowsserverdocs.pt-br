---
title: "Migrar um servidor de Federação do AD FS autônomo ou um farm de AD FS do nó único"
description: "Fornece informações sobre como migrar um autônomo ou um único nó AD FS 2.0 servidor Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Migrar um servidor de Federação do AD FS autônomo ou um farm de AD FS do nó único  
Este documento fornece informações detalhadas sobre como migrar um servidor do AD FS 2.0 autônomo para Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Migrar um anúncio autônomo servidor FS 2.0

Use o procedimento a seguir para migrar o AD FS 2.0 servidor Windows Server 2012.
  
1.  Revise e execute os procedimentos [preparar para migrar um servidor de Federação do AD FS autônomo ou um farm de AD FS do nó único](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Realizar uma atualização in-loco do sistema operacional em seu servidor do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS nesse servidor é perdida e a função de servidor 2.0 AD FS é removida. A função de servidor Windows Server 2012 AD FS é instalada em vez disso, mas ela não está configurada. Você deve criar a configuração original do AD FS e restaurar as configurações do AD FS restantes para concluir a migração do servidor de Federação manualmente.  
  
3.  Crie a configuração original do AD FS. Você pode criar a configuração original do AD FS usando qualquer um dos seguintes métodos:  
  
-   Use o **Assistente de configuração de servidor de Federação do AD FS** para criar um novo servidor de Federação. Para obter mais informações, consulte [criar o primeiro servidor de Federação em um Farm de servidores de Federação](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Conforme você avança no assistente, use as informações coletadas ao preparar para migrar seu servidor de Federação do AD FS da seguinte maneira:  
  
 |**Opção de entrada de Assistente de configuração do servidor de Federação**|**Use o seguinte valor**| 
|-----|-----| 
|**Certificado SSL** sobre o **especificar o nome do serviço de Federação** página|Selecione o certificado SSL cujo nome do requerente e a impressão digital gravados ao preparar para a migração do servidor de Federação do AD FS.|  
|**Conta de serviço** e **senha** sobre o **especificar uma conta de serviço** página|Insira as informações de conta de serviço que você gravou ao preparar para a migração do servidor de Federação do AD FS. **Observação:** se você selecionar o servidor de Federação autônomos na segunda página do assistente, serviço de rede é usado automaticamente como a conta de serviço.|  
  
> [!IMPORTANT] 
> Você pode utilizar esse método somente se você estiver usando o banco de dados interno do Windows (trabalho) para armazenar o banco de dados de configuração do AD FS para o servidor de Federação autônomo ou um farm de AD FS do nó único.  
>
>  Se você estiver usando o SQL Server para armazenar o banco de dados de configuração do AD FS do seu farm do AD FS nó único, você deve usar o Windows PowerShell para criar a configuração original do AD FS em seu servidor de Federação.  
  
-   Usar o Windows PowerShell  
  
> [!IMPORTANT]
>  Você deve usar o Windows PowerShell se você estiver usando o SQL Server para armazenar o banco de dados de configuração do AD FS para o servidor de Federação autônomo ou um farm de AD FS do nó único.  
  
A seguir está um exemplo de como usar o Windows PowerShell para criar a configuração original do AD FS em um servidor de Federação em um farm de servidores do SQL de nó único.  Abra o módulo Windows PowerShell e execute o seguinte comando:`$fscredential = Get-Credential`. Insira o nome e a senha da conta de serviço que você gravou ao preparar seu farm de servidores SQL para migração. Em seguida, execute o seguinte comando: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`onde `Data Source`é o valor de origem de dados no valor de cadeia de caracteres de conexão do política loja no arquivo a seguir:`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4.  Restaure as configurações de serviço do AD FS restantes e relações de confiança. Esta é uma etapa manual durante o qual você pode usar os arquivos que você exportou e os valores que você coletou ao preparar para a migração do AD FS. Para obter instruções detalhadas, consulte restaurar a configuração do restante AD FS Farm.  
  
> [!NOTE]
>  Essa etapa só é necessária se você estiver migrando um servidor de Federação autônomo ou um farm de trabalho único nó.  Se o servidor de federação usa um banco de dados do SQL Server como o repositório de configuração, as configurações de serviço e relações de confiança são preservadas no banco de dados.  
  
5.  Atualize suas páginas da Web do AD FS. Esta é uma etapa manual. Se você fez backup de suas páginas da Web do AD FS personalizadas ao preparar para a migração, usar seus dados de backup para substituir o padrão do AD FS páginas da Web que foram criados por padrão no **%systemdrive%\inetpub\adfs\ls** diretório como resultado da configuração do AD FS no Windows Server 2012.  
  
6.  Restaure as personalizações do AD FS restantes, como o atributo personalizado lojas.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Restaurando a AD FS Farm configuração restante  
  
-   Restaure as seguintes configurações de serviço do AD FS para um farm de trabalho único nó ou o serviço de Federação independente da seguinte maneira:  
  
    -   No console de gerenciamento do AD FS, selecione **serviço** e clique em **editar o serviço de Federação...**. Verifique se as configurações de serviço de Federação verificando cada um dos valores contra os valores exportadas para o arquivo properties.txt ao preparar para a migração:  
  
    
|**Nome da propriedade de serviço de Federação conforme relatado pelo Get-ADFSProperties**|**Nome da propriedade de serviço de federação no console de gerenciamento do AD FS**|  
|-----|-----|
|DisplayName|Nome de exibição do serviço de Federação|  
|Nome do host|Nome do serviço de Federação|  
|Identificador|Identificador de serviço de Federação|  
  
-   No console de gerenciamento do AD FS, selecione **certificados**. Verifique se as comunicações de serviço, descriptografar o token e o token de assinatura certificados pela verificação de cada contra os valores exportadas para o arquivo certificates.txt ao preparar para a migração.  
  
Para alterar os certificados de descriptografia de token ou token de assinatura de certificados autoassinados padrão para certificados externos, primeiro desabilite o recurso de sobreposição de certificado automática é habilitado por padrão.  Para fazer isso, você pode usar o seguinte comando do Windows PowerShell:`PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   No console de gerenciamento do AD FS, selecione **pontos de extremidade**. Verifique os pontos de extremidade do AD FS habilitados com a lista de pontos de extremidade do AD FS habilitados exportadas para um arquivo ao preparar para a migração do AD FS.  
  
-   No console de gerenciamento do AD FS, selecione **reivindicação descrições**. Verifique a lista de descrições de declaração do AD FS contra a lista das descrições reivindicação que você exportou para um arquivo ao preparar para a migração do AD FS. Adicione descrições qualquer reivindicação personalizados incluídos no seu arquivo, mas não incluído na lista padrão no AD FS.  Observe que o identificador de declaração no console de gerenciamento é mapeado para ClaimType no arquivo.  Para obter mais informações sobre como adicionar descrições de declaração, veja [adicionar uma descrição reivindicar](../operations/add-a-claim-description.md). Para obter mais informações, consulte a seção "Etapa 1 – exportar configurações de serviço" em preparação para migrar o AD FS 2.0 federação Server.  
  
-   No console de gerenciamento do AD FS, selecione **requerimentos judiciais ou Extrajudiciais provedor confie**. Você deve recriar cada relação de confiança do provedor de declarações manualmente usando o **declarações do provedor de confiança Assistente para adicionar**.  Use a lista de relações de confiança de provedor de declarações que você exportou e gravados ao preparar para a migração do AD FS. Você pode ignorar a confiança do provedor de declarações com identificador "AD autoridade" no arquivo porque esta é a confiança do provedor de declarações de "Active Directory" que faz parte da configuração do AD FS padrão.  No entanto, verifica se há quaisquer regras de declaração personalizada que você adicionou a relação de confiança do Active Directory antes da migração. Para obter mais informações sobre como criar declarações relações de confiança do provedor, consulte [criar um requerimentos judiciais ou Extrajudiciais provedor confiar usando federação metadados](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) ou [criar um requerimentos judiciais ou Extrajudiciais provedor confiar manualmente](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   No console de gerenciamento do AD FS, **selecione contar terceiros confie**. Você deve recriar cada relação de confiança do parceiro dependente manualmente usando o **dependência terceiros confiança Assistente para adicionar**. Use a lista de terceira relações de confiança de terceiros que você exportou e gravados ao preparar para a migração do AD FS. Para obter mais informações sobre a criação de relações de confiança de terceiros confiante, consulte [criar uma dependência terceiros confiar usando federação metadados](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) ou [criar uma dependência terceiros confiar manualmente](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)




-   
-    