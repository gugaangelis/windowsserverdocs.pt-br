---
title: What's New for Managed Service Accounts
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cac55d04a40c84ce160eb3883d6095a7db0ef3be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872177"
---
# <a name="what39s-new-for-managed-service-accounts"></a>O que&#39;s novo para contas de serviço gerenciado

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico para profissionais de TI descreve as alterações na funcionalidade para contas de serviço gerenciado com a introdução do grupo de conta de serviço gerenciado (gMSA) no Windows Server 2012 e Windows 8.

A conta de serviço gerenciado foi desenvolvida para fornecer serviços e tarefas, como serviços do Windows e pools de aplicativos do IIS, para compartilhar suas próprias contas de domínio, ao mesmo tempo em que elimina a necessidade de um administrador para administrar manualmente as senhas dessas contas. É uma conta de domínio gerenciado que oferece o gerenciamento automático de senha.

## <a name="versions"></a>Quais são as novidades para contas de serviço gerenciado no Windows Server 2012 e Windows 8
O exemplo a seguir descreve quais alterações na funcionalidade foram feitas para MSA no Windows Server 2012 e Windows 8.

### <a name="group-managed-service-accounts"></a>Contas de serviço gerenciado de grupo
Quando uma conta de domínio está configurada para um servidor em um domínio, o computador cliente pode autenticar e conectar a esse serviço. Anteriormente, apenas dois tipos de conta forneciam identidade sem exigir o gerenciamento de senha. Porém, esses tipos de conta têm limitações:

-   A conta de computador é limitada a um servidor de domínio e as senhas são gerenciadas pelo computador.

-   A Conta de Serviço Gerenciado é limitada a um servidor de domínio e as senhas são gerenciadas pelo computador.

Essas contas não podem ser compartilhadas por vários sistemas. Portanto, você deve manter regularmente a conta de cada serviço em cada sistema para impedir a expiração indesejada da senha.

**Qual é o valor agregado desta alteração?**

A conta de serviço gerenciado de grupo resolve esse problema porque a senha da conta é gerenciada por controladores de domínio do Windows Server 2012 e pode ser recuperada por vários sistemas Windows Server 2012. Isso minimiza a sobrecarga administrativa de uma conta de serviço, permitindo que o Windows controle o gerenciamento de senha dessas contas.

**O que passou a funcionar de maneira diferente?**

Em computadores que executam o Windows Server 2012 ou Windows 8, um grupo de MSA pode ser criada e gerenciada por meio do Gerenciador de controle de serviço para que várias instâncias do serviço, como implantadas em um farm de servidor, pode ser gerenciado de um servidor. Ferramentas e utilitários que você usava para administrar Contas de Serviço Gerenciado, como o Gerenciador de Pool de Aplicativos do IIS, podem ser usados com Contas de Serviço Gerenciado de grupo. Administradores de domínio podem delegar o gerenciamento de serviços a administradores de serviço, que por sua vez podem gerenciar todo o ciclo de vida de uma Conta de Serviços Gerenciados ou da Conta de Serviços Gerenciados de grupo. Os computadores cliente existentes poderão autenticar em qualquer serviço desse tipo sem saber em que instância de serviço estão autenticando.

### <a name="interoperability"></a>Funcionalidade removida ou preterida
Para Windows Server 2012, o padrão de cmdlets do Windows PowerShell para gerenciar as contas de serviço de gerenciado de grupo em vez das contas de serviço gerenciado do servidor.

## <a name="see-also"></a>Consulte também

-   [Visão geral das contas de serviço gerenciado de grupo](group-managed-service-accounts-overview.md)

-   [Visão geral dos serviços de domínio do Active Directory](active-directory-domain-services-overview.md)

-   [Contas de serviço gerenciado: Compreendendo, Implementando, práticas recomendadas e solução de problemas](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


