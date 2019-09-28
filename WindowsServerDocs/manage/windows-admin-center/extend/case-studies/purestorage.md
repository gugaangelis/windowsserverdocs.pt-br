---
title: Estudo de caso do SDK do centro de administração do Windows – armazenamento puro
description: Estudo de caso do SDK do centro de administração do Windows – armazenamento puro
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c6ab75a2375368d7c87d9dffd6175eb611508dd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406955"
---
# <a name="pure-storage-extension"></a>Extensão de armazenamento pura

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Fornecimento de gerenciamento de matriz de ponta a ponta para o centro de administração do Windows 

O [armazenamento puro](https://www.purestorage.com/) fornece soluções de armazenamento de dados empresariais e totalmente flash que fornecem uma arquitetura centrada em dados para acelerar sua empresa para uma vantagem competitiva.  O puro é um parceiro Gold da Microsoft, certificado para Microsoft Windows Server e desenvolve integrações técnicas para as principais soluções da Microsoft, como Azure, Hyper-V, SQL Server, System Center, Windows PowerShell e Windows SMB. O puro anunciou recentemente uma visualização técnica de uma extensão que dá suporte à versão mais recente do centro de administração do Windows, que fornece uma exibição de painel único em produtos FlashArray puros.  A partir dessa extensão, os usuários são capacitados de uma ferramenta para realizar tarefas de monitoramento, exibir métricas de desempenho em tempo real e gerenciar os volumes e os iniciadores de armazenamento.

No início, quando o centro de administração do Windows era conhecido como "projeto Honolulu", ele viu de forma pura o valor de ser capaz de fornecer aos clientes e parceiros a capacidade de gerenciar vários FlashArrays de armazenamento puro a partir do único painel de vidro que o centro de administração do Windows fornece.

Quando começou a Pesquisar novamente o caso de uso com "Project Honolulu", eles perceberam imediatamente o potencial para fornecer uma experiência de gerenciamento unificada entre o centro de administração do Windows e o FlashArray. Com uma colaboração de forma pura com a equipe de engenharia do centro de administração do Windows, que ajudou a definir os detalhes de implementação para os recursos. O puro também foi capaz de fornecer comentários nos primeiros estágios do centro de administração do Windows e fazer contribuições para a equipe da Microsoft. 

![Extensão de armazenamento pura](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite> "nós integramos um conjunto de recursos que imita nossa interface da Web FlashArray para habilitar o gerenciamento direto no centro de administração do Windows. Nossos clientes e parceiros se beneficiarão de um único painel de vidro versus a necessidade de trabalhar com duas ferramentas de gerenciamento diferentes. Além dos benefícios de ponto único de gerenciamento, os clientes poderão gerenciar contextualmente os servidores do Windows que estão conectados à FlashArray. " </cite>
>
> --Barkz, diretor técnico da Microsoft Solutions & integração, armazenamento puro

Os recursos incluídos na extensão da solução de armazenamento puro incluem:
- Conectando-se a vários FlashArrays.
- Exibindo os detalhes do FlashArray, incluindo IOPs, largura de banda, latência, redução de dados e gerenciamento de espaço. Esses são todos os mesmos detalhes obtidos da GUI de gerenciamento do FlashArray.
- Exiba grupos de hosts configurados que são usados para habilitar o acesso de volume compartilhado para hosts do Windows Server e CSVs (volumes compartilhados clusterizados).
- Exibir hosts – todas as informações de conectividade estão disponíveis, incluindo nomes de host, IQNs (nome qualificado iSCSI) e WWNs (World Wide Names).
- Gerenciar volumes — isso inclui a capacidade de criar e destruir volumes. Depois que um volume é destruído, ele será colocado no Bucket de itens destruídos e você precisará erradicar da principal GUI de gerenciamento de FlashArray.
- Gerenciar iniciadores — esse é um dos recursos mais interessantes que fornece contexto para os servidores individuais gerenciados pela implantação do centro de administração do Windows. Você pode exibir os discos conectados (volumes) para servidores Windows individuais, verificar se o MPIO (Multipath-IO) está instalado/configurado e criar/montar novos volumes.

Foi criado um [vídeo de demonstração](https://youtu.be/IFAeCAd6V2g) que mostra todos os recursos fornecidos pela extensão de solução de armazenamento pura. 

A captura de tela abaixo ilustra como exibir quais discos (volumes) estão conectados a um host do Windows Server específico. Além de exibir os detalhes de conectividade, verificamos se o multipath-IO está configurado.

![Extensão de armazenamento pura](../../media/extend-case-study-purestorage/purestorage-2.png)

Além de exibir os discos, novos volumes podem ser criados e montados imediatamente no host sem a necessidade de usar a ferramenta de gerenciamento de disco do Windows.

![Extensão de armazenamento pura](../../media/extend-case-study-purestorage/purestorage-3.png)

Desde a liberação de nossa Technical Preview, os comentários dos clientes coletados até o momento têm sido muito positivos e também nos forneceram informações sobre diferentes recursos para adicionar em versões futuras. 

Recursos adicionais:
- [Postagem de blog de anúncio de extensão de armazenamento puro](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- Podcast [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)
