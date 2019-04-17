---
title: "Criar o arquivo Oobe.xml incluindo logotipo e termos de licença"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8f99a2051e114b3c890f1cdac23aebf58689980
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Criar o arquivo Oobe.xml incluindo logotipo e termos de licença

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar seus próprios contrato de licença de usuário final (EULA) para a configuração inicial usando o arquivo Oobe.xml. O Oobe.xml é um arquivo usado para fornecer texto e imagens para a configuração inicial, boas-vindas do Windows e outras páginas apresentadas ao usuário final. Você pode adicionar vários arquivos Oobe.xml para personalizar o conteúdo com base nas seleções de idioma e país ou região do usuário final. Para obter mais informações, consulte o [de avaliação do Windows e Kit de implantação para o Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) documentação.  
  
 O EULA para sua empresa é exibido além do EULA da Microsoft. A Microsoft EULA será o primeiro EULA exibido durante a experiência de usuário final da configuração inicial e, em seguida, sua vontade de EULA ser exibido. Seu EULA pode ser colocada em qualquer lugar no servidor e especifique o local no arquivo Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Para adicionar sua empresa EULA e o logotipo  
  
1.  Abra o arquivo Oobe.xml em um editor de texto, como o bloco de notas.  
  
2.  Dentro de < logopath\ >< / logopath\ > marcas, informe o caminho absoluto para o arquivo de logotipo. Esse arquivo deve conter um arquivo de (.png) de elementos gráficos de rede portátil de 32 bits 240 x 100 pixels.  
  
3.  Dentro de < eulafilename\ >< / eulafilename\ > marcas, digite o caminho absoluto para o arquivo de EULA. O arquivo de EULA deve ser um arquivo de (.rtf) de formato rich text.  
  
4.  Dentro de < name\ >< / name\ > marcas, insira o nome da empresa.  
  
     O exemplo a seguir mostra as marcas em um arquivo Oobe.xml:  
  
    ```  
  
    <FirstExperience>  
       <oobe>  
          <oem>  
             <name>Fabrikam</name>  
             <logopath>c:\fabrikam\fabrikam.png</logopath>  
             <eulafilename>c:\fabrikam\fabrikam_eula.rtf</eulafilename>  
          </oem>  
       </oobe>  
    </FirstExperience>  
  
    ```  
  
5.  Salve o arquivo.  
  
6.  Coloque o arquivo Oobe.xml em um dos seguintes locais:  
  
    |Local de Oobe.xml|Condição para determinar a localização|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\<|O servidor está sendo lançado em um único país/região e um sistema de um único idioma.|  
    |%windir%\system32\oobe\info\default\\ < language\ >|O servidor está sendo lançado em um único país/região e vários sistema de idiomas.|  
    |%windir%\system32\oobe\info\\ < país/região > \ e %windir%\system32\oobe\info\\ < país/região > \ \ < language\ > \|O servidor está enviando para mais de um país/região e as configurações exigem personalizações em uma base por país/região, cada um com um único idioma. Onde < país/região > é a versão decimal do identificador de localização geográfica (GeoID) do país ou região em que o servidor está sendo implantado e < language\ > é a versão decimal do identificador de localidade (LCID).|  
  
 Se você tiver um logotipo da empresa alternativos com texto branco, ele pode exibir melhor no fluxo de instalação devido a tela de fundo azul.  Opcionalmente, você pode especificar esse logotipo, definindo uma chave do registro e um valor.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Para especificar um logotipo da empresa, definindo a chave de registro do OEM  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
2.  Na caixa de pesquisa, digite **regedit**e clique no aplicativo Regedit.  
  
3.  No painel de navegação, navegue até **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se a chave OEM não existir, crie a chave da seguinte maneira:  
  
    1.  Clique com botão direito **Windows Server**, clique em **nova**e clique em **chave **.  
  
    2.  Nome de chave, digite **OEM**.  
  
4.  (Opcional) Se você estiver criando uma entrada para um logotipo, você pode criar chaves diferentes para diferenciar as versões de idioma do logotipo. Por exemplo, se você tiver versões inglesa e outra alemãs de um logotipo, você pode criar uma en-us chave e uma chave de-de. Como todos os arquivos de logotipo são armazenados na mesma pasta, você deve fornecer instâncias do arquivo de imagem de logotipo com um nome exclusivo para cada idioma. Por exemplo, você pode criar um arquivo chamado LogoWithWhiteText_en.png e LogoWithWhiteText_de.png.  
  
5.  O botão direito do mouse **OEM** ou clique com botão direito a chave de idioma apropriada, clique em para **nova**e clique em **valor de cadeia de caracteres**.  
  
6.  Insira LogoWithWhiteText como a cadeia de caracteres e pressione ENTER.  
  
7.  Clique com botão direito na nova cadeia e, em seguida, clique em **modificar**.  
  
8.  Insira o caminho que contém a imagem do logotipo e, em seguida, clique em Okey.  
  
## <a name="see-also"></a>Consulte também  
 [Introdução ao Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)