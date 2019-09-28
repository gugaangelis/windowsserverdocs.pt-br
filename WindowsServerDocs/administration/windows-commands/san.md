---
title: solução
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 970a5d6403804db7585bf3895dbb61eead286c37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384376"
---
# <a name="san"></a>solução

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe ou define a política de San (rede de área de armazenamento) para o sistema operacional.
> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Parâmetros

|                          Parâmetro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| política = {onlineAll &#124; offlineAll &#124; offlineShared}] | Define a política San para o sistema operacional inicializado no momento. A política San determina se um disco descoberto recentemente é colocado online ou permanece offline e se ele se torna leitura/gravação ou permanece somente leitura. Quando um disco está offline, o layout do disco pode ser lido, mas nenhum dispositivo de volume é exibido por meio de Plug and Play. Isso significa que nenhum sistema de arquivos pode ser montado no disco. Quando um disco está online, um ou mais dispositivos de volume são instalados para o disco. Veja a seguir uma explicação de cada parâmetro:<br /><br />-   **onlineAll**. Especifica que todos os discos recém-descobertos serão colocados online e criados para leitura/gravação. **FUNDAMENTAL**     Especificar **onlineAll** em um servidor que compartilha discos pode causar corrupção de dados. Portanto, você não deve definir essa política se os discos forem compartilhados entre servidores, a menos que o servidor faça parte de um cluster.<br />-   **offlineAll**. Especifica que todos os discos recentemente descobertos, exceto o disco de inicialização, estarão offline somente Andread por padrão.<br />-   **offlineShared**. Especifica que todos os discos recentemente descobertos que não residem em um barramento compartilhado (como SCSI e iSCSI) sejam colocados online e tenham sido gravados na leitura. Os discos que são deixados offline serão somente leitura por padrão.<br /><br />para obter mais informações, consulte [VDS_san_POLICY Enumeration](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            NOERR                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentários
- Se o comando for fornecido sem parâmetros, a política San atual será exibida.
  ## <a name="BKMK_Examples"></a>Disso
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
