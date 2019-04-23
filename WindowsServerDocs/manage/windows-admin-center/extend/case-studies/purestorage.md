---
title: Estudo de caso SDK do Windows Admin Center - armazenamento puro
description: Estudo de caso SDK do Windows Admin Center - armazenamento puro
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849917"
---
# <a name="pure-storage-extension"></a>Extensão de armazenamento puro

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Fornecendo gerenciamento de matriz de ponta a ponta para Windows Admin Center 

[Armazenamento puro](https://www.purestorage.com/) fornece enterprise, soluções de armazenamento de dados de todos os flash que fornecem uma arquitetura centrados em dados para acelerar seus negócios como uma vantagem competitiva.  Puro é um Microsoft Gold Partner, certificados para o Microsoft Windows Server e desenvolve técnicas integrações de principais soluções da Microsoft, como o Azure, Hyper-V, o SQL Server, o System Center, o Windows PowerShell e o SMB do Windows. Puro anunciou recentemente uma visualização técnica de uma extensão com suporte a versão mais recente do Windows Admin Center que fornece uma exibição de painel único em produtos FlashArray puro.  Essa extensão de usuários são capacitados de uma ferramenta para realizar tarefas de monitoramento, exibir as métricas de desempenho em tempo real e gerenciar volumes de armazenamento e iniciadores.

Logo no início, quando o Windows Admin Center era conhecido como "Projeto Paulo", puro viu o valor de ser capaz de fornecer aos clientes e parceiros a capacidade de gerenciar vários FlashArrays armazenamento puro do único painel que fornece Windows Admin Center.

Quando puro iniciado pesquisando o caso de uso com "Projeto Paulo" eles perceberam imediatamente o potencial para fornecer uma experiência de gerenciamento unificado entre Windows Admin Center e FlashArray. Puro colaborado com a equipe de engenharia do Windows Admin Center, que ajudou a definir os detalhes de implementação para os recursos. Puro também era capaz de fornecer seus comentários nos primeiros estágios de Windows Admin Center e fazer contribuições à equipe da Microsoft. 

![Extensão de armazenamento puro](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Podemos ter integrado a um conjunto de recursos que imita a nossa interface da web FlashArray para habilitar o gerenciamento direto de dentro do Windows Admin Center. Nossos clientes e parceiros se beneficiarão de um único painel versus precise trabalhar com duas ferramentas de gerenciamento diferentes. Além do único ponto de gerenciamento de benefícios que os clientes poderão contextualmente gerenciar servidores Windows conectados para o FlashArray."</cite>
>
> – Barkz, diretor técnico de soluções da Microsoft e integração, armazenamento puro

Os recursos que estão incluídos na extensão de solução de armazenamento puro incluem:
- Conectando a vários FlashArrays.
- Exibindo os detalhes de FlashArray, incluindo IOPs, largura de banda, latência, redução de dados e gerenciamento de espaço. Esses são todos os detalhes mesmos que obter da GUI de gerenciamento FlashArray.
- Exiba grupos de host configurados que são usados para habilitar o acesso ao volume compartilhado para hosts do Windows Server e Volumes Compartilhados clusterizados (CSVs).
- Hosts do modo de exibição — Todas as informações de conectividade está disponível o incluindo nomes de Host iSCSI (IQN) do nome qualificado e World Wide Names (WWNs).
- Gerenciar Volumes – Isso inclui a capacidade de criar e destruir volumes. Depois que um volume é destruído serão colocado no bucket de itens destruídos e você precisará Eradicate da GUI de gerenciamento de FlashArray principal.
- Gerenciar iniciadores — Isso é um dos recursos mais interessantes, fornecendo contexto para servidores individuais que estão sendo gerenciadas pela implantação do Windows Admin Center. Você pode exibir os discos conectados (volumes) para servidores individuais do Windows, verifique se a e/s de vários caminhos (MPIO) está instalados e configurados e montagem/criar novos volumes.

Um [demonstração em vídeo](https://youtu.be/IFAeCAd6V2g) tiver sido criado que mostra todos os recursos que fornece a extensão de solução de armazenamento puro. 

A captura de tela abaixo ilustra exibindo quais discos (volumes) estão conectados a um host específico do Windows Server. Além de exibir os detalhes de conectividade, verificamos se Multipath IO está configurado.

![Extensão de armazenamento puro](../../media/extend-case-study-purestorage/purestorage-2.png)

Além de exibir os discos, os novos volumes podem ser criados e montados imediatamente para o host sem ter que usar a ferramenta de gerenciamento de disco do Windows.

![Extensão de armazenamento puro](../../media/extend-case-study-purestorage/purestorage-3.png)

Como a liberação de nossos Technical Preview, os comentários de clientes coletados até agora tem sido muito positivo e também nos proporcionou insight de diferentes recursos para adicionar em futuras versões. 

Recursos adicionais:
- [Postagem do blog de comunicado da extensão do pura armazenamento](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) podcast
