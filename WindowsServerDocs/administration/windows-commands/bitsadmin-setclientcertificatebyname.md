---
title: bitsadmin setclientcertificatebyname
description: Tópico de comandos do Windows para **Bitsadmin setclientcertificatebyname** -especifica o nome da entidade do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL).
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: de2e84401673848ecc8823bb6dd3f91224d9a87e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380673"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname



Especifica o nome do assunto do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Store_location|Identifica o local de um armazenamento do sistema a ser usado para pesquisar o certificado. Os valores possíveis incluem:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIÇOS)</br>5 (USUÁRIOS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|O nome do repositório de certificados. Os valores possíveis incluem:</br>AC (certificados de autoridade de certificação)</br>MEU (certificados pessoais)</br>RAIZ (certificados raiz)</br>SPC (certificado do fornecedor de software)|
|Subject_name|Nome do certificado|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir especifica o nome do certificado de cliente *myCertificate* a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL) para o trabalho chamado *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)