---
title: tftp
description: Artigo de referência para o comando TFTP, que transfere arquivos de e para um computador remoto.
ms.topic: reference
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 89a4edb69753ae34814d13f90f21332c6f748244
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717903"
---
# <a name="tftp"></a>tftp

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfere arquivos de e para um computador remoto, normalmente um computador executando UNIX, que executa o serviço ou daemon trivial protocolo FTP (TFTP). o TFTP normalmente é usado por dispositivos ou sistemas inseridos que recuperam firmware, informações de configuração ou uma imagem do sistema durante o processo de inicialização de um servidor TFTP.

> FUNDAMENTAL O protocolo TFTP não dá suporte a nenhum mecanismo de autenticação ou criptografia e, como tal, pode introduzir um risco de segurança quando presente. A instalação do cliente tftp não é recomendada para sistemas conectados à Internet. Um serviço de servidor TFTP não é mais fornecido pela Microsoft por motivos de segurança.

## <a name="syntax"></a>Sintaxe

```
tftp [-i] [<host>] [{get | put}] <source> [<destination>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -i | Especifica o modo de transferência de imagem binária (também chamado de modo de octeto). No modo de imagem binária, o arquivo é transferido em unidades de um byte. Use esse modo ao transferir arquivos binários. Se você não usar a opção **-i** , o arquivo será transferido no modo ASCII. Esse é o modo de transferência padrão. Esse modo converte os caracteres de fim de linha (EOL) em um formato apropriado para o computador especificado. Use esse modo ao transferir arquivos de texto. Se uma transferência de arquivo for bem-sucedida, a taxa de transferência de dados será exibida. |
| `<host>` | Especifica o computador local ou remoto. |
| get | Transfere o *destino* do arquivo no computador remoto para a *origem* do arquivo no computador local. |
| put | Transfere a *origem* do arquivo no computador local para o *destino* do arquivo no computador remoto. Como o protocolo TFTP não dá suporte à autenticação de usuário, o usuário deve estar conectado ao computador remoto e os arquivos devem ser graváveis no computador remoto. |
| `<source>` | Especifica o arquivo a ser transferido. |
| `<destination>` | Especifica para onde transferir o arquivo. |

## <a name="examples"></a>Exemplos

Para copiar o arquivo *boot. img* do computador remoto *Host1*, digite:

```
tftp  -i Host1 get boot.img
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
