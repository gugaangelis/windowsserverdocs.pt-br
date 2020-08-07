---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configurar um computador para solução de problemas
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 05aa12b16d67a8f91ed82064c8464927546bfee8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947900"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurar um computador para solução de problemas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de usar técnicas de solução de problemas avançadas para identificar e corrigir problemas de Active Directory, configure seus computadores para solucionar problemas. Você também deve ter um entendimento básico de conceitos, procedimentos e ferramentas de solução de problemas.

Para obter informações sobre as ferramentas de monitoramento para o Windows Server, consulte o guia passo a passo para o [monitoramento de desempenho e confiabilidade no Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Tarefas de configuração para solução de problemas

Para configurar o computador para solução de problemas Active Directory Domain Services (AD DS), execute as seguintes tarefas:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Instalar Ferramentas de Administração de Servidor Remoto para AD DS

Quando você instala o AD DS para criar um controlador de domínio, as ferramentas administrativas que você usa para gerenciar o AD DS são instaladas automaticamente. Se você quiser gerenciar controladores de domínio remotamente de um computador que não seja um controlador de domínio, poderá instalar o Ferramentas de Administração de Servidor Remoto (RSAT) em um servidor membro ou estação de trabalho que esteja executando uma versão com suporte do Windows. O RSAT substitui as ferramentas de suporte do Windows do Windows Server 2003.

Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](../../../../remote/remote-server-administration-tools.md).

### <a name="configure-reliability-and-performance-monitor"></a>Configurar o monitor de desempenho e confiabilidade

O Windows Server inclui o monitor de desempenho e confiabilidade do Windows, que é um snap-in do MMC (console de gerenciamento Microsoft) que combina a funcionalidade das ferramentas autônomas anteriores, incluindo Logs e Alertas de Desempenho, o supervisor de desempenho do servidor e o monitor do sistema. Esse snap-in fornece uma GUI (interface gráfica do usuário) para personalizar conjuntos de coletores de dados e sessões de rastreamento de eventos.

O monitor de confiabilidade e desempenho também inclui o monitor de confiabilidade, um snap-in do MMC que controla as alterações no sistema e os compara com as alterações na estabilidade do sistema, fornecendo uma exibição gráfica de sua relação.

### <a name="set-logging-levels"></a>Definir os níveis de registros em log

Se as informações recebidas no log do serviço de diretório Visualizador de Eventos não forem suficientes para solução de problemas, aumente os níveis de log usando a entrada de registro apropriada no **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\ntds\diagnostics**.

Por padrão, os níveis de log para todas as entradas são definidos como **0**, que fornece a quantidade mínima de informações. O nível de log mais alto é **5**. O aumento do nível de uma entrada faz com que eventos adicionais sejam registrados no log de eventos do serviço de diretório.

Use o procedimento a seguir para alterar o nível de log para uma entrada de diagnóstico. A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

> [!WARNING]
> É recomendável não editar diretamente o Registro, a menos que não haja outra alternativa. As modificações no registro não são validadas pelo editor do registro ou pelo Windows antes de serem aplicadas e, como resultado, os valores incorretos podem ser armazenados. Isso pode resultar em erros irrecuperáveis no sistema. Quando possível, use Política de Grupo ou outras ferramentas do Windows, como snap-ins do MMC, para realizar tarefas, em vez de editar o registro diretamente. Se você deve editar o Registro, tenha muito cuidado.
>

Para alterar o nível de log para uma entrada de diagnóstico

1. Clique em **Iniciar**  >  **execução** > digite **regedit** > clique em **OK**.
2. Navegue até a entrada para a qual você deseja definir o logon.
   * EXEMPLO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Clique duas vezes na entrada e, em **base**, clique em **decimal**.
4. Em **valor**, digite um inteiro de **0** a **5**e, em seguida, clique em **OK**.
