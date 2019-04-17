---
title: "Adicionar categorias de nível superior à barra inicial (sistema de operacional Macintosh)"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Adicionar categorias de nível superior à barra inicial (sistema de operacional Macintosh)

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar categorias de nível superior à barra inicial em um computador executando o sistema operacional Macintosh. Para criar um suplemento barra inicial que adiciona categorias de nível superior, você pode usar uma combinação de informações nesta página e do tópico intitulado instruções: adicionar tarefas e categorias à barra inicial? no [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 O exemplo a seguir mostra como você pode especificar sua inscrição Launchpad para ser uma categoria de alto nível no arquivo .launchpad:  
  
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
  
 Para a entrada seja uma categoria de alto nível, o atributo Id do elemento categoria deve ser "Microsoft.Launchpad.HomeCategory".  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)