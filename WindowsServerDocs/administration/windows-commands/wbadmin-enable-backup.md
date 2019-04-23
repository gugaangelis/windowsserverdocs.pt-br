---
title: Wbadmin enable backup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fd0bea5da83ca9351d5ea1028c94392bdb40422
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845537"
---
# <a name="wbadmin-enable-backup"></a>Wbadmin enable backup



Cria e ativa um agendamento de backup diário ou modifica uma agenda de backup existente. Sem parâmetros especificados, ele exibe as configurações de backup agendadas no momento.

Para configurar ou modificar uma agenda de backup diária, você deve ser um membro de qualquer um de **administradores** ou **operadores de Backup** grupo. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando** e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

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
Sintaxe do Windows Server 2012 e Windows Server 2012 R2:
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

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-addtarget|Para Windows Server 2008, especifica o local de armazenamento para backups. Você deve especificar um destino para backups como um identificador de disco (consulte comentários). O disco está formatado antes do uso e quaisquer dados existentes nele são apagados permanentemente.</br>Para o Windows Server 2008 R2 e posterior, especifica o local de armazenamento para backups. Exige que você especifique o local como um disco, o volume ou o caminho de convenção de nomenclatura Universal (UNC) para uma pasta compartilhada remota (\\\\\<servername >\<sharename >\). Por padrão, o backup será salvo em: \\ \\ <servername> \<sharename > \WindowsImageBackup\<ComputerBackedUp >\. Se você especificar um disco, o disco será formatado antes do uso e quaisquer dados existentes nele são apagados permanentemente. Se você especificar uma pasta compartilhada, você não pode adicionar mais locais. Você só pode especificar uma pasta compartilhada como local de armazenamento por vez.</br>Importante: Se você salvar um backup em uma pasta compartilhada remota, esse backup será substituído se você usar a mesma pasta para fazer backup do computador mesmo novamente. Além disso, se a operação de backup falhar, você pode acabar com nenhum backup porque o backup mais antigo será substituído, mas o backup mais recente não será utilizável. Você pode evitar isso, criando subpastas na pasta compartilhada remota para organizar seus backups. Se você fizer isso, as subpastas precisará de duas vezes o espaço da pasta pai.</br>Apenas um único local pode ser especificado em um único comando. Vários locais de armazenamento de backup de disco e volume podem ser adicionados ao executar o comando novamente.|
|-removetarget|Especifica o local de armazenamento que você deseja remover da agenda de backup existente. Você deve especificar o local como um identificador de disco (consulte comentários).|
|-agenda|Especifica as horas do dia para criar um backup, formatado como hh: mm e delimitado por vírgula.|
|-incluem|Para Windows Server 2008, especifica a lista delimitada por vírgulas de letras de unidade do volume, pontos de montagem de volume ou nomes de volume baseada em GUID para incluir no backup.</br>Para Windows Server 2008 R2and mais tarde, especifica a lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\). Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo.|
|-nonRecurseInclude|Para o Windows Server 2008 R2 e posterior, especifica a não-recursivo, uma lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\). Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro - backupTarget é usado.|
|-exclude|Para o Windows Server 2008 R2 e posterior, especifica a lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\). Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo.|
|-nonRecurseExclude|Para o Windows Server 2008 R2 e posterior, especifica a não-recursivo, uma lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\). Você pode usar o caractere curinga (*) no nome do arquivo ao especificar um caminho para um arquivo.|
|-hyperv|Especifica a lista delimitada por vírgulas dos componentes a serem incluídos no backup. O identificador pode ser um nome do componente ou o GUID do componente (com ou sem as chaves).|
|-systemState|Para o Windows ° 7 e Windows Server 2008 R2 e versões posteriores, cria um backup que inclui o estado do sistema, além de outros itens que você especificou com o **-incluem** parâmetro. O estado do sistema contém arquivos de inicialização (Boot. ini, NDTLDR, NTDetect.com), o registro de Windows, incluindo configurações de COM, o SYSVOL (diretivas de grupo e Scripts de Logon), o Active Directory e o NTDS. DIT nos controladores de domínio e, se o serviço de certificados estiver instalado, o certificado de Store. Se o servidor tem a função de servidor Web instalada, o IIS metadiretório serão incluído. Se o servidor for parte de um cluster, informações de serviço de Cluster também estão incluídas.|
|-allCritical|Especifica que todos os volumes críticos (volumes que contêm o estado do sistema operacional) sejam incluídos nos backups. Esse parâmetro é útil se você estiver criando um backup completo do sistema ou recuperação de estado do sistema. Ele deve ser usado apenas quando - backupTarget é especificado, caso contrário, o comando falhará. Pode ser usado com o **-inclui** opção.</br>Dica: O volume de destino para um backup do volume crítico pode ser uma unidade local, mas ele não pode ser qualquer um dos volumes que estão incluídos no backup.|
|-vssFull|Para o Windows Server 2008 R2 e posterior, realiza uma completa de fazer backup usando o Volume Shadow Copy Service (VSS). Todos os arquivos de backup, o histórico de cada arquivo é atualizado para refletir o que foi feito o backup e os logs dos backups anteriores podem ser truncados. Se esse parâmetro não for usado wbadmin Iniciar backup faz backup de uma cópia, mas o histórico de arquivos está sendo feito não é atualizado.</br>Cuidado: Não use esse parâmetro se você estiver usando um produto diferente de Backup do Windows Server para fazer backup de aplicativos que estão em volumes incluídos no backup atual. Fazer assim pode potencialmente quebrar o incremental, diferencial ou outro tipo de backups é a criação de outro produto de backup porque o histórico do que eles estão contando para determinar a quantidade de dados para backup pode estar ausente e eles podem realizar uma completa de backup desnecessariamente.|
|-vssCopy|Para o Windows Server 2008 R2 e posterior, executa um backup de cópia usando o VSS. Todos os arquivos de backup, mas o histórico dos arquivos que estão sendo fazer backup não será atualizado para que você preserve todas as informações sobre quais arquivos onde for alterado, excluído e assim por diante, bem como arquivos de log de aplicativo. Usar esse tipo de backup não afeta a sequência de backups incrementais e diferenciais que possam ocorrer independentemente desse backup de cópia. Este é o valor padrão.</br>Aviso: Um backup de cópia não pode ser usado para backups incrementais ou diferenciais ou restaurações.|
|-usuário|Para o Windows Server 2008 R2 e posterior, especifica o usuário com permissão de gravação para o destino de armazenamento de backup (se for uma pasta compartilhada remota). O usuário precisa ser um membro do grupo Administradores ou do grupo Operadores de Backup no computador que está sendo feito.|
|-senha|Para o Windows Server 2008 R2 e posterior, especifica a senha para o nome de usuário fornecido pelo parâmetro-usuário.|
|-quiet|Executa o subcomando sem prompts para o usuário.|
|-allowDeleteOldBackups|Substitui quaisquer backups feitos antes que o computador foi atualizado.|

## <a name="remarks"></a>Comentários

Para exibir o valor do identificador de disco para seus discos, digite **wbadmin obter discos**.

## <a name="BKMK_examples"></a>Exemplos

Os exemplos a seguir mostram como o **wbadmin habilitar backup** comando pode ser usado em cenários de backup diferentes:

Cenário 1 #
-   Agendar backups dos discos rígidos e:, d:\mountpoint, e \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
-   Salve os arquivos no disco DiskID
-   Executar backups diariamente às 9h00 e às 18h
```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Cenário 2 #
-   Agendar backups de d:\documents a pasta para o local de rede \\ \\backupshare\backup1
-   Use as credenciais de rede para o administrador de backup Aaren Ekelund (aekel), que é um membro do domínio CONTOSOEAST para autenticar o acesso ao compartilhamento de rede. Senha do Aaren *US $3 hM 9 ^ 5lp*.
-   Executar backups diariamente à meia-noite e 7:00.
```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```
Cenário 3 #
-   Agendar backups de volume a d:\documents t: e a pasta para a unidade de disco h:, mas exclua a pasta d:\documents\~tmp
-   Execute um backup completo usando o serviço de cópias de sombra de Volume.
-   Executar backups diariamente à 1h00
```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)