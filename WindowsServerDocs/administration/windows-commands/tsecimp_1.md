---
title: tsecimp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38582706dfa5db2b5069415b81dafc533c8a89b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822097"
---
# <a name="tsecimp"></a>tsecimp



Importa informações sobre a atribuição de um arquivo do Extensible Markup Language (XML) para o arquivo de segurança do servidor TAPI (Tsec). Você também pode usar esse comando para exibir a lista de provedores TAPI e os dispositivos de linhas associadas a cada um deles, validar a estrutura do arquivo XML sem importar o conteúdo e verificar a associação de domínio.

## <a name="syntax"></a>Sintaxe

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/f \<Filename >|Obrigatório. Especifica o nome do arquivo XML que contém as informações de atribuição que você deseja importar.|
|/v|Valida a estrutura do arquivo XML sem importar as informações para o arquivo Tsec.|
|/u|Verifica se cada usuário é um membro do domínio especificado no arquivo XML. O computador no qual você pode usar esse parâmetro deve estar conectado à rede. Esse parâmetro pode reduzir significativamente o desempenho se você estiver processando uma grande quantidade de informações de atribuição de usuário.|
|/d|Exibe uma lista de provedores de telefonia instalados. Para cada provedor de telefonia, são listados os dispositivos de linha associados, bem como os endereços e os usuários associados a cada dispositivo de linha.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O arquivo XML do qual você deseja importar as informações de atribuição deve seguir a estrutura descrita abaixo.  
    -   **UserList** elemento

        O **UserList** é o elemento superior do arquivo XML.
    -   **Usuário** elemento

        Cada **usuário** elemento contém informações sobre um usuário que seja um membro de um domínio. Um ou mais dispositivos de linha pode ser atribuído a cada usuário.

        Além disso, cada **usuário** elemento pode ter um atributo denominado **NoMerge**. Quando esse atributo for especificado, todas as atribuições de dispositivo de linha atual do usuário são removidas antes que novas sejam feitas. Você pode usar esse atributo para remover facilmente as atribuições de usuário indesejadas. Por padrão, esse atributo não está definido.

        O **usuário** elemento deve conter uma única **DomainUserName** elemento, que especifica o nome de usuário e domínio do usuário. O **usuário** elemento também pode conter um **FriendlyName** elemento, que especifica um nome amigável para o usuário.

        O **usuário** elemento pode conter um **Lista_de_linhas** elemento. Se um **Lista_de_linhas** elemento não estiver presente, todos os dispositivos de linha para este usuário são removidos.
    -   **Lista_de_linhas** elemento

        O **Lista_de_linhas** elemento contém informações sobre cada linha ou um dispositivo que pode ser atribuído ao usuário. Cada **Lista_de_linhas** elemento pode conter mais de um **linha** elemento.
    -   **Linha** elemento

        Cada **linha** elemento Especifica um dispositivo de linha. Você deve identificar cada dispositivo de linha, adicionando a um **endereço** elemento ou um **Id_permanente** sob o elemento a **linha** elemento.

        Para cada **linha** elemento, você pode definir o **remover** atributo. Se você definir esse atributo, o usuário não está atribuído a esse dispositivo de linha. Se esse atributo não for definido, o usuário obtém acesso ao dispositivo de linha. Nenhum erro será mostrado se o dispositivo de linha não está disponível para o usuário.

## <a name="examples"></a>Exemplos
-   Os seguintes segmentos de código XML de exemplo demonstram o uso correto dos elementos definidos acima.  
    -   O código a seguir remove todos os dispositivos de linha atribuídos a User1.  
        ```
        <UserList>
          <User NoMerge="1">
            <DomainUser>domain1\user1</DomainUser>
          </User>
        </UserList>
        ```  
    -   O código a seguir remove todos os dispositivos de linha atribuídos a User1 antes de atribuir uma linha com o endereço 99999. User1 não terá nenhum outro dispositivo de linhas, independentemente de se os dispositivos de linha foram atribuídos anteriormente.  
        ```
        <UserList>
          <User NoMerge="1">
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
    -   O código a seguir adiciona um dispositivo de linha para o Usuário1 sem excluir nenhum dispositivo de linha atribuído anteriormente.  
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
    -   O código a seguir adiciona o endereço de linha 99999 e remove o endereço de linha 88888 do acesso de User1.  
        ```
        <UserList>
          <User>
            <DomainUser>domain1\user1</DomainUser>
            <FriendlyName>User1</FriendlyName>
            <LineList>
              <Line>
                <Address>99999</Address>
              </Line>
              <Line Remove="1">
                <Address>88888</Address>
              </Line>
            </LineList>
          </User>
        </UserList>
        
        ```  
    -   O código a seguir adiciona o dispositivo permanente 1000 e remove a linha 88888 do acesso de User1.  
        ```
        <UserList>
          <User>
            <DomainUser>domain1\user1</DomainUser>
            <FriendlyName>User1</FriendlyName>
            <LineList>
              <Line>
                <PermanentID>1000</PermanentID>
              </Line>
              <Line Remove="1">
                <Address>88888</Address>
              </Line>
            </LineList>
          </User>
        </UserList>
        
        
        ```  
-   A saída de exemplo a seguir é exibida após o **/d** opção de linha de comando é especificada para exibir a configuração TAPI atual. Para cada provedor de telefonia, são listados os dispositivos de linha associados, bem como os endereços e os usuários associados a cada dispositivo de linha.  
    ```
    NDIS Proxy TAPI Service Provider
            Line: "WAN Miniport (L2TP)"
                    Permanent ID: 12345678910
    
    NDIS Proxy TAPI Service Provider
            Line: "LPT1DOMAIN1\User1"
                    Permanent ID: 12345678910
    
    Microsoft H.323 Telephony Service Provider
            Line: "H323 Line"
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32
    
    ```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Visão geral do shell de comando](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)