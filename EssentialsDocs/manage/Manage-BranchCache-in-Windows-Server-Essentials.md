---
title: Gerenciar o BranchCache no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e09df21aa82260fc30df772853d4e7d8543d2204
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470791"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gerenciar o BranchCache no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O BranchCache pode ajudá-lo a otimizar o uso da Internet, melhorar o desempenho de aplicativos em rede e reduzir o tráfego na WAN quando o servidor do Windows Server Essentials está localizado remotamente do seu escritório ou quando os computadores cliente conectados a um servidor local usam recursos baseados em nuvem, como bibliotecas do SharePoint Online.

 Com o BranchCache habilitado, quando um computador cliente solicita conteúdo de um servidor remoto do Windows Server Essentials, o conteúdo é armazenado em cache no escritório local. Depois disso, outros computadores cliente da mesma filial podem obter o conteúdo localmente, em vez de baixá-lo do servidor novamente por WAN. Isso pode aprimorar o desempenho de aplicativos em rede e reduzir o consumo de largura de banda por WAN.

 Se o servidor do Windows Server Essentials é local ou remoto, o BranchCache pode melhorar os tempos de resposta para pastas compartilhadas do servidor e conteúdo da Web hospedado no servidor (como bibliotecas do SharePoint Online).

 Como o BranchCache não exige novo hardware nem mudanças na topologia de rede, esse recurso oferece a você um modo simples de otimizar o uso de largura de banda e aprimorar os tempos de resposta para serviços e recursos acessados por WAN.

## <a name="branchcache-scenarios"></a>Cenários do BranchCache
 Há três cenários básicos para usar o BranchCache com um servidor remoto:

-   O servidor do Windows Server Essentials está hospedado no Microsoft Azure.

-   O servidor do Windows Server Essentials está hospedado no datacenter de um provedor de serviços de terceiros.

-   O servidor do Windows Server Essentials está em outro escritório em um local físico diferente.

## <a name="distributed-cache-mode"></a>Modo de cache distribuído
 No Windows Server Essentials, o BranchCache é implementado no *modo de cache distribuído*, um dos dois modos de cache disponíveis no BranchCache. No modo de cache distribuído, o cache de conteúdo na filial é distribuído entre os computadores cliente. Já que não há requisitos adicionais de hardware ou mudanças de topologia, esse modo funciona bem para filiais pequenas, que usam um servidor remoto ou um servidor local para acessar serviços baseados em nuvem como o SharePoint Online. Quando você ativa o BranchCache no Windows Server Essentials, o modo de cache distribuído é implementado.

> [!NOTE]
>  Em filiais maiores com mais de uma sub-rede, ou então com um grande número de funcionários usando aplicativos em rede, pode ser vantajoso implementar o BranchCache no *modo de cache hospedado*. No modo de cache hospedado, o cache de conteúdo é armazenado em um ou mais servidores de cache hospedado na filial.

## <a name="requirements"></a>Requisitos
 Para usar o BranchCache no Windows Server Essentials, o servidor e os computadores cliente devem atender aos seguintes requisitos:

-   O servidor deve estar executando o sistema operacional Windows Server Essentials ou o sistema operacional Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter com a função experiência do Windows Server Essentials.

     Em um servidor Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter, o BranchCache é adicionado quando você adiciona a função experiência do Windows Server Essentials. Para ativar o BranchCache, será necessário entrar no painel do Windows Server Essentials com as credenciais de administrador de domínio.

-   Os computadores cliente devem estar executando o sistema operacional Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise ou Windows 8.1 Enterprise.

-   No modo de cache distribuído, todos os computadores cliente devem estar na mesma sub-rede.

    > [!NOTE]
    >  Se você conectou computadores cliente a seu servidor do Windows Server Essentials sem ingressá-los no domínio, esses computadores serão excluídos do caching por padrão. Para incluir um computador cliente que não está ingressado em domínio no caching, execute o cmdlet [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) do Windows PowerShell no computador cliente. Para obter mais informações, consulte [Cmdlets BranchCache no Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).


## <a name="turn-branchcache-on"></a>Ativar o BranchCache
 Para ativar o BranchCache no modo de cache distribuído, basta clicar em um botão no painel do Windows Server Essentials. O caching começa imediatamente e é realizado de modo transparente.

#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Para ativar o BranchCache no Windows Server Essentials

1.  Entre no servidor do Windows Server Essentials com sua conta de administrador.

2.  No painel do Windows Server Essentials, clique em **configurações**.

     O Assistente de Configurações é aberto.

3.  Clique em **BranchCache**.

4.  Na página **Configurações do BranchCache** , clique em **Ativar**.

## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Use o Windows PowerShell para ativar ou desativar o BranchCache
 Você pode usar o Windows PowerShell para verificar o status do BranchCache (Habilitado ou Desabilitado) e ativar ou desativar o BranchCache.

#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Para ativar ou desativar o BranchCache usando o Windows PowerShell

1.  No servidor, abra o Windows PowerShell como administrador. Na tela **Iniciar**, clique com o botão direito do mouse em **Windows PowerShell**, clique em **Executar como Administrador** e, por fim, clique em **Sim**.

2.  No prompt de comando, insira qualquer um dos cmdlets a seguir.

    -   Para verificar o status do BranchCache (Habilitado ou Desabilitado), insira:

        ```powershell
        Get-WSSBranchCacheStatus
        ```

    -   Para ativar o BranchCache, insira:

        ```powershell
        Enable-WSSBranchCache
        ```

    -   Para desativar o BranchCache, insira:

        ```powershell
        Disable-WSSBranchCache
        ```

## <a name="additional-references"></a>Referências adicionais

-   [Visão geral do BranchCache](https://technet.microsoft.com/library/hh831696.aspx)

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
