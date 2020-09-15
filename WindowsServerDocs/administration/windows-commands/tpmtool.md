---
title: tpmtool
description: Artigo de referência para tpmtool, que obtém informações sobre o Trusted Platform Module.
ms.topic: reference
author: ashleytqy
ms.author: asteoh
manager: raigner
ms.date: 05/07/2019
ms.openlocfilehash: f9d1a7c0bc1516feea3afaf00d750e54871ad818
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077583"
---
# <a name="tpmtool"></a>tpmtool

Esse utilitário pode ser usado para obter informações sobre o [Trusted Platform Module (TPM)](/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece garantias, expressas ou implícitas, das informações aqui fornecidas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#tpmtool_examples).

## <a name="syntax"></a>Sintaxe

```
tpmtool /parameter [<arguments>]
```
### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|getdeviceinformation|Exibe as informações básicas do TPM. O significado dos valores do sinalizador de informações pode ser encontrado [aqui](/windows/desktop/secprov/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [caminho do diretório de saída]|Coleta logs do TPM e os coloca no diretório especificado. Se esse diretório não existir, ele será criado. Por padrão, eles são colocados no diretório atual. Os arquivos possíveis gerados são: </br>-TpmEvents. evtx</br>-TpmInformation.txt</br>-SRTMBoot. dat</br>-SRTMResume. dat</br>-DRTMBoot. dat</br>-DRTMResume. dat</br>|
|drivertracing [iniciar/parar]|Iniciar/parar a coleta de rastreamentos de driver do TPM. O log de rastreamento, TPMTRACE. ETL, será gerado e colocado no diretório atual.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a><a name=tpmtool_examples></a>Exemplos

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

## <a name="decoding-error-codes"></a>Decodificando códigos de erro

Os códigos de erro específicos do TPM estão documentados [aqui](/windows/desktop/com/com-error-codes-6).
