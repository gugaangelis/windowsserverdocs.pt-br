---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Criando um projeto de Link de Site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>Criando um projeto de Link de Site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crie um design de link de site para conectar seus sites com links de site. Links de site refletem a conectividade entre sites e o método usado para transferir o tráfego de replicação. Você deve conectar sites com links de site para que os controladores de domínio em cada site podem replicar alterações do Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Conectando sites com links de site  
Para se conectar a sites com links de site, identifique os sites de membro que você deseja se conectar com o link de site, crie um objeto de link de site no respectivo contêiner transportes entre sites e nomeie o link de site. Depois de criar o link de site, você poderá definir o site de propriedades do link.  
  
Ao criar links de site, certifique-se de que cada site está incluído em um link de site. Além disso, certifique-se de que todos os sites são conectados uns aos outros por meio de outros links de sites para que as alterações podem ser replicadas em controladores de domínio em qualquer site para todos os outros sites. Se você não fizer isso, uma mensagem de erro é gerada no log do serviço de diretório no Visualizador de eventos informando que a topologia de site não está conectada.  
  
Sempre que adicionar sites em um link de site recém-criado, determinar se o site que está sendo adicionado é um membro de outros links de site e alterar a participação do link do site do site se necessário. Por exemplo, se você fizer um site um membro do Default-First-Site-Link quando você cria inicialmente o site, certifique-se de remover o site da Default-First-Site-Link depois de adicionar o site para um novo link de site. Se você não remover o site do Default-First-Site-Link, o verificador de consistência de Conhecimento (KCC) fará decisões de roteamento com base na associação dos dois links de site, que pode resultar na roteamento incorreto.  
  
Para identificar os sites de membro que você deseja se conectar com um link de site, use a lista de locais e vinculados locais que você gravou na planilha (DSSTOPO_1.doc) "Localizações geográficas e comunicação Links". Se vários sites têm a conectividade e a disponibilidade para si mesmo, você pode conectá-los com o mesmo link de site.  
  
O contêiner de transportes entre sites fornece os meios para mapear links de site para o transporte que usa o link. Quando você cria um objeto de link de site, criá-lo no contêiner IP, que associa o link de site com a chamada de procedimento remoto (RPC) no transporte IP, ou o contêiner de SMTP Simple Mail Transfer Protocol (), que associa o link de site com o transporte SMTP.  
  
> [!NOTE]  
> Replicação SMTP não terão suporte nas versões futuras dos serviços de domínio do Active Directory (AD DS); Portanto, não é recomendável criar objetos de links de site no contêiner SMTP.  
  
Quando você cria um objeto de link de site no respectivo contêiner transportes entre sites, o AD DS usa RPC sobre IP para transferir a replicação entre sites e entre sites entre controladores de domínio. Para proteger dados em trânsito, replicação RPC sobre IP usa dois a Kerberos autenticação protocolo e criptografia de dados.  
  
Quando uma conexão direta de IP não estiver disponível, você pode configurar a replicação entre sites para usar SMTP. No entanto, a funcionalidade de replicação SMTP é limitada e requer uma autoridade de certificação (CA). SMTP só pode replicar a configuração, esquema e partições de diretório do aplicativo e não oferece suporte a replicação de partições de diretório.  
  
Para nomear links de site, use um esquema de nomenclatura consistente, como name_of_site1 name_of_site2. Registre a lista de sites, sites vinculados e os nomes dos links de site conectando esses sites em uma planilha. Para uma planilha para ajudá-lo a gravação de nomes de site e link site associado, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Abra (DSSTOPO_5.doc) "E associados Links de Sites".  
  
## <a name="in-this-guide"></a>Neste guia  
[Definindo propriedades do Link de Site](Setting-Site-Link-Properties.md)  
  


