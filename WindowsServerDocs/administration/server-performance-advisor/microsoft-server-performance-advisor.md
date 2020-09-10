---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.topic: article
ms.openlocfilehash: bd359e71cfb48ecd8aab24a8538369622dd1d271
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627707"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Baixe o Microsoft Server performance Advisor (SPA) para ajudar a diagnosticar problemas de desempenho em uma implantação do Windows Server 2012 R2, do Windows Server 2012, do Windows Server 2008 R2 ou do Windows Server 2008. O SPA gera gráficos e relatórios de diagnóstico abrangentes e fornece recomendações para ajudá-lo a analisar rapidamente os problemas e desenvolver ações corretivas.

-   [Visão geral do supervisor de desempenho do servidor](#bkmk-aboutspa)

-   [Baixar o supervisor de desempenho do servidor](#bkmk-downloadspa)

-   [Guia do usuário do Server Performance Advisor](server-performance-advisor-users-guide.md)

-   [Guia de desenvolvimento do Pacote do Server Performance Advisor](server-performance-advisor-pack-development-guide.md)

## <a name="overview-of-server-performance-advisor"></a><a href="" id="bkmk-aboutspa"></a>Visão geral do supervisor de desempenho do servidor

O Server performance Advisor é composto de duas partes, da estrutura SPA e dos pacotes do supervisor do SPA.

### <a name="the-server-performance-advisor-framework"></a>A estrutura do supervisor de desempenho do servidor

O mecanismo que é responsável por coletar dados que são designados pelos pacotes do Advisor, gravando os dados coletados em um banco de Microsoft SQL Server, criando um ambiente amigável para executar scripts para os pacotes do supervisor de SPA e mostrando os relatórios finais. Você só precisa instalar a estrutura SPA no console do SPA. O console do SPA pode ser instalado em um computador autônomo para acessar remotamente os servidores em teste ou ser instalado em um servidor em teste.

### <a name="server-performance-advisor-packs"></a>Pacotes do Server Performance Advisor

Os pacotes do supervisor de SPA são o centro de todas as regras de ajuste, que consistem em uma série de metadados e arquivos de script SQL. O SPA é fornecido com os seguintes pacotes do Advisor:

-   O pacote do orientador do sistema operacional principal analisa o desempenho das funções gerais dos sistemas operacionais, independentemente das funções de servidor especializadas.

-   O pacote do supervisor do IIS (servidor de informações da Internet) rastreia o desempenho do IIS.

-   O pacote do supervisor do Hyper-V analisa o desempenho geral da função de servidor do Hyper-V.

    **Observação** O pacote do supervisor do Hyper-V não analisa sistemas operacionais convidados.



-   O pacote Advisor do Active Directory analisa o desempenho geral da função do Active Directory.

O SPA também oferece um modelo extensível para desenvolvedores que não são da Microsoft para escreverem pacotes do Advisor de acordo com suas necessidades.

**Observação** O SPA não pode entender todos os contextos de cenário de hardware e de usuário. Você deve usar as recomendações fornecidas pela ferramenta para ajudá-lo a tomar decisões e entender as consequências das possíveis alterações feitas nos servidores.



## <a name="download-server-performance-advisor"></a><a href="" id="bkmk-downloadspa"></a>Baixar o supervisor de desempenho do servidor


Use os links a seguir para baixar o Server performance Advisor para Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008:

-   [Supervisor de desempenho de servidor da Microsoft 3,1 (32 bits)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Supervisor de desempenho de servidor da Microsoft 3,1 (64 bits)](https://go.microsoft.com/fwlink/p/?linkid=327752)

Você pode extrair os arquivos no arquivo CAB usando os seguintes comandos:

-   para a versão x86: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   para a versão x64: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Cuidado** Quando você extrai o arquivo. cab, o SPA deve preservar a estrutura de diretórios hierárquica para funcionar corretamente. Dependendo das ferramentas de CAB instaladas no servidor, a extração pode resultar em uma estrutura de diretório não operacional. Para manter a estrutura hierárquica do diretório, você pode usar uma ferramenta do utilitário de extração do CAB que extrai uma estrutura de diretório de arquivos.

Se a ferramenta de extração de CAB extraiu corretamente os arquivos, as subpastas serão exibidas automaticamente na pasta de destino de extração.

### <a name="spa-prerequisites"></a>Pré-requisitos de SPA

O console do SPA requer que o seguinte software esteja instalado:

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 ou Microsoft SQL Server 2008 R2 SP1

Versões mais recentes podem ser compatíveis. Qualquer incompatibilidade de produto conhecida com o console do SPA será observada.
