---
title: tapicfg
description: Artigo de referência para os comandos Tapicfg, que cria, remove ou exibe uma partição de diretório de aplicativo TAPI ou define uma partição de diretório de aplicativo TAPI padrão.
ms.topic: reference
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 17b596036251d6ea8588de3b70359ea161b67c58
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718123"
---
# <a name="tapicfg"></a>tapicfg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, remove ou exibe uma partição de diretório de aplicativo TAPI ou define uma partição de diretório de aplicativo TAPI padrão. Os clientes TAPI 3,1 podem usar as informações nessa partição de diretório de aplicativo com o serviço localizador de serviço de diretório para localizar e se comunicar com diretórios TAPI. Você também pode usar **Tapicfg** para criar ou remover pontos de conexão de serviço, que permitem que clientes TAPI localizem partições de diretório de aplicativos TAPI com eficiência em um domínio.

Essa ferramenta de linha de comando pode ser executada em qualquer computador que seja membro do domínio.

## <a name="syntax"></a>Sintaxe

```
tapicfg install
tapicfg remove
tapicfg publishscp
tapicfg removescp
tapicfg show
tapicfg makedefault
```

### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
|--|--|
| [instalação de Tapicfg](tapicfg-install.md) | Cria uma partição de diretório de aplicativos TAPI. |
| [remover Tapicfg](tapicfg-remove.md) | Remove uma partição de diretório de aplicativos TAPI.|
| [Tapicfg publishscp](tapicfg-publishscp.md) | Cria um ponto de conexão de serviço para publicar uma partição de diretório de aplicativo TAPI. |
| [Tapicfg removescp](tapicfg-removescp.md) | Remove um ponto de conexão de serviço para uma partição de diretório de aplicativos TAPI. |
| [Tapicfg mostrar](tapicfg-show.md) | Exibe os nomes e locais das partições de diretório do aplicativo TAPI no domínio. |
| [Tapicfg MakeDefault](tapicfg-makedefault.md) | Define a partição de diretório de aplicativo TAPI padrão para o domínio. |

#### <a name="remarks"></a>Comentários

- Você deve ser membro do grupo **Administradores de empresa** em Active Directory para executar o **Tapicfg install** (para criar uma partição de diretório de aplicativo TAPI) ou **Tapicfg remove** (para remover uma partição de diretório de aplicativo TAPI).

- O texto fornecido pelo usuário (como os nomes das partições de diretório do aplicativo TAPI, servidores e domínios) com caracteres internacionais ou Unicode só será exibido corretamente se as fontes apropriadas e o suporte a idioma estiverem instalados.

- Você ainda pode usar servidores ILS (serviço de localização da Internet) em sua organização, se o ILS for necessário para dar suporte a determinados aplicativos, porque os clientes TAPI que executam o Windows XP ou um sistema operacional Windows Server 2003 podem consultar servidores ILS ou partições de diretório de aplicativos TAPI.

- Você pode usar **Tapicfg** para criar ou remover pontos de conexão de serviço. Se a partição de diretório do aplicativo TAPI for renomeada por qualquer motivo (por exemplo, se você renomear o domínio no qual ele reside), será necessário remover o ponto de conexão de serviço existente e criar um novo que contenha o novo nome DNS da partição de diretório do aplicativo TAPI a ser publicada. Caso contrário, os clientes TAPI não poderão localizar e acessar a partição de diretório de aplicativos TAPI. Você também pode remover um ponto de conexão de serviço para fins de manutenção ou segurança (por exemplo, se não quiser expor dados TAPI em uma partição de diretório de aplicativo TAPI específica).

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [instalação de Tapicfg](tapicfg-install.md)

- [remover Tapicfg](tapicfg-remove.md)

- [Tapicfg publishscp](tapicfg-publishscp.md)

- [Tapicfg removescp](tapicfg-removescp.md)

- [Tapicfg mostrar](tapicfg-show.md)

- [Tapicfg MakeDefault](tapicfg-makedefault.md)
