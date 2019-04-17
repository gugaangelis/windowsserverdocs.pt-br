---
title: "Active Directory serviços de Federação e certificado informações de propriedade de especificação de chave"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS e certificado KeySpec propriedade informações
Especificação de chave ("KeySpec") é uma propriedade associada a um certificado e a chave. Ele especifica se uma chave privada associada a um certificado pode ser usada para assinar, criptografia ou ambos.   

Um valor KeySpec incorreto pode causar erros AD FS e Proxy de aplicativo Web, como:


- Falha ao estabelecer uma conexão SSL/TLS para AD FS ou o Proxy do aplicativo da Web, com nenhum evento de AD FS registrado (embora SChannel 36888 e 36874 eventos podem ser registrados)
- Falha ao fazer logon no AD FS ou WAP forma página autenticação baseada em, sem nenhuma mensagem de erro mostrada na página.

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
A propriedade KeySpec identifica como uma chave gerada ou recuperado pelo Microsoft CryptoAPI (CAPI), de um Microsoft herdados armazenamento provedor criptografia (CSP), pode ser usada.

Um valor KeySpec **1**, ou **AT_KEYEXCHANGE**, pode ser usado para assinatura e criptografia.  Um valor de **2**, ou **AT_SIGNATURE**, só é usado para assinar.

A configuração de errados KeySpec mais comum é usando um valor de 2 para um certificado que não seja o token de certificado de assinatura.  

Para obter certificados cujos chaves foram geradas usando provedores de criptografia próxima geração (CNG), há nenhum conceito de especificação de chave e o valor de KeySpec sempre será zero.

Veja como verificar se há um valor de KeySpec válido abaixo. 

### <a name="example"></a>Exemplo
Um exemplo de um CSP herdado é o provedor criptográfico Microsoft avançado. 

Formato de blob da chave RSA CSP da Microsoft inclui um identificador de algoritmo, ou **CALG_RSA_KEYX** ou **CALG_RSA_SIGN**, respectivamente, às solicitações de serviço de um * * AT_KEYEXCHANGE * * ou **AT_SIGNATURE** chaves.
  
Os identificadores de algoritmo de chave RSA são mapeadas para valores de KeySpec da seguinte maneira

| Algoritmo de provedor de suporte| Valor de chave especificação para chamadas CAPI |
| --- | --- |
|CALG_RSA_KEYX: Chave RSA que pode ser usado para descriptografia e assinatura| AT_KEYEXCHANGE (ou KeySpec = 1)|
CALG_RSA_SIGN: Assinatura RSA somente chave |AT_SIGNATURE (ou KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>Valores de KeySpec e significados associados
Estes são os significados dos vários valores de KeySpec:

|Valor de KeySpec|Significa|Uso recomendado do AD FS|
| --- | --- | --- |
|0|O certificado é um cert CNG|Certificado SSL somente|
|1|Para um cert CAPI (não CNG) herdado, a chave pode ser usada para descriptografia e assinatura|    SSL, o token de assinatura, token descriptografia, certificados de comunicação de serviço|
|2|Para um cert CAPI (não CNG) herdado, a chave pode ser usada somente para assinatura|Não é recomendado|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>Como verificar o valor de KeySpec para os certificados / chaves
Para ver um valor de certificados, você pode usar o **certutil** ferramenta de linha de comando.  

A seguir está um exemplo: **certutil – v – armazenar meu**.  Isso irá despejar as informações de certificado na tela.

![KeySpec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

Sob CERT_KEY_PROV_INFO_PROP_ID, procure duas coisas:


1. **ProviderType:** isso indica se o certificado usa um herdados armazenamento provedor criptografia (CSP) ou um provedor de armazenamento de chave com base na mais recente certificado de última geração (CNG) APIs.  Qualquer valor diferente de zero indica um provedor herdado.
2.  **KeySpec:** a seguir estão os valores válidos de KeySpec para obter um certificado do AD FS:

    Provedor CSP herdado (ProviderType não igual a 0):
    
    |AD FS certificado finalidade|Valores válidos KeySpec|
    | --- | --- |
    |Comunicação do serviço|1|
    |Descriptografia de token|1|
    |Token de assinatura|1 e 2|
    |SSL|1|

    Provedor CNG (ProviderType = 0):
    |AD FS certificado finalidade|Valores válidos KeySpec|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>Como alterar o keyspec para seu certificado para um valor com suporte
Alterando o valor de KeySpec não exige que o certificado a ser gerado novamente ou novamente emitidos pela autoridade de certificação.  O KeySpec pode ser alterado através da importação novamente o certificado completo e a chave privada de um arquivo PFX para o repositório de certificados usando as etapas abaixo:


1. Primeiro, verifique e registre as permissões de chave privadas no certificado existente para que eles podem ser configurados novamente se necessário após o reimportar.
2. Exporte o certificado incluindo um arquivo PFX a chave privada.
3. Siga as etapas abaixo para cada servidor AD FS e WAP
    1. Excluir o certificado (do AD FS / servidor WAP)
    2. Abra um prompt de comando do PowerShell com privilégios elevados e importar o arquivo PFX em cada servidor AD FS e WAP usando a sintaxe do cmdlet abaixo, especificando o valor AT_KEYEXCHANGE (que funciona para todas as finalidades de certificados do AD FS):
        1. C:\ > certutil – importpfx certfile.pfx AT_KEYEXCHANGE
        2. Digite a senha PFX
    3. Após a conclusão acima, faça o seguinte
        1. Verifique as permissões de chave privadas
        2. Reiniciar o serviço adfs ou wap





