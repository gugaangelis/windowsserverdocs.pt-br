---
title: wbadmin start backup
description: Artigo de referência para o comando Wbadmin start backup, que cria um backup usando parâmetros especificados.
ms.topic: reference
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a99d467aed8df57660f46b5efbcb4b8df5feb215
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524841"
---
# <a name="wbadmin-start-backup"></a>wbadmin start backup

Cria um backup usando parâmetros especificados. Se nenhum parâmetro for especificado e você tiver criado um backup diário agendado, esse comando criará o backup usando as configurações para o backup agendado. Se os parâmetros forem especificados, ele criará um backup de cópia Serviço de Cópias de Sombra de Volume (VSS) e não atualizará o histórico dos arquivos que estão sendo incluídos no backup.

Para criar um backup único usando esse comando, você deve ser um membro do grupo operadores de **backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```com
wbadmin start backup [-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}] [-include:<ItemsToInclude>] [-nonRecurseInclude:<ItemsToInclude>] [-exclude:<ItemsToExclude>] [-nonRecurseExclude:<ItemsToExclude>] [-allCritical] [-systemState] [-noVerify] [-user:<UserName>] [-password:<Password>] [-noInheritAcl] [-vssFull | -vssCopy] [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -backupTarget | Especifica o local de armazenamento para este backup. Requer uma letra de unidade de disco rígido (f:), um caminho baseado em GUID de volume no formato `\\?\Volume{GUID}` ou um caminho UNC (Convenção de nomenclatura universal) para uma pasta compartilhada remota `(\\<servername>\<sharename>\)` . Por padrão, o backup será salvo em: `\\<servername>\<sharename>\WindowsImageBackup\<ComputerBackedUp>\` . |
| -incluir | Especifica a lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. O parâmetro **-include** só deve ser usado em conjunto com o parâmetro **-backupTarget** . |
| -excluir | Especifica a lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. O parâmetro **-Exclude** deve ser usado somente em conjunto com o parâmetro **-backupTarget** . |
| -nonRecurseInclude | Especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. O parâmetro **-nonRecurseInclude** só deve ser usado em conjunto com o parâmetro **-backupTarget** . |
| -nonRecurseExclude | Especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá ser encerrado com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. O parâmetro **-nonRecurseExclude** só deve ser usado em conjunto com o parâmetro **-backupTarget** . |
| -missão crítica | Especifica que todos os volumes críticos (volumes que contêm o estado do sistema operacional) sejam incluídos nos backups. Esse parâmetro será útil se você estiver criando um backup para recuperação bare-metal. Ele deve ser usado somente quando **-backupTarget** for especificado, caso contrário, o comando falhará. Pode ser usado com a opção **-include** .<p>**Dica:** O volume de destino para um backup de volume crítico pode ser uma unidade local, mas não pode ser qualquer um dos volumes incluídos no backup. |
| -SystemState | Cria um backup que inclui o estado do sistema, além de quaisquer outros itens que você especificou com o parâmetro **-include** . O estado do sistema contém arquivos de inicialização (Boot.ini, NDTLDR, NTDetect.com), o registro do Windows, incluindo configurações COM, o SYSVOL (diretivas de grupo e scripts de logon), o Active Directory e o NTDS. DIT em controladores de domínio e, se o serviço de certificados estiver instalado, o repositório de certificados. Se o servidor tiver a função de servidor Web instalada, o metadiretório do IIS será incluído. Se o servidor fizer parte de um cluster, as informações do serviço de cluster também serão incluídas. |
| -noVerify | Especifica que os backups salvos em mídia removível (como um DVD) não são verificados quanto a erros. Se você não usar esse parâmetro, os backups salvos em mídia removível serão verificados quanto a erros. |
| -usuário | Se o backup for salvo em uma pasta compartilhada remota, especifica o nome de usuário com permissão de gravação para a pasta. |
| -password | Especifica a senha para o nome de usuário que é fornecido pelo parâmetro **-User**. |
| -noInheritAcl | Aplica as permissões de ACL (lista de controle de acesso) que correspondem às credenciais fornecidas pelos parâmetros **-User** e **-password** para `\\<servername>\<sharename>\WindowsImageBackup\<ComputerBackedUp>\` (a pasta que contém o backup). Para acessar o backup mais tarde, você deve usar essas credenciais ou ser um membro do grupo Administradores ou do grupo operadores de backup no computador com a pasta compartilhada. Se **-noInheritAcl** não for usado, as permissões de ACL da pasta compartilhada remota serão aplicadas à `\<ComputerBackedUp>` pasta por padrão para que qualquer pessoa com acesso à pasta compartilhada remota possa acessar o backup. |
| -vssFull | Executa um backup completo usando o Serviço de Cópias de Sombra de Volume (VSS). Todos os arquivos são submetidos a backup, o histórico de cada arquivo é atualizado para refletir que foi feito backup e os logs de backups anteriores podem estar truncados. Se esse parâmetro não for usado, o **Wbadmin start backup** fará um backup de cópia, mas o histórico de arquivos cujo backup está sendo feito não será atualizado.<p>**Cuidado:** Não use esse parâmetro se você estiver usando um produto que não seja Backup do Windows Server para fazer backup de aplicativos que estão nos volumes incluídos no backup atual. Isso pode potencialmente dividir o tipo de backup incremental, diferencial ou outro que o outro produto de backup está criando, pois o histórico do qual eles dependem para determinar a quantidade de dados para o backup pode estar ausente e pode executar um backup completo desnecessariamente. |
| -vssCopy | Executa um backup de cópia usando o VSS. Todos os arquivos são submetidos a backup, mas o histórico dos arquivos que estão sendo atualizados não é atualizado, de modo que você preserva todas as informações em quais arquivos foram alterados, excluídos e assim por diante, bem como quaisquer arquivos de log do aplicativo. O uso desse tipo de backup não afeta a sequência de backups incrementais e diferenciais que podem ocorrer independentemente desse backup de cópia. Esse é o valor padrão.<p>**AVISO:** Um backup de cópia não pode ser usado para backups ou restaurações incrementais ou diferenciais. |
| -quiet | Executa o comando sem prompts para o usuário. |

#### <a name="remarks"></a>Comentários

- Se você salvar o backup em uma pasta compartilhada remota e, em seguida, executar outro backup no mesmo computador e na mesma pasta compartilhada remota, você substituirá o backup anterior.

- Se a operação de backup falhar, você poderá terminar sem um backup, pois o backup mais antigo é substituído, mas o backup mais recente não é utilizável. Para evitar isso, é recomendável criar subpastas na pasta compartilhada remota para organizar os backups. No entanto, por causa dessa organização, você deve ter duas vezes o espaço disponível como a pasta pai.

## <a name="examples"></a>Exemplos

Para criar um backup de volumes *e:*, *d: \\ mountpoint*e `\\?\Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}\` para o volume *f:*, digite:

```
wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

Para executar um backup único de *f: \\ Pasta1* e *h: \\ Pasta2* para o volume *d:*, para fazer backup do estado do sistema e fazer um backup de cópia para que o backup diferencial normalmente agendado não seja afetado, digite:

```
wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
```

Para executar um backup único e não recursivo de *d: \\ Pasta1* no `\\backupshare\backup1*` local de rede e restringir o acesso a membros do grupo **Administradores** ou **operadores de backup** , digite:

```
wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)
