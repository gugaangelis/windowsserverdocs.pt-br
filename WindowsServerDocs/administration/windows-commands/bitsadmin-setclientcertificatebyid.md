---
title: Bitsadmin setclientcertificatebyid
description: Tópico de comandos do Windows para **setclientcertificatebyid bitsadmin** Especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação de HTTPS (SSL)
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2424de18ee8aaec73b086207e8ef56d85df862fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863927"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>Bitsadmin setclientcertificatebyid



Especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação de HTTPS (SSL).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Store_location|Identifica o local de um repositório do sistema a ser usado para pesquisar o certificado. Os valores possíveis incluem:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIÇOS)</br>5 (USUÁRIOS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|O nome do repositório de certificados. Os valores possíveis incluem:</br>Autoridade de certificação (certificados de autoridade de certificação)</br>Meus (certificados pessoais)</br>RAIZ (certificados de raiz)</br>SPC (certificado do fornecedor de Software)|
|Hexadecimal_cert_id|Um número hexadecimal que representa o hash do certificado|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação de HTTPS (SSL) do trabalho nomeado *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)