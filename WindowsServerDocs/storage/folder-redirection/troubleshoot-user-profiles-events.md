---
title: Solucionar problemas de perfis de usuário com eventos
description: Como solucionar problemas de carregamento e descarregamento de perfis de usuário usando eventos e logs de rastreamento.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827947"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solucionar problemas de perfis de usuário com eventos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.

Este tópico discute como solucionar problemas de carregamento e descarregamento de perfis de usuário usando eventos e logs de rastreamento. As seções a seguir descrevem como usar os três logs de eventos que registram informações de perfil do usuário.

## <a name="step-1-checking-events-in-the-application-log"></a>Etapa 1: Verificar os eventos no log do aplicativo

A primeira etapa na solução de problemas com o carregamento e descarregamento de usuário (incluindo perfis de usuário móvel) de perfis são usar o Visualizador de eventos para examinar quaisquer eventos de aviso e erro que o log de registros de serviço de perfil do usuário no aplicativo.

Aqui está como exibir eventos de serviços de perfil do usuário no log do aplicativo:

1. Inicie o Visualizador de eventos. Para fazer isso, abra **painel de controle**, selecione **sistema e segurança**e, em seguida, no **ferramentas administrativas** seção, selecione **exibir logs de eventos**. Abre a janela do Visualizador de eventos.
2. Na árvore de console, primeiro navegue até **Logs do Windows**, em seguida, **aplicativo**.
3. No painel de ações, selecione **Filtrar Log atual**. Abre a caixa de diálogo Filtrar Log atual.
4. No **origens do evento** caixa, selecione a **serviço de perfis de usuário** caixa de seleção e, em seguida, selecione **Okey**.
5. Examine a lista de eventos, prestando atenção especial aos eventos de erro.
6. Quando você encontrar eventos digno de nota, selecione o link de Ajuda Online do Log de eventos para exibir informações adicionais e os procedimentos de solução de problemas.
7. Para executar a solução de problemas adicionais, observe a data e hora dos eventos notáveis e, em seguida, examine o log operacional (conforme descrito na etapa 2) para exibir detalhes sobre o que o serviço de perfil do usuário estava fazendo na época dos eventos de erro ou aviso.

>[!NOTE]
>Você pode ignorar com segurança serviço de perfil do usuário 1530 "Windows detectado evento que seu arquivo de registro ainda está em uso por outros aplicativos ou serviços."

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Etapa 2: Exibir o log operacional para o serviço de perfil do usuário

Se você não pode resolver o problema usando o log de aplicativo autônomo, use o procedimento a seguir para exibir eventos do serviço de perfil do usuário no log operacional. Esse log mostra algumas do funcionamento interno do serviço e pode ajudar a identificar por onde na carga de perfil ou descarregar o processo que o problema está ocorrendo.

O log de aplicativo do Windows e o log operacional para serviço de perfil do usuário são habilitadas por padrão em todas as instalações do Windows.

Aqui está como exibir o log operacional para o serviço de perfil do usuário:

1. Na árvore de console do Visualizador de eventos, navegue até **Applications and Services Logs**, em seguida, **Microsoft**, em seguida, **Windows**, em seguida, **deserviçodeperfildeusuário**e então **operacionais**.
2. Examine os eventos que ocorreram no momento dos eventos de erro ou aviso que você anotou no log do aplicativo.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Etapa 3: Habilitar e exibir analítica e logs de depuração

Se você precisar de mais detalhes do que o log operacional fornece, você pode habilitar a análise e depurar os logs no computador afetado. Esse nível de log é muito mais detalhado e deve ser desabilitado, exceto quando tentar solucionar um problema.

Aqui está como habilitar e exibir analítica e logs de depuração:

1. No **ações** painel do Visualizador de eventos, selecione **exibição**e, em seguida, selecione **Mostrar Logs analíticos e depuração**.
2. Navegue até **Applications and Services Logs**, em seguida, **Microsoft**, em seguida, **Windows**, em seguida, **serviço de perfil do usuário**e, em seguida,  **Diagnóstico**.
3. Selecione **Habilitar Log** e, em seguida, selecione **Sim**. Isso permite que o log de diagnóstico, que iniciará o registro em log.
4. Se você precisar de informações ainda mais detalhadas, consulte [etapa 4: Criando e decodificação de um rastreamento](#step-4:-creating-and-decoding-a-trace) para obter mais informações sobre como criar um log de rastreamento.
5. Quando tiver terminado de solucionar o problema, navegue até a **diagnóstico** log, selecione **desabilitar Log**, selecione **exibição** e, em seguida, desmarque o **Mostrar Depurar Logs analíticos e** caixa de seleção para ocultar analítica e o log de depuração.

## <a name="step-4-creating-and-decoding-a-trace"></a>Etapa 4: Criando e decodificação de um rastreamento

Se você não puder resolver o problema usando eventos, você pode criar um log de rastreamento (um arquivo ETL) ao reproduzir o problema e, em seguida, decodificá-lo usando símbolos públicos do servidor de símbolo Microsoft. Logs de rastreamento fornecem informações bastante específicas sobre o que está fazendo o serviço de perfil do usuário e podem ajudar a identificar onde a falha ocorreu.

A melhor estratégia ao usar o rastreamento de ETL é primeiro capturar o log de menor possível. Depois que o log é decodificado, pesquise o log para falhas.

Aqui está como criar e decodificar um rastreamento para o serviço de perfil do usuário:

1. Faça logon no computador em que o usuário está enfrentando problemas, usando uma conta que seja membro do grupo Administradores local.
2. Em um prompt de comando elevado, digite os comandos a seguir, onde *\<caminho\>* é o caminho para uma pasta local que você criou anteriormente, por exemplo c:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Na tela inicial, selecione o nome de usuário e, em seguida, selecione **altere a conta**, tomando cuidado para não fazer logoff do administrador. Se você estiver usando a área de trabalho remota, feche a sessão de administrador para estabelecer a sessão do usuário.
4. Reproduza o problema. O procedimento para reproduzir o problema normalmente é para fazer logon como o usuário enfrentando o problema, a fazer logon do usuário, ou ambos.
5. Depois de reproduzir o problema, faça logon no como administrador local novamente.
6. Em um prompt de comando elevado, execute o seguinte comando para salvar o log em um arquivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digite o seguinte comando para exportar o arquivo ETL em um arquivo legível por humanos no diretório atual (provavelmente sua pasta base ou a pasta % WINDIR %\\pasta System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra o **Summary. txt** arquivo e **dumpfile** arquivo (você pode abri-los no Microsoft Excel para exibir mais facilmente os detalhes completos do log). Procure eventos que incluem **falhar** ou **falha**; você pode ignorar com segurança as linhas que contêm os **desconhecido** nome do evento.

## <a name="more-information"></a>Mais informações

* [Implantar perfis de usuário móvel](deploy-roaming-user-profiles.md)