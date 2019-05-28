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
ms.openlocfilehash: d2939b693c3f9bd8d0d8c8e1fefdd5d8f892e4c9
ms.sourcegitcommit: 3582c38d87f23cc54467d63bf00c29ef07cdb7c8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517189"
---
>[!IMPORTANT]
>Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

# <a name="tpmtool"></a>tpmtool

Esse utilitário pode ser usado para obter informações sobre o [Trusted Platform Module (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

Para obter exemplos de como usar esse comando, consulte [Exemplos](#tpmtool_examples).

## <a name="syntax"></a>Sintaxe

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|getdeviceinformation|Exibe as informações básicas do TPM.|
|gatherlogs [caminho do diretório de saída]|Coleta os logs de TPM e os coloca no diretório especificado. Por padrão, eles são colocados no diretório atual.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="tpmtool_examples"></a>Exemplos

Para exibir as informações básicas do TPM, digite:
```
tpmtool getdeviceinformation
```
A coleta logs TPM e colocá-los no diretório atual, digite:
```
tpmtool gatherlogs
```
A coleta logs TPM e colocá-los em `C:\Users\Public`, tipo:
```
tpmtool gatherlogs C:\Users\Public
```
