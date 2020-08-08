---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurar DNS corporativa para o Serviço de Federação e DRS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: de0313a65234c637018b0c7b2d7a7c9212464077
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938424"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurar DNS corporativa para o Serviço de Federação e DRS

## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Etapa 6: adicionar um \( registro de \) recurso do CNAME a e \( do alias \) ao DNS corporativo para o serviço de Federação e o DRS
Você deve adicionar os seguintes registros de recursos ao \( DNS do sistema de nome de domínio corporativo \) para o serviço de Federação e o serviço de registro de dispositivo que você configurou nas etapas anteriores.

|Entrada|Type|Endereço|
|---------|--------|-----------|
|\_nome do serviço de Federação \_|Hospedar \( um\)|Endereço IP do servidor de AD FS ou o endereço IP do balanceador de carga que está configurado na frente do seu farm de servidores AD FS|
|enterpriseregistration|CNAME do alias \(\)|\_Name.contoso.com do servidor de Federação \_|

Você pode usar o procedimento a seguir para adicionar um host \( a \) e \( registros de recurso CNAME de alias \) ao DNS corporativo para o servidor de Federação e o serviço de registro de dispositivo.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Para adicionar um host \( a \) e \( os registros de recurso CNAME de alias \) ao DNS para o servidor de Federação

1.  No controlador de domínio, em Gerenciador do Servidor, no menu **ferramentas** , clique em **DNS** para abrir o snap- \- in do DNS.

2.  Na árvore de console, expanda o nó ** \_ \_ nome do controlador de domínio** , expanda zonas de **pesquisa direta**, clique com o botão direito do mouse \- em ** \_ nome de domínio**e clique em **novo host \( A ou aaaa \) **.

3.  Na caixa **nome** , digite o nome a ser usado para o farm de AD FS.

4.  Na caixa **Endereço IP**, digite o endereço IP do seu servidor de federação. Clique em **Adicionar Host**.

5.  Clique com o botão direito \- do mouse no nó do ** \_ nome de domínio** e clique em **novo alias \( CNAME \) **.

6.  Na caixa de diálogo **Novo Registro de Recurso**, digite **enterpriseregistration** na caixa **Nome do alias**.

7.  No nome de domínio totalmente qualificado \( FQDN \) da caixa host de destino, digite **nome do farm de serviço de federação \_ \_ \_ . domínio \_ Name.com**e clique em **OK**.

    > [!IMPORTANT]
    > Em uma implantação do mundo real, se sua empresa tiver vários sufixos UPN de nome de entidade de usuário \( \) , você deverá criar vários registros CNAME para cada um desses sufixos UPN no DNS.

## <a name="see-also"></a>Consulte Também

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guia de Implantação do AD FS do Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)


