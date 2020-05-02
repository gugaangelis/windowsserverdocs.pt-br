---
title: ftp
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fa956124e0c227d048d4c6eec844154187d5861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725051"
---
# <a name="ftp"></a>ftp

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfere arquivos de e para um computador que executa um serviço de servidor protocolo FTP (FTP). o **FTP** pode ser usado interativamente ou em modo de lote por meio do processamento de arquivos de texto ASCII. 
## <a name="syntax"></a>Sintaxe
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
#### <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                                                                                                      Descrição                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Suprime a exibição de respostas de servidor remoto.                                                                                                                                     |
|        -d         |                                                                                                               Habilita a depuração, exibindo todos os comandos passados entre o cliente FTP e o servidor FTP.                                                                                                                |
|        -i         |                                                                                                                            Desabilita a solicitação interativa durante várias transferências de arquivos.                                                                                                                             |
|        -n         |                                                                                                                                    Suprime o logon automático na conexão inicial.                                                                                                                                     |
|        -g         |                                         Desabilita o mascaramento do nome de arquivo.  **Glob** permite o uso do asterisco (\*) e do ponto de interrogação (?) como caracteres curinga em nomes de caminho e arquivo local. Para obter mais informações, consulte [referências adicionais](ftp.md#BKMK_additionalRef).                                          |
|   &<FileName>   | Especifica um arquivo de texto que contém comandos **FTP** . Esses comandos são executados automaticamente após o início do **FTP** . Esse parâmetro não permite nenhum espaço. Use esse parâmetro em vez de redirecionamento**<**(). **Observação:** No Windows 8 e no Windows Server 2012 ou em sistemas operacionais posteriores, o arquivo de texto deve ser escrito em UTF-8. |
|        -a         |                                                                                                                 Especifica que qualquer interface local pode ser usada ao associar a conexão de dados de FTP.                                                                                                                  |
|        -A         |                                                                                                                                        Faz logon no servidor FTP como anônimo.                                                                                                                                         |
|  w.x.y.<SendBuffer>  |                                                                                                                                     Substitui o tamanho de SO_SNDBUF padrão de 8192.                                                                                                                                     |
|  d<RecvBuffer>  |                                                                                                                                     Substitui o tamanho de SO_RCVBUF padrão de 8192.                                                                                                                                     |
| b<AsyncBuffers> |                                                                                                                                    Substitui a contagem de buffers assíncronos padrão de 3.                                                                                                                                     |
| Mostrar<WindowsSize>  |                                                                                                                   Especifica o tamanho do buffer de transferência. O tamanho da janela padrão é 4096 bytes.                                                                                                                   |
|        -?         |                                                                                                                                         Exibe a ajuda no prompt de comando.                                                                                                                                          |
|      <host>       |                                                                    Especifica o nome do computador, o endereço IP ou o endereço IPv6 do servidor FTP ao qual se conectar. O nome do host ou endereço, se especificado, deve ser o último parâmetro na linha.                                                                    |

## <a name="remarks"></a>Comentários
- para obter mais informações sobre comandos de **FTP** no Windows Server 2003, consulte [FTP](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- os parâmetros de linha de comando **FTP** diferenciam maiúsculas de minúsculas.
- Esse comando estará disponível somente se o protocolo **TCP/IP** estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.
- o **FTP** pode ser usado interativamente. Depois de iniciado, o **FTP** cria um subambiente no qual você pode usar comandos de **FTP** . Você pode retornar ao prompt de comando digitando o comando **Quit** . Quando o subambiente **FTP** está em execução, ele é indicado pelo prompt de comando **FTP >** . Para obter mais informações, consulte os comandos de **FTP** .
- o **FTP** dá suporte ao uso do IPv6 quando o protocolo IPv6 é instalado. Para obter mais informações, consulte [referências adicionais](ftp.md#BKMK_additionalRef).
  ## <a name="examples"></a>Exemplos
  Para fazer logon no servidor FTP chamado ftp.example.microsoft.com, digite:
  ```
  ftp ftp.example.microsoft.com
  ```
  Para fazer logon no servidor FTP chamado ftp.example.microsoft.com e executar os comandos de **FTP** contidos em um arquivo chamado Ressync. txt, digite:
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="additional-references"></a><a name=BKMK_additionalRef></a>referências adicionais
- [IP versão 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Aplicativos IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
