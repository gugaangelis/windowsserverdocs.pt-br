---
title: Kerberos com o nome da entidade de serviço (SPN)
description: O controlador de rede dá suporte a vários métodos de autenticação para comunicação com clientes de gerenciamento. Você pode usar a autenticação baseada em Kerberos, a autenticação baseada em certificado X509. Você também tem a opção de não usar autenticação para implantações de teste.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 78d5d2144e0def8e69a2a4ae5fdc2d7718936710
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355772"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos com o nome da entidade de serviço (SPN)

>Aplica-se a: Windows Server 2019

O controlador de rede dá suporte a vários métodos de autenticação para comunicação com clientes de gerenciamento. Você pode usar a autenticação baseada em Kerberos, a autenticação baseada em certificado X509. Você também tem a opção de não usar autenticação para implantações de teste.

O System Center Virtual Machine Manager usa a autenticação baseada em Kerberos. Se você estiver usando a autenticação baseada em Kerberos, deverá configurar um SPN (nome da entidade de serviço) para o controlador de rede no Active Directory. O SPN é um identificador exclusivo para a instância de serviço do controlador de rede, que é usado pela autenticação Kerberos para associar uma instância de serviço a uma conta de logon de serviço. Para obter mais detalhes, consulte [nomes da entidade de serviço](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurar SPN (nomes da entidade de serviço)

O controlador de rede configura automaticamente o SPN. Tudo o que você precisa fazer é fornecer permissões para que os computadores do controlador de rede registrem e modifiquem o SPN.

1.  No computador do controlador de domínio, inicie **Active Directory usuários e computadores**.

2.  Selecione **Exibir \> avançado**.

3.  Em **computadores**, localize uma das contas de computador do controlador de rede e clique com o botão direito do mouse e selecione **Propriedades**.

4.  Selecione a guia **segurança** e clique em **avançado**.

5.  Na lista, se todas as contas de computador do controlador de rede ou um grupo de segurança com todas as contas do computador do controlador de rede não estiverem listados, clique em **Adicionar** para adicioná-lo.

6.  Para cada conta de computador do controlador de rede ou um único grupo de segurança que contém as contas do computador do controlador de rede:

    a.  Selecione a conta ou o grupo e clique em **Editar**.

    b.  Em permissões, selecione **validar gravação servicePrincipalName**.

    d.  Role para baixo e, em **Propriedades** , selecione:

       -  **Ler servicePrincipalName**

       -  **Gravar servicePrincipalName**

    e.  Clique em **OK** duas vezes.

7.  Repita a etapa 3-6 para cada computador do controlador de rede.

8.  Feche **Usuários e Computadores do Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Falha ao fornecer permissões para registro/modificação de SPN

Em uma **nova** implantação do Windows Server 2019, se você escolher Kerberos para autenticação de cliente REST e não conceder permissão para os nós do controlador de rede registrarem ou modificarem o SPN, as operações REST no controlador de rede falharão impedindo que você gerencie o SDN.

Para uma atualização do Windows Server 2016 para o Windows Server 2019 e você escolheu Kerberos para autenticação de cliente REST, as operações REST não são bloqueadas, garantindo transparência para implantações de produção existentes. 

Se o SPN não estiver registrado, a autenticação de cliente REST usará NTLM, o que é menos seguro. Você também obtém um evento crítico no canal admin do canal de eventos **NetworkController-Framework** solicitando que você forneça permissões para os nós do controlador de rede para registrar o SPN. Depois de fornecer permissão, o controlador de rede registra o SPN automaticamente e todas as operações de cliente usam Kerberos.


>[!TIP]
>Normalmente, você pode configurar o controlador de rede para usar um endereço IP ou nome DNS para operações baseadas em REST. No entanto, ao configurar o Kerberos, você não pode usar um endereço IP para consultas REST ao controlador de rede. Por exemplo, você pode usar \< https://networkcontroller.consotso.com\>, mas não pode usar \< https://192.34.21.3\>. Os nomes da entidade de serviço não poderão funcionar se os endereços IP forem usados.
>
>Se você estivesse usando o endereço IP para operações REST junto com a autenticação Kerberos no Windows Server 2016, a comunicação real teria sido sobre a autenticação NTLM. Nessa implantação, depois de atualizar para o Windows Server 2019, você continuará a usar a autenticação baseada em NTLM. Para mudar para a autenticação baseada em Kerberos, você deve usar o nome DNS do controlador de rede para operações REST e fornecer permissão para que os nós do controlador de rede registrem o SPN.

---