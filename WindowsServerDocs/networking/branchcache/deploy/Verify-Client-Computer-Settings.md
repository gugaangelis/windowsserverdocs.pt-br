---
title: Verificar as configurações do computador cliente
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 14fcc91fc1fcf419a81dd5d774486dc5e0800f4e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937545"
---
# <a name="verify-client-computer-settings"></a>Verificar as configurações do computador cliente

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para verificar se o computador cliente está configurado corretamente para BranchCache.

> [!NOTE]
> Este procedimento inclui etapas para atualizar manualmente Política de Grupo e para reiniciar o serviço do BranchCache. Não será necessário executar essas ações se você reinicializar o computador, pois eles ocorrerão automaticamente nessa circunstância.

Você deve ser membro de **Administradores**ou equivalente para executar este procedimento.

### <a name="to-verify-branchcache-client-computer-settings"></a>Para verificar as configurações do computador cliente do BranchCache

1.  Para atualizar Política de Grupo no computador cliente cuja configuração do BranchCache você deseja verificar, execute o Windows PowerShell como administrador, digite o comando a seguir e pressione ENTER.

    `gpupdate /force`

2.  Para computadores cliente configurados no modo de cache hospedado e são configurados para descobrir automaticamente servidores de cache hospedados pelo ponto de conexão de serviço, execute os seguintes comandos para parar e reiniciar o serviço do BranchCache.

    `net stop peerdistsvc`

    `net start peerdistsvc`

3.  Inspecione o modo operacional do BranchCache atual executando o comando a seguir.

    `Get-BCStatus`

4.  No Windows PowerShell, examine a saída do comando **Get-BCStatus** .

    O valor de **BranchCacheIsEnabled** deve ser **true**.

    Em **ClientSettings**, o valor de **CurrentClientMode** deve ser **DistributedClient** ou **HostedCacheClient**, dependendo do modo que você configurou usando este guia.

    No **ClientSettings**, se você configurou o modo de cache hospedado e forneceu os nomes dos seus servidores de cache hospedados durante a configuração, ou se o cliente localizou automaticamente os servidores de cache hospedados usando pontos de conexão de serviço, **HostedCacheServerList** deve ter um valor igual ao nome ou aos nomes dos seus servidores de cache hospedados. Por exemplo, se o servidor de cache hospedado for denominado HCS1 e seu domínio for corp.contoso.com, o valor de **HostedCacheServerList** será **HCS1.Corp.contoso.com**.

5.  Se qualquer uma das configurações do BranchCache listadas acima não tiver os valores corretos, use as etapas neste guia para verificar as configurações de diretiva de computador local ou Política de Grupo, bem como as exceções de firewall, que você configurou e verifique se elas estão corretas. Além disso, reinicie o computador ou siga as etapas neste procedimento para atualizar Política de Grupo e reiniciar o serviço do BranchCache.



