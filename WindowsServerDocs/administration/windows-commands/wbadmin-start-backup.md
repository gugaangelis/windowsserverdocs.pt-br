---
title: Wbadmin iniciar backup
description: Tópico de referência para Wbadmin start backup, que cria um backup usando parâmetros especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e113d8c69e4e7793f70615d071889e9b9666988
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725918"
---
# <a name="wbadmin-start-backup"></a>Wbadmin iniciar backup

Cria um backup usando parâmetros especificados. Se nenhum parâmetro for especificado e você tiver criado um backup diário agendado, esse subcomando criará o backup usando as configurações para o backup agendado. Se os parâmetros forem especificados, ele criará um backup de cópia Serviço de Cópias de Sombra de Volume (VSS) e não atualizará o histórico dos arquivos que estão sendo incluídos no backup.

Para criar um backup único com esse subcomando, você deve ser um membro do grupo operadores de **backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando** e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

Sintaxe para Windows ° Vista e Windows Server 2008:
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
Sintaxe para Windows ° 7 e Windows Server 2008 R2 e posterior:
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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-backupTarget|Especifica o local de armazenamento para este backup. Requer uma letra de unidade de disco rígido (f:), um caminho baseado em GUID de volume no \\ \\formato\\ de? O volume {GUID} ou um caminho UNC (Convenção de nomenclatura universal) para uma pasta compartilhada remota\\\\\<(ServerName>\\ \<ShareName>\\). \\ \\ \<Por padrão, o backup será salvo em: nomedoservidor>\\ \<ShareName>\\ **WindowsImageBackup**\\\<ComputerBackedUp>\\.</br>Importante: se você salvar um backup em uma pasta compartilhada remota, esse backup será substituído se você usar a mesma pasta para fazer backup do mesmo computador novamente. Além disso, se a operação de backup falhar, você poderá acabar sem nenhum backup, pois o backup mais antigo será substituído, mas o backup mais recente não será utilizável. Você pode evitar isso criando subpastas na pasta compartilhada remota para organizar os backups. Se você fizer isso, as subpastas precisarão de duas vezes o espaço como a pasta pai.|
|-incluir|Para o Windows ° Vista e o Windows Server 2008, especifica a lista delimitada por vírgulas de letras de unidade de volume, pontos de montagem de volume ou nomes de volume baseados em GUID a serem incluídos no backup. Esse parâmetro deve ser usado somente quando o parâmetro **-backupTarget** é usado.</br>Para o Windows ° 7 e o Windows Server 2008 R2 e posterior, especifica a lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra\\invertida (). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro **-backupTarget** é usado.|
|-excluir|Para o Windows ° 7 e o Windows Server 2008 R2 e posterior, especifica a lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra\\invertida (). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro **-backupTarget** é usado.|
|-nonRecurseInclude|Para o Windows ° 7 e o Windows Server 2008 R2 e posterior, especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra\\invertida (). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro **-backupTarget** é usado.|
|-nonRecurseExclude|Para o Windows ° 7 e o Windows Server 2008 R2 e posterior, especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra\\invertida (). Você pode usar o caractere curinga (\*) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro **-backupTarget** é usado.|
|-missão crítica|Especifica que todos os volumes críticos (volumes que contêm o estado do sistema operacional) sejam incluídos nos backups. Esse parâmetro será útil se você estiver criando um backup para recuperação bare-metal. Ele deve ser usado somente quando **-backupTarget** for especificado, caso contrário, o comando falhará. Pode ser usado com a opção **-include** .</br>Dica: o volume de destino para um backup de volume crítico pode ser uma unidade local, mas não pode ser qualquer um dos volumes incluídos no backup.|
|-SystemState|Para o Windows ° 7 e o Windows Server 2008 R2 e posterior, o cria um backup que inclui o estado do sistema, além de quaisquer outros itens que você especificou com o parâmetro **-include** . O estado do sistema contém arquivos de inicialização (boot. ini, NDTLDR, NTDetect.com), o registro do Windows, incluindo configurações COM, o SYSVOL (diretivas de grupo e scripts de logon), o Active Directory e o NTDS. DIT em controladores de domínio e, se o serviço de certificados estiver instalado, o repositório de certificados. Se o servidor tiver a função de servidor Web instalada, o metadiretório do IIS será incluído. Se o servidor fizer parte de um cluster, as informações do serviço de cluster também serão incluídas.|
|-noVerify|Especifica que os backups salvos em mídia removível (como um DVD) não são verificados quanto a erros. Se você não usar esse parâmetro, os backups salvos em mídia removível serão verificados quanto a erros.|
|-usuário|Se o backup for salvo em uma pasta compartilhada remota, especifica o nome de usuário com permissão de gravação para a pasta.|
|-password|Especifica a senha para o nome de usuário que é fornecido pelo parâmetro **-User**.|
|-noInheritAcl|Aplica as permissões de ACL (lista de controle de acesso) que correspondem às credenciais fornecidas pelos parâmetros **-User** e **-password** \\ \\ \<a nomedoservidor \\ \<>ShareName \\>\\\<WindowsImageBackup ComputerBackedUp \\>(a pasta que contém o backup). Para acessar o backup mais tarde, você deve usar essas credenciais ou ser um membro do grupo Administradores ou do grupo operadores de backup no computador com a pasta compartilhada. Se **-noInheritAcl** não for usado, as permissões de ACL da pasta compartilhada remota serão aplicadas à \\ \<pasta ComputerBackedUp> por padrão para que qualquer pessoa com acesso à pasta compartilhada remota possa acessar o backup.|
|-vssFull|Executa um backup completo usando o Serviço de Cópias de Sombra de Volume (VSS). Todos os arquivos são submetidos a backup, o histórico de cada arquivo é atualizado para refletir que foi feito backup e os logs de backups anteriores podem estar truncados. Se esse parâmetro não for usado, o **Wbadmin start backup** fará um backup de cópia, mas o histórico de arquivos cujo backup está sendo feito não será atualizado.</br>Cuidado: não use esse parâmetro se você estiver usando um produto que não seja Backup do Windows Server para fazer backup de aplicativos que estão nos volumes incluídos no backup atual. Isso pode potencialmente dividir o tipo de backup incremental, diferencial ou outro que o outro produto de backup está criando, pois o histórico do qual eles dependem para determinar a quantidade de dados para o backup pode estar ausente e pode executar um backup completo desnecessariamente.|
|-vssCopy|Para o Windows 7 e o Windows Server 2008 R2 e posterior, o executa um backup de cópia usando o VSS. Todos os arquivos são submetidos a backup, mas o histórico dos arquivos que estão sendo atualizados não é atualizado, de modo que você preserva todas as informações em quais arquivos foram alterados, excluídos e assim por diante, bem como quaisquer arquivos de log do aplicativo. O uso desse tipo de backup não afeta a sequência de backups incrementais e diferenciais que podem ocorrer independentemente desse backup de cópia. Este é o valor padrão.</br>Aviso: um backup de cópia não pode ser usado para backups ou restaurações incrementais ou diferenciais.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="examples"></a>Exemplos

Os exemplos a seguir mostram como o comando **Wbadmin start backup** pode ser usado em diferentes cenários de backup:

Cenário #1
- Criar um backup de volumes e:, d:\\mountpoint e \\ \\? \\Volume {cc566d14-4410-11d9-9d93-806e6f6e6963}
- Salve o backup no volume f:
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Cenário #2
- Execute um backup único de *f:\\Pasta1* e *h\\: Pasta2* no volume *d:*.
- Fazer backup do estado do sistema
- Faça um backup de cópia para que o backup diferencial normalmente agendado não seja afetado.
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  Cenário #3
- Execute um backup único de *d:\\Pasta1* que deve ser submetido a backup de forma não recursiva.
- Faça backup da pasta no local * \\ \\de rede\\backupshare backup1*
- Restrinja o acesso ao backup para membros do grupo **Administradores** ou **operadores de backup** .
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
