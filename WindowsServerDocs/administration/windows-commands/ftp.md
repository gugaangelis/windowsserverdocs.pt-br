---
title: ftp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3639626962e295a54c9068c9731f1d30754a9472
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438374"
---
# <a name="ftp"></a>ftp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transferências de arquivos de e para um computador que executa um serviço do servidor de protocolo de transferência de arquivo (ftp). **FTP** pode ser usado interativamente ou em modo de lote com o processamento de arquivos de texto ASCII. 
## <a name="syntax"></a>Sintaxe
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                                                                                                      Descrição                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Suprime a exibição de respostas do servidor remoto.                                                                                                                                     |
|        -d         |                                                                                                               Habilita a depuração, exibindo todos os comandos passados entre o cliente FTP e o servidor FTP.                                                                                                                |
|        -i         |                                                                                                                            Desabilita o prompt interativo durante a transferência de vários arquivos.                                                                                                                             |
|        -n         |                                                                                                                                    Suprime o logon automático na conexão inicial.                                                                                                                                     |
|        -g         |                                         Desabilita o recurso de curinga de nome de arquivo.  **Glob** permite o uso do asterisco (\*) e o ponto de interrogação (?) como caracteres curinga em nomes de arquivo e caminho local. Para obter mais informações, consulte [referências adicionais](ftp.md#BKMK_additionalRef).                                          |
|   -s:<FileName>   | Especifica um arquivo de texto que contém **ftp** comandos. Esses comandos são executados automaticamente após **ftp** é iniciado. Esse parâmetro permite sem espaços. Use esse parâmetro, em vez de redirecionamento ( **<** ). **Observação:** No Windows 8 e Windows Server 2012 ou sistemas operacionais posteriores, o arquivo de texto deve ser escrito em UTF-8. |
|        -a         |                                                                                                                 Especifica que qualquer interface local pode ser usada ao associar a conexão de dados do ftp.                                                                                                                  |
|        -A         |                                                                                                                                        Faça logon no servidor de ftp como anônimo.                                                                                                                                         |
|  -x:<SendBuffer>  |                                                                                                                                     Substitui o tamanho SO_SNDBUF padrão de 8192.                                                                                                                                     |
|  -r:<RecvBuffer>  |                                                                                                                                     Substitui o tamanho SO_RCVBUF padrão de 8192.                                                                                                                                     |
| -b:<AsyncBuffers> |                                                                                                                                    Substitui a contagem do buffer padrão assíncrono de 3.                                                                                                                                     |
| -w:<WindowsSize>  |                                                                                                                   Especifica o tamanho do buffer de transferência. O tamanho da janela padrão é 4096 bytes.                                                                                                                   |
|        -?         |                                                                                                                                         Exibe a ajuda no prompt de comando.                                                                                                                                          |
|      <host>       |                                                                    Especifica o nome do computador, o endereço IP ou o endereço IPv6 do servidor ftp ao qual se conectar. O nome do host ou o endereço, se especificado, deve ser o último parâmetro na linha.                                                                    |

## <a name="remarks"></a>Comentários
- Para obter mais informações sobre **ftp** comandos no Windows Server 2003, consulte [ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- **FTP** parâmetros de linha de comando diferenciam maiusculas de minúsculas.
- Esse comando estará disponível somente se o **protocolo (TCP/IP)** é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.
- **FTP** pode ser usado interativamente. Depois de iniciado, **ftp** cria um ambiente de subpropriedades em que você pode usar **ftp** comandos. Você pode retornar ao prompt de comando, digitando o **encerrar** comando. Quando o **ftp** subpropriedades ambiente estiver em execução, é indicada pela **ftp >** prompt de comando. Para obter mais informações, consulte o **ftp** comandos.
- **FTP** suporta o uso de IPv6 quando o protocolo IPv6 é instalado. Para obter mais informações, consulte [referências adicionais](ftp.md#BKMK_additionalRef).
  ## <a name="BKMK_Examples"></a>Exemplos
  Para fazer logon no servidor de ftp chamado FTP example. microsoft.com, digite:
  ```
  ftp ftp.example.microsoft.com
  ```
  Para fazer logon no servidor ftp chamado FTP example. microsoft.com e execute o **ftp** comandos contidos em um arquivo chamado resync.txt, tipo:
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>Referências adicionais
- [IP versão 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Aplicativos de IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
