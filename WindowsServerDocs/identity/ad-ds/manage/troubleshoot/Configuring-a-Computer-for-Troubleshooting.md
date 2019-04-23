---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configurar um computador para solução de problemas
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1acb5f7d309d58ed4a5a3aca6bb89f01c0cbf933
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854117"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurar um computador para solução de problemas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de usar técnicas avançadas de solução de problemas para identificar e corrigir problemas do Active Directory, configure seus computadores para solução de problemas. Você também deve ter uma compreensão básica dos conceitos, procedimentos e ferramentas de solução de problemas.

Para obter informações sobre as ferramentas de monitoramento para o Windows Server, consulte o guia passo a passo para [monitoramento de desempenho e confiabilidade no Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Tarefas de configuração para solução de problemas

Para configurar seu computador para solucionar problemas de serviços de domínio Active Directory (AD DS), execute as seguintes tarefas:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Instalar ferramentas de administração de servidor remoto para o AD DS

Quando você instala o AD DS para criar um controlador de domínio, as ferramentas administrativas que você usa para gerenciar o AD DS são instaladas automaticamente. Se você quiser gerenciar controladores de domínio remotamente de um computador que não seja um controlador de domínio, você pode instalar as ferramentas de administração de servidor remoto (RSAT) em um servidor membro ou estação de trabalho que está executando uma versão com suporte do Windows. RSAT substitui as ferramentas de suporte do Windows do Windows Server 2003.

Para obter informações sobre como instalar o RSAT, consulte o artigo [ferramentas de administração de servidor remoto](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurar o Monitor de desempenho e confiabilidade

O Windows Server inclui o Monitor de desempenho, que é um snap-in do Console de gerenciamento Microsoft (MMC) que combina a funcionalidade de ferramentas autônomas anteriores, incluindo Logs e alertas, Server Performance Advisor, desempenho e confiabilidade do Windows e o Monitor do sistema. Esse snap-in fornece uma interface gráfica do usuário (GUI) para personalizar conjuntos de Coletores de dados e sessões de rastreamento de eventos.

Monitor de confiabilidade e desempenho também inclui o Monitor de confiabilidade, um snap-in do MMC que controla as alterações no sistema e compara-as com as alterações na estabilidade do sistema, fornecendo uma exibição gráfica de suas relações.

### <a name="set-logging-levels"></a>Definir níveis de log

Se as informações que você recebe no log do serviço de diretório no Visualizador de eventos não são suficientes para solução de problemas, elevar os níveis de registro em log usando a entrada de registro apropriadas em **HKEY_LOCAL MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**.

Por padrão, os níveis de log para todas as entradas são definidos **0**, que fornece a quantidade mínima de informações. É o mais alto nível de log **5**. Aumentar o nível de uma entrada faz com que eventos adicionais a serem registrados no log de eventos do serviço de diretório.

Use o procedimento a seguir para alterar o nível de log para uma entrada de diagnóstico. Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

> [!WARNING]
> É recomendável não editar diretamente o Registro, a menos que não haja outra alternativa. Modificações no registro não são validadas pelo editor do registro ou pelo Windows antes que eles são aplicados e como resultado, os valores incorretos podem ser armazenados. Isso pode resultar em erros irrecuperáveis no sistema. Quando possível, use a diretiva de grupo ou outras ferramentas do Windows, como snap-ins do MMC, para realizar tarefas, em vez de editar o registro diretamente. Se você deve editar o Registro, tenha muito cuidado.
>

Para alterar o nível de log para uma entrada de diagnóstico

1. Clique em **inicie** > **execute** > tipo **regedit** > clique em **Okey**.
2. Navegue até a entrada para o qual você deseja definir o registro em log no.
   * EXEMPLO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Duas vezes na entrada e, na **Base**, clique em **Decimal**.
4. Na **valor**, digite um inteiro de **0** por meio do **5**e, em seguida, clique em **Okey**.
