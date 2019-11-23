---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Definir um certificado de comunicações de serviço
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d0464853c73f88ed76545921ffc8a4bf8551c800
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408326"
---
# <a name="set-a-service-communications-certificate"></a>Definir um certificado de comunicações de serviço


Os servidores de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS\) usar o certificado de comunicações de serviço para proteger o tráfego de serviços da Web para protocolo SSL \(comunicação SSL\) com clientes Web ou com proxies de servidor de Federação.

> [!NOTE]  
> O certificado de comunicações do serviço não é o mesmo que um certificado SSL. Para alterar o AD FS certificado SSL, você precisará usar o PowerShell. Siga as orientações neste [artigo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


Você pode usar o procedimento a seguir para alterar o certificado de comunicações de serviço com o snap\-de gerenciamento de AD FS no.  

> [!NOTE]  
> O\-snap de gerenciamento de AD FS no refere-se aos certificados de autenticação de servidor para servidores de Federação como certificados de comunicação de serviço.  

A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   

### <a name="to-set-a-service-communications-certificate"></a>Para definir um certificado de comunicações de serviço  

1.  Na tela **Iniciar** , digite**Gerenciamento de AD FS**e pressione Enter.  

2.  Na árvore de console, clique duas\-vezes em **serviço**e em **certificados**.  

3.  No painel **ações** , clique no link **definir certificado de comunicações de serviço** .  

4.  Na caixa de diálogo **selecionar um certificado de comunicações de serviço** , navegue até o arquivo de certificado que você deseja definir como certificado de comunicações de serviço, selecione o arquivo de certificado e clique em **abrir**.  

## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
