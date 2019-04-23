---
title: certutil
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8248f5ae540866394169229f0d7cf11497c9dcf2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834717"
---
# <a name="certutil"></a>certutil



Certutil.exe é um programa de linha de comando que é instalado como parte dos serviços de certificados. Você pode usar Certutil.exe para despejar e exibir informações de configuração de autoridade de certificação, configure os serviços de certificados, backup e restaurar os componentes de autoridade de certificação e verificar certificados, pares de chaves e cadeias de certificados.

Quando certutil é executado em uma autoridade de certificação sem parâmetros adicionais, ele exibe a configuração de autoridade de certificação atual. Quando cerutil é executado em uma autoridade de certificação não, o comando padrão é executar o certutil [-despejo](#BKMK_dump) verbo.

> [!WARNING]
> Versões anteriores do certutil podem não fornecer todas as opções descritas neste documento. Você pode ver todas as opções que uma versão específica do certutil fornece executando os comandos mostrados na [notações de sintaxe](#BKMK_notations) seção.

## <a name="BKMK_menu"></a>Menu

As seções principais neste documento são:
-   [Verbs](#BKMK_Verbs)
-   [Notações de sintaxe](#BKMK_notations)
-   [Opções](#BKMK_Options)
-   [Exemplos adicionais de certutil](#BKMK_AddedExamples)

## <a name="BKMK_Verbs"></a>Verbs

A tabela a seguir descreve os verbos que podem ser usados com o comando certutil.

|Verbos|Descrição|
|-----|-----------|
|[-dump](#BKMK_dump)|Informações de configuração ou arquivos de despejo|
|[-asn](#BKMK_asn)|Analisar o arquivo de ASN. 1|
|[-decodehex](#BKMK_decodehex)|Decodificar o arquivo codificado em hexadecimal|
|[-decode](#BKMK_decode)|Decodificar um arquivo codificado na Base64|
|[-encode](#BKMK_encode)|Codificar um arquivo em Base64|
|[-deny](#BKMK_deny)|Negar uma solicitação de certificado pendente|
|[-resubmit](#BKMK_resubmit)|Reenviar a solicitação de certificado pendente|
|[-setattributes](#BKMK_setattributes)|Definir atributos para uma solicitação de certificado pendente|
|[-setextension](#BKMK_setextension)|Definir uma extensão para uma solicitação de certificado pendente|
|[-revoke](#BKMK_revoke)|Revogar um certificado|
|[-isvalid](#BKMK_isvalid)|Exibir a disposição do certificado atual|
|[-getconfig](#BKMK_getconfig)|Obter a cadeia de caracteres de configuração padrão|
|[-ping](#BKMK_ping)|Tentativa de entrar em contato com a interface de solicitação de serviços de certificados de diretório Active Directory|
|-pingadmin|Tentativa de entrar em contato com a interface de administrador de serviços de certificados do Active Directory|
|[-CAInfo](#BKMK_CAInfo)|Exibir informações sobre a autoridade de certificação|
|[-ca.cert](#BKMK_ca.cert)|Recuperar o certificado da autoridade de certificação|
|[-ca.chain](#BKMK_ca.chain)|Recuperar a cadeia de certificados da autoridade de certificação|
|[-GetCRL](#BKMK_GetCRL)|Obter uma lista de certificados revogados (CRL)|
|[-CRL](#BKMK_CRL)|Publicar novas listas de certificados revogados (CRLs) [ou apenas as CRLs delta]|
|[-shutdown](#BKMK_shutdown)|Serviços de certificados do Active Directory de desligamento|
|[-installCert](#BKMK_installcert)|Instalar um certificado de autoridade de certificação|
|[-renewCert](#BKMK_renewcert)|Renovar um certificado de autoridade de certificação|
|[-schema](#BKMK_schema)|O esquema para o certificado de despejo|
|[-view](#BKMK_view)|O modo de exibição do certificado de despejo|
|[-db](#BKMK_db)|O banco de dados bruto de despejo|
|[-deleterow](#BKMK_deleterow)|Excluir uma linha do banco de dados do servidor|
|[-backup](#BKMK_backup)|Serviços de certificados de backup do Active Directory|
|[-backupDB](#BKMK_backupDB)|Fazer backup de banco de dados de serviços de certificados do Active Directory|
|[-backupKey](#BKMK_backupKey)|O certificado de serviços de certificados do Active Directory e a chave privada de backup|
|[-restore](#BKMK_restore)|Restaurar os serviços de certificados do Active Directory|
|[-restoreDB](#BKMK_restoreDB)|Restaurar o banco de dados de serviços de certificados do Active Directory|
|[-restoreKey](#BKMK_restorekey)|Restaurar a chave privada e certificado de serviços de certificados do Active Directory|
|[-importPFX](#BKMK_importPFX)|Importar o certificado e a chave privada|
|[-dynamicfilelist](#BKMK_dynamicfilelist)|Exibir uma lista dinâmica de arquivos|
|[-databaselocations](#BKMK_databaselocations)|Exibir os locais do banco de dados|
|[-hashfile](#BKMK_hashfile)|Gerar e exibir um hash criptográfico ao longo de um arquivo|
|[-store](#BKMK_Store)|O repositório de certificados de despejo|
|[-addstore](#BKMK_addstore)|Adicionar um certificado ao repositório|
|[-delstore](#BKMK_delstore)|Excluir um certificado do armazenamento|
|[-verifystore](#BKMK_verifystore)|Verifique se um certificado no repositório|
|[-repairstore](#BKMK_repairstore)|Reparar uma associação de chave ou atualizar propriedades do certificado ou o descritor de segurança de chave|
|[-viewstore](#BKMK_viewstore)|O repositório de certificados de despejo|
|[-viewdelstore](#BKMK_viewdelstore)|Excluir um certificado do armazenamento|
|[-dsPublish](#BKMK_dsPublish)|Publicar um certificado ou uma lista de certificados revogados (CRL) no Active Directory|
|[-ADTemplate](#BKMK_ADTemplate)|Exibir modelos do AD|
|[-Template](#BKMK_template)|Exibir modelos de certificado|
|[-TemplateCAs](#BKMK_TemplateCAs)|Exibir as autoridades de certificação (CAs) para um modelo de certificado|
|[-CATemplates](#BKMK_CATemplates)|Exibir modelos de autoridade de certificação|
|[-SetCASites](#BKMK_SetCASites)|Gerenciar nomes de Site para autoridades de certificação|
|[-enrollmentServerURL](#BKMK_enrollmentServerURL)|Exibir, adicionar ou excluir URLs de servidor de registro associadas com uma autoridade de certificação|
|[-ADCA](#BKMK_ADCA)|Exibir autoridades de certificação do AD|
|[-CA](#BKMK_CA)|Exibir CAs de política de registro|
|[-Policy](#BKMK_Policy)|Exibir política de registro|
|[-PolicyCache](#BKMK_PolicyCache)|Exibir ou excluir as entradas de Cache de política de registro|
|[-CredStore](#BKMK_Credstore)|Exibir, adicionar ou excluir as entradas de Store de credencial|
|[-InstallDefaultTemplates](#BKMK_InstallDefaultTemplates)|Instalar os modelos de certificado padrão|
|[-URLCache](#BKMK_URLCache)|Exibir ou excluir as entradas de cache de URL|
|[-pulse](#BKMK_pulse)|Eventos de inscrição automática de pulso|
|[-MachineInfo](#BKMK_MachineInfo)|Exibir informações sobre o objeto de computador do Active Directory|
|[-DCInfo](#BKMK_DCInfo)|Exibir informações sobre o controlador de domínio|
|[-EntInfo](#BKMK_EntInfo)|Exibir informações sobre uma autoridade de certificação corporativa|
|[-TCAInfo](#BKMK_TCAInfo)|Exibir informações sobre a autoridade de certificação|
|[-SCInfo](#BKMK_SCInfo)|Exibir informações sobre o cartão inteligente|
|[-SCRoots](#BKMK_SCRoots)|Gerenciar certificados de raiz do cartão inteligente|
|[-verifykeys](#BKMK_verifykeys)|Verifique se um conjunto de chaves público ou privado|
|[-verify](#BKMK_verify)|Verifique se um certificado, uma lista de certificados revogados (CRL) ou uma cadeia de certificados|
|[-verifyCTL](#BKMK_verifyCTL)|Verificar AuthRoot ou lista de certificados confiáveis de certificados não permitidos|
|[-sign](#BKMK_sign)|Assinar novamente um certificado ou uma lista de certificados revogados (CRL)|
|[-vroot](#BKMK_vroot)|Criar ou excluir compartilhamentos de arquivos e raízes virtuais web|
|[-vocsproot](#BKMK_vocsproot)|Criar ou excluir raízes virtuais web para um proxy da web OCSP|
|[-addEnrollmentServer](#BKMK_addEnrollmentServer)|Adicionar um aplicativo de servidor de registro|
|[-deleteEnrollmentServer](#BKMK_deleteEnrollmentServer)|Excluir um aplicativo de servidor de registro|
|[-addPolicyServer](#BKMK_addPolicyServer)|Adicionar um aplicativo de servidor de políticas|
|[-deletePolicyServer](#BKMK_deletePolicyServer)|Excluir um aplicativo de servidor de políticas|
|[-oid](#BKMK_oid)|Exibir o identificador de objeto ou definir um nome de exibição|
|[-error](#BKMK_error)|Exibir o texto da mensagem associado com um código de erro|
|[-getreg](#BKMK_getreg)|Exibir um valor de registro|
|[-setreg](#BKMK_setreg)|Definir um valor de registro|
|[-delreg](#BKMK_delreg)|Excluir um valor do registro|
|[-ImportKMS](#BKMK_ImportKMS)|Importar certificados e chaves de usuário para o banco de dados para arquivamento de chaves|
|[-ImportCert](#BKMK_ImportCert)|Importar um arquivo de certificado para o banco de dados|
|[-GetKey](#BKMK_GetKey)|Recuperar um blob de recuperação de chave privada arquivados|
|[-RecoverKey](#BKMK_RecoverKey)|Recuperar uma chave privada arquivada|
|[-MergePFX](#BKMK_MergePFX)|Mesclar arquivos PFX|
|[-ConvertEPF](#BKMK_ConvertEPF)|Converter um arquivo PFX em um arquivo EPF|
|-?|Exibe a lista de verbos|
|-*\<verb>* -?|Exibe a Ajuda para o verbo especificado.|
|-? -v|Exibe uma lista completa de verbos e|

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_notations"></a>Notações de sintaxe

-   Para obter a sintaxe de linha de comando básica, execute `certutil -?`
-   Para obter a sintaxe sobre como usar certutil com um verbo específico, execute **certutil**  *\<verbo >* **-?**
-   Para enviar todas as sintaxes certutil em um arquivo de texto, execute os seguintes comandos:  
    -   `certutil -v -? > certutilhelp.txt`
    -   `notepad certutilhelp.txt`

A tabela a seguir descreve a notação usada para indicar a sintaxe de linha de comando.

|Notação|Descrição|
|--------|-----------|
|Texto sem colchetes ou entre chaves|Itens que você deve digitar conforme mostrado|
|\<Texto dentro de colchetes angulares >|Espaço reservado para o qual você deve fornecer um valor|
|[Texto dentro dos colchetes]|Itens opcionais|
|{Texto entre chaves}|Conjunto de itens necessários; Escolha uma|
|Barra (vertical|) simples|Separador de itens mutuamente exclusivos; Escolha uma|
|Reticências (…)|Itens que podem ser repetidos|

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_dump"></a>-dump

CertUtil [Opções] [-despejo]

CertUtil [Opções] [-despejo] arquivo

Informações de configuração ou arquivos de despejo

[-f]. [-silenciosa] [-Dividir] [-p senha] [-t tempo limite]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_asn"></a>-asn

CertUtil [Opções] - asn arquivo [tipo]

Analisar o arquivo de ASN. 1

tipo: numérico CRYPT_STRING_ * decodificação de tipo

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_decodehex"></a>-decodehex

CertUtil [Opções] - decodehex InFile OutFile [tipo]

tipo: numérico CRYPT_STRING_ * tipo de codificação

[-f]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_decode"></a>-decodificar

CertUtil [Opções] - decodificar InFile OutFile

Decodificar o arquivo codificado na Base64

[-f]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_encode"></a>-encode

CertUtil [Opções] - codificar InFile OutFile

Codificar o arquivo em Base64

[-f]. [-UnicodeText]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_deny"></a>-deny

CertUtil [Opções] - negar RequestId

Negar a solicitação pendente

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_resubmit"></a>-resubmit

CertUtil [Opções] - reenviar RequestId

Reenviar solicitação pendente

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_setattributes"></a>-setattributes

CertUtil [Opções] - setattributes RequestId Sequência_do_atributo

Definir os atributos da solicitação pendente

RequestId-- Id numérica da solicitação pendente

AttributeString-- Pares de nome e valor do atributo de solicitação
-   Nomes e valores são separados de dois-pontos.
-   Nome de vários pares de valor são separados por novas linhas.
-   Exemplo: "CertificateTemplate:User\nEMail:User@Domain.com"
-   Cada sequência "\n" é convertida em um separador de nova linha.

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_setextension"></a>-setextension

CertUtil [Opções] - setextension RequestId Nome_da_extensão Sinalizadores {Long | Data | Cadeia de caracteres | @InFile}

Definir a extensão da solicitação pendente

RequestId-- Id numérica de uma solicitação pendente

ExtensionName-- Cadeia de caracteres de ObjectId da extensão

Sinalizadores-- 0 é recomendado.  1 torna a extensão como crítica, desabilita o 2, 3 faz isso.

Se o último parâmetro for numérico, ele será considerado um longo período.

Se ele puder ser analisado como uma data, ele será interpretado como uma data.

Se ele começa com ' @', o restante do token é o nome do arquivo que contém dados binários ou um despejo hexadecimal de texto ascii.

Qualquer outra coisa é interpretada como uma cadeia de caracteres.

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_revoke"></a>-revoke

CertUtil [Opções] - revogar SerialNumber [motivo]

Revogar Certificado

SerialNumber: Lista de números de série do certificado para revogar separada por vírgulas

Motivo: motivo da revogação numérico ou simbólico
-   0: CRL_REASON_UNSPECIFIED: Não especificado (padrão)
-   1: CRL_REASON_KEY_COMPROMISE: Comprometimento da chave
-   2: CRL_REASON_CA_COMPROMISE: Comprometimento de autoridade de certificação
-   3: CRL_REASON_AFFILIATION_CHANGED: Afiliação alterada
-   4: CRL_REASON_SUPERSEDED: Substituída
-   5: CRL_REASON_CESSATION_OF_OPERATION: Cessação da operação
-   6: CRL_REASON_CERTIFICATE_HOLD: Posse de certificado
-   8: CRL_REASON_REMOVE_FROM_CRL: Remover da lista de certificados Revogados
-   -1: Unrevoke: Unrevoke

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_isvalid"></a>-isvalid

CertUtil [Opções] - isvalid SerialNumber | CertHash

Disposição do certificado atual exibição

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_getconfig"></a>-getconfig

Getconfig - CertUtil [Opções]

Obter a cadeia de caracteres de configuração padrão

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ping"></a>-ping

CertUtil [Opções] - ping [MaxSecondsToWait | CAMachineList]

Interface ping Active Directory Services solicitação de certificado

CAMachineList – Lista de nomes de máquina separados por vírgula da autoridade de certificação
1.  Para um único computador, use uma vírgula de terminação
2.  Exibe o custo do site para cada computador da autoridade de certificação

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_CAInfo"></a>-CAInfo

-Informações_sobre_ca CertUtil [Opções] [InfoName [índice | Código de erro]]

Informações de autoridade de certificação de exibição

InfoName-- indica que a propriedade de autoridade de certificação para exibir (veja abaixo). Use "*" para todas as propriedades.

Índice – índice de propriedade opcional de base zero

ErrorCode – código de erro numérico

[-f]. [-Dividir] [-config Machine\CAName]

Sintaxe do argumento InfoName:
-   Arquivo: Versão do arquivo
-   produto: Versão do produto
-   exitcount: Contagem de módulo de saída
-   saída [Index]: Descrição do módulo de saída
-   política: Descrição do módulo de política
-   name: Nome da autoridade de certificação
-   sanitizedname: Nome da autoridade de certificação corrigido
-   dsname: Nome curto de autoridade de certificação corrigido (nome do DS)
-   sharedfolder: Pasta compartilhada
-   código de erro Error1: Texto da mensagem de erro
-   código de erro error2: Texto da mensagem de erro e o código de erro
-   Tipo: Tipo de CA
-   info: Informações de autoridade de certificação
-   Pai: Autoridade de certificação pai
-   certcount: Contagem de certificado de autoridade de certificação
-   xchgcount: Contagem de certificado do exchange de autoridade de certificação
-   kracount: Contagem de certificado KRA
-   kraused: Contagem de certificado KRA usados
-   propidmax: Máximo de autoridade de certificação PropId
-   certstate [Index]: Certificado de autoridade de certificação
-   certversion [Index]: Versão do certificado de autoridade de certificação
-   certstatuscode [Index]: Certificado de autoridade de certificação verificar status
-   crlstate [Index]: CRL
-   krastate [Index]: Certificado KRA
-   crossstate + [Index]: Encaminhar certificado cruzado
-   crossstate-[Index]: Certificado cruzado com versões anteriores
-   CERT [Index]: Certificado de autoridade de certificação
-   certchain [Index]: Cadeia de certificados de autoridade de certificação
-   certcrlchain [Index]: Cadeia de certificados de autoridade de certificação com listas de certificados revogados
-   xchg [Index]: Certificado de intercâmbio de autoridade de certificação
-   xchgchain [Index]: Cadeia de certificado de troca de autoridade de certificação
-   xchgcrlchain [Index]: Cadeia de certificado do exchange de autoridade de certificação com listas de certificados revogados
-   kra [Index]: Certificado KRA
-   cross+ [Index]: Encaminhar certificado cruzado
-   Cross-[Index]: Certificado cruzado com versões anteriores
-   CRL [Index]: CRL base
-   deltacrl [Index]: Delta CRL
-   crlstatus [Index]: Status de publicação de CRL
-   deltacrlstatus [Index]: Status de publicação de CRL delta
-   DNS: Nome DNS
-   Função: Separação de funções
-   ads: Advanced Server
-   Modelos: Modelos
-   OCSP [Index]: URLs OCSP
-   aia [Index]: URLs AIA
-   cdp [Index]: URLs CDP
-   localename: Nome da localidade da autoridade de certificação
-   subjecttemplateoids: OIDs de modelo da entidade

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ca.cert"></a>-ca.cert

CertUtil [Opções] - ca.cert OutCACertFile [Index]

Recuperar o certificado da autoridade de certificação

OutCACertFile: arquivo de saída

Índice: Índice de renovação de certificado de autoridade de certificação (o padrão é a mais recente)

[-f]. [-Dividir] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ca.chain"></a>-ca.chain

CertUtil [Opções] - ca.chain OutCACertChainFile [Index]

Recuperar a cadeia de certificados da autoridade de certificação

OutCACertChainFile: arquivo de saída

Índice: Índice de renovação de certificado de autoridade de certificação (o padrão é a mais recente)

[-f]. [-Dividir] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_GetCRL"></a>-GetCRL

CertUtil [Opções] - GetCRL OutFile [Index] [delta]

Obter lista de certificados Revogados

Índice: Lista de certificados Revogados índice ou chave de índice (o padrão é a CRL para a chave mais recente)

delta: CRL delta (o padrão é CRL base)

[-f]. [-Dividir] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_CRL"></a>-CRL

[Opções] do CertUtil - CRL [DD: hh | republicar] [delta]

Publicar CRLs novo [ou apenas delta]

dd: hh – novo período de validade da CRL em dias e horas

Republique - republicar as CRLs mais recentes

– delta CRLs delta somente (o padrão é CRLs base e delta)

[-Dividir] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_shutdown"></a>-shutdown

CertUtil [Opções] - desligamento

Serviços de certificados do Active Directory de desligamento

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_installcert"></a>-installCert

CertUtil [Opções] - installCert [CACertFile]

Instalar o certificado de autoridade de certificação

[-f]. [-silenciosa] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_renewcert"></a>-renewCert

CertUtil [Opções] - renewCert [ReuseKeys] [Machine\ParentCAName]

Renovar o certificado de autoridade de certificação

Use -f para ignorar uma solicitação de renovação pendente e gerar uma nova solicitação.

[-f]. [-silenciosa] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_schema"></a>-schema

CertUtil [Opções] - esquema [Ext | Attrib | LISTA DE CERTIFICADOS REVOGADOS]

Esquema de certificado de despejo

Padrões à tabela de solicitação e o certificado

Ext: Tabela de extensão

Attrib: Tabela de atributos

CRL: Tabela CRL

[-Dividir] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_view"></a>-modo de exibição

CertUtil [Opções] - modo de exibição [fila | Log | LogFail | Revogado | Ext | Attrib | Lista de certificados Revogados] [csv]

Exibição do certificado de despejo

Fila: Fila de solicitações

Log: Os certificados emitidos ou revogados, além de solicitações com falha

LogFail: Solicitações com falha

Revogado: Certificados revogados

Ext: Tabela de extensão

Attrib: Tabela de atributos

CRL: Tabela CRL

csv: Saída como valores separados por vírgula

Para exibir a coluna StatusCode para todas as entradas:-out StatusCode

Para exibir todas as colunas para a última entrada:-restringir "RequestId = = $"

Para exibir RequestId e o descarte do três solicitações:-restringir "RequestId > = 37, RequestId\<40"-out "RequestId, descarte"

Para exibir as Ids de linha e números de CRL para todas as CRLs Base:-restringir "CRLMinBase = 0"-out "CRLRowId CRLNumber" lista de certificados Revogados

Para exibir a base de dados de lista de certificados Revogados número 3: - v-restringir "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" lista de certificados Revogados

Para exibir a tabela inteira da lista de certificados Revogados: CRL

Use "Data [+ |-DD: hh]" para restrições de data

Use "agora + DD: hh" para uma data em relação à hora atual

[-silenciosa] [-Dividir] [-config Machine\CAName] [-restringir RestrictionList] [-out ColumnList]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_db"></a>-db

CertUtil [Opções] - db

Banco de dados brutos do despejo

[-config Machine\CAName] [-restringir RestrictionList] [-out ColumnList]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_deleterow"></a>-deleterow

CertUtil [Opções] - deleterow RowId | Data [solicitação | Cert. | Ext | Attrib | LISTA DE CERTIFICADOS REVOGADOS]

Excluir linha de banco de dados do servidor

Solicitação: Falha e (data de envio) de solicitações pendentes

Cert: Certificados revogados e expirados (data de expiração)

Ext: Tabela de extensão

Attrib: Tabela de atributos

CRL: Tabela da lista de certificados Revogados (data de expiração)

Para excluir com falha e solicitações pendentes enviados por 22 de janeiro de 2001: 22/1/2001 solicitação

Para excluir todos os certificados expiraram por 22 de janeiro de 2001: 22/1/2001 cert

Para excluir a linha de certificado, atributos e extensões de RequestId 37: 37

Para excluir as CRLs expiraram por 22 de janeiro de 2001: LISTA DE CERTIFICADOS REVOGADOS 22/1/2001

[-f]. [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_backup"></a>-backup

CertUtil [Opções] - backup BackupDirectory [Incremental] [KeepLog]

Serviços de certificados de backup do Active Directory

BackupDirectory: diretório para armazenar dados de backup

Incremental: executar somente backup incremental (o padrão é o backup completo)

KeepLog: preservar arquivos de log do banco de dados (o padrão é truncar os arquivos de log)

[-f]. [-config Machine\CAName] [-p senha]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_backupDB"></a>-backupDB

[Opções] do CertUtil – backupDB BackupDirectory [Incremental] [KeepLog]

Banco de dados de serviços de certificados do Active Directory backup

BackupDirectory: diretório para armazenar arquivos de backup banco de dados

Incremental: executar somente backup incremental (o padrão é o backup completo)

KeepLog: preservar arquivos de log do banco de dados (o padrão é truncar os arquivos de log)

[-f]. [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_backupKey"></a>-backupKey

CertUtil [Opções] – backupKey BackupDirectory

Certificado de serviços de certificados do Active Directory backup e a chave privada

BackupDirectory: diretório para armazenar o backup arquivo PFX

[-f]. [-config Machine\CAName] [-p senha] [-t tempo limite]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_restore"></a>-restaurar

CertUtil [Opções] - restaurar BackupDirectory

Restaurar os serviços de certificados do Active Directory

BackupDirectory: o diretório que contém dados a serem restaurados

[-f]. [-config Machine\CAName] [-p senha]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_restoreDB"></a>-restoreDB

CertUtil [Opções] - restoreDB BackupDirectory

Restaurar banco de dados de serviços de certificados do Active Directory

BackupDirectory: o diretório que contém os arquivos de banco de dados a serem restaurados

[-f]. [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_restorekey"></a>-restoreKey

CertUtil [Opções] - restoreKey BackupDirectory | PFXFile

Restaurar a chave particular e certificado de serviços de certificados do Active Directory

BackupDirectory: o diretório que contém o arquivo PFX a ser restaurado

PFXFile: Arquivo PFX a ser restaurado

[-f]. [-config Machine\CAName] [-p senha]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_importPFX"></a>-importPFX

CertUtil [Opções] [CertificateStoreName] - importPFX PFXFile [modificadores]

Importar o certificado e a chave privada

CertificateStoreName: Nome do repositório de certificados.  Ver [-armazenar](#BKMK_Store).

PFXFile: Arquivo PFX a ser importado

Modificadores de: Lista separada por vírgulas de uma ou mais das seguintes opções:
1.  AT_SIGNATURE: Alterar KeySpec para assinatura
2.  AT_KEYEXCHANGE: Alterar KeySpec para troca de chaves
3.  NoExport: Verifique a chave privada não exportável
4.  NoCert: Não importar o certificado
5.  NoChain: Não importar a cadeia de certificados
6.  NoRoot: Não importe o certificado raiz
7.  Protege: Proteger as chaves com senha
8.  NoProtect: Senha não proteger as chaves

Padrão do repositório de computador pessoal.

[-f]. [-usuário] [-p senha] [-csp provedor]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_dynamicfilelist"></a>-dynamicfilelist

Dynamicfilelist - CertUtil [Opções]

Exibir a lista de arquivos dinâmica

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_databaselocations"></a>-databaselocations

Databaselocations - CertUtil [Opções]

Exibir os locais do banco de dados

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_hashfile"></a>-hashfile

[Opções] do CertUtil - hashfile InFile [HashAlgorithm]

Gerar e exibir o hash criptográfico em um arquivo

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_Store"></a>-store

CertUtil [Options] -store [CertificateStoreName [CertId [OutputFile]]]

Repositório de certificados do despejo

CertificateStoreName: Nome do repositório de certificados. Exemplos:
-   "Meu", "CA" (padrão), "Root",
-   "ldap: / / / CN = autoridades de certificação, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? uma? objectClass = certificationAuthority" (Exibir certificados de raiz)
-   "ldap: / / / CN = CAName, CN = autoridades de certificação, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificar certificados de raiz)
-   "ldap: / / / CN = CAName, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (exibição de listas de certificados revogados)
-   "ldap: / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificados de AC corporativa)
-   LDAP: (Certificados de objeto de computador do AD)
-   -usuário ldap: (Certificados de objeto de usuário do AD)

CertId: O certificado ou token de correspondência CRL.  Isso pode ser um número de série, um SHA-1 certificado, lista de certificados Revogados, CTL ou hash da chave pública, um índice numérico cert (0, 1 e assim por diante), um índice numérico de CRL (. 0,.1 e assim por diante), um índice numérico de lista de certificados confiáveis (... 0,... 1 e assim por diante), uma chave pública, assinatura ou extensão ObjectId, uma entidade do certificado nome comum, um endereço de email, UPN ou nome DNS, um nome de contêiner de chave ou nome do CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum do emissor CRL. Muitos deles podem resultar em várias correspondências.

Arquivo de saída: o arquivo para salvar o certificado correspondente

Use - o usuário para acessar um repositório de usuário em vez de um armazenamento de máquina.

Use - enterprise para acessar um repositório do computador corporativo.

Use - o serviço para acessar um repositório do serviço de máquina.

Use - grouppolicy para acessar um armazenamento de diretiva de grupo do computador.

Exemplos:
-   -enterprise NTAuth
-   -enterprise Root 37
-   -user My 26e0aaaf000000000004
-   AUTORIDADE DE CERTIFICAÇÃO.11

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_addstore"></a>-addstore

CertUtil [Opções] - addstore CertificateStoreName InFile

Adicionar certificado ao armazenamento

CertificateStoreName: Nome do repositório de certificados.  Ver [-armazenar](#BKMK_Store).

InFile: Arquivo de certificado ou CRL a ser adicionado para armazenar.

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_delstore"></a>-delstore

CertUtil [Opções] - delstore CertificateStoreName CertId

Excluir certificado do armazenamento

CertificateStoreName: Nome do repositório de certificados.  Ver [-armazenar](#BKMK_Store).

CertId: O certificado ou token de correspondência CRL.  Ver [-armazenar](#BKMK_Store).

[-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_verifystore"></a>-verifystore

CertUtil [Opções] - verifystore CertificateStoreName [CertId]

Verifique se o certificado no repositório

CertificateStoreName: Nome do repositório de certificados.  Ver [-armazenar](#BKMK_Store).

CertId: O certificado ou token de correspondência CRL.  Ver [-armazenar](#BKMK_Store).

[-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName] [-t Timeout]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_repairstore"></a>-repairstore

[Opções] do CertUtil – repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

Associação de chave de reparar ou atualizar o descritor de segurança de propriedades ou a chave de certificado

CertificateStoreName: Nome do repositório de certificados.  Ver [-armazenar](#BKMK_Store).

CertIdList: lista separada por vírgulas de tokens de correspondência de certificado ou CRL. Ver [-armazenar](#BKMK_Store) CertId descrição.

PropertyInfFile – O arquivo INF que contém propriedades externos:
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
[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-csp Provider]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_viewstore"></a>-viewstore

[Opções] do CertUtil - viewstore [CertificateStoreName [CertId [OutputFile]]]

Repositório de certificados do despejo

CertificateStoreName: Nome do repositório de certificados.  Exemplos:
-   "Meu", "CA" (padrão), "Root",
-   "ldap: / / / CN = autoridades de certificação, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? uma? objectClass = certificationAuthority" (Exibir certificados de raiz)
-   "ldap: / / / CN = CAName, CN = autoridades de certificação, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificar certificados de raiz)
-   "ldap: / / / CN = CAName, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (exibição de listas de certificados revogados)
-   "ldap: / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificados de AC corporativa)
-   LDAP: (Certificados de objeto do computador do AD)
-   -usuário ldap: (Certificados de objeto de usuário do AD)

CertId: O certificado ou token de correspondência CRL. Isso pode ser um número de série, um SHA-1 certificado, lista de certificados Revogados, CTL ou hash da chave pública, um índice numérico cert (0, 1 e assim por diante), um índice numérico de CRL (. 0,.1 e assim por diante), um índice numérico de lista de certificados confiáveis (... 0,... 1 e assim por diante), uma chave pública, assinatura ou extensão ObjectId, uma entidade do certificado nome comum, um endereço de email, UPN ou nome DNS, um nome de contêiner de chave ou nome do CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum do emissor CRL. Muitos deles podem resultar em várias correspondências.

Arquivo de saída: o arquivo para salvar o certificado correspondente

Use - o usuário para acessar um repositório de usuário em vez de um armazenamento de máquina.

Use - enterprise para acessar um repositório do computador corporativo.

Use - o serviço para acessar um repositório do serviço de máquina.

Use - grouppolicy para acessar um armazenamento de diretiva de grupo do computador.

Exemplos:
1.  -enterprise NTAuth
2.  -enterprise Root 37
3.  -user My 26e0aaaf000000000004
4.  AUTORIDADE DE CERTIFICAÇÃO.11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_viewdelstore"></a>-viewdelstore

CertUtil [Options] -viewdelstore [CertificateStoreName [CertId [OutputFile]]]

Excluir certificado do armazenamento

CertificateStoreName: Nome do repositório de certificados.  Exemplos:
-   "Meu", "CA" (padrão), "Root",
-   "ldap: / / / CN = autoridades de certificação, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? uma? objectClass = certificationAuthority" (Exibir certificados de raiz)
-   "ldap: / / / CN = CAName, CN = autoridades de certificação, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (modificar certificados de raiz)
-   "ldap: / / / CN = CAName, CN = MachineName, CN = CDP, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? certificateRevocationList? base? objectClass = cRLDistributionPoint" (exibição de listas de certificados revogados)
-   "ldap: / / / CN = NTAuthCertificates, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com? cACertificate? base? objectClass = certificationAuthority" (certificados de AC corporativa)
-   LDAP: (Certificados de objeto do computador do AD)
-   -usuário ldap: (Certificados de objeto de usuário do AD)

CertId: O certificado ou token de correspondência CRL. Isso pode ser um número de série, um SHA-1 certificado, lista de certificados Revogados, CTL ou hash da chave pública, um índice numérico cert (0, 1 e assim por diante), um índice numérico de CRL (. 0,.1 e assim por diante), um índice numérico de lista de certificados confiáveis (... 0,... 1 e assim por diante), uma chave pública, assinatura ou extensão ObjectId, uma entidade do certificado nome comum, um endereço de email, UPN ou nome DNS, um nome de contêiner de chave ou nome do CSP, um nome de modelo ou ObjectId, um EKU ou ObjectId de políticas de aplicativo ou um nome comum do emissor CRL. Muitos deles podem resultar em várias correspondências.

Arquivo de saída: o arquivo para salvar o certificado correspondente

Use - o usuário para acessar um repositório de usuário em vez de um armazenamento de máquina.

Use - enterprise para acessar um repositório do computador corporativo.

Use - o serviço para acessar um repositório do serviço de máquina.

Use - grouppolicy para acessar um armazenamento de diretiva de grupo do computador.

Exemplos:
1.  -enterprise NTAuth
2.  -enterprise Root 37
3.  -user My 26e0aaaf000000000004
4.  AUTORIDADE DE CERTIFICAÇÃO.11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_dsPublish"></a>-dsPublish

[Opções] do CertUtil - dsPublish CertFile [NTAuthCA | RootCA | SubCA | CrossCA | KRA | Usuário | Máquina]

Relação a - dsPublish CRLFile CertUtil [Opções] [DSCDPContainer [DSCDPCN]]

Publicar o certificado ou CRL no Active Directory

CertFile: arquivo de certificado publicar

NTAuthCA: Publicar certificados para armazenamento de DS Enterprise

RootCA: Publicar certificado no repositório raiz confiável do DS

SubCA: Publicar o certificado de autoridade de certificação no objeto DS CA

CrossCA: Publicar cruzada cert ao objeto de autoridade de certificação do DS

KRA: Publicar o certificado ao objeto de agente de recuperação de chave do DS

Usuário: Publicar o certificado ao objeto de usuário DS

Computador: Publicar o certificado ao objeto de máquina DS

Relação a CRLFile: Arquivo da CRL para publicar

DSCDPContainer: Contêiner CDP DS CN, geralmente o nome do computador da autoridade de certificação

DSCDPCN: DS CDP normalmente objeto CN, com base no índice de chave e nome curto de autoridade de certificação corrigido

Use -f para criar o objeto DS.

[-f]. [-usuário] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ADTemplate"></a>-ADTemplate

CertUtil [Opções] - ADTemplate [modelo]

Exibir modelos do AD

[-f]. [-usuário] [-ut] [-mt] [-dc DCName]

## <a name="BKMK_template"></a>-Template

CertUtil [Opções] - modelo [modelo]

Exibir modelos de política de registro

[-f] [-user] [-silent] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_TemplateCAs"></a>-TemplateCAs

Modelo de - TemplateCAs CertUtil [Opções]

Autoridades de certificação de exibição de modelo

[-f]. [-usuário] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_CATemplates"></a>-CATemplates

CertUtil [Opções] - CATemplates [modelo]

Exibir modelos de autoridade de certificação

[-f]. [-usuário] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_SetCASites"></a>-SetCASites

CertUtil [Opções] - SetCASites [conjunto] [NomeDoSite]

Verificar a SetCASites - CertUtil [Opções] [NomeDoSite]

Excluir SetCASites - CertUtil [Opções]

Nomes de site do conjunto, verifique se ou excluir da autoridade de certificação
-   Use a opção - config para uma única CA de destino (o padrão é todas as CAs)
-   *SiteName* é permitido somente quando destinada para uma única CA
-   Use -f para substituir os erros de validação especificado *SiteName*
-   Use -f para excluir todos os nomes de site da autoridade de certificação

[-f]. [-config Machine\CAName] [-dc DCName]

> [!NOTE]
> Para obter mais informações sobre como configurar autoridades de certificação para o reconhecimento de sites de serviços de domínio Active Directory (AD DS), consulte [reconhecimento de sites do AD DS para AD CS e clientes PKI](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx).

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_enrollmentServerURL"></a>-enrollmentServerURL

CertUtil [Opções] - enrollmentServerURL [URL AuthenticationType [prioridade] [modificadores]]

Exclusão de URL - enrollmentServerURL CertUtil [Opções]

Exibir, adicionar ou excluir URLs de servidor de registro associadas com uma autoridade de certificação

AuthenticationType: Especifique um dos seguintes métodos de autenticação de cliente durante a adição de uma URL
1.  Kerberos: Usar credenciais Kerberos SSL
2.  UserName: Use a conta nomeada para credenciais SSL
3.  ClientCertificate: Usar credenciais de x. 509 certificado SSL
4.  Anonymous: Use credenciais anônimas do SSL

Excluir: exclui a URL especificada associada com a autoridade de certificação

Prioridade: o padrão é '1' se não for especificado ao adicionar uma URL

Modificadores – Lista separada por vírgulas de uma ou mais das seguintes opções:
1.  AllowRenewalsOnly: Somente as solicitações de renovação podem ser enviadas para via essa URL da autoridade de certificação
2.  AllowKeyBasedRenewal: Permite o uso de um certificado que não tenha a nenhuma conta associada no AD. Isso se aplica apenas com ClientCertificate e modo AllowRenewalsOnly

[-config Machine\CAName] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ADCA"></a>-ADCA

CertUtil [Opções] [CAName] - ADCA

Exibir autoridades de certificação do AD

[-f]. [-Dividir] [-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_CA"></a>-CA

AC - CertUtil [Opções] [CAName | TemplateName]

Exibir CAs de política de registro

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_Policy"></a>-Policy

Exibir política de registro

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_PolicyCache"></a>-PolicyCache

CertUtil [Opções] - PolicyCache [excluir]

Exibir ou excluir as entradas de Cache de política de registro

Excluir: excluir as entradas de cache do servidor de políticas

-f: use -f para excluir todas as entradas de cache

[-f] [-user] [-PolicyServer URLOrId]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_Credstore"></a>-CredStore

CertUtil [Opções] - CredStore [URL]

Adicionar de URL - CredStore CertUtil [Opções]

Excluir de URL - CredStore CertUtil [Opções]

Exibir, adicionar ou excluir as entradas de Store de credencial

URL: URL de destino.  Use * para corresponder a todas as entradas. Use https://machine* para corresponder a um prefixo de URL.

Adicionar: adicionar uma entrada de Store de credencial. As credenciais de SSL também devem ser especificadas.

Excluir: excluir as entradas de Store de credencial

-f: use -f para substituir uma entrada ou para excluir várias entradas.

[-f]. [-usuário] [-silenciosa] [-Anônimo] [-Kerberos] [-ClientCertificate ClientCertId] [-Nome de usuário de nome de usuário] [-p senha]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_InstallDefaultTemplates"></a>-InstallDefaultTemplates

[Opções] do CertUtil – InstallDefaultTemplates

Instalar os modelos de certificado padrão

[-dc DCName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_URLCache"></a>-URLCache

[Opções] do CertUtil - URLCache [URL | LISTA DE CERTIFICADOS REVOGADOS | * [excluir]]

Exibir ou excluir as entradas de cache de URL

URL: URL armazenada em cache

Lista de certificados Revogados: operam em todas as URLs armazenadas em cache da CRL somente

*: operam em URLs todos armazenados em cache

Excluir: Excluir URLs relevantes do cache local do usuário atual

Use -f para forçar uma URL específica de buscar e atualizar o cache.

[-f] [-split]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_pulse"></a>-pulse

CertUtil [Opções] - pulse

Eventos de registro automático de pulso

[-usuário]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_MachineInfo"></a>-MachineInfo

CertUtil [Opções] - MachineInfo NomeDoDomínio \ NomeDoComputador$

Exibir informações do objeto de computador do Active Directory

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_DCInfo"></a>-DCInfo

CertUtil [Opções] - DCInfo [domínio] [verificar | DeleteBad | DeleteAll]

Exibir informações de controlador de domínio

O padrão é exibir certificados de controlador de domínio sem a verificação

[-f]. [-usuário] [-urlfetch] [-dc DCName] [-t tempo limite]

> [!TIP]
> A capacidade de especificar um domínio Active Directory Domain Services (AD DS) **[domínio]** e para especificar um controlador de domínio (**-dc**) foi adicionado no Windows Server 2012. Para poder executar o comando, você deve usar uma conta que seja membro da **Admins. do domínio** ou **administradores de empresa**. As modificações de comportamento deste comando é o seguinte:</br>> 1.  Se um domínio não for especificado e um controlador de domínio específico não for especificado, essa opção retorna uma lista de controladores de domínio para processar do controlador de domínio padrão.</br>> 2.  Se um domínio não for especificado, mas um controlador de domínio for especificado, um relatório dos certificados no controlador de domínio especificado é gerado.</br>> 3.  Se um domínio for especificado, mas um controlador de domínio não for especificado, uma lista de controladores de domínio é gerada, juntamente com relatórios sobre os certificados para cada controlador de domínio na lista.</br>> 4.  Se o domínio e o controlador de domínio for especificados, uma lista de controladores de domínio é gerada do controlador de domínio de destino. Também é gerado um relatório dos certificados para cada controlador de domínio na lista.

Por exemplo, suponha que há em um domínio denominado CPANDL com um controlador de domínio denominado DC1 CPANDL. Você pode executar o comando a seguir um recuperar uma lista de controladores de domínio e seus certificados que do DC1 CPANDL: certutil -dc cpandl-dc1 - dcinfo cpandl

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_EntInfo"></a>-EntInfo

CertUtil [Opções] - EntInfo NomeDoDomínio \ NomeDoComputador$

[-f]. [-usuário]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_TCAInfo"></a>-TCAInfo

TCAInfo - CertUtil [Opções] [DomainDN |-]

Informações de autoridade de certificação de exibição

[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t Timeout]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_SCInfo"></a>-SCInfo

CertUtil [Options] -SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

Exibir informações de cartão inteligente

CRYPT_DELETEKEYSET: Excluir todas as chaves no cartão inteligente

[-silent] [-split] [-urlfetch] [-t Timeout]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_SCRoots"></a>-SCRoots

Atualizar o SCRoots - CertUtil [Opções] [+] [InputRootFile] [ReaderName]

Salvar CertUtil [Opções] - SCRoots @OutputRootFile [ReaderName]

Modo de exibição de - SCRoots CertUtil [Opções] [InputRootFile | ReaderName]

CertUtil [Opções] - SCRoots excluir [ReaderName]

Gerenciar certificados de raiz do cartão inteligente

[-f]. [-Dividir] [-p senha]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_verifykeys"></a>-verifykeys

CertUtil [Opções] - verifykeys [KeyContainerName CACertFile]

Verifique se o conjunto de chaves pública/privada

KeyContainerName: nome do contêiner de chave da chave para verificar. Padrões para as chaves do computador.  Use - o usuário para chaves do usuário.

CACertFile: arquivo de certificado de assinatura ou criptografia

Se nenhum argumento for especificado, cada certificado de autoridade de certificação de assinatura é verificado em relação a sua chave privada.

Esta operação só pode ser executada em relação a uma autoridade de certificação local ou chaves locais.

[-f]. [-usuário] [-silenciosa] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_verify"></a>-Verifique se

CertUtil [Opções] - Verificar CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [Opções] - Verificar CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Opções] – Verifique se a relação a CRLFile CACertFile [IssuedCertFile]

CertUtil [Opções] – Verifique se a relação a CRLFile CACertFile [DeltaCRLFile]

Verifique se o certificado, a lista de certificados Revogados ou cadeia

CertFile: Certificado para verificar

ApplicationPolicyList: lista de opcional separada por vírgulas de ObjectIds de política de aplicativo necessário

IssuancePolicyList: lista de opcional separada por vírgulas de ObjectIds necessária de política de emissão

CACertFile: opcional certificado da CA emissora para verificar em relação a

CrossedCACertFile: certificado opcional certificação cruzada por CertFile

Relação a CRLFile: Lista de certificados Revogados para verificar

IssuedCertFile: certificado emitido opcional coberto por relação a CRLFile

DeltaCRLFile: CRL delta opcional

Se ApplicationPolicyList for especificado, a criação da cadeia é restrita a cadeias válidas para as políticas de aplicativo especificado.

Se IssuancePolicyList for especificado, a criação da cadeia é restrita a cadeias válidas para as diretivas de emissão especificado.

Se CACertFile for especificado, os campos em CACertFile serão verificados em relação a CertFile ou CRLFile.

Se CACertFile não for especificado, CertFile será usado para criar e verificar uma cadeia completa.

Se CACertFile e CrossedCACertFile forem especificados, os campos em CACertFile e CrossedCACertFile serão verificados em relação a CertFile.

Se IssuedCertFile for especificado, os campos em IssuedCertFile serão verificados em relação a CRLFile.

Se DeltaCRLFile for especificado, os campos em DeltaCRLFile serão verificados em relação a CRLFile.

[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t Timeout]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_verifyCTL"></a>-verifyCTL

CertUtil [Opções] - verifyCTL CTLObject [CertDir] [CertFile]

Verificar AuthRoot ou lista de certificados confiáveis de certificados não permitidos

CTLObject: Identifica a lista de certificados confiáveis para verificar:
-   AuthRootWU: ler AuthRoot CAB e certificados correspondentes do cache de URL. Use -f para fazer o download do Windows Update em vez disso.
-   DisallowedWU: leitura CAB de certificados não permitidos e não permitido o arquivo de repositório de certificados do cache de URL.  Use -f para fazer o download do Windows Update em vez disso.
-   AuthRoot: leitura de registro armazenadas em cache AuthRoot CTL.  Use com -f e um CertFile que já não é confiável para forçar a atualização do registro armazenado em cache AuthRoot e CTLs de certificados não permitidos.
-   Não permitido: leitura de registro armazenadas em cache a CTL de certificados não permitidos. -f tem o mesmo comportamento assim como acontece com AuthRoot.
-   CTLFileName: arquivo ou http: caminho para a lista de certificados confiáveis ou CAB

CertDir: a pasta que contém os certificados CTL entradas correspondentes. Http: caminho de pasta deve terminar com um separador de caminho. Se uma pasta não for especificada com AuthRoot ou não permitido, vários locais serão pesquisados para correspondência de certificados: repositórios de certificados local, crypt32.dll recursos e o cache local de URL. Use -f para fazer o download do Windows Update quando necessário. Caso contrário, o padrão será a mesma pasta ou site da web como o CTLObject.

CertFile: o arquivo que contém os certificados para verificar. Certificados serão comparados com as entradas da lista de certificados confiáveis e corresponderem aos resultados exibidos. Suprime a maior parte da saída padrão.

[-f]. [-usuário] [-Dividir]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_sign"></a>-sign

CertUtil [Opções] - Assinar InFileList | SerialNumber | Lista de certificados Revogados OutFileList [StartDate + DD: hh] [+ SerialNumberList | SerialNumberList-| - ObjectIdList | @ExtensionFile]

CertUtil [Opções] - Assinar InFileList | SerialNumber | Lista de certificados Revogados OutFileList [#HashAlgorithm] [+ Alternatesignaturealgorithm=1 | - Alternatesignaturealgorithm=1]

Assine novamente o certificado ou CRL

InFileList: lista de separada por vírgulas de arquivos de certificado ou CRL para modificar e assinar novamente

SerialNumber: Número de série do certificado para criar. Período de validade e outras opções não devem estar presentes.

CRL: Crie uma lista de certificados Revogados vazia. Período de validade e outras opções não devem estar presentes.

OutFileList: lista separada por vírgulas de arquivos de saída de certificado ou CRL modificados. O número de arquivos deve corresponder ao InFileList.

StartDate + DD: hh: novo período de validade: data opcional adição; dias opcionais e o período de validade de horas. Se ambos forem especificados, use um separador de sinal de adição (+). Use "agora [+ DD: hh]" para iniciar no momento atual. Use "nunca" para não fazer com nenhuma data de expiração (CRLs).

SerialNumberList: lista de número de série para adicionar ou remover separados por vírgula

ObjectIdList: lista de ObjectId de extensão separada por vírgulas a ser removido

@ExtensionFile: Arquivo INF que contém as extensões para atualizar ou remover:
```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```
HashAlgorithm: Nome do algoritmo de hash precedido por um sinal de #

Alternatesignaturealgorithm=1: especificador de algoritmo de assinatura alternativo

Um sinal de subtração faz com que números de série e as extensões a serem removidos. Um sinal de adição faz com que números de série a ser adicionado a uma CRL. Ao remover itens de uma CRL, a lista pode conter números de série e ObjectIds. Um sinal de subtração antes Alternatesignaturealgorithm=1 faz com que o formato de assinatura herdados a ser usado. Um sinal de mais antes de Alternatesignaturealgorithm=1 faz com que o formato de assinatura alternature a ser usado. Se Alternatesignaturealgorithm=1 não for especificado, em seguida, o formato de assinatura no certificado ou CRL é usado.

[-nullsign] [-f] [-silent] [-Cert CertId]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_vroot"></a>-vroot

CertUtil [Opções] - vroot [excluir]

Criar/excluir raízes virtuais web e compartilhamentos de arquivos

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_vocsproot"></a>-vocsproot

CertUtil [Opções] - vocsproot [excluir]

Criar/excluir raízes virtuais de web de proxy da web OCSP

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_addEnrollmentServer"></a>-addEnrollmentServer

CertUtil [Opções] - addEnrollmentServer Kerberos | Nome de usuário | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

Adicionar um aplicativo de servidor de registro

Adicione um aplicativo de servidor de registro e o pool de aplicativos, se necessário, para a autoridade de certificação especificada. Esse comando não instala pacotes ou binários. Um dos seguintes métodos de autenticação com o qual o cliente se conecta a um servidor de registro de certificado.
-   Kerberos: Usar credenciais Kerberos SSL
-   UserName: Use a conta nomeada para credenciais SSL
-   ClientCertificate: Usar credenciais de x. 509 certificado SSL
-   AllowRenewalsOnly: Somente as solicitações de renovação podem ser enviadas para via essa URL da autoridade de certificação
-   AllowKeyBasedRenewal – Permite o uso de um certificado que não tenha a nenhuma conta associada no AD. Isso se aplica somente com o modo ClientCertificate e AllowRenewalsOnly.

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_deleteEnrollmentServer"></a>-deleteEnrollmentServer

CertUtil [Opções] - deleteEnrollmentServer Kerberos | Nome de usuário | ClientCertificate

Excluir um aplicativo de servidor de registro

Exclua um aplicativo de servidor de registro e o pool de aplicativos, se necessário, para a autoridade de certificação especificada. Esse comando não remove os binários ou pacotes. Um dos seguintes métodos de autenticação com o qual o cliente se conecta a um servidor de registro de certificado.
1.  Kerberos: Usar credenciais Kerberos SSL
2.  UserName: Use a conta nomeada para credenciais SSL
3.  ClientCertificate: Usar credenciais de x. 509 certificado SSL

[-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_addPolicyServer"></a>-addPolicyServer

CertUtil [Opções] - addPolicyServer Kerberos | Nome de usuário | ClientCertificate [KeyBasedRenewal]

Adicionar um aplicativo de servidor de políticas

Adicione um aplicativo de servidor de políticas e o pool de aplicativos, se necessário. Esse comando não instala pacotes ou binários. Um dos seguintes métodos de autenticação com o qual o cliente se conecta a um servidor de política de certificado:
-   Kerberos: Usar credenciais Kerberos SSL
-   UserName: Use a conta nomeada para credenciais SSL
-   ClientCertificate: Usar credenciais de x. 509 certificado SSL
-   KeyBasedRenewal: Somente as políticas que contêm modelos KeyBasedRenewal são retornadas ao cliente. Esse sinalizador aplica-se apenas para autenticação de nome de usuário e ClientCertificate.

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_deletePolicyServer"></a>-deletePolicyServer

CertUtil [Opções] - deletePolicyServer Kerberos | Nome de usuário | ClientCertificate [KeyBasedRenewal]

Excluir um aplicativo de servidor de políticas

Exclua um aplicativo de servidor de políticas e o pool de aplicativos, se necessário. Esse comando não remove os binários ou pacotes. Um dos seguintes métodos de autenticação com o qual o cliente se conecta a um servidor de política de certificado:
1.  Kerberos: Usar credenciais Kerberos SSL
2.  UserName: Use a conta nomeada para credenciais SSL
3.  ClientCertificate: Usar credenciais de x. 509 certificado SSL
4.  KeyBasedRenewal: Servidor de políticas de KeyBasedRenewal

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_oid"></a>-oid

CertUtil [Options] -oid ObjectId [DisplayName | delete [LanguageId [Type]]]

GroupId do oid - CertUtil [Opções]

CertUtil [Opções] - oid AlgId | AlgorithmName [GroupId]

Exibir ObjectId ou definir o nome de exibição
-   ObjectId – ObjectId para exibir ou adicionar o nome de exibição
-   GroupId – número decimal de GroupId para ObjectIds enumerar
-   AlgId – AlgId de hexadecimal para a ObjectId pesquisar
-   AlgorithmName – Nome do algoritmo para a ObjectId pesquisar
-   DisplayName - O nome de exibição para armazenar no DS
-   Excluir – excluir o nome de exibição
-   LanguageId – Id de idioma (segue o padrão atual: 1033)
-   Tipo – Tipo a ser criado do objeto DS: 1 para o modelo (padrão), 2 para a política de emissão, 3 para a política de aplicativo
-   Use -f para criar o objeto DS.

[-f]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_error"></a>-error

CertUtil [Opções] - Erro ErrorCode

Exibir o texto de mensagem do código de erro

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_getreg"></a>-getreg

[Opções] do CertUtil - getreg [{autoridade de certificação | restaurar | diretivas | sair | modelo | registrar | cadeia | PolicyServers}\[ProgId\]] [Nome_do_valor_do_registro]

Exibir o valor do registro

ca: Chave do registro da autoridade de certificação Use

Restaure: Chave do registro da autoridade de certificação Use restauração

política: Use a chave do registro do módulo de política

saída: Use primeiro sair da chave do registro do módulo

Modelo: Use a chave do registro de modelo (use - usuário dos modelos do usuário)

enroll: Use a chave de registro de inscrição (use - o usuário para o contexto de usuário)

cadeia: Use a chave do registro de configuração de cadeia

PolicyServers: Chave do registro de servidores de diretiva de uso

ProgId: Usar a política ou sair ProgId do módulo (nome da subchave do registro)

Nome_do_valor_do_registro: nome de valor do registro (use "Nome *" para correspondência de prefixo)

Valor: novo numérico, valor de registro de data ou de cadeia de caracteres ou nome de arquivo. Se um valor numérico começa com "+" ou "-", os bits especificados em que o novo valor são definidos ou desmarcados no valor do registro existente.

Se um valor de cadeia de caracteres começa com "+" ou "-" e o valor existente for um valor REG_MULTI_SZ, a cadeia de caracteres é adicionada ou removida do valor do registro existente. Para forçar a criação de um valor REG_MULTI_SZ, adicione "\n" para o final do valor de cadeia de caracteres.

Se o valor começa com "@", o restante do valor é o nome do arquivo que contém a representação hexadecimal de um valor binário. Se ele não faz referência a um arquivo válido, em vez disso, é analisado como [Date] [+ |-] [DD: hh] – uma data opcional de mais ou menos opcional de dias e horas. Se ambos forem especificados, use um sinal de adição (+) ou o separador de sinal de subtração (-). Use "agora + DD: hh" para uma data em relação à hora atual.

Use "chain\ChainCacheResyncFiletime @now" liberar efetivamente CRLs armazenado em cache.

[-f]. [-usuário] [-GroupPolicy] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_setreg"></a>-setreg

[Opções] do CertUtil - setreg [{autoridade de certificação | restaurar | diretivas | sair | modelo | registrar | cadeia | PolicyServers}\[ProgId\]] Nome_do_valor_do_registro valor

Definir valor do registro

ca: Chave do registro da autoridade de certificação Use

Restaure: Chave do registro da autoridade de certificação Use restauração

política: Use a chave do registro do módulo de política

saída: Use primeiro sair da chave do registro do módulo

Modelo: Use a chave do registro de modelo (use - usuário dos modelos do usuário)

enroll: Use a chave de registro de inscrição (use - o usuário para o contexto de usuário)

cadeia: Use a chave do registro de configuração de cadeia

PolicyServers: Chave do registro de servidores de diretiva de uso

ProgId: Usar a política ou sair ProgId do módulo (nome da subchave do registro)

Nome_do_valor_do_registro: nome de valor do registro (use "Nome *" para correspondência de prefixo)

Valor: novo numérico, valor de registro de data ou de cadeia de caracteres ou nome de arquivo. Se um valor numérico começa com "+" ou "-", os bits especificados em que o novo valor são definidos ou desmarcados no valor do registro existente.

Se um valor de cadeia de caracteres começa com "+" ou "-" e o valor existente for um valor REG_MULTI_SZ, a cadeia de caracteres é adicionada ou removida do valor do registro existente. Para forçar a criação de um valor REG_MULTI_SZ, adicione "\n" para o final do valor de cadeia de caracteres.

Se o valor começa com "@", o restante do valor é o nome do arquivo que contém a representação hexadecimal de um valor binário. Se ele não faz referência a um arquivo válido, em vez disso, é analisado como [Date] [+ |-] [DD: hh] – uma data opcional de mais ou menos opcional de dias e horas. Se ambos forem especificados, use um sinal de adição (+) ou o separador de sinal de subtração (-). Use "agora + DD: hh" para uma data em relação à hora atual.

Use "chain\ChainCacheResyncFiletime @now" liberar efetivamente CRLs armazenado em cache.

[-f]. [-usuário] [-GroupPolicy] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_delreg"></a>-delreg

-Delreg CertUtil [Opções] [{autoridade de certificação | restaurar | diretiva | sair | modelo | registrar | cadeia | PolicyServers}\[ProgId\]] [Nome_do_valor_do_registro]

Excluir valor do registro

ca: Chave do registro da autoridade de certificação Use

Restaure: Chave do registro da autoridade de certificação Use restauração

política: Use a chave do registro do módulo de política

saída: Use primeiro sair da chave do registro do módulo

Modelo: Use a chave do registro de modelo (use - usuário dos modelos do usuário)

enroll: Use a chave de registro de inscrição (use - o usuário para o contexto de usuário)

cadeia: Use a chave do registro de configuração de cadeia

PolicyServers: Chave do registro de servidores de diretiva de uso

ProgId: Usar a política ou sair ProgId do módulo (nome da subchave do registro)

Nome_do_valor_do_registro: nome de valor do registro (use "Nome *" para correspondência de prefixo)

Valor: novo numérico, valor de registro de data ou de cadeia de caracteres ou nome de arquivo. Se um valor numérico começa com "+" ou "-", os bits especificados em que o novo valor são definidos ou desmarcados no valor do registro existente.

Se um valor de cadeia de caracteres começa com "+" ou "-" e o valor existente for um valor REG_MULTI_SZ, a cadeia de caracteres é adicionada ou removida do valor do registro existente. Para forçar a criação de um valor REG_MULTI_SZ, adicione "\n" para o final do valor de cadeia de caracteres.

Se o valor começa com "@", o restante do valor é o nome do arquivo que contém a representação hexadecimal de um valor binário. Se ele não faz referência a um arquivo válido, em vez disso, é analisado como [Date] [+ |-] [DD: hh] – uma data opcional de mais ou menos opcional de dias e horas. Se ambos forem especificados, use um sinal de adição (+) ou o separador de sinal de subtração (-). Use "agora + DD: hh" para uma data em relação à hora atual.

Use "chain\ChainCacheResyncFiletime @now" liberar efetivamente CRLs armazenado em cache.

[-f]. [-usuário] [-GroupPolicy] [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ImportKMS"></a>-ImportKMS

CertUtil [Opções] - ImportKMS UserKeyAndCertFile [CertId]

Importar certificados e chaves de usuário para o banco de dados do servidor para o arquivamento de chave

UserKeyAndCertFile-- Arquivo de dados que contém as chaves privadas do usuário e certificados a serem arquivados.  Isso pode ser qualquer um dos seguintes:
-   Arquivo de exportação do Exchange Key Management Server (KMS)
-   Arquivo PFX

CertId: Token de correspondência de certificado de descriptografia de arquivo de exportação do KMS.  Ver [-armazenar](#BKMK_Store).

Use -f para importar certificados não emitidos pela autoridade de certificação.

[-f]. [-silenciosa] [-Dividir] [-config Machine\CAName] [-p senha] [-symkeyalg SymmetricKeyAlgorithm [, KeyLength]]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ImportCert"></a>-ImportCert

CertUtil [Opções] - ImportCert Certfile [ExistingRow]

Importar um arquivo de certificado para o banco de dados

Use ExistingRow para importar o certificado no lugar de uma solicitação pendente para a mesma chave.

Use -f para importar certificados não emitidos pela autoridade de certificação.

A autoridade de certificação pode também precisam ser configurados para dar suporte à importação de certificados externos: certutil - setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f]. [-config Machine\CAName]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_GetKey"></a>-GetKey

SearchToken - GetKey de CertUtil [Opções] [RecoveryBlobOutFile]

CertUtil [Opções] - GetKey SearchToken script OutputScriptFile

Recuperar CertUtil [Opções] - GetKey SearchToken | recuperar OutputFileBaseName

Recuperar o blob de recuperação de chave privada arquivados, gerar um script de recuperação ou recuperar chaves arquivadas

script: gerar um script para recuperar e recuperar chaves (comportamento padrão se vários candidatos de recuperação correspondente for encontrados, ou se o arquivo de saída não for especificado).

recuperar: recuperar um ou mais Blobs de recuperação de chave (comportamento padrão se exatamente um candidato de recuperação correspondente for encontrado e se o arquivo de saída for especificado)

recuperar: recuperar e recuperar as chaves privadas em uma única etapa (requer certificados de agente de recuperação de chave e chaves privadas)

SearchToken: Usado para selecionar as chaves e certificados a serem recuperados.

pode ser qualquer um dos seguintes:
1.  Nome comum do certificado
2.  Número de série do certificado
3.  Hash de SHA-1 do certificado (impressão digital)
4.  Hash de KeyId SHA-1 do certificado (identificador de chave da entidade)
5.  Nome do solicitante (domínio \ usuário)
6.  UPN (user@domain)

RecoveryBlobOutFile: arquivo de saída que contém uma cadeia de certificados e uma chave privada associada, ainda criptografada para um ou mais certificados de agente de recuperação de chave.

OutputScriptFile: arquivo de saída que contém um script em lotes para recuperar e recuperar as chaves privadas.

OutputFileBaseName: base nome do arquivo. Para recuperar, qualquer extensão será truncado e uma cadeia de caracteres do certificado específico e a extensão. rec são acrescentados para cada blob de recuperação de chave.  Cada arquivo contém uma cadeia de certificados e uma chave privada associada, ainda criptografada para um ou mais certificados de agente de recuperação de chave. Para recuperar, qualquer extensão é truncada e a extensão. p12 é acrescentada.  Contém as cadeias de certificados recuperados e chaves privadas associadas, armazenadas como um arquivo PFX.

[-f]. [-UnicodeText] [-silenciosa] [-config Machine\CAName] [-p senha] [-ProtectTo SAMNameAndSIDList] [-csp provedor]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_RecoverKey"></a>-RecoverKey

CertUtil [Opções] - RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

Recuperar a chave privada arquivado

[-f]. [-usuário] [-silenciosa] [-Dividir] [-p senha] [-ProtectTo SAMNameAndSIDList] [-csp provedor] [-t tempo limite]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_MergePFX"></a>-MergePFX

CertUtil [Opções] - MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList: Lista de entrada do arquivo PFX separada por vírgulas

PFXOutFile: Arquivo de saída PFX

Propriedades estendidas: Incluir propriedades estendidas

A senha especificada na linha de comando é uma lista de senha separados por vírgula.  Se mais de uma senha for especificada, a última senha é usada para o arquivo de saída.  Se apenas uma senha for fornecida ou se a última senha é "*", o usuário será solicitado a senha do arquivo de saída.

[-f]. [-usuário] [-Dividir] [-p senha] [-ProtectTo SAMNameAndSIDList] [-csp provedor]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_ConvertEPF"></a>-ConvertEPF

ConvertEPF EPFOutFile PFXInFileList CertUtil [Opções]-[cast | cast-] [V3CACertId] [, Salt]

Converter arquivos PFX para arquivo EPF

PFXInFileList: Lista de entrada do arquivo PFX separada por vírgulas

EPF: Arquivo de saída EPF

conversão: Usar criptografia CAST 64

cast-: Usar criptografia CAST 64 (exportação)

V3CACertId: Token de correspondência de certificado de autoridade de certificação V3.  Ver [-armazenar](#BKMK_Store) CertId descrição.

Salt: EPF sequência de salt de arquivo de saída

A senha especificada na linha de comando é uma lista de senha separados por vírgula. Se mais de uma senha for especificada, a última senha é usada para o arquivo de saída.  Se apenas uma senha for fornecida ou se a última senha é "*", o usuário será solicitado a senha do arquivo de saída.

[-f]. [-silenciosa] [-Dividir] [-dc DCName] [-p senha] [-csp provedor]

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_Options"></a>Opções

Esta seção define as opções que você pode especificar com o comando.

|Opções|Descrição|
|-------|-----------|
|-nullsign|Usar o hash dos dados como assinatura|
|-f|Forçar substituição|
|-enterprise|Usar o armazenamento de certificado de registro de empresa do computador local|
|-usuário|Use chaves HKEY_CURRENT_USER ou repositório de certificados|
|-GroupPolicy|Repositório de certificados de diretiva de grupo de uso|
|-ut|Modelos de exibição do usuário|
|-mt|Exibir modelos de máquina|
|-Unicode|Gravar a saída redirecionada em Unicode|
|-UnicodeText|Gravar arquivo de saída em Unicode|
|-gmt|Exibir horas como GMT|
|-segundos|Tempos de exibição com segundos e milissegundos|
|-silent|Use o sinalizador silencioso para adquirir contexto cript|
|-split|Dividir os elementos de ASN. 1 incorporados e salvar arquivos|
|-v|Operação detalhada|
|-privatekey|Exibir dados de chave privados e a senha|
|-Fixar o PIN|PIN do cartão inteligente|
|-urlfetch|Recuperar e verificar certificados AIA e CDP CRLs|
|-config Machine\CAName|Cadeia de caracteres de nome de autoridade de certificação e computador|
|-PolicyServer URLOrId|URL do servidor de política ou ID. Para a seleção de U /, usei - PolicyServer. Para todos os servidores de política, use - PolicyServer *|
|-Anônima|Use credenciais anônimas do SSL|
|-Kerberos|Usar credenciais Kerberos SSL|
|– ClientCertificate ClientCertId|Use as credenciais de SSL de certificado x. 509. Para a seleção de U / I, use - clientCertificate.|
|Nome de usuário - UserName|Use conta nomeada credenciais SSL. Para a seleção de U /, use - o nome de usuário.|
|-CertId cert|Certificado de autenticação|
|-dc DCName|Destino de um controlador de domínio específico|
|-restringir RestrictionList|Lista de restrições separada por vírgulas. Cada restrição consiste em um nome de coluna, um operador relacional e um inteiro constante, cadeia de caracteres ou data. Um nome de coluna pode ser precedido por um sinal de adição ou subtração para indicar a ordem de classificação. Exemplos:</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>"-RequesterName > DOMAIN, Disposition = 21"|
|-out ColumnList|Lista de colunas separada por vírgulas|
|-p senha|Senha|
|-ProtectTo SAMNameAndSIDList|SAM nome/SID lista separada por vírgulas|
|-Provedor de csp|Provedor|
|-t tempo limite|Tempo limite de busca de URL em milissegundos|
|-symkeyalg SymmetricKeyAlgorithm[,KeyLength]|Nome do algoritmo de chave simétrica com comprimento de chave opcional, exemplo: AES 128 ou 3DES|

Retorne ao [Menu](#BKMK_menu)

## <a name="BKMK_AddedExamples"></a>Exemplos adicionais de certutil

Para obter alguns exemplos de como usar esse comando, consulte
1.  [Exemplos de Certutil para gerenciar os serviços de certificados do Active Directory (AD CS) da linha de comando](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2.  [Tarefas do Certutil para gerenciar certificados](https://technet.microsoft.com/library/cc772898.aspx)
3.  [Exportação de binários de solicitação usando a ferramenta de linha de comando de CertUtil.exe passo a passo](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4.  [Renovação de certificado de autoridade de certificação raiz](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5.  [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

Retorne ao [Menu](#BKMK_menu)
