---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Implantando servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847167"
---
# <a name="deploying-federation-servers"></a>Implantando servidores de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implantar servidores de federação nos serviços de Federação do Active Directory \(do AD FS\), preencha cada uma das tarefas no [lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando você usa essa lista de verificação, é recomendável que você leia primeiro as referências ao planejamento do servidor de federação na [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de começar os procedimentos para configurar os servidores. A lista de verificação dessa maneira a seguir fornece uma melhor compreensão do processo de design e implantação para servidores de Federação.  
  
## <a name="about-federation-servers"></a>Sobre servidores de Federação  
Servidores de Federação são computadores que executam o Windows Server 2008 com o software AD FS instalado e que tenham sido configurados para atuar na função de servidor de Federação. Servidores de Federação autenticar ou rotear solicitações de contas de usuário em outras organizações e de computadores cliente que podem ser localizados em qualquer lugar na Internet.  
  
O ato de instalar o software AD FS em um computador e usar o Assistente de configuração do servidor de Federação do AD FS para configurá-lo para a função de servidor de Federação torna esse computador um servidor de Federação. Ele também torna o snap de gerenciamento do AD FS\-em disponíveis nesse computador o **iniciar\\ferramentas administrativas\\**  menu para que você possa especificar o seguinte:  
  
-   O nome do host do AD FS onde aplicativos e organizações parceiras enviará solicitações de token e respostas  
  
-   O identificador do AD FS que as organizações e os aplicativos de parceiro será usado para identificar o nome exclusivo ou o local da sua organização  
  
-   O token\-certificado usarão todos os servidores de Federação em um farm de servidores para tokens de problema e entrada de assinatura  
  
-   O local das páginas da Web do ASP.NET personalizadas para descoberta de parceiro de conta que melhorarão a experiência do cliente, fazer logoff e logon do cliente  
  
    > [!NOTE]  
    > A maioria dessas principais de interface do usuário \(interface do usuário\) as configurações estão contidas no arquivo Web. config em cada servidor de Federação. O nome de host do AD FS e os valores do identificador do AD FS não são especificados no arquivo Web. config.  
  
Um mecanismo de emissão de declarações que emite tokens com base nas credenciais de host de servidores de Federação \(por exemplo, o nome de usuário e senha\) que são apresentados a ele. Um token de segurança é uma unidade de dados criptografada que expressa uma ou mais declarações. Uma declaração é uma instrução que faz com que um servidor \(por exemplo, nome, identidade, chave, grupo, privilégio ou recurso\) sobre um cliente. Depois que as credenciais são verificadas no servidor de Federação \(durante o processo de logon do usuário\), declarações para o usuário são coletadas por meio do exame dos atributos de usuário são armazenados no armazenamento de atributo especificado.  
  
No único de Web federado\-sinal\-na \(SSO\) designs \(designs de AD FS na qual duas ou mais organizações estão envolvidas\), declarações podem ser modificadas por regras de declaração para uma terceira parte confiável específica de terceiros. As declarações são criadas em um token que é enviado para um servidor de federação na organização do parceiro de recurso. Depois que um servidor de federação no parceiro de recurso recebe as declarações como declarações de entrada, ele executa o mecanismo de emissão de declarações para executar um conjunto de regras de declaração para filtrar, passagem ou transformar essas declarações. As declarações são então compiladas em um novo token que é enviado para o servidor Web no parceiro de recurso.  
  
No design SSO da Web \(um design do AD FS em que uma única organização está envolvida\), um servidor de Federação único pode ser usado para que os funcionários podem fazer logon uma vez e ainda acessar vários aplicativos.  
  
