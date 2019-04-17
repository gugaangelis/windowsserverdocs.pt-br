---
title: "O que há de novo para contas de serviço gerenciado"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-for-managed-service-accounts"></a>O que & #39; s novo para contas de serviço gerenciado

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve as alterações na funcionalidade para contas de serviço gerenciado com a introdução do grupo de conta de serviço gerenciado (gMSA) no Windows Server 2012 e Windows 8.

A conta de serviço gerenciado é projetada para fornecer serviços e tarefas como pools de aplicativos do IIS para compartilhar suas próprias contas de domínio, eliminando a necessidade de um administrador administrar manualmente as senhas para essas contas e serviços do Windows. É uma conta de domínio gerenciado que fornece gerenciamento automático de senha.

## <a name="versions"></a>Quais são as novidades para contas de serviço gerenciado no Windows Server 2012 e Windows 8
A seguir descreve quais alterações na funcionalidade foram feitas MSA no Windows Server 2012 e Windows 8.

### <a name="group-managed-service-accounts"></a>Contas de serviço gerenciado do grupo
Quando uma conta de domínio está configurada para um servidor em um domínio, o computador cliente pode autenticar e se conectar a esse serviço. Anteriormente, apenas dois tipos de conta tem fornecido identidade sem a necessidade de gerenciamento de senhas. Mas esses tipos de conta têm limitações:

-   Conta de computador é limitada a um servidor de domínio e as senhas são gerenciadas pelo computador

-   Conta de serviço gerenciado é limitada ao servidor de um domínio e as senhas são gerenciadas pelo computador.

Essas contas não podem ser compartilhadas entre vários sistemas. Portanto, você deverá manter regularmente a conta para cada serviço em cada sistema para evitar a expiração de senha indesejados.

**O valor adicione essa alteração?**

O grupo de conta de serviço gerenciado resolve esse problema porque a senha da conta é gerenciada pelos controladores de domínio do Windows Server 2012 e pode ser recuperada por vários sistemas Windows Server 2012. Isso reduz a sobrecarga administrativa de uma conta de serviço, permitindo que o Windows lidar com o gerenciamento de senhas para essas contas.

**O que funciona de modo diferente?**

Em computadores executando o Windows Server 2012 ou Windows 8, um grupo MSA pode ser criado e gerenciado pelo Gerenciador de controle de serviço para que várias instâncias do serviço, como implantados por um farm de servidores, pode ser gerenciado de um servidor. Ferramentas e utilitários que você usou para administrar as contas de serviço gerenciado, como o Gerenciador de Pool de aplicativos do IIS, podem ser usados com contas de serviço gerenciado do grupo. Os administradores de domínio podem delegar o gerenciamento de serviço para administradores de serviços, que podem gerenciar o ciclo de vida de uma conta de serviço gerenciado ou o grupo de conta de serviço gerenciado. Computadores cliente existente será capazes de autenticar tais serviços sem saber qual instância de serviço que eles sejam autenticados para.

### <a name="interoperability"></a>Funcionalidade removida ou preterida
Para o Windows Server 2012, o padrão de cmdlets do Windows PowerShell para gerenciar as contas de serviço do grupo gerenciado em vez das contas de serviço gerenciado do servidor.

## <a name="see-also"></a>Consulte também

-   [Visão geral de contas de serviço gerenciado grupo](group-managed-service-accounts-overview.md)

-   [Visão geral dos serviços de domínio do Active Directory](active-directory-domain-services-overview.md)

-   [Noções básicas sobre contas de serviço gerenciado:, Implementação de práticas recomendadas e solução de problemas](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


