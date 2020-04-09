---
title: Adicionar Identidade Visual para o Dashboard, o Acesso Remoto da Web e a Barra Inicial
description: Descreve como usar o Windows Server Essentials
ms.date: 04/10/2014
ms.prod: windows-server
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 50f132d7f6422c32b2a72948ca96b5bd82e701df
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817739"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Adicionar Identidade Visual para o Dashboard, o Acesso Remoto da Web e a Barra Inicial

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a><a name="BKMK_Branding"></a>Adicionar identidade visual ao painel, Acesso via Web remoto e Launchpad  
 Você pode fazer várias adições de identidade visual acrescentando entradas no Registro. Todas as entradas de identidade visual no registro para o sistema operacional estão localizadas em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM.  
  
 Todos os co-branding devem atender aos seguintes requisitos de logotipo:  
  
-   O logotipo do Windows Server Essentials deve ter uma largura mínima de **170 pixels**, mantendo a taxa de proporção correta, com **96 DPI**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Para adicionar a identidade visual alterando o Registro  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.  
  
2.  Na caixa de Pesquisa, digite **regedit**e, em seguida, clique no aplicativo **Regedit** .  
  
3.  No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se a chave **OEM** não existir, conclua as seguintes etapas para criá-la:  
  
    1.  Clique com o botão direito do mouse em **Windows Server**, clique em **Novo** e em **Chave**.  
  
    2.  Digite **OEM** como nome da chave.  
  
4.  (Opcional) Se estiver criando uma entrada para um logotipo, você poderá criar diferentes chaves para distinguir as versões de idioma do logotipo. Por exemplo, se você tiver as versões em inglês e em alemão de um logotipo, é possível criar uma chave en-us e uma chave de-de. Como todos os arquivos de logotipo são armazenados na mesma pasta, é necessário fornecer instâncias do arquivo de imagem do logotipo com um nome exclusivo para cada idioma. Por exemplo, você cria um arquivo chamado DashboardLogo_en.png e DashboardLogo_de.png.  
  
5.  Clique com o botão direito do mouse em **OEM** ou na chave de idioma apropriada, clique em **Novo** e então em **Valor da Cadeia de Caracteres**.  
  

6.  Digite o nome da cadeia de caracteres e, em seguida, pressione ENTER. Consulte a tabela [Valores e cadeias de caracteres do Registro](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) para obter nomes e valores de dados da cadeia de caracteres.  

6.  Digite o nome da cadeia de caracteres e, em seguida, pressione ENTER. Consulte a tabela [Valores e cadeias de caracteres do Registro](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) para obter nomes e valores de dados da cadeia de caracteres.  

  
7.  Clique com o botão direito do mouse na nova cadeia de caracteres e depois em **Modificar**.  
  
8.  Digite o valor da tabela associada ao nome da cadeia de caracteres e clique em **OK**.  
  
9. Se estiver criando uma entrada para uma imagem de logotipo ou links anexados, copie o arquivo em %programFiles%\Windows Server\Bin\OEM. Se o diretório OEM não existir, crie-o.  
  
10. Se as alterações feitas afetarem o Acesso Remoto da Web, depois que o cliente tiver o controle do servidor, o Acesso Remoto da Web deverá ser ativado. Solicite ao cliente que faça o seguinte:  
  
    1.  No Dashboard, clique em **Configurações** e na guia **Acesso em Qualquer Local**.  
  
    2.  Se o Acesso em Qualquer Local estiver ativado, clique em **Configurar** e desmarque a caixa de seleção Acesso via Web Remoto na página **Escolha os recursos do Acesso em Qualquer Local a serem habilitados** do Assistente para Configurar o Acesso em Qualquer Local.  
  
    3.  Clique em **Configurar**.  
  
###  <a name="the-following-table-lists-the-location-where-registry-changes-affect-branding-the-string-name-and-the-data-value"></a><a name="BKMK_RegStrings"></a>A tabela a seguir lista o local em que as alterações no registro afetam a identidade visual, o nome da cadeia de caracteres e o valor dos dados.  
  
### <a name="registry-strings-and-values"></a>Valores e cadeias de caracteres do Registro  
  
|Local da identidade visual|Descrição|Nome da cadeia de caracteres|Valor de dados|  
|--------------------------|-----------------|-----------------|----------------|  
|Logotipo do Dashboard|Adiciona a imagem do logotipo ao Dashboard. O logotipo do painel deve estar no formato .png e não deve exceder 350 pixels de largura por 38 de altura.<br /><br /> **Importante:** Para comarcar o painel com o logotipo, você deve editar o bloco de trabalho artístico que é fornecido no DVD do OPK e acrescentar o logotipo da empresa à imagem, ao seguir os requisitos de espaço em branco apropriados. Para obter mais informações, consulte o lado a lado do exemplo fornecido.|DashboardLogo|Nome do arquivo de imagem do logotipo|  
|DashboardClientLogo|Adiciona a imagem do logotipo para a tela de logon de cliente do Painel.|DashboardClientLogo|Nome do arquivo de imagem do logotipo|  
|Imagem de segundo plano do site|Altera a imagem de segundo plano exibida na página de logon do Acesso Remoto da Web. As resoluções mais frequentes aparecerão da seguinte forma:<br /><br /> -a resolução de 1024x768 pixel preencherá precisamente a página de logon<br /><br /> -800x600 a resolução de pixels será centralizada na página e aparecerá com uma borda preta<br /><br /> -a resolução de pixels de 1280x720 será centralizada e os pixels que excederem 1024x768 não serão exibidos|LogonBackground|Nome do arquivo da imagem de segundo plano|  
|Título do site|Substitui o título do site Acesso via Web remoto do Windows Server Essentials em um título que você escolher.|WebsiteName|Novo título do site do Acesso Remoto da Web|  
|Logotipo do site|Altera o logotipo padrão do site do Acesso Remoto da Web. O tamanho esperado do logotipo é 32 pixels por 32 pixels. Se seu logotipo for menor ou maior do que isso, será aumentado ou reduzido para se adequar a essas dimensões|WebsiteLogo|Nome do arquivo de imagem do logotipo|  
|Logotipo do site anexado|O logotipo do parceiro será exibido logo abaixo do logotipo da Microsoft exibido no site do Acesso via Web Remoto. O tamanho esperado do logotipo é 200 pixels de altura por 50 pixels de largura. Se o seu logotipo for maior do que isso, ele será reduzido para se ajustar, sendo mantida a taxa de proporção original. Se o seu logotipo for menor do que isso, ele será centralizado no espaço de 200 por 50 pixels, e a taxa de proporção e o tamanho não serão alterados.|OEMLogo|Nome do arquivo de imagem do logotipo|  

| Links no site home page e na página de logon | Acrescente links à página de logon e home page do site do Acesso via Web remoto. O .xml que contém as informações do link deve estar localizado em %programFiles%\Windows Server\Bin\OEM. O exemplo a seguir mostra o formato do arquivo. xml:<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> Nome do link de <\=LogonLinkName ><br /> < text\>LogonLinkDescription </text\><br /> URL de <\>LogonLinkURL </URL\><br /> Ícone de <\>LinkIcon </Icon\><br /> </link\><br /> </LogonLinks\><br /> < HomepageLinks\><br /> Nome do link de <\=HomepageLinkName ><br /> < text\>HomepageLinkDescription </text\><br /> URL de <\>HomepageLinkURL </URL\><br /> </link\><br /> </HomepageLinks\><br /> </OemLinks\>| LinksXML | Consulte a tabela de [elementos LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) para obter uma lista dos elementos e descrições. |  

| Links no site home page e na página de logon | Acrescente links à página de logon e home page do site do Acesso via Web remoto. O .xml que contém as informações do link deve estar localizado em %programFiles%\Windows Server\Bin\OEM. O exemplo a seguir mostra o formato do arquivo. xml:<br /><br /> < OemLinks\><br /> < LogonLinks\><br /> Nome do link de <\=LogonLinkName ><br /> < text\>LogonLinkDescription </text\><br /> URL de <\>LogonLinkURL </URL\><br /> Ícone de <\>LinkIcon </Icon\><br /> </link\><br /> </LogonLinks\><br /> < HomepageLinks\><br /> Nome do link de <\=HomepageLinkName ><br /> < text\>HomepageLinkDescription </text\><br /> URL de <\>HomepageLinkURL </URL\><br /> </link\><br /> </HomepageLinks\><br /> </OemLinks\>| LinksXML | Consulte a tabela de [elementos LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) para obter uma lista dos elementos e descrições. |  

| Logotipo da Launchpad | Adiciona a imagem de logotipo ao Launchpad. O logotipo Launchpad deve estar no formato. png e não deve ser mais alto que 64 pixels. | LaunchpadLogo | Nome do arquivo de imagem de logotipo |  
  
###  <a name="the-following-table-lists-and-describes-the-linksxml-string-name-elements"></a><a name="BKMK_Links"></a>A tabela a seguir lista e descreve os elementos de nome de cadeia de caracteres LinksXML.  
  
### <a name="linksxml-elements"></a>Elementos de LinksXML  
  
|Elemento LinksXML|Descrição|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|O nome do link de logon.|  
|LogonLinkDescription|O texto que é exibido como o link da página de logon.|  
|LogonLinkURL|A URL que aponta para o link da página de logon.|  
|LinkIcon|O nome do arquivo de ícone do link de logon. Este arquivo deve estar no mesmo local de pasta do arquivo .xml. As imagens do ícone de link devem ter 16x16 pixels e devem estar no formato. png. Se você não fornecer um LinkIcon, será usada a imagem padrão do ícone de link.|  
|**HomepageLinks**|  
|HomepageLinkName|O nome do link da home page.|  
|HomepageLinkDescription|O texto que aparece como o link da home page.|  
|HomepageLinkURL|A URL que aponta para o link da home page.|  
|HomepageLinkIcon|O nome do arquivo de ícone do link da home page. Este arquivo deve estar no mesmo local de pasta do arquivo .xml. As imagens do HomepageLinkIcon devem ter 16x16 pixels e devem estar no formato. png. Se você não fornecer um HomepageLinkIcon, será usada a imagem padrão do ícone de link da home page.|  
  
## <a name="see-also"></a>Consulte também  

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)

 [Criando e personalizando a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](../install/Testing-the-Customer-Experience.md)

