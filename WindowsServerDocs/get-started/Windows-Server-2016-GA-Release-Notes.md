---
title: 'Notas de versão: problemas importantes no Windows Server 2016'
description: Resume os problemas críticos que exigem solução alternativa para evitar falhas, congelamento, falha de instalação e perda de dados.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: f4568e1781dbe385d8abe8a96f07841391506738
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822159"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Notas sobre a versão: Problemas importantes no Windows Server 2016

>Aplica-se a: Windows Server 2016

Essas notas sobre a versão resumem os problemas mais críticos no sistema operacional Windows Server 2016, incluindo formas de evitar ou solucionar os problemas, se conhecidos. Para saber mais sobre alterações planejadas, novos recursos e correções desta versão, confira [Novidades no Windows Server 2016](whats-new-in-windows-server-2016.md) e os comunicados das equipes de recursos específicos. Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server 2016.

Este documento é atualizado continuamente. À medida que são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas, assim como novas soluções e correções assim que estiverem disponíveis.

## <a name="express-updates-available-starting-in-november-2018-new"></a>Atualizações Expressas disponíveis a partir de novembro de 2018 (NOVO)

Na "atualização das terças-feiras" de novembro de 2018 em diante, o Windows publicará as [Atualizações expressas](express-updates.md) novamente para o Windows Server 2016. Se estiver usando o WSUS e o Configuration Manager, você verá mais uma vez dois pacotes para a atualização do Windows Server 2016: uma atualização completa e uma atualização expressa. Caso deseje usar a Expressa nos ambientes de servidor, você precisará confirmar que o servidor fez uma atualização completa desde novembro de 2017 (KB nº 4048953) para garantir que a atualização Expressa seja instalada corretamente. Se você tentar fazer uma atualização Expressa em um servidor que ainda não foi atualizado desde a atualização 11B de 2017 (KB nº 4048953), verá falhas repetidas que consomem a largura de banda e os recursos da CPU em um loop infinito. Caso você experimente esse cenário, pare de enviar a atualização Expressa por push e, em vez disso, envie uma atualização Completa recente por push para interromper o loop de falha.

## <a name="server-core-installation-option"></a>Opção de instalação do Server Core

[comment]: # (ID: 370; Remetente: amason; estado: aprovado)

Ao instalar o Windows Server 2016 usando a opção de instalação Server Core, o spooler de impressão é instalado e iniciado por padrão, mesmo quando a função de servidor de impressão não é instalada.

Para evitar isso, após a primeira inicialização, desabilite o spooler de impressão.

## <a name="containers"></a>Contêineres

[comment]: # (ID: 371; Remetente: taylorb; estado: aprovado)
- Antes de usar contêineres, instale a [Atualização da pilha de manutenção para o Windows 10 versão 1607: 23 de agosto de 2016](https://support.microsoft.com/kb/3176936) ou as atualizações mais recentes que estiverem disponíveis. Caso contrário, vários problemas poderão ocorrer, incluindo falhas na criação, na inicialização ou na execução de contêineres, bem como erros semelhantes a "Falha de CreateProcess no Win32: O servidor RPC não está disponível."

[comment]: # (ID: 373; Remetente: plang; estado: aprovado)
- O provedor NanoServerPackage OneGet não funciona em contêineres do Windows. Para solucionar esse problema, use Find-NanoServerPackage e Save-NanoServerPackage em um computador diferente (não um contêiner) para baixar o pacote necessário. Em seguida, copie os pacotes no contêiner e instale-os.

## <a name="device-guard"></a>Device Guard

[comment]: # (ID: 369; Remetente: nirb; estado: aprovado)
Se você usar a proteção baseada em virtualização de integridade de código ou máquinas virtuais blindadas (que usam a proteção baseada em virtualização de integridade de código), fique ciente de que essas tecnologias podem ser incompatíveis com alguns dispositivos e aplicativos. Você deve testar essas configurações no laboratório antes de habilitar os recursos em sistemas de produção. Deixar de fazer isso pode resultar em erros inesperados de perda ou interrupção de dados.

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID: 375; Remetente: wgries; estado: aprovado)
Se você tentar executar o Microsoft Exchange 2016 CU3 no Windows Server 2016, verá erros no processo de host do IIS W3WP.exe. No momento, não há uma solução alternativa para esse problema. Você deve adiar a implantação do Exchange 2016 CU3 no Windows Server 2016 até que uma correção compatível esteja disponível.

## <a name="remote-server-administration-tools-rsat"></a>Ferramentas de Administração de Servidor Remoto (RSAT)

[comment]: # (ID: 374; Remetente: ryanpu; estado: aprovado)
Se você estiver executando uma versão do Windows 10 mais antiga do que a Atualização de Aniversário e estiver usando o Hyper-V e máquinas virtuais com um Trusted Platform Module virtual habilitado (incluindo máquinas virtuais blindadas) e, em seguida, instalar a versão das Ferramentas de Administração de Servidor Remoto fornecidas para o Windows Server 2016, as tentativas de iniciar as máquinas virtuais falharão.

Para evitar isso, atualize o computador cliente para a Atualização de Aniversário do Windows 10 (ou posterior) antes de instalar o RSAT. Se isso já tiver ocorrido, desinstale o RSAT, atualize o cliente para a Atualização de Aniversário do Windows 10 e, em seguida, reinstale o RSAT.

## <a name="shielded-virtual-machines"></a>Máquinas virtuais blindadas

[comment]: # (ID: 369; Remetente: nirb; estado: aprovado)  
- Verifique se você instalou todas as atualizações disponíveis antes de implantar máquinas virtuais blindadas em produção.

- Se você usar a proteção baseada em virtualização de integridade de código ou máquinas virtuais blindadas (que usam a proteção baseada em virtualização de integridade de código), fique ciente de que essas tecnologias podem ser incompatíveis com alguns dispositivos e aplicativos. Você deve testar essas configurações no laboratório antes de habilitar os recursos em sistemas de produção. Deixar de fazer isso pode resultar em erros inesperados de perda ou interrupção de dados.

## <a name="start-menu"></a>Menu Iniciar

[comment]: # (ID: 372; Remetente: samli; estado: aprovado)
Esse problema afeta o Windows Server 2016 instalado com a opção de Servidor com Experiência Desktop.

Se você instalar qualquer aplicativo que adicione itens de atalho em uma pasta no menu **Iniciar**, os atalhos não funcionarão enquanto você não fizer logoff e logon novamente.

Volte para a página principal do [Windows Server 2016](Windows-Server-2016.md).

## <a name="storport-performance"></a>Desempenho do Storport

Alguns sistemas podem apresentar um desempenho de armazenamento reduzido ao executar uma nova instalação do Windows Server 2016 em comparação com o Windows Server 2012 R2.  Foram feitas várias alterações durante o desenvolvimento do Windows Server 2016 para melhorar a segurança e a confiabilidade da plataforma. Algumas dessas alterações, como a habilitação do Windows Defender por padrão, resultam em caminhos mais longos de E/S que podem reduzir o desempenho de E/S em determinadas cargas de trabalho e padrões. A Microsoft não recomenda que o Windows Defender seja desabilitado porque ele é uma camada importante de proteção para os sistemas.  

## <a name="copyright"></a>Direitos autorais

Este documento é fornecido "na condição em que se encontra". As informações e visualizações apresentadas neste documento, incluindo URL e outras referências a sites, estão sujeitas a alterações sem prévio aviso.  

Este documento não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft. Você pode copiar e usar este documento para fins de referência interna.  

&copy; 2016 Microsoft Corporation. Todos os direitos reservados.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server são marcas registradas ou marcas comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.  

Este produto contém um software de filtro gráfico, que é parcialmente baseado no trabalho do grupo Independent JPEG.  

1.0
