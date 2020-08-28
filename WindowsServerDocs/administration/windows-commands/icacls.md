---
title: icacls
description: Artigo de referência para o comando icacls, que exibe ou modifica listas de controle de acesso discricional (DACL) em arquivos especificados, e aplica as DACLs armazenadas a arquivos em diretórios especificados.
ms.topic: reference
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 7b6d045b26adcbee31447e950533b1013288a910
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038014"
---
# <a name="icacls"></a>icacls

Exibe ou modifica DACLs (listas de controle de acesso condicional) em arquivos especificados e aplica a DACLs armazenadas a arquivos nos diretórios especificados.

> [!NOTE]
> Esse comando substitui o [comando cacls](cacls.md)preterido.

## <a name="syntax"></a>Sintaxe

```
icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]
icacls <directory> [/substitute <sidold> <sidnew> [...]] [/restore <aclfile> [/c] [/l] [/q]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<filename>` | Especifica o arquivo para o qual exibir as DACLs. |
| `<directory>` | Especifica o diretório para o qual exibir as DACLs. |
| /t | Executa a operação em todos os arquivos especificados no diretório atual e em seus subdiretórios. |
| /c | Continua a operação, apesar de quaisquer erros de arquivo. As mensagens de erro ainda serão exibidas. |
| /l | Executa a operação em um link simbólico em vez de seu destino. |
| /q | Suprime as mensagens de êxito. |
| [/Save `<ACLfile>` /t /c /l [/q]] | Armazena as DACLs de todos os arquivos correspondentes no *ACLfile* para uso posterior com **/Restore**. |
| [/SetOwner `<username>` /t /c /l [/q]] | Altera o proprietário de todos os arquivos correspondentes para o usuário especificado. |
| [/findsid `<sid>` /t /c /l [/q]] | Localiza todos os arquivos correspondentes que contêm uma DACL mencionando explicitamente o SID (identificador de segurança) especificado. |
| [/Verify [/t] [/c] [/l] [/q]] | Localiza todos os arquivos com ACLs que não são canônicas ou têm tamanhos inconsistentes com as contagens de ACE (entrada de controle de acesso). |
| [/Reset reiniciar [/t] [/c] [/l] [/q]] | Substitui ACLs por ACLs herdadas padrão para todos os arquivos correspondentes. |
| [/Grant [: r] \<sid> : <perm> [...]] | Concede direitos de acesso de usuário especificado. As permissões substituem as permissões explícitas concedidas anteriormente.<p>Não adicionar o **: r**, significa que as permissões são adicionadas a quaisquer permissões explícitas concedidas anteriormente. |
| [/Deny \<sid> : <perm> [...]] | Nega explicitamente os direitos de acesso de usuário especificados. Uma ACE de negação explícita é adicionada para as permissões declaradas e as mesmas permissões em qualquer concessão explícita são removidas. |
| [/remove `[:g | :d]]` `<sid>` [...] /t /c /l /q | Remove todas as ocorrências do SID especificado da DACL. Esse comando também pode usar:<ul><li>**: g** -remove todas as ocorrências de direitos concedidos para o SID especificado.</li><li>**:d** -remove todas as ocorrências de direitos negados para o SID especificado. |
| [/setintegritylevel [(CI) (OI)] `<Level>:<Policy>` [...]] | Adiciona explicitamente uma ACE de integridade a todos os arquivos correspondentes. O nível pode ser especificado como:<ul><li>**l** -baixo</li><li>**m**-médio</li><li>**h** -alta</li></ul>As opções de herança para a ACE de integridade podem preceder o nível e são aplicadas somente a diretórios. |
| [/substitute `<sidold> <sidnew>` [...]] | Substitui um SID existente (*sidold*) por um novo SID (*sidnew*). Requer o uso do com o `<directory>` parâmetro. |
| /Restore `<ACLfile>` [/c] [/l] [/q] | Aplica as DACLs armazenadas de `<ACLfile>` em arquivos no diretório especificado. Requer o uso do com o `<directory>` parâmetro. |
| /inheritancelevel:`[e | d | r]` | Define o nível de herança, que pode ser:<ul><li>**e** -habilita a herança</li><li>**d** -desabilita a herança e copia as ACEs</li><li>**r** -remove todas as ACEs herdadas</li></ul> |

## <a name="remarks"></a>Comentários

- Os SIDs podem estar na forma de nome numérico ou amigável. Se você usar um formato numérico, afixará o caractere curinga **&#42;** ao início do Sid.

- Esse comando preserva a ordem canônica das entradas ACE como:

    - Negações explícitas

    -  Concessões explícitas

    - Negações herdadas

    - Concessões herdadas

- A `<perm>` opção é uma máscara de permissão que pode ser especificada em uma das seguintes formas:

    - Uma sequência de direitos simples:

      - **F** -acesso completo

      - **M**-modificar o acesso

      - **RX** -acesso de leitura e execução

      - **R** -acesso somente leitura

      - **W** -acesso somente gravação

    - Uma lista separada por vírgulas em parênteses de direitos específicos:

      - **D** -excluir

      - **RC** -controle de leitura

      - **WDAC** -gravar DAC

      - **Wo** -proprietário da gravação

      - **S** -sincronizar

      - Segurança do sistema do **as** -Access

      - **Ma** -máximo permitido

      - **Gr** -leitura genérica

      - **GW** -gravação genérica

      - **GE** -execução genérica

      - **GA** -tudo genérico

      - **RD** – ler diretório de dados/lista

      - **WD** -gravar dados/Adicionar arquivo

      - **Ad** -acrescentar dados/adicionar subdiretório

      - **Rea** -atributos estendidos de leitura

      - **Tem** -gravar atributos estendidos

      - **X** -executar/percorrer

      - **DC** -excluir filho

      - **Ra** -ler atributos

      - **Wa** -gravar atributos

  - Os direitos de herança podem preceder qualquer `<perm>` forma e são aplicados somente a diretórios:

      - **(Oi)** -herança de objeto

      - **(CI)** -herança de contêiner

      - **(E/s)** -herdar somente

      - **(NP)** – não propagar herança

## <a name="examples"></a>Exemplos

Para salvar as DACLs de todos os arquivos no diretório C:\Windows e seus subdiretórios no arquivo ACLFile, digite:

```
icacls c:\windows\* /save aclfile /t
```

Para restaurar as DACLs para cada arquivo em ACLFile que existe no diretório C:\Windows e em seus subdiretórios, digite:

```
icacls c:\windows\ /restore aclfile
```

Para conceder ao usuário usuário1 excluir e gravar permissões de DAC em um arquivo chamado Test1, digite:

```
icacls test1 /grant User1:(d,wdac)
```

Para conceder ao usuário definido por SID S-1-1-0 excluir e gravar permissões de DAC em um arquivo, chamado test2, digite:

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
