---
title: certreq
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3098cb12379493a82c77412b2328f5312afb2c0c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379682"
---
# <a name="certreq"></a>certreq



O Certreq pode ser usado para solicitar certificados de uma autoridade de certificação (CA), para recuperar uma resposta a uma solicitação anterior de uma autoridade de certificação, para criar uma nova solicitação de um arquivo. inf, para aceitar e instalar uma resposta a uma solicitação, para construir uma certificação cruzada ou solicitação de subordinação qualificada de um certificado ou solicitação de autoridade de certificação existente e para assinar uma solicitação de certificação cruzada ou de subordinação qualificada.

> [!WARNING]
> - As versões anteriores do Certreq podem não fornecer todas as opções descritas neste documento. Você pode ver todas as opções que uma versão específica do Certreq fornece executando os comandos mostrados na seção de notações de sintaxe.
> - O Certreq não oferece suporte à criação de uma nova solicitação de certificado com base em um modelo de atestado de chave quando estiver em um ambiente de CEP/CES

## <a name="BKMK_Contents"></a>Índice

As principais seções deste artigo são as seguintes:
1.  [Verbos](#BKMK_Verbs)
2.  [Notações de sintaxe](#BKMK_notation)
3.  [Opções](#BKMK_Options)
4.  [Forma](#BKMK_Formats)
5.  [Exemplos adicionais de certreq](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>Verbos

A tabela a seguir descreve os verbos que podem ser usados com o comando certreq

|Alternar|Descrição|
|------|-----------|
|-Enviar|Envia uma solicitação para uma autoridade de certificação. Para obter mais informações, consulte [Certreq-Submit](#BKMK_Submit).|
|-recuperar *RequestId*|Recupera uma resposta a uma solicitação anterior de uma autoridade de certificação. Para obter mais informações, consulte [certreq-retrieve](#BKMK_Retrieve).|
|-Novo|Cria uma nova solicitação de um arquivo. inf. Para obter mais informações, consulte [Certreq-New](#BKMK_New).|
|-Aceitar|Aceita e instala uma resposta a uma solicitação de certificado. Para obter mais informações, consulte [certreq-accept](#BKMK_accept).|
|-Política|Define a política para uma solicitação. Para obter mais informações, consulte [certreq-policy](#BKMK_policy).|
|-Assinar|Assina uma solicitação de certificação cruzada ou de subordinação qualificada. Para obter mais informações, consulte [Certreq-Sign](#BKMK_sign).|
|-Registrar|Registra ou renova um certificado. Para obter mais informações, consulte [Certreq-Enroll](#BKMK_enroll).|
|-?|Exibe uma lista de sintaxe, opções e descrições de Certreq.|
|> de verbo-?  *\<*|Exibe a ajuda para o verbo especificado.|
|-v-?|Exibe uma lista detalhada da sintaxe, das opções e das descrições do Certreq.|

Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_notation"></a>Notações de sintaxe

-   Para obter a sintaxe básica de linha de comando, execute`certreq -?`
-   Para obter a sintaxe sobre como usar o Certutil com um verbo específico, execute o  *\<> do verbo* **Certreq** **-?**
-   Para enviar toda a sintaxe de certutil para um arquivo de texto, execute os seguintes comandos:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

A tabela a seguir descreve a notação usada para indicar a sintaxe da linha de comando.

|Notation|Descrição|
|--------|-----------|
|Texto sem colchetes ou chaves|Itens que você deve digitar, conforme mostrado|
|\<Texto dentro de colchetes angulares >|Espaço reservado para o qual você deve fornecer um valor|
|[Texto dentro de colchetes]|Itens opcionais|
|{Texto dentro de chaves}|Conjunto de itens necessários; Escolha um|
|Barra vertical (&#124;)|Separador para itens mutuamente exclusivos; Escolha um|
|Reticências (…)|Itens que podem ser repetidos|

Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-enviar

Esse é o parâmetro Certreq. exe padrão, se nenhuma opção for especificada explicitamente no prompt de linha de comando, o Certreq. exe tentará enviar uma solicitação de certificado a uma AC.
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
Você deve especificar um arquivo de solicitação de certificado ao usar a opção – Submit. Se esse parâmetro for omitido, uma janela de abertura de arquivo comum será exibida, onde você poderá selecionar o arquivo de solicitação de certificado apropriado.

Você pode usar esses exemplos como um ponto de partida para criar sua solicitação de envio de certificado:

Para enviar uma solicitação de certificado simples, use o exemplo a seguir:
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
Para solicitar um certificado especificando o atributo SAN, consulte as etapas detalhadas no artigo 931351 da base de dados de conhecimento Microsoft [como adicionar um nome alternativo da entidade a um certificado LDAP seguro](https://support.microsoft.com/kb/931351) no "como usar o utilitário Certreq. exe para criar e enviar um solicitação de certificado que inclui uma SAN ".

Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-recuperar

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   Se você não especificar a caixa de diálogo CAComputerName ou CAName in-config CAComputerName\CANamea será exibida e exibirá uma lista de todas as CAs disponíveis.
-   Se você usar-config-em vez de-config CAComputerName\CAName, a operação será processada usando a autoridade de certificação padrão.
-   Você pode usar certreq-retrieve *RequestId* para recuperar o certificado depois que a CA realmente o emitir. O *RequestId*PKC pode ser um decimal ou hex com o prefixo 0x e pode ser um número de série do certificado sem prefixo 0x. Você também pode usá-lo para recuperar qualquer certificado que já tenha sido emitido pela autoridade de certificação, incluindo certificados revogados ou expirados, sem considerar se a solicitação do certificado já estava no estado pendente.
-   Se você enviar uma solicitação para a autoridade de certificação, o módulo de política da autoridade de certificação poderá deixar a solicitação em um estado pendente e retornar a *RequestId* para o chamador CertReq para exibição. Eventualmente, o administrador da autoridade de certificação emitirá o certificado ou negará a solicitação.

O comando a seguir recupera a ID de certificado 20 e cria o arquivo de certificado (. cer):
```
certreq -retrieve 20 MyCertificate.cer 
```
Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-novo

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
Como o arquivo INF permite que um conjunto avançado de parâmetros e opções seja especificado, é difícil definir um modelo padrão que os administradores devem usar para todas as finalidades. Portanto, esta seção descreve todas as opções para permitir que você crie um arquivo INF personalizado para suas necessidades específicas. As palavras-chave a seguir são usadas para descrever a estrutura do arquivo INF.
1.  Uma *seção* é uma área no arquivo inf que abrange um grupo lógico de chaves. Uma seção sempre aparece entre colchetes no arquivo INF.
2.  Uma *chave* é o parâmetro que está à esquerda do sinal de igual.
3.  Um *valor* é o parâmetro que está à direita do sinal de igual.

Por exemplo, um arquivo INF mínimo seria semelhante ao seguinte:
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
A seguir estão algumas das seções possíveis que podem ser adicionadas ao arquivo INF:

**[NewRequest]**

Esta seção é obrigatória para um arquivo INF que atua como um modelo para uma nova solicitação de certificado. Esta seção requer pelo menos uma chave com um valor.

|Chave|Definição|Valor|Exemplo|
|---|----------|-----|-------|
|Subject|Vários aplicativos dependem das informações da entidade em um certificado. Portanto, é recomendável que um valor para essa chave seja especificado. Se o assunto não estiver definido aqui, é recomendável que um nome de entidade seja incluído como parte da extensão de certificado de nome alternativo da entidade.|Valores de cadeia de caracteres do nome distinto relativo|Subject = "CN = Computador1. contoso. com" Subject = "CN = John Smith, CN = Users, DC = contoso, DC = com"|
|Exportável|Se esse atributo for definido como TRUE, a chave privada poderá ser exportada com o certificado. Para garantir um alto nível de segurança, as chaves privadas não devem ser exportáveis; no entanto, em alguns casos, pode ser necessário tornar a chave privada exportável se vários computadores ou usuários precisarem compartilhar a mesma chave privada.|true, false|Exportável = TRUE. As chaves CNG podem distinguir entre esse e o texto não criptografado exportável. As chaves CAPI1 não podem.|
|ExportableEncrypted|Especifica se a chave privada deve ser definida para ser exportável.|true, false|ExportableEncrypted = true</br>Dica: Nem todos os tamanhos de chave pública e algoritmos funcionarão com todos os algoritmos de hash. O CSP especificado de Tamehe também deve oferecer suporte ao algoritmo de hash especificado. Para ver a lista de algoritmos de hash com suporte, você pode executar o comando<code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|Algoritmo de hash a ser usado para esta solicitação.|SHA256, Sha384, SHA512, SHA1, MD5, MD4, MD2|HashAlgorithm = SHA1. Para ver a lista de algoritmos de hash com suporte, use: &#124; certutil- &#124; OID 1 findstr pwszCNGAlgid findstr/v CryptOIDInfo|
|KeyAlgorithm|O algoritmo que será usado pelo provedor de serviços para gerar um par de chaves pública e privada.|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|Não é recomendável definir esse parâmetro para novas solicitações em que o novo material de chave é gerado. O contêiner de chave é automaticamente gerado e mantido pelo sistema. Para solicitações em que o material de chave existente deve ser usado, esse valor pode ser definido como o nome do contêiner de chave da chave existente. Use o comando certutil – Key para exibir a lista de contêineres de chave disponíveis para o contexto do computador. Use o comando certutil – Key – User para o contexto do usuário atual.|Valor de cadeia de caracteres aleatória</br>Dica: Você deve usar aspas duplas em qualquer valor de chave INF que tenha espaços em branco ou caracteres especiais para evitar possíveis problemas de análise de INF.|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength da|Define o comprimento da chave pública e privada. O comprimento da chave tem um impacto sobre o nível de segurança do certificado. O comprimento de chave maior geralmente fornece um nível de segurança mais alto; no entanto, alguns aplicativos podem ter limitações em relação ao comprimento da chave.|Qualquer comprimento de chave válido com suporte do provedor de serviços de criptografia.|KeyLength = 2048|
|KeySpec|Determina se a chave pode ser usada para assinaturas, para o Exchange (criptografia) ou para ambos.|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|Uso de|Define a que a chave de certificado deve ser usada.|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>Dica: Os valores mostrados são valores hexadecimais (decimais) para cada definição de bit. A sintaxe mais antiga também pode ser usada: um único valor hexadecimal com vários bits definidos, em vez da representação simbólica. Por exemplo, KeyUsage = 0XA0.</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>Dica: Vários valores usam um separador de símbolo de pipe (&#124;). Certifique-se de usar aspas duplas ao usar vários valores para evitar problemas de análise de INF.|
|Keyutilizaproperty|Recupera um valor que identifica a finalidade específica para a qual uma chave privada pode ser usada.|NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeyset|Essa chave é importante quando você precisa criar certificados que pertencem à máquina e não um usuário. O material da chave gerado é mantido no contexto de segurança da entidade de segurança (conta de usuário ou computador) que criou a solicitação. Quando um administrador cria uma solicitação de certificado em nome de um computador, o material da chave deve ser criado no contexto de segurança da máquina e não no contexto de segurança do administrador. Caso contrário, o computador não poderá acessar sua chave privada, pois ela estaria no contexto de segurança do administrador.|true, false|MachineKeyset = true</br>Dica: O padrão é False.|
|NotBefore|Especifica uma data ou data e hora antes da qual a solicitação não pode ser emitida. Não é possível usar não antes de ValidityPeriod e ValidityPeriodUnits.|Data ou data e hora|Não antes de = "7/24/2012 10:31 AM"</br>Dica: Não before e não After são somente para RequestType = CERT. As tentativas de análise de data são sensíveis à localidade. O uso de nomes de meses causará a ambiguidade e deverá funcionar em todas as localidades.|
|NotAfter|Especifica uma data ou data e hora após a qual a solicitação não pode ser emitida. Não é possível usar não após ValidityPeriod ou ValidityPeriodUnits.|Data ou data e hora|Não After = "9/23/2014 10:31 AM"</br>Dica: Não before e não After são somente para RequestType = CERT. As tentativas de análise de data são sensíveis à localidade. O uso de nomes de meses causará a ambiguidade e deverá funcionar em todas as localidades.|
|PrivateKeyArchive|A configuração PrivateKeyArchive só funcionará se o RequestType correspondente estiver definido como "CMC" porque somente o formato de solicitação do CMS (mensagens de gerenciamento de certificado por meio da CMC) permite transferir com segurança a chave privada do solicitante para a autoridade de arquivamento de chave.|true, false|PrivateKeyArchive = true|
|EncryptionAlgorithm|O algoritmo de criptografia a ser usado.|As opções possíveis variam, dependendo da versão do sistema operacional e do conjunto de provedores criptográficos instalados. Para ver a lista de algoritmos disponíveis, execute o <code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> comando o CSP especificado também deve oferecer suporte ao algoritmo de criptografia simétrica e ao comprimento especificados.|EncryptionAlgorithm = 3DES|
|EncryptionLength|Comprimento do algoritmo de criptografia a ser usado.|Qualquer comprimento permitido pelo EncryptionAlgorithm especificado.|EncryptionLength = 128|
|ProviderName|O nome do provedor é o nome de exibição do CSP.|Se você não souber o nome do provedor do CSP que está usando, execute certutil – csplist de uma linha de comando. O comando exibirá os nomes de todos os CSPs disponíveis no sistema local|ProviderName = "provedor criptográfico do Microsoft RSA SChannel"|
|ProviderType|O tipo de provedor é usado para selecionar provedores específicos com base no recurso de algoritmo específico, como "RSA Full".|Se você não souber o tipo de provedor do CSP que está usando, execute o Certutil – csplist de um prompt de linha de comando. O comando exibirá o tipo de provedor de todos os CSPs disponíveis no sistema local.|ProviderType = 1|
|RenewalCert|Se precisar renovar um certificado que existe no sistema em que a solicitação de certificado é gerada, você deve especificar seu hash de certificado como o valor para essa chave.|O hash de certificado de qualquer certificado disponível no computador em que a solicitação de certificado é criada. Se você não souber o hash de certificado, use o snap-in do MMC de certificados e examine o certificado que deve ser renovado. Abra as propriedades do certificado e veja o atributo "impressão digital" do certificado. A renovação de certificado requer um formato de solicitação PKCS n º 7 ou CMC.|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>Observação: Isso faz com que a solicitação seja registrada em nome de outra solicitação do usuário. A solicitação também deve ser assinada com um certificado de agente de registro ou a autoridade de certificação rejeitará a solicitação. Use a opção-CERT para especificar o certificado do agente de registro.|O nome do solicitante pode ser especificado para solicitações de certificado se o RequestType for definido como PKCS # 7 ou CMC. Se o RequestType for definido como PKCS # 10, essa chave será ignorada. O Requestername só pode ser definido como parte da solicitação. Você não pode manipular o Requestername em uma solicitação pendente.|Usuário|Requestername = "Contoso\BSmith"|
|RequestType|Determina o padrão usado para gerar e enviar a solicitação de certificado.|PKCS10--1</br>PKCS7--2</br>CMC--3</br>Certificado--4</br>SCEP--fd00 (64768)</br>Dica: Essa opção indica um certificado autoassinado ou emitido por conta própria. Ele não gera uma solicitação, mas sim um novo certificado e, em seguida, instala o certificado. Auto-assinado é o padrão. Especifique um certificado de assinatura usando a opção – CERT para criar um certificado autoemitido que não tenha assinatura automática.|RequestType = CMC|
|SecurityDescriptor</br>Dica: Isso é relevante apenas para chaves de cartão não inteligente de contexto de computador.|Contêm as informações de segurança associadas a objetos protegíveis. Para a maioria dos objetos protegíveis, você pode especificar o descritor de segurança de um objeto na chamada de função que cria o objeto.|Cadeias de caracteres baseadas na [linguagem de definição do descritor de segurança](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx).|SecurityDescriptor = "D:P (A;; GA;;; SY) (A;; GA;;; BA) "|
|AlternateSignatureAlgorithm|Especifica e recupera um valor booliano que indica se o OID (identificador de objeto) de algoritmo de assinatura para uma solicitação PKCS # 10 ou assinatura de certificado é discreto ou combinado.|true, false|AlternateSignatureAlgorithm = false</br>Dica: Para uma assinatura RSA, false indica um Pkcs1 v 1.5. Verdadeiro indica uma assinatura v 2.1.|
|Silencioso|Por padrão, essa opção permite que o CSP acesse a área de trabalho do usuário interativo e solicite informações como um PIN do cartão inteligente do usuário. Se essa chave for definida como TRUE, o CSP não deverá interagir com a área de trabalho e será impedido de exibir qualquer interface do usuário para o usuário.|true, false|Silent = true|
|SMIME|Se esse parâmetro for definido como TRUE, uma extensão com o valor do identificador de objeto 1.2.840.113549.1.9.15 será adicionada à solicitação. O número de identificadores de objeto depende do na versão do sistema operacional instalada e do recurso CSP, que se refere aos algoritmos de criptografia simétrica que podem ser usados por aplicativos Secure Multipurpose Internet Mail Extensions (S/MIME), como o Outlook.|true, false|SMIME = verdadeiro|
|UseExistingKeySet|Esse parâmetro é usado para especificar que um par de chaves existente deve ser usado na criação de uma solicitação de certificado. Se essa chave for definida como TRUE, você também deverá especificar um valor para a chave RenewalCert ou o nome do keycontainer. Você não deve definir a chave exportável porque não pode alterar as propriedades de uma chave existente. Nesse caso, nenhum material da chave é gerado quando a solicitação de certificado é criada.|true, false|UseExistingKeySet = true|
|Proteção contra keyprotection|Especifica um valor que indica como uma chave privada é protegida antes do uso.|XCN_NCRYPT_UI_NO_PROTCTION_FLAG--0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG--1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2|Proteção contra keyprotection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|Especifica um valor booliano que indica se as extensões e os atributos padrão estão incluídos na solicitação. Os padrões são representados por seus OIDs (identificadores de objeto).|true, false|SuppressDefaults = true|
|FriendlyName|Um nome amigável para o novo certificado.|Text|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>Observação: Isso é usado somente quando o tipo de solicitação = CERT.|Especifica um número de unidades que deve ser usado com ValidityPeriod.|Numeric|ValidityPeriodUnits = 3|
|ValidityPeriod</br>Observação: Isso é usado somente quando o tipo de solicitação = CERT.|VValidityPeriod deve ser um período de tempo no inglês dos EUA.|Anos, meses, semanas, dias, horas, minutos, segundos|ValidityPeriod = anos|

Retornar ao [conteúdo](#BKMK_Contents)

**WMZ**

Esta seção é opcional.


|  OID de extensão   | Definição | Valor |                                                                                                                                                                                                                                                                                                                                                                                                                      Exemplo                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "email =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS = host. domain. com &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DIRECTORYNAME = CN = Name, DC = Domain, DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress = 10.0.0.1 &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "registeredid = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1 = {UTF8} cadeia de caracteres &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2 = {octet} AAECAwQFBgc = &"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2 = {octeto} {hex} 00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3 = {ASN} BAgAAQIDBAUGBw = = &"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3 = {hex} 04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7.                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *continua*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continuar* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{Text} AC = 0pathlength = 3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     Crítica     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 Crítico = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE--0</br>AT_SIGNATURE--2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10--1</br>PKCS7--2</br>CMC--3</br>Certificado--4</br>SCEP--fd00 (64768)                                                                                                                                                                                                                                                                                                                                                                                   |
|     Uso de     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE--40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 (32768)                                                                                                                                                                                                       |
| Keyutilizaproperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--FFFFFF (16777215)                                                                                                                                                                                                                                                                                                                                             |
|  Proteção contra keyprotection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG--0</br>NCRYPT_UI_PROTECT_KEY_FLAG--1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  modelo  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME--40 MILHÕES (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH--80 MILHÕES (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN--10 MILHÕES (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL--20 MILHÕES (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME--8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1 MILHÃO (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8 MILHÕES (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL--4 MILHÕES (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN--800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2 MILHÕES (33554432)                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE--0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG--40 MILHÕES (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG--20 MILHÕES (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG--10 MILHÕES (268435456)</br>CERT_NAME_STR_CRLF_FLAG--8 MILHÕES (134217728)</br>CERT_NAME_STR_COMMA_FLAG--4 MILHÕES (67108864)</br>CERT_NAME_STR_REVERSE_FLAG--2 MILHÕES (33554432)</br>CERT_NAME_STR_FORWARD_FLAG--1 MILHÃO (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG--10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG--20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG--40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG--80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG--100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG--200000 (2097152) |

Retornar ao [conteúdo](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags permite que o arquivo INF especifique quais campos de extensão de assunto e SubjectAltName devem ser preenchidos automaticamente pelo Certreq com base no usuário atual ou nas propriedades do computador atual: Nome DNS, UPN e assim por diante. Usar o "modelo" literal significa que os sinalizadores de nome de modelo são usados. Isso permite que um único arquivo INF seja usado em vários contextos para gerar solicitações com informações de assunto específicas do contexto.
>
> X500NameFlags especifica os sinalizadores a serem passados diretamente para a API do CertStrToName quando o valor das chaves INF do assunto é convertido em um nome diferenciado com codificação ASN. 1.

Para solicitar um certificado baseado usando Certreq-New, use as etapas do exemplo abaixo:

> [!WARNING]
> O conteúdo deste tópico baseia-se nas configurações padrão do Windows Server 2008 AD CS; por exemplo, definindo o comprimento da chave como 2048, selecionando provedor de armazenamento de chaves de software da Microsoft como o CSP e usando Secure Hash Algorithm 1 (SHA1). Avalie essas seleções em relação aos requisitos da política de segurança da sua empresa.

Para criar uma cópia de arquivo de política (. inf) e salve o exemplo abaixo no bloco de notas e salve como RequestConfig. inf:
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
O exemplo a seguir demonstra a implementação da sintaxe da seção [Strings] para OIDs e outros dados difíceis de interpretar. O novo exemplo de sintaxe de {Text} para extensão EKU, que usa uma lista separada por vírgulas de OIDs:
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
Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq-aceitar

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
O parâmetro – Accept vincula a chave privada gerada anteriormente com o certificado emitido e remove a solicitação de certificado pendente do sistema em que o certificado é solicitado (se houver uma solicitação correspondente).

Você pode usar este exemplo para aceitar um certificado manualmente:
```
certreq -accept certnew.cer 
```

> [!WARNING]
> O verbo-Accept, as opções-user e – Machine indicam se o certificado que está sendo instalado deve ser instalado no contexto do usuário ou da máquina. Se houver uma solicitação pendente em qualquer contexto que corresponda à chave pública que está sendo instalada, essas opções não serão necessárias. Se não houver nenhuma solicitação pendente, um deles deverá ser especificado.

Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-política

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   O arquivo de configuração que define as restrições que são aplicadas a um certificado de autoridade de certificação quando a subordinação qualificada é definida é chamado de Policy. inf..
-   Você pode encontrar um exemplo do arquivo Policy. inf no [Apêndice a de planejando e implementando a certificação cruzada e a subordinação qualificada](https://technet.microsoft.com/library/cc738878(WS.10).aspx) White Paper.
-   Se você digitar o certreq-policy sem nenhum parâmetro adicional, ele abrirá uma janela de diálogo para que você possa selecionar o arquivos solicitado (req, CMC, txt, der, CER ou CRT). Depois de selecionar o arquivo solicitado e clicar no botão abrir, outra janela de diálogo será aberta para selecionar o arquivo INF.

Você pode usar este exemplo para criar uma solicitação de certificado cruzado:
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-assinar

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   Se você digitar o Certreq-Sign sem nenhum parâmetro adicional, ele abrirá uma janela de diálogo para que você possa selecionar o arquivo solicitado (req, CMC, txt, der, CER ou CRT).
-   Assinar a solicitação de subordinação qualificada pode exigir credenciais de administrador corporativo. Essa é uma prática recomendada para emitir certificados de assinatura para subordinação qualificada.
-   O certificado usado para assinar a solicitação de subordinação qualificada é criado usando o modelo de subordinação qualificado. Os administradores de empresa terão que assinar a solicitação ou conceder permissões de usuário para os indivíduos que assinarão o certificado.
-   Ao assinar a solicitação CMC, talvez seja necessário que várias pessoas assinem essa solicitação, dependendo do nível de garantia associado à subordinação qualificada.
-   Se a autoridade de certificação pai da AC subordinada qualificada que você está instalando estiver offline, você deverá obter o certificado de autoridade de certificação para a AC subordinada qualificada do pai offline. Se a AC pai estiver online, especifique o certificado de autoridade de certificação para a autoridade de certificação subordinada qualificada durante o assistente de instalação de serviços de certificados.

A sequência de comandos abaixo mostrará como criar uma nova solicitação de certificado, conectá-la e enviá-la:
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-registrar

Para registrar-se em um certificado
```
certreq –enroll [Options] TemplateName
```
Para renovar um certificado existente
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
Você só pode renovar certificados que são válidos. Os certificados expirados não podem ser renovados e devem ser substituídos por um novo certificado.

Aqui está um exemplo de renovação de um certificado usando seu número de série:
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
Aqui, um exemplo de registro em um modelo de certificado chamado WebServer usando o asterisco (*) para selecionar o servidor de políticas por meio de U/I:
```
certreq -enroll –machine –policyserver * "WebServer"
```
Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Options"></a>Opções

|Opções|Descrição|
|-------|-----------|
|-qualquer|Force ICertRequest:: Submit para determinar o tipo de codificação.|
|-Atrib \<AttributeString >|Especifica os pares de cadeia de caracteres de nome e valor, separados por dois-pontos.</br>Separe os pares de cadeia de caracteres de nome e valor com \n (por exemplo, Nome1: Value1\nName2: value2).|
|-binário|Formata arquivos de saída como binários em vez de codificados em base64.|
|-PolicyServer  *\<PolicyServer >*|"LDAP:  *\<Path >* "</br>Insira o URI ou a ID exclusiva para um computador que executa o Serviço Web de Política de Registro de Certificado.</br>Para especificar que você gostaria de usar um arquivo de solicitação navegando, basta usar um sinal de menos (-) para  *\<policyserver >* .|
|-config \<ConfigString >|Processa a operação usando a autoridade de certificação especificada na cadeia de caracteres de configuração, que é CAHostName\CAName. Para uma conexão HTTPS, especifique o URI do servidor de registro. Para a AC do repositório do computador local, use um sinal de menos (-).|
|-Anônimo|Use credenciais anônimas para serviços Web de registro de certificado.|
|-Kerberos|Use credenciais Kerberos (domínio) para serviços Web de registro de certificado.|
|-ClientCertificate  *\<ClientCertId >*|Você pode substituir o  *\<> ClientCertID* por uma impressão digital do certificado, CN, EKU, modelo, email, UPN e a nova sintaxe name = value.|
|-Nome  *\<de usuário username >*|Usado com serviços Web de registro de certificado. Você pode substituir  *\<username >* pelo nome Sam ou Domain\User. Essa opção é para uso com a opção-p.|
|-p  *\<> de senha*|Usado com serviços Web de registro de certificado. Substitua a senha > pela senha do usuário real.  *\<* Essa opção é para uso com a opção-UserName.|
|-usuário|Configura o contexto do usuário para uma nova solicitação de certificado ou especifica o contexto para uma aceitação de certificado. Esse é o contexto padrão, se nenhum for especificado no INF ou no modelo.|
|-computador|Configura uma nova solicitação de certificado ou especifica o contexto de uma aceitação de certificado para o contexto da máquina. Para novas solicitações, ele deve ser consistente com a chave do MachineKeyset INF e o contexto do modelo. Se essa opção não for especificada e o modelo não definir um contexto, o padrão será o contexto do usuário.|
|-CRL|Inclui listas de certificados revogados (CRLs) na saída para o arquivo de #7 PKCS codificado em base64 especificado por CertChainFileOut ou para o arquivo codificado em base64 especificado por RequestFileOut.|
|-RPC|Instrui Active Directory serviços de certificados (AD CS) para usar uma conexão de servidor RPC (chamada de procedimento remoto) em vez de COM distribuído.|
|-AdminForceMachine|Use o serviço de chave ou a representação para enviar a solicitação do contexto do sistema local. Requer que o usuário que invoca essa opção seja membro de administradores locais.|
|-RenewOnBehalfOf|Envie uma renovação em nome do assunto identificado no certificado de autenticação. Isso define CR_IN_ROBO ao chamar [ICertRequest:: Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|Forçar os arquivos existentes a serem substituídos. Isso também ignora os modelos de cache e a política.|
|-q|Usar o modo silencioso; suprimir todos os prompts interativos.|
|-Unicode|Grava a saída Unicode quando a saída padrão é redirecionada ou canalizada para outro comando, o que ajuda quando é invocado do Windows PowerShell® scripts).|
|-UnicodeText|Envia a saída Unicode ao gravar blobs de dados codificados em texto base64 em arquivos.|

Retornar ao [conteúdo](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Forma

|Forma|Descrição|
|-------|-----------|
|RequestFileIn|Nome do arquivo de entrada binário ou codificado na base64: PKCS #10 solicitação de certificado, solicitação de certificado de CMS, solicitação de renovação de certificado de #7 PKCS, certificado X. 509 para que haja certificação cruzada ou solicitação de certificado de formato de marca KeyGen.|
|RequestFileOut|Nome do arquivo de saída codificado na base64|
|CertFileOut|Nome de arquivo X-509 codificado na base64.|
|PKCS10FileOut|Para uso com somente o verbo [certreq-policy](#BKMK_policy) . Nome do arquivo de saída PKCS10 codificado na base64.|
|CertChainFileOut|Nome de arquivo de #7 PKCS codificado em base64.|
|FullResponseFileOut|Nome de arquivo de resposta completa codificado em base64.|
|PolicyFileIn|Para uso com somente o verbo [certreq-policy](#BKMK_policy) . Arquivo INF que contém uma representação textual das extensões usadas para qualificar uma solicitação.|

## <a name="BKMK_Examples"></a>Exemplos adicionais de certreq

Os artigos a seguir contêm exemplos de uso de Certreq:
-   [Como solicitar um certificado com um nome alternativo da entidade personalizada](https://technet.microsoft.com/library/ff625722.aspx)
-   [Guia de laboratório de teste: Implantando uma hierarquia de PKI de duas camadas do AD CS](https://technet.microsoft.com/library/hh831348.aspx)
-   [Apêndice 3: Sintaxe de Certreq. exe](https://technet.microsoft.com/library/cc736326.aspx)
-   [Como criar um certificado SSL do servidor Web manualmente](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [Solicitar um certificado de provisionamento AMT usando uma AC do Windows Server 2008](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [Registro de certificado para o agente de System Center Operations Manager](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [Guia passo a passo do AD CS: Implantação de hierarquia de PKI de duas camadas](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [Como habilitar LDAP sobre SSL com uma autoridade de certificação de terceiros](https://support.microsoft.com/kb/321051)

Retornar ao [conteúdo](#BKMK_Contents)
