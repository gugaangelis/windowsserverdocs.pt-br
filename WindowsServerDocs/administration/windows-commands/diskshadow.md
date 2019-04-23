---
title: diskshadow
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c5648235a1c856c6aef09621e2381e74d08d70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869177"
---
# <a name="diskshadow"></a>diskshadow

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DiskShadow.exe é uma ferramenta que expõe a funcionalidade oferecida pela cópia de sombra de volume Service \(VSS\). Por padrão, o diskshadow usa um interpretador de comandos interativo semelhante ao encontrado no diskraid ou DiskPart. o DiskShadow também inclui um modo programável por script.  
  
> [!NOTE]  
> A associação no grupo Administradores local, ou equivalente, é o mínimo necessário para executar o diskshadow.  
  
Para obter exemplos de como usar comandos do diskshadow, consulte [exemplos](#BKMK_examples).  
  
## <a name="syntax"></a>Sintaxe  
para o modo interativo, digite o seguinte no prompt de comando para iniciar o interpretador de comando do diskshadow:  
  
```  
diskshadow  
```  
  
para o modo de script, digite o seguinte, onde *script* é um arquivo de script que contém os comandos do diskshadow:  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>o DiskShadow comandos  
Você pode executar os comandos a seguir no interpretador de comando do diskshadow ou por meio de um arquivo de script:  
  
|Parâmetro|Descrição|  
|-------|--------|  
|[set_2](set_2.md)|Define o contexto, opções, o modo detalhado e arquivo de metadados para a criação de cópias de sombra.|  
|[Simular a restauração](simulate-restore.md)|Testa o envolvimento do gravador em sessões de restauração no computador sem emitir **PreRestore** ou **PostRestore** eventos para gravadores.|  
|[Carregar metadados](load-metadata.md)|Carrega um arquivo. cab de metadados antes de importar uma cópia de sombra transportáveis ou carrega os metadados do gravador no caso de uma restauração.|  
|[writer](writer.md)|Verifica se um gravador ou o componente está incluído ou exclui um componente ou o gravador do procedimento de backup ou restauração.|  
|[add_1](add_1.md)|Adiciona os volumes para o conjunto de volumes que devem ser copiados em sombra ou aliases para o ambiente de alias.|  
|[create_1](create_1.md)|Inicia o processo de criação de cópia de sombra, usando as configurações de opção e o contexto atuais.|  
|[exec](exec.md)|Executa um arquivo no computador local.|  
|[Início do backup](begin-backup.md)|Inicia uma sessão de backup completo.|  
|[Backup do final](end-backup.md)|Encerra uma sessão de backup completo e problemas de um **Backupcomplete** evento com o estado do gravador apropriado, se necessário.|  
|[Iniciar a restauração](begin-restore.md)|Inicia uma sessão de restauração e problemas de um **PreRestore** eventos para gravadores envolvidos.|  
|[Restauração do fim](end-restore.md)|Encerra uma sessão de restauração e problemas de um **PostRestore** eventos para gravadores envolvidos.|  
|[reset](reset.md)|Redefine o diskshadow para o estado padrão.|  
|[list](list.md)|Lista os gravadores, cópias de sombra ou provedores de cópia de sombra registrados atualmente estão no sistema.|  
|[Excluir sombras](delete-shadows.md)|Exclusões de cópias de sombra.|  
|[import](import.md)|Importa uma cópia de sombra transportáveis de um arquivo de metadados carregados no sistema.|  
|[mask](mask.md)|Remove as cópias de sombra de hardware que foram importadas usando o **importação** comando.|  
|[expose](expose.md)|Expõe uma cópia de sombra persistente como uma letra de unidade, compartilhamento ou ponto de montagem.|  
|[unexpose](unexpose.md)|Unexposes uma cópia de sombra foi exposta por meio de **expor** comando.|  
|[break_2](break_2.md)|Desassocia um volume de cópia de sombra do VSS.|  
|[revert](revert.md)|Reverte o volume de volta para uma cópia de sombra especificado.|  
|[exit_1](exit_1.md)|sai do diskshadow.|  
  
## <a name="remarks"></a>Comentários  
  
-   no mínimo, apenas **adicione** e **criar** são necessárias para criar uma cópia de sombra. No entanto, isso, perderá as configurações de contexto e a opção, será um backup de cópia e apenas criará uma cópia de sombra com nenhum script de execução do backup.  
  
## <a name="BKMK_examples"></a>Exemplos  
Isso é um exemplo de sequência de comandos que criará uma cópia de sombra de backup. Ele pode ser salvo para o arquivo como script.dsh e executado com o diskshadow \/script.dsh s  
  
Supor o seguinte:  
  
-   Você tem um diretório existente chamado c:\\diskshadowdata.  
  
-   O volume do sistema é c: e seu volume de dados é a unidade d:.  
  
-   Você tem um arquivo backupscript.cmd em c:\\diskshadowdata.  
  
-   Seu arquivo backupscript.cmd executará a cópia de sombra dados p e p: a unidade de backup.  
  
Você pode inserir esses comandos manualmente ou script:  
  
```  
#diskshadow script file  
set context persistent nowriters  
set metadata c:\diskshadowdata\example.cab  
set verbose on  
begin backup  
add volume c: alias Systemvolumeshadow  
add volume d: alias Datavolumeshadow  
  
create  
  
expose %Systemvolumeshadow% p:  
expose %Datavolumeshadow% q:  
exec c:\diskshadowdata\backupscript.cmd  
end backup  
#End of script  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

