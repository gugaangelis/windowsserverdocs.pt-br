---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: Criptografia baseada em classificação de cenário para documentos do Office
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865177"
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Cenário: criptografia baseada em classificação para documentos do Office

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A proteção de informações confidenciais se refere principalmente à minimização dos riscos para a organização. Várias regulamentações de conformidade, como HIPAA (Health Insurance Portability and Accountability Act) e PCI-DSS (Payment Card Industry Data Security Standard), ditam a criptografia das informações, e há inúmeros motivos dos negócios para criptografar as informações confidenciais. Entretanto, criptografar as informações é caro e pode prejudicar a produtividade dos negócios. Assim, as organizações tendem a adotar diferentes abordagens e prioridades para criptografar suas informações.  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
 Windows Server 2012 fornece a capacidade de criptografar automaticamente os arquivos confidenciais do Microsoft Office, com base em sua classificação. Isso é feito através de tarefas de gerenciamento que invocam a proteção AD RMS (Serviços de Gerenciamento de Direitos do Active Directory) para documentos confidenciais alguns segundos depois que um arquivo é identificado como confidencial no servidor de arquivos. Isso se torna mais fácil por meio de tarefas contínuas de gerenciamento de arquivos no servidor de arquivos.  
  
A criptografia de AD RMS fornece outra camada de proteção aos arquivos. Mesmo se uma pessoa com acesso a um arquivo confidencial inadvertidamente enviar este arquivo por email, o arquivo estará protegido pela criptografia do AD RMS. Usuários que desejam acessar o arquivo deverão primeiro autenticar-se em um servidor AD RMS para receber a chave de descriptografia. A figura a seguir mostra esse processo.  
  
![guias de soluções](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figura 6** Proteção RMS baseada em classificação  
  
O suporte a formatos de arquivo não Microsoft está disponível através de outros fornecedores. Depois de um arquivo ser protegido pela criptografia AD RMS, recursos de gerenciamento de dados, como classificação baseada em pesquisa ou conteúdo, não estarão mais disponíveis para o arquivo em questão.  
  
## <a name="in-this-scenario"></a>Neste cenário  
A seguir estão as diretrizes disponíveis para este cenário:  
  
-   [Considerações de planejamento para criptografia de documentos do Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [A implantação da criptografia de arquivos do Office &#40;etapas de demonstração&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e os recursos que fazem parte deste cenário e descreve como dar suporte a ele.  
  
|Função/recurso|Como este cenário tem suporte|  
|-----------------|---------------------------------|  
|Função de AD DS (Serviços de Domínio Active Directory)|O AD DS fornece um banco de dados distribuído que armazena e gerencia informações sobre recursos da rede e dados específicos de aplicativos habilitados por diretório. Nesse cenário, o AD DS no Windows Server 2012 apresenta uma plataforma de autorização baseada em declarações que permite a criação de declarações de usuário e declarações de dispositivo, identidades compostas (declarações de usuário + dispositivo), um novo modelo de políticas de acesso central e o uso de informações de classificação de arquivos em decisões de autorização.|  
|Função dos Serviços de Arquivo e Armazenamento<br /><br />Gerenciador de Recursos de Servidor de Arquivos|Os Serviços de Arquivo e Armazenamento oferecem tecnologias que ajudam a configurar e gerenciar um ou mais servidores de arquivos, os quais são servidores que fornecem localizações centrais na rede onde é possível armazenar arquivos e compartilhá-los com os usuários. Se os usuários da rede precisam ter acesso aos mesmos arquivos e aplicativos, ou se o backup centralizado e gerenciamento de arquivos são importantes para sua organização, você deve configurar um ou mais computadores como servidor de arquivos, adicionando a função de Serviços de Arquivo e Armazenamento e os serviços de função apropriados aos computadores. Neste cenário, os administradores do servidor de arquivos podem configurar as tarefas de gerenciamento de arquivo que invocam a proteção do AD RMS para documentos confidenciais alguns segundos depois de o arquivo ser identificado como confidencial no servidor de arquivos (tarefas de gerenciamento de arquivos contínuo no servidor de arquivos).|  
|Função do AD RMS (Active Directory Rights Management Services)|O AD RMS permite que indivíduos e administradores, por meio de políticas de IRM (Gerenciamento de Direitos de Informação), para especificar as permissões de acesso a documentos, livros e apresentações. Isso ajuda a evitar que informações confidenciais sejam impressas, encaminhadas ou copiadas por pessoas não autorizadas. Depois que a permissão de um arquivo é restrita usando o IRM, as restrições de acesso e uso são aplicadas, independentemente de onde as informações estejam, já que a permissão a um arquivo é armazenada no arquivo de documento em si. Neste cenário, a criptografia de AD RMS fornece outra camada de proteção aos arquivos. Mesmo se uma pessoa com acesso a um arquivo confidencial inadvertidamente enviar este arquivo por email, o arquivo estará protegido pela criptografia do AD RMS. Usuários que desejam acessar o arquivo deverão primeiro autenticar-se em um servidor AD RMS para receber a chave de descriptografia.|  
  


