---
title: freedisk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 138d637ba3da983ccb931d489f7c22b651e07a0c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817217"
---
# <a name="freedisk"></a>freedisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica se a quantidade especificada de espaço em disco está disponível antes de continuar com um processo de instalação.

## <a name="syntax"></a>Sintaxe
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/s <computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando.|
|/u [<Domain>\\]<User>|Executa o script com as permissões da conta de usuário especificada. O padrão é permissões do sistema.|
|/p [<Password>]|Especifica a senha da conta de usuário que é especificada na **/u**.|
|/d <Drive>|Especifica a unidade para o qual você deseja descobrir a disponibilidade de espaço livre. Você deve especificar <Drive>para um computador remoto.|
|<Value>|Verifica se há uma quantidade específica de espaço livre em disco. Você pode especificar <Value>em bytes, KB, MB, GB, TB, PB, EB, ZB ou Anuário.|
## <a name="remarks"></a>Comentários
-   Usando o **/s**, **/u**, e **p** opções de linha de comando estão disponíveis somente quando você usa **/s**. Você deve usar **/p** com **/u**para fornecer a senha do usuário s.
-   para instalações autônomas, você pode usar **freedisk** em arquivos em lotes de instalação para verificar se há o pré-requisito de quantidade de espaço livre antes de continuar com a instalação.
-   Quando você usa **freedisk** em um arquivo em lotes, ele retorna um **0** se há espaço suficiente e um **1** se não houver espaço suficiente.
## <a name="BKMK_examples"></a>Exemplos
Para determinar se há pelo menos 50 MB de espaço livre na unidade c:, digite:
```
freedisk 50mb 
```
Saída semelhante à seguinte será exibida na tela:
```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
