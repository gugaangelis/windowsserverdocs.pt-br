---
title: diskshadow
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8d9f34377473608d71ce7753972e5312d9eea0f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377770"
---
# <a name="diskshadow"></a>diskshadow

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

o DiskShadow. exe é uma ferramenta que expõe a funcionalidade oferecida pelo serviço de cópias de sombra de volume \(VSS @ no__t-1. Por padrão, o DiskShadow usa um interpretador de comando interativo semelhante ao do DiskRAID ou DiskPart. o DiskShadow também inclui um modo programável.  
  
> [!NOTE]  
> A associação no grupo local de administradores, ou equivalente, é o requisito mínimo necessário para executar o DiskShadow.  
  
para obter exemplos de como usar comandos DiskShadow, consulte [exemplos](#BKMK_examples).  
  
## <a name="syntax"></a>Sintaxe  
para o modo interativo, digite o seguinte no prompt de comando para iniciar o interpretador de comando do DiskShadow:  
  
```  
diskshadow  
```  
  
em modo de script, digite o seguinte, em que *script. txt* é um arquivo de script que contém comandos DiskShadow:  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>comandos do DiskShadow  
Você pode executar os seguintes comandos no interpretador de comandos do DiskShadow ou por meio de um arquivo de script:  
  
|Parâmetro|Descrição|  
|-------|--------|  
|[set_2](set_2.md)|Define o contexto, as opções, o modo detalhado e o arquivo de metadados para criar cópias de sombra.|  
|[Simular restauração](simulate-restore.md)|Testa o envolvimento do gravador em sessões de restauração no computador sem emitir eventos **Prerestore** ou **createrestore** para gravadores.|  
|[Carregar metadados](load-metadata.md)|Carrega um arquivo Metadata. cab antes de importar uma cópia de sombra transportável ou carrega os metadados do gravador no caso de uma restauração.|  
|[escritor](writer.md)|verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração.|  
|[add_1](add_1.md)|Adiciona volumes ao conjunto de volumes que devem ser copiados em sombra ou adiciona aliases ao ambiente de alias.|  
|[create_1](create_1.md)|inicia o processo de criação de cópia de sombra usando as configurações de contexto e opção atuais.|  
|[exec](exec.md)|executa um arquivo no computador local.|  
|[Iniciar backup](begin-backup.md)|inicia uma sessão de backup completo.|  
|[Finalizar backup](end-backup.md)|Encerra uma sessão de backup completo e emite um evento **Backupcomplete** com o estado de gravador apropriado, se necessário.|  
|[Iniciar restauração](begin-restore.md)|inicia uma sessão de restauração e emite um evento de **Prerestore** para gravadores envolvidos.|  
|[Finalizar restauração](end-restore.md)|Encerra uma sessão de restauração e emite um evento de **restauração** para gravadores envolvidos.|  
|[definido](reset.md)|Redefine o DiskShadow para o estado padrão.|  
|[lista](list.md)|lista gravadores, cópias de sombra ou provedores de cópia de sombra atualmente registrados que estão no sistema.|  
|[excluir sombras](delete-shadows.md)|exclui cópias de sombra.|  
|[importe](import.md)|importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema.|  
|[mascara](mask.md)|Remove cópias de sombra de hardware que foram importadas usando o comando de **importação** .|  
|[Expo](expose.md)|expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.|  
|[Cancele](unexpose.md)|Não expõe uma cópia de sombra que foi exposta usando o comando **Expo** .|  
|[break_2](break_2.md)|Desassocia um volume de cópia de sombra do VSS.|  
|[voltar](revert.md)|reverte um volume de volta para uma cópia de sombra especificada.|  
|[exit_1](exit_1.md)|Sai do DiskShadow.|  
  
## <a name="remarks"></a>Comentários  
  
-   no mínimo, apenas **Adicionar** e **criar** são necessários para criar uma cópia de sombra. No entanto, isso perderá as configurações de contexto e opção, será um backup de cópia e criará apenas uma cópia de sombra sem nenhum script de execução de backup.  
  
## <a name="BKMK_examples"></a>Disso  
Esta é uma sequência de exemplo de comandos que criará uma cópia de sombra para backup. Ele pode ser salvo em arquivo como script. DSH e executado com DiskShadow \/s script. DSH  
  
Suponha o seguinte:  
  
-   Você tem um diretório existente chamado c: \\diskshadowdata.  
  
-   O volume do sistema é C: e seu volume de dados é D:.  
  
-   Você tem um arquivo backupscript. cmd em c: \\diskshadowdata.  
  
-   O arquivo backupscript. cmd executará a cópia dos dados de sombra p: e q: em sua unidade de backup.  
  
Você pode inserir esses comandos manualmente ou fazer o script deles:  
  
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
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

