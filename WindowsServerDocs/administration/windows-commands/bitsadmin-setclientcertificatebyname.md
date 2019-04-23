---
title: Bitsadmin setclientcertificatebyname
description: Tópico de comandos do Windows para **setclientcertificatebyname bitsadmin** -Especifica o nome da entidade do certificado do cliente a ser usado para autenticação de cliente em uma solicitação de HTTPS (SSL).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76150ccb34693eb692d27efbd6538f5363ba1c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871307"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>Bitsadmin setclientcertificatebyname



Especifica o nome da entidade do certificado do cliente a ser usado para autenticação de cliente em uma solicitação de HTTPS (SSL).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Store_location|Identifica o local de um repositório do sistema a ser usado para pesquisar o certificado. Os valores possíveis incluem:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIÇOS)</br>5 (USUÁRIOS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|O nome do repositório de certificados. Os valores possíveis incluem:</br>Autoridade de certificação (certificados de autoridade de certificação)</br>Meus (certificados pessoais)</br>RAIZ (certificados de raiz)</br>SPC (certificado do fornecedor de Software)|
|Subject_name|Nome do certificado|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir especifica o nome do certificado do cliente *myCertificate* a ser usado para autenticação de cliente em uma solicitação de HTTPS (SSL) do trabalho nomeado *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)