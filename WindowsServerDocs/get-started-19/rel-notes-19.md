---
title: Notas de versão – problemas importantes no Windows Server 2019
description: Resume os problemas críticos que exijam soluções alternativas para evitar falhas, deslocamento, a instalação falha e perda de dados
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 515255c301d343aa1b83bcfb506f2e3baa6ca969
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810733"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Notas de versão – problemas importantes no Windows Server 2019

>Aplica-se a: Windows Server 2019

Essas notas de versão resumem os problemas mais importantes no sistema operacional Windows Server 2019, inclusive maneiras de evitar ou solucionar os problemas, se conhecido. Para obter informações sobre alterações planejadas, novos recursos e correções nesta versão, consulte [o que há de novo no Windows Server 2019](whats-new-19.md) e os comunicados das equipes de recursos específicos. A menos que especificado de outra forma, cada problema reportado se aplica a todas as edições e opções de instalação do Windows Server 2019.  

Este documento é atualizado continuamente. À medida que são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas, assim como novas soluções e correções assim que estiverem disponíveis.  

## <a name="release-notes"></a>Notas de versão

Os seguintes problemas conhecidos estão presentes no Windows Server 2019.

| Título         | Descrição                            |
| -----         | -----------                            |
| Menu de opção de instalação durante a instalação do servidor foi truncado texto em alemão | Ao executar a instalação da mídia de alemão de servidor, na janela de seleção do sistema operacional intitulada "Selecionar o sistema operacional que você deseja instalar," a descrição de opções de instalação Experiência Desktop terá caracteres incorreto e ausente no final da sentença. Aqui está o texto em alemão completo como deve aparecer.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Isso só afeta a mídia alemão lançada em disponibilidade pública do Windows Server 2019, Windows Server, versão 1809 e Microsoft Hyper-V Server 2019.|
| Imagem de marca do Windows Server incorreta durante a instalação do Windows Server, versão 1809 | Durante a experiência de instalação do Windows Server, versão 1809, a imagem de plano de fundo em alguns inicial telas mostra &quot;Windows Server 2019&quot;.  Como com o Windows Server, as versões 1709 e 1803, isso deve simplesmente dizer &quot;Windows Server&quot;.  Não há nenhum impacto em qualquer outro lugar no produto e não há nenhum impacto sobre o produto Windows Server 2019.  O problema é limitado a esse uma imagem durante a instalação do Windows Server, versão 1809, disponível somente para clientes de licença de volume, acessar o Centro de serviços de licença de Volume.<br/> |

### <a name="copyright"></a>Copyright

Este documento é fornecido "na condição em que se encontra". As informações e visualizações apresentadas neste documento, incluindo URL e outras referências a sites, estão sujeitas a alterações sem prévio aviso.  

Este documento não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft. Você pode copiar e usar este documento para fins de referência interna.

&copy; 2019 Microsoft Corporation. Todos os direitos reservados.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server são marcas registradas ou marcas comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.  

Este produto contém um software de filtro gráfico, que é parcialmente baseado no trabalho do grupo Independent JPEG.  


1.0  
