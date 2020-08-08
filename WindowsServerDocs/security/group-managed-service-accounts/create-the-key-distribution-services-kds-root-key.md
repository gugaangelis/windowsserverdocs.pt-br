---
title: Criar a chave raiz do KDS (serviço de distribuição de chave)
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b90ea772874c2a5731e03f4bcbc44de6efc34a20
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995033"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Criar a chave raiz do KDS (serviço de distribuição de chave)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti descreve como criar uma chave raiz do serviço de distribuição de chaves da Microsoft (kdssvc.dll) no controlador de domínio usando o Windows PowerShell para gerar senhas de conta de serviço gerenciado de grupo no Windows Server 2012 ou posterior.

Controladores de domínio (DC) exigem uma chave raiz para começar a gerar senhas gMSA. Os controladores de domínio aguardarão até 10 horas desde a criação para permitir que todos os controladores de domínio convirjam a replicação do AD antes de permitir a criação de uma gMSA. O período de 10 horas é uma medida de segurança para impedir que a geração de senhas ocorra antes que todos os DCs no ambiente sejam capazes de responder às solicitações da MSA de grupo.  Se você tentar usar um gMSA muito cedo, a chave poderá não ter sido replicada para todos os controladores de domínio e, portanto, a recuperação de senha poderá falhar quando o host gMSA tentar recuperar a senha. as falhas de recuperação de senha do gMSA também podem ocorrer ao usar DCs com agendamentos de replicação limitados ou se houver um problema de replicação.

> [!NOTE]
> Excluir e recriar a chave raiz pode levar a problemas em que a chave antiga continua a ser usada após a exclusão devido ao cache da chave. O KDC (serviço de distribuição de chaves) deve ser reiniciado em todos os controladores de domínio se a chave raiz for recriada.

A associação ao grupo **Admins. do Domínio** ou **Administradores de Empresa** ou equivalente é o mínimo exigido para concluir este procedimento. Para obter informações detalhadas sobre como usar as contas e associações de grupo apropriadas, consulte [grupos padrão de domínio e locais](/previous-versions/orphan-topics/ws.10/dd728026(v=ws.10)).

> [!NOTE]
> Uma arquitetura de 64 bits é exigida para executar os comandos do Windows PowerShell, que são usados para administrar as contas de serviço gerenciado de grupo.

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>Para criar a chave raiz KDS usando o cmdlet Add-KdsRootKey

1.  No controlador de domínio do Windows Server 2012 ou posterior, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Add-KdsRootKey-EffectiveImmediately**

    > [!TIP]
    > O parâmetro de tempo Efetivo pode ser usado para dar tempo para que as chaves sejam propagadas a todos os DCs antes do uso. O uso de Add-KdsRootKey-EffectiveImmediately adicionará uma chave raiz ao controlador de domínio de destino que será usado pelo serviço KDS imediatamente. No entanto, outros controladores de domínio não poderão usar a chave raiz até que a replicação seja bem-sucedida.

Para ambientes de teste com apenas um DC, você pode criar uma chave raiz do KDS e definir a hora de início no passado para evitar a espera do intervalo para a geração de chave usando o procedimento a seguir. Valide que um evento 4004 foi registrado no log de eventos do KDS.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Para criar a chave raiz do KDS em um ambiente de teste e obter eficiência imediata

1.  No controlador de domínio do Windows Server 2012 ou posterior, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **$a = obter Data**

    **$b = $a. AddHours (-10)**

    **Add-KdsRootKey-efetivo $b**

    Como opção, use um só comando

    **Add-KdsRootKey-efetivo (Get-Date). AddHours (-10))**

## <a name="see-also"></a>Consulte Também
[Introdução com contas de serviço gerenciado de grupo](getting-started-with-group-managed-service-accounts.md)