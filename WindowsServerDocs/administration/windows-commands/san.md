---
title: san
description: Artigo de referência para o comando San, que exibe ou define a política de San (rede de área de armazenamento) para o sistema operacional.
ms.topic: reference
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8881855e77f8df579cb054954d26296edf4c971f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388365"
---
# <a name="san"></a>san

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe ou define a política de San (rede de área de armazenamento) para o sistema operacional. Se usado sem parâmetros, a política San atual será exibida.

## <a name="syntax"></a>Sintaxe

```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| política = {onlineAll|offlineAll|offlineShared}] | Define a política San para o sistema operacional inicializado no momento. A política San determina se um disco descoberto recentemente é colocado online ou permanece offline e se ele se torna leitura/gravação ou permanece somente leitura. Quando um disco está offline, o layout do disco pode ser lido, mas nenhum dispositivo de volume é exibido por meio de Plug and Play. Isso significa que nenhum sistema de arquivos pode ser montado no disco. Quando um disco está online, um ou mais dispositivos de volume são instalados para o disco. Veja a seguir uma explicação de cada parâmetro:<ul><li>**onlineAll**. Especifica que todos os discos recém-descobertos serão colocados online e criados para leitura/gravação. **Importante:** Especificar **onlineAll** em um servidor que compartilha discos pode causar corrupção de dados. Portanto, você não deve definir essa política se os discos forem compartilhados entre servidores, a menos que o servidor faça parte de um cluster.</li><li>**offlineAll**. Especifica que todos os discos recentemente descobertos, exceto o disco de inicialização, estarão offline e somente leitura por padrão.</li><li>**offlineShared**. Especifica que todos os discos recentemente descobertos que não residem em um barramento compartilhado (como SCSI e iSCSI) sejam colocados online e tenham sido gravados na leitura. Os discos que são deixados offline serão somente leitura por padrão.</li></ul>Para obter mais informações, consulte [VDS_san_POLICY Enumeration](https://docs.microsoft.com/windows/win32/api/vds/ne-vds-vds_san_policy?redirectedfrom=MSDN). |
| NOERR | Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

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

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
