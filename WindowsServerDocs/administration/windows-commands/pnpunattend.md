---
title: pnpunattend
description: Artigo de referência para o comando pnpunattend, que audita os drivers de dispositivo em um computador, bem como executa instalações de driver silenciosas.
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 72cb158804bcec3c57ef9bae8d21f8e15a7978d9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884936"
---
# <a name="pnpunattend"></a>pnpunattend

Audita um computador para drivers de dispositivo e executa instalações autônomas de driver ou pesquisa de drivers sem instalar e, opcionalmente, relatar os resultados para a linha de comando. Use esse comando para especificar a instalação de drivers específicos para dispositivos de hardware específicos.

## <a name="prerequisites"></a>Pré-requisitos

A preparação preliminar é necessária para versões mais antigas do sistema operacional Windows. Antes de usar esse comando, você deve concluir as seguintes tarefas:

1. Crie um diretório para os drivers que você deseja instalar. Por exemplo, crie uma pasta em **C:\Drivers\Video** para drivers de adaptador de vídeo.

2. Baixe e extraia o pacote de driver para seu dispositivo. Copie o conteúdo da subpasta que contém o arquivo INF para sua versão do sistema operacional e quaisquer subpastas na pasta de vídeo que você criou. Por exemplo, copie os arquivos de driver de vídeo para **C:\Drivers\Video**.

3. Adicione uma variável de caminho do ambiente do sistema à pasta que você criou na etapa 1. por exemplo, **C:\Drivers\Video**.

4. Crie a seguinte chave do registro e, em seguida, para a chave **DriverPaths** que você criar, defina os **dados do valor** como **1**.

5. Para o Windows® 7, navegue pelo caminho do registro: **HKEY_LOCAL_Machine \Software\microsoft\windows NT\CurrentVersion \\ **e, em seguida, crie as chaves: **UnattendSettings\PnPUnattend\DriverPaths \\ **

## <a name="syntax"></a>Sintaxe

```
PnPUnattend.exe auditsystem [/help] [/?] [/h] [/s] [/l]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| auditSystem | Especifica a instalação do driver online.<p>Obrigatório, exceto quando esse comando é executado com a **/Help** ou **/?** parâmetros. |
| /s | Opcional. Especifica a pesquisa de drivers sem instalar o. |
| /l | Opcional. Especifica para exibir as informações de log para este comando no prompt de comando. |
| `/? | /help` | Opcional. Exibe a ajuda para este comando no prompt de comando. |

### <a name="examples"></a>Exemplos

O comando a mostra como usar o **PNPUnattend.exe** para auditar um computador quanto a possíveis atualizações de driver e, em seguida, relatar as descobertas para o prompt de comando, digite:

```
pnpunattend auditsystem /s /l
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
