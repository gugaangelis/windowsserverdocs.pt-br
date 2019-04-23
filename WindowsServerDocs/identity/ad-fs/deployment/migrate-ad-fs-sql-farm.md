---
title: Migrar um farm do AD FS 2.0 federation server SQL
description: Fornece informações sobre como migrar um farm do AD FS 2.0 server SQL para o Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843567"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar um farm WID do AD FS 2.0  
Este documento fornece informações detalhadas sobre como migrar um farm do AD FS 2.0 SQL para o Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrar um farm de SQL Server  
 Para migrar um farm do SQL Server para Windows Server 2012, execute o procedimento a seguir:  
  
1.  Para cada servidor no farm do SQL Server, analise e realize os procedimentos [migrar um farm de servidores SQL](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Remova os servidores do seu farm do SQL Server do balanceador de carga.  
  
3.  Atualize o sistema operacional nesse servidor no farm do SQL Server do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor AD FS do Windows Server 2012 está instalada em vez disso, mas ele não está configurado. Você deverá criar manualmente a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.  
  
4.  Crie a configuração original do AD FS neste servidor no seu farm do SQL Server usando cmdlets do AD FS Windows PowerShell para adicionar um servidor a um farm existente.  
  
> [!IMPORTANT]
>  Você deve usar o Windows PowerShell para criar a configuração original do AD FS se usar o SQL Server para armazenar seu banco de dados de configuração do AD FS.  

  - Abra o Windows PowerShell e execute o seguinte comando: `$fscredential = Get-Credential`.  
  - Digite o nome e a senha da conta de serviço definida ao preparar a migração do farm do SQL Server.  
  - Em seguida, execute o seguinte comando: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`onde `Data Source` é o valor da fonte de dados no valor da cadeia de conexão do repositório de políticas no seguinte arquivo: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5.  Adicione o servidor que você acabou de atualizar para o Windows Server 2012 para o balanceador de carga.  
  
6.  Repita as etapas 2 a 6 para os nós restantes no seu farm do SQL Server.  
  
7.  Quando todos os servidores no farm do SQL Server forem atualizados para o Windows Server 2012, restaure as personalizações restantes do AD FS, como repositórios de atributos personalizados.  

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)



