---
title: Ajuste de desempenho para HTTP 1.1/2
description: Recomendações de ajuste de desempenho para HTTP 1.1/2
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: ivanpash; gmonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5e62f7428f015193896aba5c7d9c146bd11e7225
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851679"
---
# <a name="performance-tuning-http-112"></a>Ajuste de desempenho HTTP 1.1/2

O HTTP/2 destina-se a melhorar o desempenho no lado do cliente (por exemplo, o tempo de carregamento de página em um navegador). No servidor, ele pode representar um pequeno aumento no custo da CPU. Enquanto o servidor não requer mais uma única conexão TCP para cada solicitação, parte desse Estado agora será mantida na camada HTTP. Além disso, HTTP/2 tem compactação de cabeçalho, que representa carga adicional de CPU.

Algumas situações exigem um fallback de HTTP/1.1 (redefinindo a conexão HTTP/2 e, em vez disso, estabelecendo uma nova conexão para usar HTTP/1.1). Em particular, a renegociação de TLS e a autenticação HTTP (além de básica e Digest) exigem fallback de HTTP/1.1. Embora isso adicione sobrecarga, essas operações já implicam algum atraso e, portanto, não são particularmente sensíveis ao desempenho.

## <a name="see-also"></a>Consulte também
- [Ajuste de desempenho do servidor Web](index.md) 
- [Ajuste de desempenho do IIS 10.0](tuning-iis-10.md)