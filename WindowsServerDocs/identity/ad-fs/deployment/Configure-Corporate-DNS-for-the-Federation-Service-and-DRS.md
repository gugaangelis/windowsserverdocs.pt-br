---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: "Configurar o DNS corporativa para o serviço de Federação e DRS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar o DNS corporativa para o serviço de Federação e DRS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Etapa 6: Adicionar um Host de registro de recurso \(CNAME\) \(A\) e Alias de DNS corporativo para o serviço de Federação e DRS  
Você deve adicionar os seguintes registros de recurso para \(DNS\) Domain Name System corporativa para seu serviço de Federação e serviço de registro de dispositivo que você configurou nas etapas anteriores.  
  
|Entrada|Tipo|Endereço|  
|---------|--------|-----------|  
|federation\_service\_name|Hospedar \(A\)|Endereço IP do servidor do AD FS ou o endereço IP do balanceador de carga é configurado na frente de seu farm de servidores do AD FS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
Você pode usar o procedimento a seguir para adicionar registros de recurso do \(CNAME\) um host \(A\) e alias DNS corporativo para o servidor de Federação e o serviço de registro do dispositivo.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para adicionar host \(A\) e alias \(CNAME\) registros de recurso no DNS para o servidor de Federação  
  
1.  Em você controlador de domínio, no Gerenciador do servidor, no **ferramentas** menu, clique em **DNS** para abrir o DNS snap\-in.  
  
2.  Na árvore de console, expanda o **domain\_controller\_name** nó, expanda **zonas de pesquisa direta**, clique right\ **domain\_name**e clique em **novo Host \(A or AAAA\)**.  
  
3.  No **nome**, digite o nome a ser usado para seu farm AD FS.  
  
4.  No **endereço IP**, digite o endereço IP do seu servidor de Federação. Clique em **Adicionar Host**.  
  
5.  Clique em Right\ o **domain\_name** nó e clique **novo Alias \(CNAME\)**.  
  
6.  No **novo registro de recurso** caixa de diálogo, digite **enterpriseregistration** no **apelido** caixa.  
  
7.  No domínio totalmente qualificado nomear \(FQDN\) da caixa de host de destino, o tipo **federation\_service\_farm\_name.domain\_name.com**e clique em **Okey**.  
  
    > [!IMPORTANT]  
    > Em uma implantação do mundo real, se sua empresa tiver vários sufixos UPN \(UPN\), você deve criar vários registros CNAME para cada uma dessas sufixos UPN no DNS.  
  
## <a name="see-also"></a>Consulte também 

[AD FS implantação](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implantando um Farm de servidores de Federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

