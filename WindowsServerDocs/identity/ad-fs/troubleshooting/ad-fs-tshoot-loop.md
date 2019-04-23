---
title: Solucionando problemas do AD FS - detecção de Loop
description: Este documento descreve como solucionar problemas de detecção de loop
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc8eeb11e44da3b8f26b1ab94143c189bca9ed38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830907"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>Solucionando problemas do AD FS - detecção de Loop 
 
Um loop no AD FS ocorre quando uma terceira parte confiável redireciona de volta para o AD FS e rejeita um token de segurança válido continuamente.

## <a name="loop-detection-cookie"></a>Cookie de detecção de loop
Para evitar que isso aconteça, o AD FS tiver implementado o que é chamado um cookie de detecção de loop. Por padrão, o AD FS grava um cookie aos clientes passivo do web denominados **MSISLoopDetectionCookie**. Esse cookie contém um valor de carimbo de hora e um número de tokens emitidos de valor.  Isso permite que o AD FS para controlar a frequência com que e como muitas vezes, um cliente visitou o serviço de Federação em um período específico.

Se um cliente passivo visitar o serviço de federação para um token de cinco (5) vezes dentro de 20 segundos, AD FS gera o seguinte erro:

**MSIS7042: A mesma sessão do navegador cliente fez '{0}'solicitações no último'{1}' segundos. Entre em contato com seu administrador para obter detalhes.**

Inserindo em loops infinitos geralmente é causado por um aplicativo que não esteja consumindo com êxito o token emitido pelo AD FS de terceira parte se comportando e o aplicativo está enviando o cliente passivo para o AD FS, repetidamente, para um novo token.  O AD FS será emitir o cliente passivo um novo token de cada vez, desde que eles não excedem 5 solicitações em até 20 segundos. 

## <a name="adjusting-the-loop-detection-cookie"></a>Ajustando o cookie de detecção de loop
Você pode usar o PowerShell para alterar o número de tokens emitidos de valor e o valor de timespan.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
O valor mínimo para **LoopDetectionMaximumTokensIssuedInterval** é 1.

O valor mínimo para **LoopDetectionTimeIntervalInSeconds** é 5.

Você também pode desabilitar a detecção de loops, quando você estiver realizando o teste de desempenho.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>É recomendável não desabilitar permanentemente a detecção de loops, pois isso impede que os usuários de entrar em loop infinito de estados.


## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)



