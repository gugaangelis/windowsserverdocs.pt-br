---
title: Migrar um farm de WID do servidor de Federação AD FS 2,0
description: Fornece informações sobre a migração de um farm de WID do servidor AD FS 2,0 para o Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: 8ee9d015b56dfec59e1d13a9ffa51f6297d60787
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962750"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar um farm de WID AD FS 2,0
Este documento fornece informações detalhadas sobre como migrar um farm de WID (banco de dados interno do Windows) AD FS 2,0 para o Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrar um farm AD FS WID
Para migrar um farm WID para o Windows Server 2012, execute o seguinte procedimento:

1.  Para cada nó (servidor) no farm WID, examine e execute os procedimentos em [preparar para migrar um farm wid](prepare-to-migrate-a-wid-farm.md).

2.  Remova os nós não primários do balanceador de carga.

3.  Atualização do sistema operacional neste servidor do Windows Server 2008 R2 ou do Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Instalando o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)).

> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor do Windows Server 2012 AD FS está instalada, mas não está configurada. Você deverá criar a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.

4. Criar a configuração original do AD FS neste servidor.

Você pode criar a configuração original do usando o **Assistente de configuração do servidor de federação do AD FS** para adicionar um servidor de federação a um farm do WID. Para saber mais, confira [Adicionar um servidor de federação a um farm do servidor de federação](add-a-federation-server-to-a-federation-server-farm.md).

> [!NOTE]
> Ao chegar na página **Especifique o servidor de federação primário e uma conta de serviço** no **Assistente de configuração de federação do AD FS**, digite o nome do servidor de federação primário do farm do WID e as informações da conta de serviço definidas durante a preparação para a migração do AD FS. Para obter mais informações, consulte [preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-a-wid-farm.md).
>
> Quando você atingir a página **especificar o nome do serviço de Federação** , certifique-se de selecionar o mesmo certificado SSL que você registrou em "preparar para migrar um farm wid" em [preparar para migrar o servidor de Federação AD FS 2,0](prepare-to-migrate-a-wid-farm.md).

5. Atualize suas páginas da web do AD FS neste servidor. Se você tiver feito backup de suas páginas da Web AD FS personalizadas ao se preparar para a migração, será necessário usar seus dados de backup para substituir as páginas da Web do AD FS padrão que foram criadas por padrão no diretório **%systemdrive%\inetpub\adfs\ls** como resultado da configuração de AD FS no Windows Server 2012.

6. Adicione o servidor que você acabou de atualizar para o Windows Server 2012 para o balanceador de carga.

7. Repita as etapas 1 a 6 para os servidores restantes no seu farm de WID.

8. Promova um dos servidores secundários atualizados como servidor primário do seu farm de WID. Para fazer isso, abra o Windows PowerShell e execute o seguinte comando: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.

9. Remover o servidor primário original do seu farm de WID do balanceador de carga.

10. Remova o servidor primário original do seu farm de WID do balanceador de carga usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o comando para rebaixar o servidor primário original como servidor secundário: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.

11. Atualização do sistema operacional neste último nó (servidor) em seu farm de WID do Windows Server 2008 R2 ou do Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Instalando o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)).

> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor do Windows Server 2012 AD FS está instalada, mas não está configurada. Você deverá criar manualmente a configuração original do AD FS e restaurar as definições restantes do AD FS para concluir a migração do servidor de federação.

12. Crie a configuração original do AD FS neste último nó (servidor) no seu farm de WID.

Você pode criar a configuração original do usando o **Assistente de configuração do servidor de federação do AD FS** para adicionar um servidor de federação a um farm do WID. Para saber mais, confira [Adicionar um servidor de federação a um farm do servidor de federação](add-a-federation-server-to-a-federation-server-farm.md).

> [!NOTE]
> Ao chegar à página **Especifique o servidor de federação primário e uma conta de serviço** no **Assistente de configuração de federação do AD FS**, digite as informações da conta de serviço definidas durante a preparação para a migração do AD FS. Para obter mais informações, consulte [preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-a-wid-farm.md).
>
> Quando você atingir a página **especificar o nome do serviço de Federação** , certifique-se de selecionar o mesmo certificado SSL que você registrou em [preparar para migrar o servidor de federação do AD FS 2,0](prepare-to-migrate-a-wid-farm.md).

13. Atualize suas páginas da web do AD FS neste último servidor do seu farm do WID. Se você tiver feito backup de suas páginas da Web AD FS personalizadas ao se preparar para a migração, use seus dados de backup para substituir as páginas da Web do AD FS padrão que foram criadas por padrão no diretório **%systemdrive%\inetpub\adfs\ls** como resultado da configuração de AD FS no Windows Server 2012.

14. Adicione esse último servidor do seu farm de WID que você acabou de atualizar para o Windows Server 2012 para o balanceador de carga.

15. Restaure as personalizações restantes do AD FS, tais como armazenamento de atributos personalizados.

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparar para migrar o proxy do servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar o AD FS servidor de Federação do 2,0](migrate-the-ad-fs-fed-server.md) [migrar o proxy do servidor de Federação do AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) migrar os [agentes Web do AD FS 1,1](migrate-the-ad-fs-web-agent.md)
