---
title: Gerenciar BranchCache no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 13d1d439eb9eeb60de9779d783e36405aee3ddfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gerenciar BranchCache no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache pode ajudá-lo a otimizar o uso da Internet, melhorar o desempenho de aplicativos em rede e reduzir o tráfego em sua rede de longa distância (WAN) quando o servidor Windows Server Essentials está localizado remotamente do seu escritório ou quando os computadores cliente conectado a um servidor local usar recursos baseados em nuvem, como bibliotecas do SharePoint Online.  
  
 Com BranchCache habilitado, quando um computador cliente solicita o conteúdo de um servidor remoto do Windows Server Essentials, o conteúdo é armazenado em cache no escritório local. Depois disso, outros computadores no mesmo escritório podem obter o conteúdo localmente, em vez de baixar o conteúdo do servidor novamente na WAN. Isso pode melhorar o desempenho de aplicativos em rede e reduz o consumo de largura de banda na WAN.  
  
 Se o servidor Windows Server Essentials é local ou remoto, BranchCache pode melhorar os tempos de resposta para pastas de servidor compartilhada e o conteúdo da Web hospedado no servidor (por exemplo, o SharePoint Online bibliotecas).  
  
 Porque BranchCache não exige alterações de topologia de rede ou hardware novo, esse recurso oferece uma maneira simple para otimizar o uso de largura de banda e melhorar os tempos de resposta para serviços e recursos acessados pela WAN.  
  
## <a name="branchcache-scenarios"></a>BranchCache cenários  
 Há três cenários básicos de uso BranchCache com um servidor remoto:  
  
-   O servidor Windows Server Essentials é hospedado no Microsoft Azure.  
  
-   O servidor Windows Server Essentials é hospedado no datacenter de um provedor de serviços de terceiros.  
  
-   O servidor Windows Server Essentials está em outro escritório em um local físico diferente.  
  
## <a name="distributed-cache-mode"></a>Modo de cache distribuído  
 No Windows Server Essentials, BranchCache é implementado em *modo cache distribuído*, um dos dois modos de cache disponíveis no BranchCache. No modo de cache distribuído, o cache de conteúdo na filial é distribuído entre computadores cliente. Como nenhum hardware adicional ou alterações de topologia são necessárias, esse modo funciona bem para pequenas empresas que usam um servidor remoto ou usam um servidor local para acessar serviços baseados em nuvem como o SharePoint Online. Quando você ativa BranchCache no Windows Server Essentials, modo de cache distribuído é implementado.  
  
> [!NOTE]
>  Em filiais maiores com mais de uma sub-rede ou com um grande número de funcionários usem aplicativos em rede, pode ser benéfico implementar BranchCache em *hospedado modo cache*. No modo de cache hospedado, o cache de conteúdo é armazenado em um ou mais servidores de cache hospedado na filial.
  
## <a name="requirements"></a>Requisitos  
 Para usar BranchCache no Windows Server Essentials, seus computadores cliente e servidor devem atender aos seguintes requisitos:  
  
-   O servidor deve estar executando o sistema operacional Windows Server Essentials ou o Windows Server 2012 R2 Standard ou sistema de operacional Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials.  
  
     Em um servidor Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter, BranchCache é adicionado quando você adiciona a função de experiência do Windows Server Essentials. Para ativar BranchCache, você precisará fazer logon no painel Windows Server Essentials com credenciais de administrador do domínio.  
  
-   Computadores cliente devem estar executando o Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise ou sistema operacional Windows 8.1 Enterprise.  
  
-   No modo de cache distribuído, todos os computadores cliente devem estar na mesma sub-rede.  
  
    > [!NOTE]
    >  Se você tiver conectado computadores cliente ao seu servidor Windows Server Essentials sem adicionando-os ao domínio, esses computadores são excluídos do cache por padrão. Para incluir um computador cliente que não é ingressado no domínio em cache, execute o [habilitar BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) cmdlet do Windows PowerShell no computador cliente. Para obter mais informações, consulte [BranchCache Cmdlets do Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Ativar BranchCache  
 Para ativar BranchCache no modo de cache distribuído, você simplesmente clicar em um botão no painel Windows Server Essentials. Armazenamento em cache começa imediatamente e é executado de forma transparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Para ativar BranchCache no Windows Server Essentials  
  
1.  Faça login no servidor Windows Server Essentials com sua conta de administrador.  
  
2.  No painel Windows Server Essentials, clique em **configurações**.  
  
     O Assistente de configurações é aberto.  
  
3.  Clique em **BranchCache**.  
  
4.  Sobre o **BranchCache configurações** página, clique em **ativar**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Use o Windows PowerShell para ativar ou desativar o BranchCache  
 Você pode usar o Windows PowerShell para verificar o status de BranchCache (ativado ou desativado) e para ativar ou desativar o BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Para ativar o BranchCache ou desativar usando o Windows PowerShell  
  
1.  No servidor, abra o Windows PowerShell como administrador. Sobre o **iniciar** página, clique com botão direito **do Windows PowerShell**, clique em **executar como administrador**e clique em **Sim**.  
  
2.  Digite qualquer um dos cmdlets a seguir no prompt de comando.  
  
    -   Para verificar o status do BranchCache (ativado ou desativado), digite:  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   Para ativar BranchCache, digite:  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   Para desativar BranchCache, digite:  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>Consulte também  
    
-   [Visão geral do BranchCache](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
