---
title: Instalar ou Remover Pacotes de Idiomas
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860087"
---
# <a name="install-or-remove-language-packs"></a>Instalar ou Remover Pacotes de Idiomas

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Você deve primeiro criar uma imagem multilíngue do Windows, conforme descrito na [pacotes de idioma e implantação](https://technet.microsoft.com/library/hh824829) antes de adicionar o pacote de idiomas do Windows Server Essentials.  
  
 Os pacotes de idiomas estão disponíveis somente para a criação de imagens em vários idiomas. As informações nesta seção são específicas para instalar ou remover pacotes de idioma no Windows Server Essentials.  
  
> [!NOTE]
>  Se você pretende executar a IC (Configuração Inicial) em um computador cliente que não seja compatível com idiomas do leste asiático, como ja-jp, e se inglês não estiver incluído na imagem multilíngue no servidor, a página da Web da IC exibirá quadrados. Para a página da Web da IC assumir inglês como padrão, a imagem multilíngue criada deve incluir inglês.  
  
## <a name="adding-language-packs-to-an-image"></a>Adicionando pacotes de idiomas a uma imagem  
 Os pacotes de idiomas estão disponíveis no DVD de personalização de OEM. Recomenda-se copiar os pacotes de idiomas ao computador do técnico antes de adicionar os pacotes de idiomas à imagem.  
  
 É necessário usar o comando a seguir para instalar pacotes de idiomas:  
  
 **DISM.exe /online /Add-Package /PackagePath:\\< caminho completo para o diretório do arquivo\>\lp.cab**  
  
 Por exemplo, o comando a seguir mostra como adicionar um pacote do idioma alemão:  
  
 **dism.exe /online /Add-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  Você também deve aplicar os pacotes de idiomas do Windows Server Essentials localizar completamente o sistema operacional.  
  
## <a name="removing-language-packs-from-an-image"></a>Removendo pacotes de idiomas a partir de uma imagem  
 Você pode usar o comando a seguir para remover um pacote de idioma que você não deseja mais incluir em uma imagem:  
  
 **DISM.exe /online /Remove-Package /PackagePath:\\< caminho completo para o diretório do arquivo\>\lp.cab**  
  
 Por exemplo, o comando a seguir mostra como remover um pacote do idioma alemão:  
  
 **dism.exe /online /Remove-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Consulte também  

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](Testing-the-Customer-Experience.md)

 [Criando e personalizando a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do usuário](../install/Testing-the-Customer-Experience.md)

