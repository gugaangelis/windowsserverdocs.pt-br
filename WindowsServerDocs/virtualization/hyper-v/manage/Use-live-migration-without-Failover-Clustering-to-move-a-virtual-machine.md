---
title: Use a migração ao vivo sem Clustering de Failover para mover uma máquina virtual
description: Fornece os pré-requisitos e instruções sobre como fazer uma migração ao vivo em um ambiente autônomo.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: a33912e09d664296f6eda964c40177353718d49c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851537"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Use a migração ao vivo sem Clustering de Failover para mover uma máquina virtual

>Aplica-se a: Windows Server 2016

Este artigo mostra como mover uma máquina virtual, fazendo uma migração ao vivo sem usar o Clustering de Failover. Uma migração ao vivo move máquinas virtuais em execução entre os hosts do Hyper-V sem qualquer tempo de inatividade perceptível.   
  
Para ser capaz de fazer isso, você precisará de:   

- Uma conta de usuário que seja membro do grupo local de administradores do Hyper-V ou o grupo de administradores nos computadores de origem e de destino. 
  
- A função Hyper-V no Windows Server 2016 ou Windows Server 2012 R2 instalado nos servidores de origem e destino e configurar para migrações ao vivo. Você pode fazer uma migração dinâmica entre hosts que executam o Windows Server 2016 e Windows Server 2012 R2 se a máquina virtual é pelo menos a versão 5. <br>Para instruções de atualização de versão, consulte [máquinas virtuais de atualização de versão no Hyper-V no Windows 10 ou Windows Server 2016](..\deploy\Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obter instruções de instalação, consulte [configurar hosts para migração ao vivo](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md). 
  
- As ferramentas de gerenciamento do Hyper-V instaladas em um computador que executa o Windows Server 2016 ou Windows 10, a menos que as ferramentas são instaladas no servidor de origem ou destino e você vai executá-los a partir daí.  
   
## <a name="BKMK_Step3"></a>Use o Gerenciador do Hyper-V para mover uma máquina virtual em execução  
  
1.  Abra o Gerenciador Hyper-V. (No Gerenciador do servidor, clique em **ferramentas** >>**Gerenciador Hyper-V**.)  
  
2.  No painel de navegação, selecione um dos servidores. (Se não estiver listado, clique com botão direito **Gerenciador do Hyper-V**, clique em **conectar ao servidor**, digite o nome do servidor e clique em **Okey**. Repita para adicionar mais servidores).  
  
3.  Dos **máquinas virtuais** painel, a máquina virtual com o botão direito e, em seguida, clique em **mover**. Isso abre o Assistente para mover. 
  
4.  Use as páginas do Assistente para escolher o tipo de movimentação, o servidor de destino e opções.
  
5.  Na página **Resumo**, reveja suas escolhas e clique em **Concluir**.  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Usar o Windows PowerShell para mover uma máquina virtual em execução
  
O exemplo a seguir usa o cmdlet Move-VM para mover uma máquina virtual denominada *LMTest* a um servidor de destino chamado *TestServer02* e move os discos rígidos virtuais e outros arquivos, esses pontos de verificação e Inteligente de arquivos de paginação, como o *D:\LMTest* diretório no servidor de destino.  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>Solução de problemas

### <a name="failed-to-establish-a-connection"></a>Falha ao estabelecer uma conexão 

Se você ainda não tiver configurado a delegação restrita, você deve entrar no servidor de origem antes de poder mover uma máquina virtual. Se você não fizer isso, a tentativa de autenticação falhar, ocorre um erro, e esta mensagem é exibida:  
  
"Falha na operação de migração de máquina Virtual na origem da migração.  
Falha ao estabelecer uma conexão com host *nome do computador*: Nenhuma credencial estiver disponível no pacote de segurança 0x8009030E."
  
 Para corrigir esse problema, entrar no servidor de origem e tente a migração novamente. Para evitar ter de fazer logon em um servidor de origem antes de fazer uma migração ao vivo, configure a delegação restrita. Você precisará de credenciais de administrador de domínio para configurar a delegação restrita. Para obter instruções, consulte [configurar hosts para migração ao vivo](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md). 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>Falhou porque o hardware do host não é compatível
 
 Se uma máquina virtual não tem compatibilidade de processador ativada e tem um ou mais instantâneos, a movimentação falhará se os hosts possuem versões diferentes de processador. Ocorrerá um erro e esta mensagem é exibida:
 
**A máquina virtual não pode ser movida para o computador de destino. O hardware no computador de destino não é compatível com os requisitos de hardware da máquina virtual.**
 
 Para corrigir esse problema, desligue a máquina virtual e ativar a configuração de compatibilidade do processador.
 
1. No Gerenciador do Hyper-V, na **máquinas virtuais** painel, clique com botão direito na máquina virtual e clique em configurações.
2. No painel de navegação, expanda **processadores** e clique em **compatibilidade**.
3. Verifique **migrar para um computador com uma versão de processador diferente**.
4. Clique em **OK**.
 
 Para usar o Windows PowerShell, use o [Set-VMProcessor](https://technet.microsoft.com/library/hh848533.aspx) cmdlet:
 
  ```
  PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
  ```