---
title: san
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c047dc99ac8c1584ce92ea5b8ab1367c298b0900
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036194"
---
# <a name="san"></a>san

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe ou define a política de San (rede de área de armazenamento) para o sistema operacional.
> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
#### <a name="parameters"></a>Parâmetros

|                          Parâmetro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| política = {onlineAll &#124; offlineAll &#124; offlineShared}] | Define a política San para o sistema operacional inicializado no momento. A política San determina se um disco descoberto recentemente é colocado online ou permanece offline e se ele se torna leitura/gravação ou permanece somente leitura. Quando um disco está offline, o layout do disco pode ser lido, mas nenhum dispositivo de volume é exibido por meio de Plug and Play. Isso significa que nenhum sistema de arquivos pode ser montado no disco. Quando um disco está online, um ou mais dispositivos de volume são instalados para o disco. Veja a seguir uma explicação de cada parâmetro:<p>-   **onlineAll**. Especifica que todos os discos recém-descobertos serão colocados online e criados para leitura/gravação. **Importante:**     Especificar **onlineAll** em um servidor que compartilha discos pode causar corrupção de dados. Portanto, você não deve definir essa política se os discos forem compartilhados entre servidores, a menos que o servidor faça parte de um cluster.<br />-   **offlineAll**. Especifica que todos os discos recentemente descobertos, exceto o disco de inicialização, estarão offline somente Andread por padrão.<br />-   **offlineShared**. Especifica que todos os discos recentemente descobertos que não residem em um barramento compartilhado (como SCSI e iSCSI) sejam colocados online e tenham sido gravados na leitura. Os discos que são deixados offline serão somente leitura por padrão.<p>para obter mais informações, consulte [VDS_san_POLICY Enumeration](https://go.microsoft.com/fwlink/?LinkId=203815) ( <https://go.microsoft.com/fwlink/?LinkId=203815> ). |
|                            NOERR                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentários
- Se o comando for fornecido sem parâmetros, a política San atual será exibida.
  ## <a name="examples"></a>Exemplos
  Para exibir a política atual, digite:
  ```
  san
  ```
  Para tornar todos os discos recém-descobertos, exceto o disco de inicialização, offline e somente leitura por padrão, digite:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Referências adicionais
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
