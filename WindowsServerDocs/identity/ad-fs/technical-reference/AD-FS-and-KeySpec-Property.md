---
title: Informações de propriedade de especificação de chave de certificado e Serviços de Federação do Active Directory (AD FS)
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 51c9828cfe494c68422f4985e5b17113020c8414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407411"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>Informações da propriedade KeySpec AD FS e do certificado
A especificação de chave ("KeySpec") é uma propriedade associada a um certificado e a uma chave. Especifica se uma chave privada associada a um certificado pode ser usada para assinatura, criptografia ou ambos.   

Um valor KeySpec incorreto pode causar erros de AD FS e de proxy de aplicativo Web, como:


- Falha ao estabelecer uma conexão SSL/TLS com o AD FS ou o proxy de aplicativo Web, sem eventos de AD FS registrados (embora os eventos SChannel 36888 e 36874 possam ser registrados em log)
- Falha ao fazer logon na página de autenticação baseada em formulários do AD FS ou WAP, sem nenhuma mensagem de erro mostrada na página.

Você pode ver o seguinte no log de eventos:

    Log Name:      AD FS Tracing/Debug
    Source:        AD FS Tracing
    Date:          2/12/2015 9:03:08 AM
    Event ID:      67
    Task Category: None
    Level:         Error
    Keywords:      ADFSProtocol
    User:          S-1-5-21-3723329422-3858836549-556620232-1580884
    Computer:      ADFS1.contoso.com
    Description:
    Ignore corrupted SSO cookie.

## <a name="what-causes-the-problem"></a>O que causa o problema
A propriedade KeySpec identifica como uma chave gerada ou recuperada pelo Microsoft CryptoAPI (CAPI), de um CSP (provedor de armazenamento criptográfico) herdado da Microsoft, pode ser usada.

Um valor KeySpec de **1**, ou **AT_KEYEXCHANGE**, pode ser usado para assinatura e criptografia.  Um valor de **2**, ou **AT_SIGNATURE**, é usado apenas para assinatura.

A configuração de mis de KeySpec mais comum está usando um valor de 2 para um certificado diferente do certificado de autenticação de token.  

Para certificados cujas chaves foram geradas usando provedores CNG (Cryptography Next Generation), não há nenhum conceito de especificação de chave e o valor KeySpec será sempre zero.

Consulte Como verificar um valor de KeySpec válido abaixo. 

### <a name="example"></a>Exemplo
Um exemplo de CSP herdado é o provedor criptográfico aprimorado da Microsoft. 

O formato de BLOB da chave RSA CSP da Microsoft inclui um identificador de algoritmo, **CALG_RSA_KEYX** ou **CALG_RSA_SIGN**, respectivamente, para atender a solicitações para as chaves <strong>AT_KEYEXCHANGE * * ou * * AT_SIGNATURE</strong> .

Os identificadores de algoritmo de chave RSA são mapeados para valores KeySpec da seguinte maneira

| Algoritmo com suporte do provedor| Valor de especificação de chave para chamadas CAPI |
| --- | --- |
|CALG_RSA_KEYX : Chave RSA que pode ser usada para assinatura e descriptografia| AT_KEYEXCHANGE (ou KeySpec = 1)|
CALG_RSA_SIGN : Chave somente de assinatura RSA |AT_SIGNATURE (ou KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valores de KeySpec e significados associados
A seguir estão os significados dos vários valores KeySpec:

|Valor KeySpec|Maneira|Recomendado AD FS uso|
| --- | --- | --- |
|0|O certificado é um certificado CNG|Somente certificado SSL|
|1|Para um certificado CAPI (não CNG) herdado, a chave pode ser usada para assinatura e descriptografia|    SSL, assinatura de token, descriptografia de token, certificados de comunicação de serviço|
|2|Para um certificado CAPI (não CNG) herdado, a chave pode ser usada somente para assinatura|Não recomendado|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Como verificar o valor de KeySpec para seus certificados/chaves
Para ver um valor de certificados, você pode usar a ferramenta de linha de comando **certutil** .  

Veja a seguir um exemplo: **certutil – v – Store My**.  Isso irá despejar as informações do certificado na tela.

![Certificado KeySpec](media/AD-FS-and-KeySpec-Property/keyspec1.png)

Em CERT_KEY_PROV_INFO_PROP_ID, procure duas coisas:


1. **ProviderType:** indica se o certificado usa um CSP (provedor de armazenamento criptográfico) herdado ou um provedor de armazenamento de chaves baseado em APIs CNG (mais recentes).  Qualquer valor diferente de zero indica um provedor herdado.
2. **KeySpec** Estes são os valores de KeySpec válidos para um certificado de AD FS:

   Provedor CSP herdado (ProviderType diferente de 0):

   |Finalidade do certificado AD FS|Valores de KeySpec válidos|
   | --- | --- |
   |Comunicação de serviço|1|
   |Descriptografia de token|1|
   |Assinatura de token|1 e 2|
   |SSL|1|

   Provedor CNG (ProviderType = 0):

   |Finalidade do certificado AD FS|Valores de KeySpec válidos|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Como alterar o KeySpec do seu certificado para um valor com suporte
Alterar o valor KeySpec não exige que o certificado seja gerado novamente ou emitido novamente pela autoridade de certificação.  O KeySpec pode ser alterado importando novamente o certificado completo e a chave privada de um arquivo PFX para o repositório de certificados usando as etapas abaixo:


1. Primeiro, verifique e registre as permissões de chave privada no certificado existente para que elas possam ser reconfiguradas, se necessário, após a reimportação.
2. Exporte o certificado, incluindo a chave privada, para um arquivo PFX.
3. Execute as seguintes etapas para cada AD FS e servidor WAP
    1. Excluir o certificado (do servidor AD FS/WAP)
    2. Abra um prompt de comando do PowerShell com privilégios elevados e importe o arquivo PFX em cada AD FS e servidor WAP usando a sintaxe de cmdlet abaixo, especificando o valor AT_KEYEXCHANGE (que funciona para todos os AD FS fins de certificado):
        1. C: \>certutil – importpfx CertFile. pfx AT_KEYEXCHANGE
        2. Inserir senha PFX
    3. Depois de concluir, faça o seguinte
        1. verificar as permissões de chave privada
        2. reiniciar o serviço ADFS ou WAP





