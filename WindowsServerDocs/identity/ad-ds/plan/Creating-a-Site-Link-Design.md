---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Criar um design de link de site
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4e0607cf66d41e1747b108a3ecc10562120d9174
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861927"
---
# <a name="creating-a-site-link-design"></a>Criar um design de link de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crie um design de link de site para se conectar a seus sites com links de site. Links de site refletem a conectividade entre sites e o método usado para transferir o tráfego de replicação. Você deve conectar sites com links de site para que os controladores de domínio em cada site podem replicar alterações do Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Conectando sites com links de site

Para se conectar a sites com links de site, identifique os sites de membro que você deseja conectar-se com o link de site, criar um objeto de link de site no respectivo contêiner de transportes entre sites e, em seguida, nomeie o link de site. Depois de criar o link de site, você pode prosseguir para definir propriedades do link de site.  
  
Ao criar links de site, certifique-se de que todos os sites está incluído em um link de site. Além disso, certifique-se de que todos os sites são conectados uns aos outros por meio de outros links de site para que as alterações possam ser replicadas de controladores de domínio em qualquer site para todos os outros sites. Se você não conseguir fazer isso, uma mensagem de erro será gerada no log do serviço de diretório no Visualizador de eventos, informando que a topologia de site não está conectada.  
  
Sempre que você pode adicionar sites a um link de site recém-criado, determinar se o site que está sendo adicionado é um membro de outros links de site e alterar a associação de link de site do site, se necessário. Por exemplo, se você fizer um site do membro padrão-First-Site-Link quando você cria inicialmente o site, certifique-se de remover o site do padrão-First-Site-Link depois de adicionar o site para um novo link de site. Se você não remover o site do padrão-First-Site-Link, o Knowledge Consistency Checker (KCC) será tomar decisões de roteamento com base na associação de ambos os links de site, que pode resultar em roteamento incorreto.  
  
Para identificar os sites de membro que você deseja se conectar com um link de site, use a lista de locais e vinculados locais que você registrou na planilha "Geográficas locais e Links de comunicação" (DSSTOPO_1.doc). Se vários sites têm a mesma conectividade e disponibilidade entre si, você pode conectá-los com o mesmo link de site.  
  
O contêiner de transportes entre sites fornece os meios para mapeamento de links de site para o transporte que usa o link. Quando você cria um objeto de link de site, você criá-lo em contêiner IP, que associa o link de site com a chamada de procedimento remoto (RPC) no transporte IP, ou o contêiner de SMTP Simple Mail Transfer Protocol (), que associa o link de site com o SMTP transporte.  
  
> [!NOTE]  
> A replicação SMTP não terão suporte em versões futuras dos serviços de domínio Active Directory (AD DS); Portanto, não é recomendável criar objetos de links de site no contêiner de SMTP.  
  
Quando você cria um objeto de link de site no respectivo contêiner de transportes entre sites, o AD DS usa RPC sobre IP para transferir a replicação entre sites e intra-site entre controladores de domínio. Para proteger dados em trânsito, a replicação RPC sobre IP usa os dois a Kerberos autenticação protocolo e criptografia de dados.  
  
Quando uma conexão IP direta não estiver disponível, você pode configurar a replicação entre sites para usar o SMTP. No entanto, a funcionalidade de replicação SMTP é limitada e requer uma autoridade de certificação corporativa (CA). SMTP só pode replicar a configuração, esquema e as partições de diretório de aplicativo e não oferece suporte a replicação das partições de diretório de domínio.  
  
Para nomear os links de site, use um esquema de nomenclatura consistente, como name_of_site1 name_of_site2. Registre a lista de sites, sites vinculados e os nomes dos links de site se conectar a esses sites em uma planilha. Para uma planilha para ajudá-lo no registro de nomes de site e os nomes de link de site associado, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Abra "Sites e Links de Site associados" (DSSTOPO_5.doc).  
  
## <a name="in-this-guide"></a>Neste guia

[Definindo propriedades de Link de Site](Setting-Site-Link-Properties.md)  
