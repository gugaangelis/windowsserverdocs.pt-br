---
title: certreq
description: Artigo de referência para o comando certreq, que solicita certificados de uma autoridade de certificação (CA), recupera uma resposta a uma solicitação anterior de uma CA, cria uma nova solicitação de um arquivo. inf, aceita e instala uma resposta a uma solicitação, constrói uma solicitação de certificação cruzada ou de subordinação qualificada de um certificado ou solicitação de autoridade de certificação existente e assina uma solicitação de certificação cruzada ou de subordinação qualificada
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3beb043272de304edfcac294bc9b831a60b1003
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992997"
---
# <a name="certreq"></a>certreq

O comando certreq pode ser usado para solicitar certificados de uma autoridade de certificação (CA), para recuperar uma resposta a uma solicitação anterior de uma autoridade de certificação, para criar uma nova solicitação de um arquivo. inf, para aceitar e instalar uma resposta a uma solicitação, para construir uma solicitação de certificação cruzada ou de subordinação qualificada de um certificado ou solicitação de autoridade de certificação existente e para assinar uma solicitação de certificação cruzada ou de subordinação qualificada.

> [!IMPORTANT]
> As versões anteriores do comando certreq podem não fornecer todas as opções descritas aqui. Para ver as opções com suporte com base em versões específicas do certreq, execute a opção de ajuda de linha de comando, `certreq -v -?` .
>
> O comando certreq não dá suporte à criação de uma nova solicitação de certificado com base em um modelo de atestado de chave quando estiver em um ambiente de CEP/CES.

> [!WARNING]
> O conteúdo deste tópico baseia-se nas configurações padrão do Windows Server; por exemplo, definindo o comprimento da chave como 2048, selecionando provedor de armazenamento de chaves de software da Microsoft como o CSP e usando Secure Hash Algorithm 1 (SHA1). Avalie essas seleções em relação aos requisitos da política de segurança da sua empresa.

## <a name="syntax"></a>Sintaxe

```
certreq [-submit] [options] [requestfilein [certfileout [certchainfileout [fullresponsefileOut]]]]
certreq -retrieve [options] requestid [certfileout [certchainfileout [fullresponsefileOut]]]
certreq -new [options] [policyfilein [requestfileout]]
certreq -accept [options] [certchainfilein | fullresponsefilein | certfilein]
certreq -sign [options] [requestfilein [requestfileout]]
certreq –enroll [options] templatename
certreq –enroll –cert certId [options] renew [reusekeys]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------- | ----------- |
| -Enviar | Envia uma solicitação para uma autoridade de certificação. |
| -recuperar`<requestid>` | Recupera uma resposta a uma solicitação anterior de uma autoridade de certificação. |
| -novo | Cria uma nova solicitação de um arquivo. inf. |
| -aceitar | Aceita e instala uma resposta a uma solicitação de certificado. |
| -política | Define a política para uma solicitação. |
| -assinar | Assina uma solicitação de certificação cruzada ou de subordinação qualificada. |
| -registrar | Registra ou renova um certificado. |
| -? | Exibe uma lista de sintaxe, opções e descrições de Certreq. |
| `<parameter>` -? | Exibe a ajuda para o parâmetro especificado. |
| -v-? | Exibe uma lista detalhada da sintaxe, das opções e das descrições do Certreq. |

## <a name="examples"></a>Exemplos

### <a name="certreq--submit"></a>certreq-enviar

Para enviar uma solicitação de certificado simples:

```
certreq –submit certrequest.req certnew.cer certnew.pfx
```

#### <a name="remarks"></a>Comentários

- Esse é o parâmetro de certreq.exe padrão. Se nenhuma opção for especificada no prompt da linha de comando, certreq.exe tentará enviar uma solicitação de certificado a uma autoridade de certificação. Você deve especificar um arquivo de solicitação de certificado ao usar a opção **– Submit** . Se esse parâmetro for omitido, uma janela de **abertura de arquivo** comum será exibida, permitindo que você selecione o arquivo de solicitação de certificado apropriado.

- Para solicitar um certificado especificando o atributo SAN, consulte a seção *como usar o utilitário certreq.exe para criar e enviar uma solicitação de certificado* do artigo 931351 da base de dados de conhecimento Microsoft [como adicionar um nome alternativo da entidade a um certificado LDAP seguro](https://support.microsoft.com/kb/931351).

### <a name="certreq--retrieve"></a>certreq-recuperar

Para recuperar a ID de certificado 20 e criar um arquivo de certificado (. cer), chamado *myCertificate*:

```
certreq -retrieve 20 MyCertificate.cer
```

#### <a name="remarks"></a>Comentários

- Use Certreq-recuperar *RequestId* para recuperar o certificado depois que a autoridade de certificação o tiver emitido. O *RequestId* PKC pode ser um decimal ou hex com o prefixo 0x e pode ser um número de série do certificado sem prefixo 0x. Você também pode usá-lo para recuperar qualquer certificado que já tenha sido emitido pela autoridade de certificação, incluindo certificados revogados ou expirados, sem considerar se a solicitação do certificado já estava no estado pendente.

- Se você enviar uma solicitação para a autoridade de certificação, o módulo de política da autoridade de certificação poderá deixar a solicitação em um estado pendente e retornar a *RequestId* para o chamador CertReq para exibição. Eventualmente, o administrador da autoridade de certificação emitirá o certificado ou negará a solicitação.

### <a name="certreq--new"></a>certreq-novo

Para criar uma nova solicitação:

```
[newrequest]
; At least one value must be set in this section
subject = CN=W2K8-BO-DC.contoso2.com
```

A seguir estão algumas das seções possíveis que podem ser adicionadas ao arquivo INF:

#### <a name="newrequest"></a>[newrequest]

Essa área do arquivo INF é obrigatória para qualquer novo modelo de solicitação de certificado e deve incluir pelo menos um parâmetro com um valor.

| Chave<sup>1</sup> | Descrição | Valor<sup>2</sup> | Exemplo |
| --- | ---------- | ----- | ------- |
| Assunto | Vários aplicativos dependem das informações da entidade em um certificado. É recomendável especificar um valor para essa chave. Se o assunto não estiver definido aqui, recomendamos que você inclua um nome de assunto como parte da extensão de certificado de nome alternativo da entidade. | Valores de cadeia de caracteres do nome distinto relativo | Subject = CN = Computador1. contoso. com Subject = CN = John Smith, CN = Users, DC = contoso, DC = com |
| Exportável | Se definido como TRUE, a chave privada pode ser exportada com o certificado. Para garantir um alto nível de segurança, as chaves privadas não devem ser exportáveis; no entanto, em alguns casos, pode ser necessário se vários computadores ou usuários tiverem que compartilhar a mesma chave privada. | `true | false` | `Exportable = TRUE`. As chaves CNG podem distinguir entre esse e o texto não criptografado exportável. As chaves CAPI1 não podem. |
| ExportableEncrypted | Especifica se a chave privada deve ser definida para ser exportável. | `true | false` | `ExportableEncrypted = true`<p>**Dica:** Nem todos os tamanhos de chave pública e algoritmos funcionarão com todos os algoritmos de hash. O CSP especificado também deve oferecer suporte ao algoritmo de hash especificado. Para ver a lista de algoritmos de hash com suporte, você pode executar o comando:`certutil -oid 1 | findstr pwszCNGAlgid | findstr /v CryptOIDInfo` |
| HashAlgorithm | Algoritmo de hash a ser usado para esta solicitação. | `Sha256, sha384, sha512, sha1, md5, md4, md2` | `HashAlgorithm = sha1`. Para ver a lista de algoritmos de hash com suporte, use: certutil-OID 1 | findstr pwszCNGAlgid | findstr/v CryptOIDInfo|
| KeyAlgorithm| O algoritmo que será usado pelo provedor de serviços para gerar um par de chaves pública e privada.| `RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521` | `KeyAlgorithm = RSA` |
| KeyContainer | Não recomendamos definir esse parâmetro para novas solicitações nas quais o novo material de chave é gerado. O contêiner de chave é automaticamente gerado e mantido pelo sistema.<p>Para solicitações em que o material de chave existente deve ser usado, esse valor pode ser definido como o nome do contêiner de chave da chave existente. Use o `certutil –key` comando para exibir a lista de contêineres de chave disponíveis para o contexto do computador. Use o `certutil –key –user` comando para o contexto do usuário atual.| Valor de cadeia de caracteres aleatória<p>**Dica:** Use aspas duplas em qualquer valor de chave INF que tenha espaços em branco ou caracteres especiais para evitar possíveis problemas de análise de INF. | `KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}` |
| KeyLength | Define o comprimento da chave pública e privada. O comprimento da chave tem um impacto sobre o nível de segurança do certificado. O comprimento de chave maior geralmente fornece um nível de segurança mais alto; no entanto, alguns aplicativos podem ter limitações em relação ao comprimento da chave. | Qualquer comprimento de chave válido com suporte do provedor de serviços de criptografia. | `KeyLength = 2048` |
| KeySpec | Determina se a chave pode ser usada para assinaturas, para o Exchange (criptografia) ou para ambos. | `AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE` | `KeySpec = AT_KEYEXCHANGE` |
| Uso de | Define a que a chave de certificado deve ser usada. | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> | `KeyUsage = CERT_DIGITAL_SIGNATURE_KEY_USAGE | CERT_KEY_ENCIPHERMENT_KEY_USAGE`<p>**Dica:** Vários valores usam um pipe (|) separador de símbolo. Certifique-se de usar aspas duplas ao usar vários valores para evitar problemas de análise de INF. Os valores mostrados são valores hexadecimais (decimais) para cada definição de bit. A sintaxe mais antiga também pode ser usada: um único valor hexadecimal com vários bits definidos, em vez da representação simbólica. Por exemplo, `KeyUsage = 0xa0`. |
| Keyutilizaproperty | Recupera um valor que identifica a finalidade específica para a qual uma chave privada pode ser usada. | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> | `KeyUsageProperty = NCRYPT_ALLOW_DECRYPT_FLAG | NCRYPT_ALLOW_SIGNING_FLAG` |
| MachineKeyset | Essa chave é importante quando você precisa criar certificados que pertencem à máquina e não um usuário. O material da chave gerado é mantido no contexto de segurança da entidade de segurança (conta de usuário ou computador) que criou a solicitação. Quando um administrador cria uma solicitação de certificado em nome de um computador, o material da chave deve ser criado no contexto de segurança da máquina e não no contexto de segurança do administrador. Caso contrário, o computador não poderá acessar sua chave privada, pois ela estaria no contexto de segurança do administrador. | `true | false`. O padrão é falso. | `MachineKeySet = true` |
| NotBefore | Especifica uma data ou data e hora antes da qual a solicitação não pode ser emitida. `NotBefore`pode ser usado com `ValidityPeriod` e `ValidityPeriodUnits` . | Data ou data e hora | `NotBefore = 7/24/2012 10:31 AM`<p>**Dica:** `NotBefore` e `NotAfter` são apenas para R `equestType=cert` . As tentativas de análise de data são sensíveis à localidade. O uso de nomes de meses causará a ambiguidade e deverá funcionar em todas as localidades. |
| NotAfter | Especifica uma data ou data e hora após a qual a solicitação não pode ser emitida. `NotAfter`Não pode ser usado com `ValidityPeriod` ou `ValidityPeriodUnits` . | Data ou data e hora | `NotAfter = 9/23/2014 10:31 AM`<p>**Dica:** `NotBefore` e `NotAfter` são `RequestType=cert` apenas para. As tentativas de análise de data são sensíveis à localidade. O uso de nomes de meses causará a ambiguidade e deverá funcionar em todas as localidades. |
| PrivateKeyArchive | A configuração PrivateKeyArchive só funcionará se o RequestType correspondente for definido como CMC porque apenas o formato de solicitação de mensagens de gerenciamento de certificado sobre CMS (CMC) permite transferir com segurança a chave privada do solicitante para a autoridade de arquivamento de chave. | `true | false` | `PrivateKeyArchive = true` |
| EncryptionAlgorithm | O algoritmo de criptografia a ser usado. | As opções possíveis variam, dependendo da versão do sistema operacional e do conjunto de provedores criptográficos instalados. Para ver a lista de algoritmos disponíveis, execute o comando: `certutil -oid 2 | findstr pwszCNGAlgid` . O CSP especificado também deve oferecer suporte ao algoritmo de criptografia simétrica especificado e ao comprimento. | `EncryptionAlgorithm = 3des` |
| EncryptionLength | Comprimento do algoritmo de criptografia a ser usado. | Qualquer comprimento permitido pelo EncryptionAlgorithm especificado. | `EncryptionLength = 128` |
| ProviderName | O nome do provedor é o nome de exibição do CSP. | Se você não souber o nome do provedor do CSP que está usando, execute `certutil –csplist` de uma linha de comando. O comando exibirá os nomes de todos os CSPs disponíveis no sistema local | `ProviderName = Microsoft RSA SChannel Cryptographic Provider` |
| ProviderType | O tipo de provedor é usado para selecionar provedores específicos com base no recurso de algoritmo específico, como RSA Full. | Se você não souber o tipo de provedor do CSP que está usando, execute `certutil –csplist` de um prompt de linha de comando. O comando exibirá o tipo de provedor de todos os CSPs disponíveis no sistema local. | `ProviderType = 1` |
| RenewalCert | Se precisar renovar um certificado que existe no sistema em que a solicitação de certificado é gerada, você deve especificar seu hash de certificado como o valor para essa chave. | O hash de certificado de qualquer certificado disponível no computador em que a solicitação de certificado é criada. Se você não souber o hash de certificado, use o snap-in do MMC de certificados e examine o certificado que deve ser renovado. Abra as propriedades do certificado e veja o `Thumbprint` atributo do certificado. A renovação de certificado requer um `PKCS#7` ou um `CMC` formato de solicitação. | `RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D` |
| RequesterName | Faz com que a solicitação se registre em nome de outra solicitação de usuário. A solicitação também deve ser assinada com um certificado de agente de registro ou a autoridade de certificação rejeitará a solicitação. Use a `-cert` opção para especificar o certificado do agente de registro. O nome do solicitante pode ser especificado para solicitações de certificado se o `RequestType` for definido como `PKCS#7` ou `CMC` . Se o `RequestType` for definido como `PKCS#10` , essa chave será ignorada. O `Requestername` só pode ser definido como parte da solicitação. Você não pode manipular o `Requestername` em uma solicitação pendente. | `Domain\User` | `Requestername = Contoso\BSmith` |
| RequestType | Determina o padrão usado para gerar e enviar a solicitação de certificado. | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul>**Dica:** Essa opção indica um certificado autoassinado ou emitido por conta própria. Ele não gera uma solicitação, mas sim um novo certificado e, em seguida, instala o certificado. Auto-assinado é o padrão. Especifique um certificado de assinatura usando a opção – CERT para criar um certificado autoemitido que não tenha assinatura automática. | `RequestType = CMC` |
| SecurityDescriptor | Contém as informações de segurança associadas a objetos protegíveis. Para a maioria dos objetos protegíveis, você pode especificar o descritor de segurança de um objeto na chamada de função que cria o objeto. Cadeias de caracteres baseadas na [linguagem de definição do descritor de segurança](/windows/win32/secauthz/security-descriptor-definition-language).<p>**Dica:** Isso é relevante apenas para chaves de cartão não inteligente de contexto de computador. | `SecurityDescriptor = D:P(A;;GA;;;SY)(A;;GA;;;BA)` |
| AlternateSignatureAlgorithm | Especifica e recupera um valor booliano que indica se o OID (identificador de objeto) de algoritmo de assinatura para uma solicitação PKCS # 10 ou assinatura de certificado é discreto ou combinado. | `true | false` | `AlternateSignatureAlgorithm = false`<p>Para uma assinatura RSA, `false` indica um `Pkcs1 v1.5` , enquanto `true` indica uma `v2.1` assinatura. |
| Silencioso | Por padrão, essa opção permite que o CSP acesse a área de trabalho do usuário interativo e solicite informações como um PIN do cartão inteligente do usuário. Se essa chave for definida como TRUE, o CSP não deverá interagir com a área de trabalho e será impedido de exibir qualquer interface do usuário para o usuário. | `true | false` | `Silent = true` |
| SMIME | Se esse parâmetro for definido como TRUE, uma extensão com o valor do identificador de objeto 1.2.840.113549.1.9.15 será adicionada à solicitação. O número de identificadores de objeto depende do na versão do sistema operacional instalada e do recurso CSP, que se refere aos algoritmos de criptografia simétrica que podem ser usados por aplicativos Secure Multipurpose Internet Mail Extensions (S/MIME), como o Outlook. | `true | false` | `SMIME = true` |
| UseExistingKeySet | Esse parâmetro é usado para especificar que um par de chaves existente deve ser usado na criação de uma solicitação de certificado. Se essa chave for definida como TRUE, você também deverá especificar um valor para a chave RenewalCert ou o nome do keycontainer. Você não deve definir a chave exportável porque não pode alterar as propriedades de uma chave existente. Nesse caso, nenhum material da chave é gerado quando a solicitação de certificado é criada. | `true | false` | `UseExistingKeySet = true` |
| Proteção contra keyprotection | Especifica um valor que indica como uma chave privada é protegida antes do uso. | <ul><li>`XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0`</li><li>`XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> | `KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG` |
| SuppressDefaults | Especifica um valor booliano que indica se as extensões e os atributos padrão estão incluídos na solicitação. Os padrões são representados por seus OIDs (identificadores de objeto). | `true | false` | `SuppressDefaults = true` |
| FriendlyName | Um nome amigável para o novo certificado. | Texto | `FriendlyName = Server1` |
| ValidityPeriodUnits | Especifica um número de unidades que deve ser usado com ValidityPeriod. Observação: isso é usado somente quando o `request type=cert` . | Numérico | `ValidityPeriodUnits = 3` |
| ValidityPeriod | ValidityPeriod deve ser um período de tempo no inglês dos EUA. Observação: isso é usado somente quando o tipo de solicitação = CERT. | `Years |  Months | Weeks | Days | Hours | Minutes | Seconds` | `ValidityPeriod = Years` |

<sup>1</sup> O parâmetro à esquerda do sinal de igual (=)

<sup>2</sup> O parâmetro à direita do sinal de igual (=)

#### <a name="extensions"></a>WMZ

Esta seção é opcional.

| OID de extensão | Definição | Exemplo |
| ------------- | ---------- | ----- | ------- |
| 2.5.29.17 | | 2.5.29.17 = {Text} |
| *continue* | | `continue = UPN=User@Domain.com&` |
| *continue* | | `continue = EMail=User@Domain.com&` |
| *continue* | | `continue = DNS=host.domain.com&` |
| *continue* | | `continue = DirectoryName=CN=Name,DC=Domain,DC=com&` |
| *continue* | | `continue = URL=<http://host.domain.com/default.html&>` |
| *continue* | | `continue = IPAddress=10.0.0.1&` |
| *continue* | | `continue = RegisteredId=1.2.3.4.5&` |
| *continue* | | `continue = 1.2.3.4.6.1={utf8}String&` |
| *continue* | | `continue = 1.2.3.4.6.2={octet}AAECAwQFBgc=&` |
| *continue* | | `continue = 1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&` |
| *continue* | | `continue = 1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&` |
| *continue* | | `continue = 1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07` |
| 2.5.29.37 | | `2.5.29.37={text}` |
| *continue* | | `continue = 1.3.6.1.5.5.7` |
| *continue* | | `continue = 1.3.6.1.5.5.7.3.1` |
| 2.5.29.19 | | `{text}ca=0pathlength=3` |
| Crítico | | `Critical=2.5.29.19` |
| KeySpec | | <ul><li>`AT_NONE -- 0`</li><li>`AT_SIGNATURE -- 2`</li><li>`AT_KEYEXCHANGE -- 1`</ul></li> |
| RequestType | | <ul><li>`PKCS10 -- 1`</li><li>`PKCS7 -- 2`</li><li>`CMC -- 3`</li><li>`Cert -- 4`</li><li>`SCEP -- fd00 (64768)`</li></ul> |
| Uso de | | <ul><li>`CERT_DIGITAL_SIGNATURE_KEY_USAGE -- 80 (128)`</li><li>`CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)`</li><li>`CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)`</li><li>`CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)`</li><li>`CERT_KEY_AGREEMENT_KEY_USAGE -- 8`</li><li>`CERT_KEY_CERT_SIGN_KEY_USAGE -- 4`</li><li>`CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_CRL_SIGN_KEY_USAGE -- 2`</li><li>`CERT_ENCIPHER_ONLY_KEY_USAGE -- 1`</li><li>`CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)`</li></ul> |
| Keyutilizaproperty | | <ul><li>`NCRYPT_ALLOW_DECRYPT_FLAG -- 1`</li><li>`NCRYPT_ALLOW_SIGNING_FLAG -- 2`</li><li>`NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4`</li><li>`NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)`</li></ul> |
| Proteção contra keyprotection | | <ul><li>`NCRYPT_UI_NO_PROTECTION_FLAG -- 0`</li><li>`NCRYPT_UI_PROTECT_KEY_FLAG -- 1`</li><li>`NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2`</li></ul> |
| SubjectNameFlags | template | <ul><li>`CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME -- 40000000 (1073741824)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)`</li><li>`CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)`</li><li>`CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME -- 8`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)`</li><li>`CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)`</li></ul> |
| X500NameFlags | | <ul><li>`CERT_NAME_STR_NONE -- 0`</li><li>`CERT_OID_NAME_STR -- 2`</li><li>`CERT_X500_NAME_STR -- 3`</li><li>`CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)`</li><li>`CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)`</li><li>`CERT_NAME_STR_NO_QUOTING_FLAG -- 10000000 (268435456)`</li><li>`CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)`</li><li>`CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)`</li><li>`CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)`</li><li>`CERT_NAME_STR_FORWARD_FLAG -- 1000000 (16777216)`</li><li>`CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)`</li><li>`CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)`</li><li>`CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)`</li><li>`CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)`</li><li>`CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)`</li><li>`CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)`</li></ul> |

> [!NOTE]
> `SubjectNameFlags`permite que o arquivo INF especifique quais campos de extensão de **assunto** e **SubjectAltName** devem ser preenchidos automaticamente pelo Certreq com base no usuário atual ou nas propriedades do computador atual: nome DNS, UPN e assim por diante. O uso do modelo literal significa que os sinalizadores de nome do modelo são usados. Isso permite que um único arquivo INF seja usado em vários contextos para gerar solicitações com informações de assunto específicas do contexto.
>
> `X500NameFlags`Especifica os sinalizadores a serem passados diretamente para a `CertStrToName` API quando o `Subject INF keys` valor é convertido em um **nome**diferenciado de ASN. 1 codificado.

#### <a name="example"></a>Exemplo

Para criar um arquivo de política (. inf) no bloco de notas e salvá-lo como *RequestConfig. inf*:

```
[NewRequest]
Subject = CN=<FQDN of computer you are creating the certificate>
Exportable = TRUE
KeyLength = 2048
KeySpec = 1
KeyUsage = 0xf0
MachineKeySet = TRUE
[RequestAttributes]
CertificateTemplate=WebServer
[Extensions]
OID = 1.3.6.1.5.5.7.3.1
OID = 1.3.6.1.5.5.7.3.2
```

No computador para o qual você está solicitando um certificado:

```
certreq –new requestconfig.inf certrequest.req
```

Para usar a sintaxe da seção [Strings] para OIDs e outros dados difíceis de interpretar. O novo exemplo de sintaxe de {Text} para extensão EKU, que usa uma lista separada por vírgulas de OIDs:

```
[Version]
Signature=$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = 2.5.29.37
szOID_PKIX_KP_SERVER_AUTH = 1.3.6.1.5.5.7.3.1
szOID_PKIX_KP_CLIENT_AUTH = 1.3.6.1.5.5.7.3.2

[NewRequest]
Subject = CN=TestSelfSignedCert
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%={text}%szOID_PKIX_KP_SERVER_AUTH%,
_continue_ = %szOID_PKIX_KP_CLIENT_AUTH%
```

### <a name="certreq--accept"></a>certreq-aceitar

O `–accept` parâmetro vincula a chave privada gerada anteriormente com o certificado emitido e remove a solicitação de certificado pendente do sistema em que o certificado é solicitado (se houver uma solicitação correspondente).

Para aceitar um certificado manualmente:

```
certreq -accept certnew.cer
```

> [!WARNING]
> O uso do `-accept` parâmetro com `-user` as `–machine` Opções e indica se o certificado de instalação deve ser instalado no contexto do **usuário** ou da **máquina** . Se houver uma solicitação pendente em qualquer contexto que corresponda à chave pública que está sendo instalada, essas opções não serão necessárias. Se não houver nenhuma solicitação pendente, um deles deverá ser especificado.

### <a name="certreq--policy"></a>certreq-política

O arquivo Policy. inf é um arquivo de configuração que define as restrições aplicadas a uma certificação de autoridade de certificação quando uma subordinação qualificada é definida.

Para criar uma solicitação de certificado cruzado:

```
certreq -policy certsrv.req policy.inf newcertsrv.req
```

Usar `certreq -policy` sem nenhum parâmetro adicional abre uma janela da caixa de diálogo, permitindo que você selecione o arquivos solicitado (. req,. CMC,. txt,. der,. cer ou. CRT). Depois de selecionar o arquivo solicitado e clicar em **abrir**, outra janela de diálogo será aberta, permitindo que você selecione o arquivo Policy. inf.

#### <a name="examples"></a>Exemplos

Localize um exemplo do arquivo Policy. inf na sintaxe do [CAPolicy. inf](../../networking/core-network-guide/cncg/server-certs/prepare-the-capolicy-inf-file.md).

### <a name="certreq--sign"></a>certreq-assinar

Para criar uma nova solicitação de certificado, assine-a e enviá-la:

```
certreq -new policyfile.inf myrequest.req
certreq -sign myrequest.req myrequest.req
certreq -submit myrequest_sign.req myrequest_cert.cer
```

#### <a name="remarks"></a>Comentários

- Usando `certreq -sign` sem qualquer parâmetro adicional, será aberta uma janela de diálogo para que você possa selecionar o arquivo solicitado (req, CMC, txt, der, CER ou CRT).

- Assinar a solicitação de subordinação qualificada pode exigir credenciais de **administrador corporativo** . Essa é uma prática recomendada para emitir certificados de assinatura para subordinação qualificada.

- O certificado usado para assinar a solicitação de subordinação qualificada usa o modelo de subordinação qualificado. Os administradores corporativos terão que assinar a solicitação ou conceder permissões de usuário para os indivíduos que assinarem o certificado.

- Talvez seja necessário que a equipe adicional assine a solicitação CMC depois de você. Isso dependerá do nível de garantia associado à subordinação qualificada.

- Se a autoridade de certificação pai da AC subordinada qualificada que você está instalando estiver offline, você deverá obter o certificado de autoridade de certificação para a AC subordinada qualificada do pai offline. Se a AC pai estiver online, especifique o certificado de autoridade de certificação para a autoridade de certificação subordinada qualificada durante o assistente de **instalação de serviços de certificados** .

### <a name="certreq--enroll"></a>certreq-registrar

Você pode usar este comentário para registrar ou renovar seus certificados.

#### <a name="examples"></a>Exemplos

Para registrar um certificado, usando o modelo do *WebServer* e selecionando o servidor de políticas usando U/I:

```
certreq -enroll –machine –policyserver * WebServer
```

Para renovar um certificado usando um número de série:

```
certreq –enroll -machine –cert 61 2d 3c fe 00 00 00 00 00 05 renew
```

Você só pode renovar certificados válidos. Os certificados expirados não podem ser renovados e devem ser substituídos por um novo certificado.

## <a name="options"></a>Opções

| Opções | Descrição |
| ------- | ----------- |
| -qualquer | `Force ICertRequest::Submit`para determinar o tipo de codificação.|
| -Atrib.`<attributestring>` | Especifica os pares de cadeia de caracteres de **nome** e **valor** , separados por dois-pontos.<p>Separe os pares **nome** e cadeia de caracteres de **valor** usando `\n` (por exemplo, Nome1: value1\nName2: value2). |
| -binário | Formata arquivos de saída como binários em vez de codificados em base64. |
| -policyserver`<policyserver>` | LDAP`<path>`<br>Insira o URI ou a ID exclusiva para um computador que executa o serviço Web da política de registro de certificado.<p>Para especificar que você gostaria de usar um arquivo de solicitação navegando, basta usar um sinal de menos (-) para `<policyserver>` . |
| -configuração`<ConfigString>` | Processa a operação usando a autoridade de certificação especificada na cadeia de caracteres de configuração, que é **CAHostName\CAName**. Para uma conexão https: \\ \, especifique o URI do servidor de registro. Para a AC do repositório do computador local, use um sinal de menos (-). |
| -anônimo | Use credenciais anônimas para serviços Web de registro de certificado. |
| -Kerberos | Use credenciais Kerberos (domínio) para serviços Web de registro de certificado. |
| -ClientCertificate`<ClientCertId>` | Você pode substituir `<ClientCertId>` por uma impressão digital do certificado, CN, EKU, modelo, email, UPN ou a nova `name=value` sintaxe. |
| -nome de usuário`<username>` | Usado com serviços Web de registro de certificado. Você pode substituir `<username>` pelo nome Sam ou pelo **domínio \** valor. Essa opção é para uso com a `-p` opção. |
| -p`<password>` | Usado com serviços Web de registro de certificado. Substitua `<password>` pela senha do usuário real. Essa opção é para uso com a `-username` opção. |
| -usuário | Configura o `-user` contexto para uma nova solicitação de certificado ou especifica o contexto para uma aceitação de certificado. Esse é o contexto padrão, se nenhum for especificado no INF ou no modelo. |
| -computador | Configura uma nova solicitação de certificado ou especifica o contexto de uma aceitação de certificado para o contexto da máquina. Para novas solicitações, ele deve ser consistente com a chave do MachineKeyset INF e o contexto do modelo. Se essa opção não for especificada e o modelo não definir um contexto, o padrão será o contexto do usuário. |
| -CRL | Inclui listas de certificados revogados (CRLs) na saída para o arquivo de #7 PKCS codificado em base64 especificado por `certchainfileout` ou para o arquivo codificado em base64 especificado por `requestfileout` . |
| -RPC | Instrui Active Directory serviços de certificados (AD CS) para usar uma conexão de servidor RPC (chamada de procedimento remoto) em vez de COM distribuído. |
| -adminforcemachine | Use o serviço de chave ou a representação para enviar a solicitação do contexto do sistema local. Requer que o usuário que invoca essa opção seja membro de administradores locais. |
| -renewonbehalfof | Envie uma renovação em nome do assunto identificado no certificado de autenticação. Isso define CR_IN_ROBO ao chamar o [método ICertRequest:: Submit](/windows/win32/api/certcli/nf-certcli-icertrequest-submit) |
| -f | Forçar os arquivos existentes a serem substituídos. Isso também ignora os modelos de cache e a política. |
| -Q | Usar o modo silencioso; suprimir todos os prompts interativos. |
| -Unicode | Grava a saída Unicode quando a saída padrão é redirecionada ou canalizada para outro comando, o que ajuda quando é invocado de scripts do Windows PowerShell. |
| -unicodetext | Envia a saída Unicode ao gravar blobs de dados codificados em texto base64 em arquivos. |

## <a name="formats"></a>Formatos

| Formatos | Descrição |
| ------- | ----------- |
| requestfilein | Nome de arquivo de entrada binário ou codificado na base64: solicitação de certificado de #10 de dados PKCS, solicitação de certificado CMS, solicitação de renovação de certificado #7 PKCS, certificado X. 509 a ser certificados cruzados ou solicitação de certificado de formato de marca KeyGen. |
| requestfileout | Nome do arquivo de saída codificado na base64. |
| certfileout | Nome de arquivo X-509 codificado na base64. |
| PKCS10fileout | Para uso somente com o `certreq -policy` parâmetro. Nome do arquivo de saída PKCS10 codificado na base64. |
| certchainfileout | Nome de arquivo de #7 PKCS codificado em base64. |
| fullresponsefileout | Nome de arquivo de resposta completa codificado em base64. |
| policyfilein | Para uso somente com o `certreq -policy` parâmetro. Arquivo INF que contém uma representação textual das extensões usadas para qualificar uma solicitação. |

## <a name="additional-resources"></a>Recursos adicionais

Os artigos a seguir contêm exemplos de uso de Certreq:

- [Como adicionar um nome alternativo da entidade a um certificado LDAP seguro](https://support.microsoft.com/help/931351/how-to-add-a-subject-alternative-name-to-a-secure-ldap-certificate)

- [Test Lab Guide: Deploying an AD CS Two-Tier PKI Hierarchy](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831348(v=ws.11))

- [Apêndice 3: sintaxe de Certreq.exe](/previous-versions/windows/it-pro/windows-server-2003/cc736326(v=ws.10))

- [Como criar um certificado SSL do servidor Web manualmente](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/how-to-create-a-web-server-ssl-certificate-manually/ba-p/1128529)

- [Registro de certificado para o agente de System Center Operations Manager](/system-center/scom/plan-planning-agent-deployment?view=sc-om-2019)

- [Visão geral dos Serviços de Certificados do Active Directory](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))

- [Como habilitar LDAP sobre SSL com uma autoridade de certificação de terceiros](https://support.microsoft.com/help/321051/how-to-enable-ldap-over-ssl-with-a-third-party-certification-authority)