---
title: Active Directory Federation Services e informações de propriedade de especificação de chave do certificado
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b0d08f678e9e612bb0ce9cc38d254564bd9b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444095"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>O AD FS e a propriedade KeySpec informações de certificado
A especificação de chave ("KeySpec") é uma propriedade associada a um certificado e chave. Especifica se uma chave privada associada com um certificado pode ser usada para autenticação, criptografia ou ambos.   

Um valor incorreto de KeySpec pode causar erros de AD FS e Proxy de aplicativo Web, como:


- Falha ao estabelecer uma conexão SSL/TLS para o AD FS ou Proxy de aplicativo Web, com nenhum evento de AD FS registrado (embora os eventos de SChannel 36888 e 36874 podem ser registrados)
- Autenticação baseada em página, falha de logon no AD FS ou WAP do forms com nenhuma mensagem de erro mostrada na página.

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

## <a name="what-causes-the-problem"></a>O que faz com que o problema
A propriedade KeySpec identifica como uma chave gerada ou recuperados pela Microsoft CryptoAPI (CAPI), de um Microsoft herdados armazenamento provedor criptografia (CSP), pode ser usada.

Um valor KeySpec **1**, ou **AT_KEYEXCHANGE**, pode ser usado para assinatura e criptografia.  Um valor de **2**, ou **AT_SIGNATURE**, só é usada para assinatura.

A configuração incorreta de KeySpec mais comum é usando um valor de 2 para um certificado diferente do certificado de assinatura de token.  

Para certificados cujas chaves foram gerados por meio de provedores de criptografia CNG (Next Generation), não há nenhum conceito de especificação de chave e o valor de KeySpec sempre será zero.

Veja como verificar se há um valor válido de KeySpec abaixo. 

### <a name="example"></a>Exemplo
Um exemplo de um CSP herdado é o Microsoft Enhanced Cryptographic Provider. 

Formato de blob de chave RSA CSP Microsoft inclui um identificador de algoritmo, tanto **CALG_RSA_KEYX** ou **CALG_RSA_SIGN**, respectivamente, para atender a solicitações para qualquer um <strong>AT_KEYEXCHANGE * * ou * * AT_ ASSINATURA</strong> chaves.

Os identificadores de algoritmo de chave RSA mapeiam para os valores de KeySpec da seguinte maneira

| Algoritmo de provedor de suporte| Valor de especificação de chave para chamadas CAPI |
| --- | --- |
|CALG_RSA_KEYX : Chave RSA que pode ser usado para autenticação e descriptografia| AT_KEYEXCHANGE (or KeySpec=1)|
CALG_RSA_SIGN : Somente chave de assinatura RSA |AT_SIGNATURE (or KeySpec=2)|

## <a name="keyspec-values-and-associated-meanings"></a>Os valores de KeySpec e significados associados
Estes são os significados dos vários valores de KeySpec:

|Valor de KeySpec|Significa|Uso recomendado de AD FS|
| --- | --- | --- |
|0|O certificado é um certificado CNG|Somente para certificado SSL|
|1|Para um certificado CAPI (não CNG) herdado, a chave pode ser usada para assinatura e descriptografia|    SSL, autenticação, token descriptografia, certificados de comunicação de serviço de token|
|2|Para um certificado CAPI (não CNG) herdado, a chave pode ser usada apenas para a assinatura|Não é recomendado|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Como verificar o valor de KeySpec para seus certificados / chaves
Para ver um valor de certificados, você pode usar o **certutil** ferramenta de linha de comando.  

A seguir está um exemplo: **certutil – v – armazenar meu**.  Isso irá despejar as informações do certificado para a tela.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

Sob CERT_KEY_PROV_INFO_PROP_ID procure duas coisas:


1. **ProviderType:** isso denota se o certificado usa um herdados armazenamento provedor criptografia (CSP) ou um provedor de armazenamento de chaves com base na mais recente certificado CNG (Next Generation) APIs.  Qualquer valor diferente de zero indica um provedor herdado.
2. **KeySpec:** Os seguintes valores são válidos KeySpec para um certificado do AD FS:

   Provedor CSP herdado (ProviderType não é igual a 0):

   |Finalidade do AD FS certificado|Valores válidos KeySpec|
   | --- | --- |
   |Comunicação de serviço|1|
   |Descriptografia de token|1|
   |Assinatura de token|1 e 2|
   |SSL|1|

   Provedor de CNG (ProviderType = 0):

   |Finalidade do AD FS certificado|Valores válidos KeySpec|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Como alterar o keyspec para seu certificado para um valor com suporte
Alterando o valor de KeySpec não exige o certificado a ser gerado novamente ou novamente emitido pela autoridade de certificação.  KeySpec pode ser alterado com a importação novamente o certificado concluído e a chave privada de um arquivo PFX no repositório de certificados usando as etapas abaixo:


1. Primeiro, verifique e registre as permissões de chave privadas do certificado existente, para que eles possam ser reconfigurados se for necessário depois de importar novamente.
2. Exporte o certificado, incluindo a chave privada para um arquivo PFX.
3. Execute as seguintes etapas para cada servidor do AD FS e WAP
    1. Excluir o certificado (do AD FS / servidor WAP)
    2. Abra um prompt de comando com privilégios elevados do PowerShell e importe o arquivo PFX em cada servidor do AD FS e WAP usando a sintaxe de cmdlet a seguir, especificando o valor AT_KEYEXCHANGE (que funciona para todas as finalidades do certificado do AD FS):
        1. C:\>certutil – importpfx CertFile AT_KEYEXCHANGE
        2. Digite a senha PFX
    3. Após a conclusão acima, faça o seguinte
        1. Verifique as permissões de chave privadas
        2. Reinicie o serviço AD FS ou wap





