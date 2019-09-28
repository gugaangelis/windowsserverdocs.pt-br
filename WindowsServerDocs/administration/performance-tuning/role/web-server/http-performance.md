---
title: Ajuste de desempenho para HTTP 1.1/2
description: Recomendações de ajuste de desempenho para HTTP 1.1/2
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f7d7bd5145a0804b9ec86438602dfed7c75a2b02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384973"
---
# <a name="performance-tuning-http-112"></a>Ajuste de desempenho HTTP 1.1/2

O HTTP/2 destina-se a melhorar o desempenho no lado do cliente (por exemplo, o tempo de carregamento de página em um navegador). No servidor, ele pode representar um pequeno aumento no custo da CPU. Enquanto o servidor não requer mais uma única conexão TCP para cada solicitação, parte desse Estado agora será mantida na camada HTTP. Além disso, HTTP/2 tem compactação de cabeçalho, que representa carga adicional de CPU.

Algumas situações exigem um fallback de HTTP/1.1 (redefinindo a conexão HTTP/2 e, em vez disso, estabelecendo uma nova conexão para usar HTTP/1.1). Em particular, a renegociação de TLS e a autenticação HTTP (além de básica e Digest) exigem fallback de HTTP/1.1. Embora isso adicione sobrecarga, essas operações já implicam algum atraso e, portanto, não são particularmente sensíveis ao desempenho.

## <a name="see-also"></a>Consulte também
- [Ajuste de desempenho do servidor Web](index.md) 
- [Ajuste de desempenho do IIS 10.0](tuning-iis-10.md)