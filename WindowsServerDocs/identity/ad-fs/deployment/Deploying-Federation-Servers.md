---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Implantando servidores de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9e66c513c34658c40152cd7b622a1818e7198e47
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855559"
---
# <a name="deploying-federation-servers"></a>Implantando servidores de federação

Para implantar servidores de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS\), conclua cada uma das tarefas na [lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Ao usar essa lista de verificação, recomendamos que você leia primeiro as referências ao planejamento do servidor de Federação no [Guia de design de AD FS no Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de iniciar os procedimentos para configurar os servidores. Seguir a lista de verificação dessa maneira fornece uma melhor compreensão do processo de design e implantação para servidores de Federação.  
  
## <a name="about-federation-servers"></a>Sobre servidores de Federação  
Os servidores de Federação são computadores que executam o Windows Server 2008 com o software AD FS instalado que foram configurados para atuar na função de servidor de Federação. Os servidores de Federação autenticam ou roteiam solicitações de contas de usuário em outras organizações e de computadores cliente que podem estar localizados em qualquer lugar da Internet.  
  
O ato de instalar o software AD FS em um computador e usar o assistente de configuração do servidor de Federação AD FS para configurá-lo para a função de servidor de Federação torna esse computador um servidor de Federação. Ele também torna o snap de gerenciamento de AD FS\-disponível nesse computador no menu **iniciar\\ferramentas administrativas\\** para que você possa especificar o seguinte:  
  
-   O nome do host AD FS em que as organizações e os aplicativos parceiros enviarão solicitações e respostas de token  
  
-   O identificador de AD FS que as organizações e os aplicativos parceiros usarão para identificar o nome exclusivo ou o local da sua organização  
  
-   O token\-certificado de assinatura que todos os servidores de Federação em um farm de servidores usarão para emitir e assinar tokens  
  
-   O local das páginas da Web ASP.NET personalizadas para logon de cliente, logoff e descoberta de parceiro de conta que aprimorará a experiência do cliente  
  
    > [!NOTE]  
    > A maioria dessas configurações de interface do usuário \(UI\) está contida no arquivo Web. config em cada servidor de Federação. O nome do host AD FS e os valores do identificador de AD FS não são especificados no arquivo Web. config.  
  
Os servidores de Federação hospedam um mecanismo de emissão de declarações que emite tokens com base nas credenciais \(por exemplo, o nome de usuário e a senha\) que são apresentados a ele. Um token de segurança é uma unidade de dados assinada criptograficamente que expressa uma ou mais declarações. Uma declaração é uma instrução que um servidor faz \(por exemplo, nome, identidade, chave, grupo, privilégio ou capacidade\) sobre um cliente. Depois que as credenciais são verificadas no servidor de Federação \(por meio do processo de logon do usuário\), as declarações para o usuário são coletadas por meio do exame dos atributos de usuário que são armazenados no repositório de atributos especificado.  
  
No logon único de\-da Web federado\-em \(designs de\) SSO \(AD FS designs nos quais duas ou mais organizações estão envolvidas\), as declarações podem ser modificadas por regras de declaração para uma terceira parte confiável específica. As declarações são criadas em um token que é enviado para um servidor de Federação na organização do parceiro de recurso. Depois que um servidor de Federação no parceiro de recurso recebe as declarações como declarações de entrada, ele executa o mecanismo de emissão de declarações para executar um conjunto de regras de declaração para filtrar, passar ou transformar essas declarações. As declarações são então criadas em um novo token que é enviado para o servidor Web no parceiro de recurso.  
  
No design de SSO da Web \(um design de AD FS no qual apenas uma organização está envolvida\), um único servidor de Federação pode ser usado para que os funcionários possam fazer logon uma vez e ainda acessar vários aplicativos.  
  
