---
title: 'Notas de versão: problemas importantes no Windows Server 2019'
description: Resume os problemas críticos exigir soluções alternativas para evitar falhas, deslocado, instalação falha e perda de dados
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: d5d84bb1cd204a5419271cc3668343f8ac9c9a54
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149886"
---
# Notas de versão: problemas importantes no Windows Server 2019

>Aplica-se a: Windows Server 2019

Essas notas de versão resumem os problemas mais importantes no Windows Server&reg; sistema de operacional 2019, inclusive formas de evitar ou solucionar problemas, se conhecido. Para obter informações sobre alterações planejadas, novos recursos e correções desta versão, consulte [o que há de novo no Windows Server 2019](whats-new-19.md) e os comunicados das equipes de recursos específicos. Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server 2019.  

Este documento é atualizado continuamente. À medida que são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas, assim como novas soluções e correções assim que estiverem disponíveis.  
  
## Notas de versão
Os seguintes problemas conhecidos estão presentes no Windows Server 2019. 
<table border="1" rules="rows">
  <thead align="left" valign="middle">
    <tr>
      <th>Título</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody align="left" valign="middle">
    <tr>
      <td>Menu de opções de instalação durante a instalação de servidor tem truncado texto em alemão</td>
      <td>Ao executar a instalação de mídia do servidor alemão, na janela de seleção do sistema operacional intitulada "Selecionar o sistema operacional que deseja instalar," a descrição para opções de instalação de Experiência Desktop terá caracteres ausente e incorreto no final da sentença. Aqui está o texto completo alemão como deve aparecer.  
      <br/>
      <p><i>Durch diese opção wird chip vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. SIE kann hilfreich sein, wenn Sie sala de estar verfügen de aplicativo do Windows Desktop verwenden möchten ordem über eine, die chip grafische Umgebung benötigt.</i> </p>
      <p>Isso afeta somente a mídia alemão lançada a disponibilidade pública do Windows Server 2019, Windows Server, versão 1809 e Microsoft Hyper-V Server 2019.</p></td>
    </tr>
    <tr>
      <td>Imagem de marca do Windows Server incorreta durante a instalação do Windows Server, versão 1809  </td>
      <td>Durante a experiência de instalação do Windows Server, versão 1809, a imagem de plano de fundo em algumas telas iniciais mostra "Windows Server 2019".  Como com o Windows Server, versões 1709 e 1803, isso deve simplesmente dizer "Windows Server".  Não há nenhum impacto em qualquer outro lugar no produto, e não há nenhum impacto sobre o produto Windows Server 2019.  O problema é limitado a essa uma imagem durante a instalação do Windows Server, versão 1809, disponível somente para clientes de licença de volume, acessar o Centro de serviço de licença de Volume.  
      </td>
    </tr>
  </tbody>
</table>


### Direitos autorais  
Este documento é fornecido "na condição em que se encontra". As informações e visualizações apresentadas neste documento, incluindo URL e outras referências a sites, estão sujeitas a alterações sem prévio aviso.  

Este documento não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft. Você pode copiar e usar este documento para fins de referência interna.  

&copy;2019 Microsoft Corporation. Todos os direitos reservados.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server são marcas registradas ou marcas comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.  

Este produto contém software de filtro gráfico; este software baseia-se, em parte, no trabalho do Independent JPEG Group.  


1.0  
