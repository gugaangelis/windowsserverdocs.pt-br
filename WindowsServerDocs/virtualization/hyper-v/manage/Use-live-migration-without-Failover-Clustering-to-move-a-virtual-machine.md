---
title: Usar a migração ao vivo sem clustering de failover para mover uma máquina virtual
description: Fornece pré-requisitos e instruções para fazer uma migração ao vivo em um ambiente autônomo.
manager: dongill
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: kbdazure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: e28ddefb5c1718fa251aa3de6d4323a3bc14c633
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990230"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Usar a migração ao vivo sem clustering de failover para mover uma máquina virtual

>Aplica-se a: Windows Server 2016

Este artigo mostra como mover uma máquina virtual fazendo uma migração ao vivo sem usar clustering de failover. Uma migração ao vivo move as máquinas virtuais em execução entre os hosts do Hyper-V sem nenhum tempo de inatividade perceptível.

Para poder fazer isso, você precisará de:

- Uma conta de usuário que seja membro do grupo de administradores do Hyper-V local ou do grupo de administradores nos computadores de origem e de destino.

- A função Hyper-V no Windows Server 2016 ou no Windows Server 2012 R2 instalada nos servidores de origem e de destino e configurada para migrações dinâmicas. Você pode fazer uma migração ao vivo entre hosts que executam o Windows Server 2016 e o Windows Server 2012 R2 se a máquina virtual for, no mínimo, a versão 5.

    Para obter instruções de atualização de versão, consulte [atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server 2016](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obter instruções de instalação, consulte [Configurar hosts para migração dinâmica](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

- As ferramentas de gerenciamento do Hyper-V instaladas em um computador que executa o Windows Server 2016 ou o Windows 10, a menos que as ferramentas estejam instaladas no servidor de origem ou de destino e você as execute a partir daí.

## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>Usar o Gerenciador do Hyper-V para mover uma máquina virtual em execução

1.  Abra o Gerenciador do Hyper-V. (Em Gerenciador do Servidor, clique em **ferramentas**  >> **Gerenciador do Hyper-V**.)

2.  No painel de navegação, selecione um dos servidores. (Se não estiver listado, clique com o botão direito do mouse em **Gerenciador do Hyper-V**, clique em **conectar ao servidor**, digite o nome do servidor e clique em **OK**. Repita para adicionar mais servidores.)

3.  No painel **máquinas virtuais** , clique com o botão direito do mouse na máquina virtual e clique em **mover**. Isso abre o assistente de movimentação.

4.  Use as páginas do assistente para escolher o tipo de movimentação, servidor de destino e opções.

5.  Na página **Resumo**, reveja suas escolhas e clique em **Concluir**.

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Usar o Windows PowerShell para mover uma máquina virtual em execução

O exemplo a seguir usa o cmdlet Move-VM para mover uma máquina virtual chamada *LMTest* para um servidor de destino chamado *TestServer02* e move os discos rígidos virtuais e outros arquivos, tais pontos de verificação e arquivos de paginação inteligente, para o diretório *D:\LMTest* no servidor de destino.

```
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest
```

## <a name="troubleshooting"></a>Solução de problemas

### <a name="failed-to-establish-a-connection"></a>Falha ao estabelecer uma conexão

Se você ainda não configurou a delegação restrita, deverá entrar no servidor de origem antes de poder mover uma máquina virtual. Se você não fizer isso, a tentativa de autenticação falhará, ocorrerá um erro e essa mensagem será exibida:

"Falha na operação de migração da máquina virtual na origem da migração.
Falha ao estabelecer uma conexão com o *nome do computador*host: nenhuma credencial está disponível no pacote de segurança 0x8009030E. "

 Para corrigir esse problema, entre no servidor de origem e tente mover novamente. Para evitar ter que entrar em um servidor de origem antes de fazer uma migração ao vivo, configure a delegação restrita. Você precisará de credenciais de administrador de domínio para configurar a delegação restrita. Para obter instruções, consulte [Configurar hosts para migração dinâmica](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>Falha porque o hardware do host não é compatível

 Se uma máquina virtual não tiver a compatibilidade do processador ativada e tiver um ou mais instantâneos, a movimentação falhará se os hosts tiverem versões de processador diferentes. Ocorre um erro e essa mensagem é exibida:

**A máquina virtual não pode ser movida para o computador de destino. O hardware no computador de destino não é compatível com os requisitos de hardware desta máquina virtual.**

 Para corrigir esse problema, desligue a máquina virtual e ative a configuração de compatibilidade do processador.

1. No Gerenciador do Hyper-V, no painel **máquinas virtuais** , clique com o botão direito do mouse na máquina virtual e clique em configurações.
2. No painel de navegação, expanda **processadores** e clique em **compatibilidade**.
3. Verifique **migrar para um computador com uma versão de processador diferente**.
4. Clique em **OK**.

   Para usar o Windows PowerShell, use o cmdlet [set-VMProcessor](/powershell/module/hyper-v/set-vmprocessor?view=win10-ps) :

   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```