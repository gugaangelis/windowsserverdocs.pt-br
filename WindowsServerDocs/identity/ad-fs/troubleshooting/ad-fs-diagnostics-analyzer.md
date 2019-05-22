---
title: Analisador de diagnóstico do AD FS ajuda
description: Este documento descreve o analisador de diagnóstico Ajuda do AD FS e como ele pode realizar o basic verifica usando o diagnóstico do AD FS módulo do PowerShell.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8d9acd1adcb8d9566b154abfef940e21609a6684
ms.sourcegitcommit: 4ff3d00df3148e4bea08056cea9f1c3b52086e5d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64773377"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analisador de diagnóstico do AD FS ajuda

O AD FS tem várias configurações que dão suporte a ampla variedade de funcionalidades que ele fornece para desenvolvimento de aplicativos e de autenticação. Durante a solução de problemas, é recomendável para garantir que todas as configurações do AD FS estão configuradas corretamente. Fazer uma verificação manual dessas configurações, às vezes, pode ser demorado. Analisador de diagnóstico Ajuda do AD FS pode ajudar a executar as verificações básicas usando o módulo ADFSToolbox PowerShell. Depois de executar as verificações, a Ajuda do AD FS fornece o [analisador de diagnóstico](https://aka.ms/adfsdiagnosticsanalyzer) para ajudá-lo a visualizar os resultados facilmente e oferecer as etapas de correção.

Concluir a operação leva 3 etapas simples:

1. Configurar o módulo ADFSToolbox no servidor primário do AD FS ou servidor WAP
2. Execute o diagnóstico e carregue o arquivo de Ajuda do AD FS
3. Exibir análise de diagnóstico e resolva os problemas

Vá para [analisador de diagnóstico Ajuda do AD FS (https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer) para iniciar a solução de problemas.

![Ferramenta do analisador de diagnóstico do AD FS na Ajuda do AD FS](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Etapa 1: Configurar o módulo ADFSToolbox no servidor AD FS

Para executar o [analisador de diagnóstico](https://aka.ms/adfsdiagnosticsanalyzer), você deve instalar o módulo do ADFSToolbox PowerShell. Se o servidor do AD FS tiver conectividade com a internet, você pode instalar o módulo ADFSToolbox diretamente da Galeria do PowerShell. No caso de nenhuma conectividade com a internet, clone o repositório do GitHub para a instalação manual.

![Analisador de diagnóstico do AD FS - instalação](media/ad-fs-diagonostics-analyzer/step1.png)

### <a name="setup-using-powershell-gallery"></a>Configuração usando a Galeria do PowerShell

Se o servidor do AD FS tiver conectividade com a internet, é recomendável instalar o módulo de ADFSToolbox diretamente da Galeria do PowerShell usando os comandos do PowerShell abaixo.

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```
### <a name="setup-manually-by-cloning-the-repository"></a>Configurar manualmente por meio da clonagem do repositório

O módulo ADFSToolbox pode ser instalado manualmente do GitHub diretamente. Siga as instruções abaixo para clonagem do repositório e instalar o módulo ADFSToolbox no servidor do AD FS.

1. Baixe o [repositório](https://github.com/Microsoft/adfsToolbox/archive/master.zip)
2. Descompacte o arquivo baixado e copie a pasta adfsToolbox mestre para % SYSTEMDRIVE %\\Program Files\\WindowsPowerShell\\módulos\\.
3. Importe o módulo do PowerShell. Em uma janela elevada do PowerShell, execute o seguinte:

   ```powershell
    Import-Module ADFSToolbox -Force
   ```

## <a name="step-2-execute-the-diagnostics-and-upload-the-file-to-ad-fs-help"></a>Etapa 2: Execute o diagnóstico e carregue o arquivo de Ajuda do AD FS

Um único comando pode ser usado para executar convenientemente os testes de diagnóstico em todos os servidores do AD FS no farm. O módulo do PowerShell usará as sessões remotas do PS para executar os testes de diagnóstico em diferentes servidores no farm.

```powershell
    Export-AdfsDiagnosticsFile [-adfsServers <list of servers>]
```

Em um farm do AD FS Server 2016, o comando lerá a lista de servidores do AD FS da configuração do AD FS. Os testes de diagnóstico são tentados, em seguida, em cada servidor na lista. Se a lista de servidores do AD FS não está disponível (exemplo 2012R2), em seguida, os testes são executados no computador local. Para especificar uma lista de servidores em relação ao qual os testes devem ser executadas, use o **adfsServers** argumento para fornecer uma lista de servidores. Um exemplo é fornecido abaixo

```powershell
    Export-AdfsDiagnosticsFile -adfsServers @("sts1.contoso.com", "sts2.contoso.com", "sts3.contoso.com")
```

O resultado é um arquivo JSON que é criado no mesmo diretório onde o comando foi executado. O nome do arquivo é ADFSDiagnosticsFile -\<carimbo de hora\>. Um nome de arquivo de exemplo é ADFSDiagnosticsFile 20180716124030.

Na etapa 2 na [ https://aka.ms/adfsdiagnosticsanalyzer ](https://aka.ms/adfsdiagnosticsanalyzer) use o navegador de arquivos para selecionar o arquivo de resultado para carregar.

![Ferramenta de analisador de diagnóstico do AD FS - executar e carregue os resultados](media/ad-fs-diagonostics-analyzer/step2.png)

Clique em **carregar** para concluir o carregamento e mover para a próxima etapa.


Ao entrar com uma conta da Microsoft, suporte do diagnóstico de seu resultados podem ser salvas em um momento posterior de visualização e podem ser enviados à Microsoft. Se a qualquer momento em que você abrir um caso de suporte, a Microsoft será capaz de exibir os resultados do analisador de diagnóstico e ajudar a solucionar seu problema mais rapidamente.

![Ferramenta de analisador de diagnóstico do AD FS - entrar](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-3-view-diagnostics-analysis-and-resolve-any-issues"></a>Etapa 3: Exibir análise de diagnóstico e resolva os problemas

Há quatro seções dos resultados de teste:

1. Falha: Esta seção contém a lista de testes que falharam. Cada resultado é composto de:
2. Aviso: Esta seção contém uma lista de testes que resultaram em um aviso. Esses problemas serão provavelmente não resultará em quaisquer problemas em torno de autenticação em uma escala mais ampla, mas devem ser resolvidos o mais rápido possível.
3. Não aplicável: Esta seção contém a lista de testes que não foram executadas porque eles não eram aplicáveis para o servidor específico no qual o comando estava em execução.
4. Passados: Esta seção contém a lista de testes que passado e nenhum item de ação do usuário.

![Ferramenta de analisador de diagnóstico do AD FS - lista de resultados de teste](media/ad-fs-diagonostics-analyzer/step3a_v2.png) cada resultado de teste é exibido com detalhes que descrevem o teste e a resolução de etapas:

1. Nome do teste: Nome do teste que foi executado
2. Detalhes: Descrição da operação geral que foi executada durante o teste
3. Etapas de resolução: As etapas sugeridas para resolver o problema realçado pelo teste

![Ferramenta do analisador de diagnóstico do AD FS - resolução de falhas](media/ad-fs-diagonostics-analyzer/step3b_v2.png)

## <a name="next"></a>Próximo

- [Use guias de troublehshooting Ajuda do AD FS](https://aka.ms/adfshelp/troubleshooting )
- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
