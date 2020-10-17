---
title: tpmtool
description: Artigo de referência para o comando tpmtool, que obtém informações sobre o Trusted Platform Module.
ms.topic: reference
author: ashleytqy
ms.author: asteoh
manager: raigner
ms.date: 05/07/2019
ms.openlocfilehash: bc0aaa4bc4bd9c23e0cf22b0315335287867d8cd
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156436"
---
# <a name="tpmtool"></a>tpmtool

Esse utilitário pode ser usado para obter informações sobre o [Trusted Platform Module (TPM)](/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Algumas informações podem estar relacionadas ao produto de pré-lançamento, que pode ser substancialmente modificado antes de ser lançado comercialmente. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

## <a name="syntax"></a>Sintaxe

```
tpmtool /parameter [<arguments>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| getdeviceinformation | Exibe as informações básicas do TPM. Consulte o artigo [parâmetros do método Win32_Tpm:: IsReadyInformation](/windows/win32/secprov/win32-tpm-isreadyinformation#parameters) para obter detalhes sobre os valores do sinalizador de informações. |
| GatherLogs [caminho do diretório de saída] | Coleta logs do TPM e os coloca no diretório especificado. Se esse diretório não existir, ele será criado. Por padrão, os arquivos de log são colocados no diretório atual. Os arquivos possíveis gerados são:<ul><li>TpmEvents. evtx</li><li>TpmInformation.txt</li><li>SRTMBoot. dat</li><li>SRTMResume. dat</li><li>DRTMBoot. dat</li><li>DRTMResume. dat</li></ul> |
| drivertracing `[start | stop]` | Inicia ou para a coleta de rastreamentos de driver do TPM. O log de rastreamento, *TPMTRACE. etl*, é criado e colocado no diretório atual. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para exibir as informações básicas do TPM, digite:

```
tpmtool getdeviceinformation
```

Para coletar logs do TPM e colocá-los no diretório atual, digite:

```
tpmtool gatherlogs
```

Para coletar logs do TPM e colocá-los no `C:\Users\Public` , digite:

```
tpmtool gatherlogs C:\Users\Public
```

Para coletar rastreamentos de driver do TPM, digite:

```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Códigos de erro COM (TPM, PLA, FVE)](/windows/win32/com/com-error-codes-6)
