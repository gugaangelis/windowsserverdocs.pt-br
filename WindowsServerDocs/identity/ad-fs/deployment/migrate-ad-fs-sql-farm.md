---
title: Migrar um farm de SQL do servidor de Federação AD FS 2,0
description: Fornece informações sobre como migrar um farm do SQL Server AD FS 2,0 para o Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: f63a797a30d363b8662dfcc6bfcac46f79dc3eb0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938195"
---
# <a name="migrate-an-ad-fs-20-sql-farm"></a>Migrar um farm do SQL AD FS 2,0
Este documento fornece informações detalhadas sobre como migrar um farm do SQL AD FS 2,0 para o Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrar um farm de SQL Server
 Para migrar um farm de SQL Server para o Windows Server 2012, execute o seguinte procedimento:

1.  Para cada servidor em seu farm de SQL Server, examine e execute os procedimentos em [migrar um farm de SQL Server](prepare-to-migrate-a-sql-server-farm.md).

2.  Remova os servidores do seu farm do SQL Server do balanceador de carga.

3.  Atualize o sistema operacional neste servidor em seu farm de SQL Server do Windows Server 2008 R2 ou do Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Instalando o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)).

> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor do Windows Server 2012 AD FS está instalada, mas não está configurada. Você deverá criar manualmente a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.

4. Crie a configuração original do AD FS neste servidor no seu farm do SQL Server usando cmdlets do AD FS Windows PowerShell para adicionar um servidor a um farm existente.

> [!IMPORTANT]
>  Você deve usar o Windows PowerShell para criar a configuração original do AD FS se usar o SQL Server para armazenar seu banco de dados de configuração do AD FS.

  - Abra o Windows PowerShell e execute o seguinte comando: `$fscredential = Get-Credential`.
  - Digite o nome e a senha da conta de serviço definida ao preparar a migração do farm do SQL Server.
  - Em seguida, execute o seguinte comando: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`onde `Data Source` é o valor da fonte de dados no valor da cadeia de conexão do repositório de políticas no seguinte arquivo: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.

5. Adicione o servidor que você acabou de atualizar para o Windows Server 2012 para o balanceador de carga.

6. Repita as etapas 2 a 6 para os nós restantes no seu farm do SQL Server.

7. Quando todos os servidores no farm de SQL Server forem atualizados para o Windows Server 2012, restaure as personalizações restantes do AD FS, como repositórios de atributos personalizados.

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparar para migrar o proxy do servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar o AD FS servidor de Federação do 2,0](migrate-the-ad-fs-fed-server.md) [migrar o proxy do servidor de Federação do AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) migrar os [agentes Web do AD FS 1,1](migrate-the-ad-fs-web-agent.md)
