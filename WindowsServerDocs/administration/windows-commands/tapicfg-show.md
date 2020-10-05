---
title: Tapicfg mostrar
description: Artigo de referência do comando Tapicfg show, que exibe os nomes e locais das partições de diretório do aplicativo TAPI no domínio.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/29/2020
ms.openlocfilehash: b39021a1e7c76ee8db359b33f032bac11225e85c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729802"
---
# <a name="tapicfg-show"></a>Tapicfg mostrar

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe os nomes e locais das partições de diretório do aplicativo TAPI no domínio.

## <a name="syntax"></a>Sintaxe

```
tapicfg show [/defaultonly] [/domain:<domainname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /default somente | Exibe os nomes e locais apenas da partição de diretório de aplicativo TAPI padrão no domínio. |
| /Domain `<domainname>` | Especifica o nome DNS do domínio para o qual as partições de diretório do aplicativo TAPI são exibidas. Se o nome de domínio não for especificado, o nome do domínio local será usado. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Essa ferramenta de linha de comando pode ser executada em qualquer computador que seja membro do domínio.

- O texto fornecido pelo usuário (como os nomes das partições de diretório do aplicativo TAPI, servidores e domínios) com caracteres internacionais ou Unicode só será exibido corretamente se as fontes apropriadas e o suporte a idioma estiverem instalados.

- Você ainda pode usar servidores ILS (serviço de localização da Internet) em sua organização, se o ILS for necessário para dar suporte a determinados aplicativos, porque os clientes TAPI que executam o Windows XP ou um sistema operacional Windows Server 2003 podem consultar servidores ILS ou partições de diretório de aplicativos TAPI.

- Você pode usar **Tapicfg** para criar ou remover pontos de conexão de serviço. Se a partição de diretório do aplicativo TAPI for renomeada por qualquer motivo (por exemplo, se você renomear o domínio no qual ele reside), será necessário remover o ponto de conexão de serviço existente e criar um novo que contenha o novo nome DNS da partição de diretório do aplicativo TAPI a ser publicada. Caso contrário, os clientes TAPI não poderão localizar e acessar a partição de diretório de aplicativos TAPI. Você também pode remover um ponto de conexão de serviço para fins de manutenção ou segurança (por exemplo, se não quiser expor dados TAPI em uma partição de diretório de aplicativo TAPI específica).

## <a name="example"></a>Exemplo

Para exibir o nome da partição de diretório de aplicativo TAPI padrão para o novo domínio, digite:

```
tapicfg show /defaultonly
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [instalação de Tapicfg](tapicfg-install.md)

- [remover Tapicfg](tapicfg-remove.md)

- [Tapicfg publishscp](tapicfg-publishscp.md)

- [Tapicfg removescp](tapicfg-removescp.md)

- [Tapicfg MakeDefault](tapicfg-makedefault.md)
