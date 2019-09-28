---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Implantando servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 785dcad4ac8e03cc59730fb533e30a001569dd63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359634"
---
# <a name="deploying-federation-servers"></a>Implantando servidores de federação

Para implantar servidores de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1, conclua cada uma das tarefas em [Checklist: Configurando um servidor de Federação @ no__t-0.  
  
> [!NOTE]  
> Ao usar essa lista de verificação, recomendamos que você leia primeiro as referências ao planejamento do servidor de Federação no [Guia de design de AD FS no Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de iniciar os procedimentos para configurar os servidores. Seguir a lista de verificação dessa maneira fornece uma melhor compreensão do processo de design e implantação para servidores de Federação.  
  
## <a name="about-federation-servers"></a>Sobre servidores de Federação  
Os servidores de Federação são computadores que executam o Windows Server 2008 com o software AD FS instalado que foram configurados para atuar na função de servidor de Federação. Os servidores de Federação autenticam ou roteiam solicitações de contas de usuário em outras organizações e de computadores cliente que podem estar localizados em qualquer lugar da Internet.  
  
O ato de instalar o software AD FS em um computador e usar o assistente de configuração do servidor de Federação AD FS para configurá-lo para a função de servidor de Federação torna esse computador um servidor de Federação. Ele também torna o snap do AD FS Management @ no__t-0in disponível nesse computador no menu **Start @ no__t-2Administrative Tools @ no__t-3** para que você possa especificar o seguinte:  
  
-   O nome do host AD FS em que as organizações e os aplicativos parceiros enviarão solicitações e respostas de token  
  
-   O identificador de AD FS que as organizações e os aplicativos parceiros usarão para identificar o nome exclusivo ou o local da sua organização  
  
-   O certificado @ no__t-0signing de token que todos os servidores de Federação em um farm de servidores usarão para emitir e assinar tokens  
  
-   O local das páginas da Web ASP.NET personalizadas para logon de cliente, logoff e descoberta de parceiro de conta que aprimorará a experiência do cliente  
  
    > [!NOTE]  
    > A maioria dessas configurações principais de interface do usuário \(UI @ no__t-1 estão contidas no arquivo Web. config em cada servidor de Federação. O nome do host AD FS e os valores do identificador de AD FS não são especificados no arquivo Web. config.  
  
Os servidores de Federação hospedam um mecanismo de emissão de declarações que emite tokens com base nas credenciais \(for exemplo, nome de usuário e senha @ no__t-1 que são apresentados a ele. Um token de segurança é uma unidade de dados assinada criptograficamente que expressa uma ou mais declarações. Uma declaração é uma instrução que um servidor faz \(for exemplo, nome, identidade, chave, grupo, privilégio ou capacidade @ no__t-1 sobre um cliente. Depois que as credenciais são verificadas no servidor de Federação \(through o processo de logon do usuário @ no__t-1, as declarações para o usuário são coletadas por meio do exame dos atributos de usuário armazenados no repositório de atributos especificado.  
  
No Federated Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 designs \(AD FS designs nos quais duas ou mais organizações estão envolvidas @ no__t-5, as declarações podem ser modificadas por regras de declaração para uma terceira parte confiável específica. As declarações são criadas em um token que é enviado para um servidor de Federação na organização do parceiro de recurso. Depois que um servidor de Federação no parceiro de recurso recebe as declarações como declarações de entrada, ele executa o mecanismo de emissão de declarações para executar um conjunto de regras de declaração para filtrar, passar ou transformar essas declarações. As declarações são então criadas em um novo token que é enviado para o servidor Web no parceiro de recurso.  
  
No design de SSO da Web \(an AD FS design no qual apenas uma organização está envolvida @ no__t-1, um único servidor de Federação pode ser usado para que os funcionários possam fazer logon uma vez e ainda acessar vários aplicativos.  
  
