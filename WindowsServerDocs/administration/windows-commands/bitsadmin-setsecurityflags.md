---
title: bitsadmin setsecurityflags
description: O tópico de comandos do Windows para **Bitsadmin setsecurityflags**, que define sinalizadores de segurança para http para determinar se o bits deve verificar a lista de certificados revogados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d73361bceda8c0eb24992bdee176b47bf82a878
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122722"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

Define sinalizadores de segurança para HTTP para determinar se o BITS deve verificar a lista de certificados revogados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP. O valor é um inteiro sem sinal.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setsecurityflags <job> <value>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| {1&gt;Valor&lt;1} | Pode incluir um ou mais dos seguintes sinalizadores de notificação, incluindo:<ul><li>Defina o bit menos significativo para habilitar a verificação de CRL.</li><li>Defina o 2º bit da direita para ignorar nomes comuns incorretos no certificado do servidor.</li><li>Defina o terceiro bit da direita para ignorar as datas incorretas no certificado do servidor.</li><li>Defina o 4º bit da direita para ignorar autoridades de certificação incorretas no certificado do servidor.</li><li>Defina o quinto bit da direita para ignorar o uso incorreto do certificado do servidor.</li><li>Defina o nono através dos bits 11 do lado direito para implementar sua política de redirecionamento especificada, incluindo:<ul><li>**0, 0, 0.** Os redirecionamentos são permitidos automaticamente.</li><li>**0, 0, 1.** O nome remoto na interface **IBackgroundCopyFile** será atualizado se ocorrer um redirecionamento.</li><li>**0, 1, 0.** O BITS falhará no trabalho se ocorrer um redirecionamento.</li></ul></li><li>Defina o 12 bits à direita para permitir o redirecionamento de HTTPS para HTTP.</li></ul> |

## <a name="examples"></a>Exemplos

O exemplo a seguir define os sinalizadores de segurança para habilitar uma verificação de CRL para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /setsecurityflags myDownloadJob 0x0001
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)