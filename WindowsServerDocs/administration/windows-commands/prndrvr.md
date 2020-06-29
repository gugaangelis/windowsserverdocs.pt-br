---
title: prndrvr
description: Tópico de referência para o comando prndrvr, que adiciona, exclui e lista os drivers de impressora.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62c63819c175f4b3f3770d90da0bd560443ccb77
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472291"
---
# <a name="prndrvr"></a>prndrvr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona, exclui e lista drivers de impressora. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prndrvr ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr`.

Usado sem parâmetros, **prndrvr** exibe a ajuda da linha de comando.

## <a name="syntax"></a>Sintaxe

```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] [-e <environment>] [-s <Servername>] [-u <Username>] [-w <password>] [-h <path>] [-i <inf file>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -a | Instala um driver. |
| -d | Exclui um driver. |
| -l | Lista todos os drivers de impressora instalados no servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor, o Windows listará os drivers de impressora instalados no computador local. |
| -X | Exclui todos os drivers de impressora e drivers de impressora adicionais que não estão em uso por uma impressora lógica no servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor a ser removido da lista, o Windows excluirá todos os drivers de impressora não utilizados no computador local. |
| -m`<model_name>` | Especifica (por nome) o driver que você deseja instalar. Os drivers geralmente são nomeados para o modelo de impressora para os quais dão suporte. Consulte a documentação da impressora para obter mais informações. |
| `-v {0|1|2|3}` | Especifica a versão do driver que você deseja instalar. Consulte a descrição do parâmetro **-e**para obter informações sobre quais versões estão disponíveis para qual ambiente. Se você não especificar uma versão, a versão do driver apropriada para a versão do Windows em execução no computador em que você está instalando o driver será instalada. |
| -e `<environment>` | Especifica o ambiente do driver que você deseja instalar. Se você não especificar um ambiente, o ambiente do computador em que você está instalando o driver será usado. Os parâmetros de ambiente com suporte são: **Windows NT x86**, **Windows x64** ou **Windows IA64**. |
| -s`<Servername>` | Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado. |
| -u `<Username>` -w`<password>` | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
| -h`<path>` | Especifica o caminho para o arquivo de driver. Se você não especificar um caminho, será usado o caminho para o local onde o Windows foi instalado. |
| -i`<filename.inf>` | Especifica o caminho completo e o nome do arquivo para o driver que você deseja instalar. Se você não especificar um nome de arquivo, o script usará um dos arquivos. inf de impressora da caixa de entrada no subdiretório inf do diretório do Windows.<p>Se o caminho do driver não for especificado, o script pesquisará os arquivos de driver no arquivo de driver.cab. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

- O parâmetro **-x** exclui todos os drivers de impressora adicionais (drivers instalados para uso em clientes que executam versões alternativas do Windows), mesmo que o driver primário esteja em uso. Se o componente de fax estiver instalado, essa opção também excluirá drivers de fax. O driver de fax primário será excluído se ele não estiver em uso (ou seja, se não houver nenhuma fila usando-o). Se o driver de fax primário for excluído, a única maneira de reabilitar o fax é reinstalar o componente de fax.

### <a name="examples"></a>Exemplos

Para listar todos os drivers no \\ servidor do FileServer1 local, digite:

```
cscript prndrvr -l -s
```

Para adicionar um driver de impressora do Windows x64 da versão 3 para o modelo de impressora laser modelo 1 de impressora usando o arquivo de informações do driver c:\temp\Laserprinter1.inf para um driver armazenado na pasta c:\temp, digite:

```
cscript prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64 -i c:\temp\Laserprinter1.inf -h c:\temp
```

Para excluir um driver de impressora do Windows x64 versão 3 para o modelo de impressora a laser 1, digite:

```
cscript prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
