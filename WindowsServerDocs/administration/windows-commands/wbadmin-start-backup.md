---
title: WBADMIN Iniciar backup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09b2ffabcea414dd4717a2ffa1f6e860a17f3653
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871697"
---
# <a name="wbadmin-start-backup"></a>WBADMIN Iniciar backup



Cria um backup usando os parâmetros especificados. Se nenhum parâmetro for especificado e você tiver criado um backup diário agendado, este subcomando cria o backup usando as configurações para o backup agendado. Se o parâmetro for especificado, ele cria um backup de cópia do Volume Shadow Copy Service (VSS) e não atualizará o histórico dos arquivos que está sendo feito backup.

Para criar um backup único com esse subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando** e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

Sintaxe do Windows ° Vista e Windows Server 2008:
```
wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<VolumesToInclude>]
[-allCritical]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noinheritAcl]
[-vssFull]
[-quiet]
```
Sintaxe de ° do Windows 7 e Windows Server 2008 R2 e posterior:
```
Wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<ItemsToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>]
[-allCritical]
[-systemState]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noInheritAcl]
[-vssFull | -vssCopy] 
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-backupTarget|Especifica o local de armazenamento para esse backup. Requer uma letra de unidade de disco rígido (f), um caminho de volume baseada em GUID no formato \\ \\? \Volume{GUID}, ou um caminho de convenção de nomenclatura Universal (UNC) para uma pasta compartilhada remota (\\\\\<servername > \<sharename >\). Por padrão, o backup será salvo em: \\ \\ <servername> \<sharename >\** WindowsImageBackup * *\\<ComputerBackedUp>\.</br>Importante: Se você salvar um backup em uma pasta compartilhada remota, esse backup será substituído se você usar a mesma pasta para fazer backup do computador mesmo novamente. Além disso, se a operação de backup falhar, você pode acabar com nenhum backup porque o backup mais antigo será substituído, mas o backup mais recente não será utilizável. Você pode evitar isso, criando subpastas na pasta compartilhada remota para organizar seus backups. Se você fizer isso, as subpastas precisará de duas vezes o espaço como a pasta pai.|
|-incluem|Para o Windows ° Vista e Windows Server 2008, especifica a lista delimitada por vírgulas de letras de unidade do volume, pontos de montagem de volume ou nomes de volume baseada em GUID para incluir no backup. Esse parâmetro deve ser usado somente quando o **- backupTarget** parâmetro é usado.</br>Para o Windows ° 7 e Windows Server 2008 R2 e versões posteriores, especifica a lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\\). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o **- backupTarget** parâmetro é usado.|
|-exclude|Para o Windows ° 7 e Windows Server 2008 R2 e versões posteriores, especifica a lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\\). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o **- backupTarget** parâmetro é usado.|
|-nonRecurseInclude|Para o Windows ° 7 e Windows Server 2008 R2 e versões posteriores, especifica a não-recursivo, uma lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\\). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o **- backupTarget** parâmetro é usado.|
|-nonRecurseExclude|Para o Windows ° 7 e Windows Server 2008 R2 e versões posteriores, especifica a não-recursivo, uma lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseada em GUID, ele deverá terminar com uma barra invertida (\\). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o **- backupTarget** parâmetro é usado.|
|-allCritical|Especifica que todos os volumes críticos (volumes que contêm o estado do sistema operacional) sejam incluídos nos backups. Esse parâmetro é útil se você estiver criando um backup para recuperação bare-metal. Ele deve ser usado somente quando **- backupTarget** é especificado, caso contrário, o comando falhará. Pode ser usado com o **-inclui** opção.</br>Dica: O volume de destino para um backup do volume crítico pode ser uma unidade local, mas ele não pode ser qualquer um dos volumes que estão incluídos no backup.|
|-systemState|Para o Windows ° 7 e Windows Server 2008 R2 e versões posteriores, cria um backup que inclui o estado do sistema, além de outros itens que você especificou com o **-incluem** parâmetro. O estado do sistema contém arquivos de inicialização (Boot. ini, NDTLDR, NTDetect.com), o registro de Windows, incluindo configurações de COM, o SYSVOL (diretivas de grupo e Scripts de Logon), o Active Directory e o NTDS. DIT nos controladores de domínio e, se o serviço de certificados estiver instalado, o certificado de Store. Se o servidor tem a função de servidor Web instalada, o IIS metadiretório serão incluído. Se o servidor for parte de um cluster, informações de serviço de Cluster também serão incluídas.|
|-noVerify|Especifica que os backups salvos em mídia removível (como um DVD) não são verificados para erros. Se você não usar esse parâmetro, backups salvos em mídia removível são verificados quanto a erros.|
|-usuário|Se o backup é salvo em uma pasta compartilhada remota, especifica o nome de usuário com permissão de gravação para a pasta.|
|-senha|Especifica a senha para o nome de usuário que é fornecido pelo parâmetro **-usuário**.|
|-noInheritAcl|Aplica-se as permissões de ACL (lista) de controle de acesso que correspondem às credenciais fornecidas pelo **-usuário** e **-senha** parâmetros a serem \\ \\ \< ServerName >\<sharename > \WindowsImageBackup\<ComputerBackedUp > \ (a pasta que contém o backup). Para acessar o backup mais tarde, você deve usar essas credenciais ou ser um membro do grupo Administradores ou do grupo de operadores de Backup no computador com a pasta compartilhada. Se **- noInheritAcl** não é usado, as permissões de ACL de pasta compartilhada remota são aplicadas para o \<ComputerBackedUp > pasta por padrão, para que qualquer pessoa com acesso à pasta compartilhada remota possa acessar o backup.|
|-vssFull|Executa um backup completo usando o serviço de cópia de sombra de Volume (VSS). Todos os arquivos de backup, o histórico de cada arquivo é atualizado para refletir o que foi feito o backup e os logs dos backups anteriores podem ser truncados. Se esse parâmetro não for usado **wbadmin Iniciar backup** faz um backup de cópia, mas o histórico de arquivos está sendo feito não é atualizada.</br>Cuidado: Não use esse parâmetro se você estiver usando um produto diferente de Backup do Windows Server para fazer backup de aplicativos que estão em volumes incluídos no backup atual. Fazer assim pode potencialmente quebrar o incremental, diferencial ou outro tipo de backups é a criação de outro produto de backup porque o histórico do que eles estão contando para determinar a quantidade de dados para backup pode estar ausente e eles podem realizar uma completa de backup desnecessariamente.|
|-vssCopy|Para o Windows 7 e Windows Server 2008 R2 e versões posteriores, executa um backup de cópia usando o VSS. Todos os arquivos de backup, mas o histórico dos arquivos que estão sendo fazer backup não será atualizado para que você preserve todas as informações sobre quais arquivos onde for alterado, excluído e assim por diante, bem como arquivos de log de aplicativo. Usar esse tipo de backup não afeta a sequência de backups incrementais e diferenciais que possam ocorrer independentemente desse backup de cópia. Este é o valor padrão.</br>Aviso: Um backup de cópia não pode ser usado para backups incrementais ou diferenciais ou restaurações.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="BKMK_examples"></a>Exemplos

Os exemplos a seguir mostram como o **wbadmin Iniciar backup** comando pode ser usado em cenários de backup diferentes:

Cenário 1 #
-   Criar um backup de volumes e:, d:\mountpoint, e \\ \\? \Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}
-   Salvar o backup em f: de volume
```
wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Cenário 2 #
-   Executar um backup único de *f:\folder1* e *h:\folder2* volume *unidade d:*.
-   Fazer backup do estado do sistema
-   Faça um backup de cópia para que o backup diferencial normalmente agendado não é afetado.
```
wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
```
Cenário 3 #
-   Executar um backup único de *d:\folder1* que deve ser feito backup não recursiva.
-   Fazer backup da pasta para o local de rede  *\\ \\backupshare\backup1*
-   Restringir o acesso para o backup para os membros de **administradores** ou **operadores de Backup** grupo.
```
wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
