---
title: Adicionar identidade visual ao painel, Acesso via Web remoto e Launchpad
description: Como adicionar materiais de identidade visual às telas de painel, Acesso via Web remota e Launchpad.
ms.date: 04/10/2014
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4d1b3ada8888de9885bfe88af56e5b0656fc92b7
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181602"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Adicionar Identidade Visual para o Dashboard, o Acesso Remoto da Web e a Barra Inicial

> Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode fazer várias adições de identidade visual acrescentando entradas no Registro. Todas as entradas de identidade visual no registro para o sistema operacional estão localizadas em `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM` .

Para fins de comarcação, o logotipo do Windows Server Essentials deve ter uma largura mínima de **170 pixels**, mantendo a taxa de proporção correta, com **96 DPI**.

## <a name="to-add-branding-by-changing-the-registry"></a>Para adicionar a identidade visual alterando o Registro

1. No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.

2. Na caixa de Pesquisa, digite **regedit** e, em seguida, clique no aplicativo **Regedit**.

3. No painel de navegação, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se a chave **OEM** não existir, você poderá criá-la:

    1. Clique com o botão direito do mouse em **Windows Server**, clique em **Novo** e em **Chave**.

    2. Digite **OEM** para o nome da chave.

4. Adicional Se você estiver criando uma entrada para um logotipo, poderá criar chaves diferentes para diferenciar as versões de idioma do logotipo. Por exemplo, se você tiver as versões em inglês e em alemão de um logotipo, é possível criar uma chave en-us e uma chave de-de. Como todos os arquivos de logotipo são armazenados na mesma pasta, é necessário fornecer instâncias do arquivo de imagem do logotipo com um nome exclusivo para cada idioma. Por exemplo, você cria um arquivo chamado DashboardLogo_en.png e DashboardLogo_de.png.

5. Clique com o botão direito do mouse em **OEM** ou clique com o botão direito do mouse na chave de idioma apropriada, clique em **novo**e em **valor da cadeia de caracteres**.

6. Digite o nome da cadeia de caracteres e, em seguida, pressione ENTER. Para obter mais informações sobre os nomes de cadeia de caracteres e os valores de dados, consulte a seção **valores e cadeias de registro** deste artigo.

7. Digite o nome da cadeia de caracteres e, em seguida, pressione ENTER. Para obter mais informações sobre os nomes de cadeia de caracteres e os valores de dados, consulte a seção **valores e cadeias de registro** deste artigo.

8. Clique com o botão direito do mouse na nova cadeia de caracteres e depois em **Modificar**.

9. Digite o valor da tabela associada ao nome da cadeia de caracteres e clique em **OK**.

10. Se você estiver criando uma entrada para uma imagem de logotipo ou links anexados, copie o arquivo para `%programFiles%\Windows Server\Bin\OEM` . Se o diretório OEM não existir, crie-o.

11. Se você fizer alterações no Acesso via Web remoto depois que o cliente tiver a posse do servidor, ele deverá ativar o Acesso via Web remoto:

    1. No Dashboard, clique em **Configurações** e na guia **Acesso em Qualquer Local**.

    2. Se o **acesso em qualquer local** estiver ativado, clique em **Configurar**e, em seguida, desmarque a caixa de seleção Acesso via Web remota na página **escolher recursos de acesso em qualquer local para habilitar** o assistente de **configuração de acesso em qualquer local** .

    3. Clique em **Configurar**.

### <a name="registry-strings-and-values"></a>Valores e cadeias de caracteres do Registro

A tabela a seguir fornece informações sobre os nomes de cadeia de caracteres disponíveis e os valores de dados.

| Local de identidade visual | Descrição | Nome da cadeia de caracteres | Valor de dados |
|--|--|--|--|
| Logotipo do Dashboard | Adiciona a imagem do logotipo ao Dashboard. O logotipo do painel deve estar no formato .png e não deve exceder 350 pixels de largura por 38 de altura.<p>**Importante:** Para comarcar o painel com o logotipo, você deve editar o bloco de trabalho artístico que é fornecido no DVD do OPK e acrescentar o logotipo da empresa à imagem, ao seguir os requisitos de espaço em branco apropriado. | DashboardLogo | Insira o nome do arquivo de imagem do logotipo. |
| DashboardClientLogo | Adiciona a imagem de logotipo à tela de entrada do cliente do painel. | DashboardClientLogo | Insira o nome do arquivo de imagem do logotipo. |
| Imagem de segundo plano do site | Altera a imagem de plano de fundo exibida na página de entrada do Acesso via Web remoto. As resoluções mais frequentes aparecerão da seguinte forma:<ul><li>**1024x768 pixels** -essa resolução preenche a página de entrada com precisão.</li><li>**800x600 pixels** -essa resolução centraliza a imagem na página e aparece com uma borda preta.</li><li>**1280x720 pixels** -essa resolução centraliza a imagem. Qualquer pixel que exceda 1024x720 não aparecerá. | LogonBackground | Insira o nome do arquivo de imagem de plano de fundo. |
| Título do site | Substitui o título do site Acesso via Web remoto do Windows Server Essentials ao título especificado. | WebsiteName | Insira o novo título do site de Acesso via Web remoto. |
| Logotipo do site | Altera o logotipo padrão do site do Acesso Remoto da Web. O tamanho esperado do logotipo é 32 pixels por 32 pixels. Se o seu logotipo for menor ou maior do que isso, ele será ampliado ou reduzido para corresponder a essas dimensões. | WebsiteLogo | Insira o nome do arquivo de imagem do logotipo |
| Logotipo do site anexado | O logotipo do seu parceiro será exibido logo abaixo do logotipo da Microsoft que aparece no site Acesso via Web remoto. O tamanho esperado do logotipo é 200 pixels de altura por 50 pixels de largura. Se o seu logotipo for maior do que isso, ele será reduzido para se ajustar, sendo mantida a taxa de proporção original. Se o seu logotipo for menor do que isso, ele será centralizado no espaço de 200 por 50 pixels, e a taxa de proporção e o tamanho não serão alterados. | OEMLogo | Insira o nome do arquivo de imagem do logotipo. |
| Links na página inicial do site e na página de entrada | Acrescenta links à página de entrada e home page do site de Acesso via Web remoto. O arquivo XML que contém as informações de link deve estar localizado na `%programFiles%\Windows Server\Bin\OEM` pasta. | LinksXML | Consulte a tabela Elementos de LinksXML para uma lista de elementos e descrições. |
| Logotipo da Barra Inicial | Adiciona a imagem do logotipo para a Barra Inicial. O logotipo da Barral Inicial deve estar no formato .png e não deve ser mais alto que 64 pixels. | LaunchpadLogo | Insira o nome do arquivo de imagem do logotipo. |

#### <a name="xml-linking-format"></a>Formato de vinculação XML

Você deve formatar os links na página **inicial** do site e na página de entrada como:

```xml
<OemLinks>
    <LogonLinks>
        <Link Name=LogonLinkName>
        <Text>LogonLinkDescription</Text>
        <Url>LogonLinkURL</Url>
        <Icon>LinkIcon</Icon>
        </Link>
    </LogonLinks>

    <HomepageLinks>
        <Link Name=HomepageLinkName>
        <Text>HomepageLinkDescription</Text>
        <Url>HomepageLinkURL</Url>
        <Icon>HomepageLinkIcon</Icon>
        </Link>
    </HomepageLinks>
</OemLinks>
```

### <a name="linksxml-elements"></a>Elementos de LinksXML

| Elemento LinksXML | Descrição |
|--|--|
| LogonLinks | A entrada pai para links de entrada. |
| Nome do vínculo | O nome do link de entrada. |
| Texto | O texto exibido como o link da página de entrada. |
| URL | A URL que é resolvida para o link da página de entrada. |
| Ícone | O nome do arquivo de ícone para o link de entrada. Este arquivo deve estar no mesmo local de pasta do arquivo .xml. As imagens de ícone devem ter 16x16 pixels e no formato. png. Se você não fornecer um ícone, a imagem do ícone de link padrão será usada. |
| HomepageLinks | A entrada pai da **Home** Page. |
| Nome do vínculo | O nome do link da **Home** Page. |
| Texto | O texto que aparece como o link da **Home** Page. |
| URL | A URL que é resolvida para o link da **Home** Page. |
| Ícone | O nome do arquivo de ícone para o link da **Home** Page. Este arquivo deve estar no mesmo local de pasta do arquivo .xml. As imagens de ícone devem ter 16x16 pixels e no formato. png. Se você não fornecer um ícone, a imagem do ícone do link da página **inicial** padrão será usada. |

## <a name="additional-references"></a>Referências adicionais

- [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)

- [Personalizações adicionais](Additional-Customizations.md)

- [Preparar a imagem para implantação](Preparing-the-Image-for-Deployment.md)

- [Testar a experiência do usuário](Testing-the-Customer-Experience.md)