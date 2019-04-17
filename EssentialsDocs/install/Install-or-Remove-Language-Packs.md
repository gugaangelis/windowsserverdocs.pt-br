---
title: Instalar ou remover pacotes de idiomas
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a41b491bbe4b4a8ee7f9743dc85e5bdaffb08496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-or-remove-language-packs"></a>Instalar ou remover pacotes de idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Você deve primeiro criar uma imagem multilíngue do Windows, conforme descrito no [pacotes de idiomas e implantação](https://technet.microsoft.com/library/hh824829) antes de adicionar o pacote de idiomas do Windows Server Essentials.  
  
 Pacotes de idiomas só estão disponíveis para a criação de imagens em vários idiomas. As informações nesta seção são específicas para instalar ou remover pacotes de idiomas no Windows Server Essentials.  
  
> [!NOTE]
>  Se você pretende executar a configuração inicial (IC) de um computador cliente que não oferece suporte a idiomas do Leste Asiático, como ja-jp, e em inglês não está incluído na imagem multilíngue no servidor, a página da Web IC exibirá quadrados. Da página da Web IC como padrão para o inglês, a imagem multilíngue que você cria deve incluir o inglês.  
  
## <a name="adding-language-packs-to-an-image"></a>Adicionar idiomas pacotes a uma imagem  
 Pacotes de idiomas estão disponíveis no DVD de personalização do OEM. É recomendável que você copiar os pacotes de idiomas para o computador do técnico antes de adicionar os pacotes de idiomas à imagem.  
  
 Você deve usar o seguinte comando para instalar pacotes de idiomas:  
  
 **DISM.exe /online /Add-Package /PackagePath:C: \ \ < caminho completo do arquivo cab Directory \ arquivo > \lp.cab**  
  
 Por exemplo, o comando a seguir mostra como adicionar um pacote de idioma alemão:  
  
 **DISM.exe /online /Add-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  Você também deve aplicar pacotes de idiomas do Windows Server Essentials para totalmente traduzida do sistema operacional.  
  
## <a name="removing-language-packs-from-an-image"></a>Removendo pacotes de idiomas de uma imagem  
 Você pode usar o seguinte comando para remover um pacote de idiomas que você não deseja incluir em uma imagem:  
  
 **DISM.exe /online /Remove-Package /PackagePath:C: \ \ < caminho completo do arquivo cab Directory \ arquivo > \lp.cab**  
  
 Por exemplo, o comando a seguir mostra como remover um pacote de idioma alemão:  
  
 **DISM.exe /online /Remove-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Consulte também  

 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

