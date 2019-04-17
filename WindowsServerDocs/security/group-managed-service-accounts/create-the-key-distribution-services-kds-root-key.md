---
title: "Crie a chave raiz KDS de serviços de distribuição de chaves"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 30075e56f3ca8e90a0655508efeacfcf2aaa0337
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Crie a chave raiz KDS de serviços de distribuição de chaves

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve como criar uma chave de raiz do serviço de distribuição de chaves do Microsoft (kdssvc.dll) no controlador de domínio usando o Windows PowerShell para gerar agrupar conta de serviço gerenciado senhas no Windows Server 2012.

 Controladores de domínio do Windows Server 2012 (DC) exigem uma chave raiz para começar a gerar gMSA senhas. Os controladores de domínio esperará até 10 horas de tempo de criação para permitir que todos os controladores de domínio convergir sua replicação do AD antes de permitir a criação de um gMSA. 10 horas é uma medida de segurança para evitar a geração de senha ocorra antes de todos os controladores de domínio no ambiente são capazes de responder às solicitações de gMSA.  Se você tentar usar um gMSA muito cedo a chave não pode ter sido replicada a todos os Windows Server 2012 DC e, portanto, a senha de recuperação pode falhar quando o host gMSA tenta recuperar a senha. Falhas de recuperação de senha gMSA também podem ocorrer quando usando controladores de domínio com agendas de duplicação limitada ou se há um problema de replicação.

A associação a **Admins. do domínio** ou **administradores corporativos** grupos, ou equivalente, é o requisito mínimo para concluir este procedimento. Para obter informações detalhadas sobre como usar as contas apropriadas e associações de grupo, consulte [Local e os grupos de domínio padrão](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Uma arquitetura de 64 bits é necessária para executar os comandos do Windows PowerShell que são usados para administrar as contas de serviço gerenciado do grupo.

#### <a name="to-create-the-kds-root-key-using-the-new-kdsrootkey-cmdlet"></a>Para criar a chave de raiz KDS usando o cmdlet New-KdsRootKey

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Add-KdsRootKey - EffectiveImmediately**

    > [!TIP]
    > O parâmetro de tempo efetivo pode ser usado para conceder tempo para chaves sejam propagadas para todos os controladores de domínio antes de usar. Usar Add-KdsRootKey - EffectiveImmediately adicionará uma chave raiz para o destino de controlador de domínio que será usado pelo serviço KDS imediatamente. No entanto, outros controladores de domínio do Windows Server 2012 não será capaz de usar a chave raiz até replicação for bem-sucedida.

Para ambientes de teste com apenas um controlador de domínio, você pode criar uma chave de raiz KDS e definir a hora de início no passado para evitar a espera de intervalo na geração de chaves usando o procedimento a seguir. Valide que um evento 4004 tiver sido registrado no log de eventos kds.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Para criar a chave de raiz KDS em um ambiente de teste para eficácia imediato

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando para o módulo do Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **$um = Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey - EffectiveTime $b**

    Ou use um único comando

    **Add-KdsRootKey - EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Consulte também
[Introdução ao grupo de contas de serviço gerenciado](getting-started-with-group-managed-service-accounts.md)


