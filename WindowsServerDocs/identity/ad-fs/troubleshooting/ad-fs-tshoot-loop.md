---
title: Solução de problemas AD FS detecção de loop
description: Este documento descreve como solucionar problemas de detecção de loop
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: e3d654f44ba75d9b647c0c1d9db7345c7ea75435
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953086"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>Solução de problemas AD FS detecção de loop

O loop em AD FS ocorre quando uma terceira parte confiável rejeita continuamente um token de segurança válido e redireciona de volta para AD FS.

## <a name="loop-detection-cookie"></a>Cookie de detecção de loop
Para evitar que isso aconteça, AD FS implementou o que é chamado de cookie de detecção de loop. Por padrão, AD FS grava um cookie para clientes Web passivos chamados **MSISLoopDetectionCookie**. Esse cookie contém um valor de carimbo de data/hora e um número de tokens emitidos.  Isso permite que AD FS controle a frequência e quantas vezes um cliente visitou o Serviço de Federação dentro de um período de tempo específico.

Se um cliente passivo visitar a Serviço de Federação por um token cinco (5) vezes em 20 segundos, AD FS lançará o seguinte erro:

**MSIS7042: a mesma sessão do navegador do cliente fez {0} solicitações ' ' nos últimos ' {1} ' segundos. Contate o administrador para obter detalhes.**

A entrada em loops infinitos geralmente é causada por um aplicativo de terceira parte confiável de comportamento inadequado que não consome com êxito o token emitido por AD FS, e o aplicativo está enviando o cliente passivo de volta para AD FS, repetidamente, para um novo token.  AD FS irá emitir o cliente passivo um novo token a cada vez, desde que ele não exceda 5 solicitações dentro de 20 segundos.

## <a name="adjusting-the-loop-detection-cookie"></a>Ajustando o cookie de detecção de loop
Você pode usar o PowerShell para alterar o número de tokens emitidos e o valor de TimeSpan.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
O valor mínimo para **LoopDetectionMaximumTokensIssuedInterval** é 1.

O valor mínimo para **LoopDetectionTimeIntervalInSeconds** é 5.

Você também pode desabilitar a detecção de loop quando estiver fazendo testes de desempenho.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>Não é recomendável desabilitar permanentemente a detecção de loops, pois isso impede que os usuários entrem em Estados de loop infinito.


## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)



