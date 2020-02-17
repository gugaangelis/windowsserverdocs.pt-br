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
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394384"
---
# <a name="troubleshoot-user-profiles-with-events"></a>Solucionar problemas de perfis de usuário com eventos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server (Canal Semestral).

Este tópico discute como solucionar problemas de carregamento e descarregamento de perfis de usuário usando eventos e logs de rastreamento. As seções a seguir descrevem como usar os três logs de eventos que registram informações de perfil do usuário.

## <a name="step-1-checking-events-in-the-application-log"></a>Etapa 1: Como verificar eventos no Log do aplicativo

A primeira etapa na solução de problemas com o carregamento e o descarregamento de perfis do usuário (incluindo perfis de usuários móveis) é usar o Visualizador de Eventos para examinar os eventos de aviso e de erro que o Serviço de Perfil do Usuário registra no Log do aplicativo.

Veja como ver eventos de Serviços de Perfil do Usuário no Log do aplicativo:

1. Inicie o Visualizador de Eventos. Para fazer isso, abra o **Painel de Controle**, selecione **Sistema e Segurança** e, na seção **Ferramentas Administrativas**, selecione **Exibir logs de eventos**. A janela Visualizador de Eventos é aberta.
2. Na árvore de console, primeiro navegue até **Logs do Windows** e então para **Aplicativo**.
3. No painel Ações, selecione **Filtrar Log Atual**. A caixa de diálogo Filtrar Log Atual é aberta.
4. Na caixa **Origens de evento**, marque as caixas de seleção **Serviço de Perfis de Usuário** e, em seguida, selecione **OK**.
5. Examine a listagem de eventos, prestando atenção especial aos eventos de erro.
6. Ao encontrar eventos dignos de nota, selecione o link de Ajuda Online do Log de Eventos para exibir informações adicionais e procedimentos de solução de problemas.
7. Para executar mais solução de problemas, observe a data e a hora de eventos dignos de nota e examine o log operacional (conforme descrito na Etapa 2) para ver detalhes sobre o que o serviço de perfil do usuário estava fazendo por volta do horário dos eventos de Erro ou Aviso.

>[!NOTE]
>Você pode ignorar com segurança o evento 1530 do serviço de perfil do usuário "O Windows detectou que o arquivo do Registro ainda está em uso por outros aplicativos ou serviços".

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>Etapa 2: Exibir o log operacional para o Serviço de Perfil do Usuário

Se você não puder resolver o problema usando apenas o Log do aplicativo, use o procedimento a seguir para ver os eventos do Serviço de Perfil do Usuário no log operacional. Esse log mostra alguns dos funcionamentos internos do serviço e pode ajudar a detectar o local em que o problema está acontecendo no processo de carga e descarga do perfil.

O log de Aplicativos do Windows e o log operacional do Serviço de Perfil do Usuário estão habilitados por padrão em todas as instalações do Windows.

Veja como ver o log operacional para o Serviço de Perfil do Usuário:

1. Na árvore de console do Visualizador de Eventos, navegue até **Logs de Aplicativos e Serviços**, **Microsoft**, **Windows**, **Serviço de Perfil do Usuário** e **Operacional**.
2. Examine os eventos que ocorreram por volta do horário dos eventos de Erro ou Aviso que você anotou no Log do aplicativo.

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>Etapa 3: Habilitar e exibir logs analíticos e de depuração

Se precisar de mais detalhes do que o log operacional fornece, você poderá habilitar os logs analíticos e de depuração no computador afetado. Esse nível de registro em log é muito mais detalhado e deve ser desabilitado, exceto ao tentar solucionar problemas.

Veja como habilitar e ver logs analíticos e de depuração:

1. No painel **Ações** do Visualizador de Eventos, selecione **Exibir** e, em seguida, selecione **Mostrar Logs Analíticos e de Depuração**.
2. Navegue até **Logs de Aplicativos e Serviços**, depois **Microsoft**, **Windows**, **Serviço de Perfil do Usuário** e **Diagnóstico**.
3. Selecione **Habilitar Log** e, em seguida, selecione **Sim**. Isso habilita o Log de diagnóstico, que iniciará o registro em log.
4. Se você precisar de informações ainda mais detalhadas, confira a [Etapa 4: Como criar e decodificar um rastreamento](#step-4-creating-and-decoding-a-trace) para obter mais informações sobre como criar um log de rastreamento.
5. Quando você terminar a solução de problemas, navegue até o log de **Diagnóstico**, selecione **Desabilitar Log**, selecione **Exibir** e desmarque a caixa de seleção **Mostrar Logs Analíticos e de Depuração** para ocultar o registro em log analítico e de depuração.

## <a name="step-4-creating-and-decoding-a-trace"></a>Etapa 4: Como criar e decodificar um rastreamento

Se você não puder resolver o problema usando eventos, poderá criar um log de rastreamento (um arquivo ETL) enquanto reproduz o problema e, em seguida, decodificá-lo usando símbolos públicos do servidor de símbolos da Microsoft. Os logs de rastreamento fornecem informações muito específicas sobre o que o Serviço de Perfil do Usuário está fazendo e pode ajudar a identificar em que local a falha ocorreu.

A melhor estratégia ao usar o rastreamento de ETL é primeiro capturar o menor log possível. Depois da decodificação do log, pesquise falhas no log.

Veja como criar e decodificar um rastreamento para o Serviço de Perfil do Usuário:

1. Faça logon no computador em que o usuário está enfrentando problemas, usando uma conta que é membro do grupo de Administradores local.
2. Em um prompt de comandos com privilégios elevados, insira os seguintes comandos, em que *\<Caminho\>* é o caminho para uma pasta local que você criou anteriormente, por exemplo C:\\logs:
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. Na tela iniciar, selecione o nome de usuário e, em seguida, selecione **Alternar conta**, cuidando para não fazer logoff do administrador. Se você estiver usando Área de Trabalho Remota, feche a sessão de Administrador para estabelecer a sessão do usuário.
4. Reproduza o problema. O procedimento para reproduzir o problema normalmente é entrar como o usuário que está enfrentando o problema, desconectar o usuário ou ambos.
5. Depois de reproduzir o problema, faça logon novamente como administrador local.
6. Em um prompt de comandos com privilégios elevados, execute o seguinte comando para salvar o log em um arquivo ETL:
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. Digite o seguinte comando para exportar o arquivo ETL para um arquivo legível no diretório atual (provavelmente a pasta base ou a pasta %WINDIR%\\System32):
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. Abra o arquivo **Summary.txt** e o arquivo **Dumpfile.xml** (você pode abri-los no Microsoft Excel para ver mais facilmente os detalhes completos do log). Procure eventos que incluam **falha** ou **falhou**; você pode ignorar com segurança as linhas que incluem o nome de evento **Desconhecido**.

## <a name="more-information"></a>Mais informações

* [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)