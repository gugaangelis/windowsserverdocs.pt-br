---
title: Analisador de diagnóstico do AD FS ajuda
description: Este documento descreve AD FS o analisador de diagnóstico de ajuda e como ele pode executar as verificações básicas usando o módulo do PowerShell de diagnóstico de AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.openlocfilehash: e6d6063d3d01cda48a662160c9ad661169371677
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956193"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analisador de diagnóstico do AD FS ajuda

AD FS tem várias configurações que dão suporte à ampla variedade de funcionalidades que ele fornece para a autenticação e o desenvolvimento de aplicativos. Durante a solução de problemas, é recomendável garantir que todas as configurações de AD FS estejam configuradas corretamente. Às vezes, fazer uma verificação manual dessas configurações pode ser demorado. AD FS o help Diagnostics Analyzer pode ajudar a executar as verificações básicas usando o módulo ADFSToolbox do PowerShell. Depois de executar as verificações, AD FS ajuda fornece o [analisador de diagnóstico](https://aka.ms/adfsdiagnosticsanalyzer) para ajudá-lo a visualizar facilmente os resultados e a oferecer etapas de correção.

A operação completa leva 3 etapas simples:

1. Configurar o módulo ADFSToolbox no servidor de AD FS primário ou no servidor WAP
2. Execute o diagnóstico e carregue o arquivo para AD FS ajuda
3. Exibir análise de diagnóstico e resolver problemas

Vá para [AD FS Help Diagnostics Analyzer ( https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer) para iniciar a solução de problemas.

![AD FS ferramenta Analisador de diagnóstico na ajuda do AD FS](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Etapa 1: configurar o módulo ADFSToolbox no servidor AD FS

Para executar o [Diagnostics Analyzer](https://aka.ms/adfsdiagnosticsanalyzer), você deve instalar o módulo do PowerShell do ADFSToolbox. Se o servidor de AD FS tiver conectividade com a Internet, você poderá instalar o módulo ADFSToolbox diretamente da galeria do PowerShell. No caso de nenhuma conectividade com a Internet, você pode instalá-la manualmente.

[!WARNING]
Se você estiver usando AD FS 2,1 ou inferior, deverá instalar a versão 1.0.13 do ADFSToolbox. O ADFSToolbox não dá mais suporte ao AD FS 2,1 ou inferior nas versões mais recentes.

![Analisador de diagnóstico de AD FS-instalação](media/ad-fs-diagonostics-analyzer/step1_v2.png)

### <a name="setup-using-powershell-gallery"></a>Configurar usando a galeria do PowerShell

Se o servidor de AD FS tiver conectividade com a Internet, é recomendável instalar o módulo ADFSToolbox diretamente da galeria do PowerShell usando os comandos do PowerShell fornecidos abaixo.

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```

### <a name="setup-manually"></a>Configurar manualmente

O módulo ADFSToolbox deve ser copiado manualmente para os servidores AD FS ou WAP. Siga as instruções abaixo.

1. Inicie uma janela do PowerShell com privilégios elevados em um computador que tenha acesso à Internet.
2. Instale o módulo da caixa de ferramentas AD FS.

    ```powershell
    Install-Module -Name ADFSToolbox -Force
    ```
3. Copie a pasta ADFSToolbox localizada `%SYSTEMDRIVE%\Program Files\WindowsPowerShell\Modules\` em seu computador local para o mesmo local em seu computador AD FS ou WAP.

4. Inicie uma janela do PowerShell com privilégios elevados no computador AD FS e execute o cmdlet a seguir para importar o módulo.

    ```powershell
    Import-Module ADFSToolbox -Force
    ```

## <a name="step-2-execute-the-diagnostics-cmdlet"></a>Etapa 2: executar o cmdlet de diagnóstico

![Ferramenta do analisador de diagnóstico de AD FS-executar e carregar resultados](media/ad-fs-diagonostics-analyzer/step2_v2.png)

Um único comando pode ser usado para executar convenientemente os testes de diagnóstico em todos os servidores de AD FS no farm. O módulo do PowerShell usará sessões de PS remotas para executar os testes de diagnóstico em diferentes servidores no farm.

```powershell
    Export-AdfsDiagnosticsFile [-ServerNames <list of servers>]
```

Em um servidor 2016 ou superior AD FS farm, o comando lerá a lista de servidores AD FS da configuração AD FS. Os testes de diagnóstico são então tentados em relação a cada servidor na lista. Se a lista de servidores de AD FS não estiver disponível (exemplo 2012R2), os testes serão executados no computador local. Para especificar uma lista de servidores em que os testes devem ser executados, use o argumento **servernames** para fornecer uma lista de servidores. Um exemplo é fornecido abaixo

```powershell
    Export-AdfsDiagnosticsFile -ServerNames @("adfs1.contoso.com", "adfs2.contoso.com")
```

O resultado é um arquivo JSON que é criado no mesmo diretório em que o comando foi executado. O nome do arquivo é AdfsDiagnosticsFile- \<timestamp\> . Um nome de arquivo de exemplo é AdfsDiagnosticsFile-07312019-184201.jsem.

## <a name="step-3-upload-the-diagnostics-file"></a>Etapa 3: carregar o arquivo de diagnóstico

Na etapa 3, em [https://aka.ms/adfsdiagnosticsanalyzer](https://aka.ms/adfsdiagnosticsanalyzer) usar o navegador de arquivos para selecionar o arquivo de resultado a ser carregado.

Clique em **carregar** para concluir o carregamento.

Ao entrar com um conta Microsoft, os resultados do diagnóstico podem ser salvos para um ponto de exibição posterior e podem ser enviados ao suporte da Microsoft. Se a qualquer momento você abrir um caso de suporte, a Microsoft poderá exibir os resultados do analisador de diagnóstico e ajudar a solucionar o problema mais rapidamente.

![Ferramenta AD FS Diagnostics Analyzer – entrar](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-4-view-diagnostics-analysis-and-resolve-any-issues"></a>Etapa 4: exibir a análise de diagnóstico e resolver quaisquer problemas

Há cinco seções dos resultados do teste:

1. Falha: Esta seção contém uma lista de testes que falharam. Cada resultado é composto por:
2. Aviso: Esta seção contém uma lista de testes que resultaram em um aviso. Esses problemas provavelmente não resultarão em problemas relacionados à autenticação em uma escala mais ampla, mas devem ser abordados no início.
3. Aprovado: Esta seção contém a lista de testes que passaram e não tem nenhum item de ação para o usuário.
4. Não executado: Esta seção contém a lista de testes que não puderam ser executados devido a informações ausentes.
5. Não aplicável: Esta seção contém a lista de testes que não foram executados porque eles não são aplicáveis para o servidor específico no qual o comando estava em execução.

![Ferramenta do analisador de diagnóstico de AD FS-lista de resultados de teste ](media/ad-fs-diagonostics-analyzer/step3a_v3.png) cada resultado de teste é exibido com detalhes que descrevem o teste e a resolução das etapas:

1. Nome do teste: nome do teste que foi executado
2. Descrição: uma descrição do teste.
3. Detalhes: Descrição da operação geral que foi executada durante o teste
4. Etapas de resolução: as etapas sugeridas para resolver o problema realçado pelo teste

![Ferramenta Analisador de diagnóstico de AD FS-resolução de falha](media/ad-fs-diagonostics-analyzer/step3b_v3.png)

## <a name="next"></a>Avançar

- [Usar AD FS guias de troublehshooting de ajuda](https://aka.ms/adfshelp/troubleshooting )
- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
