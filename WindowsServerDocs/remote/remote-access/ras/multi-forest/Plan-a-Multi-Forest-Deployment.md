---
title: Planejar uma implantação de várias florestas
description: Este tópico faz parte do guia implantar o acesso remoto em um ambiente de várias florestas no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8e483f5986a5a23123495e3a13440ddc57a6c521
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314046"
---
# <a name="plan-a-multi-forest-deployment"></a>Planejar uma implantação de várias florestas

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve as etapas de planejamento necessárias à configuração do Acesso Remoto em uma implantação de várias florestas.  
  
## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  
  
-   É necessária uma relação de confiança bidirecional.  
  
## <a name="plan-trust-between-forests"></a>Planejar a confiança entre florestas  
Quando você decide habilitar o acesso a recursos de uma nova floresta, permitir que os clientes da nova floresta usem o DirectAccess ou adicionar servidores de acesso remoto da nova floresta à implantação do acesso remoto como pontos de entrada, você precisa assegurar que uma confiança total, isto é, uma relação de confiança transitiva bidirecional, seja configurada entre as duas florestas. Consulte o tópico sobre [Tipos de relação de confiança](https://technet.microsoft.com/library/cc775736.aspx). A confiança total entre florestas é um pré-requisito para que uma implantação de várias florestas permita aos administradores realizar operações como: edição de GPOs na nova floresta; uso de grupos de segurança da nova floresta como o grupo de segurança de cliente; execução de chamadas remotas (WinRM, RPC) para computadores na nova floresta; e autenticação de clientes remotos da nova floresta.  
  
## <a name="plan-remote-access-administrator-permissions"></a>Planejar permissões de administrador do Acesso Remoto  
Quando você configura o Acesso Remoto, ele atualiza e às vezes cria GPOs em cada um dos domínios que contêm clientes ou servidores de acesso remoto. Em um ambiente de várias florestas, assim como nos ambientes de uma única floresta, o administrador do Acesso Remoto precisa ter permissões para criar e modificar GPOs do DirectAccess e os respectivos filtros de segurança e, opcionalmente, ter permissões para criar links para os GPOs do DirectAccess em todas as florestas envolvidas. Essas permissões são necessárias seja qual for a floresta a que o administrador do Acesso Remoto pertence.  
  
Além disso, o administrador do Acesso Remoto precisa ser um administrador local em todos os servidores de acesso remoto, inclusive aqueles da nova floresta que são adicionados como pontos de entrada à implantação do Acesso Remoto original.  
  
## <a name="plan-client-security-groups"></a><a name="ClientSG"></a>Planejar grupos de segurança do cliente  
Você precisa configurar pelo menos um grupo de segurança na nova floresta para computadores cliente do DirectAccess na nova floresta. A razão é que um único grupo de segurança não pode incluir contas de várias florestas.  
  
> [!NOTE]  
> -   O DirectAccess requer pelo menos um grupo de segurança de cliente do Windows 10&reg; ou do Windows&reg; 8 para cada floresta. No entanto, é recomendável ter um grupo de segurança de cliente do Windows 10 ou do Windows 8 para cada domínio que contenha clientes Windows 10 ou Windows 8.  
> -   Quando o multissite está habilitado, o DirectAccess requer pelo menos um grupo de segurança de cliente do Windows 7&reg; por floresta para cada ponto de entrada do DirectAccess no qual os computadores cliente do Windows 7 têm suporte. No entanto, é recomendável ter um grupo de segurança de cliente do Windows 7 separado para cada ponto de entrada para cada domínio que contenha clientes do Windows 7.  
>   
> Para que o DirectAccess seja aplicado a computadores cliente em domínios adicionais, é preciso criar GPOs de cliente nesses domínios. A adição de grupos de segurança dispara a geração de novos GPOs de cliente para os novos domínios. Portanto, se você adicionar um novo grupo de segurança de um novo domínio à lista de grupos de segurança de cliente do DirectAccess, um GPO de cliente será criado automaticamente no novo domínio e os computadores cliente do novo domínio receberão as configurações do DirectAccess através do GPO de cliente.  
>   
> Observe que, se você adicionar um cliente de um novo domínio a um grupo de segurança existente que já está configurado como um grupo de segurança de cliente do DirectAccess, o GPO de cliente não será criado automaticamente pelo DirectAccess no novo domínio. O cliente no novo domínio não receberá as configurações do DirectAccess e não poderá se conectar usando o DirectAccess.  
  
## <a name="plan-certification-authorities"></a>Planejar autoridades de certificação  
Se a implantação do DirectAccess estiver configurada para usar a autenticação OTP (senha de uso único), cada floresta conterá os mesmos modelos de certificado de autenticação, mas com valores de Oid diferentes. Como resultado, as florestas não podem ser configuradas como uma unidade de configuração única. Para resolver esse problema e configurar a OTP em um ambiente de várias florestas, consulte a seção "configurar a OTP em uma implantação de várias florestas" no tópico [Configurar uma implantação de várias florestas](Configure-a-Multi-Forest-Deployment.md).  
  
Quando é usada a autenticação de certificado de computador IPsec, todos os computadores cliente e servidor precisam ter um certificado de computador emitido pela mesma autoridade de certificação raiz ou intermediária, seja qual for a floresta a que pertencem.  
  
## <a name="plan-otp-exemptions"></a>Planejar isenções de OTP  
Se você estiver usando a autenticação OTP do DirectAccess, observe que o grupo de segurança de isenções de OTP está limitado aos usuários de uma mesma floresta. Isso acontece porque cada grupo de segurança só pode conter usuários de uma mesma floresta, e só é possível configurar um único grupo de segurança desse tipo.  
  


