---
title: cipher
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d801d6e6286e97319766c879f7289f6191cc7101
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434329"
---
# <a name="cipher"></a>cipher



Exibe ou altera a criptografia de diretórios e pastas em volumes NTFS. Se usado sem parâmetros, **cipher** exibe o estado de criptografia do diretório atual e os arquivos nele contidos.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
cipher [/e | /d | /c] [/s:<Directory>] [/b] [/h] [PathName [...]]
cipher /k
cipher /r:<FileName> [/smartcard]
cipher /u [/n]
cipher /w:<Directory>
cipher /x[:efsfile] [FileName]
cipher /y
cipher /adduser [/certhash:<Hash> | /certfile:<FileName>] [/s:Directory] [/b] [/h] [PathName [...]]
cipher /removeuser /certhash:<Hash> [/s:<Directory>] [/b] [/h] [<PathName> [...]]
cipher /rekey [PathName [...]]
```

## <a name="parameters"></a>Parâmetros

|          Parâmetros           |                                                                                                                                                   Descrição                                                                                                                                                    |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /b               |                                                                                                    Será anulada se um erro for encontrado. Por padrão, **cipher** continua em execução, mesmo se forem encontrados erros.                                                                                                    |
|              /c               |                                                                                                                                   Exibe informações sobre o arquivo criptografado.                                                                                                                                    |
|              /d               |                                                                                                                                   Descriptografa os arquivos especificados ou diretórios.                                                                                                                                   |
|              /e               |                                                                                          Criptografa os arquivos especificados ou diretórios. Diretórios são marcados para que os arquivos que são adicionados posteriormente serão criptografados.                                                                                           |
|              /h               |                                                                                                     Exibe os arquivos com oculto ou atributos do sistema. Por padrão, esses arquivos não são criptografados ou descriptografados.                                                                                                     |
|              /k               |                                                                            Cria um novo certificado e a chave para uso com arquivos de sistema de arquivos com criptografia (EFS). Se o **/k** parâmetro for especificado, todos os outros parâmetros serão ignorados.                                                                            |
|  /r:\<FileName> [/smartcard]  |   Gera uma chave de agente de recuperação do EFS e o certificado e, em seguida, grava em um arquivo. pfx (contendo o certificado e chave privada) e um arquivo. cer (que contém apenas o certificado). Se **/smartcard** for especificado, ele grava a chave de recuperação e o certificado em um cartão inteligente, e nenhum arquivo. pfx será gerado.   |
|        /s:\<Directory>        |                                                                                                               Executa a operação especificada em todos os subdiretórios especificado na *Directory*.                                                                                                               |
|            /u [/n]            |  Localiza arquivos criptografados tudo nas unidades locais. Se usado com o **/n** parâmetro, não há atualizações são feitas. Se usado sem **/n**, **/u** compara a chave de criptografia de arquivo do usuário ou a chave do agente de recuperação para as mais atuais e atualiza-os se eles tiverem sido alteradas. Esse parâmetro funciona somente com **/n**.  |
|        /w:\<Directory>        | Remove dados de espaço em disco não utilizado disponível em todo o volume. Se você usar o **/w** parâmetro, todos os outros parâmetros serão ignorados. O diretório especificado pode ser localizado em qualquer lugar em um volume local. Se for uma montagem de ponto ou aponta para um diretório em outro volume, os dados em que o volume é removido. |
|  /x[:efsfile] [\<FileName>]   |                                 Faz backup de chaves e o certificado EFS para o nome de arquivo especificado. Se usado com **: efsfile**, **/x** faz backup de certificados do usuário que foram usados para criptografar o arquivo. Caso contrário, o certificado EFS atual do usuário e chaves são feitas backup.                                 |
|              /y               |                                                                                                                      Exibe sua miniatura atual do certificado EFS no computador local.                                                                                                                      |
|  /adduser [/ certhash:\<Hash >  |                                                                                                                                              /certfile:<FileName>]                                                                                                                                               |
|            /rekey             |                                                                                                                 Atualiza os arquivos criptografados especificados para usar a chave EFS configurada no momento.                                                                                                                 |
| /RemoveUser /certhash:\<hash > |                                                                                       Remove um usuário de arquivos especificados. O *Hash* fornecido para **/certhash** deve ser o hash SHA1 do certificado a ser removida.                                                                                       |
|              /?               |                                                                                                                                       Exibe a ajuda no prompt de comando.                                                                                                                                       |

## <a name="remarks"></a>Comentários

-   Se o diretório pai não estiver criptografado, um arquivo criptografado pode ser descriptografado quando ele é modificado. Portanto, quando você criptografa um arquivo, você também deve criptografar o diretório pai.
-   Um administrador pode adicionar o conteúdo de um arquivo. cer para a política de recuperação do EFS para criar o agente de recuperação para os usuários e, em seguida, importe o arquivo. pfx para recuperar arquivos individuais.
-   Você pode usar vários nomes de diretório e caracteres curinga.
-   Você deve colocar espaços entre vários parâmetros.

## <a name="BKMK_examples"></a>Exemplos

Para exibir o status de criptografia de cada um dos arquivos e subdiretórios no diretório atual, digite:
```
cipher
```
Arquivos criptografados e diretórios são marcados com um **eletrônico**. Diretórios e arquivos não criptografados são marcados com um **U**. Por exemplo, a saída a seguir indica que o diretório atual e todo seu conteúdo não é criptografado atualmente:
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```
Para habilitar a criptografia no diretório particular usado no exemplo anterior, digite:
```
cipher /e private
```
Exibe a saída a seguir:
```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```
O **cipher** comando exibe a saída a seguir:
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```
Observe que o diretório particular está marcado como criptografados.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)