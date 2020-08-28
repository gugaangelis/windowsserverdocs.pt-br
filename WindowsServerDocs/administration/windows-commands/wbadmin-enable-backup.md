---
title: wbadmin enable backup
description: Artigo de referência para Wbadmin enable backup, que cria e habilita um agendamento de backup diário ou modifica um agendamento de backup existente.
ms.topic: reference
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6be32f6134bacf698d6e28998cbed76e8b50155f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032064"
---
# <a name="wbadmin-enable-backup"></a>wbadmin enable backup



Cria e habilita um agendamento de backup diário ou modifica um agendamento de backup existente. Sem parâmetros especificados, ele exibe as configurações de backup agendadas no momento.

Para configurar ou modificar um agendamento de backup diário, você deve ser membro do grupo **Administradores** ou **operadores de backup** . Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando** e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

Sintaxe do Windows Server 2008:
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
Sintaxe do Windows Server 2008 R2:
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-allCritical]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
```
Sintaxe do Windows Server 2012 e do Windows Server 2012 R2:
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-hyperv:<HyperVComponentsToExclude>]
[-allCritical]
[-systemState]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
[-allowDeleteOldBackups]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-AddTarget|Para o Windows Server 2008, especifica o local de armazenamento para backups. Exige que você especifique um destino para backups como um identificador de disco (consulte comentários). O disco é formatado antes do uso, e todos os dados existentes nele são apagados permanentemente.</br>Para o Windows Server 2008 R2 e posterior, especifica o local de armazenamento para backups. Exige que você especifique o local como um disco, volume ou caminho UNC (Convenção de nomenclatura universal) para uma pasta compartilhada remota ( \\ \\ \<servername> \<sharename> \) . Por padrão, o backup será salvo em: \\ \\ <servername> \<sharename> \WindowsImageBackup\<ComputerBackedUp>\. Se você especificar um disco, o disco será formatado antes do uso e todos os dados existentes nele serão apagados permanentemente. Se você especificar uma pasta compartilhada, não poderá adicionar mais locais. Você só pode especificar uma pasta compartilhada como um local de armazenamento de cada vez.</br>Importante: se você salvar um backup em uma pasta compartilhada remota, esse backup será substituído se você usar a mesma pasta para fazer backup do mesmo computador novamente. Além disso, se a operação de backup falhar, você poderá acabar sem nenhum backup, pois o backup mais antigo será substituído, mas o backup mais recente não será utilizável. Você pode evitar isso criando subpastas na pasta compartilhada remota para organizar os backups. Se você fizer isso, as subpastas precisarão de duas vezes o espaço da pasta pai.</br>Apenas um local pode ser especificado em um único comando. Vários locais de armazenamento de backup de volume e disco podem ser adicionados executando o comando novamente.|
|-removetarget|Especifica o local de armazenamento que você deseja remover do agendamento de backup existente. Exige que você especifique o local como um identificador de disco (consulte comentários).|
|-agenda|Especifica as horas do dia para criar um backup, formatado como HH: MM e delimitado por vírgula.|
|-incluir|Para o Windows Server 2008, especifica a lista delimitada por vírgulas de letras de unidade de volume, pontos de montagem de volume ou nomes de volume baseados em GUID a serem incluídos no backup.</br>Para o Windows Server 2008 R2and posterior, especifica a lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( \) . Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo.|
|-nonRecurseInclude|Para o Windows Server 2008 R2 e posterior, especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( \) . Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro-backupTarget é usado.|
|-excluir|Para o Windows Server 2008 R2 e posterior, especifica a lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( \) . Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo.|
|-nonRecurseExclude|Para o Windows Server 2008 R2 e posterior, especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( \) . Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo.|
|-HyperV|Especifica a lista delimitada por vírgulas de componentes a serem incluídos no backup. O identificador pode ser um nome de componente ou GUID de componente (com ou sem chaves).|
|-SystemState|Para o Windows ° 7 e o Windows Server 2008 R2 e posterior, o cria um backup que inclui o estado do sistema, além de quaisquer outros itens que você especificou com o parâmetro **-include** . O estado do sistema contém arquivos de inicialização (Boot.ini, NDTLDR, NTDetect.com), o registro do Windows, incluindo configurações COM, o SYSVOL (diretivas de grupo e scripts de logon), o Active Directory e o NTDS. DIT em controladores de domínio e, se o serviço de certificados estiver instalado, o repositório de certificados. Se o servidor tiver a função de servidor Web instalada, o metadiretório do IIS será incluído. Se o servidor fizer parte de um cluster, Serviço de cluster informações também serão incluídas.|
|-missão crítica|Especifica que todos os volumes críticos (volumes que contêm o estado do sistema operacional) sejam incluídos nos backups. Esse parâmetro será útil se você estiver criando um backup para recuperação completa do sistema ou do estado do sistema. Ele deve ser usado somente quando-backupTarget for especificado, caso contrário, o comando falhará. Pode ser usado com a opção **-include** .</br>Dica: o volume de destino para um backup de volume crítico pode ser uma unidade local, mas não pode ser qualquer um dos volumes incluídos no backup.|
|-vssFull|Para o Windows Server 2008 R2 e posterior, o executa um backup completo usando o Serviço de Cópias de Sombra de Volume (VSS). Todos os arquivos são submetidos a backup, o histórico de cada arquivo é atualizado para refletir que foi feito backup e os logs de backups anteriores podem estar truncados. Se esse parâmetro não for usado, o Wbadmin start backup fará um backup de cópia, mas o histórico de arquivos cujo backup está sendo feito não será atualizado.</br>Cuidado: não use esse parâmetro se você estiver usando um produto que não seja Backup do Windows Server para fazer backup de aplicativos que estão nos volumes incluídos no backup atual. Isso pode potencialmente dividir o tipo de backup incremental, diferencial ou outro que o outro produto de backup está criando, pois o histórico do qual eles dependem para determinar a quantidade de dados para o backup pode estar ausente e pode executar um backup completo desnecessariamente.|
|-vssCopy|Para o Windows Server 2008 R2 e posterior, o executa um backup de cópia usando o VSS. Todos os arquivos são submetidos a backup, mas o histórico dos arquivos que estão sendo atualizados não é atualizado, de modo que você preserva todas as informações em quais arquivos foram alterados, excluídos e assim por diante, bem como quaisquer arquivos de log do aplicativo. O uso desse tipo de backup não afeta a sequência de backups incrementais e diferenciais que podem ocorrer independentemente desse backup de cópia. Esse é o valor padrão.</br>Aviso: um backup de cópia não pode ser usado para backups ou restaurações incrementais ou diferenciais.|
|-usuário|Para o Windows Server 2008 R2 e posterior, especifica o usuário com permissão de gravação para o destino de armazenamento de backup (se for uma pasta compartilhada remota). O usuário precisa ser um membro do grupo de administradores ou do grupo de operadores de backup no computador que está sendo submetido a backup.|
|-password|Para o Windows Server 2008 R2 e posterior, especifica a senha para o nome de usuário fornecido pelo parâmetro-User.|
|-quiet|Executa o subcomando sem prompts para o usuário.|
|-allowDeleteOldBackups|Substitui todos os backups feitos antes da atualização do computador.|

## <a name="remarks"></a>Comentários

Para exibir o valor do identificador de disco para seus discos, digite **Wbadmin Get disks**.

## <a name="examples"></a>Exemplos

Os exemplos a seguir mostram como o comando **Wbadmin enable backup** pode ser usado em diferentes cenários de backup:

Cenário #1
- Agendar backups de unidades de disco rígido e:, d:\mountpoint e \\ \\ ? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
- Salvar os arquivos na DiskId do disco
- Executar backups diariamente às 9:00 A.M. e 18h.
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Cenário #2
- Agendar backups da pasta d:\Documents para o local de rede \\ \\ backupshare\backup1
- Use as credenciais de rede para o aekel (administrador de backup aaren Ekelund), que é membro do domínio CONTOSOEAST para autenticar o acesso ao compartilhamento de rede. A senha do aaren é *$3hM9 ^ 5lp*.
- Executar backups diariamente às 12:00 A.M. e 7:00 P.M.
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  Cenário #3
- Agendar backups do volume t: e da pasta d:\Documents para a unidade h:, mas excluir a pasta d:\Documents \~ tmp
- Execute um backup completo usando o Serviço de Cópias de Sombra de Volume.
- Executar backups diariamente às 1:00 A.M.
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)