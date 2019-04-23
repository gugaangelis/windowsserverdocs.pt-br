---
title: Kerberos com o nome da entidade de serviço (SPN)
description: Controlador de rede dá suporte a vários métodos de autenticação para a comunicação com clientes de gerenciamento. Você pode usar a autenticação baseada em Kerberos, X509 autenticação baseada em certificado. Você também tem a opção para não usar autenticação para implantações de teste.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2459cfa8dfec3de4aa23da7aba192d6eeed7f8ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828967"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos com o nome da entidade de serviço (SPN)

>Aplica-se a: Windows Server 2019

Controlador de rede dá suporte a vários métodos de autenticação para a comunicação com clientes de gerenciamento. Você pode usar a autenticação baseada em Kerberos, X509 autenticação baseada em certificado. Você também tem a opção para não usar autenticação para implantações de teste.

System Center Virtual Machine Manager usa a autenticação baseada em Kerberos. Se você estiver usando autenticação baseada em Kerberos, você deve configurar um nome Principal de serviço (SPN) para o controlador de rede no Active Directory. O SPN é um identificador exclusivo para a instância de serviço do controlador de rede, que é usado pela autenticação do Kerberos para associar uma instância de serviço com uma conta de logon do serviço. Para obter mais detalhes, consulte [nomes de entidade de serviço](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurar nomes de entidade de serviço (SPN)

O controlador de rede configura automaticamente o SPN. Tudo o que você precisa fazer é fornecer permissões para as máquinas de controlador de rede para se registrar e modificar o SPN.

1.  No computador do controlador de domínio, inicie **Active Directory Users and Computers**.

2.  Selecione **modo de exibição \> Advanced**.

3.  Sob **computadores**, localize uma das contas de computador do controlador de rede e, em seguida, clique com botão direito e selecione **propriedades**.

4.  Selecione o **segurança** guia e clique em **avançado**.

5.  Na lista, se todas as contas de computador do controlador de rede ou de uma segurança de grupo tendo a todas as contas de computador do controlador de rede não estiver listado, clique em **adicionar** para adicioná-lo.

6.  Para cada conta de computador do controlador de rede ou um único grupo de segurança que contêm as contas de computador do controlador de rede:

    a.  Selecione a conta ou grupo e clique em **editar**.

    b.  Em Selecionar permissões **validar gravar servicePrincipalName**.

    d.  Role para baixo e, sob **propriedades** selecione:

       -  **Ler servicePrincipalName**

       -  **Gravar servicePrincipalName**

    e.  Clique em **OK** duas vezes.

7.  Repita a etapa 3 a 6 para cada máquinas de controlador de rede.

8.  Feche **Usuários e Computadores do Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Falha ao fornecer permissões para registro SPN/modificação

Em um **NEW** operações REST em controlador de rede de implantação do Windows Server 2019, se você escolheu o Kerberos para autenticação de cliente REST e não concede permissão para nós de controlador de rede registrar ou modificar o SPN, falhar impedindo o gerenciamento de SDN.

Para uma atualização do Windows Server 2016 para Windows Server 2019 e você tiver escolhido a Kerberos para autenticação de cliente REST, operações REST seja bloqueadas, garantindo a transparência para implantações de produção existentes. 

Se o SPN não estiver registrado, autenticação de cliente REST usa NTLM, que é menos seguro. Você também obtém um evento crítico no canal de administração do **NetworkController Framework** solicitando que você forneça permissões para os nós de controlador de rede para registrar o SPN do canal de evento. Depois que você fornecer permissão, controlador de rede registra o SPN automaticamente e todas as operações de cliente usam o Kerberos.


>[!TIP]
>Normalmente, você pode configurar o controlador de rede para usar um endereço IP ou nome DNS para operações baseadas em REST. No entanto, quando você configura o Kerberos, é possível usar um endereço IP para consultas REST para o controlador de rede. Por exemplo, você pode usar \< https://networkcontroller.consotso.com\>, mas não é possível usar \< https://192.34.21.3\>. Nomes de entidade de serviço não funcionará se os endereços IP são usados.
>
>Se você estivesse usando o endereço IP para operações de REST, junto com a autenticação Kerberos no Windows Server 2016, a comunicação real teria sido sobre a autenticação NTLM. Na implantação, depois de atualizar para o Windows Server de 2019, continue a usar a autenticação baseada em NTLM. Para mover para a autenticação baseada em Kerberos, você deve usar o nome DNS do controlador de rede para operações REST e fornecer permissão para nós de controlador de rede registrar o SPN.

---