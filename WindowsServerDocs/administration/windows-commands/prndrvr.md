---
title: prndrvr
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eebb28ae50f4546ac5ab3d495994c96e293f6928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837299"
---
# <a name="prndrvr"></a>prndrvr

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use o comando **prndrvr** para adicionar, excluir e listar drivers de impressora.

## <a name="syntax"></a>Sintaxe
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|-a|Instala um driver.|
|-d|exclui um driver.|
|-l|lista todos os drivers de impressora instalados no servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor, o Windows listará os drivers de impressora instalados no computador local.|
|{1&gt;-&lt;1}x|exclui todos os drivers de impressora e drivers de impressora adicionais que não estão em uso por uma impressora lógica no servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor a ser removido da lista, o Windows excluirá todos os drivers de impressora não utilizados no computador local.|
|-m \<DrivermodelName\>|Especifica (por nome) o driver que você deseja instalar. Os drivers geralmente são nomeados para o modelo de impressora para os quais dão suporte. Consulte a documentação da impressora para obter mais informações.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Especifica a versão do driver que você deseja instalar. Consulte a descrição do parâmetro **-e**para obter informações sobre quais versões estão disponíveis para qual ambiente. Se você não especificar uma versão, a versão do driver apropriada para a versão do Windows em execução no computador em que você está instalando o driver será instalada.<p>-a versão **0** dá suporte ao Windows 95, Windows 98 e Windows Millennium Edition.<br />-a versão **1** dá suporte ao Windows NT 3,51.<br />-a versão **2** dá suporte ao Windows NT 4,0.<br />-a versão **3** dá suporte ao Windows Vista, ao Windows XP, ao Windows 2000 e aos sistemas operacionais windows Server 2003. Observe que essa é a única versão do driver de impressora à qual o Windows Vista dá suporte.|
|-e \<ambiente >|Especifica o ambiente do driver que você deseja instalar. Se você não especificar um ambiente, o ambiente do computador em que você está instalando o driver será usado. Os parâmetros de ambiente com suporte são:<p>-   o **Windows NT x86**<br />-   **Windows x64**<br />-   **Windows IA64**|
|-s \<ServerName >|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|
|-u \<nome de usuário >-w \<senha >|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione.|
|-h \<caminho >|Especifica o caminho para o arquivo de driver. Se você não especificar um caminho, será usado o caminho para o local onde o Windows foi instalado.|
|-i \<filename. inf >|Especifica o caminho completo e o nome do arquivo para o driver que você deseja instalar. Se você não especificar um nome de arquivo, o script usará um dos arquivos. inf de impressora da caixa de entrada no subdiretório inf do diretório do Windows.<p>Se o caminho do driver não for especificado, o script pesquisará os arquivos de driver no arquivo. cab do driver.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
- O comando **prndrvr** é um script de Visual Basic localizado no diretório <language> de\\do%windir%\system32\ printing_Admin_Scripts. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prndrvr ou altere os diretórios para a pasta apropriada.

  Por exemplo:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `computer Name`).
- A opção-x exclui todos os drivers de impressora adicionais (drivers instalados para uso em clientes que executam versões alternativas do Windows), mesmo que o driver primário esteja em uso. Se o componente de fax estiver instalado, essa opção também excluirá drivers de fax. O driver de fax primário será excluído se ele não estiver em uso (ou seja, se não houver nenhuma fila usando-o). Se o driver de fax primário for excluído, a única maneira de reabilitar o fax é reinstalar o componente de fax.
- Usado sem parâmetros, **prndrvr** exibe a ajuda de linha de comando para o comando **prndrvr** .

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para listar todos os drivers no servidor do \\\printServer1, digite:
```
cscript Prndrvr -l -s
```

Para adicionar um driver de impressora do Windows x64 da versão 3 para o modelo de impressora laser modelo 1 de impressora usando o arquivo de informações do driver C:\temp\Laserprinter1.inf para um driver armazenado no tipo de pasta C:\Temp:
```
cscript Prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64 -i c:\temp\Laserprinter1.inf -h c:\temp
```

Para excluir um driver de impressora do Windows NT x86 versão 3 para o modelo de impressora a laser 1, digite:
```
cscript Prndrvr -a -m Laser printer model 1 -v 3 -e Windows NT x86 
```

## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando de impressão](print-command-reference.md)
