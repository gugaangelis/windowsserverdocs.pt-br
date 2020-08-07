---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Como URIs são usados no AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 25f017da1d450484110dd43b3bc8cb25cbe06afb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937943"
---
# <a name="how-uris-are-used-in-ad-fs"></a>Como URIs são usados no AD FS
Um \( URI \) de Uniform Resource Identifier é uma cadeia de caracteres que é usada como um identificador exclusivo.  No AD FS, os URIs são usados para identificar os endereços de rede do parceiro e objetos de configuração.  Quando usado para identificar os endereços de rede do parceiro, o URI é sempre uma URL.  Quando usado para identificar objetos de configuração, o URI pode ser uma URL ou um URN.  Para obter mais informações sobre URIs, consulte [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) e [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).

## <a name="uris-as-partner-network-addresses"></a>URIs como endereços de rede do parceiro
A seguir estão as URLs de endereço de rede mais manipuladas pelos administradores do AD FS.

-   As URLs do Serviço de Federação, incluindo WS \- Federation, SAML, WS \- Trust, metadados de Federação, WS \- MetadataExchange, privacidade e URLs da organização

-   As URLs de uma relação de confiança de terceira parte confiável, incluindo WS \- Federation, SAML e URLs de metadados de Federação

-   As URLs de uma confiança do provedor de declarações, incluindo URLs do WS \- Federation, SAML e Federation Metadata

## <a name="uris-as-object-identifiers"></a>URIs como identificadores de objeto
A tabela a seguir descreve os identificadores mais manipulados pelos administradores do AD FS.

|Nome do identificador|Descrição|Comparações|
|-------------------|---------------|---------------|
|Identificador do Serviço de Federação|Esse identificador é usado para identificar o Serviço de Federação.  Ele é usado pelas partes confiáveis que usam declarações deste Serviço de Federação, bem como provedores de declarações que emitem declarações para esse Serviço de Federação.|Quando um usuário solicitar declarações de um provedor de declarações para esse serviço de federação, o identificador do Serviço de Federação será usado para identificar o destino para as declarações.<p>Quando esse Serviço de Federação receber as declarações de um provedor de declarações, ele verificará para garantir que as declarações tenham o escopo relevante procurando pelo seu identificador de Serviço de Federação.<p>Quando uma terceira parte confiável estiver recebendo declarações desse serviço de federação, a terceira parte confiável verificará se o emissor das declarações corresponde ao identificador do Serviço de Federação.|
|Identificador terceira parte confiável|Esse identificador é usado para identificar a terceira parte confiável para esse Serviço de Federação.  Ele é usado ao emitir declarações à terceira parte confiável.|Quando um usuário solicitar declarações desse serviço de federação à terceira parte confiável, o identificador da terceira parte será usado para identificar a terceira parte confiável para a qual as declarações devem ser direcionadas.  Essa comparação é feita usando a correspondência de prefixo, \( Veja abaixo \) .<p>Ao receber as declarações, a terceira parte confiável verifica seu identificador no token de segurança para garantir que as declarações sejam direcionadas a ela.|
|Identificador do provedor de declarações|Esse identificador é usado para identificar o provedor de declarações para esse Serviço de Federação.  Ele é usado ao receber declarações do provedor de declarações.|Quando esse Serviço de Federação está recebendo declarações do provedor de declarações, esse Serviço de Federação verificará se o emissor das declarações corresponde ao identificador do provedor de declarações.|
|Tipo de declaração|Esse identificador é usado para definir o tipo de declaração.  Ele é usado por esse Serviço de Federação, provedores de declarações e terceiras partes confiáveis ao enviar e receber declarações.|Quando o Serviço de Federação recebe declarações de um provedor de declarações, as regras de declaração associadas à confiança do provedor de declarações correspondente permitem que o administrador compare os tipos de declaração e processe as declarações.  As regras de declaração associadas a um objeto de confiança de terceira parte confiável também permitem que o administrador compare os tipos de declaração das declarações que vem das regras de confiança do provedor de declarações e decida quais declarações emitir.|

## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Correspondência de prefixo do URI para identificadores de terceira parte confiável
A sintaxe de caminho de um URI é organizada hierarquicamente e delimitada por todos os caracteres " \/ " ou todos ":".Portanto, o caminho pode ser dividido em seções de caminho com base no caractere de delimitação.Quando a correspondência de prefixo, cada seção deve ser uma correspondência completa de acordo com as regras de correspondência que \( essas regras regem a capitalização de correspondências \) . Para obter mais informações sobre regras de correspondência, consulte a RFC mencionada acima.

Quando uma terceira parte confiável é identificada em uma solicitação ao serviço de federação, o AD FS usa a lógica de correspondência de prefixo para determinar se há um objeto de confiança de terceira parte confiável correspondentes no banco de dados de configuração do AD FS.

Por exemplo, se o identificador de terceira parte confiável no banco de dados de configuração de AD FS \( uri1 \) for um prefixo para o identificador de terceira parte confiável na solicitação de entrada \( URI2 \) , o seguinte deverá ser verdadeiro:

-   Barras delimitadores à direita \( e dois-pontos \) de seções ou autoridades de caminho devem ser ignorados

-   As partes do esquema e da autoridade de URI1 e URI2 devem ser uma correspondência exata sem diferenciação entre maiúsculas e minúsculas

-   Cada seção de caminho de URI1 deve ser uma correspondência exata \( com base na distinção de maiúsculas e minúsculas escolhida \) à seção de caminho correspondente de URI2

-   URI2 pode ter mais seções de caminho que URI1, mas URI1 não pode ter mais seções de caminho que URI2

-   URI1 não pode ter mais seções de caminho que URI2

-   Se URI1 tiver um fragmento, ele deve corresponder exatamente a um fragmento de URI2

 >[!NOTE]
 > Não há suporte para parâmetros de cadeia de caracteres de consulta e eles serão ignorados em identificadores de terceira parte confiável.

A tabela a seguir fornece exemplos adicionais.

|Identificador de terceira parte confiável no banco de dados de configuração do AD FS|Identificador de terceira parte confiável na mensagem de solicitação|O identificador de solicitação corresponde ao identificador de configuração?|Motivo|
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|
|http: \/ \/ contoso.com|http: \/ \/ contoso.com|TRUE|Correspondência exata|
|http: \/ \/ contoso.com\/|http: \/ \/ contoso.com|TRUE|Barras à direita são ignoradas|
|http: \/ \/ contoso.com|http: \/ \/ contoso.com\/|TRUE|Barras à direita são ignoradas|
|http: \/ \/ contoso.com|http: \/ \/ contoso.com \/ hr|TRUE|O URI1 não tem caminho e combina esquema e autoridade com URI2|
|http: \/ \/ contoso.com \/ hr|http: \/ \/ contoso.com \/ HR \/ Web|TRUE|As primeiras seções do caminho são correspondentes, URI1 não tem nenhuma segunda seção do caminho|
|http: \/ \/ contoso.com \/ hr\/|http: \/ \/ contoso.com \/ HRW \/ Main|FALSE|A seção 1 do caminho do URI1 não coincide com a seção do caminho de URI2 1|
|http: \/ \/ contoso.com \/ hr|http: \/ \/ contoso.com|FALSE|URI1 tem mais seções de caminho que URI2|
|http: \/ \/ contoso.com \/ hr|http: \/ \/ contoso.com \/ hrweb|FALSE|As primeiras seções do caminho não coincidem|
|https: \/ \/ contoso.com|http: \/ \/ contoso.com|FALSE|Partes do esquema não coincidem|
|http: \/ \/ STS.contoso.com|http: \/ \/ contoso.com|FALSE|As partes de autoridade não coincidem|
|http: \/ \/ contoso.com|http: \/ \/ STS.contoso.com|FALSE|As partes de autoridade não coincidem|


