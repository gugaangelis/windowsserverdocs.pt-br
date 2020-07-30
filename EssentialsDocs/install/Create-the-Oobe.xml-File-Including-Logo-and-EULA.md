---
title: Criar o Arquivo Oobe.xml Incluindo Logotipo e EULA
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 498fa182c74a40f5a4b6b9c3b1dbc43df1a5d6a2
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181362"
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Criar o Arquivo Oobe.xml Incluindo Logotipo e EULA

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode adicionar seu próprio Contrato de Licença de Usuário Final (EULA) à Configuração Inicial usando o arquivo Oobe.xml. O arquivo Oobe.xml é usado para fornecer texto e imagens para a Configuração Inicial, a tela de Boas-Vindas do Windows e outras páginas apresentadas ao usuário final. Você pode adicionar diversos arquivos Oobe.xml para personalizar o conteúdo com base nas seleções de idioma e país ou região do usuário final. Para mais informações, consulte a documentação [Kit de Avaliação e Implantação do Windows para o Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) .

 Além do EULA da Microsoft, o EULA de sua empresa também é exibido. O EULA da Microsoft será o primeiro exibido durante a experiência do usuário final da configuração inicial e, então, seu EULA será exibido. Seu EULA pode ser colocado em qualquer local do servidor e você especificará o local no arquivo Oobe.xml.

#### <a name="to-add-your-company-eula-and-logo"></a>Para adicionar o EULA e o logotipo de sua empresa

1. Abra o arquivo Oobe.xml em um editor de texto, como o Bloco de Notas.

2. Dentro das marcas <logopath \></logopath \> , insira o caminho absoluto para o seu arquivo de logotipo. Esse arquivo deve conter um arquivo em formato PNG de 32 bits com 240 x 100 pixels.

3. Dentro das marcas <eulafilename \></eulafilename \> , insira o caminho absoluto para o arquivo de EULA. O arquivo EULA deve ser um arquivo de formato RTF.

4. Dentro do nome do <\>< \> marcações/Name, insira o nome da empresa.

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

5. Salve o arquivo.

6. Coloque o arquivo Oobe.xml em um dos seguintes locais:

   |Local do Oobe.xml|Condição para determinar o local|
   |-----------------------|----------------------------------------|
   |%WINDIR%\System32\Oobe\Info \| o servidor está enviando um único país/região e um único sistema de linguagem.|
   |\\linguagem de<%windir%\system32\oobe\info\default\>|O servidor está enviando em um sistema de país/região único e vários idiomas.|
   |%WINDIR%\System32\Oobe\Info \\<país/região> \ e%windir%\system32\oobe\info \\<país/região>\\<linguagem \> \| o servidor está enviando para mais de um país/região e as configurações exigem personalizações por país/região, cada uma com um único idioma. Onde <país/região> é a versão decimal do GeoID (identificador de localização geográfica) do país ou da região onde o servidor está sendo implantado e <idioma \> é a versão decimal do LCID (identificador de localidade).|

   Se você tiver um logotipo de empresa alternativo com texto branco, ele pode ser exibido melhor no fluxo de configuração devido ao plano de fundo azul.  Você pode, como opção, especificar esse logotipo configurando uma chave de registro e um valor.

#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Para especificar um logotipo da empresa configurando a chave de registro do OEM

1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.

2.  Na caixa Pesquisar, digite **regedit** e clique no aplicativo Regedit.

3.  No painel de navegação, navegue para **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se a chave do OEM não existir, crie a chave da seguinte maneira:

    1.  Clique com o botão direito do mouse em **Windows Server**, clique em **Novo** e em **Chave**.

    2.  Para o nome da chave, digite **OEM**.

4.  (Opcional) Se estiver criando uma entrada para um logotipo, você poderá criar diferentes chaves para distinguir as versões de idioma do logotipo. Por exemplo, se você tiver as versões em inglês e em alemão de um logotipo, é possível criar uma chave en-us e uma chave de-de. Como todos os arquivos de logotipo são armazenados na mesma pasta, é necessário fornecer instâncias do arquivo de imagem do logotipo com um nome exclusivo para cada idioma. Por exemplo, é possível criar um arquivo chamado LogoWithWhiteText_en.png e LogoWithWhiteText_de.png.

5.  Clique com o botão direito do mouse em **OEM** ou na chave de idioma apropriada, clique em **Novo** e então em **Valor da Cadeia de Caracteres**.

6.  Digite LogoWithWhiteText como a cadeia de caracteres e depois pressione ENTER.

7.  Clique com o botão direito do mouse na nova cadeia de caracteres e depois em **Modificar**.

8.  Insira o caminho que contém a imagem do logotipo e depois clique em OK.

## <a name="see-also"></a>Consulte Também
 [Introdução com o Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)