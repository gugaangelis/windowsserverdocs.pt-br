---
title: Requisitos para implantar o controlador de rede
description: Prepare seu datacenter para implantação do controlador de rede, o que exige um ou mais computadores ou máquinas virtuais e um computador ou VM. Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, locais de arquivo de log (se necessário) e o registro DNS dinâmico.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 9db7609f6f1273c46cba1dd29f81c297bb26f94b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829857"
---
# <a name="requirements-for-deploying-network-controller"></a>Requisitos para implantar o controlador de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Prepare seu datacenter para implantação do controlador de rede, o que exige um ou mais computadores ou máquinas virtuais e um computador ou VM. Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, locais de arquivo de log (se necessário) e o registro DNS dinâmico.


## <a name="network-controller-requirements"></a>Requisitos do controlador de rede

Implantação do controlador de rede requer um ou mais computadores ou máquinas virtuais que servem como o controlador de rede e um computador ou VM para servir como um cliente de gerenciamento para o controlador de rede. 

- Todas as VMs e computadores planejadas como nós de controlador de rede devem estar executando o Windows Server 2016 Datacenter edition. 
- Qualquer computador ou máquina virtual (VM) no qual você instala o controlador de rede deve estar executando a Datacenter edition do Windows Server 2016. 
- O computador cliente de gerenciamento ou a VM para o controlador de rede deve estar executando o Windows 10. 

  
## <a name="configuration-requirements"></a>Requisitos de configuração

Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, locais de arquivo de log (se necessário) e o registro DNS dinâmico.
  
### <a name="step-1-configure-your-security-groups"></a>Etapa 1. Configurar seus grupos de segurança
  
A primeira coisa que você deseja fazer é criar dois grupos de segurança para a autenticação Kerberos. 

Você cria grupos para os usuários que têm permissão para: 

1. Configurar o controlador de rede<p>Você pode nomear este grupo de administradores do controlador de rede, por exemplo. 
2.  Configurar e gerenciar a rede usando o controlador de rede<p>Você pode nomear essa usuários de controlador de rede de grupo, por exemplo. Use REST Representational State Transfer () para configurar e gerenciar o controlador de rede.

>[!NOTE]
>Todos os usuários que você adicione devem ser membros do grupo de usuários de domínio no Active Directory Users and Computers.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Etapa 2. Configurar locais de arquivo de log, se necessário

A próxima coisa que você deseja fazer é configurar os locais de arquivo para armazenar os logs de depuração do controlador de rede no computador do controlador de rede ou na VM ou em um compartilhamento de arquivo remoto. 

>[!NOTE]
>Se você armazenar os logs em um compartilhamento de arquivo remoto, certifique-se de que o compartilhamento está acessível do controlador de rede.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Etapa 3. Configurar o registro DNS dinâmico para o controlador de rede
  
Por fim, a próxima coisa que você deseja fazer é implantar nós de cluster de controlador de rede na mesma sub-rede ou sub-redes diferentes. 

|Se...  |Então...  |
|---------|---------|
|Na mesma sub-rede, |Você deve fornecer o endereço IP de REST do controlador de rede. |
|Em sub-redes diferentes, |Você deve fornecer o nome DNS de REST do controlador de rede que você criar durante o processo de implantação. Você também deve fazer o seguinte:<ul><li>Configure atualizações dinâmicas de DNS para o nome DNS do controlador de rede no servidor DNS.</li><li>Restringir as atualizações dinâmicas de DNS a somente nós de controlador de rede.</li></ul> |
---

> [!NOTE]
> Associação na **Admins. do domínio**, ou equivalente, é o mínimo necessário para executar esses procedimentos.
  
1. Permitir atualizações dinâmicas de DNS para uma zona.

   a. Abra o Gerenciador de DNS, na árvore de console, clique com botão direito na zona aplicável e, em seguida, clique em **propriedades**. 
      
   b. Sobre o **gerais** , verifique se que o tipo de zona é **primário** ou **integradas ao Active Directory**.

   c. Na **atualizações dinâmicas**, verifique **somente Secure** está selecionado e, em seguida, clique em **Okey**.

2. Configurar permissões de segurança de zona DNS para nós de controlador de rede

   a.  Clique na guia **Segurança** e clique em **Avançada**. 

   b. Na **configurações de segurança avançadas**, clique em **Add**. 
  
   c. Clique em **Selecionar um principal**. 

   d. No **Selecionar usuário, computador, conta de serviço ou grupo** caixa de diálogo, clique em **tipos de objeto**. 

   e. Na **tipos de objetos**, selecione **computadores**e, em seguida, clique em **Okey**.

   f. No **Selecionar usuário, computador, conta de serviço ou grupo** caixa de diálogo, digite o nome NetBIOS de um de nós de controlador de rede em sua implantação e, em seguida, clique em **Okey**.

   g. Na **entrada de permissão**, verifique se os seguintes valores:

      - **Tipo** = permitir
      - **Aplica-se a** = este objeto e todos os objetos descendentes
  
   h. Na **permissões**, selecione **gravar todas as propriedades** e **excluir**e, em seguida, clique em **Okey**.

3. Repita para todos os computadores e VMs do cluster de controlador de rede.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Etapa 4. Configurar o nome da entidade de serviço se usando o Kerberos, autenticação baseada em

Se o controlador de rede está usando a autenticação Kerberos para se comunicar com clientes de gerenciamento, você deve configurar um nome Principal de serviço (SPN) para o controlador de rede no Active Directory. O controlador de rede configura automaticamente o SPN. Tudo o que você precisa fazer é fornecer permissões para as máquinas de controlador de rede para se registrar e modificar o SPN. Para obter mais detalhes, consulte [configurar o serviço de nomes de SPN (entidade)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Opções de implantação

### <a name="network-controller-deployment"></a>Implantação do controlador de rede

A instalação está altamente disponível com três nós de controlador de rede configurados em máquinas virtuais. Também mostrado é dois locatários com a rede virtual do locatário 2, dividida em duas sub-redes virtuais para simular uma camada da web e uma camada de banco de dados.  

![Planejamento de NC de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Implantação de Balanceador de carga de software e o controlador de rede

Para obter alta disponibilidade, há dois ou mais nós SLB/MUX.
   
![Planejamento de NC de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Implantação do controlador de rede, o balanceador de carga de Software e o Gateway de RAS

Há três máquinas virtuais de gateway; dois estão ativos e um é redundante.

![Planejamento de NC de SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Para a automação de implantação com base em TP5, Active Directory deve estar disponível e acessível a partir dessas sub-redes. Para obter mais informações sobre o Active Directory, consulte [visão geral dos serviços do domínio do Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).  
  
>[!IMPORTANT] 
>Se você implantar usando o VMM, verifique se suas máquinas virtuais de infraestrutura (servidor do VMM, AD/DNS, SQL Server, etc.) não são hospedados em qualquer um dos quatro hosts mostrados nos diagramas.  


## <a name="next-steps"></a>Próximas etapas
[Plano de infraestrutura de rede definida por um Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Tópicos relacionados
- [Controlador de rede](../technologies/network-controller/Network-Controller.md) 
- [Alta disponibilidade do controlador de rede](../technologies/network-controller/network-controller-high-availability.md) 
- [Implantar o controlador de rede usando o Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Instalar a função de servidor de controlador de rede usando o Gerenciador do servidor](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
