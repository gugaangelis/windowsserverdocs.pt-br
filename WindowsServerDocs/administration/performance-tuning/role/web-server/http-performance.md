---
title: Ajuste de desempenho para o HTTP 1.1/2
description: Para o HTTP 1.1/2 de recomendações de ajuste de desempenho
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bf85efa88e377966135c23a548119f19c39cceba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866367"
---
# <a name="performance-tuning-http-112"></a>O HTTP 1.1/2 de ajuste de desempenho

HTTP/2 destina-se para melhorar o desempenho no lado do cliente (por exemplo, tempo de carregamento página em um navegador). No servidor, ele pode representar um ligeiro aumento no custo de CPU. Enquanto o servidor não requer uma única conexão de TCP para cada solicitação, alguns dos que o estado agora serão mantidos na camada de HTTP. Além disso, o HTTP/2 tem compactação de cabeçalho, que representa a carga de CPU adicional.

Algumas situações exijam um fallback (Redefinir a conexão HTTP/2 e em vez disso, o estabelecimento de uma nova conexão para usar o HTTP/1.1) de HTTP/1.1. Em particular, renegociação TLS e autenticação de HTTP (que não seja básica e Digest) exigem HTTP/1.1 fallback. Embora isso adiciona sobrecarga, essas operações já implicam algum atraso e portanto, não são especialmente sensíveis ao desempenho.

## <a name="see-also"></a>Consulte também
- [Ajuste de desempenho do servidor de Web](index.md) 
- [Ajuste de desempenho do IIS 10.0](tuning-iis-10.md)