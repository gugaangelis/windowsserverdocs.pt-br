---
title: Defina o método de ordenação dos destinos nas referências
description: Este artigo descreve como definir o método de ordenação para alvos em referências.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: bb42a98666941c5dfa50a8dfbf45635ad25dc767
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386136"
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>Defina o método de ordenação dos destinos nas referências

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Uma referência é uma lista ordenada de destinos que um computador cliente recebe de um controlador de domínio ou servidor de namespace quando o usuário acessa uma raiz de namespace ou pasta com destinos. Depois de receber o cliente, o cliente tenta acessar o primeiro alvo na lista. Se o alvo não estiver disponível, o cliente tenta acessar o próximo alvo.
Os destinos no site do cliente sempre são listados primeiro em uma referência. Os destinos fora do site do cliente são listados acordo com o método de ordenação.

Use as seções a seguir para especificar em que ordem destinos devem ser encaminhados aos clientes e para entender os diferentes métodos de ordenação indicações de destino:

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>Para definir o método de ordenação para alvos em referências de raiz do namespace

Use o procedimento a seguir para definir o método de ordenação na raiz do namespace:

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em um namespace e, em seguida, clique em **Propriedades**.

3.  Sobre o **indicações**, selecione um método de ordenação.

> [!NOTE]
> Para usar o Windows PowerShell para definir o método de ordenação de destinos em referências de raiz do namespace, use o [conjunto DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) cmdlet com um dos seguintes parâmetros:
>    -   **EnableSiteCosting** Especifica o **ordenação de custo mais baixo** método
>    -   **EnableInsiteReferrals** especifica o método de ordenação **Excluir destinos fora do site do cliente**
>    -   Omitir o parâmetro especifica o **ordem aleatória** ordenação de método de referência. 

O módulo DFSN do Windows PowerShell foi introduzido no Windows Server 2012.
   
## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>Para definir o método de ordenação para alvos nas referências de pasta

Pastas com destinos herdam o método de ordenação da raiz do namespace. Você pode substituir o método de ordenação usando o seguinte procedimento:

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em uma pasta com destinos e, em seguida, clique em **Propriedades**.

3.  Sobre o **indicações**, selecione o **excluir destinos fora do site do cliente** caixa de seleção.

> [!NOTE]
> Para usar o Windows PowerShell para excluir destinos de pasta fora do site do cliente, use o [conjunto DfsnFolder – EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx) cmdlet.

## <a name="target-referral-ordering-methods"></a>Métodos de ordenação de referência de destino

Os três métodos de ordenação são:

-   Ordem aleatória
-   Custo mais baixo
-   Excluir destinos fora do site do cliente

## <a name="random-order"></a>Ordem aleatória

Nesse método, os destinos são ordenados da seguinte maneira:

1.  Os destinos no mesmo site de AD DS (serviços de diretório Active Directory) como o cliente são listados em ordem aleatória na parte superior da referência.
2.  Os destinos fora do site do cliente são listados em ordem aleatória.

Se não houver servidores de destino mesmo site disponíveis, o computador cliente é chamado em um servidor de destino aleatório, independentemente de quanto a conexão custa ou distante como o destino.

## <a name="lowest-cost"></a>Custo mais baixo

Nesse método, os destinos são ordenados da seguinte maneira:

1.  Os destinos no mesmo site como o cliente são listados em ordem aleatória na parte superior da referência.
2.  Os destinos fora do site do cliente são listados em ordem de menor custo mais alto custo. Referências com o mesmo custo são agrupadas, e os destinos são listados em ordem aleatória dentro de cada grupo.

> [!NOTE]
> Custos do link de site não são mostrados no Gerenciamento DFS snap-in. Para ver custos de link de site, use o snap-in Serviços e Sites do Active Directory.

## <a name="exclude-targets-outside-of-the-clients-site"></a>Excluir destinos fora do site do cliente

Nesse método, a referência contém apenas os alvos que estão no mesmo site que o cliente. Essas metas de mesmo site são listadas em ordem aleatória. Se nenhum destinos no mesmo site existirem, o cliente não recebe uma referência e não pode acessar essa parte do namespace.

> [!NOTE]
> Os destinos que têm prioridade de destino definida como "primeiro entre todos os destinos" ou "última entre todos os destinos" ainda são listados na referência, mesmo que o método de ordenação é definido como **excluir destinos fora do site do cliente**.

## <a name="see-also"></a>Consulte também 

-   [Ajustar namespaces do DFS](tuning-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)