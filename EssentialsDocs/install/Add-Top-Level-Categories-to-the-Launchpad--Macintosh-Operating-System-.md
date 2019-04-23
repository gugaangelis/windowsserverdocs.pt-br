---
title: Adicionar Categorias de Nível Superior à Barra Inicial (Sistema Operacional Macintosh)
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae4eb5943d37b4a9d3b554af28cb425420782cf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869957"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Adicionar Categorias de Nível Superior à Barra Inicial (Sistema Operacional Macintosh)

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar categorias de nível superior para a Barra Inicial em um computador executando o sistema operacional Macintosh. Para criar um suplemento de barra inicial que adicione categorias de nível superior, você pode usar uma combinação de informações dessa página e do tópico intitulado instruções: Adicionar tarefas e categorias à barra inicial? no [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 O seguinte exemplo mostra como você pode especificar para que a entrada da Barra Inicial seja uma categoria de nível superior no arquivo .launchpad:  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<LaunchPad resFile="Localizable">  
   <Category id="Microsoft.Launchpad.HomeCategory" name="SampleAddin"  image="SampleImage01.png">  
      <Task id="Microsoft.Launchpad.SampleAddin.Pictures"   
          name="Pictures"       
           src="smb://%ServerAddress%/Pictures"   
         image="SampleImage02.png"/>  
   </Category>  
</LaunchPad>  
```  
  
 Para que a entrada seja uma categoria de nível superior, o atributo Id do elemento Category deve ser "Microsoft.Launchpad.HomeCategory".  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](Testing-the-Customer-Experience.md)