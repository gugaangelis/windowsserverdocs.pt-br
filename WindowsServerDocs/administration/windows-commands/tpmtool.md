---
title: tpmtool
description: O tópico de comandos do Windows para tpmtool-Obtém informações sobre o Trusted Platform Module.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: 3967136bc64d1e06425a019466dea15ddce3a563
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385721"
---
# <a name="tpmtool"></a>tpmtool

Esse utilitário pode ser usado para obter informações sobre o [Trusted Platform Module (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#tpmtool_examples).

## <a name="syntax"></a>Sintaxe

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|getdeviceinformation|Exibe as informações básicas do TPM. O significado dos valores do sinalizador de informações pode ser encontrado [aqui](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [caminho do diretório de saída]|Coleta logs do TPM e os coloca no diretório especificado. Se esse diretório não existir, ele será criado. Por padrão, eles são colocados no diretório atual. Os arquivos possíveis gerados são: </br>-TpmEvents. evtx</br>-TpmInformation. txt</br>-SRTMBoot. dat</br>-SRTMResume. dat</br>-DRTMBoot. dat</br>-DRTMResume. dat</br>|
|drivertracing [iniciar/parar]|Iniciar/parar a coleta de rastreamentos de driver do TPM. O log de rastreamento, TPMTRACE. ETL, será gerado e colocado no diretório atual.|
|parsetcglogs [-Validate (-v)]|Exibe o log de TCG analisado, também conhecido como o log de configuração de inicialização do Windows (WBCL). As descrições de eventos mais atualizadas podem ser encontradas no [site do TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), em descrições de **eventos**. Se o sinalizador `-validate` definido, o validará que os valores de PCR (registro de configuração de plataforma) no TPM correspondem aos valores no log.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="tpmtool_examples"></a>Disso

Para exibir as informações básicas do TPM, digite:
```
tpmtool getdeviceinformation
```
Para coletar logs do TPM e colocá-los no diretório atual, digite:
```
tpmtool gatherlogs
```
Para coletar logs do TPM e colocá-los em `C:\Users\Public`, digite:
```
tpmtool gatherlogs C:\Users\Public
```
Para coletar rastreamentos de driver do TPM, digite:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Para analisar o log de TCG:
```
tpmtool parsetcglogs
```
Para analisar o log do TCG e validar o PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Decodificando códigos de erro

Os códigos de erro específicos do TPM estão documentados [aqui](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
