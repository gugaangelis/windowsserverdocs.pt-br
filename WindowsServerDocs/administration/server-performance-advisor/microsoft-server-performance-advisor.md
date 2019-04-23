---
title: Microsoft Server Performance Advisor
description: Microsoft Server Performance Advisor
ms.assetid: 468ebcb3-9eaf-477c-ab10-e3f1b3ce63dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: ab124f3efabf2ac3ae8904157a81587c0c21ebb5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856327"
---
# <a name="microsoft-server-performance-advisor"></a>Microsoft Server Performance Advisor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Baixe o Microsoft Server Performance Advisor (SPA) para ajudar a diagnosticar problemas de desempenho em um Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou implantação do Windows Server 2008. SPA gera gráficos e relatórios de diagnóstico abrangentes e fornece recomendações para ajudá-lo rapidamente a analisem problemas e desenvolvem ações corretivas.

-   [Visão geral do Server Performance Advisor](#bkmk-aboutspa)

-   [Baixe o Server Performance Advisor](#bkmk-downloadspa)

-   [Guia do usuário das Supervisor de desempenho do servidor](server-performance-advisor-users-guide.md)

-   [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md)

## <a href="" id="bkmk-aboutspa"></a>Visão geral do Server Performance Advisor

Server Performance Advisor é composto de duas partes, a estrutura do SPA e os pacotes do SPA Advisor.

### <a name="the-server-performance-advisor-framework"></a>A estrutura do Server Performance Advisor

O mecanismo é responsável pela coleta de dados que são designados pelos pacotes de Supervisor, gravando os dados coletados em um banco de dados do Microsoft SQL Server, criando um ambiente amigável de área de TI para executar scripts para pacotes do SPA Advisor e mostrando os relatórios finais. Você só precisará instalar o framework do SPA no console do SPA. O console do SPA pode ser instalado em um computador autônomo para acessar os servidores em teste remotamente ou ser instalado em um servidor de teste.

### <a name="server-performance-advisor-packs"></a>Pacotes do Server Performance Advisor

Pacotes do SPA Advisor são o Centro de todas as regras de ajustes, que consistem em uma série de metadados e arquivos de script SQL. SPA é fornecido com os seguintes pacotes de Supervisor:

-   O pacote de Supervisor do sistema operacional do Core analisa o desempenho das funções gerais do sistema operacional, independentes de funções de servidor especializadas.

-   O pacote de Supervisor do servidor de informações da Internet (IIS) controla o desempenho do IIS.

-   O pacote de Supervisor do Hyper-V analisa o desempenho geral da função de servidor Hyper-V.

    **Observação** o pacote de Supervisor do Hyper-V não analisa os sistemas operacionais convidados.

     

-   O pacote de Supervisor do Active Directory analisa o desempenho geral da função de diretório Active Directory.

SPA também oferece um modelo extensível para desenvolvedores de terceiros gravar os pacotes de Supervisor para atender às suas necessidades.

**Observação** SPA não é possível entender todos os contextos de cenário de hardware e de usuário. Você deve usar as recomendações que são fornecidas pela ferramenta para ajudá-lo a tomar decisões e entenda as consequências de quaisquer possíveis alterações feitas aos servidores.

 

## <a href="" id="bkmk-downloadspa"></a>Baixe o Server Performance Advisor


Use os links a seguir para baixar o servidor de desempenho do Advisor para Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008:

-   [Microsoft Server Performance Advisor 3.1 (32 bits)](https://go.microsoft.com/fwlink/p/?linkid=327751)

-   [Microsoft Server Performance Advisor 3.1 (64 bits)](https://go.microsoft.com/fwlink/p/?linkid=327752)

Você pode extrair os arquivos no arquivo CAB, usando os comandos a seguir:

-   para x86 versão: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_x86.cab`

-   para o x64 versão: `extrac32.exe /e /a /l  d:\SPA   d:\SPA\SPAPlus\_amd64.cab`

**Cuidado** quando você extrai o arquivo. cab, o SPA deve preservar a estrutura de diretório hierárquico para funcionar corretamente. Dependendo das ferramentas CAB que estão instaladas no seu servidor, a extração pode resultar em uma estrutura de diretório não-operacional. Para manter a estrutura de diretório hierárquico, você pode usar uma ferramenta de utilitário de extração de CAB que extrai uma estrutura de diretório do arquivo.

Se a ferramenta de extração do CAB corretamente extraiu os arquivos, as subpastas aparecerão automaticamente na pasta de destino de extração.

### <a name="spa-prerequisites"></a>Pré-requisitos do SPA

O Console do SPA requer que o seguinte software está instalado:

-   Microsoft .NET Framework 4

-   SQL Server 2008 R2 Express SP1 ou Microsoft SQL Server 2008 R2 SP1

As versões mais recentes podem ser compatíveis. Quaisquer incompatibilidades conhecidas do produto com o Console do SPA serão observadas.
