---
title: Ajuste de desempenho para HTTP 1.1/2
description: Recomendações de ajuste de desempenho para HTTP 1.1/2
ms.topic: article
ms.author: ivanpash; gmonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 362ceea398e13c5e537d1d828b86eec8b5d66a8f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896028"
---
# <a name="performance-tuning-http-112"></a>Ajuste de desempenho HTTP 1.1/2

O HTTP/2 destina-se a melhorar o desempenho no lado do cliente (por exemplo, o tempo de carregamento de página em um navegador). No servidor, ele pode representar um pequeno aumento no custo da CPU. Enquanto o servidor não requer mais uma única conexão TCP para cada solicitação, parte desse Estado agora será mantida na camada HTTP. Além disso, HTTP/2 tem compactação de cabeçalho, que representa carga adicional de CPU.

Algumas situações exigem um fallback de HTTP/1.1 (redefinindo a conexão HTTP/2 e, em vez disso, estabelecendo uma nova conexão para usar HTTP/1.1). Em particular, a renegociação de TLS e a autenticação HTTP (além de básica e Digest) exigem fallback de HTTP/1.1. Embora isso adicione sobrecarga, essas operações já implicam algum atraso e, portanto, não são particularmente sensíveis ao desempenho.

## <a name="additional-references"></a>Referências adicionais
- [Ajuste de desempenho do servidor Web](index.md)
- [Ajuste de desempenho do IIS 10.0](tuning-iis-10.md)