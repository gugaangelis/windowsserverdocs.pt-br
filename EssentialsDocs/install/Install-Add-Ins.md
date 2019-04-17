---
title: Instalar Add-Ins
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d00cb6886e812ee2b780ad79e1fba44442e279ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-add-ins"></a>Instalar Add-Ins

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode incluir suplementos em todos os servidores ou computadores cliente instalando-los antes de criar uma imagem. Estes suplementos, em seguida, serão incluídos automaticamente em todos os computadores replicados usando essa imagem. Você pode instalar um suplemento executando o arquivo. wssx ou você pode instalar arquivos de suplemento individuais seguindo as orientações a [documentação do SDK](https://go.microsoft.com/fwlink/?LinkID=248648)para cada tipo de suplemento. Se você instalar usando um arquivo. wssx, o suplemento poderá ser desinstalado por meio do Gerenciador de suplementos. Se você instalar os arquivos individuais, o suplemento não for gerenciado do Gerenciador de suplementos.  
  
> [!NOTE]
>  Qualquer software que seja instalado ou baixado no servidor (inclusive guias do painel e notificações de integridade) deve ter uma interface traduzida que corresponda ao idioma do sistema operacional que está instalado no servidor.  
  
#### <a name="to-install-an-add-in"></a>Para instalar um suplemento  
  
1.  (Opcional) Se você estiver instalando o suplemento usando um arquivo. wssx, conclua as seguintes etapas:  
  
    1.  Salve o arquivo. wssx de < AddinName\ > no computador de referência.  
  
    2.  Clique duas vezes no arquivo. wssx para abrir o Assistente de instalação de suplemento.  
  
    3.  Siga as instruções no Assistente para concluir a instalação.  
  
2.  (Opcional) Instale os arquivos de suplementos individuais nos locais apropriados, conforme definido no SDK para cada tipo de suplemento.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)