---
title: Adicionar Categorias de Nível Superior à Barra Inicial (Sistema Operacional Macintosh)
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c8a15a831a89afc55d20db4e1c1195173d466b3c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817509"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Adicionar Categorias de Nível Superior à Barra Inicial (Sistema Operacional Macintosh)

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar categorias de nível superior para a Barra Inicial em um computador executando o sistema operacional Macintosh. Para criar um suplemento de Launchpad que adiciona categorias de nível superior, você pode usar uma combinação de informações desta página e do tópico intitulado como: adicionar tarefas e categorias à Launchpad? no [SDK das soluções do Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
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
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)