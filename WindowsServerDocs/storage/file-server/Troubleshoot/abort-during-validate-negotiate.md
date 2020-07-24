---
title: A conexão TCP foi anulada durante a validação de negociação
description: Apresenta como solucionar o problema de SMB quando a conexão TCP é anulada durante a validação de negociação.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 762ad3b20c389771a9c0e6783d2a6fb1e73b8f0d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960998"
---
# <a name="tcp-connection-is-aborted-during-validate-negotiate"></a>A conexão TCP foi anulada durante a validação de negociação

No rastreamento de rede para o problema SMB, você percebe que uma anulação de redefinição de TCP ocorreu durante o processo de negociação de validação. Este artigo descreve como solucionar problemas da situação.

## <a name="cause"></a>Causa

Esse problema pode ser causado por uma falha na validação da negociação. Isso normalmente ocorre porque um acelerador de WAN modifica o pacote de negociação SMB original.

A Microsoft não permite mais modificação do pacote Validate Negotiate por qualquer motivo. Isso ocorre porque esse comportamento cria um sério risco de segurança.

Os seguintes requisitos se aplicam ao pacote Validate Negotiate:

- O processo Validate Negotiate usa o \_ comando fsctl Validate \_ Negotiate \_ info.

- A resposta de negociação de validação deve ser assinada. Caso contrário, a conexão será anulada.

- Você deve comparar as mensagens de informações de negociação de FSCTL \_ \_ \_ para as mensagens de negociação para garantir que nada tenha sido alterado.

## <a name="workaround"></a>Solução alternativa

Você pode desabilitar temporariamente o processo de negociação de validação. Para fazer isso, localize a seguinte subchave do registro:

**HKEY \_ local \_ Machine \\ System \\ CurrentControlSet \\ Services \\ LanmanWorkstation \\ Parameters**

Na chave **parâmetros** , defina **RequireSecureNegotiate** como **0**.

No Windows PowerShell, você pode executar o seguinte comando para definir esse valor:

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecureNegotiate -Value 0 -Force
```

> [!NOTE]
> O processo de negociação de validação não pode ser desabilitado no Windows 10, no Windows Server 2016 ou em versões posteriores do Windows.

Se o cliente ou o servidor não puder oferecer suporte ao comando Validate Negotiate, você poderá contornar esse problema Configurando a assinatura SMB a ser necessária. A assinatura SMB é considerada mais segura do que validar Negotiate. No entanto, também poderá haver degradação de desempenho se a assinatura for necessária.

## <a name="reference"></a>Referência

Para obter mais informações, confira os seguintes artigos:

[3.3.5.15.12 lidando com uma solicitação Validate Negotiate info](/openspecs/windows_protocols/ms-smb2/0b7803eb-d561-48a4-8654-327803f59ec6)

[3.2.5.14.12 manipulando uma resposta de negociar informações de negociação](/openspecs/windows_protocols/ms-smb2/6a5bc90d-3c08-4498-905b-e7dab30b2e0e)
