---
title: Monitorar o status de distribuição de configuração do servidor de Acesso Remoto
description: Este tópico faz parte do guia de monitoramento e contabilidade de acesso remoto no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 459471c87034fbeaf11f9099e643c495a6b75389
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997291"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>Monitorar o status de distribuição de configuração do servidor de Acesso Remoto

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

**Observação:** O Windows Server 2012 combina o DirectAccess e o serviço de acesso remoto (RAS) em uma única função de acesso remoto.

O Console de Gerenciamento de Acesso Remoto compara as versões de configuração de todos os servidores monitorados para verificar se eles coincidem e se estão usando a versão de configuração mais recente. Isso mostra se a versão de configuração mais recente (que está especificada em Objetos de Política de Grupo ou GPOs) foi distribuída a todos os servidores e se ela foi aplicada com êxito nos servidores.

### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>Para usar o painel de monitoramento para monitorar a distribuição de configuração

1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.

2.  Clique em **PAINEL** para navegar até o **Painel de Acesso Remoto** no **Console de Gerenciamento de Acesso Remoto**.

3.  No painel de monitoramento, observe o bloco **Status de Configuração** na parte superior central. Esse bloco mostra o status atual da distribuição de configuração.

A tabela a seguir mostra as mensagens geradas pelo bloco **Status de Configuração**, seus significados e a ação administrativa necessária (se houver alguma).

|Gravidade|Mensagem|Significado|O que fazer?|
|--|--|--|
|Sucesso|A configuração foi distribuída com êxito.|A configuração no GPO foi aplicada com êxito no servidor.|Nenhuma ação necessária.|
|Aviso|Configuração para o servidor [*nome do servidor*] não recuperada do controlador de domínio. O GPO não está vinculado.|A configuração no GPO ainda não atingiu o servidor. Isso pode ser devido ao fato de o GPO não estar vinculado ao servidor.|Vincule o GPO a um escopo de gerenciamento que é aplicado ao servidor ou em um cenário GPO de preparação, exporte manualmente as configurações do GPO de preparação e importe-as para o GPO de produção. Para obter mais informações sobre os GPOs de preparo, consulte **Gerenciando GPOs de acesso remoto com permissões limitadas** na [etapa-1-Plan-The-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/da-adv-plan-s1-infrastructure.md). Para etapas de preparo de GPO, consulte **Configurando GPOs de acesso remoto com permissões limitadas** na [etapa 1: configurar a infraestrutura do DirectAccess](../../directaccess/single-server-advanced/da-adv-configure-s1-infrastructure.md).|
|Aviso|A configuração para o servidor [*nome do servidor*] ainda não foi recuperada do controlador de domínio.|A configuração no GPO ainda não atingiu o servidor.<p>Pode levar até 10 minutos para propagar uma nova configuração.|Permitir mais tempo para que as políticas sejam atualizadas no servidor.|
|Erro|A configuração para o servidor [*nome do servidor*] não pode ser recuperada do controlador de domínio.|A configuração no GPO não atingiu o servidor e passaram-se mais de 10 minutos desde que a configuração foi alterada.|Isso poderia ocorrer em um dos seguintes cenários:<p>-O servidor não tem conectividade com o domínio para atualizar as políticas. Você pode executar "gpupdate/force" no servidor para forçar uma atualização de política.<br />-A replicação de GPO pode ser necessária para recuperar a configuração atualizada.<br />-Não há nenhum controlador de domínio gravável no site de Active Directory do servidor de acesso remoto.<p>Aguarde que os GPOs repliquem a todos os controladores de domínio e, em seguida, use o cmdlet do Windows PowerShell **Set-DAEntryPointDC** para associar o ponto de entrada a um controlador de domínio gravável no Active Directory no servidor de Acesso Remoto.|
|Aviso|A configuração para o servidor [*nome do servidor*] foi recuperada do controlador de domínio, mas ainda não foi aplicada.|A configuração no GPO atingiu o servidor, mas ainda não foi aplicada.<p>A aplicação de uma configuração pode levar até 15 minutos.|Permitir mais tempo para que a configuração seja completamente aplicada ao servidor.|
|Erro|A configuração do servidor [*nome do servidor*] recuperada do controlador de domínio não pode ser aplicada.|A configuração no GPO atingiu o servidor, mas não foi aplicada com êxito, e passaram-se mais de 15 minutos desde que a configuração foi alterada.|Isso poderia ocorrer em um dos seguintes cenários:<p>1. a configuração está atualmente em processo de aplicação. Isso é exibido como um erro, pois pode ter levado um longo tempo para recuperar a configuração do GPO.<br />    Para verificar se essa é a razão, use o **Agendador de Tarefas** e navegue até Microsoft\Windows\RemoteAccess para verificar se o **RAConfigTask** está atualmente em execução.<br />2. se o **RAConfigTask** não estiver em execução no momento, ele poderá ter falhado ao aplicar a configuração no servidor.<br />    Verifique se há erros em **Visualizador de Eventos** no canal de operações do servidor de Acesso Remoto, que está localizado em \Applications and Services Logs\Microsoft\Windows\RemoteAccess-RemoteAccessServer.<br />    Verifique se há erros em **STATUS DE OPERAÇÕES** no Console de Gerenciamento de Acesso Remoto. Para obter mais informações, consulte [Monitorar o status das operações do servidor de Acesso Remoto e seus componentes](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md).|
|Erro|Configuração de servidores multissite recuperada do controlador de domínio. A configuração não corresponde com todos os servidores.|Há uma inconsistência entre as versões de configuração dos GPOs de servidor na implantação multissite.<p>Idealmente, todos os GPOs de servidor para todos os pontos de entrada terão a mesma configuração global, mas por alguma razão, eles estão fora de sincronia.|Isso pode ocorrer quando uma alteração de configuração falha e não é revertida com êxito.<p>Você deve restaurar os GPOs de um estado de backup em que todos os GPOs de servidor estejam sincronizados. Para obter informações sobre um script que você pode usar, consulte [fazer backup e restaurar a configuração de acesso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).|