---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: "Criptografia baseada em classificação cenário para documentos do Office"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Cenário: A criptografia baseada em classificação para documentos do Office

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Proteção de informações confidenciais é principalmente sobre reduz o risco para a organização. Vários regulamentos de conformidade, como a portabilidade de seguros de saúde e responsabilidade Act (HIPAA) e pagamento Card Industry Data Security Standard (PCI DSS), ditam criptografia de informações, e há várias razões comerciais para criptografar informações comerciais confidenciais. No entanto, criptografar informações é cara, e ele pode prejudicar a produtividade de negócios. Assim, as organizações tendem a ter diferentes abordagens e prioridades para criptografar as informações deles.  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
 Windows Server 2012 fornece a capacidade para criptografar automaticamente os arquivos do Microsoft Office confidenciais, com base em suas classificações. Isso é feito por meio de tarefas de gerenciamento de arquivos que invocar proteção Active Directory Rights Management Services (AD RMS) de documentos confidenciais por alguns segundos depois que o arquivo é identificado como sendo um arquivo confidencial no servidor de arquivos. Isso é mais fácil por tarefas de gerenciamento contínuo de arquivo no servidor de arquivos.  
  
Criptografia do AD RMS fornece outra camada de proteção de arquivos. Mesmo que uma pessoa com acesso a um arquivo confidencial inadvertidamente envia esse arquivo por email, o arquivo está protegido pela criptografia do AD RMS. Os usuários que desejam para acessar o arquivo devem primeiro autenticar para um servidor de AD RMS para receber a chave de descriptografia. A figura a seguir mostra esse processo.  
  
![guias de soluções](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figura 6** proteção baseada em classificação RMS  
  
Suporte para formatos de arquivo não são da Microsoft está disponível por meio de fornecedores não são da Microsoft. Depois que um arquivo foi protegido pela criptografia de AD RMS, recursos de gerenciamento de dados, como a classificação baseado em conteúdo ou pesquisa não estão mais disponíveis para esse arquivo.  
  
## <a name="in-this-scenario"></a>Neste cenário  
A seguir é a orientação que está disponível para este cenário:  
  
-   [Considerações de planejamento para criptografia de documentos do Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Implantar a criptografia de arquivos do Office & #40; etapas de demonstração & #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e recursos que fazem parte desse cenário e descreve como suporte.  
  
|Função/recurso|Como ele dá suporte a esse cenário|  
|-----------------|---------------------------------|  
|Função de serviços de domínio do diretório ativa (AD DS)|O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos de rede e dados específicos do aplicativo de aplicativos habilitados para diretórios. Nesse cenário, o AD DS no Windows Server 2012 apresenta uma plataforma de autorização baseada em declarações que permite a criação de declarações do usuário e declarações de dispositivo, composta identidade (usuário e declarações de dispositivo), um novo modelo de políticas de acesso central e o uso das informações de classificação de arquivos nas decisões de autorização.|  
|Função Serviços de arquivo e armazenamento<br /><br />Gerenciador de recursos do servidor de arquivos|Serviços de arquivo e armazenamento fornece tecnologias para ajudar você a configurar e gerenciar um ou mais servidores de arquivos que fornecem locais centrais em sua rede onde você pode armazenar arquivos e compartilhá-los com os usuários. Se os usuários da rede precisam acessar os mesmos arquivos e aplicativos, ou se o gerenciamento centralizado de backup e arquivo é importante para sua organização, você deve configurar um ou mais computadores como um servidor de arquivos, adicionando a função Serviços de arquivo e armazenamento e os serviços de função apropriada para os computadores. Nesse cenário, os administradores de servidor de arquivos podem configurar tarefas de gerenciamento de arquivos que invoca a proteção do AD RMS de documentos confidenciais por alguns segundos depois que o arquivo é identificado como sendo um arquivo confidencial no servidor de arquivos (tarefas de gerenciamento de arquivos contínuo no servidor de arquivos).|  
|Função do Active Directory Rights Management Services (AD RMS)|AD RMS permite indivíduos e administradores (por meio de políticas de gerenciamento de direitos de informação (IRM)) para especificar as permissões de acesso a documentos, pastas de trabalho e apresentações. Isso ajuda a evitar que informações confidenciais sejam impressas, encaminhadas ou copiadas por pessoas não autorizadas. Depois de permissão para um arquivo foi limitado usando o IRM, as restrições de acesso e uso são impostas não importa onde as informações estiverem, como a permissão para um arquivo é armazenada no próprio arquivo do documento. Nesse cenário, a criptografia de AD RMS fornece outra camada de proteção de arquivos. Mesmo que uma pessoa com acesso a um arquivo confidencial inadvertidamente envia esse arquivo por email, o arquivo está protegido pela criptografia do AD RMS. Os usuários que desejam para acessar o arquivo devem primeiro autenticar para um servidor de AD RMS para receber a chave de descriptografia.|  
  


