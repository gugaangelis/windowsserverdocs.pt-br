---
title: prnport
description: Saiba como criar, excluir e listar as portas de impressora.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb6ef7632c10217c4fdf114d4e67c8bc52be07e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856887"
---
# <a name="prnport"></a>prnport

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, exclui e lista de portas de impressora TCP/IP padrão, além de exibir e alterar a configuração de porta.

## <a name="syntax"></a>Sintaxe
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|-a|cria uma porta de impressora TCP/IP padrão.|
|-d|Exclui uma porta de impressora TCP/IP padrão.|
|-l|lista todas as portas de impressora TCP/IP padrão no computador especificado com o **-s** parâmetro.|
|-g|Exibe a configuração de uma porta de impressora TCP/IP padrão.|
|-t|Define as configurações de porta para uma porta de impressora TCP/IP padrão.|
|-r \<PortName>|Especifica a porta à qual a impressora está conectada.|
|-s \<ServerName>|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|
|-u \<nome de usuário > -w <Password>|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores local do computador de destino têm essas permissões, mas também podem ser concedidas as permissões a outros usuários. Se você não especificar uma conta, você deve fazer logon em uma conta com essas permissões para o comando funcione.|
|-o {brutos &#124; lpr}|Especifica que usa a porta de protocolo: TCP bruto ou TCP lpr. Se você usar o TCP não processado, você pode, opcionalmente, especificar o número da porta usando o **- n** parâmetro. O número da porta padrão é 9100.|
|-h \<Hostaddress>|Especifica a impressora para a qual você deseja configurar a porta (por endereço IP).|
|-q \<QueueName>|Especifica o nome da fila para uma porta TCP não processado.|
|-n \<PortNumber >|Especifica o número da porta para uma porta TCP não processado. O número da porta padrão é 9100.|
|-m{e &#124; d}|Especifica se o SNMP está habilitado. O parâmetro **eletrônico** permite que o SNMP. O parâmetro **1!d** desabilita SNMP.|
|-i \<SNMPIndex|Especifica o índice SNMP, se SNMP está habilitado. Para obter mais informações, consulte a norma Rfc 1759 na [site do editor de Rfc](https://go.microsoft.com/fwlink/?LinkId=569).|
|-y \<CommunityName>|Especifica o nome da comunidade SNMP, se SNMP está habilitado.|
|-2{e &#124; -d}|Especifica se spools duplos (também conhecido como respool) estão habilitados para as portas TCP lpr. Spools duplos são necessários porque TCP lpr deve incluir uma contagem de bytes exata no arquivo de controle que é enviado para a impressora, mas o protocolo não é possível obter a contagem do provedor de impressão local. Portanto, quando um arquivo está em spool para um TCP lpr fila de impressão, ele também está em spool como um arquivo temporário no diretório system32. TCP lpr determina o tamanho do arquivo temporário e envia o tamanho para o servidor que executa LPD. O parâmetro **eletrônico** ativa spools duplos. O parâmetro **1!d** os desativa.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O **prnport** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo do arquivo prnport ou altere os diretórios para a pasta apropriada. Por exemplo: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).
-   O protocolo bruto de TCP é um protocolo de desempenho mais alto no Windows que o protocolo lpr.

## <a name="BKMK_examples"></a>Exemplos
Para exibir todas as portas de impressão TCP/IP padrão no servidor \\\Server1, digite:
```
cscript prnport -l -s Server1
```
Para excluir a porta de impressão TCP/IP padrão no servidor \\\Server1 que se conecta a uma impressora de rede em 10.2.3.4, tipo:
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Para adicionar uma porta de impressão TCP/IP padrão no servidor \\\Server1 que se conecta a uma impressora de rede em 10.2.3.4 e usa o protocolo bruto de TCP na porta 9100, tipo:
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Para habilitar o SNMP, especifique o nome da comunidade "public" e defina o índice SNMP para 1 em uma impressora de rede em 10.2.3.4 compartilhados pelo servidor \\\Server1, digite:
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Para adicionar uma porta de impressão TCP/IP padrão no computador local que se conecta a uma impressora de rede em 10.2.3.4 e obter automaticamente as configurações de dispositivo da impressora, digite:
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência do comando Imprimir](print-command-reference.md)
