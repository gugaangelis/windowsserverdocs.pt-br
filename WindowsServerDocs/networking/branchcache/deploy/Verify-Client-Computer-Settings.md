---
title: Verifique as configurações do computador cliente
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d628886186474d3f05d7961ca3d3b45b8bf12e73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834047"
---
# <a name="verify-client-computer-settings"></a>Verifique as configurações do computador cliente

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para verificar se o computador cliente está configurado corretamente para BranchCache.  
  
> [!NOTE]  
> Esse procedimento inclui as etapas para atualizar manualmente a política de grupo e para reiniciar o serviço BranchCache. Você não precisará realizar essas ações se você reinicializar o computador, conforme elas ocorrem automaticamente nessa circunstância.  
  
Você deve ser um membro da **administradores**, ou equivalente a executar este procedimento.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Para verificar as configurações do computador cliente BranchCache  
  
1.  Para atualizar a política de grupo no computador cliente cuja configuração do BranchCache que você deseja verificar, execute o Windows PowerShell como administrador, digite o seguinte comando e pressione ENTER.  
  
    `gpupdate /force`  
  
2.  Para computadores cliente que são configurados no modo de cache hospedado e são configurados descobrir automaticamente os servidores de cache hospedado pelo ponto de conexão de serviço, execute os seguintes comandos para parar e reiniciar o serviço BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Inspecione o modo operacional atual do BranchCache, executando o comando a seguir.  
  
    `Get-BCStatus`  
  
4.  No Windows PowerShell, examine a saída do **Get-BCStatus** comando.  
  
    O valor para **BranchCacheIsEnabled** deve ser **verdadeiro**.  
  
    Na **ClientSettings**, o valor para **CurrentClientMode** deve ser **DistributedClient** ou **HostedCacheClient**, dependendo do modo que você configurou usando este guia.  
  
    Na **ClientSettings**, se você configurou o modo de cache hospedado e fornecido os nomes dos seus servidores de cache hospedado durante a configuração ou se o cliente tem automaticamente localizado hospedado usando pontos de conexão de serviço, de servidores de cache  **HostedCacheServerList** deve ter um valor que é o mesmo que o nome ou nomes de seus servidores de cache hospedado. Por exemplo, se seu servidor de cache hospedado é chamado HCS1 e seu domínio for corp.contoso.com, o valor para **HostedCacheServerList** é **HCS1.corp.contoso.com**.  
  
5.  Se qualquer um do BranchCache configurações listadas acima não tem os valores corretos, use as etapas neste guia para verificar as configurações de diretiva de grupo ou diretiva de computador Local, bem como as exceções de firewall que você configurou, e certifique-se de que elas estão corretas. Além disso, reinicie o computador ou siga as etapas neste procedimento para atualizar a política de grupo e reinicie o serviço de BranchCache.  
  


