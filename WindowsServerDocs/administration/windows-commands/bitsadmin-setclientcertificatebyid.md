---
title: bitsadmin setclientcertificatebyid
description: Tópico de comandos do Windows para Bitsadmin setclientcertificatebyid, que especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80c97b21194c773d1b21aab2ee31794624da671c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849659"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

Especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Store_location|Identifica o local de um armazenamento do sistema a ser usado para pesquisar o certificado. Os valores possíveis incluem:</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVIÇOS)</br>5 (USUÁRIOS)</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|O nome do repositório de certificados. Os valores possíveis incluem:</br>AC (certificados de autoridade de certificação)</br>MEU (certificados pessoais)</br>RAIZ (certificados raiz)</br>SPC (certificado do fornecedor de software)|
|Hexadecimal_cert_id|Um número hexadecimal que representa o hash do certificado|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL) para o trabalho chamado *myJob*.
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)