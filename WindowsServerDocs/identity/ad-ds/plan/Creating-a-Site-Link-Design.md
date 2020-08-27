---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Criar um design de link de site
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 881ca5f2d932a8e13aaa7467179360ca8bb4af66
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941136"
---
# <a name="creating-a-site-link-design"></a>Criar um design de link de site

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crie um design de link de site para conectar seus sites com links de site. Os links de site refletem a conectividade entre sites e o método usado para transferir o tráfego de replicação. Você deve conectar sites com links de site para que os controladores de domínio em cada site possam replicar Active Directory alterações.

## <a name="connecting-sites-with-site-links"></a>Conectando sites com links de site

Para conectar sites com links de site, identifique os sites membros que você deseja conectar com o link do site, crie um objeto de link de site no respectivo contêiner Transportes entre sites e, em seguida, nomeie o link do site. Depois de criar o link do site, você pode continuar a definir as propriedades do link do site.

Ao criar links de site, verifique se todos os sites estão incluídos em um link de site. Além disso, verifique se todos os sites estão conectados entre si por meio de outros links de site para que as alterações possam ser replicadas de controladores de domínio em qualquer site para todos os outros sites. Se você não conseguir fazer isso, uma mensagem de erro será gerada no log do serviço de diretório no Visualizador de Eventos informando que a topologia do site não está conectada.

Sempre que você adicionar sites a um link de site recém-criado, determine se o site que está sendo adicionado é membro de outros links de site e altere a associação de link de site do site, se necessário. Por exemplo, se você fizer com que um site seja membro do link padrão-primeiro-site ao criar inicialmente o site, certifique-se de remover o site do link de primeiro site padrão depois de adicionar o site a um novo link de site. Se você não remover o site do link de primeiro site padrão, o Knowledge Consistency Checker (KCC) fará decisões de roteamento com base na associação de ambos os links de site, o que pode resultar em roteamento incorreto.

Para identificar os sites membros que você deseja conectar a um link de site, use a lista de locais e locais vinculados que você registrou na planilha "locais geográficos e links de comunicação" (DSSTOPO_1.doc). Se vários sites tiverem a mesma conectividade e disponibilidade entre si, você poderá conectá-los com o mesmo link de site.

O contêiner Transportes entre sites fornece os meios para mapear links de site para o transporte usado pelo link. Ao criar um objeto de link de site, você o cria no contêiner IP, que associa o link de site à RPC (chamada de procedimento remoto) sobre o transporte de IP ou o contêiner SMTP, que associa o link de site ao transporte SMTP.

> [!NOTE]
> A replicação SMTP não terá suporte em versões futuras do Active Directory Domain Services (AD DS); Portanto, a criação de objetos de links de site no contêiner SMTP não é recomendada.

Quando você cria um objeto de link de site no respectivo contêiner de transportes entre sites, AD DS usa RPC sobre IP para transferir a replicação entre sites e intra-site entre controladores de domínio. Para manter os dados protegidos em trânsito, a replicação de RPC sobre IP usa o protocolo de autenticação Kerberos e a criptografia de dados.

Quando uma conexão IP direta não está disponível, você pode configurar a replicação entre sites para usar o SMTP. No entanto, a funcionalidade de replicação SMTP é limitada e requer uma autoridade de certificação (CA) corporativa. O SMTP só pode replicar a configuração, o esquema e as partições de diretório de aplicativo e não oferece suporte à replicação de partições de diretório de domínio.

Para nomear links de site, use um esquema de nomenclatura consistente, como name_of_site1 name_of_site2. Registre a lista de sites, sites vinculados e os nomes dos links de site que conectam esses sites em uma planilha. Para uma planilha para ajudá-lo a registrar nomes de site e nomes de links de site associados, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir "sites e links de site associados" (DSSTOPO_5.doc).

## <a name="in-this-guide"></a>Neste guia

[Como definir propriedades do link de site](Setting-Site-Link-Properties.md)
