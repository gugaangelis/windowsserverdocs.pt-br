---
title: Solucionar problemas de perfis de usuário com eventos
description: Como solucionar problemas de carregamento e descarregamento de perfis de usuário usando os eventos e logs de rastreamento.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081821"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solucionar problemas de perfis de usuário com eventos

>Aplica-se a: 10 do Windows, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.

Este tópico aborda como solucionar problemas de carregamento e descarregamento de perfis de usuário usando os eventos e logs de rastreamento. As seções a seguir descrevem como usar os três logs de eventos que registram as informações de perfil de usuário.

## <a name="step-1-checking-events-in-the-application-log"></a>Etapa 1: Verificando eventos no log de aplicativos

A primeira etapa na solução de problemas com o carregamento e descarregamento de perfis (incluindo os perfis de usuário móvel) são usar o Visualizador de eventos para examinar quaisquer eventos de aviso e de erro que o log de registros de serviço de perfil de usuário no aplicativo do usuário.

Aqui está como exibir eventos de serviços de perfil de usuário no log do aplicativo:

1. Inicie o Visualizador de eventos. Para fazer isso, abra o **Painel de controle**, selecione **sistema e segurança**e, na seção **Ferramentas administrativas** , selecione **Exibir logs de eventos**. Abre a janela do Visualizador de eventos.
2. Na árvore de console, navegue pela primeira vez **Logs do Windows**, em seguida, o **aplicativo**.
3. No painel Ações, selecione **Filtrar Log atual**. Abre a caixa de diálogo Filtrar Log atual.
4. Na caixa **fontes de evento** , marque a caixa de seleção **Serviço de perfis de usuário** e, em seguida, selecione **Okey**.
5. Revise a lista de eventos, prestando atenção especial para eventos de erro.
6. Quando você encontrar eventos notável, selecione o link de Ajuda Online do Log de eventos para exibir informações adicionais e solução de problemas de procedimentos.
7. Para executar a investigar, observe a data e hora de eventos notável e examine o log operacional (conforme descrito na etapa 2) para exibir detalhes sobre o que o serviço de perfil de usuário estava fazendo perto da hora dos eventos de erro ou aviso.

>[!NOTE]
>Você pode ignorar com segurança User Profile Service 1530 "Windows detectados eventos que seu arquivo de registro ainda está em uso por outros aplicativos ou serviços."

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Etapa 2: Exibir o log operacional para o serviço de perfil de usuário

Se você não pode resolver o problema usando o log de aplicativo sozinho, use o procedimento a seguir para exibir eventos de serviço de perfil de usuário do log operacional. Esse log mostra algumas do funcionamento interno do serviço e pode ajudar a identificar onde em que a carga de perfil ou descarregar processo que está ocorrendo.

O log de aplicativo do Windows e o log de operacionais para serviço de perfil do usuário estão habilitados por padrão em todas as instalações do Windows.

Aqui está como exibir o log operacional para o serviço de perfil de usuário:

1. No caso de visualizador a árvore de console, navegue até **Logs aplicativos e serviços**, e em seguida, **Microsoft**, e em seguida, **Windows**, e em seguida, **O serviço de perfil de usuário**e, em seguida, **operacional**.
2. Examine os eventos que ocorreram no momento, os eventos de erro ou aviso que você anotou no log de aplicativos.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Etapa 3: Habilitar e exibir analítico e logs de depuração

Se você precisar de mais detalhes do que o log operacional fornece, você pode habilitar analítico e logs no computador afetado de depuração. Este nível de log é muito mais detalhado e deve ser desabilitado, exceto quando tentando solucionar um problema.

Aqui está como habilitar e exibir analítico e logs de depuração:

1. No painel de **ações** do Visualizador de eventos, selecione o **modo de exibição**e selecione **Mostrar depurar Logs analíticos e**.
2. Navegue até **Logs de aplicativos e serviços**, e em seguida, **Microsoft**, e em seguida, **Windows**, e em seguida, **serviço de perfil de usuário**e, em seguida, **Diagnóstico**.
3. Selecione **Habilitar Log** e, em seguida, selecione **Sim**. Isso permite que o log de diagnóstico, que iniciará o registro em log.
4. Se você precisar ainda mais informações detalhadas, consulte [etapa 4: Criando e um rastreamento de decodificação](#step-4:-creating-and-decoding-a-trace) para obter mais informações sobre como criar um log de rastreamento.
5. Quando tiver terminado de resolver o problema, navegue até o log de **Diagnóstico** , selecione **Desabilitar o Log**, selecione o **modo de exibição** e, em seguida, desmarque a caixa de seleção **Mostrar depurar Logs analíticos e** para ocultar analítico e log de depuração.

## <a name="step-4-creating-and-decoding-a-trace"></a>Etapa 4: Criando e decodificação um rastreamento

Se você não puder resolver o problema usando eventos, você pode criar um log de rastreamento (um arquivo ETL) ao reproduzir o problema e, em seguida, decodificá-lo usando símbolos públicos do servidor Microsoft símbolo. Logs de rastreamento fornecem informações muito específicas sobre o que está fazendo o serviço de perfil de usuário e pode ajudar a identificar onde a falha ocorreu.

A melhor estratégia ao usar o rastreamento de ETL é primeiramente capturar o log menor possível. Depois que o log está decodificado, pesquise o log de falhas.

Aqui está como criar e decodificar um rastreamento para o serviço de perfil de usuário:

1. Inscreva-se para o computador onde o usuário está tendo problemas, usando uma conta que seja membro do grupo Administradores local.
2. Em um prompt de comando elevado, digite os comandos a seguir, onde *\ < caminho >* é o caminho para uma pasta local que você criou anteriormente, por exemplo, C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Na tela Iniciar, selecione o nome de usuário e selecione **conta de comutador**, tomando cuidado para não fazer logoff do administrador. Se você estiver usando a área de trabalho remota, feche a sessão de administrador para estabelecer a sessão do usuário.
4. Reproduza o problema. O procedimento para reproduzir o problema é geralmente para fazer logon como usuário enfrentando o problema, a aprovação de usuário, ou ambos.
5. Depois de reproduzir o problema, entre como administrador local novamente.
6. A partir de um prompt de comando elevado, execute o seguinte comando para salvar o log em um arquivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digite o seguinte comando para exportar o arquivo ETL em um arquivo legíveis no diretório atual (provavelmente sua pasta base ou a pasta %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra o arquivo **Summary. txt** e o arquivo **dumpfile** (você pode abri-los no Microsoft Excel para ver mais facilmente os detalhes completos do log). Procure por eventos que incluem **falhar** ou **Falha**; ignore as linhas que incluem o nome do evento **desconhecido** .

## <a name="more-information"></a>Mais informações

* [Implantar a perfis de usuários móveis](deploy-roaming-user-profiles.md)