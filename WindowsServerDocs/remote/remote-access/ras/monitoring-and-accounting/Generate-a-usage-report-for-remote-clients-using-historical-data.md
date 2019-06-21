---
title: Gerar um relatório de uso para clientes remotos usando dados históricos
description: Este tópico faz parte do guia de monitoramento de acesso remoto e contabilização no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cfbac18f64123f97c54b29c1aeef7364af55e49a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281127"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Gerar um relatório de uso para clientes remotos usando dados históricos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

**Observação:** O Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O console de gerenciamento no servidor de acesso remoto pode ser usado para gerar um relatório de uso para os clientes remotos que estão acessando o servidor. Para gerar um relatório de uso para clientes remotos, primeiro habilite contabilidade no servidor de acesso remoto. Depois de gerar o relatório, você pode usar o painel de monitoramento está disponível no console de gerenciamento no servidor de acesso remoto para exibir as estatísticas de carga no servidor.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente realizar a tarefa enquanto você está conectado com uma conta que seja um membro do grupo Admins. do domínio.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Para habilitar as estatísticas no servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **REPORTING** para navegar até **relatórios de acesso remoto** no **Console de gerenciamento de acesso remoto**.  
  
3.  Clique em **configurar a contabilização** na **relatórios de acesso remoto** painel de tarefas.  
  
4.  Selecione o **usar estatísticas de caixa de entrada** caixa de seleção para habilitar as estatísticas no servidor de acesso remoto.  
  
5.  Clique em **Apply** para habilitar a configuração das estatísticas no servidor e, em seguida, clique em **fechar** depois que o servidor foi aplicada com êxito a configuração.  
  
#### <a name="to-generate-the-usage-report"></a>Para gerar o relatório de uso  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **REPORTING** para navegar até **relatórios de acesso remoto** no **Console de gerenciamento de acesso remoto**.  
  
3.  No painel central, clique em datas no calendário para selecionar a duração do relatório **data de início:** e **data de término:** e, em seguida, clique em **Gerar relatório**.  
  
4.  Você verá a lista de usuários que se conectou ao servidor de acesso remoto dentro do tempo selecionado e as estatísticas detalhadas sobre eles. Clique na primeira linha na lista. Quando você seleciona uma linha, a atividade de usuário remoto é mostrada no painel de visualização. Agora, selecione a **estatísticas de carga do servidor** guia no painel de visualização para ver o histórico de carga no servidor.  
  
    Clique o **estatísticas de carga do servidor** guia no painel de visualização para ver o histórico de carga no servidor.  
  
> [!NOTE]  
> **Entendendo sessões**  
>   
> Estatísticas de acesso remota baseia-se no conceito de **sessões**. Em contraste com um **conexão**, um **sessão** é identificada exclusivamente por uma combinação de nome de usuário e endereço IP de cliente remoto. Por exemplo, se o túnel de um computador é formado por cliente remoto, chamado Client1, uma sessão será ser criada e armazenada no banco de dados de estatísticas. Quando um usuário nomeado Usuário1 conecta-se de que o cliente após algum tempo passa (mas o túnel do computador ainda está ativo), a sessão é registrada como uma sessão separada. A distinção de sessões é manter a distinção entre o túnel de computador e o túnel do usuário.  
  
![Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
No script a seguir, altere o intervalo de datas para o qual você deseja um relatório na **- StartDateTime** e **- EndDateTime** parâmetros.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


