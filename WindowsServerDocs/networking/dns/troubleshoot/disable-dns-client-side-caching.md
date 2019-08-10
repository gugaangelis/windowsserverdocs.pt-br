---
title: Desabilitar o cache do lado do cliente DNS em clientes DNS
description: Este artigo apresenta como desabilitar o cache do lado do cliente DNS em clientes DNS.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 0b20400029b462798587c2291431b5a7c3d61775
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917803"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>Desabilitar o cache do lado do cliente DNS em clientes DNS

O Windows contém um cache DNS do lado do cliente. O recurso de cache DNS do lado do cliente pode gerar uma falsa impressão de que o balanceamento de carga do DNS "round robin" não está ocorrendo do servidor DNS para o computador cliente do Windows. Quando você usa o comando ping para pesquisar o mesmo nome de domínio de registro a, o cliente pode usar o mesmo endereço IP.  

## <a name="how-to-disable-client-side-caching"></a>Como desabilitar o armazenamento em cache do lado do cliente

Para parar o cache DNS, execute um dos seguintes comandos:

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


Para desabilitar o cache DNS permanentemente no Windows, use a ferramenta do controlador de serviço ou a ferramenta serviços para definir o tipo de inicialização doserviço cliente DNS como desabilitado. Observe que o nome do serviço cliente DNS do Windows também pode aparecer como "dnscache". 

> [!NOTE]
> Se o cache do resolvedor de DNS for desativado, o desempenho geral do computador cliente diminuirá e o tráfego de rede para consultas DNS aumentará. 

O serviço cliente DNS otimiza o desempenho da resolução de nomes DNS armazenando nomes anteriormente resolvidos na memória. Se o serviço cliente DNS estiver desativado, o computador ainda poderá resolver nomes DNS usando os servidores DNS da rede. 

Quando o resolvedor do Windows recebe uma resposta, positiva ou negativa, para uma consulta, ele adiciona essa resposta ao seu cache e, portanto, cria um registro de recurso DNS. O resolvedor sempre verifica o cache antes de consultar qualquer servidor DNS. Se um registro de recurso DNS estiver no cache, o resolvedor usará o registro do cache em vez de consultar um servidor. Esse comportamento acelera as consultas e diminui o tráfego de rede para consultas DNS. 

Você pode usar a ferramenta ipconfig para exibir e liberar o cache do resolvedor de DNS. Para exibir o cache do resolvedor de DNS, execute o seguinte comando no prompt de comando:

```cmd
ipconfig /displaydns 
```

Esse comando exibe o conteúdo do cache do resolvedor de DNS, incluindo os registros de recursos de DNS pré-carregados do arquivo de hosts e quaisquer nomes consultados recentemente que foram resolvidos pelo sistema. Depois de algum tempo, o resolvedor descarta o registro do cache. O período de tempo é especificado pelo valor **TTL (vida útil)** associado ao registro de recurso DNS. Você também pode liberar o cache manualmente. Depois de liberar o cache, o computador deve consultar os servidores DNS novamente para todos os registros de recursos DNS que foram resolvidos anteriormente pelo computador. Para excluir as entradas no cache do resolvedor de DNS `ipconfig /flushdns` , execute em um prompt de comando.

## <a name="next-step"></a>Próximas etapas

Consulte [como desabilitar o cache DNS do lado do cliente no Windows](https://support.microsoft.com/kb/318803) para obter mais informações.
