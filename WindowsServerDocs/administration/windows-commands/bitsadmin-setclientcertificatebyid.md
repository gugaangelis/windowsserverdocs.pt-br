---
title: bitsadmin setclientcertificatebyid
description: Artigo de referência para o comando Bitsadmin setclientcertificatebyid, que especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL)
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfa3a65051cad9ef21f146888fc9d0e3e80f4023
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927888"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

Especifica o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL).

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| store_location | Identifica o local de um armazenamento do sistema a ser usado para pesquisar o certificado, incluindo:<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>SERVIÇOS</li><li>USUÁRIOS</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE.</li></ul> |
| store_name | O nome do repositório de certificados, incluindo:<ul><li>AC (certificados de autoridade de certificação)</li><li>MEU (certificados pessoais)</li><li>RAIZ (certificados raiz)</li><li>SPC (certificado do fornecedor de software).</li></ul> |
| hexadecimal_cert_id | Um número hexadecimal que representa o hash do certificado. |

## <a name="examples"></a>Exemplos

Para especificar o identificador do certificado do cliente a ser usado para autenticação de cliente em uma solicitação HTTPS (SSL) para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
