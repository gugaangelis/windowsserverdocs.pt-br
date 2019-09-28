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
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Etapa 6: Adicione um registro de recurso do host \(A @ no__t-1 e alias \(CNAME @ no__t-3 ao DNS corporativo para o Serviço de Federação e o DRS  
Você deve adicionar os seguintes registros de recursos ao sistema de nome de domínio corporativo \(DNS @ no__t-1 para o serviço de Federação e o serviço de registro de dispositivo que você configurou nas etapas anteriores.  
  
|Entrada|type|Endereço|  
|---------|--------|-----------|  
|Federação @ no__t-0service @ no__t-1Name|Host \(A @ no__t-1|Endereço IP do servidor de AD FS ou o endereço IP do balanceador de carga que está configurado na frente do seu farm de servidores AD FS|  
|enterpriseregistration|Alias \(CNAME @ no__t-1|Federação @ no__t-0server\_name.contoso.com|  
  
Você pode usar o procedimento a seguir para adicionar um host \(A @ no__t-1 e alias \(CNAME @ no__t-3 registros de recursos ao DNS corporativo para o servidor de Federação e o serviço de registro de dispositivo.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para adicionar um host \(A @ no__t-1 e alias \(CNAME @ no__t-3 registros de recursos ao DNS para o servidor de Federação  
  
1.  No controlador de domínio, em Gerenciador do Servidor, no menu **ferramentas** , clique em **DNS** para abrir o snap-2in do DNS.  
  
2.  Na árvore de console, expanda o nó **domínio @ no__t-1controller @ no__t-2name** , expanda **zonas de pesquisa direta**, direito @ no__t-4click **domínio @ no__t-6Name**e clique em **novo host \(a ou aaaa @ no__t-9**.  
  
3.  Na caixa **nome** , digite o nome a ser usado para o farm de AD FS.  
  
4.  Na caixa **Endereço IP**, digite o endereço IP do seu servidor de federação. Clique em **Adicionar Host**.  
  
5.  Right @ no__t-0click o nó **domínio @ no__t-2name** e clique em **novo alias \(CNAME @ no__t-5**.  
  
6.  Na caixa de diálogo **Novo Registro de Recurso**, digite **enterpriseregistration** na caixa **Nome do alias**.  
  
7.  No nome de domínio totalmente qualificado \(FQDN @ no__t-1 da caixa host de destino, digite **Federation @ no__t-3service @ no__t-4farm @ no__t-5name. Domain @ no__t-6name. com**e clique em **OK**.  
  
    > [!IMPORTANT]  
    > Em uma implantação do mundo real, se sua empresa tiver vários sufixos de nome principal de usuário \(UPN @ no__t-1, você deverá criar vários registros CNAME para cada um desses sufixos UPN no DNS.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

