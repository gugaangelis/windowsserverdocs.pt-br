---
title: Solucionar problemas de perfis de usuário com eventos
description: Como solucionar problemas de carregamento e descarregamento de perfis de usuário usando eventos e logs de rastreamento.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e927a77627e786015a928d798aafee13a2cc34b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394384"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solucionar problemas de perfis de usuário com eventos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server (canal semestral).

Este tópico discute como solucionar problemas de carregamento e descarregamento de perfis de usuário usando eventos e logs de rastreamento. As seções a seguir descrevem como usar os três logs de eventos que registram informações de perfil do usuário.

## <a name="step-1-checking-events-in-the-application-log"></a>Etapa 1: Verificando eventos no log do aplicativo

A primeira etapa na solução de problemas com o carregamento e o descarregamento de perfis de usuário (incluindo perfis de usuários móveis) é usar Visualizador de Eventos para examinar os eventos de aviso e de erro que o serviço de perfil de usuário registra no log do aplicativo.

Veja como exibir eventos de serviços de perfil de usuário no log do aplicativo:

1. Iniciar Visualizador de Eventos. Para fazer isso, abra **o painel de controle**, selecione **sistema e segurança**e, na seção **Ferramentas administrativas** , selecione **Exibir logs de eventos**. A janela Visualizador de Eventos é aberta.
2. Na árvore de console, primeiro navegue até **logs do Windows**e, em seguida, **aplicativo**.
3. No painel Ações, selecione **Filtrar log atual**. A caixa de diálogo Filtrar log atual é aberta.
4. Na caixa **origens do evento** , selecione a CheckBox **serviço perfis de usuário** e, em seguida, selecione **OK**.
5. Examine a listagem de eventos, prestando atenção especial aos eventos de erro.
6. Ao encontrar eventos notáveis, selecione o link de ajuda online do log de eventos para exibir informações adicionais e procedimentos de solução de problemas.
7. Para executar mais soluções de problemas, observe a data e a hora de eventos notáveis e examine o log operacional (conforme descrito na etapa 2) para exibir detalhes sobre o que o serviço de perfil de usuário estava fazendo em torno do tempo dos eventos de erro ou de aviso.

>[!NOTE]
>Você pode ignorar com segurança o evento 1530 do serviço de perfil de usuário "o Windows detectou que o arquivo de registro ainda está em uso por outros aplicativos ou serviços".

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Etapa 2: Exibir o log operacional para o serviço de perfil de usuário

Se você não puder resolver o problema usando o log do aplicativo sozinho, use o procedimento a seguir para exibir os eventos do serviço de perfil do usuário no log operacional. Esse log mostra alguns dos trabalhos internos do serviço e pode ajudar a identificar onde o problema é carregado ou descarregado no processo de carregamento do perfil.

O log de aplicativos do Windows e o log operacional do serviço de perfil de usuário são habilitados por padrão em todas as instalações do Windows.

Veja como exibir o log operacional para o serviço de perfil de usuário:

1. Na árvore de console do Visualizador de Eventos, navegue **até logs de aplicativos e serviços**, em seguida, **Microsoft**, **Windows**, serviço de **perfil de usuário**e, em seguida, **operacional**.
2. Examine os eventos que ocorreram em vez do erro ou dos eventos de aviso que você anotou no log do aplicativo.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Etapa 3: Habilitar e exibir logs analíticos e de depuração

Se precisar de mais detalhes do que o log operacional fornece, você poderá habilitar os logs analíticos e de depuração no computador afetado. Esse nível de registro em log é muito mais detalhado e deve ser desabilitado, exceto ao tentar solucionar um problema.

Veja como habilitar e exibir logs analíticos e de depuração:

1. No painel **ações** de visualizador de eventos, selecione **Exibir**e, em seguida, selecione **Mostrar logs analíticos e de depuração**.
2. Navegue até **aplicativos e serviços logs**, em seguida, **Microsoft**, **Windows**, **serviço de perfil de usuário**e **diagnóstico**.
3. Selecione **habilitar log** e, em seguida, selecione **Sim**. Isso habilita o log de diagnóstico, que iniciará o registro em log.
4. Se você precisar de informações ainda mais detalhadas, consulte [Step 4: Criando e decodificando um Trace @ no__t-0 para obter mais informações sobre como criar um log de rastreamento.
5. Quando você terminar de solucionar o problema, navegue até o log de **diagnóstico** , **selecione Desabilitar log**, selecione **Exibir** e desmarque a caixa de seleção **Mostrar logs analíticos e de depuração** para ocultar o log analítico e de depuração.

## <a name="step-4-creating-and-decoding-a-trace"></a>Etapa 4: Criando e decodificando um rastreamento

Se você não puder resolver o problema usando eventos, poderá criar um log de rastreamento (um arquivo ETL) ao reproduzir o problema e, em seguida, decodificá-lo usando símbolos públicos do servidor de símbolos da Microsoft. Os logs de rastreamento fornecem informações muito específicas sobre o que o serviço de perfil de usuário está fazendo e pode ajudar a identificar onde a falha ocorreu.

A melhor estratégia ao usar o rastreamento de ETL é primeiro capturar o menor log possível. Depois que o log for decodificado, procure falhas no log.

Veja como criar e decodificar um rastreamento para o serviço de perfil de usuário:

1. Faça logon no computador em que o usuário está enfrentando problemas, usando uma conta que seja membro do grupo local de administradores.
2. Em um prompt de comandos com privilégios elevados, insira os comandos a seguir, em que *\<Path @ no__t-2* é o caminho para uma pasta local que você criou anteriormente, por exemplo C: \\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Na tela iniciar, selecione o nome de usuário e, em seguida, selecione **mudar conta**, tendo cuidado para não fazer logoff do administrador. Se você estiver usando Área de Trabalho Remota, feche a sessão de administrador para estabelecer a sessão do usuário.
4. Reproduza o problema. O procedimento para reproduzir o problema normalmente é entrar como o usuário que está enfrentando o problema, desconectar o usuário ou ambos.
5. Depois de reproduzir o problema, faça logon novamente como administrador local.
6. Em um prompt de comandos com privilégios elevados, execute o seguinte comando para salvar o log em um arquivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digite o seguinte comando para exportar o arquivo ETL para um arquivo legível no diretório atual (provavelmente a pasta base ou a pasta% WINDIR% \\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra o arquivo **Summary. txt** e o arquivo **dumpfile. xml** (você pode abri-los no Microsoft Excel para exibir mais facilmente os detalhes completos do log). Procure eventos que incluem **falha** ou **falha**; Você pode ignorar com segurança as linhas que incluem o nome de evento **desconhecido** .

## <a name="more-information"></a>Mais informações

* [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)