---
title: prnport
description: Tópico de referência para o comando prnport, que cria, exclui e lista portas de impressora TCP/IP padrão, além de exibir e alterar a configuração de porta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c209c06c2253e924e5a71753fec0b8ab0ee158d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472201"
---
# <a name="prnport"></a>prnport

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, exclui e lista portas de impressora TCP/IP padrão, além de exibir e alterar a configuração de porta. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnport ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport`.

## <a name="syntax"></a>Sintaxe

```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <portname>] [-s <Servername>] [-u <Username>] [-w <password>] [-o {raw | lpr}] [-h <Hostaddress>] [-q <Queuename>] [-n <portnumber>] -m{e | d} [-i <SNMPindex>] [-y <communityname>] -2{e | -d}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -a | Cria uma porta de impressora TCP/IP padrão. |
| -d | Exclui uma porta de impressora TCP/IP padrão. |
| -l | Lista todas as portas de impressora TCP/IP padrão no computador especificado pelo parâmetro **-s** . |
| -g | Exibe a configuração de uma porta de impressora TCP/IP padrão. |
| -T | Define as configurações de porta para uma porta de impressora TCP/IP padrão. |
| -r`<portname>` | Especifica a porta à qual a impressora está conectada. |
| -s`<Servername>` | Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado. |
| -u `<Username>` -w`<password>` | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
| -o`{raw|lpr}` | Especifica o protocolo que a porta usa: TCP bruto ou TCP LPR. O protocolo TCP bruto é um protocolo de desempenho mais alto no Windows do que o protocolo LPR. Se você usar TCP bruto, poderá opcionalmente especificar o número da porta usando o parâmetro **-n** . O número da porta padrão é 9100. |
| -h`<Hostaddress>` | Especifica (por endereço IP) a impressora para a qual você deseja configurar a porta. |
| -q`<Queuename>` | Especifica o nome da fila para uma porta bruta TCP. |
| -n`<portnumber>` | Especifica o número da porta para uma porta bruta TCP. O número da porta padrão é 9100. |
| -m`{e|d}` | Especifica se o SNMP está habilitado. O parâmetro **e** HABILITA o SNMP. O parâmetro **d** DESABILITA o SNMP. |
| -i`<SNMPindex` | Especifica o índice SNMP, se SNMP estiver habilitado. Para obter mais informações, consulte **rfc 1759** no [site do editor RFC](https://www.ietf.org/rfc/rfc1759.txt?number=1759). |
| -y`<communityname>` | Especifica o nome da comunidade SNMP, se o SNMP estiver habilitado. |
| -2`{e|-d}` | Especifica se spools duplos (também conhecidos como recolocação em spool) estão habilitados para portas TCP LPR. Spools duplos são necessários porque TCP LPR deve incluir uma contagem de bytes precisa no arquivo de controle que é enviado para a impressora, mas o protocolo não pode obter a contagem do provedor de impressão local. Portanto, quando um arquivo é colocado em spool em uma fila de impressão TCP LPR, ele também é colocado em spool como um arquivo temporário no diretório system32. TCP LPR determina o tamanho do arquivo temporário e envia o tamanho para o servidor que executa o LPD. O parâmetro **e** habilita spools duplos. O parâmetro **d** desabilita spools duplos. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

### <a name="examples"></a>Exemplos

Para exibir todas as portas de impressão TCP/IP padrão no servidor \\ Server1, digite:

```
cscript prnport -l -s Server1
```

Para excluir a porta de impressão TCP/IP padrão no servidor \\ Server1 que se conecta a uma impressora de rede em 10.2.3.4, digite:

```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```

Para adicionar uma porta de impressão TCP/IP padrão no servidor \\ Server1 que se conecta a uma impressora de rede em 10.2.3.4 e usa o protocolo TCP bruto na porta 9100, digite:

```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```

Para habilitar o SNMP, especifique o nome da Comunidade "público" e defina o índice SNMP como 1 em uma impressora de rede em 10.2.3.4 compartilhado pelo servidor \\ Server1, digite:

```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```

Para adicionar uma porta de impressão TCP/IP padrão no computador local que se conecta a uma impressora de rede em 10.2.3.4 e obter automaticamente as configurações do dispositivo da impressora, digite:

```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
