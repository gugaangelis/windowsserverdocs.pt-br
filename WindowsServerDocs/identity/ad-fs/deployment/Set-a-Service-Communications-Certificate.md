---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Definir um certificado de comunicações de serviço
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 140e8e4204148dd8862385054554d7b8336856ec
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192013"
---
# <a name="set-a-service-communications-certificate"></a>Definir um certificado de comunicações de serviço


Servidores de federação nos serviços de Federação do Active Directory \(do AD FS\) usar o certificado de comunicações de serviço para proteger o tráfego de serviços da Web para Secure Sockets Layer \(SSL\) comunicação com a Web os clientes ou com proxies do servidor de Federação.

> [!NOTE]  
> O certificado de comunicações de serviço não é o mesmo que um certificado SSL. Para alterar o certificado SSL do AD FS, você precisará usar o Powershell. Siga as orientações desta [artigo](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


Você pode usar o procedimento a seguir para alterar o certificado de comunicações de serviço com o snap de gerenciamento do AD FS\-no.  

> [!NOTE]  
> O snap de gerenciamento do AD FS\-refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação de serviço.  

A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Para definir um certificado de comunicações de serviço  

1.  Sobre o **inicie** tela, digite**gerenciamento do AD FS**, e pressione ENTER.  

2.  Na árvore de console, clique duas vezes\-clique em **Service**e, em seguida, clique em **certificados**.  

3.  No **ações** painel, clique no **definir certificado de comunicações de serviço** link.  

4.  No **selecione um certificado de comunicações de serviço** diálogo caixa, navegue até o arquivo de certificado que você deseja definir como o certificado de comunicações de serviço, selecione o arquivo de certificado e, em seguida, clique em **abrir**.  

## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
