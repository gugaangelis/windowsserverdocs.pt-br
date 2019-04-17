---
title: "Migrar um farm SQL do servidor de Federação 2.0 do AD FS"
description: "Fornece informações sobre como migrar um farm SQL server 2.0 AD FS para o Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar um farm de trabalho do AD FS 2.0  
Este documento fornece informações detalhadas sobre como migrar um farm SQL do AD FS 2.0 para o Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrar um farm de servidores do SQL  
 Para migrar um farm de servidores do SQL para o Windows Server 2012, execute o seguinte procedimento:  
  
1.  Para cada servidor no SQL Server farm, revise e execute os procedimentos [migrar um farm de servidores SQL](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Remova o balanceador qualquer servidor no SQL Server farm.  
  
3.  Atualize o sistema operacional nesse servidor no SQL Server farm do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS nesse servidor é perdida e a função de servidor 2.0 AD FS é removida. A função de servidor Windows Server 2012 AD FS é instalada em vez disso, mas ela não está configurada. Você deve criar a configuração original do AD FS e restaurar as configurações do AD FS restantes para concluir a migração do servidor de Federação manualmente.  
  
4.  Crie a configuração original do AD FS nesse servidor no SQL Server farm usando cmdlets do Windows PowerShell do AD FS para adicionar um servidor a um farm existente.  
  
> [!IMPORTANT]
>  Você deve usar o Windows PowerShell para criar a configuração original do AD FS se você estiver usando o SQL Server para armazenar seu banco de dados de configuração do AD FS.  

  - Abra o Windows PowerShell e execute o seguinte comando:`$fscredential = Get-Credential`.  
  - Insira o nome e a senha da conta de serviço que você gravou ao preparar seu farm do SQL Server para migração.  
  - Execute o seguinte comando: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, onde `Data Source`é o valor de origem de dados no valor de cadeia de caracteres de conexão do política loja no arquivo a seguir:`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5.  Adicione o servidor que tenha acabado de atualizar para o Windows Server 2012 ao balanceador de carga.  
  
6.  Repita as etapas 2 a 6 para os nós restantes no SQL Server farm.  
  
7.  Quando todos os servidores no SQL Server farm são atualizados para o Windows Server 2012, restaure as personalizações do AD FS restantes, como o atributo personalizado lojas.  

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)



