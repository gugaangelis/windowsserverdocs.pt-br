---
title: certutil
description: Artigo de referência para o comando certutil, que é um programa de linha de comando que despeja e exibe informações de configuração de autoridade de certificação (CA), configura os serviços de certificados, os componentes de AC de backup e restauração e verifica certificados, pares de chaves e cadeias de certificados.
ms.topic: reference
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99c9d0ddca6ce1b91d86733995c30c46b747b7af
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031204"
---
# <a name="certutil"></a>certutil

Certutil.exe é um programa de linha de comando, instalado como parte dos serviços de certificados. Você pode usar certutil.exe para despejar e exibir informações de configuração da AC (autoridade de certificação), configurar os serviços de certificados, fazer backup e restaurar os componentes da AC e verificar certificados, pares de chaves e cadeias de certificados.

Se o Certutil for executado em uma autoridade de certificação sem parâmetros adicionais, ele exibirá a configuração da autoridade de certificação atual. Se o Certutil for executado em uma autoridade que não seja de certificação, o comando usa como padrão a execução do `certutil [-dump]` comando.

> [!IMPORTANT]
> As versões anteriores do certutil podem não fornecer todas as opções descritas neste documento. Você pode ver todas as opções que uma versão específica do certutil fornece executando o `certutil -?` ou o `certutil <parameter> -?` .

## <a name="parameters"></a>Parâmetros

### <a name="-dump"></a>-despejo

Despejar informações de configuração ou arquivos.

```
certutil [options] [-dump]
certutil [options] [-dump] file
```

```
[-f] [-silent] [-split] [-p password] [-t timeout]
```

### <a name="-asn"></a>-ASN

Analise o arquivo ASN. 1.

```
certutil [options] -asn file [type]
```

`[type]`: numeric CRYPT_STRING_ * tipo de decodificação

### <a name="-decodehex"></a>-decodehex

Decodificar um arquivo codificado em hexadecimal.

```
certutil [options] -decodehex infile outfile [type]
```

`[type]`: tipo de codificação de CRYPT_STRING_ numérico *

```
[-f]
```

### <a name="-decode"></a>-decodificar

Decodifique um arquivo codificado em base64.

```
certutil [options] -decode infile outfile
```

```
[-f]
```

### <a name="-encode"></a>-codificar

Codifique um arquivo para Base64.

```
certutil [options] -encode infile outfile
```

```
[-f] [-unicodetext]
```

### <a name="-deny"></a>-negar

Negar uma solicitação pendente.

```
certutil [options] -deny requestID
```

```
[-config Machine\CAName]
```

### <a name="-resubmit"></a>-reenviar

Reenvie uma solicitação pendente.

```
certutil [options] -resubmit requestId
```

```
[-config Machine\CAName]
```

### <a name="-setattributes"></a>-SetAttributes

Defina atributos para uma solicitação de certificado pendente.

```
certutil [options] -setattributes RequestID attributestring
```

Em que:

- **RequestId** é a ID de solicitação numérica para a solicitação pendente.

- **AttributeString** é o nome do atributo de solicitação e os pares de valor.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

- Os nomes e valores devem ser separados por dois-pontos, enquanto vários pares de nome e valor devem ser separados por nova linha. Por exemplo: `CertificateTemplate:User\nEMail:User@Domain.com` onde a `\n` sequência é convertida em um separador de nova linha.

### <a name="-setextension"></a>-setextension

Defina uma extensão para uma solicitação de certificado pendente.

```
certutil [options] -setextension requestID extensionname flags {long | date | string | \@infile}
```

Em que:

- **RequestId** é a ID de solicitação numérica para a solicitação pendente.

- **ExtensionName** é a cadeia de caracteres ObjectID da extensão.

- **sinalizadores** define a prioridade da extensão. `0` é recomendado, enquanto `1` define a extensão como crítica, `2` desabilita a extensão e `3` faz as duas.

```
[-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

- Se o último parâmetro for numérico, ele será obtido como um **longo**.

- Se o último parâmetro puder ser analisado como uma data, ele será obtido como uma **Data**.

- Se o último parâmetro começar com `\@` , o restante do token será obtido como o nome de arquivo com dados binários ou um despejo hexadecimal de texto ASCII.

- Se o último parâmetro for qualquer outra coisa, ele será usado como uma cadeia de caracteres.

### <a name="-revoke"></a>-REVOKE

Revogar um certificado.

```
certutil [options] -revoke serialnumber [reason]
```

Em que:

- **SerialNumber** é uma lista separada por vírgulas de números de série do certificado a revogar.

- o **motivo** é a representação numérica ou simbólica do motivo da revogação, incluindo:

  - **0. CRL_REASON_UNSPECIFIED** -não especificado (padrão)

  - **1. CRL_REASON_KEY_COMPROMISE** -comprometimento de chave

  - **2. CRL_REASON_CA_COMPROMISE** -comprometimento da autoridade de certificação

  - **3. CRL_REASON_AFFILIATION_CHANGED** -filiação alterada

  - **4. CRL_REASON_SUPERSEDED** substituído

  - **5. CRL_REASON_CESSATION_OF_OPERATION** -cessação da operação

  - **6. CRL_REASON_CERTIFICATE_HOLD** -reter certificado

  - **8. CRL_REASON_REMOVE_FROM_CRL** -remover da CRL

  - **1. cancelar revogação** -cancelar revogação

```
[-config Machine\CAName]
```

### <a name="-isvalid"></a>-IsValid

Exibe a disposição do certificado atual.

```
certutil [options] -isvalid serialnumber | certhash
```

```
[-config Machine\CAName]
```

### <a name="-getconfig"></a>-GetConfig

Obtenha a cadeia de caracteres de configuração padrão.

```
certutil [options] -getconfig
```

```
[-config Machine\CAName]
```

### <a name="-ping"></a>-ping

Tente entrar em contato com a interface de solicitação de serviços de certificados Active Directory.

```
certutil [options] -ping [maxsecondstowait | camachinelist]
```

Em que:

- **camachinelist** é uma lista separada por vírgulas de nomes de computador de autoridade de certificação. Para um único computador, use uma vírgula de terminação. Essa opção também exibe o custo do site para cada computador de autoridade de certificação.

```
[-config Machine\CAName]
```

### <a name="-cainfo"></a>-cainfo

Exibir informações sobre a autoridade de certificação.

```
certutil [options] -cainfo [infoname [index | errorcode]]
```

Em que:

- **InfoName** indica a propriedade de autoridade de certificação a ser exibida, com base na sintaxe do argumento InfoName a seguir:

  - versão do arquivo de **arquivo**

  - **produto** -versão do produto

  - contagem de módulos **exitcount** -Exit

  - **sair `[index]` ** -Sair da descrição do módulo

  - Descrição do módulo de política de **política**

  - **nome** -nome da autoridade de certificação

  - **sanitizedname** -nome de autoridade de certificação limpo

  - **dsname** -nome curto da autoridade de certificação limpo (nome DS)

  - **SharedFolder** -pasta compartilhada

  - **Error1 ErrorCode** -texto da mensagem de erro

  - **Error2 ErrorCode** -texto da mensagem de erro e código de erro

  - **tipo-tipo** de CA

  - **informações** -informações de autoridade de certificação

  - autoridade **de certificação pai-** pai

  - **certcount** -contagem de certificados de autoridade de certificação

  - **xchgcount** -contagem de certificados de troca de AC

  - contagem de certificados **kracount** -kra

  - contagem usada de certificados **kraused** -kra

  - **propidmax** -PropId de autoridade de certificação máximo

  - ** `[index]` certstate** -CERT CA

  - **certversion `[index]` ** -Versão do certificado da autoridade de certificação

  - **certstatuscode `[index]` ** -Status de verificação de certificado de CA

  - **crlstate `[index]` ** -CRL

  - **krastate `[index]` ** -Certificado KRA

  - **crossstate + `[index]` ** -Encaminhar certificado cruzado

  - **crossstate- `[index]` ** -Certificado cruzado para trás

  - **certificado `[index]` ** -CERT CA

  - **certchain `[index]` ** -Cadeia de certificados da autoridade de certificação

  - **certcrlchain `[index]` ** -Cadeia de certificados de autoridade de certificação com CRLs

  - **xchg `[index]` ** -Certificado de troca de AC

  - **xchgchain `[index]` ** -Cadeia de certificados de troca de AC

  - **xchgcrlchain `[index]` ** -Cadeia de certificados de troca de AC com CRLs

  - **kra `[index]` ** -Certificado KRA

  - ** `[index]` cruzada** -Encaminhar certificado cruzado

  - **entre `[index]` vários** -Certificado cruzado para trás

  - **CRL `[index]` ** -CRL base

  - **deltacrl `[index]` ** -CRL delta

  - **crlstatus `[index]` ** -Status de publicação da CRL

  - **deltacrlstatus `[index]` ** -Status de publicação de CRL delta

  - **DNS** -nome DNS

  - Separação de funções de **função**

  - **anúncios** -Advanced Server

  - **modelos** -modelos

  - **CSP `[index]` ** -URLs OCSP

  - **AIA `[index]` ** -URLs de AIA

  - **CDP `[index]` ** -URLs de CDP

  - **localename** -nome da localidade da autoridade de certificação

  - **subjecttemplateoids** -OIDs do modelo de assunto

  - **&#42;** -exibe todas as propriedades

- **índice** é o índice opcional de propriedades com base em zero.

- **ErrorCode** é o código de erro numérico.

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cacert"></a>-ca. cert

Recupere o certificado para a autoridade de certificação.

```
certutil [options] -ca.cert outcacertfile [index]
```

Em que:

- **outcacertfile** é o arquivo de saída.

- **índice** é o índice de renovação de certificado de CA (o padrão é o mais recente).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-cachain"></a>-ca. Chain

Recupere a cadeia de certificados para a autoridade de certificação.

```
certutil [options] -ca.chain outcacertchainfile [index]
```

Em que:

- **outcacertchainfile** é o arquivo de saída.

- **índice** é o índice de renovação de certificado de CA (o padrão é o mais recente).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-getcrl"></a>-getcrl

Obtém uma CRL (lista de certificados revogados).

certutil [opções]-getcrl outfile [índice] [Delta]

Em que:

- **index** é o índice de CRL ou índice de chave (o padrão é CRL para a chave mais recente).

- **Delta** é a CRL delta (o padrão é CRL base).

```
[-f] [-split] [-config Machine\CAName]
```

### <a name="-crl"></a>-CRL

Publicar novas listas de certificados revogados (CRLs) ou CRLs delta.

```
certutil [options] -crl [dd:hh | republish] [delta]
```

Em que:

- **DD: hh** é o novo período de validade da CRL em dias e horas.

- a **republicação** Republica as CRLs mais recentes.

- o **Delta** publica somente as CRLs delta (o padrão é CRLs de base e Delta).

```
[-split] [-config Machine\CAName]
```

### <a name="-shutdown"></a>-desligar

Desliga o Active Directory serviços de certificados.

```
certutil [options] -shutdown
```

```
[-config Machine\CAName]
```

### <a name="-installcert"></a>-installcert

Instala um certificado de autoridade de certificação.

```
certutil [options] -installcert [cacertfile]
```

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-renewcert"></a>-renewcert

Renova um certificado de autoridade de certificação.

```
certutil [options] -renewcert [reusekeys] [Machine\ParentCAName]
```

- Use `-f` para ignorar uma solicitação de renovação pendente e para gerar uma nova solicitação.

```
[-f] [-silent] [-config Machine\CAName]
```

### <a name="-schema"></a>-esquema

Despeja o esquema para o certificado.

```
certutil [options] -schema [ext | attrib | cRL]
```
Em que:

- O comando usa como padrão a tabela de solicitação e certificado.

- **ext** é a tabela de extensão.

- o **atributo** é a tabela de atributos.

- **CRL** é a tabela de CRL.

```
[-split] [-config Machine\CAName]
```

### <a name="-view"></a>-exibição

Despeja a exibição de certificado.

```
certutil [options] -view [queue | log | logfail | revoked | ext | attrib | crl] [csv]
```

Em que:

- a **fila** despeja uma fila de solicitações específica.

- o **log** despeja os certificados emitidos ou revogados, além de quaisquer solicitações com falha.

- **logfail** despeja as solicitações com falha.

- a **revogação** despeja os certificados revogados.

- **ext** despeja a tabela de extensão.

- o **atributo** despeja a tabela de atributos.

- a **CRL** despeja a tabela de CRL.

- o **CSV** fornece a saída usando valores separados por vírgulas.

```
[-silent] [-split] [-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

#### <a name="remarks"></a>Comentários

- Para exibir a coluna **StatusCode** para todas as entradas, digite `-out StatusCode`

- Para exibir todas as colunas da última entrada, digite: `-restrict RequestId==$`

- Para exibir **RequestId** e **disposição** para três solicitações, digite: `-restrict requestID>37,requestID<40 -out requestID,disposition`

- Para exibir IDs de**linha de** IDs de linha e **números de CRL** para todas as CRLs base, digite: `-restrict crlminbase=0 -out crlrowID,crlnumber crl`

- Para exibir, digite: `-v -restrict crlminbase=0,crlnumber=3 -out crlrawcrl crl`

- Para exibir a tabela de CRL inteira, digite: `CRL`

- Use `Date[+|-dd:hh]` para restrições de data.

- Use `now+dd:hh` para uma data relativa à hora atual.

### <a name="-db"></a>-DB

Despeja o banco de dados bruto.

```
certutil [options] -db
```

```
[-config Machine\CAName] [-restrict RestrictionList] [-out ColumnList]
```

### <a name="-deleterow"></a>-deleteRow

Exclui uma linha do banco de dados do servidor.

```
certutil [options] -deleterow rowID | date [request | cert | ext | attrib | crl]
```

Em que:

- a **solicitação** exclui as solicitações com falha e pendentes, com base na data de envio.

- **CERT** exclui os certificados expirados e revogados, com base na data de expiração.

- **ext** exclui a tabela de extensão.

- **atributo** exclui a tabela de atributos.

- a **CRL** exclui a tabela de CRL.

```
[-f] [-config Machine\CAName]
```

#### <a name="examples"></a>Exemplos

- Para excluir solicitações com falha e pendentes enviadas por 22 de janeiro de 2001, digite: `1/22/2001 request`

- Para excluir todos os certificados que expiraram em 22 de janeiro de 2001, digite: `1/22/2001 cert`

- Para excluir a linha, os atributos e as extensões do certificado para RequestID 37, digite: `37`

- Para excluir as CRLs que expiraram em 22 de janeiro de 2001, digite: `1/22/2001 crl`

### <a name="-backup"></a>-backup

Faz backup dos serviços de certificados Active Directory.

```
certutil [options] -backup backupdirectory [incremental] [keeplog]
```

Em que:

- **BackupDirectory** é o diretório para armazenar os dados de backup.

- **incremental** executa apenas um backup incremental (o padrão é backup completo).

- o **keeplog** preserva os arquivos de log do banco de dados (o padrão é truncar arquivos de log).

```
[-f] [-config Machine\CAName] [-p Password]
```

### <a name="-backupdb"></a>-backupdb

Faz backup do banco de dados de serviços de certificados Active Directory.

```
certutil [options] -backupdb backupdirectory [incremental] [keeplog]
```

Em que:

- **BackupDirectory** é o diretório para armazenar os arquivos de banco de dados de backup.

- **incremental** executa apenas um backup incremental (o padrão é backup completo).

- o **keeplog** preserva os arquivos de log do banco de dados (o padrão é truncar arquivos de log).

```
[-f] [-config Machine\CAName]
```

### <a name="-backupkey"></a>-backupkey

Faz backup da Active Directory certificado de serviços de certificados e da chave privada.

```
certutil [options] -backupkey backupdirectory
```

Em que:

- **BackupDirectory** é o diretório para armazenar o arquivo PFX de backup.

```
[-f] [-config Machine\CAName] [-p password] [-t timeout]
```

### <a name="-restore"></a>-restore

Restaura os serviços de certificados Active Directory.

```
certutil [options] -restore backupdirectory
```

Em que:

- **BackupDirectory** é o diretório que contém os dados a serem restaurados.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-restoredb"></a>-restoredb

Restaura o banco de dados de serviços de certificados Active Directory.

```
certutil [options] -restoredb backupdirectory
```

Em que:

- **BackupDirectory** é o diretório que contém os arquivos de banco de dados a serem restaurados.

```
[-f] [-config Machine\CAName]
```

### <a name="-restorekey"></a>-restorekey

Restaura a Active Directory certificado de serviços de certificados e a chave privada.

```
certutil [options] -restorekey backupdirectory | pfxfile
```

Em que:

- **BackupDirectory** é o diretório que contém o arquivo PFX a ser restaurado.

```
[-f] [-config Machine\CAName] [-p password]
```

### <a name="-importpfx"></a>-importpfx

Importe o certificado e a chave privada. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

```
certutil [options] -importpfx [certificatestorename] pfxfile [modifiers]
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- os **modificadores** são a lista separada por vírgulas, que pode incluir um ou mais dos seguintes:

  1. **AT_SIGNATURE** -altera o KeySpec para assinatura

  2. **AT_KEYEXCHANGE** -altera o KeySpec para a troca de chaves

  3. **Noexporte** – torna a chave privada não exportável

  4. **Nocert** -não importa o certificado

  5. **Nochain** -não importa a cadeia de certificados

  6. **Noroot** -não importa o certificado raiz

  7. **Proteger** – protege as chaves usando uma senha

  8. **Noprotect** -não protege as chaves com senha usando uma senha

```
[-f] [-user] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Comentários

- O padrão é o repositório de computador pessoal.

### <a name="-dynamicfilelist"></a>-dynamicfilelist

Exibe uma lista de arquivos dinâmicos.

```
certutil [options] -dynamicfilelist
```

```
[-config Machine\CAName]
```

### <a name="-databaselocations"></a>-databaselocations

Exibe locais de banco de dados.

```
certutil [options] -databaselocations
```

```
[-config Machine\CAName]
```

### <a name="-hashfile"></a>-hashfile

Gera e exibe um hash criptográfico em um arquivo.

```
certutil [options] -hashfile infile [hashalgorithm]
```

### <a name="-store"></a>-Store

Despeja o repositório de certificados.

```
certutil [options] -store [certificatestorename [certID [outputfile]]]
```

Em que:

- **certificatestorename** é o nome do repositório de certificados. Por exemplo:

  - `My, CA (default), Root,`

  - `ldap:///CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?one?objectClass=certificationAuthority (View Root Certificates)`

  - `ldap:///CN=CAName,CN=Certification Authorities,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Modify Root Certificates)`

  - `ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?certificateRevocationList?base?objectClass=cRLDistributionPoint (View CRLs)`

  - `ldap:///CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=cpandl,DC=com?cACertificate?base?objectClass=certificationAuthority (Enterprise CA Certificates)`

  - `ldap: (AD computer object certificates)`

  - `-user ldap: (AD user object certificates)`

- **certid** é o token de correspondência de certificado ou CRL. Pode ser um número de série, um certificado SHA-1, CRL, CTL ou hash de chave pública, um índice de certificado numérico (0, 1 e assim por diante), um índice de CRL numérico (. 0, 0,1 e assim por diante), um índice de CTL numérico (.. 0,.. 1 e assim por diante), uma chave pública, uma assinatura ou um ObjectId de extensão, um nome comum de entidade de certificado, um endereço de email, nome DNS ou UPN, um nome de contêiner de chave ou nome CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum de emissor de CRL. Muitas delas podem resultar em várias correspondências.

- **arquivo_de_saída** é o arquivo usado para salvar os certificados correspondentes.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-silent] [-split] [-dc DCName]
```

#### <a name="options"></a>Opções

- A `-user` opção acessa um repositório de usuários em vez de um repositório de computador.

- A `-enterprise` opção acessa uma loja empresarial do computador.

- A `-service` opção acessa um repositório de serviço de computador.

- A `-grouppolicy` opção acessa um repositório de política de grupo de computadores.

Por exemplo:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-addstore"></a>-addstore

Adiciona um certificado ao repositório. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

```
certutil [options] -addstore certificatestorename infile
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- **INFILE** é o arquivo de certificado ou CRL que você deseja adicionar ao repositório.

```
[-f] [-user] [-enterprise] [-grouppolicy] [-dc DCName]
```

### <a name="-delstore"></a>-delstore

Exclui um certificado do repositório. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

```
certutil [options] -delstore certificatestorename certID
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- **certid** é o token de correspondência de certificado ou CRL.

```
[-enterprise] [-user] [-grouppolicy] [-dc DCName]
```

### <a name="-verifystore"></a>-verifystore

Verifica um certificado no repositório. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

```
certutil [options] -verifystore certificatestorename [certID]
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- **certid** é o token de correspondência de certificado ou CRL.

```
[-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-dc DCName] [-t timeout]
```

### <a name="-repairstore"></a>-repairstore

Repara uma associação de chave ou atualiza as propriedades do certificado ou o descritor de segurança da chave. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

```
certutil [options] -repairstore certificatestorename certIDlist [propertyinffile | SDDLsecuritydescriptor]
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- **certIDlist** é a lista separada por vírgulas de tokens de correspondência de certificado ou CRL. Para obter mais informações, consulte a `-store certID` Descrição neste artigo.

- **propertyinffile** é o arquivo inf que contém propriedades externas, incluindo:

  ```
  [Properties]
      19 = Empty ; Add archived property, OR:
      19 =       ; Remove archived property

      11 = {text}Friendly Name ; Add friendly name property

      127 = {hex} ; Add custom hexadecimal property
          _continue_ = 00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f
          _continue_ = 10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f

      2 = {text} ; Add Key Provider Information property
        _continue_ = Container=Container Name&
        _continue_ = Provider=Microsoft Strong Cryptographic Provider&
        _continue_ = ProviderType=1&
        _continue_ = Flags=0&
        _continue_ = KeySpec=2

      9 = {text} ; Add Enhanced Key Usage property
        _continue_ = 1.3.6.1.5.5.7.3.2,
        _continue_ = 1.3.6.1.5.5.7.3.1,
  ```

```
[-f] [-enterprise] [-user] [-grouppolicy] [-silent] [-split] [-csp provider]
```

### <a name="-viewstore"></a>-viewstore

Despeja o repositório de certificados. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

```
certutil [options] -viewstore [certificatestorename [certID [outputfile]]]
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- **certid** é o token de correspondência de certificado ou CRL.

- **arquivo_de_saída** é o arquivo usado para salvar os certificados correspondentes.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Opções

- A `-user` opção acessa um repositório de usuários em vez de um repositório de computador.

- A `-enterprise` opção acessa uma loja empresarial do computador.

- A `-service` opção acessa um repositório de serviço de computador.

- A `-grouppolicy` opção acessa um repositório de política de grupo de computadores.

Por exemplo:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-viewdelstore"></a>-viewdelstore

Exclui um certificado do repositório.

```
certutil [options] -viewdelstore [certificatestorename [certID [outputfile]]]
```

Em que:

- **certificatestorename** é o nome do repositório de certificados.

- **certid** é o token de correspondência de certificado ou CRL.

- **arquivo_de_saída** é o arquivo usado para salvar os certificados correspondentes.

```
[-f] [-user] [-enterprise] [-service] [-grouppolicy] [-dc DCName]
```

#### <a name="options"></a>Opções

- A `-user` opção acessa um repositório de usuários em vez de um repositório de computador.

- A `-enterprise` opção acessa uma loja empresarial do computador.

- A `-service` opção acessa um repositório de serviço de computador.

- A `-grouppolicy` opção acessa um repositório de política de grupo de computadores.

Por exemplo:

- `-enterprise NTAuth`

- `-enterprise Root 37`

- `-user My 26e0aaaf000000000004`

- `CA .11`

### <a name="-dspublish"></a>-dspublish

Publica um certificado ou CRL (lista de certificados revogados) para Active Directory.

```
certutil [options] -dspublish certfile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | User | Machine]
```

```
certutil [options] -dspublish CRLfile [DSCDPContainer [DSCDPCN]]
```

Em que:

- **CertFile** é o nome do arquivo de certificado a ser publicado.

- **NTAuthCA** publica o certificado na Enterprise Store do DS.

- **RootCA** publica o certificado no repositório de raiz confiável do DS.

- **SubCA** publica o certificado de autoridade de certificação no objeto de AC do DS.

- **CrossCA** publica o certificado cruzado no objeto de AC do DS.

- O **kra** publica o certificado no objeto do agente de recuperação de chave do DS.

- O **usuário** publica o certificado no objeto DS do usuário.

- O **computador** publica o certificado no objeto DS do computador.

- **CRLfile** é o nome do arquivo de CRL a ser publicado.

- **DSCDPContainer** é o CN do contêiner de CDP do DS, geralmente o nome do computador da autoridade de certificação.

- **DSCDPCN** é o objeto CN de CDP do DS, geralmente com base no nome curto e índice de chave da AC corrigido.

- Use `-f` para criar um novo objeto DS.

```
[-f] [-user] [-dc DCName]
```

### <a name="-adtemplate"></a>-adtemplate

Exibe modelos de Active Directory.

```
certutil [options] -adtemplate [template]
```

```
[-f] [-user] [-ut] [-mt] [-dc DCName]
```

### <a name="-template"></a>-modelo

Exibe os modelos de certificado.

```
certutil [options] -template [template]
```

```
[-f] [-user] [-silent] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-templatecas"></a>-templatecas

Exibe as CAs (autoridades de certificação) de um modelo de certificado.

```
certutil [options] -templatecas template
```

```
[-f] [-user] [-dc DCName]
```

### <a name="-catemplates"></a>-catemplates

Exibe modelos para a autoridade de certificação.

```
certutil [options] -catemplates [template]
```

```
[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]
```

### <a name="-setcasites"></a>-setcasites

Gerencia nomes de site, incluindo configuração, verificação e exclusão de nomes de site de autoridade de certificação

```
certutil [options] -setcasites [set] [sitename]
certutil [options] -setcasites verify [sitename]
certutil [options] -setcasites delete
```

Em que:

- **SiteName** só é permitido quando direcionado a uma única autoridade de certificação.

```
[-f] [-config Machine\CAName] [-dc DCName]
```

#### <a name="remarks"></a>Comentários

- A `-config` opção se destina a uma única autoridade de certificação (o padrão é todas as CAS).

- A `-f` opção pode ser usada para substituir erros de validação para o **SiteName** especificado ou para excluir todos os sites de autoridade de certificação.

> [!NOTE]
> Para obter mais informações sobre como configurar o reconhecimento de site de CAs para Active Directory Domain Services (AD DS), consulte [AD DS reconhecimento de site para clientes do AD CS e PKI](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11)).

### <a name="-enrollmentserverurl"></a>-enrollmentserverURL

Exibe, adiciona ou exclui as URLs de servidor de registro associadas a uma CA.

```
certutil [options] -enrollmentServerURL [URL authenticationtype [priority] [modifiers]]
certutil [options] -enrollmentserverURL URL delete
```

Em que:

- **AuthenticationType** especifica um dos seguintes métodos de autenticação de cliente, ao adicionar uma URL:

  1. **Kerberos** -use as credenciais SSL Kerberos.

  2. **nome de usuário** – use uma conta nomeada para credenciais SSL.

  3. **ClientCertificate**:-Use as credenciais SSL do certificado X. 509.

  4. **anônimo** – usar credenciais SSL anônimas.

- **excluir** exclui a URL especificada associada à autoridade de certificação.

- a **prioridade** padrão será `1` se não for especificada ao adicionar uma URL.

- **modificadores** é uma lista separada por vírgulas, que inclui um ou mais dos seguintes:

1. as solicitações de renovação somente **allowrenewalsonly** podem ser enviadas para essa AC por meio desta URL.

2. **allowkeybasedrenewal** – permite o uso de um certificado que não tem nenhuma conta associada no AD. Isso se aplica somente ao modo ClientCertificate e allowrenewalsonly

```
[-config Machine\CAName] [-dc DCName]
```

### <a name="-adca"></a>-adca

Exibe Active Directory autoridades de certificação.

```
certutil [options] -adca [CAName]
```

```
[-f] [-split] [-dc DCName]
```

### <a name="-ca"></a>-CA

Exibe as autoridades de certificação da política de registro.

```
certutil [options] -CA [CAName | templatename]
```

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policy"></a>-política

Exibe a política de registro.

```
[-f] [-user] [-silent] [-split] [-policyserver URLorID] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-policycache"></a>-policycache

Exibe ou exclui entradas de cache de política de registro.

```
certutil [options] -policycache [delete]
```

Em que:

- **excluir** exclui as entradas do cache do servidor de políticas.

- **-f** exclui todas as entradas de cache

```
[-f] [-user] [-policyserver URLorID]
```

### <a name="-credstore"></a>-credstore

Exibe, adiciona ou exclui entradas de repositório de credenciais.

```
certutil [options] -credstore [URL]
certutil [options] -credstore URL add
certutil [options] -credstore URL delete
```

Em que:

- A **URL** é a URL de destino. Você também pode usar `*` para corresponder a todas as entradas ou `https://machine*` para corresponder a um prefixo de URL.

- **Adicionar** adiciona uma entrada de repositório de credenciais. Usar essa opção também requer o uso de credenciais SSL.

- **excluir** exclui as entradas do repositório de credenciais.

- **-f** substitui uma única entrada ou exclui várias entradas.

```
[-f] [-user] [-silent] [-anonymous] [-kerberos] [-clientcertificate clientcertID] [-username username] [-p password]
```

### <a name="-installdefaulttemplates"></a>-installdefaulttemplates

Instala modelos de certificado padrão.

```
certutil [options] -installdefaulttemplates
```

```
[-dc DCName]
```

### <a name="-urlcache"></a>-URLcache

Exibe ou exclui entradas de cache de URL.

```
certutil [options] -URLcache [URL | CRL | * [delete]]
```

Em que:

- A **URL** é a URL armazenada em cache.

- A **CRL** é executada somente em todas as URLs de CRL em cache.

- **&#42;** opera em todas as URLs em cache.

- **excluir** exclui as URLs relevantes do cache local do usuário atual.

- **-f** força a busca de uma URL específica e a atualização do cache.

```
[-f] [-split]
```

### <a name="-pulse"></a>-Pulse

Pulsa eventos de registro automático.

```
certutil [options] -pulse
```

```
[-user]
```

### <a name="-machineinfo"></a>-machineinfo

Exibe informações sobre o objeto de computador Active Directory.

```
certutil [options] -machineinfo domainname\machinename$
```

### <a name="-dcinfo"></a>-DCInfo

Exibe informações sobre o controlador de domínio. O padrão exibe certificados de DC sem verificação.

```
certutil [options] -DCInfo [domain] [verify | deletebad | deleteall]
```

```
[-f] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

> [!TIP]
> A capacidade de especificar um domínio de Active Directory Domain Services (AD DS) **[domain]** e especificar um controlador de domínio (**-DC**) foi adicionada ao Windows Server 2012. Para executar o comando com êxito, você deve usar uma conta que seja membro de **Administradores de domínio** ou **Administradores de empresa**. As modificações de comportamento desse comando são as seguintes:<ol><li>1. se um domínio não for especificado e um controlador de domínio específico não for especificado, essa opção retornará uma lista de controladores de domínio a serem processados a partir do controlador de domínio padrão.</li><li>2. se um domínio não for especificado, mas um controlador de domínio for especificado, um relatório dos certificados no controlador de domínio especificado será gerado.</li><li>3. se um domínio for especificado, mas um controlador de domínio não for especificado, uma lista de controladores de domínio será gerada junto com os relatórios nos certificados para cada controlador de domínio na lista.</li><li>4. se o domínio e o controlador de domínio forem especificados, uma lista de controladores de domínio será gerada a partir do controlador de domínio de destino. Um relatório dos certificados para cada controlador de domínio na lista também é gerado.</li></ol>
>
>Por exemplo, suponha que haja um domínio chamado CPANDL com um controlador de domínio chamado CPANDL-DC1. Você pode executar o seguinte comando para recuperar uma lista de controladores de domínio e seus certificados do CPANDL-DC1: `certutil -dc cpandl-dc1 -DCInfo cpandl`

### <a name="-entinfo"></a>-entinfo

Exibe informações sobre uma autoridade de certificação corporativa.

```
certutil [options] -entinfo domainname\machinename$
```

```
[-f] [-user]
```

### <a name="-tcainfo"></a>-tcainfo

Exibe informações sobre a autoridade de certificação.

```
certutil [options] -tcainfo [domainDN | -]
```

```
[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t timeout]
```

### <a name="-scinfo"></a>-scinfo

Exibe informações sobre o cartão inteligente.

```
certutil [options] -scinfo [readername [CRYPT_DELETEKEYSET]]
```

Em que:

- **CRYPT_DELETEKEYSET** exclui todas as chaves no cartão inteligente.

```
[-silent] [-split] [-urlfetch] [-t timeout]
```

### <a name="-scroots"></a>-scroots

Gerencia certificados raiz de cartão inteligente.

```
certutil [options] -scroots update [+][inputrootfile] [readername]
certutil [options] -scroots save \@in\\outputrootfile [readername]
certutil [options] -scroots view [inputrootfile | readername]
certutil [options] -scroots delete [readername]
```

```
[-f] [-split] [-p Password]
```

### <a name="-verifykeys"></a>-verifykeys

Verifica um conjunto de chaves pública ou privada.

```
certutil [options] -verifykeys [keycontainername cacertfile]
```

Em que:

- **KeyContainerName** é o nome do contêiner de chave para a chave a ser verificada. Essa opção assume como padrão as chaves do computador. Para alternar para as chaves de usuário, use `-user` .

- os **cacertrs** assinam ou criptografam arquivos de certificado.

```
[-f] [-user] [-silent] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

- Se nenhum argumento for especificado, cada certificado de autoridade de certificação de assinatura será verificado em relação à sua chave privada.

- Esta operação só pode ser executada em uma autoridade de certificação local ou em chaves locais.

### <a name="-verify"></a>-verificar

Verifica um certificado, a CRL (lista de certificados revogados) ou a cadeia de certificados.

```
certutil [options] -verify certfile [applicationpolicylist | - [issuancepolicylist]]
certutil [options] -verify certfile [cacertfile [crossedcacertfile]]
certutil [options] -verify CRLfile cacertfile [issuedcertfile]
certutil [options] -verify CRLfile cacertfile [deltaCRLfile]
```

Em que:

- **CertFile** é o nome do certificado a ser verificado.

- **ApplicationPolicyList** é a lista separada por vírgulas opcional de ObjectIDs de política de aplicativo necessárias.

- **issuancepolicylist** é a lista separada por vírgulas opcional de ObjectIDs de política de emissão necessárias.

- **CACertFile** é o certificado de AC emissor opcional para verificar.

- **crossedcacertfile** é o certificado opcional entre certificados por **CertFile**.

- **CRLfile** é o arquivo de CRL usado para verificar o **cacertm**.

- **issuedcertfile** é o certificado opcional emitido coberto pelo CRLfile.

- **deltaCRLfile** é o arquivo de CRL delta opcional.

```
[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t timeout]
```

#### <a name="remarks"></a>Comentários

- O uso de **ApplicationPolicyList** restringe a criação de cadeias somente a cadeias válidas para as políticas de aplicativo especificadas.

- O uso de **issuancepolicylist** restringe a construção de cadeia a apenas cadeias válidas para as políticas de emissão especificadas.

- O uso de **cacerts**  verifica os campos no arquivo em relação a **CertFile** ou **CRLfile**.

- O uso de **issuedcertfile** verifica os campos no arquivo em relação a **CRLfile**.

- O uso de deltaCRLfile verifica os campos no arquivo em relação a **CertFile**.

- Se o **CACertFile** não for especificado, a cadeia completa será criada e verificada em relação a **CertFile**.

- Se **cacertvalue** e **crossedcacertfile** forem especificados, os campos em ambos os arquivos serão verificados em relação a **CertFile**.

### <a name="-verifyctl"></a>-verifyCTL

Verifica a CTL de certificados AuthRoot ou não permitido.

```
certutil [options] -verifyCTL CTLobject [certdir] [certfile]
```

Em que:

- **CTLobject** identifica a CTL a ser verificada, incluindo:

  - **AuthRootWU** -lê o CAB AuthRoot e os certificados correspondentes do cache de URL. Use `-f` para baixar de Windows Update em vez disso.

  - **DisallowedWU** -lê o CAB de certificados não permitidos e o arquivo de repositório de certificados não permitido do cache de URL. Use `-f` para baixar de Windows Update em vez disso.

  - **AuthRoot** -lê o registro em cache AuthRoot CTL. Use com `-f` e um **CertFile** não confiável para forçar o registro armazenado em cache AuthRoot e o certificado não permitido de CTLs para atualizar.

  - Não **permitido** – lê a CTL de certificados não permitidos em cache do registro. Use com `-f` e um **CertFile** não confiável para forçar o registro armazenado em cache AuthRoot e o certificado não permitido de CTLs para atualizar.

- **CTLfilename** especifica o arquivo ou caminho http para o arquivo CTL ou cab.

- **certdir** especifica a pasta que contém os certificados que correspondem às entradas de CTL. O padrão é a mesma pasta ou site que o **CTLobject**. O uso de um caminho de pasta http requer um separador de caminho no final. Se você não especificar **AuthRoot** ou não **permitido**, vários locais serão pesquisados para certificados correspondentes, incluindo repositórios de certificados locais, crypt32.dll recursos e o cache de URL local. Use `-f` para baixar do Windows Update, conforme necessário.

- **CertFile** especifica os certificados a serem verificados. Os certificados são correspondidos nas entradas de CTL, exibindo os resultados. Essa opção suprime a maior parte da saída padrão.

```
[-f] [-user] [-split]
```

### <a name="-sign"></a>-assinar

Assina novamente uma CRL (lista de certificados revogados) ou certificado.

```
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [startdate+dd:hh] [+serialnumberlist | -serialnumberlist | -objectIDlist | \@extensionfile]
certutil [options] -sign infilelist | serialnumber | CRL outfilelist [#hashalgorithm] [+alternatesignaturealgorithm | -alternatesignaturealgorithm]
```

Em que:

- **infilelist** é a lista separada por vírgulas de certificados ou arquivos CRL para modificar e assinar novamente.

- **SerialNumber** é o número de série do certificado a ser criado. O período de validade e outras opções não podem estar presentes.

- A **CRL** cria uma CRL vazia. O período de validade e outras opções não podem estar presentes.

- **outfilelist** é a lista separada por vírgulas de certificados modificados ou arquivos de saída de CRL. O número de arquivos deve corresponder a FileList.

- **StartDate + DD: hh** é o novo período de validade para os arquivos de certificado ou CRL, incluindo:

  - Data opcional mais

  - período de validade de dias e horas opcionais

  Se ambos forem especificados, você deverá usar um separador de sinal de mais (+). Use `now[+dd:hh]` para iniciar na hora atual. Use `never` para não ter nenhuma data de expiração (somente para CRLs).

- **serialnumberlist** é a lista de números de série separados por vírgula dos arquivos a serem adicionados ou removidos.

- **objectidlist** é a lista de ObjectID da extensão separada por vírgula dos arquivos a serem removidos.

- ** \@ extensãofile** é o arquivo inf que contém as extensões a serem atualizadas ou removidas. Por exemplo:

  ```
  [Extensions]
      2.5.29.31 = ; Remove CRL Distribution Points extension
      2.5.29.15 = {hex} ; Update Key Usage extension
      _continue_=03 02 01 86
  ```

- **HashAlgorithm** é o nome do algoritmo de hash. Esse deve ser apenas o texto precedido pelo `#` sinal.

- **alternatesignaturealgorithm** é o especificador de algoritmo de assinatura alternativo.

```
[-nullsign] [-f] [-silent] [-cert certID]
```

#### <a name="remarks"></a>Comentários

- O uso do sinal de menos (-) remove os números de série e as extensões.

- O uso do sinal de adição (+) adiciona números de série a uma CRL.

- Você pode usar uma lista para remover números de série e ObjectIDs de uma CRL ao mesmo tempo.

- Usar o sinal de subtração antes de **alternatesignaturealgorithm** permite que você use o formato de assinatura herdado. O uso do sinal de adição permite que você use o formato de assinatura alternativo. Se você não especificar **alternatesignaturealgorithm**, o formato de assinatura no certificado ou na CRL será usado.

### <a name="-vroot"></a>-vroot

Cria ou exclui os compartilhamentos de arquivos e as raízes virtuais da Web.

```
certutil [options] -vroot [delete]
```

### <a name="-vocsproot"></a>-vocsproot

Cria ou exclui raízes virtuais da Web para um proxy Web OCSP.

```
certutil [options] -vocsproot [delete]
```

### <a name="-addenrollmentserver"></a>-addenrollmentserver

Adicione um aplicativo de servidor de registro e um pool de aplicativos, se necessário, para a autoridade de certificação especificada. Esse comando não instala binários ou pacotes.

```
certutil [options] -addenrollmentserver kerberos | username | clientcertificate [allowrenewalsonly] [allowkeybasedrenewal]
```

Em que:

- o **addenrollmentserver** exige que você use um método de autenticação para a conexão do cliente com o servidor de registro de certificado, incluindo:

  - o **Kerberos** usa credenciais SSL Kerberos.

  - o **nome de usuário** usa a conta nomeada para credenciais SSL.

  - **ClientCertificate** usa credenciais SSL do certificado X. 509.

- o **allowrenewalsonly** permite apenas envios de solicitação de renovação à autoridade de certificação por meio da URL.

- **allowkeybasedrenewal** permite o uso de um certificado sem nenhuma conta associada no Active Directory. Isso se aplica quando usado com o modo **ClientCertificate** e **allowrenewalsonly** .

```
[-config Machine\CAName]
```

### <a name="-deleteenrollmentserver"></a>-deleteenrollmentserver

Exclui um aplicativo de servidor de registro e um pool de aplicativos, se necessário, para a autoridade de certificação especificada. Esse comando não instala binários ou pacotes.

```
certutil [options] -deleteenrollmentserver kerberos | username | clientcertificate
```

Em que:

- o **deleteenrollmentserver** exige que você use um método de autenticação para a conexão do cliente com o servidor de registro de certificado, incluindo:

  - o **Kerberos** usa credenciais SSL Kerberos.

  - o **nome de usuário** usa a conta nomeada para credenciais SSL.

  - **ClientCertificate** usa credenciais SSL do certificado X. 509.

```
[-config Machine\CAName]
```

### <a name="-addpolicyserver"></a>-addpolicyserver

Adicione um aplicativo de servidor de política e um pool de aplicativos, se necessário. Esse comando não instala binários ou pacotes.

```
certutil [options] -addpolicyserver kerberos | username | clientcertificate [keybasedrenewal]
```

Em que:

- o **addpolicyserver** exige que você use um método de autenticação para a conexão do cliente com o servidor de política de certificado, incluindo:

  - o **Kerberos** usa credenciais SSL Kerberos.

  - o **nome de usuário** usa a conta nomeada para credenciais SSL.

  - **ClientCertificate** usa credenciais SSL do certificado X. 509.

- **keybasedrenewal** permite o uso de políticas retornadas para o cliente que contém modelos keybasedrenewal. Essa opção se aplica somente à autenticação de **nome de usuário** e **ClientCertificate** .

### <a name="-deletepolicyserver"></a>-deletepolicyserver

Exclui um aplicativo de servidor de política e um pool de aplicativos, se necessário. Esse comando não remove binários ou pacotes.

certutil [opções]-deletePolicyServer Kerberos | nome de usuário | ClientCertificate [keybasedrenewal]

Em que:

- o **deletepolicyserver** exige que você use um método de autenticação para a conexão do cliente com o servidor de política de certificado, incluindo:

  - o **Kerberos** usa credenciais SSL Kerberos.

  - o **nome de usuário** usa a conta nomeada para credenciais SSL.

  - **ClientCertificate** usa credenciais SSL do certificado X. 509.

- **keybasedrenewal** permite o uso de um servidor de políticas keybasedrenewal.

### <a name="-oid"></a>-OID

Exibe o identificador de objeto ou define um nome de exibição.

```
certutil [options] -oid objectID [displayname | delete [languageID [type]]]
certutil [options] -oid groupID
certutil [options] -oid agID | algorithmname [groupID]
```

Em que:

- **ObjectID** exibe ou adiciona o nome de exibição.

- **GroupId** é o número GroupId (Decimal) que ObjectIDs enumera.

- **algID** é a ID hexadecimal que o ObjectID pesquisa.

- **algorithmName** é o nome do algoritmo que o ObjectID pesquisa.

- **DisplayName** exibe o nome a ser armazenado no DS.

- **excluir** exclui o nome de exibição.

- **LanguageID** é o valor da ID de idioma (o padrão é atual: 1033).

- **Tipo** é o tipo de objeto DS a ser criado, incluindo:

  - `1` -Modelo (padrão)

  - `2` -Política de emissão

  - `3` -Política de aplicativo

- `-f` Cria um objeto DS.

### <a name="-error"></a>-erro

Exibe o texto da mensagem associado a um código de erro.

```
certutil [options] -error errorcode
```

### <a name="-getreg"></a>-getreg

Exibe um valor de registro.

```
certutil [options] -getreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Em que:

- a **AC** usa a chave do registro de uma autoridade de certificação.

- **Restore** usa a chave do registro Restore da autoridade de certificação.

- a **política** usa a chave do registro do módulo de política.

- **Exit** usa a chave do registro do primeiro módulo de saída.

- o **modelo** usa a chave do registro de modelo (use `-user` para modelos de usuário).

- o **registro** usa a chave do registro de registro (use `-user` para o contexto do usuário).

- a **cadeia** usa a chave do registro de configuração da cadeia.

- **policyservers** usa a chave do registro de servidores de política.

- **ProgID** usa o ProgID do módulo de política ou saída (nome da subchave do registro).

- **registryvaluename** usa o nome do valor do registro (use `Name*` para correspondência de prefixo).

- o **valor** usa o novo valor de registro numérico, de cadeia de caracteres ou de data ou nome de arquivo. Se um valor numérico começar com `+` ou `-` , os bits especificados no novo valor serão definidos ou apagados no valor do registro existente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

- Se um valor de cadeia de caracteres começar com `+` ou `-` , e o valor existente for um `REG_MULTI_SZ` valor, a cadeia de caracteres será adicionada ou removida do valor de registro existente. Para forçar a criação de um `REG_MULTI_SZ` valor, adicione `\n` ao final do valor da cadeia de caracteres.

- Se o valor começar com `\@` , o restante do valor será o nome do arquivo que contém a representação de texto hexadecimal de um valor binário. Se não se referir a um arquivo válido, ele será analisado como `[Date][+|-][dd:hh]` uma data opcional mais ou menos dias e horas opcionais. Se ambos forem especificados, use um separador de sinal de adição (+) ou sinal de subtração (-). Use `now+dd:hh` para uma data relativa à hora atual.

- Use `chain\chaincacheresyncfiletime \@now` para liberar efetivamente as CRLs em cache.

### <a name="-setreg"></a>-setreg

Define um valor de registro.

```
certutil [options] -setreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]]registryvaluename value
```

Em que:

- a **AC** usa a chave do registro de uma autoridade de certificação.

- **Restore** usa a chave do registro Restore da autoridade de certificação.

- a **política** usa a chave do registro do módulo de política.

- **Exit** usa a chave do registro do primeiro módulo de saída.

- o **modelo** usa a chave do registro de modelo (use `-user` para modelos de usuário).

- o **registro** usa a chave do registro de registro (use `-user` para o contexto do usuário).

- a **cadeia** usa a chave do registro de configuração da cadeia.

- **policyservers** usa a chave do registro de servidores de política.

- **ProgID** usa o ProgID do módulo de política ou saída (nome da subchave do registro).

- **registryvaluename** usa o nome do valor do registro (use `Name*` para correspondência de prefixo).

- o **valor** usa o novo valor de registro numérico, de cadeia de caracteres ou de data ou nome de arquivo. Se um valor numérico começar com `+` ou `-` , os bits especificados no novo valor serão definidos ou apagados no valor do registro existente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

- Se um valor de cadeia de caracteres começar com `+` ou `-` , e o valor existente for um `REG_MULTI_SZ` valor, a cadeia de caracteres será adicionada ou removida do valor de registro existente. Para forçar a criação de um `REG_MULTI_SZ` valor, adicione `\n` ao final do valor da cadeia de caracteres.

- Se o valor começar com `\@` , o restante do valor será o nome do arquivo que contém a representação de texto hexadecimal de um valor binário. Se não se referir a um arquivo válido, ele será analisado como `[Date][+|-][dd:hh]` uma data opcional mais ou menos dias e horas opcionais. Se ambos forem especificados, use um separador de sinal de adição (+) ou sinal de subtração (-). Use `now+dd:hh` para uma data relativa à hora atual.

- Use `chain\chaincacheresyncfiletime \@now` para liberar efetivamente as CRLs em cache.

### <a name="-delreg"></a>-delreg

Exclui um valor de registro.

```
certutil [options] -delreg [{ca | restore | policy | exit | template | enroll |chain | policyservers}\[progID\]][registryvaluename]
```

Em que:

- a **AC** usa a chave do registro de uma autoridade de certificação.

- **Restore** usa a chave do registro Restore da autoridade de certificação.

- a **política** usa a chave do registro do módulo de política.

- **Exit** usa a chave do registro do primeiro módulo de saída.

- o **modelo** usa a chave do registro de modelo (use `-user` para modelos de usuário).

- o **registro** usa a chave do registro de registro (use `-user` para o contexto do usuário).

- a **cadeia** usa a chave do registro de configuração da cadeia.

- **policyservers** usa a chave do registro de servidores de política.

- **ProgID** usa o ProgID do módulo de política ou saída (nome da subchave do registro).

- **registryvaluename** usa o nome do valor do registro (use `Name*` para correspondência de prefixo).

- o **valor** usa o novo valor de registro numérico, de cadeia de caracteres ou de data ou nome de arquivo. Se um valor numérico começar com `+` ou `-` , os bits especificados no novo valor serão definidos ou apagados no valor do registro existente.

```
[-f] [-user] [-grouppolicy] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

- Se um valor de cadeia de caracteres começar com `+` ou `-` , e o valor existente for um `REG_MULTI_SZ` valor, a cadeia de caracteres será adicionada ou removida do valor de registro existente. Para forçar a criação de um `REG_MULTI_SZ` valor, adicione `\n` ao final do valor da cadeia de caracteres.

- Se o valor começar com `\@` , o restante do valor será o nome do arquivo que contém a representação de texto hexadecimal de um valor binário. Se não se referir a um arquivo válido, ele será analisado como `[Date][+|-][dd:hh]` uma data opcional mais ou menos dias e horas opcionais. Se ambos forem especificados, use um separador de sinal de adição (+) ou sinal de subtração (-). Use `now+dd:hh` para uma data relativa à hora atual.

- Use `chain\chaincacheresyncfiletime \@now` para liberar efetivamente as CRLs em cache.

### <a name="-importkms"></a>-importKMS

Importa chaves de usuário e certificados para o banco de dados do servidor para arquivamento de chave.

```
certutil [options] -importKMS userkeyandcertfile [certID]
```

Em que:

- **userkeyandcertfile** é um arquivo de dados com chaves privadas do usuário e certificados que devem ser arquivados. Esse arquivo pode ser:

  - Um arquivo de exportação do servidor de gerenciamento de chaves do Exchange (KMS).

  - Um arquivo PFX.

- certid é um token de correspondência de certificado de descriptografia de arquivo de exportação KMS. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

- `-f` importa certificados não emitidos pela autoridade de certificação.

```
[-f] [-silent] [-split] [-config Machine\CAName] [-p password] [-symkeyalg symmetrickeyalgorithm[,keylength]]
```

### <a name="-importcert"></a>-importcert

Importa um arquivo de certificado para o banco de dados.

```
certutil [options] -importcert certfile [existingrow]
```

Em que:

- **existingrow** importa o certificado no lugar de uma solicitação pendente para a mesma chave.

- `-f` importa certificados não emitidos pela autoridade de certificação.

```
[-f] [-config Machine\CAName]
```

#### <a name="remarks"></a>Comentários

A autoridade de certificação também pode precisar ser configurada para dar suporte a certificados estrangeiros. Para fazer isso, digite `import - certutil -setreg ca\KRAFlags +KRAF_ENABLEFOREIGN` .

### <a name="-getkey"></a>-GetKey

Recupera um blob de recuperação de chave privada arquivada, gera um script de recuperação ou recupera chaves arquivadas.

```
certutil [options] -getkey searchtoken [recoverybloboutfile]
certutil [options] -getkey searchtoken script outputscriptfile
certutil [options] -getkey searchtoken retrieve | recover outputfilebasename
```

Em que:

- o **script** gera um script para recuperar e recuperar chaves (comportamento padrão se vários candidatos à recuperação correspondentes forem encontrados ou se o arquivo de saída não for especificado).

- **recuperar** recupera um ou mais blobs de recuperação de chave (comportamento padrão se exatamente um candidato de recuperação correspondente for encontrado e se o arquivo de saída for especificado). O uso dessa opção trunca qualquer extensão e acrescenta a cadeia de caracteres específica do certificado e a extensão. rec para cada blob de recuperação de chave.  Cada arquivo contém uma cadeia de certificados e uma chave privada associada, ainda criptografada para um ou mais certificados de agente de recuperação de chave.

- a **recuperação** recupera e recuperar chaves privadas em uma única etapa (requer certificados e chaves privadas do agente de recuperação de chave). O uso dessa opção trunca qualquer extensão e acrescenta a extensão. p12.  Cada arquivo contém as cadeias de certificados recuperadas e as chaves privadas associadas, armazenadas como um arquivo PFX.

- **SearchToken** seleciona as chaves e os certificados a serem recuperados, incluindo:

  - 1. Nome comum do certificado

  - 2. Número de série do certificado

  - 3. Hash de SHA-1 de certificado (impressão digital)

  - 4. Hash KeyId SHA-1 de certificado (identificador de chave da entidade)

  - 5. Nome do solicitante (domínio \ usuário)

  - 6. UPN (domínio de usuário \@ )

- o **recoverybloboutfile** gera um arquivo com uma cadeia de certificados e uma chave privada associada, ainda criptografada para um ou mais certificados de agente de recuperação de chave.

- **outputscriptfile** gera um arquivo com um script em lotes para recuperar e recuperar chaves privadas.

- **outputfilebasename** gera um nome de base de arquivo.

```
[-f] [-unicodetext] [-silent] [-config Machine\CAName] [-p password] [-protectto SAMnameandSIDlist] [-csp provider]
```

### <a name="-recoverkey"></a>-recoverkey

Recuperar uma chave privada arquivada.

```
certutil [options] -recoverkey recoveryblobinfile [PFXoutfile [recipientindex]]
```

```
[-f] [-user] [-silent] [-split] [-p password] [-protectto SAMnameandSIDlist] [-csp provider] [-t timeout]
```

### <a name="-mergepfx"></a>-mergePFX

Mescla arquivos PFX.

```
certutil [options] -mergePFX PFXinfilelist PFXoutfile [extendedproperties]
```

Em que:

- **PFXinfilelist** é uma lista separada por vírgulas de arquivos de entrada PFX.

- **PFXoutfile** é o nome do arquivo de saída PFX.

- **ExtendedProperties** inclui todas as propriedades estendidas.

```
[-f] [-user] [-split] [-p password] [-protectto SAMnameAndSIDlist] [-csp provider]
```

#### <a name="remarks"></a>Comentários

- A senha especificada na linha de comando deve ser uma lista de senhas separadas por vírgula.

- Se mais de uma senha for especificada, a última senha será usada para o arquivo de saída. Se apenas uma senha for fornecida ou se a última senha for `*` , o usuário será solicitado a fornecer a senha do arquivo de saída.

### <a name="-convertepf"></a>-convertEPF

Converte um arquivo PFX em um arquivo EPF.

```
certutil [options] -convertEPF PFXinfilelist PFXoutfile [cast | cast-] [V3CAcertID][,salt]
```

Em que:


- **PFXinfilelist** é uma lista separada por vírgulas de arquivos de entrada PFX.

- **PFXoutfile** é o nome do arquivo de saída PFX.

- **EPF** é o nome do arquivo de saída EPF.

- **Cast** usa criptografia 64 de conversão.

- **Cast –** usa criptografia 64 de conversão (exportação)

- **V3CAcertID** é o token de correspondência de certificado de autoridade de certificação v3. Para obter mais informações, consulte o `-store` parâmetro neste artigo.

- **Salt** é a cadeia de caracteres de Salt do arquivo de saída EPF.

```
[-f] [-silent] [-split] [-dc DCName] [-p password] [-csp provider]
```

#### <a name="remarks"></a>Comentários

- A senha especificada na linha de comando deve ser uma lista de senhas separadas por vírgula.

- Se mais de uma senha for especificada, a última senha será usada para o arquivo de saída. Se apenas uma senha for fornecida ou se a última senha for `*` , o usuário será solicitado a fornecer a senha do arquivo de saída.

### <a name="-"></a>-?

Exibe a lista de parâmetros.

```
certutil -?
certutil <name_of_parameter> -?
certutil -? -v
```

Em que:

- **-?** exibe a lista completa de parâmetros

- **-`<name_of_parameter>` -?** exibe o conteúdo da ajuda para o parâmetro especificado.

- **-?-v** exibe uma lista completa de parâmetros e opções.

## <a name="options"></a>Opções

Esta seção define todas as opções que você pode especificar, com base no comando. Cada parâmetro inclui informações sobre quais opções são válidas para uso.

| Opções | Descrição |
| ------- | ----------- |
| -nullsign | Use o hash dos dados como uma assinatura. |
| -f | Forçar substituição. |
| -enterprise | Use o repositório de certificados do registro empresarial do computador local. |
| -usuário | Use as chaves de HKEY_CURRENT_USER ou o repositório de certificados. |
| -GroupPolicy | Use o repositório de certificados da política de grupo. |
| -UT | Exibir modelos do usuário. |
| -MT | Exibir modelos de máquina. |
| -Unicode | Gravar saída redirecionada em Unicode. |
| -UnicodeText | Gravar arquivo de saída em Unicode. |
| -GMT | Exibir horários usando GMT. |
| -segundos | Exibir horários usando segundos e milissegundos. |
| -silencioso | Use o `silent` sinalizador para adquirir o contexto cript. |
| -Divisão | Dividir elementos ASN internos. 1 e salvar em arquivos. |
| -v | Forneça informações mais detalhadas (detalhados). |
| -PrivateKey | Exibir dados de senha e de chave privada. |
| -fixar PIN | PIN do cartão inteligente. |
| -urlfetch | Recuperar e verificar certificados AIA e CRLs de CDP. |
| -config Machine\CAName | Autoridade de certificação e nome do computador cadeia de caracteres. |
| -policyserver URLorID | URL ou ID do servidor de política. Para a seleção U/I, use `-policyserver` . Para todos os servidores de política, use `-policyserver *`|
| -anônimo | Usar credenciais SSL anônimas. |
| -Kerberos | Use as credenciais SSL do Kerberos. |
| -ClientCertificate clientcertID | Use as credenciais SSL do certificado X. 509. Para a seleção U/I, use `-clientcertificate` . |
| -nome de usuário username | Use a conta nomeada para credenciais SSL. Para a seleção U/I, use `-username` . |
| -certid do certificado | Certificado de autenticação. |
| -DC DCName | Direcione um controlador de domínio específico. |
| -restringir restrição | Lista de restrições separadas por vírgulas. Cada restrição consiste em um nome de coluna, um operador relacional e um inteiro constante, uma cadeia de caracteres ou uma data. Um nome de coluna pode ser precedido por um sinal de mais ou menos para indicar a ordem de classificação. Por exemplo: `requestID = 47`, `+requestername >= a, requestername` ou `-requestername > DOMAIN, Disposition = 21` |
| -saída da coluna | Lista de colunas separadas por vírgulas. |
| -p senha | Senha |
| -protectto SAMnameandSIDlist | Lista de nome/SID do SAM separada por vírgulas. |
| -provedor CSP | Provedor |
| -t tempo limite | Tempo limite de busca de URL em milissegundos. |
| -symkeyalg symmetrickeyalgorithm [, KeyLength] | Nome do algoritmo de chave simétrica com comprimento de chave opcional. Por exemplo: `AES,128` ou `3DES` |

### <a name="additional-references"></a>Referências adicionais

Para obter mais exemplos de como usar esse comando, consulte

- [AD CS (Serviços de Certificados do Active Directory)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831740(v=ws.11))

- [Tarefas de certutil para gerenciar certificados](/previous-versions/orphan-topics/ws.10/cc772898(v=ws.10))

- [comando certutil](certutil.md)