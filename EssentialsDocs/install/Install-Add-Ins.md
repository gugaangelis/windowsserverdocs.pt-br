---
title: Instalar Suplementos
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8428a0198459e1dc036647a3d47fa1a49c00c01
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820129"
---
# <a name="install-add-ins"></a>Instalar Suplementos

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode incluir suplementos em todos os servidores ou computadores clientes instalando-os antes de criar uma imagem. Assim, esses suplementos serão automaticamente incluídos em todos os computadores replicados usando essa imagem. Você pode instalar um suplemento executando o arquivo .wssx ou instalar arquivos de suplemento individuais seguindo as diretrizes na [documentação do SDK](https://go.microsoft.com/fwlink/?LinkID=248648)para cada tipo de suplemento. Se você instalar usando um arquivo .wssx, o suplemento pode ser desinstalado através do Gerenciador de Suplementos. Se você instalar os arquivos individuais, o suplemento não será gerenciado no Gerenciador de Suplementos.  
  
> [!NOTE]
>  Qualquer software instalado ou baixado no servidor (incluindo guias do painel e notificações de integridade) deve ter uma interface localizada que corresponda ao idioma do sistema operacional instalado no servidor.  
  
#### <a name="to-install-an-add-in"></a>Para instalar um suplemento  
  
1.  (Opcional) Se você estiver instalando o suplemento usando um arquivo .wssx, siga etapas a seguir:  
  
    1.  Salve o arquivo < AddInName\>. WSSX no computador de referência.  
  
    2.  Clique duas vezes no arquivo .wssx para abrir o Assistente de Instalação do Suplemento.  
  
    3.  Siga as instruções do assistente para concluir a instalação.  
  
2.  (Opcional) Instale os arquivos suplementares individuais nos locais adequados conforme definido no SDK para cada tipo de suplemento.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)