---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Como URIs são usados no AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 677d3136305cbddd29f2fd782be33ae1e824d096
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188555"
---
# <a name="how-uris-are-used-in-ad-fs"></a>Como URIs são usados no AD FS
Um Uniform Resource Identifier \(URI\) é uma cadeia de caracteres que é usada como um identificador exclusivo.  No AD FS, os URIs são usados para identificar os endereços de rede do parceiro e objetos de configuração.  Quando usado para identificar os endereços de rede do parceiro, o URI é sempre uma URL.  Quando usado para identificar objetos de configuração, o URI pode ser uma URL ou um URN.  Para obter mais informações sobre URIs, consulte [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) e [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URIs como endereços de rede do parceiro  
A seguir estão as URLs de endereço de rede mais manipuladas pelos administradores do AD FS.  
  
-   As URLs do serviço de federação, incluindo WS\-Federation, SAML, WS\-confiar, metadados de federação, WS\-MetadataExchange, privacidade e as URLs da organização  
  
-   As URLs de uma terceira parte confiável, incluindo WS\-Federation, SAML e URLs de metadados de Federação  
  
-   As URLs de uma relação de confiança de provedor de declarações, incluindo WS\-Federation, SAML e URLs de metadados de Federação  
  
## <a name="uris-as-object-identifiers"></a>URIs como identificadores de objeto  
A tabela a seguir descreve os identificadores mais manipulados pelos administradores do AD FS.  
  
|Nome do identificador|Descrição|Comparações|  
|-------------------|---------------|---------------|  
|Identificador do Serviço de Federação|Esse identificador é usado para identificar o Serviço de Federação.  Ele é usado pelas partes confiáveis que usam declarações deste Serviço de Federação, bem como provedores de declarações que emitem declarações para esse Serviço de Federação.|Quando um usuário solicita declarações de um provedor de declarações para esse serviço de federação, o identificador do serviço de Federação será usado para identificar o destino para as declarações.<br /><br />Quando esse Serviço de Federação receber as declarações de um provedor de declarações, ele verificará para garantir que as declarações tenham o escopo relevante procurando pelo seu identificador de Serviço de Federação.<br /><br />Quando uma terceira parte confiável estiver recebendo declarações desse serviço de federação, a terceira parte confiável verificará se o emissor das declarações corresponde ao identificador do Serviço de Federação.|  
|Identificador terceira parte confiável|Esse identificador é usado para identificar a terceira parte confiável para esse Serviço de Federação.  Ele é usado ao emitir declarações à terceira parte confiável.|Quando um usuário solicitar declarações desse serviço de federação à terceira parte confiável, o identificador da terceira parte será usado para identificar a terceira parte confiável para a qual as declarações devem ser direcionadas.  Essa comparação é feita usando a correspondência de prefixo \(veja abaixo\).<br /><br />Ao receber as declarações, a terceira parte confiável verifica seu identificador no token de segurança para garantir que as declarações sejam direcionadas a ela.|  
|Identificador do provedor de declarações|Esse identificador é usado para identificar o provedor de declarações para esse Serviço de Federação.  Ele é usado ao receber declarações do provedor de declarações.|Quando esse Serviço de Federação está recebendo declarações do provedor de declarações, esse Serviço de Federação verificará se o emissor das declarações corresponde ao identificador do provedor de declarações.|  
|Tipo de declaração|Esse identificador é usado para definir o tipo de declaração.  Ele é usado por esse Serviço de Federação, provedores de declarações e terceiras partes confiáveis ao enviar e receber declarações.|Quando o Serviço de Federação recebe declarações de um provedor de declarações, as regras de declaração associadas à confiança do provedor de declarações correspondente permitem que o administrador compare os tipos de declaração e processe as declarações.  As regras de declaração associadas a um objeto de confiança de terceira parte confiável também permitem que o administrador compare os tipos de declaração das declarações que vem das regras de confiança do provedor de declarações e decida quais declarações emitir.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Correspondência de prefixo do URI para identificadores de terceira parte confiável  
A sintaxe do caminho de um URI é organizada hierarquicamente e é delimitada por todos os "\/"caracteres ou tudo":" caracteres.  Portanto, o caminho pode ser dividido em seções de caminho com base no caractere de delimitação.  Ao realizar correspondência de prefixo, cada seção deve ser uma correspondência completa de acordo com as regras de correspondência \(essas regras regem o uso de maiusculas e minúsculas de correspondências\). Para obter mais informações sobre regras de correspondência, consulte os RFCs mencionados acima.  
  
Quando uma terceira parte confiável é identificada em uma solicitação ao serviço de federação, o AD FS usa a lógica de correspondência de prefixo para determinar se há um objeto de confiança de terceira parte confiável correspondentes no banco de dados de configuração do AD FS.  
  
Por exemplo, se o identificador de terceira parte confiável no banco de dados de configuração do AD FS \(URI1\) é um prefixo para o identificador de terceira parte confiável na solicitação de entrada \(URI2\), em seguida, o seguinte deve ser verdadeiro:  
  
-   Delimitadores de espaçamento \(barras e dois-pontos\) do caminho de seções ou autoridades devem ser ignoradas  
  
-   As partes do esquema e da autoridade de URI1 e URI2 devem ser uma correspondência exata sem diferenciação entre maiúsculas e minúsculas  
  
-   Cada seção do caminho de URI1 deve ser uma correspondência exata \(com base em maiusculas e minúsculas escolhida\) para a seção correspondente do caminho de URI2  
  
-   URI2 pode ter mais seções de caminho que URI1, mas URI1 não pode ter mais seções de caminho que URI2  
  
-   URI1 não pode ter mais seções de caminho que URI2  
  
-   Se URI1 tiver uma cadeia de caracteres de consulta, ela deve corresponder exatamente a uma cadeia de caracteres de consulta de URI2  
  
-   Se URI1 tiver um fragmento, ele deve corresponder exatamente a um fragmento de URI2  
  
A tabela a seguir fornece exemplos adicionais.  
  
|Identificador de terceira parte confiável no banco de dados de configuração do AD FS|Identificador de terceira parte confiável na mensagem de solicitação|O identificador de solicitação corresponde ao identificador de configuração?|Reason|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:\/\/contoso.com|http:\/\/contoso.com|TRUE|Correspondência exata|  
|http:\/\/contoso.com\/|http:\/\/contoso.com|TRUE|Barras à direita são ignoradas|  
|http:\/\/contoso.com|http:\/\/contoso.com\/|TRUE|Barras à direita são ignoradas|  
|http:\/\/contoso.com|http:\/\/contoso.com\/hr|TRUE|O URI1 não tem caminho e combina esquema e autoridade com URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web|TRUE|As primeiras seções do caminho são correspondentes, URI1 não tem nenhuma segunda seção do caminho|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web\/? m\=t|TRUE|Mesmos motivos que os acima, a cadeia de caracteres de consulta não altera nada|  
|http:\/\/contoso.com\/hr\/|http:\/\/contoso.com\/hrw\/main|FALSE|A seção 1 do caminho do URI1 não coincide com a seção do caminho de URI2 1|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com|FALSE|URI1 tem mais seções de caminho que URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hrweb|FALSE|As primeiras seções do caminho não coincidem|  
|http:\/\/contoso.com\/?m\=t|http:\/\/contoso.com\/?m\=f|FALSE|As partes da cadeia de caracteres de consulta não coincidem|  
|https:\/\/contoso.com|http:\/\/contoso.com|FALSE|Partes do esquema não coincidem|  
|http:\/\/sts.contoso.com|http:\/\/contoso.com|FALSE|As partes de autoridade não coincidem|  
|http:\/\/contoso.com|http:\/\/sts.contoso.com|FALSE|As partes de autoridade não coincidem|  
  

