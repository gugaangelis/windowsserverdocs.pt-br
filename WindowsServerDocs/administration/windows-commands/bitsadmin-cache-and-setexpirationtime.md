---
título: descrição de cache e setexpirationtime bitsadmin: "Tópico de comandos do Windows para **bitsadmin cache e setexpirationtime** -define o tempo de expiração de cache."
ms.custom: na ms.prod: windows-server-threshold ms.reviewer: na ms.suite: na ms.technology: manage-windows-commands ms.tgt_pltfrm: na ms.topic: article ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d author: coreyp-at-msft ms.author: coreyp manager: dongill ms.date: 10/16/2017 --

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>setexpirationtime e cache bitsadmin
Define o tempo de expiração do cache.
## <a name="syntax"></a>Sintaxe
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|segundos|O número de segundos até que o cache expire.|
## <a name="BKMK_examples"></a>Exemplos
O exemplo a seguir expira o cache em 60 segundos.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
