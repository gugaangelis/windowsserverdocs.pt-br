---
title: dfsrmig
description: Tópico de referência para DFSRMIG, que migra a replicação de SYSvol do FRS (serviço de replicação de arquivo) para a replicação Sistema de Arquivos Distribuído (DFS), fornece informações sobre o progresso da migração e modifica objetos do AD DS (serviços de domínio Active Directory) para dar suporte à migração.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1b6a464-6a93-4e66-9969-04f175226d8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a5cac54a1ad96994059f3131b81702136b26aad
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719532"
---
# <a name="dfsrmig"></a>dfsrmig

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Migra a replicação do SYSvol do serviço de replicação de arquivo (FRS) para a replicação do Sistema de Arquivos Distribuído (DFS), fornece informações sobre o progresso da migração e modifica os objetos do AD DS (serviços de domínio Active Directory) para dar suporte à migração.



## <a name="syntax"></a>Sintaxe

```
dfsrmig [/SetGlobalState <state> | /GetGlobalState | /GetMigrationState | /createGlobalObjects | 
/deleteRoNtfrsMember [<read_only_domain_controller_name>] | /deleteRoDfsrMember [<read_only_domain_controller_name>] | /?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição | | -----_ | ------ | | /SetGlobalState <state> | Define o estado de migração global desejado para o domínio como o estado que corresponde ao valor especificado por *estado*.<p>Para prosseguir com os processos de migração ou de reversão, use esse comando para percorrer os Estados válidos. Essa opção permite que você inicie e controle o processo de migração definindo o estado de migração global em AD DS no emulador de PDC. Se o emulador de PDC não estiver disponível, esse comando falhará.<p>Você só pode definir o estado de migração global para um estado estável. Os valores válidos para *State*, portanto, são **0** para o estado de início, **1** para o estado preparado, **2** para o estado Redirecionado e **3** para o estado eliminado.<p>A migração para o estado eliminado é irreversível e a reversão desse Estado não é possível, portanto, use um valor de **3** para o *estado* somente quando você estiver totalmente comprometido a usar o replicação do DFS para replicação de SYSvol. | | /GetGlobalState | Recupera o estado de migração global atual para o domínio da cópia local do banco de dados AD DS, quando executado no emulador de PDC.<p>Use esta opção para confirmar que você definiu o estado de migração global correto. Somente os Estados de migração estáveis podem ser Estados de migração global, portanto, os resultados que o comando **DFSRMIG** relata com a opção **/GetGlobalState** correspondem aos Estados que você pode definir com a opção **/SetGlobalState** .<p>Você deve executar o comando **DFSRMIG** com a opção **/GetGlobalState** somente no emulador do PDC. a replicação do Active Directory Replica o estado global para os outros controladores de domínio no domínio, mas as latências de replicação podem causar inconsistências se você executar o comando **DFSRMIG** com a opção **/GetGlobalState** em um controlador de domínio diferente do emulador PDC. Para verificar o status de migração local de um controlador de domínio diferente do emulador de PDC, use a opção **/GetMigrationState** em vez disso. | | /GetMigrationState | Recupera o estado atual de migração local para todos os controladores de domínio no domínio e determina se os Estados locais correspondem ao estado de migração global atual.<p>Use esta opção para determinar se todos os controladores de domínio atingiram o estado de migração global. A saída do comando **dsfrmig** quando você usa a opção **/GetMigrationState** indica se a migração para o estado global atual está concluída e lista o estado de migração local para todos os controladores de domínio que não alcançaram o estado de migração global atual. O estado de migração local para controladores de domínio pode incluir Estados de transição para controladores de domínio que não atingiram o estado de migração global atual. | | /createGlobalObjects | Cria os objetos globais e as configurações em AD DS que Replicação do DFS usa.<p>Você não deve usar essa opção durante um processo de migração normal, pois o serviço de Replicação do DFS cria automaticamente essas AD DS objetos e configurações durante a migração do estado de início para o estado preparado. Use esta opção para criar manualmente esses objetos e configurações nas seguintes situações:<p>-  **Um novo controlador de domínio somente leitura é promovido durante a migração**. O serviço de Replicação do DFS cria automaticamente os objetos de AD DS e as configurações para Replicação do DFS durante a migração do estado de início para o estado preparado. Se um novo controlador de domínio somente leitura for promovido no domínio após essa transição, mas antes da migração para o estado eliminado, os objetos que corresponderem ao controlador de domínio somente leitura recentemente ativado não serão criados no AD DS causando a falha da replicação e da migração.<br />-Nesse caso, você pode executar o comando **DFSRMIG** associadas a opção **/createGlobalObjects** para criar manualmente os objetos em qualquer controlador de domínio somente leitura que ainda não os tenha. A execução desse comando não afeta os controladores de domínio que já têm os objetos e as configurações do serviço de Replicação do DFS.<p>- **As configurações globais para o serviço de replicação do DFS estão ausentes ou foram excluídas**. Se essas configurações estiverem ausentes para um controlador de domínio específico, a migração do estado de início para o estado preparado parará no estado de transição de preparação para o controlador de domínio. Nesse caso, você pode usar o comando **DFSRMIG** com a opção **/createGlobalObjects** para criar manualmente as configurações. **Observação:** Como as configurações de AD DS global para o serviço de Replicação do DFS para um controlador de domínio somente leitura são criadas no emulador de PDC, essas configurações precisam ser replicadas para o controlador de domínio somente leitura do emulador de PDC antes que o serviço de Replicação do DFS no controlador de domínio somente leitura possa usar essas configurações. Devido a latências de replicação diretório ativas, essa replicação pode levar algum tempo para ocorrer. | | /deleteRoNtfrsMember [<read_only_domain_controller_name>] | Exclui as configurações de AD DS global para replicação FRS que correspondem ao controlador de domínio somente leitura especificado ou exclui as configurações de AD DS globais para replicação FRS para todos os controladores de domínio somente leitura se nenhum valor for especificado para *read_only_domain_controller_name*.<p>Você não precisa usar essa opção durante um processo de migração normal, pois o serviço de Replicação do DFS exclui automaticamente essas configurações de AD DS durante a migração do estado Redirecionado para o estado eliminado. Como os controladores de domínio somente leitura não podem excluir essas configurações de AD DS, o emulador de PDC executa essa operação e as alterações eventualmente replicam para os controladores de domínio somente leitura após as latências aplicáveis para a replicação do Active Directory.<p>Use essa opção para excluir manualmente as configurações de AD DS somente quando a exclusão automática falhar em um controlador de domínio somente leitura e parar o controlador de domínio somente leitura para um IME longo durante a migração do estado Redirecionado para o estado eliminado. | | /deleteRoDfsrMember [<read_only_domain_controller_name>] | Exclui as configurações de AD DS global para Replicação do DFS que correspondem ao controlador de domínio somente leitura especificado ou exclui as configurações de AD DS globais para Replicação do DFS para todos os controladores de domínio somente leitura se nenhum valor for especificado para *read_only_domain_controller_name*.<p>Use esta opção para excluir manualmente as configurações de AD DS somente quando a exclusão automática falhar em um controlador de domínio somente leitura e parar o controlador de domínio somente leitura por um longo período ao reverter a migração do estado preparado para o estado de início. | | /? | Exibe a ajuda no prompt de comando. Equivalente a executar **DFSRMIG** sem nenhuma opção. |

## <a name="remarks"></a>Comentários
- o Dfsrmig. exe, a ferramenta de migração para o serviço de Replicação do DFS, é instalado com o serviço Replicação do DFS.
 para um novo servidor do Windows Server 2008, o Dcpromo. exe instala e inicia o serviço de Replicação do DFS ao promover o computador a um controlador de domínio. Quando você atualiza um servidor do Windows Server 2003 para o Windows Server 2008, o processo de atualização instala e inicia o serviço de Replicação do DFS. Você não precisa instalar o serviço de função Replicação do DFS para que o serviço Replicação do DFS seja instalado e iniciado.
- A ferramenta **DFSRMIG** tem suporte apenas em controladores de domínio que são executados no nível funcional de domínio do Windows Server 2008, pois a migração SysVol do FRS para o replicação do DFS só é possível em controladores de domínio que operam no nível funcional de domínio do Windows Server 2008.
- Você pode executar o comando **DFSRMIG** em qualquer controlador de domínio, mas as operações que criam ou manipulam AD DS objetos só são permitidas em controladores de domínio com capacidade de leitura/gravação (não em controladores de domínio somente leitura).
- A execução de **DFSRMIG** sem nenhuma opção exibe a ajuda no prompt de comando.

## <a name="examples"></a>Exemplos
Para definir o estado de migração global como preparado (**1**) e iniciar a migração para ou reversão do estado preparado, digite:
 ```
 dfsrmig /SetGlobalState 1
 ```
 Para definir o estado de migração global como iniciar (**0**) e iniciar a reversão para o estado de início, digite:
 ```
 dfsrmig /SetGlobalState 0
 ```
 Para exibir o estado de migração global, digite:
 ```
 dfsrmig /GetGlobalState
 ```
 Este exemplo mostra a saída típica do comando **DFSRMIG/GetGlobalState** .
 ```
 Current DFSR global state: Prepared 
 Succeeded.
 ```
 Para exibir as informações sobre se os Estados de migração local em todos os controladores de domínio correspondem ao estado de migração global e aos Estados de migração local para todos os controladores de domínio em que o estado local não corresponde ao estado global, digite:
 ```
 dfsrmig /GetMigrationState
 ```
 Este exemplo mostra a saída típica do comando **DFSRMIG/GetMigrationState** quando os Estados de migração local em todos os controladores de domínio correspondem ao estado de migração global.
 ```
 All Domain Controllers have migrated successfully to Global state ( Prepared ).
 Migration has reached a consistent state on all Domain Controllers.
 Succeeded.
 ```
 Este exemplo mostra a saída típica do comando **DFSRMIG/GetMigrationState** quando os Estados de migração local em alguns controladores de domínio não correspondem ao estado de migração global.
 ```
  The following Domain Controllers are not in sync with Global state ( Prepared ):
  Domain Controller (Local Migration State)  DC type
  =========
  CONTOSO-DC2 ( start )  ReadOnly DC
  CONTOSO-DC3 ( Preparing )  Writable DC
  Migration has not yet reached a consistent state on all domain controllers
  State information might be stale due to AD latency.
 ```
Para criar os objetos globais e as configurações que o Replicação do DFS usa em AD DS controladores de domínio em que essas configurações não foram criadas automaticamente durante a migração ou onde essas configurações estão ausentes, digite:
```
dfsrmig /createGlobalObjects
```
Para excluir as configurações de AD DS global para replicação do FRS para um controlador de domínio somente leitura chamado contoso-DC2 se essas configurações não foram excluídas automaticamente pelo processo de migração, digite:
```
dfsrmig /deleteRoNtfrsMember contoso-dc2
```
Para excluir as configurações de AD DS global para replicação do FRS para todos os controladores de domínio somente leitura se essas configurações não forem excluídas automaticamente pelo processo de migração, digite:
```
dfsrmig /deleteRoNtfrsMember
```
Para excluir as configurações globais de AD DS para Replicação do DFS para um controlador de domínio somente leitura chamado contoso-DC2 se essas configurações não foram excluídas automaticamente pelo processo de migração, digite:
```
dfsrmig /deleteRoDfsrMember contoso-dc2
```
Para excluir as configurações globais de AD DS para Replicação do DFS para todos os controladores de domínio somente leitura se essas configurações não forem excluídas automaticamente pelo processo de migração, digite:
```
dfsrmig /deleteRoDfsrMember
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](https://go.microsoft.com/fwlink/?LinkId=122056)

- [Série de migração do SYSvol: parte 2 Dfsrmig. exe: a ferramenta de migração do SYSvol](https://go.microsoft.com/fwlink/?LinkID=121757)
