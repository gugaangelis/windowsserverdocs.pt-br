---
title: prndrvr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edf27852c13566ba8c7ca8c16d789d586e749160
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436215"
---
# <a name="prndrvr"></a>prndrvr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Use o **prndrvr** comando para adicionar, excluir e listar drivers de impressora.

## <a name="syntax"></a>Sintaxe
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|-a|Instala um driver.|
|-d|Exclui um driver.|
|-l|lista todos os drivers de impressora instalados no servidor especificado pela **-s** parâmetro. Se você não especificar um servidor, o Windows listará os drivers de impressora instalados no computador local.|
|-x|Exclui todos os drivers de impressora e drivers de impressora adicionais não está em uso por uma impressora lógica no servidor especificado o **-s** parâmetro. Se você não especificar um servidor a ser removido da lista, o Windows exclui todos os drivers de impressora não usados no computador local.|
|-m \<DrivermodelName\>|Especifica o driver que você deseja instalar (por nome). Drivers geralmente são nomeados para o modelo de impressora para que dar suporte a eles. Consulte a documentação da impressora para obter mais informações.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Especifica a versão do driver que você deseja instalar. Consulte a descrição do **-e**parâmetro para obter informações sobre quais versões estão disponíveis para cada ambiente. Se você não especificar uma versão, a versão do driver apropriado para a versão do Windows em execução no computador onde você está instalando o driver está instalada.<br /><br />-versão **0** dá suporte ao Windows 95, Windows 98 e Windows Millennium edition.<br />-versão **1** dá suporte ao Windows NT 3.51.<br />-versão **2** dá suporte ao Windows NT 4.0.<br />-versão **3** dá suporte ao Windows Vista, Windows XP, Windows 2000 e os sistemas operacionais Windows Server 2003. Observe que isso é a única versão de driver de impressora que dá suporte ao Windows Vista.|
|-e \<Environment>|Especifica o ambiente para o driver que você deseja instalar. Se você não especificar um ambiente, o ambiente do computador onde você está instalando o driver será usado. Os parâmetros de ambiente com suporte são:<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows IA64"**|
|-s \<ServerName>|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|
|-u \<nome de usuário > -w \<senha >|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores local do computador de destino têm essas permissões, mas também podem ser concedidas as permissões a outros usuários. Se você não especificar uma conta, você deve fazer logon em uma conta com essas permissões para o comando funcione.|
|-h \<path>|Especifica o caminho para o arquivo de driver. Se você não especificar um caminho, o caminho para o local em que o Windows foi instalado será usado.|
|-i \<Filename.inf>|Especifica o nome de arquivo e caminho completo para o driver que você deseja instalar. Se você não especificar um nome de arquivo, o script usa um dos arquivos da caixa de entrada impressora. inf no subdiretório do diretório Windows inf.<br /><br />Se o caminho do driver não for especificado, o script procura arquivos de driver no arquivo CAB.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
- O **prndrvr** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo do arquivo prndrvr ou altere os diretórios para a pasta apropriada.

  Por exemplo:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).
- O x - opção exclui todos os drivers de impressora adicionais (drivers instalados para uso em clientes alternativas executando versões do Windows), mesmo se o driver primário está em uso. Se o componente de fax é instalado, essa opção também exclui os drivers de fax. O driver de fax primário é excluído se não estiver em uso (ou seja, se não houver nenhuma fila usá-lo). Se o driver de fax primário for excluído, a única maneira de habilitar novamente o fax é reinstalar o componente de fax.
- Usado sem parâmetros, **prndrvr** exibe a Ajuda de linha de comando para o **prndrvr** comando.

## <a name="BKMK_examples"></a>Exemplos

Para listar todos os drivers no \\\printServer1 server, digite:
```
cscript Prndrvr -l -s
```

Para adicionar um driver de impressora do Windows x64 versão 3 para o modelo de "modelo de impressora a Laser 1" da impressora usando o arquivo de informações do driver de C:\temp\Laserprinter1.inf para um driver armazenado no tipo de pasta C:\temp:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

Para excluir um driver de impressora do Windows NT x86 versão 3 para o "modelo de impressora a Laser 1", digite:
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência do comando Imprimir](print-command-reference.md)
