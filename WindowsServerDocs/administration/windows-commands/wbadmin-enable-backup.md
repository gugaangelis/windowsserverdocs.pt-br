---
title: wbadmin enable backup
description: Artigo de referência do comando Wbadmin enable backup, que cria e habilita um agendamento de backup diário ou modifica um agendamento de backup existente.
ms.topic: reference
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3135df39135a1d085509bdbdabdfa7f88817c3be
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156068"
---
# <a name="wbadmin-enable-backup"></a>wbadmin enable backup

Cria e habilita um agendamento de backup diário ou modifica um agendamento de backup existente. Sem parâmetros especificados, ele exibe as configurações de backup agendadas no momento.

Para configurar ou modificar um agendamento de backup diário usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** . Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

Para exibir o valor do identificador de disco para seus discos, execute o comando [Wbadmin Get disks](wbadmin-get-disks.md) .

## <a name="syntax"></a>Sintaxe

```
wbadmin enable backup [-addtarget:<BackupTarget>] [-removetarget:<BackupTarget>] [-schedule:<TimeToRunBackup>] [-include:<VolumesToInclude>] [-nonRecurseInclude:<ItemsToInclude>] [-exclude:<ItemsToExclude>] [-nonRecurseExclude:<ItemsToExclude>][-systemState] [-hyperv:<HyperVComponentsToExclude>] [-allCritical] [-systemState] [-vssFull | -vssCopy] [-user:<UserName>] [-password:<Password>] [-allowDeleteOldBackups]  [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -AddTarget | Especifica o local de armazenamento para backups. Exige que você especifique o local como um disco, volume ou caminho UNC (Convenção de nomenclatura universal) para uma pasta compartilhada remota ( `\\<servername>\<sharename>` ). Por padrão, o backup será salvo em: `\\<servername>\<sharename> WindowsImageBackup <ComputerBackedUp>` . Se você especificar um disco, o disco será formatado antes do uso e todos os dados existentes nele serão apagados permanentemente. Se você especificar uma pasta compartilhada, não poderá adicionar mais locais. Você só pode especificar uma pasta compartilhada como um local de armazenamento de cada vez.<p>**Importante:** Se você salvar um backup em uma pasta compartilhada remota, esse backup será substituído se você usar a mesma pasta para fazer backup do mesmo computador novamente. Além disso, se a operação de backup falhar, você poderá terminar sem backup porque o backup mais antigo será substituído, mas o backup mais recente não poderá ser usado. Você pode evitar isso criando subpastas na pasta compartilhada remota para organizar os backups. Se você fizer isso, as subpastas precisarão de duas vezes o espaço da pasta pai.<p>Apenas um local pode ser especificado em um único comando. Vários locais de armazenamento de backup de volume e disco podem ser adicionados executando o comando novamente. |
| -removetarget | Especifica o local de armazenamento que você deseja remover do agendamento de backup existente. Exige que você especifique o local como um identificador de disco. |
| -agenda | Especifica as horas do dia para criar um backup, formatado como HH: MM e delimitado por vírgula. |
| -incluir | Especifica a lista delimitada por vírgulas de itens a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá terminar com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. |
| -nonRecurseInclude | Especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem incluídos no backup. Você pode incluir vários arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá terminar com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. Deve ser usado somente quando o parâmetro **-backupTarget** é usado. |
| -excluir | Especifica a lista delimitada por vírgulas de itens a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá terminar com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. |
| -nonRecurseExclude | Especifica a lista de itens não recursivos, delimitadas por vírgulas, a serem excluídos do backup. Você pode excluir arquivos, pastas ou volumes. Os caminhos de volume podem ser especificados usando letras de drive de volume, pontos de montagem de volume ou nomes de volume com base em GUID. Se você usar um nome de volume baseado em GUID, ele deverá terminar com uma barra invertida ( `\` ). Você pode usar o caractere curinga ( `*` ) no nome do arquivo ao especificar um caminho para um arquivo. |
| -HyperV | Especifica a lista delimitada por vírgulas de componentes a serem incluídos no backup. O identificador pode ser um nome de componente ou GUID de componente (com ou sem chaves). |
| -SystemState | Cria um backup que inclui o estado do sistema, além de quaisquer outros itens que você especificou com o parâmetro **-include** . O estado do sistema contém arquivos de inicialização (Boot.ini, NDTLDR, NTDetect.com), o registro do Windows, incluindo configurações COM, o SYSVOL (diretivas de grupo e scripts de logon), o Active Directory e o NTDS. DIT em controladores de domínio e, se o serviço de certificados estiver instalado, o repositório de certificados. Se o servidor tiver a função de servidor Web instalada, o metadiretório do IIS será incluído. Se o servidor fizer parte de um cluster, Serviço de cluster informações também serão incluídas. |
| -missão crítica | Especifica que todos os volumes críticos (volumes que contêm o estado do sistema operacional) sejam incluídos nos backups. Esse parâmetro será útil se você estiver criando um backup para recuperação completa do sistema ou do estado do sistema. Ele deve ser usado somente quando **-backupTarget** é especificado; caso contrário, o comando falhará. Pode ser usado com a opção **-include** .<p>**Dica:** O volume de destino para um backup de volume crítico pode ser uma unidade local, mas não pode ser qualquer um dos volumes incluídos no backup. |
| -vssFull | Executa um backup completo usando o Serviço de Cópias de Sombra de Volume (VSS). Todos os arquivos são submetidos a backup, o histórico de cada arquivo é atualizado para refletir que foi feito backup e os logs de backups anteriores podem estar truncados. Se esse parâmetro não for usado, o comando [Wbadmin start backup](wbadmin-start-backup.md) fará um backup de cópia, mas o histórico de arquivos cujo backup está sendo feito não será atualizado.<p>**Cuidado:** Não use esse parâmetro se você estiver usando um produto que não seja Backup do Windows Server para fazer backup de aplicativos que estão nos volumes incluídos no backup atual. Isso pode potencialmente dividir o tipo de backup incremental, diferencial ou outro que o outro produto de backup está criando, pois o histórico do qual eles dependem para determinar a quantidade de dados para o backup pode estar ausente e pode executar um backup completo desnecessariamente. |
| -vssCopy | Executa um backup de cópia usando o VSS. Todos os arquivos são submetidos a backup, mas o histórico dos arquivos que estão sendo atualizados não é atualizado, de modo que você preserva todas as informações em quais arquivos foram alterados, excluídos e assim por diante, bem como quaisquer arquivos de log do aplicativo. O uso desse tipo de backup não afeta a sequência de backups incrementais e diferenciais que podem ocorrer independentemente desse backup de cópia. Este é o valor padrão.<p>**AVISO:** Uma cópia de backup não pode ser usada para backups ou restaurações incrementais ou diferenciais. |
| -usuário | Especifica o usuário com permissão de gravação para o destino de armazenamento de backup (se for uma pasta compartilhada remota). O usuário precisa ser um membro do grupo **Administradores** ou **operadores de backup** no computador que está sendo submetido a backup. |
| -password | Especifica a senha para o nome de usuário fornecido pelo parâmetro **-User**. |
| -allowDeleteOldBackups | Substitui todos os backups feitos antes da atualização do computador. |
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="examples"></a>Exemplos

Para agendar backups diários às 9:00 AM e 6:00 para as unidades de disco rígido E:, D:\mountpoint, e `\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\` para salvar os arquivos no disco nomeado, DiskId, digite:

```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:E:,D:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

Para agendar backups diários da pasta D:\Documents às 12:00 AM e 7:00 PM para o local de rede `\\backupshare\backup1` , usando as credenciais de rede para o **operador de backup**, aaren Ekelund (aekel), quem é a senha *$3hM9 ^ 5lp* e quem é membro do domínio CONTOSOEAST, usado para autenticar o acesso ao compartilhamento de rede, digite:

```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: D:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```

Para agendar backups diários do volume T: e a pasta D:\Documents às 1:00 na unidade H:, excluindo a pasta `d:\documents\~tmp` e executando um backup completo usando o serviço de cópias de sombra de volume, digite:

```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin enable backup](wbadmin-enable-backup.md)

- [comando Wbadmin start backup](wbadmin-start-backup.md)

- [comando Wbadmin Get disks](wbadmin-get-disks.md)
