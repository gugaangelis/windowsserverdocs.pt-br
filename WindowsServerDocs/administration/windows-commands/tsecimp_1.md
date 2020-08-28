---
title: tsecimp
description: Artigo de referência para TSecImp, que importa informações de atribuição de um arquivo linguagem XML (XML) para o arquivo de segurança do servidor TAPI (Tsec.ini).
ms.topic: reference
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ad2f4f29537c2f575ba96d172c9405d9680187
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026864"
---
# <a name="tsecimp"></a>tsecimp

Importa informações de atribuição de um arquivo linguagem XML (XML) para o arquivo de segurança do servidor TAPI (Tsec.ini). Você também pode usar esse comando para exibir a lista de provedores TAPI e os dispositivos de linhas associados a cada um deles, validar a estrutura do arquivo XML sem importar o conteúdo e verificar a associação ao domínio.

## <a name="syntax"></a>Sintaxe

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/f \<Filename>|Obrigatórios. Especifica o nome do arquivo XML que contém as informações de atribuição que você deseja importar.|
|/v|Valida a estrutura do arquivo XML sem importar as informações para o arquivo de Tsec.ini.|
|/u|Verifica se cada usuário é um membro do domínio especificado no arquivo XML. O computador no qual você usa esse parâmetro deve estar conectado à rede. Esse parâmetro poderá reduzir significativamente o desempenho se você estiver processando uma grande quantidade de informações de atribuição de usuário.|
|/d|Exibe uma lista de provedores de telefonia instalados. Para cada provedor de telefonia, os dispositivos de linha associados são listados, bem como os endereços e usuários associados a cada dispositivo de linha.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O arquivo XML do qual você deseja importar informações de atribuição deve seguir a estrutura descrita abaixo.
    -   Elemento **UserList**

        O **UserList** é o elemento superior do arquivo XML.
    -   Elemento **User**

        Cada elemento de **usuário** contém informações sobre um usuário que é membro de um domínio. Cada usuário pode ser atribuído a um ou mais dispositivos de linha.

        Além disso, cada elemento de **usuário** pode ter um atributo chamado **nomerge**. Quando esse atributo é especificado, todas as atribuições de dispositivo de linha atual para o usuário são removidas antes que novas sejam feitas. Você pode usar esse atributo para remover facilmente as atribuições de usuário indesejadas. Por padrão, esse atributo não é definido.

        O elemento **User** deve conter um único elemento **DomainUserName** , que especifica o domínio e o nome de usuário do usuário. O elemento **User** também pode conter um elemento **FriendlyName** , que especifica um nome amigável para o usuário.

        O elemento **User** pode conter um elemento **LineList** . Se um elemento **LineList** não estiver presente, todos os dispositivos de linha para esse usuário serão removidos.
    -   Elemento **LineList**

        O elemento **LineList** contém informações sobre cada linha ou dispositivo que pode ser atribuído ao usuário. Cada elemento **LineList** pode conter mais de um elemento de **linha** .
    -   Elemento de **linha**

        Cada elemento de **linha** especifica um dispositivo de linha. Você deve identificar cada dispositivo de linha adicionando um elemento **Address** ou um elemento **permanente** no elemento **line** .

        Para cada elemento de **linha** , você pode definir o atributo **Remove** . Se você definir esse atributo, o usuário não será mais atribuído a esse dispositivo de linha. Se esse atributo não estiver definido, o usuário obtém acesso a esse dispositivo de linha. Nenhum erro será fornecido se o dispositivo de linha não estiver disponível para o usuário.

## <a name="examples"></a>Exemplos
- Os seguintes segmentos de código XML de exemplo ilustram o uso correto dos elementos definidos acima.
  - O código a seguir remove todos os dispositivos de linha atribuídos ao Usuário1.
    ```
    <UserList>
      <User NoMerge=1>
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```
  - O código a seguir remove todos os dispositivos de linha atribuídos ao Usuário1 antes de atribuir uma linha com o endereço 99999. O Usuário1 não terá nenhum outro dispositivo de linhas atribuído, independentemente de os dispositivos de linha terem sido atribuídos anteriormente.
    ```
    <UserList>
      <User NoMerge=1>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```
  - O código a seguir adiciona um dispositivo de linha para user1 sem excluir nenhum dispositivo de linha atribuído anteriormente.
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```
  - O código a seguir adiciona o endereço de linha 99999 e remove o endereço de linha 88888 do acesso User1's.
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
          <Line Remove=1>
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```
  - O código a seguir adiciona o dispositivo permanente 1000 e remove a linha 88888 do acesso User1's.
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <PermanentID>1000</PermanentID>
          </Line>
          <Line Remove=1>
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   A saída de exemplo a seguir aparece depois que a opção de linha de comando **/d** é especificada para exibir a configuração atual da TAPI. Para cada provedor de telefonia, os dispositivos de linha associados são listados, bem como os endereços e usuários associados a cada dispositivo de linha.
    ```
    NDIS Proxy TAPI Service Provider
            Line: WAN Miniport (L2TP)
                    Permanent ID: 12345678910

    NDIS Proxy TAPI Service Provider
            Line: LPT1DOMAIN1\User1
                    Permanent ID: 12345678910

    Microsoft H.323 Telephony Service Provider
            Line: H323 Line
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32

    ```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Visão geral do Shell de comando](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))
