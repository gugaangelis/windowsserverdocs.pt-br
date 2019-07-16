---
title: tpmtool
description: Tópico de comandos do Windows para tpmtool - obtém informações sobre o Trusted Platform Module.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: e125dbf6127b92c91e041c431f1e462e1f884168
ms.sourcegitcommit: 0ff812a80f654fa2c35b1632524e27841eca75c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68230861"
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
|getdeviceinformation|Exibe as informações básicas do TPM. O significado dos valores de sinalizador de informações pode ser encontrado [aqui](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|gatherlogs [caminho do diretório de saída]|Coleta os logs de TPM e os coloca no diretório especificado. Se esse diretório não existir, ele é criado. Por padrão, eles são colocados no diretório atual. Os possíveis arquivos gerados são: </br>-TpmEvents.evtx</br>-TpmInformation.txt</br>-SRTMBoot.dat</br>-SRTMResume.dat</br>-DRTMBoot.dat</br>-DRTMResume.dat</br>|
|drivertracing [iniciar / parar]|Iniciar / interromper a coleta de rastreamentos de driver TPM. O log de rastreamento, TPMTRACE.etl, será gerado e colocado no diretório atual.|
|parsetcglogs [-validar (-v)]|Exibe o log TCG analisado, também conhecido como o Windows inicialização configuração Log (WBCL). As descrições de eventos mais recentes podem ser encontradas na [site do TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), em **descrições de eventos**. Se o `-validate` sinalizador definido, valida que os valores de plataforma configuração PCR (registro) do TPM correspondem aos valores no log.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="tpmtool_examples"></a>Exemplos

Para exibir as informações básicas do TPM, digite:
```
tpmtool getdeviceinformation
```
Para coletar logs TPM e colocá-los no diretório atual, digite:
```
tpmtool gatherlogs
```
Para coletar logs TPM e colocá-los em `C:\Users\Public`, tipo:
```
tpmtool gatherlogs C:\Users\Public
```
Para coletar rastreamentos de driver TPM, digite:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Para analisar o log TCG:
```
tpmtool parsetcglogs
```
Para analisar o log TCG e validar o PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Decodificando códigos de erro

Códigos de erro específicos do TPM estão documentados [aqui](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
