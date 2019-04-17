---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: "Como os URIs são usados no AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

# <a name="how-uris-are-used-in-ad-fs"></a>Como os URIs são usados no AD FS
Um Uniform Resource Identifier \(URI\) é uma cadeia de caracteres que é usada como um identificador exclusivo.  No AD FS, os URIs são usados para identificar os endereços de rede de parceiros e objetos de configuração.  Quando usado para identificar os endereços de rede de parceiros, o URI é sempre uma URL.  Quando usado para identificar os objetos de configuração, o URI pode ser um URN ou uma URL.  Para obter informações mais gerais sobre URIs, consulte [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) e [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URIs como endereços de rede de parceiros  
A seguir estão as URLs de endereço de rede geralmente são manipuladas por administradores do AD FS.  
  
-   As URLs do serviço de federação, incluindo WS\ federação, SAML, WS\ confiança, metadados de federação, WS\-MetadataExchange, privacidade e as URLs de organização  
  
-   As URLs de uma terceira confiança de terceiros, incluindo federação WS\, SAML e URLs de metadados de Federação  
  
-   As URLs de uma relação de confiança de provedor de declarações, incluindo federação WS\, SAML e URLs de metadados de Federação  
  
## <a name="uris-as-object-identifiers"></a>URIs como identificadores de objeto  
A tabela a seguir descreve os identificadores que geralmente são manipulados por administradores do AD FS.  
  
|Nome de identificador|Descrição|Comparações|  
|-------------------|---------------|---------------|  
|Identificador de serviço de Federação|Esse identificador é usado para identificar o serviço de Federação.  Ele é usado pelas partes confiantes que usam declarações desse serviço de federação, bem como provedores de declarações que o problema diz ao serviço de Federação.|Quando um usuário solicita requerimentos judiciais ou Extrajudiciais de um provedor de requerimentos judiciais ou Extrajudiciais por este serviço de federação, o identificador de serviço de Federação será usado para identificar o destino para que as declarações.<br /><br />Quando esse serviço de Federação recebe as declarações de um provedor de declarações, ele verificará para garantir que as declarações passam para ele, procurando por seu identificador de serviço de Federação.<br /><br />Quando um terceiro estiver recebendo requerimentos judiciais ou Extrajudiciais desse serviço de federação, o terceiro verificará se o emissor das declarações coincide com o identificador de serviço de Federação.|  
|Terceira identificador de terceiros|Esse identificador é usado para identificar o terceiro para esse serviço de Federação.  Ele é usado quando emitir declarações para a parte confiante.|Quando um usuário solicita requerimentos judiciais ou Extrajudiciais desse serviço de federação para o terceiro, o identificador de terceiros terceira será usado para identificar o terceiro para o qual as declarações devem ser direcionadas.  Essa comparação é feita usando o prefixo \(see below\) correspondente.<br /><br />Quando o terceiro recebe as declarações, ele verificará seu identificador no token de segurança para garantir que as declarações são direcionadas para ele.|  
|Identificador do provedor de declarações|Esse identificador é usado para identificar o provedor de declarações para esse serviço de Federação.  Ele é usado quando o recebimento de declarações do provedor de declarações.|Quando esse serviço de Federação está recebendo requerimentos judiciais ou Extrajudiciais do provedor de declarações, esse serviço de Federação verificará se o emissor das declarações coincide com o identificador do provedor de declarações.|  
|Tipo de declaração|Esse identificador é usado para definir o tipo de declaração.  Ele é usado por esse serviço de federação, provedores de declarações e partes confiantes ao enviar e receber solicitações.|Quando o serviço de Federação recebe declarações de um provedor de declarações, as regras de declaração associadas a relação de confiança de provedor correspondente declarações permitem que o administrador comparar os tipos de declaração e processar solicitações.  As regras de declaração associadas a uma relação de confiança terceira parte também permitem que o administrador comparar os tipos de declaração de requerimentos judiciais ou Extrajudiciais oriundo das regras de confiança do provedor de declarações e decidir quais alega dar emitir.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Prefixo URI correspondência para terceira identificadores de terceiros  
A sintaxe de caminho de um URI é organizada hierarquicamente e é delimitada por todos os "\ /"caracteres ou todas as":" caracteres.  Portanto, o caminho pode ser dividido em seções de caminho com base no caractere de delimitação.  Quando prefixo correspondência, cada seção deve ser uma correspondência completa de acordo com as regras correspondentes \ (essas regras regem o uso de maiusculas de matches\). Para saber mais sobre as regras de correspondência, veja a RFC mencionado acima.  
  
Quando um terceiro é identificado em uma solicitação ao serviço de federação, o AD FS usa o prefixo lógica de correspondência para determinar se há uma correspondência terceira parte relação de confiança no banco de dados de configuração do AD FS.  
  
Por exemplo, se o identificador de terceiros terceira no banco de dados de configuração do AD FS \(URI1\) é um prefixo para o identificador de terceiros confiante na entrada solicitar \(URI2\), em seguida, a seguir deve ser verdadeira:  
  
-   À direita delimitadores \(slashes and colons\) de caminho devem ser ignoradas seções ou autoridades  
  
-   As partes de esquema e a autoridade de URI1 e URI2 devem ser uma correspondência exata de diferenciação de maiusculas e minúsculas  
  
-   Cada seção caminho da URI1 deve ser uma correspondência exata \ (com base em chosen\ a diferenciação de maiusculas e minúsculas) para a seção correspondente do caminho de URI2  
  
-   URI2 pode ter mais seções de caminho que URI1, mas URI1 não deve ter mais seções de caminho que URI2  
  
-   URI1 não pode ter mais seções de caminho que URI2  
  
-   Se URI1 tem uma cadeia de caracteres de consulta, ele deve corresponder exatamente a uma cadeia de caracteres de consulta URI2  
  
-   Se URI1 tiver um fragmento, ele deve corresponder exatamente a um fragmento URI2  
  
A tabela a seguir fornece exemplos adicionais.  
  
|Dependendo do identificador de terceiros no banco de dados de configuração do AD FS|Identificador de terceiros na mensagem de solicitação de dependência|Identificador correspondências solicitar o identificador de configuração?|Motivo|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:///\/contoso.com|http:///\/contoso.com|TRUE|Correspondência exata|  
|http:///\/contoso.com\/|http:///\/contoso.com|TRUE|Barras à direita são ignoradas|  
|http:///\/contoso.com|http:///\/contoso.com\/|TRUE|Barras à direita são ignoradas|  
|http:///\/contoso.com|http:///\/contoso.com\/hr|TRUE|URI1 não tem nenhum caminho e correspondências esquema e autoridade para URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/Web|TRUE|Seções do primeiro caminho correspondem, URI1 não tem nenhuma segunda seção caminho|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/web\/?m\=t|TRUE|Mesmos motivos acima, cadeia de caracteres de consulta não altera qualquer coisa|  
|http:///\/contoso.com\/hr\/|http:///\/contoso.com\/hrw\/Main|FALSE|Caminho URI1 seção 1 não coincide com a seção de caminho URI2 1|  
|http:///\/contoso.com\/hr|http:///\/contoso.com|FALSE|URI1 tem mais seções de caminho que URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hrweb|FALSE|Seções do primeiro caminho não coincidem|  
|http:///\/contoso.com\/?m\=t|http:///\/contoso.com\/?m\=f|FALSE|Partes de cadeia de caracteres de consulta não coincidem|  
|https:\/\/contoso.com|http:///\/contoso.com|FALSE|Partes do esquema não coincidem|  
|http:///\/STS.contoso.com|http:///\/contoso.com|FALSE|Partes de autoridade não coincidem|  
|http:///\/contoso.com|http:///\/STS.contoso.com|FALSE|Partes de autoridade não coincidem|  
  

