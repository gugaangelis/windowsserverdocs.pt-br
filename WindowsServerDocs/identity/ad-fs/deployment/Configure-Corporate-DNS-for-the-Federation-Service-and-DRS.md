---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar DNS corporativa para o Serviço de Federação e DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f0b04f9dc050117fdefc630759c86d2b1bb1ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408449"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar DNS corporativa para o Serviço de Federação e DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Etapa 6: adicionar um host \(um\) e alias \(registro de recurso do CNAME\) ao DNS corporativo para o Serviço de Federação e o DRS  
Você deve adicionar os seguintes registros de recursos ao sistema de nome de domínio corporativo \(\) DNS para o serviço de Federação e o serviço de registro de dispositivo que você configurou nas etapas anteriores.  
  
|Entrada|Tipo|Endereço|  
|---------|--------|-----------|  
|nome do\_do serviço de\_de Federação|Host \(um\)|Endereço IP do servidor de AD FS ou o endereço IP do balanceador de carga que está configurado na frente do seu farm de servidores AD FS|  
|enterpriseregistration|Alias \(CNAME\)|servidor de\_de Federação\_name.contoso.com|  
  
Você pode usar o procedimento a seguir para adicionar um host \(um\) e alias \(registros de recursos de\) CNAME ao DNS corporativo para o servidor de Federação e o serviço de registro de dispositivo.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para adicionar um host \(um\) e alias \(CNAME\) registros de recursos no DNS para o servidor de Federação  
  
1.  No controlador de domínio, em Gerenciador do Servidor, no menu **ferramentas** , clique em **DNS** para abrir o snap-in de DNS\-em.  
  
2.  Na árvore de console, expanda o **controlador de\_do domínio\_** nó de nome, expanda **zonas de pesquisa direta**,\-clique em **domínio\_nome**e clique em **novo host \(A ou aaaa\)** .  
  
3.  Na caixa **nome** , digite o nome a ser usado para o farm de AD FS.  
  
4.  Na caixa **Endereço IP**, digite o endereço IP do seu servidor de federação. Clique em **Adicionar Host**.  
  
5.  À direita\-clique no nó **nome do domínio\_** e, em seguida, clique em **Novo Alias \(CNAME\)** .  
  
6.  Na caixa de diálogo **Novo Registro de Recurso**, digite **enterpriseregistration** na caixa **Nome do alias**.  
  
7.  No nome de domínio totalmente qualificado \(\) FQDN da caixa host de destino, digite **federação\_serviço\_farm\_nome. domínio\_Name.com**e clique em **OK**.  
  
    > [!IMPORTANT]  
    > Em uma implantação do mundo real, se sua empresa tiver vários nomes de entidade de usuário \(sufixo de\) UPN, você deverá criar vários registros CNAME para cada um desses sufixos UPN no DNS.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

