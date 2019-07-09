---
title: Notas de versão - Problemas importantes no Windows Server 2019
description: Resume os problemas críticos que exigem soluções alternativas para evitar falhas, travamento, falha de instalação e perda de dados
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810733"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Notas de versão - Problemas importantes no Windows Server 2019

>Aplica-se a: Windows Server 2019

Essas notas de versão resumem os problemas mais críticos no sistema operacional do Windows Server 2019, incluindo formas de evitar ou solucionar os problemas, se conhecidos. Para saber mais sobre alterações planejadas, novos recursos e correções desta versão, confira [Novidades no Windows Server 2019](whats-new-19.md) e os comunicados das equipes de recursos específicos. Exceto quando especificado de outra forma, cada problema relatado é aplicável a todas as edições e opções de instalação do Windows Server 2019.  

Este documento é atualizado continuamente. À medida que são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas, assim como novas soluções e correções assim que estiverem disponíveis.  

## <a name="release-notes"></a>Notas de versão

Os seguintes problemas conhecidos estão presentes no Windows Server 2019.

| Título         | Descrição                            |
| -----         | -----------                            |
| Menu de opções de instalação durante a configuração do servidor tem texto truncado em alemão | Ao executar a configuração da mídia de servidor em alemão na janela de seleção do sistema operacional com título "Selecionar o sistema operacional que deseja instalar," a descrição de opções de instalação da Experiência Desktop terá caracteres incorretos e ausentes no final da sentença. Segue o texto em alemão completo como deve aparecer.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Isso afeta somente a mídia em alemão lançada em disponibilidade pública do Windows Server 2019, Windows Server, versão 1809 e Microsoft Hyper-V Server 2019.|
| Imagem da identidade visual do Windows Server incorreta durante a configuração do Windows Server, versão 1809 | Durante a experiência de instalação do Windows Server, versão 1809, a imagem de plano de fundo em algumas telas iniciais mostra &quot;Windows Server 2019&quot;.  Da mesma forma que no Windows Server, versões 1709 e 1803, a tela deve simplesmente mostrar &quot;Windows Server&quot;.  Não há nenhum impacto adicional em qualquer outro lugar no produto e não há nenhum impacto no produto Windows Server 2019.  O problema é limitado a essa imagem durante a instalação do Windows Server, versão 1809, disponível somente para clientes de licença de volume, acessando o Centro de Serviços de Licenciamento por Volume.<br/> |

### <a name="copyright"></a>Copyright

Este documento é fornecido "na condição em que se encontra". As informações e visualizações apresentadas neste documento, incluindo URL e outras referências a sites, estão sujeitas a alterações sem prévio aviso.  

Este documento não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft. Você pode copiar e usar este documento para fins de referência interna.

&copy; 2019 Microsoft Corporation. Todos os direitos reservados.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server são marcas registradas ou marcas comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.  

Este produto contém um software de filtro gráfico, que é parcialmente baseado no trabalho do grupo Independent JPEG.  


1.0  
