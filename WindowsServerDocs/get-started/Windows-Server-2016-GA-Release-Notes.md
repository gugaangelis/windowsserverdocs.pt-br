---
title: 'Notas de versão: problemas importantes no Windows Server 2016'
description: Resume os problemas críticos que exigem solução alternativa para evitar falhas, congelamento, falha de instalação e perda de dados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 5a25a9152298a38ad77a377a87b71917ee586947
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507709"
---
# Notas de versão: problemas importantes no Windows Server 2016

>Aplica-se a: Windows Server 2016

Essas notas de versão resumem os problemas mais importantes no sistema operacional do Windows Server&reg; 2016, inclusive formas de evitar ou solucionar problemas, se conhecidos. Para saber mais sobre alterações planejadas, novos recursos e correções desta versão, confira [Novidades no Windows Server 2016](what-s-new-in-windows-server-2016.md) e os comunicados das equipes de recursos específicos. Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server 2016.  

Este documento é atualizado continuamente. À medida que são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas, assim como novas soluções e correções assim que estiverem disponíveis.  

## Express atualizações disponíveis a partir de novembro de 2018 (novo)

A partir de novembro de 2018 "Atualizar terça-feira" atualização, Windows novamente publicará [Express atualizações](express-updates.md) para o Windows Server 2016. Se você estiver usando o WSUS e o System Center Configuration Manager (SCCM) você verá mais uma vez dois pacotes para a atualização do Windows Server 2016: uma atualização completa e uma atualização Express. Se você quiser usar Express para seus ambientes de servidor, você precisa confirmar que o servidor teve uma atualização completa desde novembro de 2017 (KB # 4048953) para garantir que a atualização Express instalado corretamente. Se você tentar uma atualização Express em um servidor que ainda não foram atualizado desde a atualização de 11B 2017 (KB # 4048953), você verá falhas repetidas que consomem largura de banda e recursos da CPU em um loop infinito. Se você receber para esse cenário, pare enviando a atualização Express e, em vez disso, por push uma atualização completa recente para interromper o loop de falha.  

## Opção de instalação do Server Core
[comment]: # (ID: 370; Remetente: amason; estado: aprovado)  
Ao instalar o Windows Server 2016 usando a opção de instalação Server Core, o spooler de impressão é instalado e iniciado por padrão, mesmo quando a função de servidor de impressão não é instalada.

Para evitar isso, após a primeira inicialização, desabilite o spooler de impressão.


## Contêineres  

[comment]: # (ID: 371; Remetente: taylorb; estado: aprovado)  
- Antes de usar contêineres, instale a [Atualização de pilha de manutenção para Windows 10 versão 1607: 23 de agosto de 2016](https://support.microsoft.com/en-us/kb/3176936) ou quaisquer uma das atualizações mais recentes disponíveis. Caso contrário, vários problemas podem ocorrer, inclusive falhas de criação, inicialização ou execução de contêineres, e erros semelhantes a "Falha de CreateProcess no Win32: o servidor RPC não está disponível".

[comment]: # (ID: 373; Remetente: plang; estado: aprovado)  
- O provedor NanoServerPackage OneGet não funciona em contêineres do Windows. Para solucionar esse problema, use Find-NanoServerPackage e Save-NanoServerPackage em um computador diferente (não um contêiner) para baixar o pacote necessário. Em seguida, copie os pacotes no contêiner e instale-os.

## Device Guard
[comment]: # (ID: 369; Remetente: nirb; estado: aprovado)
Se você usar a proteção baseada em virtualização de integridade de código ou máquinas virtuais blindadas (que usam a proteção baseada em virtualização de integridade de código), fique ciente de que essas tecnologias podem ser incompatíveis com alguns dispositivos e aplicativos. Você deve testar essas configurações no laboratório antes de habilitar os recursos em sistemas de produção. Deixar de fazer isso pode resultar em erros de perda ou parar de dados inesperados.

## Microsoft Exchange
[comment]: # (ID: 375; Remetente: wgries; estado: aprovado)
Se você tentar executar o Microsoft Exchange 2016 CU3 no Windows Server 2016, encontrará erros no processo de host do IIS W3WP.exe. No momento, não há uma solução alternativa para esse problema. Você deve adiar a implantação do Exchange 2016 CU3 no Windows Server 2016 até que uma correção com suporte esteja disponível.

## Ferramentas de Administração de Servidor Remoto (RSAT)
[comment]: # (ID: 374; Remetente: ryanpu; estado: aprovado)
Se você estiver executando uma versão do Windows 10 mais antiga que a Atualização de Aniversário, e estiver usando o Hyper-V e máquinas virtuais com um Trusted Platform Module virtual habilitado (incluindo máquinas virtuais blindadas) e, em seguida, instalar a versão do RSAT fornecida para Windows Server 2016, tentativas de iniciar as máquinas virtuais falharão.

Para evitar isso, atualize o computador cliente para a Atualização de Aniversário do Windows 10 (ou posterior) antes de instalar o RSAT. Se isso já tiver ocorrido, desinstale o RSAT, atualize o cliente para a Atualização de Aniversário do Windows 10 e, em seguida, reinstale o RSAT.


## Máquinas virtuais blindadas
[comment]: # (ID: 369; Remetente: nirb; estado: aprovado)  
- Certifique-se de que você instalou todas as atualizações disponíveis antes de implantar máquinas de virtuais blindadas em produção.

- Se você usar a proteção baseada em virtualização de integridade de código ou máquinas virtuais blindadas (que usam a proteção baseada em virtualização de integridade de código), fique ciente de que essas tecnologias podem ser incompatíveis com alguns dispositivos e aplicativos. Você deve testar essas configurações no laboratório antes de habilitar os recursos em sistemas de produção. Deixar de fazer isso pode resultar em erros de perda ou parar de dados inesperados.


## Menu Iniciar
[comment]: # (ID: 372; Remetente: samli; estado: aprovado)
Esse problema afeta o Windows Server 2016 instalado com a opção Servidor com Experiência Desktop.

Se instalar qualquer aplicativo que adicione itens de atalho dentro de uma pasta no menu Iniciar, os atalhos não funcionarão até que você faça logoff e logon novamente.



Volte para a página principal do [Windows Server 2016](Windows-Server-2016.md).

## Desempenho do Storport
Alguns sistemas podem apresentar um desempenho de armazenamento reduzido ao executar uma nova instalação do Windows Server 2016 em comparação ao Windows Server 2012 R2.Foram feitas algumas alterações durante o desenvolvimento do Windows Server 2016 para melhorar a segurança e a confiabilidade da plataforma. Algumas dessas alterações, como habilitar o Windows Defender por padrão, resultam em caminhos mais longos de E/S que podem reduzir o desempenho de E/S em determinadas cargas de trabalho e padrões. A Microsoft não recomenda desabilitar o Windows Defender, já que ele é uma importante camada de proteção para seus sistemas.  

## Direitos autorais  
Este documento é fornecido "na condição em que se encontra". As informações e visualizações apresentadas neste documento, incluindo URL e outras referências a sites, estão sujeitas a alterações sem prévio aviso.  

Este documento não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft. Você pode copiar e usar este documento para fins de referência interna.  

&copy;2016 Microsoft Corporation. Todos os direitos reservados.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server são marcas registradas ou marcas comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.  

Este produto contém software de filtro gráfico; este software baseia-se, em parte, no trabalho do Independent JPEG Group.  


1.0  
