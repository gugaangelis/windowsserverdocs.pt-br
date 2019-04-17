---
title: Adicionar identidade visual para o painel, acesso via Web remoto e Launchpad
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Adicionar identidade visual para o painel, acesso via Web remoto e Launchpad

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a>Adicionar identidade visual para o painel, acesso via Web remoto e Launchpad  
 Você pode fazer adições de identidade visual muitos adicionando entradas no registro. Todas as entradas de identidade visual no registro para o sistema operacional estão localizadas em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows server\OEM..  
  
 Todo o Co-branding deve atender aos seguintes requisitos de logotipo:  
  
-   O logotipo do Windows Server Essentials deve ter uma largura mínima de **170 pixels**, mantendo a taxa de proporção correta, com **96 DPI**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Para adicionar identidade visual alterando o registro  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
2.  Na caixa de pesquisa, digite **regedit**e, em seguida, clique no **Regedit** aplicativo.  
  
3.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se o **OEM** chave não existe, conclua as seguintes etapas para criá-lo:  
  
    1.  Clique com botão direito **Windows Server**, clique em **nova**e clique em **chave **.  
  
    2.  Tipo **OEM** para o nome da chave.  
  
4.  (Opcional) Se você estiver criando uma entrada para um logotipo, você pode criar chaves diferentes para diferenciar as versões de idioma do logotipo. Por exemplo, se você tiver versões inglesa e outra alemãs de um logotipo, você pode criar uma en-us chave e uma chave de-de. Como todos os arquivos de logotipo são armazenados na mesma pasta, você deve fornecer instâncias do arquivo de imagem de logotipo com um nome exclusivo para cada idioma. Por exemplo, você criaria um arquivo chamado DashboardLogo_en.png e DashboardLogo_de.png.  
  
5.  O botão direito do mouse **OEM** ou clique com botão direito a chave de idioma apropriada, clique em para **nova**e clique em **valor de cadeia de caracteres**.  
  

6.  Insira o nome da cadeia de caracteres e pressione ENTER. Consulte o [cadeias de caracteres do registro e valores](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) tabela para os valores de dados e nomes de cadeia de caracteres.  

6.  Insira o nome da cadeia de caracteres e pressione ENTER. Consulte o [cadeias de caracteres do registro e valores](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) tabela para os valores de dados e nomes de cadeia de caracteres.  

  
7.  Clique com botão direito na nova cadeia e, em seguida, clique em **modificar**.  
  
8.  Insira o valor da tabela que está associada com o nome de cadeia de caracteres e, em seguida, clique em **Okey**.  
  
9. Se você estiver criando uma entrada para uma imagem de logotipo ou links anexados, copie o arquivo para %programFiles%\Windows server\bin\OEM.. Se o diretório OEM não existir, crie-o.  
  
10. Se forem feitas alterações que afetam o acesso via Web remoto, depois que o cliente tomar posse do servidor, ele devem ativar o acesso via Web remoto. Instrua o cliente a fazer o seguinte:  
  
    1.  No painel, clique em **configurações**e, em seguida, clique no **acesso em qualquer local** guia.  
  
    2.  Se o acesso em qualquer local estiver ativado, clique em **configurar**e desmarque a caixa de seleção acesso via Web remoto no **escolher qualquer lugar acessar os recursos para habilitar** página do conjunto no Assistente de acesso em qualquer local.  
  
    3.  Clique em **configurar**.  
  
###  <a name="BKMK_RegStrings"></a>A tabela a seguir lista o local onde as alterações do Registro afetam a identidade visual, o nome de cadeia de caracteres e o valor de dados.  
  
### <a name="registry-strings-and-values"></a>Cadeias de caracteres do registro e valores  
  
|Localização da identidade Visual|Descrição|Nome de cadeia de caracteres|Valor de dados|  
|--------------------------|-----------------|-----------------|----------------|  
|Logotipo do painel|Adiciona a imagem de logotipo para o painel. O logotipo do painel deve estar no formato. PNG e não deve ter mais de 350 pixels de largura por 38 pixels de altura.<br /><br /> **Importante:** para cobrand o painel com seu logotipo, você deve editar o bloco de arte final que é fornecido no DVD do OPK e acrescentar o logotipo da empresa à imagem enquanto segue os requisitos de espaço em branco apropriados. Para informações adicionais, consulte o bloco de exemplo que é fornecido.|DashboardLogo|Nome do arquivo de imagem de logotipo|  
|DashboardClientLogo|Adiciona a imagem de logotipo para a tela de logon do cliente de painel.|DashboardClientLogo|Nome do arquivo de imagem de logotipo|  
|Imagem de plano de fundo do site|Altera a imagem de fundo que é exibida na página de logon do acesso via Web remoto. As resoluções típicas serão exibidas da seguinte maneira:<br /><br /> -1024 x 768 a resolução de pixels preencherá exatamente a página de logon<br /><br /> -800 x 600 pixels será centralizada na página e aparecem com uma borda preta<br /><br /> -1280 x 720 a resolução de pixels será centralizada e os pixels que excederem 1024x768 não serão exibida.|LogonBackground|Nome do arquivo de imagem de plano de fundo|  
|Título do site|Substitui o título do site acesso via Web remoto do Windows Server Essentials por um título que você escolher.|WebsiteName|Novo título do site de acesso via Web remoto|  
|Logotipo do site|Altera o logotipo padrão no site de acesso via Web remoto. O tamanho esperado do logotipo é de 32 pixels por 32 pixels. Se seu logotipo for menor ou maior do que isso, ele será alongado ou reduzido para corresponder a essas dimensões|WebsiteLogo|Nome do arquivo de imagem de logotipo|  
|Logotipo de site anexado|Seu logotipo de parceiro será exibido logo abaixo do logotipo da Microsoft que é exibido no site de acesso via Web remoto. O tamanho esperado do logotipo é de 200 pixels de altura por 50 pixels de largura. Se o seu logotipo for maior do que isso, ele ficará menores para ajustá-lo, mantendo a taxa de proporção original. Se seu logotipo for menor do que isso, ele será centralizado dentro do espaço de pixel de 200 por 50 e o tamanho e a taxa de proporção será alterada.|OEMLogo|Nome do arquivo de imagem de logotipo|  

| Links na página de logon de home page do site e | Anexe links à página de logon e a página inicial do site acesso via Web remoto. O arquivo. XML que contém as informações de link deve estar localizado em %programFiles%\Windows server\bin\OEM.. O exemplo a seguir mostra o formato do arquivo. XML:<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < link Name\ = LogonLinkName ><br /> LogonLinkDescription < Text\ > < / Text\ ><br /> LogonLinkURL < Url\ > < / Url\ ><br /> LinkIcon < Icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < link Name\ = HomepageLinkName ><br /> HomepageLinkDescription < Text\ > < / Text\ ><br /> HomepageLinkURL < Url\ > < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > | LinksXML | Consulte o [elementos LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) tabela para obter uma lista dos elementos e descrições. |  

| Links na página de logon de home page do site e | Anexe links à página de logon e a página inicial do site acesso via Web remoto. O arquivo. XML que contém as informações de link deve estar localizado em %programFiles%\Windows server\bin\OEM.. O exemplo a seguir mostra o formato do arquivo. XML:<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < link Name\ = LogonLinkName ><br /> LogonLinkDescription < Text\ > < / Text\ ><br /> LogonLinkURL < Url\ > < / Url\ ><br /> LinkIcon < Icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < link Name\ = HomepageLinkName ><br /> HomepageLinkDescription < Text\ > < / Text\ ><br /> HomepageLinkURL < Url\ > < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > | LinksXML | Consulte o [elementos LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) tabela para obter uma lista dos elementos e descrições. |  

| Logotipo da barra inicial | Adiciona a imagem de logotipo à barra inicial. O logotipo da barra inicial deve estar no formato. PNG e deve não altura maior do que 64 pixels. | LaunchpadLogo | Nome do arquivo de imagem de logotipo |  
  
###  <a name="BKMK_Links"></a>A tabela a seguir lista e descreve os elementos de nome de cadeia de caracteres LinksXML.  
  
### <a name="linksxml-elements"></a>Elementos LinksXML  
  
|Elemento LinksXML|Descrição|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|O nome do link de logon.|  
|LogonLinkDescription|O texto que é exibido como o link da página de logon.|  
|LogonLinkURL|A url que é resolvida com o link da página de logon.|  
|LinkIcon|O nome do arquivo de ícone para o link de logon. Esse arquivo deve estar no mesmo local de pasta que o arquivo. XML. Imagens de ícone de link devem ter 16x16 pixels e devem estar formato. PNG. Se você não fornecer um LinkIcon, a imagem do ícone de link padrão é usada.|  
|**HomepageLinks**|  
|HomepageLinkName|O nome do link Home Page.|  
|HomepageLinkDescription|O texto que aparece como o link da página inicial.|  
|HomepageLinkURL|A URL que é resolvida com o link da página inicial.|  
|HomepageLinkIcon|O nome do arquivo de ícone para o link da página inicial. Esse arquivo deve estar no mesmo local de pasta que o arquivo. XML. Imagens de HomepageLinkIcon devem ter 16x16 pixels e devem estar formato. PNG. Se você não fornecer um HomepageLinkIcon, a imagem de ícone de link de página inicial padrão é usada.|  
  
## <a name="see-also"></a>Consulte também  

 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

