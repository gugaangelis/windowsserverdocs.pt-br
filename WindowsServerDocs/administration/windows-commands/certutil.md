---
title: certutil
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71525e4051a079eb9a3d0c8c197c8157b53e5e67
ms.sourcegitcommit: 1f3ffff0af340868dcf3a2cfef5b8f8aea69d96d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78278541"
---
# <a name="certutil"></a>certutil

O Certutil. exe é um programa de linha de comando instalado como parte dos serviços de certificados. Você pode usar o Certutil. exe para despejar e exibir as informações de configuração da AC (autoridade de certificação), configurar os serviços de certificados, fazer backup e restaurar os componentes da AC e verificar certificados, pares de chaves e cadeias de certificados.

Quando o Certutil é executado em uma autoridade de certificação sem parâmetros adicionais, ele exibe a configuração da autoridade de certificação atual. Quando o Certutil é executado em uma autoridade que não é de certificação, o padrão do comando é executar o verbo certutil [-dump](#-dump) .

> [!WARNING]
> As versões anteriores do certutil podem não fornecer todas as opções descritas neste documento. Você pode ver todas as opções que uma versão específica do certutil fornece executando os comandos mostrados na seção de [notações de sintaxe](#syntax-notations) .

## <a name="menu"></a>Menu

As principais seções deste documento são:

- [Verbos](#verbs)
- [Notações de sintaxe](#syntax-notations)
- [Opções](#options)
- [Exemplos adicionais de certutil](#additional-certutil-examples)

## <a name="verbs"></a>Verbos

A tabela a seguir descreve os verbos que podem ser usados com o comando certutil.

|Verbos|Descrição|
|-----|-----------|
|[-despejo](#-dump)|Despejar arquivos ou informações de configuração|
|[-ASN](#-asn)|Analisar arquivo ASN. 1|
|[-decodehex](#-decodehex)|Decodificar arquivo codificado em hexadecimal|
|[-decodificar](#-decode)|Decodificar um arquivo codificado em base64|
|[-codificar](#-encode)|Codificar um arquivo em base64|
|[-negar](#-deny)|Negar uma solicitação de certificado pendente|
|[-reenviar](#-resubmit)|Reenviar uma solicitação de certificado pendente|
|[-SetAttributes](#-setattributes)|Definir atributos para uma solicitação de certificado pendente|
|[-setextension](#-setextension)|Definir uma extensão para uma solicitação de certificado pendente|
|[-REVOKE](#-revoke)|Revogar um certificado|
|[-IsValid](#-isvalid)|Exibir a disposição do certificado atual|
|[-GetConfig](#-getconfig)|Obter a cadeia de caracteres de configuração padrão|
|[-ping](#-ping)|Tentativa de entrar em contato com a interface de solicitação de serviços de certificados Active Directory|
|-pingadmin|Tente entrar em contato com a interface do administrador dos serviços de certificados Active Directory|
|[-CAInfo](#-cainfo)|Exibir informações sobre a autoridade de certificação|
|[-ca. cert](#-cacert)|Recuperar o certificado para a autoridade de certificação|
|[-ca. Chain](#-cachain)|Recuperar a cadeia de certificados para a autoridade de certificação|
|[-GetCRL](#-getcrl)|Obter uma CRL (lista de certificados revogados)|
|[-CRL](#-crl)|Publicar novas listas de certificados revogados (CRLs) [ou somente CRLs delta]|
|[-desligar](#-shutdown)|Desligar Active Directory serviços de certificados|
|[-installCert](#-installcert)|Instalar um certificado de autoridade de certificação|
|[-renewCert](#-renewcert)|Renovar um certificado de autoridade de certificação|
|[-esquema](#-schema)|Despejar o esquema para o certificado|
|[-exibição](#-view)|Despejar a exibição do certificado|
|[-DB](#-db)|Despejar o banco de dados bruto|
|[-deleteRow](#-deleterow)|Excluir uma linha do banco de dados do servidor|
|[-backup](#-backup)|Serviços de certificados de Active Directory de backup|
|[-backupDB](#-backupdb)|Fazer backup do banco de dados de serviços de certificados Active Directory|
|[-backupKey](#-backupkey)|Fazer backup do certificado de serviços de certificados Active Directory e da chave privada|
|[-restaurar](#-restore)|Restaurar Active Directory serviços de certificados|
|[-restoreDB](#-restoredb)|Restaurar o banco de dados de serviços de certificados Active Directory|
|[-restoreKey](#-restorekey)|Restaurar o certificado de serviços de certificados Active Directory e a chave privada|
|[-importPFX](#-importpfx)|Importar certificado e chave privada|
|[-dynamicfilelist](#-dynamicfilelist)|Exibir uma lista de arquivos dinâmicos|
|[-databaselocations](#-databaselocations)|Exibir locais de banco de dados|
|[-hashfile](#-hashfile)|Gerar e exibir um hash criptográfico em um arquivo|
|[-Store](#-store)|Despejar o repositório de certificados|
|[-addstore](#-addstore)|Adicionar um certificado ao repositório|
|[-delstore](#-delstore)|Excluir um certificado da loja|
|[-verifystore](#-verifystore)|Verificar um certificado no repositório|
|[-repairstore](#-repairstore)|Reparar uma associação de chave ou atualizar as propriedades do certificado ou o descritor de segurança da chave|
|[-viewstore](#-viewstore)|Despejar o armazenamento de certificados|
|[-viewdelstore](#-viewdelstore)|Excluir um certificado da loja|
|[-dsPublish](#-dspublish)|Publicar um certificado ou CRL (lista de certificados revogados) para Active Directory|
|[-ADTemplate](#-adtemplate)|Exibir modelos do AD|
|[-Modelo](#-template)|Exibir modelos de certificado|
|[-TemplateCAs](#-templatecas)|Exibir as CAs (autoridades de certificação) de um modelo de certificado|
|[-CATemplates](#-catemplates)|Exibir modelos para CA|
|[-SetCASites](#-setcasites)|Gerenciar nomes de site para CAs|
|[-enrollmentServerURL](#-enrollmentserverurl)|Exibir, adicionar ou excluir as URLs de servidor de registro associadas a uma AC|
|[-ADCA](#-adca)|Exibir CAs do AD|
|[-CA](#-ca)|Exibir CAs da política de registro|
|[-Política](#-policy)|Exibir política de registro|
|[-PolicyCache](#-policycache)|Exibir ou excluir entradas do cache da política de registro|
|[-CredStore](#-credstore)|Exibir, adicionar ou excluir entradas de repositório de credenciais|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|Instalar modelos de certificado padrão|
|[-URLCache](#-urlcache)|Exibir ou excluir entradas de cache de URL|
|[-Pulse](#-pulse)|Eventos de registro automático de pulso|
|[-MachineInfo](#-machineinfo)|Exibir informações sobre o objeto de computador Active Directory|
|[-DCInfo](#-dcinfo)|Exibir informações sobre o controlador de domínio|
|[-EntInfo](#-entinfo)|Exibir informações sobre uma AC corporativa|
|[-TCAInfo](#-tcainfo)|Exibir informações sobre a autoridade de certificação|
|[-SCInfo](#-scinfo)|Exibir informações sobre o cartão inteligente|
|[-SCRoots](#-scroots)|Gerenciar certificados raiz do cartão inteligente|
|[-verifykeys](#-verifykeys)|Verificar um conjunto de chaves pública ou privada|
|[-verificar](#-verify)|Verificar um certificado, CRL (lista de certificados revogados) ou cadeia de certificados|
|[-verifyCTL](#-verifyctl)|Verificar CTL de certificados AuthRoot ou não permitido|
|[-assinar](#-sign)|Assinar novamente uma CRL (lista de certificados revogados) ou certificado|
|[-vroot](#-vroot)|Criar ou excluir compartilhamentos de arquivos e raízes virtuais da Web|
|[-vocsproot](#-vocsproot)|Criar ou excluir raízes virtuais da Web para um proxy Web OCSP|
|[-addEnrollmentServer](#-addenrollmentserver)|Adicionar um aplicativo de servidor de registro|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|Excluir um aplicativo de servidor de registro|
|[-addPolicyServer](#-addpolicyserver)|Adicionar um aplicativo de servidor de política|
|[-deletePolicyServer](#-deletepolicyserver)|Excluir um aplicativo de servidor de política|
|[-OID](#-oid)|Exibir o identificador de objeto ou definir um nome de exibição|
|[-erro](#-error)|Exibir o texto da mensagem associado a um código de erro|
|[-getreg](#-getreg)|Exibir um valor de registro|
|[-setreg](#-setreg)|Definir um valor de registro|
|[-delreg](#-delreg)|Excluir um valor de registro|
|[-ImportKMS](#-importkms)|Importar chaves de usuário e certificados para o banco de dados do servidor para arquivamento de chaves|
|[-ImportCert](#-importcert)|Importar um arquivo de certificado para o banco de dados|
|[-GetKey](#-getkey)|Recuperar um blob de recuperação de chave privada arquivada|
|[-RecoverKey](#-recoverkey)|Recuperar uma chave privada arquivada|
|[-MergePFX](#-mergepfx)|Mesclar arquivos PFX|
|[-ConvertEPF](#-convertepf)|Converter um arquivo PFX em um arquivo EPF|
|-?|Exibe a lista de verbos|
|*> -\<verbo* -?|Exibe a ajuda para o verbo especificado.|
|-? -v|Exibe uma lista completa de verbos e|

Retornar ao [menu](#menu)

## <a name="syntax-notations"></a>Notações de sintaxe

- Para obter a sintaxe básica de linha de comando, execute `certutil -?`
- Para obter a sintaxe sobre como usar o Certutil com um verbo específico, execute **certutil** *\<Verb >* **-?**
- Para enviar toda a sintaxe de certutil para um arquivo de texto, execute os seguintes comandos:  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

A tabela a seguir descreve a notação usada para indicar a sintaxe da linha de comando.


|            Notation             |                  Descrição                  |
|---------------------------------|-----------------------------------------------|
| Texto sem colchetes ou chaves |         Itens que você deve digitar, conforme mostrado          |
|  \<texto dentro de colchetes angulares >  | Espaço reservado para o qual você deve fornecer um valor |
|  [Texto dentro de colchetes]  |                Itens opcionais                 |
|      {Texto dentro de chaves}       |       Conjunto de itens necessários; Escolha um       |
|         Barra vertical (          |                       )                       |
|          Reticências (…)           |          Itens que podem ser repetidos           |

Retornar ao [menu](#menu)

## <a name="-dump"></a>-despejo

CertUtil [opções] [-dump]

CertUtil [opções] [-dump] arquivo

Despejar arquivos ou informações de configuração

[-f] [-Silent] [-divisão] [-p senha] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-asn"></a>-ASN

CertUtil [opções]-arquivo ASN [tipo]

Analisar arquivo ASN. 1

tipo: numérico CIFRAdo\_cadeia de caracteres\_\* tipo de decodificação

Retornar ao [menu](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [opções]-decodehex InFile outfile [tipo]

tipo: numérico CRIPTOGRAFAdo\_cadeia de caracteres\_\* tipo de codificação

[-f]

Retornar ao [menu](#menu)

## <a name="-decode"></a>-decodificar

CertUtil [opções] – decodificar outfile InFile

Decodificar arquivo codificado em base64

[-f]

Retornar ao [menu](#menu)

## <a name="-encode"></a>-codificar

CertUtil [opções] – codificar outfile InFile

Codificar arquivo em base64

[-f] [-UnicodeText]

Retornar ao [menu](#menu)

## <a name="-deny"></a>-negar

CertUtil [opções] – negar RequestId

Negar solicitação pendente

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-resubmit"></a>-reenviar

CertUtil [opções] – reenviar RequestId

Reenviar solicitação pendente

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-setattributes"></a>-SetAttributes

CertUtil [opções]-SetAttributes RequestId atributoString

Definir atributos para solicitação pendente

RequestId--ID de solicitação numérica de solicitação pendente

AttributeString--nomes de atributo de solicitação e pares de valor

- Os nomes e valores são separados por dois-pontos.
- Vários pares nome, valor são separados por nova linha.
- Exemplo: "CertificateTemplate:User\nEMail:User@Domain.com"
- Cada sequência "\n" é convertida em um separador de nova linha.

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [opções]-setextension RequestIdid sinalizadores {Long | Data | Cadeia de caracteres | \@InFile}

Definir a extensão para a solicitação pendente

RequestId--ID de solicitação numérica de uma solicitação pendente

ExtensionName--cadeia de caracteres ObjectId da extensão

Sinalizadores--0 é recomendado.  1 faz com que a extensão seja crítica, 2 a desativa, e 3 faz as duas.

Se o último parâmetro for numérico, ele será obtido como um longo.

Se ele puder ser analisado como uma data, ele será levado como uma data.

Se começar com '\@', o restante do token será o nome de arquivo que contém dados binários ou um despejo hexadecimal de texto ASCII.

Qualquer outra coisa é executada como uma cadeia de caracteres.

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-revoke"></a>-REVOKE

CertUtil [opções]-revogar SerialNumber [motivo]

Revogar certificado

SerialNumber: lista separada por vírgulas de números de série do certificado a revogar

Motivo: razão de revogação numérica ou simbólica

- 0: CRL_REASON_UNSPECIFIED: não especificado (padrão)
- 1: CRL_REASON_KEY_COMPROMISE: comprometimento de chave
- 2: CRL_REASON_CA_COMPROMISE: comprometimento da CA
- 3: CRL_REASON_AFFILIATION_CHANGED: associação alterada
- 4: CRL_REASON_SUPERSEDED: substituído
- 5: CRL_REASON_CESSATION_OF_OPERATION: cessação de operação
- 6: CRL_REASON_CERTIFICATE_HOLD: retenção de certificado
- 8: CRL_REASON_REMOVE_FROM_CRL: remover da CRL
- -1: cancelar revogação: cancelar revogação

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-isvalid"></a>-IsValid

CertUtil [opções]-IsValid SerialNumber | CertHash

Exibir disposição do certificado atual

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-getconfig"></a>-GetConfig

CertUtil [opções]-GetConfig

Obter cadeia de caracteres de configuração padrão

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-ping"></a>-ping

CertUtil [opções]-Ping [MaxSecondsToWait | CAMachineList]

Executar ping Active Directory interface de solicitação de serviços de certificados

CAMachineList--lista de nome da máquina CA separada por vírgulas

1. Para um único computador, use uma vírgula de terminação
2. Exibe o custo do site para cada computador de autoridade de certificação

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [opções]-CAInfo [InfoName [índice | ErrorCode]]

Exibir informações de AC

InfoName – indica a propriedade de autoridade de certificação a ser exibida (veja abaixo). Use "\*" para todas as propriedades.

Índice--índice de propriedades com base em zero opcional

ErrorCode--código de erro numérico

[-f] [-divisão] [-config Machine\CAName]

Sintaxe do argumento InfoName:

- arquivo: versão do arquivo
- produto: versão do produto
- exitcount: sair da contagem de módulos
- Exit [índice]: sair da descrição do módulo
- política: Descrição do módulo de política
- Nome: nome da autoridade de certificação
- sanitizedname: nome de autoridade de certificação limpo
- dsname: nome curto da AC corrigido (nome DS)
- SharedFolder: pasta compartilhada
- error1 ErrorCode: texto da mensagem de erro
- error2 ErrorCode: texto da mensagem de erro e código de erro
- tipo: tipo de CA
- informações: informações de CA
- pai: autoridade de certificação pai
- certcount: contagem de certificados da autoridade de certificação
- xchgcount: contagem de certificados de troca de AC
- kracount: contagem de certificados de KRA
- kraused: contagem usada de certificados KRA
- propidmax: PropId de autoridade de certificação máxima
- certstate [índice]: certificado de autoridade de certificação
- certversion [índice]: versão do certificado da autoridade de certificação
- certstatuscode [index]: status de verificação do certificado da AC
- crlstate [índice]: CRL
- krastate [índice]: certificado de KRA
- crossstate + [índice]: encaminhar certificado cruzado
- crossstate-[índice]: certificado cruzado para trás
- CERT [índice]: certificado de autoridade de certificação
- certchain [índice]: cadeia de certificados de autoridade de certificação
- certcrlchain [índice]: cadeia de certificados de autoridade de certificação com CRLs
- xchg [índice]: certificado de troca de AC
- xchgchain [índice]: cadeia de certificados de troca de AC
- xchgcrlchain [índice]: cadeia de certificados de troca de AC com CRLs
- kra [índice]: certificado de KRA
- Cross + [index]: encaminhar certificado cruzado
- Cross-[index]: certificado cruzado para trás
- CRL [índice]: CRL base
- deltacrl [índice]: CRL delta
- crlstatus [índice]: status de publicação da CRL
- deltacrlstatus [índice]: status de publicação da CRL delta
- DNS: nome DNS
- função: separação de funções
- anúncios: Advanced Server
- modelos: modelos
- CSP [índice]: URLs OCSP
- AIA [índice]: URLs de AIA
- CDP [índice]: URLs de CDP
- localename: nome da localidade da autoridade de certificação
- subjecttemplateoids: OIDs do modelo de assunto

Retornar ao [menu](#menu)

## <a name="-cacert"></a>-ca. cert

CertUtil [opções]-ca. cert OutCACertFile [índice]

Recuperar o certificado da autoridade de certificação

OutCACertFile: arquivo de saída

Índice: índice de renovação de certificado de CA (o padrão é o mais recente)

[-f] [-divisão] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-cachain"></a>-ca. Chain

CertUtil [opções]-ca. Chain OutCACertChainFile [índice]

Recuperar a cadeia de certificados da autoridade de certificação

OutCACertChainFile: arquivo de saída

Índice: índice de renovação de certificado de CA (o padrão é o mais recente)

[-f] [-divisão] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [opções]-GetCRL outfile [índice] [Delta]

Obter CRL

Índice: índice de CRL ou índice de chave (o padrão é CRL para a chave mais recente)

Delta: CRL delta (o padrão é CRL base)

[-f] [-divisão] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-crl"></a>-CRL

CertUtil [opções]-CRL [DD: hh | republicar] [Delta]

Publicar novas CRLs [ou somente CRLs delta]

DD: hh--novo período de validade da CRL em dias e horas

republicar--republicar as CRLs mais recentes

Delta--somente CRLs delta (o padrão é CRLs de base e Delta)

[-divisão] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-shutdown"></a>-desligar

CertUtil [opções] – desligar

Desligar Active Directory serviços de certificados

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [opções]-installCert [CACertFile]

Instalar certificado de autoridade de certificação

[-f] [-Silent] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [opções]-renewCert [ReuseKeys] [Machine\ParentCAName]

Renovar certificado de autoridade de certificação

Use-f para ignorar uma solicitação de renovação pendente e gerar uma nova solicitação.

[-f] [-Silent] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-schema"></a>-esquema

CertUtil [opções]-esquema [ext | Atrib | REVOGA

Despejar esquema de certificado

O padrão é a tabela de solicitação e certificado

Ext: tabela de extensão

Attrib: tabela de atributos

CRL: tabela CRL

[-divisão] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-view"></a>-exibição

CertUtil [opções]-exibir [fila | Log | LogFail | Revogado | Ext | Atrib | CRL] [CSV]

Despejar exibição de certificado

Fila: fila de solicitações

Log: certificados emitidos ou revogados, além de solicitações com falha

LogFail: solicitações com falha

Revogado: certificados revogados

Ext: tabela de extensão

Attrib: tabela de atributos

CRL: tabela CRL

CSV: saída como valores separados por vírgula

Para exibir a coluna StatusCode para todas as entradas:-out StatusCode

Para exibir todas as colunas da última entrada:-restringir "RequestId = = $"

Para exibir RequestId e disposição para três solicitações:-Restrict "RequestId > = 37, RequestId\<40"-out "RequestId, disposição"

Para exibir IDs de linha e números de CRL para todas as CRLs base:-restringir "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

Para exibir o número de CRL base 3:-v-restringir "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

Para exibir a tabela de CRL inteira: CRL

Use "Date [+ |-DD: hh]" para restrições de data

Use "Now + DD: hh" para uma data relativa à hora atual

[-Silent] [-divisão] [-config Machine\CAName] [-restringir restrição] [-out ColumnList]

Retornar ao [menu](#menu)

## <a name="-db"></a>-DB

CertUtil [opções]-BD

Despejar banco de dados bruto

[-config Machine\CAName] [-restringir restrição] [-out ColumnList]

Retornar ao [menu](#menu)

## <a name="-deleterow"></a>-deleteRow

CertUtil [opções]-deleteRow RowId | Data [solicitação | Certificado | Ext | Atrib | REVOGA

Excluir linha de banco de dados do servidor

Solicitação: solicitações com falha e pendentes (data de envio)

CERT: certificados expirados e revogados (data de expiração)

Ext: tabela de extensão

Attrib: tabela de atributos

CRL: tabela CRL (data de expiração)

Para excluir solicitações com falha e pendentes enviadas pela solicitação de 22 de janeiro de 2001:1/22/2001

Para excluir todos os certificados que expiraram em 22 de janeiro de 2001: certificado 1/22/2001

Para excluir a linha, os atributos e as extensões do certificado para RequestId 37:37

Para excluir CRLs que expiraram em 22 de janeiro de 2001:1/22/2001 CRL

[-f] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-backup"></a>-backup

CertUtil [opções]-backup BackupDirectory [incremental] [KeepLog]

Serviços de certificados de Active Directory de backup

BackupDirectory: diretório para armazenar dados de backup

Incremental: executar backup incremental somente (o padrão é backup completo)

KeepLog: preservar arquivos de log de banco de dados (o padrão é truncar arquivos de log)

[-f] [-config Machine\CAName] [-p senha]

Retornar ao [menu](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [opções]-backupDB BackupDirectory [incremental] [KeepLog]

Backup Active Directory banco de dados de serviços de certificados

BackupDirectory: diretório para armazenar arquivos de banco de dados de backup

Incremental: executar backup incremental somente (o padrão é backup completo)

KeepLog: preservar arquivos de log de banco de dados (o padrão é truncar arquivos de log)

[-f] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [opções]-backupKey BackupDirectory

Backup Active Directory certificado de serviços de certificados e chave privada

BackupDirectory: diretório para armazenar o arquivo PFX de backup

[-f] [-config Machine\CAName] [-p senha] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-restore"></a>-restaurar

CertUtil [opções] – restaurar BackupDirectory

Restaurar Active Directory serviços de certificados

BackupDirectory: diretório que contém os dados a serem restaurados

[-f] [-config Machine\CAName] [-p senha]

Retornar ao [menu](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [opções]-restoreDB BackupDirectory

Restaurar Active Directory banco de dados de serviços de certificados

BackupDirectory: diretório que contém os arquivos de banco de dados a serem restaurados

[-f] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [opções]-restoreKey BackupDirectory | PFXFile

Restaurar Active Directory certificado de serviços de certificados e chave privada

BackupDirectory: diretório que contém o arquivo PFX a ser restaurado

PFXFile: arquivo PFX a ser restaurado

[-f] [-config Machine\CAName] [-p senha]

Retornar ao [menu](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [opções]-importPFX [CertificateStoreName] PFXFile [modificadores]

Importar certificado e chave privada

CertificateStoreName: nome do repositório de certificados.  Consulte [-Store](#-store).

PFXFile: arquivo PFX a ser importado

Modificadores: lista separada por vírgulas de um ou mais dos seguintes:

1. AT_SIGNATURE: alterar a KeySpec para assinatura
2. AT_KEYEXCHANGE: alterar a KeySpec para a troca de chaves
3. Noexporte: tornar a chave privada não exportável
4. Nocert: não importar o certificado
5. Nochain: não importar a cadeia de certificados
6. Noroot: não importar o certificado raiz
7. Proteger: proteger chaves com senha
8. Noprotect: não proteger chaves com senha

O padrão é o repositório de computador pessoal.

[-f] [-usuário] [-p senha] [-provedor CSP]

Retornar ao [menu](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [opções]-dynamicfilelist

Exibir lista de arquivos dinâmicos

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [opções]-databaselocations

Exibir locais de banco de dados

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [opções]-hashfile InFile [HashAlgorithm]

Gerar e exibir o hash criptográfico em um arquivo

Retornar ao [menu](#menu)

## <a name="-store"></a>-Store

CertUtil [opções]-Store [CertificateStoreName [certid [arquivo_de_saída]]]

Despejar repositório de certificados

CertificateStoreName: nome do repositório de certificados. Exemplos:

- "My", "CA" (padrão), "root",
- "ldap:///CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? One? objectClass = certificationAuthority" (exibir certificados raiz)
- "ldap:///CN=CAName,CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificar certificados raiz)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (Exibir CRLs)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificados de AC corporativa)
- LDAP: (certificados de objeto de computador do AD)
- -LDAP de usuário: (certificados de objeto de usuário do AD)

Certid: token de correspondência de certificado ou CRL.  Pode ser um número de série, um certificado SHA-1, CRL, CTL ou hash de chave pública, um índice de certificado numérico (0, 1 e assim por diante), um índice de CRL numérico (. 0, 0,1 e assim por diante), um índice de CTL numérico (.. 0,.. 1 e assim por diante), uma chave pública, uma assinatura ou um ObjectId de extensão, um nome comum de entidade de certificado, um endereço de email, nome DNS ou UPN, um nome de contêiner de chave ou nome CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum de emissor de CRL. Muitas delas podem resultar em várias correspondências.

Arquivo_de_saída: arquivo para salvar o certificado correspondente

Use-User para acessar um repositório de usuários em vez de um repositório de computador.

Use-Enterprise para acessar um computador Enterprise Store.

Use-Service para acessar um repositório de serviços de máquina.

Use-GroupPolicy para acessar um repositório de política de grupo de computadores.

Exemplos:

- -NTAuth corporativo
- -Raiz da empresa 37
- -Meu 26e0aaaf000000000004 de usuário
- CA. 11

[-f] [-Enterprise] [-usuário] [-GroupPolicy] [-Silent] [-divisão] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [opções]-addstore CertificateStoreName InFile

Adicionar certificado ao repositório

CertificateStoreName: nome do repositório de certificados.  Consulte [-Store](#-store).

InFile: o certificado ou arquivo CRL a ser adicionado ao repositório.

[-f] [-Enterprise] [-usuário] [-GroupPolicy] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [opções]-delstore CertificateStoreName certid

Excluir certificado do repositório

CertificateStoreName: nome do repositório de certificados.  Consulte [-Store](#-store).

Certid: token de correspondência de certificado ou CRL.  Consulte [-Store](#-store).

[-Enterprise] [-usuário] [-GroupPolicy] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [opções]-verifystore CertificateStoreName [certid]

Verificar o certificado no repositório

CertificateStoreName: nome do repositório de certificados.  Consulte [-Store](#-store).

Certid: token de correspondência de certificado ou CRL.  Consulte [-Store](#-store).

[-Enterprise] [-usuário] [-GroupPolicy] [-Silent] [-divisão] [-DC DCName] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [opções]-repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Reparar Associação de chave ou atualizar propriedades de certificado ou descritor de segurança de chave

CertificateStoreName: nome do repositório de certificados.  Consulte [-Store](#-store).

CertIdList: lista separada por vírgulas de tokens de correspondência de certificado ou CRL. Consulte [a descrição de](#-store) certid da Store.

PropertyInfFile--arquivo INF contendo Propriedades externas:

```
[Properties]
     19 = Empty ; Add archived property, OR:
     19 =       ; Remove archived property

     11 = "{text}Friendly Name" ; Add friendly name property

     127 = "{hex}" ; Add custom hexadecimal property
         _continue_ = "00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f"
         _continue_ = "10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f"

     2 = "{text}" ; Add Key Provider Information property
       _continue_ = "Container=Container Name&"
       _continue_ = "Provider=Microsoft Strong Cryptographic Provider&"
       _continue_ = "ProviderType=1&"
       _continue_ = "Flags=0&"
       _continue_ = "KeySpec=2"

     9 = "{text}" ; Add Enhanced Key Usage property
       _continue_ = "1.3.6.1.5.5.7.3.2,"
       _continue_ = "1.3.6.1.5.5.7.3.1,"
```

[-f] [-Enterprise] [-usuário] [-GroupPolicy] [-Silent] [-divisão] [-provedor CSP]

Retornar ao [menu](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [opções]-viewstore [CertificateStoreName [certid [arquivo_de_saída]]]

Despejar repositório de certificados

CertificateStoreName: nome do repositório de certificados. Exemplos:

- "My", "CA" (padrão), "root",
- "ldap:///CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? One? objectClass = certificationAuthority" (exibir certificados raiz)
- "ldap:///CN=CAName,CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificar certificados raiz)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (Exibir CRLs)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificados de AC corporativa)
- LDAP: (certificados de objeto de máquina do AD)
- -LDAP de usuário: (certificados de objeto de usuário do AD)

Certid: token de correspondência de certificado ou CRL. Pode ser um número de série, um certificado SHA-1, CRL, CTL ou hash de chave pública, um índice de certificado numérico (0, 1 e assim por diante), um índice de CRL numérico (. 0, 0,1 e assim por diante), um índice de CTL numérico (.. 0,.. 1 e assim por diante), uma chave pública, uma assinatura ou um ObjectId de extensão, um nome comum de entidade de certificado, um endereço de email, nome DNS ou UPN, um nome de contêiner de chave ou nome CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum de emissor de CRL. Muitas delas podem resultar em várias correspondências.

Arquivo_de_saída: arquivo para salvar o certificado correspondente

Use-User para acessar um repositório de usuários em vez de um repositório de computador.

Use-Enterprise para acessar um computador Enterprise Store.

Use-Service para acessar um repositório de serviços de máquina.

Use-GroupPolicy para acessar um repositório de política de grupo de computadores.

Exemplos:

1. -NTAuth corporativo
2. -Raiz da empresa 37
3. -Meu 26e0aaaf000000000004 de usuário
4. CA. 11

[-f] [-Enterprise] [-usuário] [-GroupPolicy] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [opções]-viewdelstore [CertificateStoreName [certid [arquivo_de_saída]]]

Excluir certificado do repositório

CertificateStoreName: nome do repositório de certificados. Exemplos:

- "My", "CA" (padrão), "root",
- "ldap:///CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? One? objectClass = certificationAuthority" (exibir certificados raiz)
- "ldap:///CN=CAName,CN=Certification autoridades, CN = Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificar certificados raiz)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (Exibir CRLs)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = CPANDL, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificados de AC corporativa)
- LDAP: (certificados de objeto de máquina do AD)
- -LDAP de usuário: (certificados de objeto de usuário do AD)

Certid: token de correspondência de certificado ou CRL. Pode ser um número de série, um certificado SHA-1, CRL, CTL ou hash de chave pública, um índice de certificado numérico (0, 1 e assim por diante), um índice de CRL numérico (. 0, 0,1 e assim por diante), um índice de CTL numérico (.. 0,.. 1 e assim por diante), uma chave pública, uma assinatura ou um ObjectId de extensão, um nome comum de entidade de certificado, um endereço de email, nome DNS ou UPN, um nome de contêiner de chave ou nome CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum de emissor de CRL. Muitas delas podem resultar em várias correspondências.

Arquivo_de_saída: arquivo para salvar o certificado correspondente

Use-User para acessar um repositório de usuários em vez de um repositório de computador.

Use-Enterprise para acessar um computador Enterprise Store.

Use-Service para acessar um repositório de serviços de máquina.

Use-GroupPolicy para acessar um repositório de política de grupo de computadores.

Exemplos:

1. -NTAuth corporativo
2. -Raiz da empresa 37
3. -Meu 26e0aaaf000000000004 de usuário
4. CA. 11

[-f] [-Enterprise] [-usuário] [-GroupPolicy] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-dspublish"></a>-dsPublish

CertUtil [opções]-dsPublish CertFile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | Usuário | Tradução

CertUtil [opções]-dsPublish CRLFile [DSCDPContainer [DSCDPCN]]

Publicar certificado ou CRL para Active Directory

CertFile: arquivo de certificado a ser publicado

NTAuthCA: publicar o certificado na Enterprise Store do DS

RootCA: publicar o certificado no repositório de raiz confiável do DS

SubCA: publicar o certificado de autoridade de certificação no objeto de AC DS

CrossCA: publicar objeto de autoridade de certificação de certificado cruzado para DS

KRA: publicar certificado no objeto agente de recuperação de chave DS

Usuário: publicar certificado no objeto DS do usuário

Computador: publicar o certificado no objeto DS do computador

CRLFile: arquivo de CRL a ser publicado

DSCDPContainer: CN do contêiner de CDP do DS, geralmente o nome do computador da autoridade de certificação

DSCDPCN: CN do objeto de CDP DS, geralmente com base no nome curto e índice de chave da AC corrigido

Use-f para criar o objeto DS.

[-f] [-usuário] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [opções]-ADTemplate [modelo]

Exibir modelos do AD

[-f] [-usuário] [-ut] [-MT] [-DC DCName]

## <a name="-template"></a>-Modelo

CertUtil [opções] – modelo [modelo]

Exibir modelos de política de registro

[-f] [-usuário] [-Silent] [-PolicyServer URLOrId] [-Anônimo] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nome de usuário] [-p senha]

Retornar ao [menu](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [opções] – modelo TemplateCAs

Exibir CAs para o modelo

[-f] [-usuário] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-catemplates"></a>-CATemplates

CertUtil [opções]-CATemplates [modelo]

Exibir modelos para CA

[-f] [-usuário] [-ut] [-MT] [-config Machine\CAName] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [opções]-SetCASites [conjunto] [SiteName]

CertUtil [opções]-SetCASites verificar [SiteName]

CertUtil [opções] – exclusão de SetCASites

Definir, verificar ou excluir nomes de site da AC

- Use a opção-config para direcionar uma única AC (o padrão é todas as CAs)
- *SiteName* só é permitido quando direcionado a uma única AC
- Use-f para substituir erros de validação para o *SiteName* especificado
- Use-f para excluir todos os nomes de site da AC

[-f] [-config Machine\CAName] [-DC DCName]

> [!NOTE]
> Para obter mais informações sobre como configurar o reconhecimento de site de CAs para Active Directory Domain Services (AD DS), consulte [AD DS reconhecimento de site para clientes do AD CS e PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Retornar ao [menu](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [opções]-enrollmentServerURL [URL AuthenticationType [prioridade] [modificadores]]

CertUtil [opções]-enrollmentServerURL de exclusão de URL

Exibir, adicionar ou excluir as URLs de servidor de registro associadas a uma AC

AuthenticationType: especifique um dos seguintes métodos de autenticação de cliente ao adicionar uma URL

1. Kerberos: usar credenciais SSL Kerberos
2. Nome de usuário: usar conta nomeada para credenciais SSL
3. ClientCertificate: usar credenciais SSL do certificado X. 509
4. Anônimo: usar credenciais SSL anônimas

excluir: exclui a URL especificada associada à autoridade de certificação

Prioridade: o padrão é ' 1 ' se não for especificado ao adicionar uma URL

Modificadores – lista separada por vírgulas de um ou mais dos seguintes:

1. AllowRenewalsOnly: somente as solicitações de renovação podem ser enviadas para esta AC por meio desta URL
2. AllowKeyBasedRenewal: permite o uso de um certificado que não tem nenhuma conta associada no AD. Isso se aplica somente ao modo ClientCertificate e AllowRenewalsOnly

[-config Machine\CAName] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [opções]-ADCA [CAName]

Exibir CAs do AD

[-f] [-divisão] [-DC DCName]

Retornar ao [menu](#menu)

## <a name="-ca"></a>-CA

CertUtil [opções]-AC [CAName | TemplateName

Exibir CAs da política de registro

[-f] [-usuário] [-Silent] [-divisão] [-PolicyServer URLOrId] [-Anônimo] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nome de usuário] [-p senha]

Retornar ao [menu](#menu)

## <a name="-policy"></a>-Política

Exibir política de registro

[-f] [-usuário] [-Silent] [-divisão] [-PolicyServer URLOrId] [-Anônimo] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nome de usuário] [-p senha]

Retornar ao [menu](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [opções]-PolicyCache [excluir]

Exibir ou excluir entradas do cache da política de registro

excluir: Excluir entradas de cache do servidor de política

-f: Use-f para excluir todas as entradas de cache

[-f] [-usuário] [-PolicyServer URLOrId]

Retornar ao [menu](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [opções]-CredStore [URL]

CertUtil [opções]-CredStore URL Add

CertUtil [opções]-CredStore de exclusão de URL

Exibir, adicionar ou excluir entradas de repositório de credenciais

URL: URL de destino.  Use \* para corresponder a todas as entradas. Use https://machine\* para corresponder a um prefixo de URL.

Adicionar: adicionar uma entrada de repositório de credenciais. As credenciais SSL também devem ser especificadas.

excluir: Excluir entradas do repositório de credenciais

-f: Use-f para substituir uma entrada ou excluir várias entradas.

[-f] [-usuário] [-Silent] [-Anônimo] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName nome de usuário] [-p senha]

Retornar ao [menu](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [opções]-InstallDefaultTemplates

Instalar modelos de certificado padrão

[-DC DCName]

Retornar ao [menu](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [opções]-URLCache [URL | CRL | \* [excluir]]

Exibir ou excluir entradas de cache de URL

URL: URL armazenada em cache

CRL: operar em todas as URLs de CRL em cache somente

\*: operar em todas as URLs em cache

excluir: excluir URLs relevantes do cache local do usuário atual

Use-f para forçar a busca de uma URL específica e a atualização do cache.

[-f] [-divisão]

Retornar ao [menu](#menu)

## <a name="-pulse"></a>-Pulse

CertUtil [opções]-pulso

Eventos de registro automático de pulso

[-usuário]

Retornar ao [menu](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [opções]-MachineInfo DomainName\MachineName $

Exibir informações do objeto de computador Active Directory

Retornar ao [menu](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [opções]-DCInfo [domínio] [verificar | DeleteBad | DeleteAll

Exibir informações do controlador de domínio

O padrão é exibir certificados DC sem verificação

[-f] [-usuário] [-urlfetch] [-DC DCName] [-t tempo limite]

> [!TIP]
> A capacidade de especificar um domínio de Active Directory Domain Services (AD DS) **[domain]** e especificar um controlador de domínio ( **-DC**) foi adicionada ao Windows Server 2012. Para executar o comando com êxito, você deve usar uma conta que seja membro de **Administradores de domínio** ou **Administradores de empresa**. As modificações de comportamento desse comando são as seguintes:</br>> 1.  Se um domínio não for especificado e um controlador de domínio específico não for especificado, essa opção retornará uma lista de controladores de domínio a serem processados a partir do controlador de domínio padrão.</br>> 2.  Se um domínio não for especificado, mas um controlador de domínio for especificado, um relatório dos certificados no controlador de domínio especificado será gerado.</br>> 3.  Se um domínio for especificado, mas um controlador de domínio não for especificado, uma lista de controladores de domínio será gerada juntamente com os relatórios nos certificados para cada controlador de domínio na lista.</br>> 4.  Se o domínio e o controlador de domínio forem especificados, uma lista de controladores de domínio será gerada a partir do controlador de domínio de destino. Um relatório dos certificados para cada controlador de domínio na lista também é gerado.

Por exemplo, suponha que haja um domínio chamado CPANDL com um controlador de domínio chamado CPANDL-DC1. Você pode executar o seguinte comando para recuperar uma lista de controladores de domínio e seus certificados de CPANDL-DC1: certutil-DC CPANDL-DC1-dcinfo cpandl

Retornar ao [menu](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [opções]-EntInfo DomainName\MachineName $

[-f] [-usuário]

Retornar ao [menu](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [opções]-TCAInfo [DomainDN |-]

Exibir informações de AC

[-f] [-Enterprise] [-usuário] [-urlfetch] [-DC DCName] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [opções]-SCInfo [Readername [CRYPT_DELETEKEYSET]]

Exibir informações de cartão inteligente

CRYPT_DELETEKEYSET: excluir todas as chaves no cartão inteligente

[-Silent] [-divisão] [-urlfetch] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [opções]-SCRoots atualização [+] [InputRootFile] [Readername]

CertUtil [opções]-SCRoots salvar \@OutputRootFile [Readername]

CertUtil [opções]-exibição de SCRoots [InputRootFile | Readername]

CertUtil [opções]-SCRoots excluir [Readername]

Gerenciar certificados raiz do cartão inteligente

[-f] [-divisão] [-p senha]

Retornar ao [menu](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [opções]-verifykeys [KeyContainerName cacertde]

Verificar conjunto de chaves pública/privada

KeyContainerName: nome do contêiner de chave da chave a ser verificada. O padrão é as chaves do computador.  Use-User para chaves de usuário.

Cacertm: arquivo de certificado de autenticação ou criptografia

Se nenhum argumento for especificado, cada certificado de autoridade de certificação de assinatura será verificado em relação à sua chave privada.

Esta operação só pode ser executada em uma autoridade de certificação local ou em chaves locais.

[-f] [-usuário] [-Silent] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-verify"></a>-verificar

CertUtil [opções]-verificar CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [opções]-verificar CertFile [CACertFile [CrossedCACertFile]]

CertUtil [opções]-verificar o cacertde CRLFile [IssuedCertFile]

CertUtil [opções]-verificar o cacertde CRLFile [DeltaCRLFile]

Verificar certificado, CRL ou cadeia

CertFile: certificado a ser verificado

ApplicationPolicyList: lista separada por vírgulas opcional de ObjectIds de política de aplicativo necessárias

IssuancePolicyList: lista separada por vírgulas opcional de ObjectIds de política de emissão necessárias

CAcert: certificado de AC emissor opcional para verificação

CrossedCACertFile: certificado opcional entre certificados por CertFile

CRLFile: CRL a ser verificada

IssuedCertFile: certificado emitido opcional coberto por CRLFile

DeltaCRLFile: CRL delta opcional

Se ApplicationPolicyList for especificado, a criação de cadeia será restrita às cadeias válidas para as políticas de aplicativo especificadas.

Se IssuancePolicyList for especificado, a criação de cadeia será restrita às cadeias válidas para as políticas de emissão especificadas.

Se o CACertFile for especificado, os campos no cacertm serão verificados em relação a CertFile ou CRLFile.

Se o CACertFile não for especificado, CertFile será usado para criar e verificar uma cadeia completa.

Se cacertvalue e CrossedCACertFile forem especificados, os campos em cacertde e CrossedCACertFile serão verificados em relação a CertFile.

Se IssuedCertFile for especificado, os campos em IssuedCertFile serão verificados em relação a CRLFile.

Se DeltaCRLFile for especificado, os campos em DeltaCRLFile serão verificados em relação a CRLFile.

[-f] [-Enterprise] [-usuário] [-Silent] [-divisão] [-urlfetch] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [opções]-verifyCTL CTLObject [CertDir] [CertFile]

Verificar CTL de certificados AuthRoot ou não permitido

CTLObject: identifica a CTL a ser verificada:

- AuthRootWU: ler AuthRoot CAB e certificados correspondentes do cache de URL. Use-f para baixar de Windows Update em vez disso.
- DisallowedWU: ler os certificados não permitidos CAB e o arquivo de repositório de certificados não permitido do cache de URL.  Use-f para baixar de Windows Update em vez disso.
- AuthRoot: ler registro em cache AuthRoot CTL.  Use with-f e um CertFile que ainda não é confiável para forçar a atualização do registro AuthRoot armazenado em cache e de certificados confiáveis não permitidos.
- Não permitido: ler CTL de certificados não permitidos armazenados em cache do registro. -f tem o mesmo comportamento que com AuthRoot.
- CTLFileName: File ou http: caminho para CTL ou CAB

CertDir: pasta que contém certificados que correspondem a entradas de CTL. Um caminho http: Folder deve terminar com um separador de caminho. Se uma pasta não for especificada com AuthRoot ou não permitido, vários locais serão pesquisados para certificados correspondentes: repositórios de certificados locais, recursos crypt32. dll e o cache de URL local. Use-f para baixar de Windows Update quando necessário. Caso contrário, o padrão será a mesma pasta ou site da CTLObject.

CertFile: arquivo que contém certificado (s) para verificar. Os certificados serão correspondidos em relação a entradas de CTL e resultados de correspondência serão exibidos. Suprime a maior parte da saída padrão.

[-f] [-usuário] [-divisão]

Retornar ao [menu](#menu)

## <a name="-sign"></a>-assinar

CertUtil [opções]-assinar FileList | SerialNumber | Corfilelist da CRL [StartDate + DD: hh] [+ SerialNumberlist |-SerialNumberlist |-ObjectIdlist | \@Extensãofile]

CertUtil [opções]-assinar FileList | SerialNumber | Lista de arquivos da CRL [#HashAlgorithm] [+ AlternateSignatureAlgorithm |-AlternateSignatureAlgorithm]

Assinar novamente a CRL ou o certificado

Infilelist: lista separada por vírgulas de certificados ou arquivos CRL para modificar e assinar novamente

SerialNumber: número de série do certificado a ser criado. O período de validade e outras opções não devem estar presentes.

CRL: Crie uma CRL vazia. O período de validade e outras opções não devem estar presentes.

Outfilelist: lista separada por vírgulas de certificados modificados ou arquivos de saída de CRL. O número de arquivos deve corresponder a FileList.

StartDate + DD: hh: novo período de validade: data adicional opcional; período de validade de dias e horas opcionais; Se ambos forem especificados, use um separador de sinal de adição (+). Use "Now [+ DD: hh]" para iniciar na hora atual. Use "Never" para não ter nenhuma data de expiração (somente para CRLs).

SerialNumberlist: lista de números de série separados por vírgula para adicionar ou remover

ObjectIdlist: lista de ObjectId da extensão separada por vírgula a ser removida

\@extensão: arquivo INF contendo extensões para atualizar ou remover:

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm: nome do algoritmo de hash precedido por um # Sign

AlternateSignatureAlgorithm: especificador de algoritmo de assinatura alternativo

Um sinal de subtração faz com que os números de série e extensões sejam removidos. Um sinal de adição faz com que os números de série sejam adicionados a uma CRL. Ao remover itens de uma CRL, a lista pode conter tanto números de série quanto ObjectIds. Um sinal de subtração antes de AlternateSignatureAlgorithm faz com que o formato de assinatura herdado seja usado. Um sinal de adição antes de AlternateSignatureAlgorithm faz com que o formato de assinatura Alternature seja usado. Se AlternateSignatureAlgorithm não for especificado, o formato de assinatura no certificado ou CRL será usado.

[-nullsign] [-f] [-Silent] [-Certid do certificado]

Retornar ao [menu](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [opções]-vroot [excluir]

Criar/excluir compartilhamentos de arquivos e raízes virtuais da Web

Retornar ao [menu](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [opções]-vocsproot [excluir]

Criar/excluir raízes virtuais da Web para o proxy Web OCSP

Retornar ao [menu](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [opções]-addEnrollmentServer Kerberos | Nome de usuário | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Adicionar um aplicativo de servidor de registro

Adicione um aplicativo de servidor de registro e um pool de aplicativos, se necessário, para a autoridade de certificação especificada. Esse comando não instala binários ou pacotes. Um dos métodos de autenticação a seguir com os quais o cliente se conecta a um servidor de registro de certificado.

- Kerberos: usar credenciais SSL Kerberos
- Nome de usuário: usar conta nomeada para credenciais SSL
- ClientCertificate: usar credenciais SSL do certificado X. 509
- AllowRenewalsOnly: somente as solicitações de renovação podem ser enviadas para esta AC por meio desta URL
- AllowKeyBasedRenewal – permite o uso de um certificado que não tem nenhuma conta associada no AD. Isso se aplica somente ao modo ClientCertificate e AllowRenewalsOnly.

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [opções]-deleteEnrollmentServer Kerberos | Nome de usuário | ClientCertificate

Excluir um aplicativo de servidor de registro

Exclua um aplicativo de servidor de registro e um pool de aplicativos, se necessário, para a autoridade de certificação especificada. Esse comando não remove binários ou pacotes. Um dos métodos de autenticação a seguir com os quais o cliente se conecta a um servidor de registro de certificado.

1. Kerberos: usar credenciais SSL Kerberos
2. Nome de usuário: usar conta nomeada para credenciais SSL
3. ClientCertificate: usar credenciais SSL do certificado X. 509

[-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [opções]-addPolicyServer Kerberos | Nome de usuário | ClientCertificate [KeyBasedRenewal]

Adicionar um aplicativo de servidor de política

Adicione um aplicativo de servidor de política e um pool de aplicativos, se necessário. Esse comando não instala binários ou pacotes. Um dos seguintes métodos de autenticação com os quais o cliente se conecta a um servidor de política de certificado:

- Kerberos: usar credenciais SSL Kerberos
- Nome de usuário: usar conta nomeada para credenciais SSL
- ClientCertificate: usar credenciais SSL do certificado X. 509
- KeyBasedRenewal: somente as políticas que contêm modelos KeyBasedRenewal são retornadas ao cliente. Esse sinalizador se aplica somente à autenticação de nome de usuário e ClientCertificate.

Retornar ao [menu](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [opções]-deletePolicyServer Kerberos | Nome de usuário | ClientCertificate [KeyBasedRenewal]

Excluir um aplicativo de servidor de política

Exclua um aplicativo de servidor de política e um pool de aplicativos, se necessário. Esse comando não remove binários ou pacotes. Um dos seguintes métodos de autenticação com os quais o cliente se conecta a um servidor de política de certificado:

1. Kerberos: usar credenciais SSL Kerberos
2. Nome de usuário: usar conta nomeada para credenciais SSL
3. ClientCertificate: usar credenciais SSL do certificado X. 509
4. KeyBasedRenewal: servidor de políticas do KeyBasedRenewal

Retornar ao [menu](#menu)

## <a name="-oid"></a>-OID

CertUtil [opções]-OID ObjectId [DisplayName | excluir [LanguageID [tipo]]]

CertUtil [opções]-OID GroupId

CertUtil [opções]-OID AlgId | AlgorithmName [GroupId]

Exibir ObjectId ou definir nome de exibição

- ObjectId--ObjectId a exibir ou adicionar nome de exibição
- GroupId--número decimal de GroupId para ObjectIds a serem enumeradas
- AlgId--AlgId hexadecimal para ObjectId a ser pesquisada
- AlgorithmName--nome do algoritmo para o ObjectId a ser pesquisado
- DisplayName--nome de exibição para armazenar no DS
- excluir--excluir nome de exibição
- LanguageID--ID do idioma (o padrão é atual: 1033)
- Tipo--tipo de objeto do DS a ser criado: 1 para modelo (padrão), 2 para política de emissão, 3 para política de aplicativo
- Use-f para criar o objeto DS.

[-f]

Retornar ao [menu](#menu)

## <a name="-error"></a>-erro

CertUtil [opções]-erro ErrorCode

Exibir texto da mensagem de código de erro

Retornar ao [menu](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [opções]-getreg [{Ca | restaurar | política | sair | modelo | registrar | cadeia | PolicyServers}\[ProgId\]] [RegistryValueName]

Exibir valor do registro

AC: usar a chave do registro da autoridade de certificação

restaurar: usar a chave de registro de restauração da autoridade de certificação

política: usar chave do registro do módulo de política

sair: usar a chave do registro do primeiro módulo de saída

modelo: usar chave do registro de modelo (use-User para modelos de usuário)

registrar: usar a chave do registro de registro (use-User para o contexto do usuário)

cadeia: Use a chave do registro de configuração da cadeia

PolicyServers: usar chave do registro de servidores de política

ProgId: usar o ProgId do módulo de política ou saída (nome da subchave do registro)

RegistryValueName: nome do valor do registro (use "nome\*" para corresponder ao prefixo)

Valor: novo valor de registro numérico, de cadeia de caracteres ou de data ou nome de arquivo. Se um valor numérico começar com "+" ou "-", os bits especificados no novo valor serão definidos ou apagados no valor do registro existente.

Se um valor de cadeia de caracteres começar com "+" ou "-" e o valor existente for um valor REG_MULTI_SZ, a cadeia de caracteres será adicionada ou removida do valor de registro existente. Para forçar a criação de um valor de REG_MULTI_SZ, adicione um "\n" ao final do valor da cadeia de caracteres.

Se o valor começar com "\@", o restante do valor será o nome do arquivo que contém a representação de texto hexadecimal de um valor binário. Se não se referir a um arquivo válido, ele será analisado como [data] [+ |-] [DD: hh]--uma data opcional mais ou menos dias e horas opcionais. Se ambos forem especificados, use um separador de sinal de adição (+) ou sinal de subtração (-). Use "Now + DD: hh" para uma data relativa à hora atual.

Use "chain\ChainCacheResyncFiletime \@Now" para liberar com eficiência as CRLs em cache.

[-f] [-usuário] [-GroupPolicy] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [opções]-setreg [{Ca | restaurar | política | sair | modelo | registrar | cadeia | PolicyServers}\[ProgId\]] valor de RegistryValueName

Definir valor do registro

AC: usar a chave do registro da autoridade de certificação

restaurar: usar a chave de registro de restauração da autoridade de certificação

política: usar chave do registro do módulo de política

sair: usar a chave do registro do primeiro módulo de saída

modelo: usar chave do registro de modelo (use-User para modelos de usuário)

registrar: usar a chave do registro de registro (use-User para o contexto do usuário)

cadeia: Use a chave do registro de configuração da cadeia

PolicyServers: usar chave do registro de servidores de política

ProgId: usar o ProgId do módulo de política ou saída (nome da subchave do registro)

RegistryValueName: nome do valor do registro (use "nome\*" para corresponder ao prefixo)

Valor: novo valor de registro numérico, de cadeia de caracteres ou de data ou nome de arquivo. Se um valor numérico começar com "+" ou "-", os bits especificados no novo valor serão definidos ou apagados no valor do registro existente.

Se um valor de cadeia de caracteres começar com "+" ou "-" e o valor existente for um valor REG_MULTI_SZ, a cadeia de caracteres será adicionada ou removida do valor de registro existente. Para forçar a criação de um valor de REG_MULTI_SZ, adicione um "\n" ao final do valor da cadeia de caracteres.

Se o valor começar com "\@", o restante do valor será o nome do arquivo que contém a representação de texto hexadecimal de um valor binário. Se não se referir a um arquivo válido, ele será analisado como [data] [+ |-] [DD: hh]--uma data opcional mais ou menos dias e horas opcionais. Se ambos forem especificados, use um separador de sinal de adição (+) ou sinal de subtração (-). Use "Now + DD: hh" para uma data relativa à hora atual.

Use "chain\ChainCacheResyncFiletime \@Now" para liberar com eficiência as CRLs em cache.

[-f] [-usuário] [-GroupPolicy] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [opções]-delreg [{Ca | restaurar | política | sair | modelo | registrar | cadeia | PolicyServers}\[ProgId\]] [RegistryValueName]

Excluir valor do registro

AC: usar a chave do registro da autoridade de certificação

restaurar: usar a chave de registro de restauração da autoridade de certificação

política: usar chave do registro do módulo de política

sair: usar a chave do registro do primeiro módulo de saída

modelo: usar chave do registro de modelo (use-User para modelos de usuário)

registrar: usar a chave do registro de registro (use-User para o contexto do usuário)

cadeia: Use a chave do registro de configuração da cadeia

PolicyServers: usar chave do registro de servidores de política

ProgId: usar o ProgId do módulo de política ou saída (nome da subchave do registro)

RegistryValueName: nome do valor do registro (use "nome\*" para corresponder ao prefixo)

Valor: novo valor de registro numérico, de cadeia de caracteres ou de data ou nome de arquivo. Se um valor numérico começar com "+" ou "-", os bits especificados no novo valor serão definidos ou apagados no valor do registro existente.

Se um valor de cadeia de caracteres começar com "+" ou "-" e o valor existente for um valor REG_MULTI_SZ, a cadeia de caracteres será adicionada ou removida do valor de registro existente. Para forçar a criação de um valor de REG_MULTI_SZ, adicione um "\n" ao final do valor da cadeia de caracteres.

Se o valor começar com "\@", o restante do valor será o nome do arquivo que contém a representação de texto hexadecimal de um valor binário. Se não se referir a um arquivo válido, ele será analisado como [data] [+ |-] [DD: hh]--uma data opcional mais ou menos dias e horas opcionais. Se ambos forem especificados, use um separador de sinal de adição (+) ou sinal de subtração (-). Use "Now + DD: hh" para uma data relativa à hora atual.

Use "chain\ChainCacheResyncFiletime \@Now" para liberar com eficiência as CRLs em cache.

[-f] [-usuário] [-GroupPolicy] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [opções]-ImportKMS UserKeyAndCertFile [certid]

Importar chaves de usuário e certificados no banco de dados do servidor para arquivamento de chaves

UserKeyAndCertFile--arquivo de dados que contém chaves privadas do usuário e certificados a serem arquivados.  Isso pode ser qualquer um dos seguintes:

- Arquivo de exportação do servidor de gerenciamento de chaves do Exchange (KMS)
- Arquivo PFX

Certid: token de correspondência do certificado de descriptografia do arquivo de exportação KMS.  Consulte [-Store](#-store).

Use-f para importar certificados não emitidos pela autoridade de certificação.

[-f] [-Silent] [-divisão] [-config Machine\CAName] [-p senha] [-symkeyalg SymmetricKeyAlgorithm [, KeyLength]]

Retornar ao [menu](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [opções]-ImportCert CertFile [ExistingRow]

Importar um arquivo de certificado para o banco de dados

Use ExistingRow para importar o certificado no lugar de uma solicitação pendente para a mesma chave.

Use-f para importar certificados não emitidos pela autoridade de certificação.

A CA também pode precisar ser configurada para dar suporte à importação de certificado estrangeiro: certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

Retornar ao [menu](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [opções]-GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [opções]-GetKey SearchToken script OutputScriptFile

CertUtil [opções]-GetKey SearchToken recuperar | recuperar OutputFileBaseName

Recuperar BLOB de recuperação de chave privada arquivada, gerar um script de recuperação ou recuperar chaves arquivadas

script: gerar um script para recuperar e recuperar chaves (comportamento padrão se vários candidatos à recuperação correspondentes forem encontrados ou se o arquivo de saída não for especificado).

recuperar: recuperar um ou mais blobs de recuperação de chave (comportamento padrão se exatamente um candidato de recuperação correspondente for encontrado e se o arquivo de saída for especificado)

recuperar: recuperar e recuperar chaves privadas em uma única etapa (requer certificados e chaves privadas do agente de recuperação de chave)

SearchToken: usado para selecionar as chaves e os certificados a serem recuperados.

Pode ser qualquer um dos seguintes:

1. Nome comum do certificado
2. Número de série do certificado
3. Hash de SHA-1 de certificado (impressão digital)
4. Hash KeyId SHA-1 de certificado (identificador de chave da entidade)
5. Nome do solicitante (domínio \ usuário)
6. UPN (usuário\@domínio)

RecoveryBlobOutFile: o arquivo de saída que contém uma cadeia de certificados e uma chave privada associada ainda é criptografado para um ou mais certificados de agente de recuperação de chave.

OutputScriptFile: arquivo de saída que contém um script em lotes para recuperar e recuperar chaves privadas.

OutputFileBaseName: nome base do arquivo de saída. Para recuperar, qualquer extensão é truncada e uma cadeia de caracteres específica de certificado e a extensão. rec são acrescentadas para cada blob de recuperação de chave.  Cada arquivo contém uma cadeia de certificados e uma chave privada associada, ainda criptografada para um ou mais certificados de agente de recuperação de chave. Para a recuperação, qualquer extensão é truncada e a extensão. p12 é anexada.  Contém as cadeias de certificados recuperadas e as chaves privadas associadas, armazenadas como um arquivo PFX.

[-f] [-UnicodeText] [-Silent] [-config Machine\CAName] [-p senha] [-Proteger para SAMNameAndSIDList] [-provedor CSP]

Retornar ao [menu](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [opções]-RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Recuperar chave privada arquivada

[-f] [-usuário] [-Silent] [-divisão] [-p senha] [-Proteger para SAMNameAndSIDList] [-provedor CSP] [-t tempo limite]

Retornar ao [menu](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [opções]-MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList: lista de arquivos de entrada PFX separados por vírgula

PFXOutFile: arquivo de saída PFX

ExtendedProperties: incluir propriedades estendidas

A senha especificada na linha de comando é uma lista de senhas separadas por vírgula.  Se mais de uma senha for especificada, a última senha será usada para o arquivo de saída.  Se apenas uma senha for fornecida ou se a última senha for "\*", o usuário será solicitado a fornecer a senha do arquivo de saída.

[-f] [-usuário] [-divisão] [-p senha] [-Proteger para SAMNameAndSIDList] [-provedor CSP]

Retornar ao [menu](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [opções]-ConvertEPF PFXInFileList EPFOutFile [Cast | Cast-] [V3CACertId] [, Salt]

Converter arquivos PFX em arquivo EPF

PFXInFileList: lista de arquivos de entrada PFX separados por vírgula

EPF: arquivo de saída EPF

conversão: usar criptografia de 64 de conversão

conversão-: usar criptografia de 64 de conversão (exportação)

V3CACertId: token de correspondência de certificado de autoridade de certificação v3.  Consulte [a descrição de](#-store) certid da Store.

Salt: cadeia de caracteres de Salt do arquivo de saída EPF

A senha especificada na linha de comando é uma lista de senhas separadas por vírgula. Se mais de uma senha for especificada, a última senha será usada para o arquivo de saída.  Se apenas uma senha for fornecida ou se a última senha for "\*", o usuário será solicitado a fornecer a senha do arquivo de saída.

[-f] [-Silent] [-divisão] [-DC DCName] [-p senha] [-provedor CSP]

Retornar ao [menu](#menu)

## <a name="options"></a>{1&gt;Opções&lt;1}

Esta seção define as opções que você pode especificar com o comando.

|{1&gt;Opções&lt;1}|Descrição|
|-------|-----------|
|-nullsign|Usar hash de dados como assinatura|
|-f|Forçar substituição|
|-enterprise|Usar o repositório de certificados do registro empresarial do computador local|
|-usuário|Usar chaves de HKEY_CURRENT_USER ou repositório de certificados|
|-GroupPolicy|Usar Política de Grupo repositório de certificados|
|-UT|Exibir modelos do usuário|
|-MT|Exibir modelos de máquina|
|-Unicode|Gravar saída redirecionada em Unicode|
|-UnicodeText|Gravar arquivo de saída em Unicode|
|-GMT|Exibir horários como GMT|
|-segundos|Exibir tempos com segundos e milissegundos|
|-silencioso|Usar sinalizador silencioso para adquirir contexto cript|
|-Divisão|Dividir elementos ASN internos. 1 e salvar em arquivos|
|-v|Operação detalhada|
|-PrivateKey|Exibir dados de senha e de chave privada|
|-fixar PIN|PIN do cartão inteligente|
|-urlfetch|Recuperar e verificar certificados AIA e CRLs de CDP|
|-config Machine\CAName|Cadeia de caracteres de nome de computador e CA|
|-PolicyServer URLOrId|URL ou ID do servidor de política. Para a seleção U/I, use-PolicyServer. Para todos os servidores de política, use-PolicyServer \*|
|-Anônimo|Usar credenciais SSL anônimas|
|-Kerberos|Usar credenciais SSL Kerberos|
|-ClientCertificate ClientCertId|Use as credenciais SSL do certificado X. 509. Para a seleção U/I, use-clientCertificate.|
|-Nome de usuário UserName|Use a conta nomeada para credenciais SSL. Para a seleção U/I, use-UserName.|
|-Certid do certificado|Certificado de assinatura|
|-DC DCName|Direcionar um controlador de domínio específico|
|-restringir restrição|Lista de restrições separadas por vírgula. Cada restrição consiste em um nome de coluna, um operador relacional e um inteiro constante, uma cadeia de caracteres ou uma data. Um nome de coluna pode ser precedido por um sinal de mais ou menos para indicar a ordem de classificação. Exemplos:</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>"-RequesterName > domínio, disposição = 21"|
|-saída da coluna|Lista de colunas separadas por vírgula|
|-p senha|Senha|
|-Protectto SAMNameAndSIDList|Lista de nome/SID do SAM separados por vírgula|
|-Provedor CSP|Provedor|
|-t tempo limite|Tempo limite de busca de URL em milissegundos|
|-symkeyalg SymmetricKeyAlgorithm [, KeyLength]|Nome do algoritmo de chave simétrica com comprimento de chave opcional, exemplo: AES, 128 ou 3DES|

Retornar ao [menu](#menu)

## <a name="additional-certutil-examples"></a>Exemplos adicionais de certutil

Para obter alguns exemplos de como usar esse comando, consulte

1. [Exemplos de certutil para gerenciar Active Directory serviços de certificados (AD CS) na linha de comando](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [Tarefas de certutil para gerenciar certificados](https://technet.microsoft.com/library/cc772898.aspx)
3. [A exportação de solicitação binária usando a ferramenta de linha de comando CertUtil. exe](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [Renovação de certificado de AC raiz](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Retornar ao [menu](#menu)
