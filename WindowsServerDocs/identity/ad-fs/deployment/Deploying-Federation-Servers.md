---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: "Implantando servidores de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-servers"></a>Implantando servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implantar servidores de Federação em serviços de Federação do Active Directory \(AD FS\), concluir cada uma das tarefas em [lista de verificação: configuração de um servidor federação](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando você usa essa lista de verificação, recomendamos que você leia primeiro as referências ao servidor de Federação planejamento no [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de começar os procedimentos para configurar os servidores. A lista de verificação dessa forma a seguir fornece uma melhor compreensão do processo de design e implantação para servidores de Federação.  
  
## <a name="about-federation-servers"></a>Sobre os servidores de Federação  
Servidores de Federação são computadores que executam o Windows Server 2008 com o software do AD FS instalado e que foram configurados para atuar na função de servidor de Federação. Servidores de federação autenticam ou rotear solicitações de contas de usuário em outras organizações em computadores cliente que podem estar localizados em qualquer lugar na Internet.  
  
O ato de instalar o software do AD FS em um computador e usar o Assistente para configuração do servidor de Federação do AD FS para configurá-lo para a função de servidor de Federação torna esse computador um servidor de Federação. Isso também torna o gerenciamento do AD FS snap\-in disponíveis nesse computador o **Start\\Administrative Tools\\** menu para que você possa especificar o seguinte:  
  
-   O nome de host do AD FS onde aplicativos e organizações parceiras enviará respostas e solicitações de token  
  
-   O identificador do AD FS que as organizações e os aplicativos de parceiro usará para identificar o nome exclusivo ou o local da sua organização  
  
-   O certificado de assinatura de token\ que todos os servidores de Federação em um farm de servidores usará para tokens de problema e entrada  
  
-   A localização de páginas da Web de ASP.NET personalizado para descoberta de parceiros de conta melhorar a experiência do cliente, logoff e logon do cliente  
  
    > [!NOTE]  
    > A maioria dessas configurações de \(UI\) de interface de usuário principais estão contidos no arquivo Web. config em cada servidor de Federação. O nome de host do AD FS e valores de identificador do AD FS não são especificados no arquivo Web. config.  
  
Servidores de Federação hospedarem um mecanismo de emissão de declarações que emite tokens baseados nas credenciais \ (por exemplo, nome de usuário e password\) que são apresentadas a ele. Um token de segurança é uma unidade de dados assinados criptograficamente que expressa uma ou mais declarações. Uma declaração é uma instrução que faz com que um servidor \ (por exemplo, nome, identidade, chave, grupo, privilégio ou capability\) sobre um cliente. Depois que as credenciais forem verificadas no servidor de Federação \ (por meio do process\ de logon do usuário), declarações para o usuário são coletadas através do exame dos atributos de usuário que são armazenadas no repositório de atributo especificado.  
  
Em federados da Web Single\-Sign\-On \(SSO\) designs \ (designs do AD FS em que as organizações de dois ou mais são involved\), declarações podem ser modificadas pelo reivindicação regras para um terceiro específico. As declarações são criadas em um token que é enviado para um servidor de federação na organização do parceiro de recurso. Depois que um servidor de federação no parceiro de recurso recebe as declarações como declarações de entrada, ele executa o mecanismo de emissão de declarações para executar um conjunto de regras de declaração para filtrar, passagem ou transformar essas declarações. As declarações são criadas, em seguida, em um novo token que é enviado para o servidor Web no parceiro de recurso.  
  
No design da Web SSO \ (um design AD FS em que apenas uma organização é involved\), um servidor de Federação único pode ser usado para que os funcionários podem fazer logon uma vez e ainda acessar vários aplicativos.  
  
