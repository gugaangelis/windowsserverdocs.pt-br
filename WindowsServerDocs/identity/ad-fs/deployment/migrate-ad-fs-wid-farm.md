---
title: Migrar um farm WID do AD FS 2.0 federation server
description: Fornece informações sobre como migrar um farm WID do AD FS 2.0 server para o Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868317"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar um farm WID do AD FS 2.0  
Este documento fornece informações detalhadas sobre como migrar do AD FS 2.0 Windows interno banco de dados) farm de WID (Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrar um farm do AD FS WID
Para migrar um farm de WID para o Windows Server 2012, execute o procedimento a seguir:  
  
1.  Para cada nó (servidor) no farm de WID, analise e realize os procedimentos [preparar para migrar um farm de WID](prepare-to-migrate-a-wid-farm.md).  
  
2.  Remova os nós não primários do balanceador de carga.  
  
3.  Atualização do sistema operacional nesse servidor do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor AD FS do Windows Server 2012 está instalada em vez disso, mas ele não está configurado. Você deverá criar a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.  
  
4.  Criar a configuração original do AD FS neste servidor.  
  
Você pode criar a configuração original do usando o **Assistente de configuração do servidor de federação do AD FS** para adicionar um servidor de federação a um farm do WID. Para saber mais, confira [Adicionar um servidor de federação a um farm do servidor de federação](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Ao chegar na página **Especifique o servidor de federação primário e uma conta de serviço** no **Assistente de configuração de federação do AD FS**, digite o nome do servidor de federação primário do farm do WID e as informações da conta de serviço definidas durante a preparação para a migração do AD FS. Para obter mais informações, consulte [Prepare to Migrate the AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando você chegar a **especifique o nome do serviço de Federação** página, certifique-se de selecionar o mesmo certificado SSL que você registrou na "preparar para migrar um farm de WID" em [Prepare to Migrate the AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md).  
  
5.  Atualize suas páginas da web do AD FS neste servidor. Se você tiver feito backup de suas páginas personalizadas do AD FS durante a preparação para a migração, você precisa usar seus dados de backup para substituir a páginas da Web do AD FS que foram criadas por padrão no padrão de **%systemdrive%\inetpub\adfs\ls** diretório como um resultado da configuração do AD FS no Windows Server 2012.  
  
6.  Adicione o servidor que você acabou de atualizar para o Windows Server 2012 para o balanceador de carga.  
  
7.  Repita as etapas 1 a 6 para os servidores restantes no seu farm de WID.  
  
8.  Promova um dos servidores secundários atualizados como servidor primário do seu farm de WID. Para fazer isso, abra o Windows PowerShell e execute o seguinte comando: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Remover o servidor primário original do seu farm de WID do balanceador de carga.  
  
10. Remova o servidor primário original do seu farm de WID do balanceador de carga usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o comando para rebaixar o servidor primário original como servidor secundário: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. A atualização do sistema operacional nesse último nó (servidor) em seu farm de WID do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor AD FS do Windows Server 2012 está instalada em vez disso, mas ele não está configurado. Você deverá criar manualmente a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.  
  
12. Crie a configuração original do AD FS neste último nó (servidor) no seu farm de WID.  
  
Você pode criar a configuração original do usando o **Assistente de configuração do servidor de federação do AD FS** para adicionar um servidor de federação a um farm do WID. Para saber mais, confira [Adicionar um servidor de federação a um farm do servidor de federação](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Ao chegar à página **Especifique o servidor de federação primário e uma conta de serviço** no **Assistente de configuração de federação do AD FS**, digite as informações da conta de serviço definidas durante a preparação para a migração do AD FS. Para obter mais informações, consulte [Prepare to Migrate the AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando você chegar a **especifique o nome do serviço de Federação** página, certifique-se de selecionar o mesmo certificado SSL que você registrou na [Prepare to Migrate the AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md).  
  
13. Atualize suas páginas da web do AD FS neste último servidor do seu farm do WID. Se você fez backup suas páginas personalizadas do AD FS durante a preparação para a migração, use seus dados de backup para substituir a páginas da Web do AD FS que foram criadas por padrão no padrão de **%systemdrive%\inetpub\adfs\ls** diretório como resultado de configuração do AD FS no Windows Server 2012.  
  
14. Adicione este último servidor do seu farm do WID recém-atualizado para o Windows Server 2012 para o balanceador de carga.  
  
15. Restaure as personalizações restantes do AD FS, tais como armazenamento de atributos personalizados.  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)