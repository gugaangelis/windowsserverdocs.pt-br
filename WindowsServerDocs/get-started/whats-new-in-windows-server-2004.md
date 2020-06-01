---
title: Novidades no Windows Server, versão 2004
description: Novos recursos do Windows Server, versão 2004
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: Heidilohr
ms.author: helohr
ms.date: 05/27/2020
ms.localizationpriority: high
ms.openlocfilehash: e0136dad7180e41f15ae6226008aa7580ec53283
ms.sourcegitcommit: c63672805c93d5bf2a9eb71b3e2de2df00194529
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84124894"
---
# <a name="whats-new-in-windows-server-version-2004"></a>Novidades no Windows Server, versão 2004

>Aplica-se a: Windows Server (Canal semestral)

Para saber mais sobre os recursos mais recentes do Windows, consulte [Novidades no Windows Server](whats-new-in-windows-server.md). Este tópico descreve alguns dos novos recursos do Windows Server, versão 2004.

## <a name="server-core-container-improvements"></a>Aprimoramentos no contêiner do Server Core

Reduzimos o tamanho total das imagens de contêiner do Server Core para melhorar o desempenho e as velocidades de download. Incluímos os seguintes aprimoramentos:

- A maioria das imagens NGEN foi removida da imagem de contêiner do Server Core para diminuir o tamanho dela.
- As imagens do runtime do .NET Framework criadas com base em imagens de contêiner do Server Core agora estão otimizadas para os aplicativos ASP.NET e para o desempenho de script do Windows PowerShell.
- A equipe do .NET também se certificou de que há apenas uma cópia de cada imagem NGEN, resultando em um tamanho menor de imagens do .NET Framework.

Para dar uma ideia melhor do tamanho desses contêineres, a tabela a seguir compara a versão atual do contêiner a partir da [atualização de segurança mensal de maio de 2020](https://support.microsoft.com/help/4561769/windows-server-containers-for-may-2020) (também conhecida como atualização "5B") com versões anteriores.

| Versão do contêiner | Tamanho do download | Tamanho em disco |
|---|---|---|
| Windows Server, versão 1903 | 2,311 GB | 5,1 GB |
| Windows Server, versão 1909 | 2,257 GB | 4,97 GB |
| Windows Server, versão 2004 | 1,830 GB | 3,98 GB |

Para obter mais informações sobre a atualização do Windows Server, versão 2004, confira [nossa postagem no blog](https://techcommunity.microsoft.com/t5/containers/windows-server-version-2004-now-available/ba-p/1419194). Para saber mais sobre as atualizações de contêiner do Windows em geral, confira [Atualizar contêineres do Windows Server](/virtualization/windowscontainers/deploy-containers/update-containers/).
