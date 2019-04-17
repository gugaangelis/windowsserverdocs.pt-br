---
title: Verifique se as configurações do computador cliente
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d507f2097a9349cbefba520ad0dc143e0dd7452e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="verify-client-computer-settings"></a>Verifique se as configurações do computador cliente

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para verificar se o computador cliente está configurado corretamente para BranchCache.  
  
> [!NOTE]  
> Este procedimento inclui as etapas para atualizar manualmente a política de grupo e para reiniciar o serviço BranchCache. Você não precisa executar essas ações se você reiniciar o computador, assim que elas ocorrem automaticamente nesta circunstância.  
  
Você deve ser um membro do **administradores**, ou equivalente a executar este procedimento.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Para verificar as configurações do computador cliente BranchCache  
  
1.  Para atualizar a política de grupo no computador cliente cuja configuração BranchCache que você deseja verificar, execute o Windows PowerShell como administrador, digite o seguinte comando e pressione ENTER.  
  
    `gpupdate /force`  
  
2.  Para computadores cliente que são configurados no modo de cache hospedado e são configurados detectar automaticamente hospedado servidores de cache por ponto de conexão de serviço, execute os seguintes comandos para parar e reiniciar o serviço BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Inspecione o modo operacional atual do BranchCache executando o comando a seguir.  
  
    `Get-BCStatus`  
  
4.  No Windows PowerShell, examine a saída do **Get-BCStatus** comando.  
  
    O valor para **BranchCacheIsEnabled** deve ser **True**.  
  
    Em **ClientSettings**, o valor para **CurrentClientMode** deve ser **DistributedClient** ou **HostedCacheClient**, dependendo do modo em que você configurou usando neste guia.  
  
    Em **ClientSettings**, se o modo de cache hospedado configurado e fornecidas os nomes dos seus servidores de cache hospedado durante a configuração ou se o cliente automaticamente localizou hospedado servidores de cache usando pontos de conexão do serviço, **HostedCacheServerList** deve ter um valor que é o mesmo que o nome ou os nomes de seus servidores de cache hospedado. Por exemplo, se o servidor de cache hospedado é chamado HCS1 e seu domínio é corp.contoso.com, o valor para **HostedCacheServerList** é **HCS1.corp.contoso.com**.  
  
5.  Se qualquer um do BranchCache configurações listadas acima não têm os valores corretos, use as etapas neste guia para verificar as configurações de política de grupo ou política de computador Local, bem como as exceções de firewall, o que você configurou, e verifique se elas estão corretas. Além disso, reinicie o computador ou siga as etapas neste procedimento para atualizar a política de grupo e reinicie o serviço BranchCache.  
  


