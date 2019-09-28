---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configurar um computador para solução de problemas
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8e11883de9f89d0b95ed0fc35b4f5f3941ef82a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368899"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurar um computador para solução de problemas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de usar técnicas de solução de problemas avançadas para identificar e corrigir problemas de Active Directory, configure seus computadores para solucionar problemas. Você também deve ter um entendimento básico de conceitos, procedimentos e ferramentas de solução de problemas.

Para obter informações sobre as ferramentas de monitoramento para o Windows Server, consulte o guia passo a passo para o [monitoramento de desempenho e confiabilidade no Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Tarefas de configuração para solução de problemas

Para configurar o computador para solução de problemas Active Directory Domain Services (AD DS), execute as seguintes tarefas:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Instalar Ferramentas de Administração de Servidor Remoto para AD DS

Quando você instala o AD DS para criar um controlador de domínio, as ferramentas administrativas que você usa para gerenciar o AD DS são instaladas automaticamente. Se você quiser gerenciar controladores de domínio remotamente de um computador que não seja um controlador de domínio, poderá instalar o Ferramentas de Administração de Servidor Remoto (RSAT) em um servidor membro ou estação de trabalho que esteja executando uma versão com suporte do Windows. O RSAT substitui as ferramentas de suporte do Windows do Windows Server 2003.

Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurar o monitor de desempenho e confiabilidade

O Windows Server inclui o monitor de desempenho e confiabilidade do Windows, que é um snap-in do MMC (console de gerenciamento Microsoft) que combina a funcionalidade de ferramentas autônomas anteriores, incluindo Logs e Alertas de Desempenho, supervisor de desempenho de servidor, e o monitor do sistema. Esse snap-in fornece uma GUI (interface gráfica do usuário) para personalizar conjuntos de coletores de dados e sessões de rastreamento de eventos.

O monitor de confiabilidade e desempenho também inclui o monitor de confiabilidade, um snap-in do MMC que controla as alterações no sistema e os compara com as alterações na estabilidade do sistema, fornecendo uma exibição gráfica de sua relação.

### <a name="set-logging-levels"></a>Definir níveis de log

Se as informações recebidas no log do serviço de diretório Visualizador de Eventos não forem suficientes para solução de problemas, aumente os níveis de log usando a entrada de registro apropriada no **HKEY_LOCAL_ MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**.

Por padrão, os níveis de log para todas as entradas são definidos como **0**, que fornece a quantidade mínima de informações. O nível de log mais alto é **5**. O aumento do nível de uma entrada faz com que eventos adicionais sejam registrados no log de eventos do serviço de diretório.

Use o procedimento a seguir para alterar o nível de log para uma entrada de diagnóstico. Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

> [!WARNING]
> É recomendável não editar diretamente o Registro, a menos que não haja outra alternativa. As modificações no registro não são validadas pelo editor do registro ou pelo Windows antes de serem aplicadas e, como resultado, os valores incorretos podem ser armazenados. Isso pode resultar em erros irrecuperáveis no sistema. Quando possível, use Política de Grupo ou outras ferramentas do Windows, como snap-ins do MMC, para realizar tarefas, em vez de editar o registro diretamente. Se você deve editar o Registro, tenha muito cuidado.
>

Para alterar o nível de log para uma entrada de diagnóstico

1. Clique em **iniciar** > **executar** > digite **regedit** > clique em **OK**.
2. Navegue até a entrada para a qual você deseja definir o logon.
   * EXEMPLO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Clique duas vezes na entrada e, em **base**, clique em **decimal**.
4. Em **valor**, digite um inteiro de **0** a **5**e, em seguida, clique em **OK**.
