---
title: "Migrar um farm de trabalho do servidor de Federação 2.0 do AD FS"
description: "Fornece informações sobre como migrar um farm de trabalho do AD FS server 2.0 para o Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar um farm de trabalho do AD FS 2.0  
Este documento fornece informações detalhadas sobre como migrar um anúncio farm FS 2.0 Windows interno banco de dados (trabalho) para o Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrar um farm de trabalho do AD FS
Para migrar um farm de trabalho para Windows Server 2012, execute o seguinte procedimento:  
  
1.  Para cada nó (servidor) no farm trabalho, revise e execute os procedimentos [preparar para migrar um farm de trabalho](prepare-to-migrate-a-wid-farm.md).  
  
2.  Remova todos os nós não principais balanceador de carga.  
  
3.  Atualização do sistema operacional neste servidor do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS nesse servidor é perdida e a função de servidor 2.0 AD FS é removida. A função de servidor Windows Server 2012 AD FS é instalada em vez disso, mas ela não está configurada. Você deve criar a configuração original do AD FS e restaurar as configurações do AD FS restantes para concluir a migração do servidor de Federação.  
  
4.  Crie a configuração original do AD FS nesse servidor.  
  
Você pode criar a configuração do AD FS original usando o **Assistente de configuração de servidor de Federação do AD FS** para adicionar um servidor de Federação a um farm de trabalho. Para obter mais informações, consulte [adicionar um servidor de federação para um Farm de servidores de Federação](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quando você atingir o **especificar o servidor de Federação primário e uma conta de serviço** de página no **Assistente de configuração de servidor de Federação do AD FS**, insira o nome do servidor de Federação principal do sítio trabalho e certifique-se de inserir as informações de conta de serviço que você gravou ao preparar para a migração do AD FS. Para obter mais informações, consulte [preparar para migrar o AD FS 2.0 servidor de Federação](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando você atingir o **especificar o nome do serviço de Federação** página, certifique-se de selecionar o mesmo certificado SSL você gravou no "preparar para migrar um farm de trabalho" no [preparar para migrar o AD FS 2.0 servidor de Federação](prepare-to-migrate-a-wid-farm.md).  
  
5.  Atualize suas páginas da Web do AD FS nesse servidor. Se você backup de suas páginas da Web do AD FS personalizadas ao preparar para a migração, você precisará usar seus dados de backup para substituir o padrão do AD FS páginas da Web que foram criados por padrão no **%systemdrive%\inetpub\adfs\ls** diretório como resultado da configuração do AD FS no Windows Server 2012.  
  
6.  Adicione o servidor que tenha acabado de atualizar para o Windows Server 2012 ao balanceador de carga.  
  
7.  Repita as etapas 1 a 6 para os servidores secundários restantes em farm seu trabalho.  
  
8.  Promova um dos servidores secundários atualizados para ser o principal servidor farm seu trabalho. Para fazer isso, abra o Windows PowerShell e execute o seguinte comando:`PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Remova o servidor primário original da Fazenda seu trabalho balanceador de carga.  
  
10. Rebaixe o servidor primário original no farm seu trabalho para ser um servidor secundário usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para rebaixar o servidor principal original para ser um servidor secundário:`PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. Atualização do sistema operacional neste nó último (servidor) em seu farm de trabalho do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS nesse servidor é perdida e a função de servidor 2.0 AD FS é removida. A função de servidor Windows Server 2012 AD FS é instalada em vez disso, mas ela não está configurada. Você deve criar a configuração original do AD FS e restaurar as configurações do AD FS restantes para concluir a migração do servidor de Federação manualmente.  
  
12. Crie a configuração original do AD FS neste último nó (servidor) no farm seu trabalho.  
  
Você pode criar a configuração do AD FS original usando o **Assistente de configuração de servidor de Federação do AD FS** para adicionar um servidor de Federação a um farm de trabalho. Para obter mais informações, consulte [adicionar um servidor de federação para um Farm de servidores de Federação](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quando você atingir o **especificar o servidor de Federação principal e uma conta de serviço** de página no **Assistente de configuração de servidor de Federação do AD FS**, insira as informações de conta de serviço que você gravou ao preparar para a migração do AD FS. Para obter mais informações, consulte [preparar para migrar o AD FS 2.0 servidor de Federação](prepare-to-migrate-a-wid-farm.md). 
>  
> Quando você atingir o **especificar o nome do serviço de Federação** página, certifique-se de selecionar o mesmo certificado SSL gravados no [preparar para migrar o AD FS 2.0 servidor de Federação](prepare-to-migrate-a-wid-farm.md).  
  
13. Atualize suas páginas da Web do AD FS nesse servidor última farm seu trabalho. Se você fez backup de suas páginas da Web do AD FS personalizadas ao preparar para a migração, usar seus dados de backup para substituir o padrão do AD FS páginas da Web que foram criados por padrão no **%systemdrive%\inetpub\adfs\ls** diretório como resultado da configuração do AD FS no Windows Server 2012.  
  
14. Adicione este último servidor do seu farm de trabalho que você acabou de atualizar para o Windows Server 2012 ao balanceador de carga.  
  
15. Restaure as personalizações do AD FS restantes, como o atributo personalizado lojas.  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)