---
title: dfsrmig
description: Artigo de referência para o comando DFSRMIG, que migra a replicação do SYSvol do FRS para o Replicação do DFS, fornece informações sobre o progresso da migração e modifica AD DS objetos para dar suporte à migração.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87882ebe0beb687f704c5573091f56c067c278ee
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928637"
---
# <a name="dfsrmig"></a>dfsrmig

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A ferramenta de migração para o serviço de Replicação do DFS, dfsrmig.exe, é instalada com o serviço de Replicação do DFS. Essa ferramenta migra a replicação do SYSvol do serviço de replicação de arquivos (FRS) para replicação do Sistema de Arquivos Distribuído (DFS). Ele também fornece informações sobre o progresso da migração e modifica os objetos Active Directory Domain Services (AD DS) para dar suporte à migração.

## <a name="syntax"></a>Sintaxe

```
dfsrmig [/setglobalstate <state> | /getglobalstate | /getmigrationstate | /createglobalobjects |
/deleterontfrsmember [<read_only_domain_controller_name>] | /deleterodfsrmember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `/setglobalstate <state>` | Define o estado de migração global do domínio como um que corresponde ao valor especificado por *estado*. Você só pode definir o estado de migração global para um estado estável. Os valores de *estado* incluem:<ul><li>**0** -estado de início</li><li>**1** -estado preparado</li><li>**2** -estado Redirecionado</li><li>**3** -elimina o estado</li></ul> |
| /getglobalstate | Recupera o estado de migração global atual para o domínio da cópia local do banco de dados AD DS, quando executado no emulador de PDC. Use esta opção para confirmar que você definiu o estado de migração global correto.<p>**Importante:** Você só deve executar este comando no emulador de PDC. |
| /getmigrationstate | Recupera o estado atual de migração local para todos os controladores de domínio no domínio e determina se os Estados locais correspondem ao estado de migração global atual. Use esta opção para determinar se todos os controladores de domínio atingiram o estado de migração global. |
| /createglobalobjects | Cria os objetos globais e as configurações em AD DS usados pelo Replicação do DFS usa. As únicas situações em que você deve usar essa opção para criar objetos e configurações manualmente são:<ul><li>**Um novo controlador de domínio somente leitura é promovido durante a migração**. Se um novo controlador de domínio somente leitura for promovido no domínio depois de passar para o estado **preparado** , mas antes da migração para o estado **eliminado** , os objetos que corresponderem ao novo controlador de domínio não serão criados, causando a falha da replicação e da migração.</li><li>**As configurações globais para o serviço de replicação do DFS estão ausentes ou foram excluídas**. Se essas configurações estiverem ausentes para um controlador de domínio, a migração do estado de **início** para o estado **preparado** será interrompida no estado de transição de **preparação** . **Observação:** Como as configurações de AD DS global para o serviço de Replicação do DFS para um controlador de domínio somente leitura são criadas no emulador de PDC, essas configurações precisam ser replicadas para o controlador de domínio somente leitura do emulador de PDC antes que o serviço de Replicação do DFS no controlador de domínio somente leitura possa usar essas configurações. Devido a latências de replicação Active Directory, essa replicação pode levar algum tempo para ocorrer. |
| `/deleterontfrsmember [<read_only_domain_controller_name>]` | Exclui as configurações de AD DS global para replicação FRS que correspondem ao controlador de domínio somente leitura especificado ou exclui as configurações de AD DS globais para replicação FRS para todos os controladores de domínio somente leitura se nenhum valor for especificado para `<read_only_domain_controller_name>` .<p>Você não precisará usar essa opção durante um processo de migração normal, pois o serviço de Replicação do DFS exclui automaticamente essas configurações de AD DS durante a migração do estado **Redirecionado** para o estado **eliminado** . Use esta opção para excluir manualmente as configurações de AD DS somente quando a exclusão automática falhar em um controlador de domínio somente leitura e parar o controlador de domínio somente leitura para um IME longo durante a migração do estado **Redirecionado** para o estado **eliminado** . |
| `/deleterodfsrmember [<read_only_domain_controller_name>]` | Exclui as configurações de AD DS globais para Replicação do DFS que correspondem ao controlador de domínio somente leitura especificado ou exclui as configurações de AD DS globais para Replicação do DFS para todos os controladores de domínio somente leitura se nenhum valor for especificado para `<read_only_domain_controller_name>` .<p>Use esta opção para excluir manualmente as configurações de AD DS somente quando a exclusão automática falhar em um controlador de domínio somente leitura e parar o controlador de domínio somente leitura por um longo período ao reverter a migração do estado preparado para o estado de início. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Use o `/setglobalstate <state>` comando para definir o estado de migração global em AD DS no emulador de PDC para iniciar e controlar o processo de migração. Se o emulador de PDC não estiver disponível, esse comando falhará.

- A migração para o estado **eliminado** é irreversível e a reversão não é possível. portanto, use um valor de **3** para o *estado* somente quando estiver totalmente comprometido em usar replicação do DFS para replicação de SYSvol.

- Os Estados de migração global devem ser um estado de migração estável.

- Active Directory replicação Replica o estado global para outros controladores de domínio no domínio, mas devido a latências de replicação, você poderá obter inconsistências se executar `dfsrmig /getglobalstate` em um controlador de domínio diferente do emulador PDC.

- A saída de `dsfrmig /getmigrationstate` indica se a migração para o estado global atual está concluída, listando o estado de migração local para todos os controladores de domínio que ainda não atingiram o estado de migração global atual. O estado de migração local para controladores de domínio também pode incluir Estados de transição para controladores de domínio que não atingiram o estado de migração global atual.

- Controladores de domínio somente leitura não podem excluir configurações de AD DS, o emulador de PDC executa essa operação e as alterações eventualmente replicam para os controladores de domínio somente leitura após as latências aplicáveis para a replicação do Active Directory.

- O comando **DFSRMIG** só tem suporte em controladores de domínio executados no nível funcional de domínio do Windows Server, pois a migração SysVol do FRS para o replicação do DFS só é possível em controladores de domínio que operam nesse nível.

- Você pode executar o comando **DFSRMIG** em qualquer controlador de domínio, mas as operações que criam ou manipulam AD DS objetos só são permitidas em controladores de domínio com capacidade de leitura/gravação (não em controladores de domínio somente leitura).

## <a name="examples"></a>Exemplos

Para definir o estado de migração global como preparado (**1**) e iniciar a migração ou para reverter a partir do estado preparado, digite:

```
dfsrmig /setglobalstate 1
```

Para definir o estado de migração global como iniciar (**0**) e iniciar a reversão para o estado de início, digite:

```
dfsrmig /setglobalstate 0
```

Para exibir o estado de migração global, digite:

```
dfsrmig /getglobalstate
```

Saída do `dfsrmig /getglobalstate` comando:

```
Current DFSR global state: Prepared
Succeeded.
```

Para exibir informações sobre se os Estados de migração local em todos os controladores de domínio correspondem ao estado de migração global e se existem Estados de migração local em que o estado local não corresponde ao estado global, digite:

```
dfsrmig /GetMigrationState
```

Saída do `dfsrmig /getmigrationstate` comando quando os Estados de migração local em todos os controladores de domínio corresponderem ao estado de migração global:

```
All Domain Controllers have migrated successfully to Global state (Prepared).
Migration has reached a consistent state on all Domain Controllers.
Succeeded.
```

Saída do `dfsrmig /getmigrationstate` comando quando os Estados de migração local em alguns controladores de domínio não correspondem ao estado de migração global.

```
The following Domain Controllers are not in sync with Global state (Prepared):
Domain Controller (Local Migration State) DC type
=========
CONTOSO-DC2 (start) ReadOnly DC
CONTOSO-DC3 (Preparing) Writable DC
Migration has not yet reached a consistent state on all domain controllers
State information might be stale due to AD latency.
```

Para criar os objetos globais e as configurações que o Replicação do DFS usa em AD DS controladores de domínio em que essas configurações não foram criadas automaticamente durante a migração ou onde essas configurações estão ausentes, digite:

```
dfsrmig /createglobalobjects
```

Para excluir as configurações de AD DS global para replicação do FRS para um controlador de domínio somente leitura chamado contoso-DC2 se essas configurações não foram excluídas automaticamente pelo processo de migração, digite:

```
dfsrmig /deleterontfrsmember contoso-dc2
```

Para excluir as configurações de AD DS global para replicação do FRS para todos os controladores de domínio somente leitura se essas configurações não forem excluídas automaticamente pelo processo de migração, digite:

```
dfsrmig /deleterontfrsmember
```

Para excluir as configurações globais de AD DS para Replicação do DFS para um controlador de domínio somente leitura chamado contoso-DC2 se essas configurações não foram excluídas automaticamente pelo processo de migração, digite:

```
dfsrmig /deleterodfsrmember contoso-dc2
```

Para excluir as configurações globais de AD DS para Replicação do DFS para todos os controladores de domínio somente leitura se essas configurações não forem excluídas automaticamente pelo processo de migração, digite:

```
dfsrmig /deleterodfsrmember
```

Para exibir a ajuda no prompt de comando:

```
dfsrmig
```

```
dfsrmig /?
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Série de migração do SYSvol: parte 2 dfsrmig.exe: a ferramenta de migração do SYSvol](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

- [Active Directory Domain Services](../../identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview.md)
