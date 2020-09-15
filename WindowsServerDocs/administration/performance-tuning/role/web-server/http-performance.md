---
title: Ajuste de desempenho para HTTP 1.1/2
description: Recomendações de ajuste de desempenho para HTTP 1.1/2
ms.topic: article
ms.author: ivanpash
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 853c71aea99d296c89f772b0ddba03ed024e385a
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078073"
---
# <a name="performance-tuning-http-112"></a>Ajuste de desempenho HTTP 1.1/2

O HTTP/2 destina-se a melhorar o desempenho no lado do cliente (por exemplo, o tempo de carregamento de página em um navegador). No servidor, ele pode representar um pequeno aumento no custo da CPU. Enquanto o servidor não requer mais uma única conexão TCP para cada solicitação, parte desse Estado agora será mantida na camada HTTP. Além disso, HTTP/2 tem compactação de cabeçalho, que representa carga adicional de CPU.

Algumas situações exigem um fallback de HTTP/1.1 (redefinindo a conexão HTTP/2 e, em vez disso, estabelecendo uma nova conexão para usar HTTP/1.1). Em particular, a renegociação de TLS e a autenticação HTTP (além de básica e Digest) exigem fallback de HTTP/1.1. Embora isso adicione sobrecarga, essas operações já implicam algum atraso e, portanto, não são particularmente sensíveis ao desempenho.

## <a name="additional-references"></a>Referências adicionais
- [Ajuste de desempenho do servidor Web](index.md)
- [Ajuste de desempenho do IIS 10.0](tuning-iis-10.md)