---
title: certreq
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b947a231228d66084bc61146f7347a76cf41406
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434513"
---
# <a name="certreq"></a>certreq



CertReq pode ser usado para solicitar certificados de uma autoridade de certificação (CA), para recuperar uma resposta a uma solicitação anterior de uma autoridade de certificação, para criar uma nova solicitação de um arquivo. inf, para aceitar e instalar uma resposta a uma solicitação, para construir uma certificação cruzada ou solicitação de subordinação qualificada de um certificado de autoridade de certificação ou solicitação existente e para assinar uma solicitação de certificação cruzada ou de subordinação qualificada.

> [!WARNING]
> - Versões anteriores do certreq podem não fornecer todas as opções descritas neste documento. Você pode ver todas as opções que uma versão específica do certreq fornece executando os comandos mostrados na seção de sintaxe de notações.
> - CertReq não dá suporte à criação de uma nova solicitação de certificado com base em um modelo de atestado de chaves quando em um ambiente de CEP/CES

## <a name="BKMK_Contents"></a>Conteúdo

As seções principais neste artigo são da seguinte maneira:
1.  [Verbs](#BKMK_Verbs)
2.  [Notações de sintaxe](#BKMK_notation)
3.  [Opções](#BKMK_Options)
4.  [Formatos](#BKMK_Formats)
5.  [Exemplos adicionais certreq](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbs

Lá, a tabela a seguir descreve os verbos que podem ser usados com o comando certreq

|Alternar|Descrição|
|------|-----------|
|-Submit|Envia uma solicitação para uma autoridade de certificação. Para obter mais informações, consulte [Certreq-enviar](#BKMK_Submit).|
|-recuperar *RequestID*|Recupera uma resposta para uma solicitação anterior de uma autoridade de certificação. Para obter mais informações, consulte [Certreq-recuperar](#BKMK_Retrieve).|
|-New|Cria uma nova solicitação de um arquivo. inf. Para obter mais informações, consulte [Certreq-novo](#BKMK_New).|
|-Accept|Aceita e instala uma resposta a uma solicitação de certificado. Para obter mais informações, consulte [Certreq-aceitar](#BKMK_accept).|
|-Policy|Define a política para uma solicitação. Para obter mais informações, consulte [Certreq-política](#BKMK_policy).|
|-Sign|Assina uma solicitação de certificação cruzada ou de subordinação qualificada. Para obter mais informações, consulte [Certreq-entrada](#BKMK_sign).|
|-Enroll|Se inscreve para obter ou renova um certificado. Para obter mais informações, consulte [Certreq-registrar](#BKMK_enroll).|
|-?|Exibe uma lista de descrições de sintaxe certreq e opções.|
|*\<verb>* -?|Exibe a Ajuda para o verbo especificado.|
|-v -?|Exibe uma lista detalhada da sintaxe do certreq, opções e descrições.|

Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notações de sintaxe

-   Para obter a sintaxe de linha de comando básica, execute `certreq -?`
-   Para obter a sintaxe sobre como usar certutil com um verbo específico, execute **certreq**  *\<verbo >* **-?**
-   Para enviar todas as sintaxes certutil em um arquivo de texto, execute os seguintes comandos:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

A tabela a seguir descreve a notação usada para indicar a sintaxe de linha de comando.

|Notação|Descrição|
|--------|-----------|
|Texto sem colchetes ou entre chaves|Itens que você deve digitar conforme mostrado|
|\<Texto dentro de colchetes angulares >|Espaço reservado para o qual você deve fornecer um valor|
|[Texto dentro dos colchetes]|Itens opcionais|
|{Texto entre chaves}|Conjunto de itens necessários; Escolha uma|
|Barra vertical (&#124;)|Separador de itens mutuamente exclusivos; Escolha uma|
|Reticências (…)|Itens que podem ser repetidos|

Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq -submit

Esse é o parâmetro de certreq.exe de padrão, se nenhuma opção for especificada explicitamente no prompt de linha de comando, certreq.exe tenta enviar uma solicitação de certificado para uma autoridade de certificação.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Você deve especificar um arquivo de solicitação de certificado ao usar – opção de enviar. Se esse parâmetro for omitido, uma janela Abrir arquivo comum é exibida onde você pode selecionar o arquivo de solicitação de certificado apropriado.

Você pode usar esses exemplos como ponto de partida para criar a solicitação de envio do certificado:

Para enviar uma solicitação de certificado simples usar o exemplo a seguir:
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Para solicitar um certificado, especificando o atributo de SAN, consulte as etapas detalhadas no artigo da Base de Conhecimento Microsoft 931351 [como adicionar um nome alternativo da entidade a um certificado LDAP seguro](https://support.microsoft.com/kb/931351) em "como usar o utilitário Certreq.exe para seção criar e enviar uma solicitação de certificado que inclui uma rede SAN".

Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>CertReq-recuperar

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Se você não especificar o CAComputerName ou CAName na caixa de diálogo - config CAComputerName\CANamea aparece e exibe uma lista de todas as CAs que estão disponíveis.
-   Se você usar - config - em vez de-config CAComputerName\CAName, a operação é processada usando a autoridade de certificação padrão.
-   Você pode usar o certreq-recuperar *RequestID* para recuperar o certificado depois que a autoridade de certificação, na verdade, o emitiu. O *RequestID*PKC pode ser um decimal ou hexadecimal com 0x prefixo e ele pode ser um número de série do certificado sem prefixo 0x. Você também pode usá-lo para recuperar qualquer certificado que já foi emitido pela autoridade de certificação, incluindo certificados revogados ou expirados, independentemente da solicitação do certificado ter no estado pendente.
-   Se você enviar uma solicitação para a autoridade de certificação, o módulo de política da autoridade de certificação pode deixar a solicitação em um estado pendente e retornar os *RequestID* ao chamador Certreq para exibição. Eventualmente, o administrador da autoridade de certificação será emitir o certificado ou negar a solicitação.

O comando a seguir recupera a id do certificado 20 e cria o arquivo de certificado (. cer):
```
certreq -retrieve 20 MyCertificate.cer 
```
Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_New"></a>CertReq-novo

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Uma vez que o arquivo INF permite para um conjunto avançado de parâmetros e opções sejam especificadas, é difícil de definir um modelo padrão que os administradores devem usar para todas as finalidades. Portanto, esta seção descreve todas as opções para que você possa criar um arquivo INF sob medido para suas necessidades específicas. As seguintes palavras-chave são usadas para descrever a estrutura do arquivo INF.
1.  Um *seção* é uma área no arquivo INF que abrange um grupo lógico de chaves. Uma seção sempre aparece entre colchetes no arquivo INF.
2.  Um *chave* é o parâmetro que está à esquerda do sinal de igual.
3.  Um *valor* é o parâmetro que está à direita do sinal de igual.

Por exemplo, um arquivo INF mínimo seria semelhante ao seguinte:
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
Seguem algumas das seções possíveis que podem ser adicionadas ao arquivo INF:

**[NewRequest]**

Esta seção é obrigatória para um arquivo INF que atua como um modelo para uma nova solicitação de certificado. Esta seção requer pelo menos uma chave com um valor.

|Chave|Definição|Valor|Exemplo|
|---|----------|-----|-------|
|Subject|Vários aplicativos se baseiam nas informações da entidade de um certificado. Portanto, é recomendável que um valor para essa chave seja especificado. Se a entidade não for definida aqui, é recomendável que um nome de entidade pode ser incluído como parte da extensão de certificado de nome alternativo da entidade.|Valores de cadeia de caracteres de nome distinto relativos|Assunto = Subject "CN=computer1.contoso.com" = "CN = John Smith, CN = Users, DC = Contoso, DC = com"|
|Exportável|Se esse atributo é definido como TRUE, a chave privada pode ser exportada com o certificado. Para garantir um alto nível de segurança, as chaves privadas não devem ser exportáveis; No entanto, em alguns casos, pode ser necessário para tornar a chave privada exportável se vários computadores ou usuários devem compartilhar a mesma chave privada.|verdadeiro, FALSO|Exportável = TRUE. Chaves CNG podem distinguir entre esta e exportáveis de texto sem formatação. Não é possível CAPI1 chaves.|
|ExportableEncrypted|Especifica se a chave privada deve ser definida para ser exportável.|verdadeiro, FALSO|ExportableEncrypted = true</br>Dica: Nem todos os tamanhos de chave públicos e algoritmos funcionará com todos os algoritmos de hash. Tamehe especificado que CSP também deve suportar o algoritmo de hash especificado. Para ver a lista de algoritmos de hash com suporte, você pode executar o comando <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algoritmo de hash a ser usado para esta solicitação.|Sha256, sha384, sha512, sha1, md5, md4, md2|HashAlgorithm = sha1. Para ver a lista de algoritmos de hash com suporte, use: certutil - oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo|
|KeyAlgorithm|O algoritmo que será usado pelo provedor de serviço para gerar um par de chaves público e privado.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|Não é recomendável definir esse parâmetro para novas solicitações onde o novo material da chave é gerada. O contêiner de chave é gerado automaticamente e mantido pelo sistema. Para solicitações onde o material da chave existente deve ser usado, esse valor pode ser definido para o nome do contêiner de chave da chave existente. Use o certutil – chave de comando para exibir a lista de contêineres de chaves disponíveis para o contexto do computador. Use o certutil – chave – comando de usuário para o contexto do usuário atual.|Valor de cadeia de caracteres aleatória</br>Dica: Você deve usar aspas duplas em torno de qualquer valor de chave INF que tem espaços em branco ou caracteres especiais para evitar possíveis problemas de análise de INF.|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|Define o comprimento da chave pública e privada. O comprimento de chave tem um impacto sobre o nível de segurança do certificado. Comprimento de chave maior geralmente fornece um nível mais alto de segurança; No entanto, alguns aplicativos podem ter limitações em relação ao comprimento da chave.|Qualquer comprimento de chave válido que é compatível com o provedor de serviços de criptografia.|KeyLength = 2048|
|KeySpec|Determina se a chave pode ser usada para assinaturas, para o Exchange (criptografia) ou para ambos.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|KeyUsage|Define para que a chave do certificado deve ser usada.|CERT_DIGITAL_SIGNATURE_KEY_USAGE – 80 (128)</br>Dica: Os valores mostrados são os valores hexadecimais de (decimais) para cada definição de bit. Sintaxe mais antigo também pode ser usado: um único valor hexadecimal com vários bits de conjunto, em vez da representação simbólica. Por exemplo, KeyUsage = 0xa0.</br>CERT_NON_REPUDIATION_KEY_USAGE – 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE – 8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Dica: Vários valores de usam um pipe (&#124;) o separador de símbolo. Certifique-se de que você use aspas duplas ao usar vários valores para evitar problemas de análise de INF.|
|KeyUsageProperty|Recupera um valor que identifica o objetivo específico para o qual uma chave privada pode ser usada.|NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|Essa chave é importante quando você precisa criar certificados que pertencem a máquina e não um usuário. O material da chave que é gerado é mantido no contexto de segurança da entidade de segurança (conta de usuário ou computador) que criou a solicitação. Quando um administrador cria uma solicitação de certificado em nome de um computador, o material da chave deve ser criado no contexto de segurança do computador e não o contexto de segurança do administrador. Caso contrário, o computador não pôde acessar sua chave privada, pois ele seria no contexto de segurança do administrador.|verdadeiro, FALSO|MachineKeySet = true</br>Dica: O padrão é False.|
|NotBefore|Especifica uma data ou data e hora antes da qual a solicitação não pode ser emitida. NotBefore pode ser usado com ValidityPeriod e ValidityPeriodUnits.|data ou data e hora|NotBefore = "24/7/2012 31 10h"</br>Dica: NotBefore e NotAfter são para RequestType = cert apenas. Data de análise tenta ser sensíveis à localidade. Usando nomes removerá a ambiguidade e devem funcionar em todas as localidades de mês.|
|NotAfter|Especifica uma data ou data e hora após o qual a solicitação não pode ser emitida. NotAfter não pode ser usado com ValidityPeriod ou ValidityPeriodUnits.|data ou data e hora|NotAfter = "9/23/2014 10:31 AM"</br>Dica: NotBefore e NotAfter são para RequestType = cert apenas. Data de análise tenta ser sensíveis à localidade. Usando nomes removerá a ambiguidade e devem funcionar em todas as localidades de mês.|
|PrivateKeyArchive|A configuração PrivateKeyArchive só funcionará se o RequestType correspondente é definido como "CMC", porque permite que apenas as mensagens de gerenciamento de certificado em formato de solicitação de CMS (CMC) para transferir com segurança a chave privada do solicitante à autoridade de certificação para arquivamento de chaves.|verdadeiro, FALSO|PrivateKeyArchive = True|
|EncryptionAlgorithm|O algoritmo de criptografia a ser usado.|As possíveis opções variam, dependendo da versão do sistema operacional e o conjunto de provedores de criptografia instalados. Para ver a lista de algoritmos disponíveis, execute o comando <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> CSP especificado usado também deve dar suporte o algoritmo de criptografia simétrica especificado e o comprimento.|EncryptionAlgorithm = 3des|
|EncryptionLength|Comprimento do algoritmo de criptografia a ser usado.|Qualquer tamanho permitido pelo EncryptionAlgorithm especificado.|EncryptionLength = 128|
|ProviderName|O nome do provedor é o nome de exibição do CSP...|Se você não souber o nome do provedor do CSP que você está usando, execute certutil – csplist de uma linha de comando. O comando exibirá os nomes de todos os CSPs que estão disponíveis no sistema local|ProviderName = "Microsoft RSA SChannel Cryptographic Provider"|
|ProviderType|O tipo de provedor é usado para selecionar provedores específicos com base na capacidade de algoritmo específico, como "Completa" RSA".|Se você não souber o tipo de provedor do CSP que você está usando, execute certutil – csplist em um prompt de linha de comando. O comando exibirá o tipo de provedor de todos os CSPs que estão disponíveis no sistema local.|ProviderType = 1|
|RenewalCert|Se você precisar renovar um certificado que existe no sistema em que a solicitação de certificado é gerada, você deve especificar o hash de certificado como o valor para essa chave.|O hash de certificado de qualquer certificado que está disponível no computador em que a solicitação de certificado é criada. Se você não souber o hash de certificado, use o Snap-In de MMC de certificados e examine o certificado que deve ser renovado. Abra as propriedades do certificado e veja o atributo "Impressão digital" do certificado. Renovação de certificado exige um PKCS #7 ou um formato de solicitação CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Observação: Isso torna a solicitação para inscrições em nome de outra solicitação de usuário. A solicitação também deve ser assinada com um certificado de agente de registro ou a autoridade de certificação rejeitará a solicitação. Use-opção de certificado para especificar o certificado de agente de registro.|O nome do solicitante pode ser especificado para solicitações de certificado se o RequestType for definido como PKCS #7 ou CMC. Se o RequestType é definido como PKCS n º 10, essa chave será ignorada. O Requestername só pode ser definido como parte da solicitação. Você não pode manipular o Requestername em uma solicitação pendente.|Domínio \ usuário|Requestername = "Contoso\BSmith"|
|RequestType|Determina o padrão que é usado para gerar e enviar a solicitação de certificado.|PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>CERT – 4</br>SCEP - fd00 (64768)</br>Dica: Esta opção indica que um certificado autoassinado ou emitido por conta própria. Ele não gera uma solicitação, mas em vez disso, um novo certificado e, em seguida, instala o certificado. Autoassinado é o padrão. Especifique um certificado de assinatura usando a cert opção – para criar um certificado emitido por conta própria que não é autoassinado.|RequestType = CMC|
|SecurityDescriptor</br>Dica: Isso é relevante apenas para as chaves não é do cartão inteligente de contexto do computador.|Contém as informações de segurança associadas a objetos protegíveis. Para objetos protegíveis mais, você pode especificar o descritor de segurança de um objeto na chamada de função que cria o objeto.|Cadeias de caracteres com base no [linguagem de definição do descritor de segurança](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P(A;;GA;;;SY)(A;;GA;;;BA)"|
|AlternateSignatureAlgorithm|Especifica e recupera um valor booliano que indica se o identificador de objeto de algoritmo de assinatura (OID) para uma assinatura de solicitação ou de certificado PKCS n º 10 é discreto ou combinados.|verdadeiro, FALSO|Alternatesignaturealgorithm=1 = false</br>Dica: Para uma assinatura RSA, falso indica um Pkcs1 v1.5. Verdadeiro indica uma assinatura de v 2.1.|
|Silencioso|Por padrão, essa opção permite que o CSP acessem as informações de solicitação e de área de trabalho do usuário interativo como um PIN do cartão inteligente do usuário. Se essa chave é definida como TRUE, o CSP não deve interagir com a área de trabalho e será impedido de exibir qualquer interface do usuário para o usuário.|verdadeiro, FALSO|Silenciosa = true|
|SMIME|Se esse parâmetro for definido como TRUE, uma extensão com o valor do identificador de objeto 1.2.840.113549.1.9.15 é adicionada à solicitação. O número de identificadores de objeto depende na versão do sistema operacional instalado e funcionalidade do CSP, que se referem a algoritmos de criptografia simétrica que podem ser usados por aplicativos Secure Multipurpose Internet Mail Extensions (S/MIME), como o Outlook.|verdadeiro, FALSO|SMIME = true|
|UseExistingKeySet|Esse parâmetro é usado para especificar que um par de chaves existente deve ser usado na criação de uma solicitação de certificado. Se essa chave é definida como TRUE, você também deve especificar um valor para a chave RenewalCert ou o nome de KeyContainer. Você não deve definir a chave exportável porque você não pode alterar as propriedades de uma chave existente. Nesse caso, nenhum material de chave é gerada quando a solicitação de certificado é criada.|verdadeiro, FALSO|UseExistingKeySet = true|
|KeyProtection|Especifica um valor que indica como uma chave privada é protegida antes do uso.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Especifica um valor booliano que indica se os atributos e as extensões padrão são incluídos na solicitação. Os padrões são representados por seus identificadores de objeto (OIDs).|verdadeiro, FALSO|SuppressDefaults = true|
|FriendlyName|Um nome amigável para o novo certificado.|Text|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>Observação: Isso é usado somente quando a solicitação de tipo = cert.|Especifica um número de unidades a ser usado com ValidityPeriod.|Numeric|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Observação: Isso é usado somente quando a solicitação de tipo = cert.|VValidityPeriod deve ser um inglês dos EUA no plural período de tempo.|Anos, meses, semanas, dias, horas, minutos, segundos|ValidityPeriod = anos|

Retorne ao [conteúdo](#BKMK_Contents)

**[Extensions]**

Esta seção é opcional.


|  Extensão de OID   | Definição | Valor |                                                                                                                                                                                                                                                                                                                                                                                                                      Exemplo                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN=User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *Continue* = "EMail =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS=host.domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DirectoryName=CN=Name,DC=Domain,DC=com&"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *Continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress=10.0.0.1&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId=1.2.3.4.5&"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1={utf8}String&"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2={octet}AAECAwQFBgc=&"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37="{text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continue*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{text}ca=0pathlength=3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     Crítico     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Crítico = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE -- 0</br>AT_SIGNATURE – 2</br>AT_KEYEXCHANGE -- 1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>CERT – 4</br>SCEP - fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     KeyUsage     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE – 80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE – 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE – 8000 (32768)                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  KeyProtection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG -- 0</br>NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  modelo  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH – 80000000 (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL – 20000000 (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID – 1000000 (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL – 4000000 (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE -- 0</br>CERT_OID_NAME_STR -- 2</br>CERT_X500_NAME_STR -- 3</br>CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)</br>CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)</br>CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)</br>CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)</br>CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152) |

Retorne ao [conteúdo](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags permite que o arquivo INF especificar quais campos de assunto e SubjectAltName extensão devem ser preenchida automaticamente pelo certreq com base no usuário atual ou propriedades da máquina atual: Nome DNS, UPN e assim por diante. Usar o literal "template" significa que os sinalizadores de nome de modelo são usados em vez disso. Isso permite que um único arquivo INF a ser usado em vários contextos para gerar solicitações com informações de entidade de contexto específico.
>
> X500NameFlags Especifica os sinalizadores a serem passados diretamente para a API CertStrToName quando o valor de chaves da entidade INF é convertido em uma ASN.1 codificado nome distinto.

Para solicitar um certificado com base usando certreq-novo, use as etapas de exemplo a seguir:

> [!WARNING]
> O conteúdo deste tópico baseia-se nas configurações padrão para o Windows Server 2008 AD CS; Por exemplo, definindo o comprimento da chave como 2048, selecionando o Microsoft Software Key Storage Provider como o CSP e, usando o Secure Hash Algorithm 1 (SHA1). Avalie essas seleções em relação as requisitos de política de segurança da sua empresa.

Para criar uma cópia do arquivo de política (. inf) e salve o exemplo a seguir no bloco de notas e salve como RequestConfig.inf:
```
[NewRequest] 
Subject = "CN=<FQDN of computer you are creating the certificate>" 
Exportable = TRUE 
KeyLength = 2048 
KeySpec = 1 
KeyUsage = 0xf0 
MachineKeySet = TRUE 
[RequestAttributes]
CertificateTemplate="WebServer"
[Extensions] 
OID = 1.3.6.1.5.5.7.3.1 
OID = 1.3.6.1.5.5.7.3.2  
```
No computador para o qual você está solicitando um certificado, digite o comando a seguir:
```
CertReq –New RequestConfig.inf CertRequest.req 
```
O exemplo a seguir demonstra como implementar a sintaxe de seção [Strings] para OIDs e outros difíceis de interpretar os dados. O exemplo de sintaxe novo {text} para extensão EKU, que usa uma lista separada por vírgulas dos OIDs:
```
[Version]
Signature="$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1"
szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

[NewRequest]
Subject = "CN=TestSelfSignedCert"
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%="{text}%szOID_PKIX_KP_SERVER_AUTH%,"
_continue_ = "%szOID_PKIX_KP_CLIENT_AUTH%"
```
Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_accept"></a>CertReq-aceitar

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– Aceitar o parâmetro vincula-se a chave privada gerada anteriormente com o certificado emitido e remove a solicitação de certificado pendente do sistema em que o certificado é solicitado (se houver uma solicitação correspondente).

Você pode usar este exemplo para aceitar manualmente um certificado:
```
certreq -accept certnew.cer 
```

> [!WARNING]
> -Aceitar verbo, a - opções de usuário e – machine indicam se o certificado que está sendo instalado deve ser instalado no contexto do computador ou de usuário. Se houver uma solicitação pendente em qualquer contexto que corresponde à chave pública que está sendo instalada, essas opções não são necessários. Se não houver nenhuma solicitação pendente, um deles deve ser especificado.

Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_policy"></a>CertReq-política

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   O arquivo de configuração que define as restrições que são aplicadas a um certificado de autoridade de certificação quando subordinação qualificada é definida é chamado Policy...
-   Você pode encontrar um exemplo do arquivo Policy na [Apêndice A de Planejando e implementando certificação cruzada e subordinação qualificada](https://technet.microsoft.com/library/cc738878(WS.10).aspx) white paper.
-   Se você digitar o certreq-política sem nenhum parâmetro adicional ele abrirá uma janela de diálogo para que você possa selecionar o solicitado CAM (req, cmc, txt, der, cer ou crt). Depois de selecionar o arquivo solicitado e clique em botão Abrir, outra janela da caixa de diálogo será aberta para selecionar o arquivo INF.

Você pode usar este exemplo para criar uma solicitação de certificado cruzado:
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq -sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Se você digitar o certreq-sinal sem nenhum parâmetro adicional ele abrirá uma janela de caixa de diálogo para que você possa selecionar o arquivo solicitado (req, cmc, txt, der, cer ou crt).
-   A solicitação de subordinação qualificada de assinatura pode exigir credenciais de administrador corporativo. Isso é uma prática recomendada para emitir certificados de autenticação para subordinação qualificada.
-   O certificado usado para assinar a solicitação de subordinação qualificada é criado usando o modelo de subordinação qualificada. Administradores de empresa precisará assinar a solicitação ou conceder permissões de usuário para os indivíduos que assinará o certificado.
-   Quando você assina a solicitação CMC, você precisa ter vários membros da equipe assinar essa solicitação, dependendo do nível de garantia que está associado com a subordinação qualificada.
-   Se a AC pai da autoridade de certificação subordinada qualificado você está instalando estiver offline, você deve obter o certificado de autoridade de certificação para a autoridade de certificação do pai offline. Se a AC pai estiver online, especifique o certificado de autoridade de certificação para a autoridade de certificação durante o Assistente de instalação de serviços de certificado.

A sequência de comandos a seguir mostra como criar uma nova solicitação de certificado, assiná-lo e enviá-lo:
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq -enroll

Para registrar um certificado
```
certreq –enroll [Options] TemplateName
```
Para renovar um certificado existente
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
Somente você pode renovar certificados de tempo válido. Certificados expirados não podem ser renovados e devem ser substituídos por um novo certificado.

Veja um exemplo de renovar um certificado usando seu número de série:
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Aqui, um exemplo de registro para um modelo de certificado chamado servidor Web usando o asterisco (*) para selecionar o servidor de políticas por meio de U/i:
```
certreq -enroll –machine –policyserver * "WebServer"
```
Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Options"></a>Opções

|Opções|Descrição|
|-------|-----------|
|-qualquer|Força ICertRequest:: Enviar para determinar o tipo de codificação.|
|-attrib \<Sequência_do_atributo >|Especifica os pares de cadeia de caracteres de nome e valor, separados por dois-pontos.</br>Cadeia de caracteres separada de nome e valor pares com \n (por exemplo, Name1:Value1\nName2:Value2).|
|-binário|Formatos de arquivos de saída como binários em vez de codificada em base64.|
|-PolicyServer *\<PolicyServer>*|"ldap:  *\<caminho >* "</br>Insira o URI ou uma ID exclusiva para um computador que executa o serviço Web de política de registro de certificado.</br>Para especificar que você deseja usar um arquivo de solicitação navegando, basta usar um sinal de subtração (-) Inscreva  *\<policyserver >* .|
|-config \<ConfigString>|Processa a operação usando a CA especificada na cadeia de configuração, que é CAHostName\CAName. Para uma conexão https, especifique o URI do servidor de registro. Para a máquina local de armazenamento da autoridade de certificação, use um sinal de subtração (-) de entrada.|
|-Anônima|Use credenciais anônimas para serviços de Web de registro de certificado.|
|-Kerberos|Use as credenciais do Kerberos (domínio) para serviços de Web de registro de certificado.|
|-ClientCertificate *\<ClientCertId>*|Você pode substituir a  *\<ClientCertID >* com uma impressão digital do certificado, CN, EKU, modelo, email, UPN e o nome do novo valor sintaxe =.|
|-UserName  *\<nome de usuário >*|Usado com serviços de Web de registro de certificado. Você pode substituir  *\<nome de usuário >* com o nome SAM ou domínio \ usuário. Essa opção é para uso com a opção -p.|
|-p  *\<senha >*|Usado com serviços de Web de registro de certificado. SUBSTITUTE  *\<senha >* com a senha do usuário real. Essa opção é para uso com a opção - nome de usuário.|
|-usuário|Configura o - contexto do usuário para uma nova solicitação de certificado ou especifica o contexto para um uma aceitação de certificado. Este é o contexto de padrão, se nenhum for especificado no INF ou no modelo.|
|-machine|Configura uma nova solicitação de certificado ou especifica o contexto para um uma aceitação de certificado para o contexto do computador. Para novas solicitações deve ser consistente com a chave MachineKeyset INF e o contexto do modelo. Se essa opção não for especificada e o modelo não define um contexto, o padrão é o contexto do usuário.|
|-crl|Inclui listas de certificados revogados (CRLs) na saída para o arquivo PKCS #7 codificado na base64 especificado pelo CertChainFileOut ou para o arquivo codificado em base64 especificado por RequestFileOut.|
|-rpc|Instrui os serviços de certificados do Active Directory (AD CS) para usar uma conexão de servidor RPC (chamada) de procedimento remoto em vez de com distribuído.|
|-AdminForceMachine|Use a chave de serviço ou a representação para enviar a solicitação no contexto do sistema Local. Requer que o usuário chamar essa opção seja um membro do grupo Local administradores.|
|-RenewOnBehalfOf|Envie uma renovação em nome do sujeito identificado no certificado de assinatura. Isso define CR_IN_ROBO ao chamar [ICertRequest:: enviar](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Força arquivos existentes sejam substituídos. Isso também ignora o cache de modelos e política.|
|-q|Use o modo silencioso; suprima todos os prompts interativos.|
|-Unicode|Grava saída Unicode quando a saída padrão é redirecionada ou enviado por pipe para outro comando, que ajuda quando invocada a partir de scripts Windows PowerShell®).|
|-UnicodeText|Envia a saída de Unicode ao gravar o texto de base64 codificada em blobs de dados e arquivos.|

Retorne ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formatos

|Formatos|Descrição|
|-------|-----------|
|RequestFileIn|Nome do arquivo de entrada codificada em Base64 ou binário: Solicitação de certificado PKCS n º 10, solicitação de certificado CMS, solicitação de renovação de certificado PKCS n º 7, certificado x. 509 seja certificação cruzada ou solicitação de certificado de formato de marca KeyGen.|
|RequestFileOut|Nome do arquivo de saída codificada em Base64|
|CertFileOut|Nome do arquivo x-509 codificado na Base64.|
|PKCS10FileOut|Para uso com o [Certreq-política](#BKMK_policy) apenas o verbo. Codificado na Base64 PKCS10 nome do arquivo.|
|CertChainFileOut|Nome do arquivo PKCS #7 codificado na Base64.|
|FullResponseFileOut|Nome do arquivo de resposta completa codificado na Base64.|
|PolicyFileIn|Para uso com o [Certreq-política](#BKMK_policy) apenas o verbo. Arquivo INF que contém uma representação textual das extensões usadas para qualificar uma solicitação.|

## <a name="BKMK_Examples"></a>Exemplos adicionais certreq

Os artigos a seguir contêm exemplos de uso do certreq:
-   [Como solicitar um certificado com um nome alternativo da entidade personalizado](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guia de laboratório de teste: Implantando uma hierarquia PKI do AD CS com duas camadas](https://technet.microsoft.com/library/hh831348.aspx)
-   [Apêndice 3: Sintaxe de certreq.exe](https://technet.microsoft.com/library/cc736326.aspx)
-   [Como criar um certificado SSL do servidor web manualmente](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Solicitar um certificado usando uma autoridade de certificação do Windows Server 2008 de provisionamento AMT](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Registro de certificado para o agente do System Center Operations Manager](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guia passo a passo do AD CS: Implantação de hierarquia PKI de duas camadas](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Como habilitar LDAP sobre SSL com uma autoridade de certificação de terceiros](https://support.microsoft.com/kb/321051)

Retorne ao [conteúdo](#BKMK_Contents)
