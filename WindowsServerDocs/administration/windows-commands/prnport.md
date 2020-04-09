---
title: prnport
description: Saiba como criar, excluir e listar portas de impressora.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17f81b127927a41e60c290535032876def109989
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837219"
---
# <a name="prnport"></a>prnport

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria, exclui e lista portas de impressora TCP/IP padrão, além de exibir e alterar a configuração de porta.

## <a name="syntax"></a>Sintaxe
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                                                                                                                                                                                     Descrição                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a              |                                                                                                                                                                                                                                                                                       Cria uma porta de impressora TCP/IP padrão.                                                                                                                                                                                                                                                                                        |
|              -d              |                                                                                                                                                                                                                                                                                       exclui uma porta de impressora TCP/IP padrão.                                                                                                                                                                                                                                                                                        |
|              -l              |                                                                                                                                                                                                                                                             lista todas as portas de impressora TCP/IP padrão no computador especificado com o parâmetro **-s** .                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            Exibe a configuração de uma porta de impressora TCP/IP padrão.                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           Define as configurações de porta para uma porta de impressora TCP/IP padrão.                                                                                                                                                                                                                                                                           |
|        -r \<PortName >        |                                                                                                                                                                                                                                                                                Especifica a porta à qual a impressora está conectada.                                                                                                                                                                                                                                                                                 |
|       -s \<ServerName >       |                                                                                                                                                                                                                               Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.                                                                                                                                                                                                                                |
| -u \<nome de usuário >-w <Password> |                                                                                                              Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione.                                                                                                               |
|     -o {LPR &#124; não processado}      |                                                                                                                                                                                                              Especifica o protocolo que a porta usa: TCP bruto ou TCP LPR. Se você usar TCP bruto, poderá opcionalmente especificar o número da porta usando o parâmetro **-n** . O número da porta padrão é 9100.                                                                                                                                                                                                              |
|      -h \<HostAddress >       |                                                                                                                                                                                                                                                                   Especifica (por endereço IP) a impressora para a qual você deseja configurar a porta.                                                                                                                                                                                                                                                                    |
|       -q \<QueueName >        |                                                                                                                                                                                                                                                                                     Especifica o nome da fila para uma porta bruta TCP.                                                                                                                                                                                                                                                                                     |
|       -n \<PortNumber >       |                                                                                                                                                                                                                                                                    Especifica o número da porta para uma porta bruta TCP. O número da porta padrão é 9100.                                                                                                                                                                                                                                                                    |
|        -m {e &#124; d}        |                                                                                                                                                                                                                                                       Especifica se o SNMP está habilitado. O parâmetro **e** HABILITA o SNMP. O parâmetro **d** DESABILITA o SNMP.                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             Especifica o índice SNMP, se SNMP estiver habilitado. Para obter mais informações, consulte RFC 1759 no [site do editor da RFC](https://go.microsoft.com/fwlink/?LinkId=569).                                                                                                                                                                                                                              |
|     -y \<Comunitárioname >      |                                                                                                                                                                                                                                                                                Especifica o nome da comunidade SNMP, se o SNMP estiver habilitado.                                                                                                                                                                                                                                                                                |
|       -2 {e &#124; -d}        | Especifica se spools duplos (também conhecidos como recolocação em spool) estão habilitados para portas TCP LPR. Spools duplos são necessários porque TCP LPR deve incluir uma contagem de bytes precisa no arquivo de controle que é enviado para a impressora, mas o protocolo não pode obter a contagem do provedor de impressão local. Portanto, quando um arquivo é colocado em spool em uma fila de impressão TCP LPR, ele também é colocado em spool como um arquivo temporário no diretório system32. TCP LPR determina o tamanho do arquivo temporário e envia o tamanho para o servidor que executa o LPD. O parâmetro **e** habilita spools duplos. O parâmetro **d** desabilita spools duplos. |
|              /?              |                                                                                                                                                                                                                                                                                         Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>Comentários
-   O comando **prnport** é um script Visual Basic localizado no diretório <language> do printing_Admin_Scripts\\do%windir%\system32\. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnport ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `"computer Name"`).
-   O protocolo TCP bruto é um protocolo de desempenho mais alto no Windows do que o protocolo LPR.

## <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para exibir todas as portas de impressão TCP/IP padrão no servidor \\\Server1, digite:
```
cscript prnport -l -s Server1
```
Para excluir a porta de impressão TCP/IP padrão no servidor \\\Server1 que se conecta a uma impressora de rede em 10.2.3.4, digite:
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Para adicionar uma porta de impressão TCP/IP padrão no servidor \\\Server1 que se conecta a uma impressora de rede em 10.2.3.4 e usa o protocolo TCP bruto na porta 9100, digite:
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Para habilitar o SNMP, especifique o nome da Comunidade "público" e defina o índice SNMP como 1 em uma impressora de rede em 10.2.3.4 compartilhado pelo servidor \\\Server1, digite:
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Para adicionar uma porta de impressão TCP/IP padrão no computador local que se conecta a uma impressora de rede em 10.2.3.4 e obter automaticamente as configurações do dispositivo da impressora, digite:
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando de impressão](print-command-reference.md)
