---
title: Estudo de caso do Windows Admin Center SDK - armazenamento pura
description: Estudo de caso do Windows Admin Center SDK - armazenamento pura
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: ebeec824f802f020d0ece17524ba43b1baeba893
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2019
ms.locfileid: "8995285"
---
# Extensão de armazenamento pura

## Fornecimento de gerenciamento de matriz de ponta a ponta para o Windows Admin Center 

[Armazenamento pura](https://www.purestorage.com/) fornece enterprise, soluções de armazenamento de dados de todos os flash que oferecem arquitetura centrados em dados para acelerar sua empresa para uma vantagem competitiva.  Puro é um Microsoft Gold Partner, certificados para Microsoft Windows Server e desenvolve integração técnica para principais soluções da Microsoft, como Azure, Hyper-V, SQL Server, o System Center, Windows PowerShell e Windows SMB. Puro anunciou recentemente uma visualização tech de uma extensão, dando suporte a versão mais recente do Windows Admin Center que fornece uma exibição de painel único em produtos FlashArray pura.  Dessa extensão, os usuários estão capacitados de uma ferramenta para realizar tarefas de monitoramento, exibir métricas de desempenho em tempo real e gerenciar iniciadores e volumes de armazenamento.

Logo no início, quando o Windows Admin Center era conhecido como "Project Honolulu", puro viu o valor de ser capaz de fornecer aos clientes e parceiros a capacidade de gerenciar vários pura FlashArrays de armazenamento do painel único que fornece o Windows Admin Center.

Quando puro iniciado pesquisando o caso de uso com "Project Honolulu" eles perceberam imediatamente o potencial para fornecer uma experiência de gerenciamento unificada entre o Windows Admin Center e FlashArray. Puro em estrita colaboração com a equipe de engenharia do Windows Admin Center, que ajudou a definir os detalhes de implementação para os recursos. Puro também foi capaz de fornecer comentários nos estágios iniciais do Windows Admin Center e faça contribuição para a equipe da Microsoft. 

![Extensão de armazenamento pura](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Integramos um conjunto de recursos que imita a interface da web FlashArray para habilitar o gerenciamento direto de dentro do Windows Admin Center. Nossos clientes e parceiros se beneficiará um único painel versus a necessidade de trabalhar com duas ferramentas de gerenciamento diferentes. Além de único ponto de gerenciamento benefícios clientes poderão contextualmente gerenciar servidores do Windows que estão conectados à FlashArray."</cite>
>
> – Barkz, diretor técnico soluções da Microsoft e integração, armazenamento pura

Os recursos que estão incluídos na extensão de solução de armazenamento pura incluem:
- Conexão aos vários FlashArrays.
- Exibir os detalhes de FlashArray, incluindo IOPs, largura de banda, latência, redução de dados e gerenciamento de espaço. Esses são todos os detalhes mesmo obtido a GUI de gerenciamento FlashArray.
- Exiba os grupos de host configurado que são usados para habilitar o acesso de volume compartilhado para hosts do Windows Server e Volumes Compartilhados do cluster (CSVs).
- Hosts de modo de exibição — Todas as informações de conectividade está disponível incluindo nomes de Host, iSCSI nome qualificado (IQNs) e World Wide Names (WWNs).
- Gerenciar Volumes — Isso inclui a capacidade de criar e destruir volumes. Depois que um volume é destruído será colocado no compartimento de memória itens Destroyed e você precisará Eradicate pela interface do usuário de gerenciamento de FlashArray principal.
- Gerenciar iniciadores — Isso é um dos recursos mais interessantes fornecer contexto para os servidores individuais que está sendo gerenciado pela implantação do Windows Admin Center. Você pode exibir os discos conectados (volumes) para servidores individuais do Windows, verifique se a e/s de vários caminhos (MPIO) é instalados e configurados e montagem/criar novos volumes.

Foi criada uma [demonstração de vídeo](https://youtu.be/IFAeCAd6V2g) que mostra todos os recursos que fornece a extensão de solução de armazenamento pura. 

A seguir captura de tela ilustra exibindo quais discos (volumes) estão conectados a um host específico do Windows Server. Além de exibir os detalhes de conectividade, verificamos se Multipath i / o é configurado.

![Extensão de armazenamento pura](../../media/extend-case-study-purestorage/purestorage-2.png)

Além de exibir os discos, novos volumes podem ser criados e montados imediatamente para o host sem ter que usar a ferramenta de gerenciamento de disco do Windows.

![Extensão de armazenamento pura](../../media/extend-case-study-purestorage/purestorage-3.png)

Desde lançando nosso Technical Preview, os comentários dos clientes coletados até agora têm sido muito positivos e também nos proporcionou insight recursos diferentes para adicionar em futuras versões. 

Recursos adicionais:
- [Postagem do blog de lançamento da extensão do pura armazenamento](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) podcast
