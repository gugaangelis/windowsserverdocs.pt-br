---
title: SAN
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90b6cec9e44ae91b21932c1e4c46a33e0b5da5e4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441670"
---
# <a name="san"></a>SAN

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe ou define a política de san (rede) de área de armazenamento para o sistema operacional.
> [!NOTE]
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Parâmetros

|                          Parâmetro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| policy={ onlineAll &#124; offlineAll &#124; offlineShared }] | Define a política de san para o sistema operacional inicializado no momento. A política de san determina se um disco recém-descobertos é colocado online ou permanece offline, e se ele se tornará de leitura/gravação ou permanece somente leitura. Quando um disco estiver offline, o layout de disco pode ser lido, mas nenhum dispositivo de volume é exibido por meio de Plug and Play. Isso significa que nenhum sistema de arquivos pode ser montado no disco. Quando um disco estiver online, um ou mais dispositivos de volume são instalados para o disco. Veja a seguir uma explicação de cada parâmetro:<br /><br />-   **onlineAll**. Especifica que todas as descobertas recentemente discos serão colocados online e é feita de leitura/gravação. **IMPORTANTE:**     Especificando **onlineAll** em um servidor que compartilha discos pode levar à corrupção de dados. Portanto, você deve definir essa política não se os discos são compartilhados entre servidores, a menos que o servidor for parte de um cluster.<br />-   **offlineAll**. Especifica que todas as descobertas recentemente discos, exceto o disco de inicialização será offline somente andread por padrão.<br />-   **offlineShared**. Especifica que todos os recentemente descobertos discos que não residem em um barramento compartilhado (por exemplo, SCSI e iSCSI) sejam colocados online e feita a leitura e gravação. Os discos são deixados offline será somente leitura por padrão.<br /><br />Para obter mais informações, consulte [enumeração VDS_san_POLICY](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            noerr                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Usado somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentários
- Se o comando é fornecido sem parâmetros, a política de san atual é exibida.
  ## <a name="BKMK_Examples"></a>Exemplos
  Para exibir a política atual, digite:
  ```
  san
  ```
  Para tornar todos os discos recém-descobertos, exceto o disco de inicialização, offline e somente leitura por padrão, digite:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
