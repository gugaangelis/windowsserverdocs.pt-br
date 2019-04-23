---
title: Instalar Suplementos
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833067"
---
# <a name="install-add-ins"></a>Instalar Suplementos

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode incluir suplementos em todos os servidores ou computadores clientes instalando-os antes de criar uma imagem. Assim, esses suplementos serão automaticamente incluídos em todos os computadores replicados usando essa imagem. Você pode instalar um suplemento executando o arquivo .wssx ou instalar arquivos de suplemento individuais seguindo as diretrizes na [documentação do SDK](https://go.microsoft.com/fwlink/?LinkID=248648)para cada tipo de suplemento. Se você instalar usando um arquivo .wssx, o suplemento pode ser desinstalado através do Gerenciador de Suplementos. Se você instalar os arquivos individuais, o suplemento não será gerenciado no Gerenciador de Suplementos.  
  
> [!NOTE]
>  Qualquer software instalado ou baixado no servidor (incluindo guias do painel e notificações de integridade) deve ter uma interface localizada que corresponda ao idioma do sistema operacional instalado no servidor.  
  
#### <a name="to-install-an-add-in"></a>Para instalar um suplemento  
  
1.  (Opcional) Se você estiver instalando o suplemento usando um arquivo .wssx, siga etapas a seguir:  
  
    1.  Salvar o < arquivo\>arquivo. wssx no computador de referência.  
  
    2.  Clique duas vezes no arquivo .wssx para abrir o Assistente de Instalação do Suplemento.  
  
    3.  Siga as instruções do assistente para concluir a instalação.  
  
2.  (Opcional) Instale os arquivos suplementares individuais nos locais adequados conforme definido no SDK para cada tipo de suplemento.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](Testing-the-Customer-Experience.md)