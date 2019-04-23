---
title: Criar a chave raiz do KDS (serviço de distribuição de chave)
description: Segurança do Windows Server
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
ms.openlocfilehash: 3d5f7b46b28e6a2fbfafb664b69aebc8d34886fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867207"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Criar a chave raiz do KDS (serviço de distribuição de chave)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico para profissionais de TI descreve como criar uma chave de raiz do serviço de distribuição de chaves do Microsoft (kdssvc) no controlador de domínio usando o Windows PowerShell para gerar senhas de conta de serviço gerenciado de grupo no Windows Server 2012.

 Controladores de domínio do Windows Server 2012 (DC) exigem uma chave de raiz para começar a gerar senhas gMSA. Os controladores de domínio aguardarão até 10 horas desde a criação para permitir que todos os controladores de domínio convirjam a replicação do AD antes de permitir a criação de uma gMSA. O período de 10 horas é uma medida de segurança para impedir que a geração de senhas ocorra antes que todos os DCs no ambiente sejam capazes de responder às solicitações da MSA de grupo.  Se você tentar usar uma gMSA muito em breve a chave não pode ter sido replicada para todos os DCs do Windows Server 2012 e, portanto, a recuperação de senha poderá falhar quando o host de gMSA tenta recuperar a senha. falhas de recuperação de senha de gMSA também podem ocorrer ao usar controladores de domínio com agendamentos de replicação limitados ou se há um problema de replicação.

A associação ao grupo **Admins. do Domínio** ou **Administradores de Empresa** ou equivalente é o mínimo exigido para concluir este procedimento. Para obter informações detalhadas sobre como usar as contas e as associações a grupos apropriadas, consulte [Grupos padrão Local e Domínio](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Uma arquitetura de 64 bits é exigida para executar os comandos do Windows PowerShell, que são usados para administrar as contas de serviço gerenciado de grupo.

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>Para criar a chave de raiz do KDS usando o cmdlet Add-KdsRootKey

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **Adicionar-KdsRootKey – EffectiveImmediately**

    > [!TIP]
    > O parâmetro de tempo Efetivo pode ser usado para dar tempo para que as chaves sejam propagadas a todos os DCs antes do uso. Usando Add-KdsRootKey – EffectiveImmediately adicionará uma chave raiz ao DC de destino que será usada pelo serviço do KDS imediatamente. No entanto, outros controladores de domínio do Windows Server 2012 não poderá usar a chave de raiz até que a replicação seja bem-sucedida.

Para ambientes de teste com apenas um DC, você pode criar uma chave raiz do KDS e definir a hora de início no passado para evitar a espera do intervalo para a geração de chave usando o procedimento a seguir. Valide que um evento 4004 foi registrado no log de eventos do KDS.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Para criar a chave raiz do KDS em um ambiente de teste e obter eficiência imediata

1.  No controlador de domínio do Windows Server 2012, execute o Windows PowerShell na barra de tarefas.

2.  No prompt de comando do módulo Active Directory do Windows PowerShell, digite os seguintes comandos e pressione ENTER:

    **$a=Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    Como opção, use um só comando

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Consulte também
[Introdução ao grupo de contas de serviço gerenciado](getting-started-with-group-managed-service-accounts.md)


