---
title: Gerar um relatório de uso para clientes remotos usando dados históricos
description: Este tópico faz parte do guia de monitoramento e contabilidade de acesso remoto no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bae50345e8a6fd4018857e2a754d0274ce02855d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367246"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Gerar um relatório de uso para clientes remotos usando dados históricos

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

**Observação:** o Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O console de gerenciamento do no servidor de acesso remoto pode ser usado para gerar um relatório de uso para os clientes remotos que estão acessando o servidor. Para gerar um relatório de uso para clientes remotos, primeiro habilite a contabilidade no servidor de acesso remoto. Depois de gerar o relatório, você pode usar o painel de monitoramento que está disponível no console de gerenciamento do no servidor de acesso remoto para exibir as estatísticas de carga no servidor.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente executar a tarefa enquanto estiver conectado com uma conta que seja membro do grupo Admins. do domínio.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Para habilitar a contabilidade no servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **relatório** para navegar até **relatórios de acesso remoto** no console de gerenciamento de **acesso remoto**.  
  
3.  Clique em **Configurar contabilidade** no painel de tarefas **relatórios de acesso remoto** .  
  
4.  Marque a caixa de seleção **usar contabilização de caixa de entrada** para habilitar a contabilidade no servidor de acesso remoto.  
  
5.  Clique em **aplicar** para habilitar a configuração de contabilidade no servidor e clique em **fechar** depois que o servidor aplicou a configuração com êxito.  
  
#### <a name="to-generate-the-usage-report"></a>Para gerar o relatório de uso  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **relatório** para navegar até **relatórios de acesso remoto** no console de gerenciamento de **acesso remoto**.  
  
3.  No painel central, clique em datas no calendário para selecionar a **data de início** da duração do relatório: e **data de término:** e clique em **gerar relatório**.  
  
4.  Você verá a lista de usuários que se conectaram ao servidor de acesso remoto dentro do tempo selecionado e as estatísticas detalhadas sobre eles. Clique na primeira linha da lista. Quando você seleciona uma linha, a atividade de usuário remoto é mostrada no painel de visualização. Agora, selecione a guia **Estatísticas de carregamento do servidor** no painel de visualização para ver a carga histórica no servidor.  
  
    Clique na guia **Estatísticas de carregamento do servidor** no painel de visualização para ver a carga histórica no servidor.  
  
> [!NOTE]  
> **Noções básicas sobre sessões**  
>   
> A contabilização de acesso remoto é baseada no conceito de **sessões**. Ao contrário de uma **conexão**, uma **sessão** é identificada exclusivamente por uma combinação de endereço IP do cliente remoto e nome de usuário. Por exemplo, se um túnel de máquina for formado a partir do cliente remoto, chamado CLIENT1, uma sessão será criada e armazenada no banco de dados de estatísticas. Quando um usuário chamado user1 se conecta desse cliente após algum tempo (mas o túnel de máquina ainda está ativo), a sessão é registrada como uma sessão separada. A distinção de sessões é manter a distinção entre o túnel do computador e o túnel do usuário.  
  
![](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
No script a seguir, altere o intervalo de datas para o qual você deseja um relatório nos parâmetros **-StartDateTime** e **-EndDateTime** .  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


