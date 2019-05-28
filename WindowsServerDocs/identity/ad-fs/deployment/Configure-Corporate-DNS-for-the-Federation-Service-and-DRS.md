---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar DNS corporativa para o Serviço de Federação e DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd8febf9eff300b1a83d22828874b4a577b8af36
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192321"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar DNS corporativa para o Serviço de Federação e DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Etapa 6: Adicionar um Host \(um\) e o Alias \(CNAME\) registro de recurso ao DNS corporativo para o serviço de Federação e o DRS  
Você deve adicionar os registros de recursos a seguir ao sistema de nomes de domínio corporativo \(DNS\) do seu serviço de Federação e o serviço de registro de dispositivo que você configurou nas etapas anteriores.  
  
|Entrada|Tipo|Endereço|  
|---------|--------|-----------|  
|federation\_service\_name|Host \(A\)|Endereço IP do servidor do AD FS ou o endereço IP do balanceador de carga que está configurado na frente do seu farm de servidores do AD FS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
Você pode usar o procedimento a seguir para adicionar um host \(um\) e o alias \(CNAME\) registros de recursos DNS corporativo para o servidor de Federação e o serviço de registro do dispositivo.  
  
Associação na **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para adicionar um host \(um\) e o alias \(CNAME\) registros de recursos para o DNS para o servidor de Federação  
  
1.  Em você controlador de domínio, no Gerenciador do servidor, sobre o **ferramentas** menu, clique em **DNS** para abrir o snap DNS\-no.  
  
2.  Na árvore de console, expanda o **domínio\_controlador\_nome** nó, expanda **zonas de pesquisa direta**, à direita\-clique **domínio\_nome**e, em seguida, clique em **novo Host \(A ou AAAA\)** .  
  
3.  No **nome** , digite o nome a ser usado para o farm do AD FS.  
  
4.  Na caixa **Endereço IP**, digite o endereço IP do seu servidor de federação. Clique em **Adicionar Host**.  
  
5.  À direita\-clique em de **domínio\_nome** nó e, em seguida, clique **novo Alias \(CNAME\)** .  
  
6.  Na caixa de diálogo **Novo Registro de Recurso**, digite **enterpriseregistration** na caixa **Nome do alias**.  
  
7.  O nome de domínio totalmente qualificado \(FQDN\) da caixa de host de destino, digite **federação\_service\_farm\_name.domain\_empresa.com**e, em seguida, Clique em **Okey**.  
  
    > [!IMPORTANT]  
    > Em uma implantação do mundo real, se sua empresa tiver vários UPN \(UPN\) sufixos, você deve criar vários registros CNAME para cada um desses sufixos UPN no DNS.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

