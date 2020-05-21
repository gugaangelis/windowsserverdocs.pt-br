---
title: Requisitos para a implantação do controlador de rede
description: Prepare seu datacenter para a implantação do controlador de rede, que exige um ou mais computadores ou VMs e um computador ou VM. Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, os locais do arquivo de log (se necessário) e o registro de DNS dinâmico.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/10/2018
ms.openlocfilehash: f5a7ec331c9d70214cbd0a772de6e2b2c7f4f58e
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433170"
---
# <a name="requirements-for-deploying-network-controller"></a>Requisitos para a implantação do controlador de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Prepare seu datacenter para a implantação do controlador de rede, que exige um ou mais computadores ou VMs e um computador ou VM. Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, os locais do arquivo de log (se necessário) e o registro de DNS dinâmico.


## <a name="network-controller-requirements"></a>Requisitos do controlador de rede

A implantação do controlador de rede requer um ou mais computadores ou VMs que servem como o controlador de rede e um computador ou VM para servir como um cliente de gerenciamento para o controlador de rede. 

- Todas as VMs e computadores planejados como nós do controlador de rede devem estar executando o Windows Server 2016 Datacenter Edition. 
- Qualquer computador ou VM (máquina virtual) no qual você instala o controlador de rede deve estar executando a Datacenter Edition do Windows Server 2016. 
- O computador cliente de gerenciamento ou a VM para o controlador de rede deve estar executando o Windows 10. 


## <a name="configuration-requirements"></a>Requisitos de configuração

Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, os locais do arquivo de log (se necessário) e o registro de DNS dinâmico.

### <a name="step-1-configure-your-security-groups"></a>Etapa 1. Configurar seus grupos de segurança

A primeira coisa que você deseja fazer é criar dois grupos de segurança para a autenticação Kerberos. 

Você cria grupos para usuários que têm permissão para: 

1. Configurar o controlador de rede<p>Você pode nomear esse grupo Administradores de controlador de rede, por exemplo. 
2.  Configurar e gerenciar a rede usando o controlador de rede<p>Você pode nomear esse grupo de usuários do controlador de rede, por exemplo. Use REST (Representational State Transfer) para configurar e gerenciar o controlador de rede.

>[!NOTE]
>Todos os usuários que você adicionar devem ser membros do grupo usuários do domínio em Active Directory usuários e computadores.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Etapa 2. Configurar locais do arquivo de log, se necessário

A próxima coisa que você quer fazer é configurar os locais de arquivo para armazenar logs de depuração do controlador de rede no computador do controlador de rede ou VM ou em um compartilhamento de arquivo remoto. 

>[!NOTE]
>Se você armazenar os logs em um compartilhamento de arquivos remoto, verifique se o compartilhamento está acessível no controlador de rede.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Etapa 3. Configurar o registro de DNS dinâmico para o controlador de rede

Por fim, a próxima coisa que você deseja fazer é implantar nós de cluster do controlador de rede na mesma sub-rede ou em sub-redes diferentes. 


|         Se...         |                                                                                                                                                         Então...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Na mesma sub-rede,  |                                                                                                                                Você deve fornecer o endereço IP REST do controlador de rede.                                                                                                                                 |
| Em sub-redes diferentes, | Você deve fornecer o nome DNS REST do controlador de rede, que você cria durante o processo de implantação. Você também deve fazer o seguinte:<ul><li>Configure atualizações dinâmicas de DNS para o nome DNS do controlador de rede no servidor DNS.</li><li>Restringir as atualizações dinâmicas de DNS somente a nós do controlador de rede.</li></ul> |

---

> [!NOTE]
> A associação em **Admins**. do domínio, ou equivalente, é o requisito mínimo necessário para executar esses procedimentos.

1. Permitir atualizações dinâmicas de DNS para uma zona.

   a. Abra o Gerenciador de DNS e, na árvore de console, clique com o botão direito do mouse na zona aplicável e clique em **Propriedades**. 

   b. Na guia **geral** , verifique se o tipo de zona é **primário** ou **integrado ao Active Directory**.

   c. Em **atualizações dinâmicas**, verifique se a seleção **somente segurança** está selecionada e clique em **OK**.

2. Configurar permissões de segurança de zona DNS para nós do controlador de rede

   a.  Clique na guia **Segurança** e clique em **Avançado**. 

   b. Em **configurações de segurança avançadas**, clique em **Adicionar**. 

   c. Clique em **Selecionar uma entidade**. 

   d. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , clique em **tipos de objeto**. 

   e. Em **tipos de objeto**, selecione **computadores**e clique em **OK**.

   f. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , digite o nome NetBIOS de um dos nós do controlador de rede em sua implantação e clique em **OK**.

   g. Em **entrada de permissão**, verifique os seguintes valores:

      - **Tipo** = permitir
      - **Aplica-se a** = este objeto e todos os objetos descendentes

   h. Em **permissões**, selecione **gravar todas as propriedades** e **excluir**e, em seguida, clique em **OK**.

3. Repita para todos os computadores e VMs no cluster do controlador de rede.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Etapa 4. Configurar o nome da entidade de serviço se estiver usando a autenticação baseada em Kerberos

Se o controlador de rede estiver usando a autenticação baseada em Kerberos para comunicação com clientes de gerenciamento, você deverá configurar um SPN (nome da entidade de serviço) para o controlador de rede no Active Directory. O controlador de rede configura automaticamente o SPN. Tudo o que você precisa fazer é fornecer permissões para que os computadores do controlador de rede registrem e modifiquem o SPN. Para obter mais detalhes, consulte [configurar nomes de entidade de serviço (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Opções de implantação

### <a name="network-controller-deployment"></a>Implantação do controlador de rede

A instalação está altamente disponível com três nós de controlador de rede configurados em máquinas virtuais. Também são mostrados dois locatários com a rede virtual do locatário 2 dividida em duas sub-redes virtuais para simular uma camada da Web e uma camada de banco de dados.  

![Planejamento do SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Implantação do controlador de rede e do balanceador de carga de software

Para alta disponibilidade, há dois ou mais nós SLB/MUX.

![Planejamento do SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Implantação do controlador de rede, Load Balancer de software e gateway de RAS

Há três máquinas virtuais de gateway; dois estão ativos e um é redundante.

![Planejamento do SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  

>[!IMPORTANT] 
>Se você implantar usando o VMM, verifique se suas máquinas virtuais de infraestrutura (servidor VMM, AD/DNS, SQL Server etc.) não estão hospedadas em nenhum dos quatro hosts mostrados nos diagramas.  


## <a name="next-steps"></a>Próximas etapas
[Planejar uma infraestrutura de rede definida pelo software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Tópicos relacionados
- [Controlador de rede](../technologies/network-controller/Network-Controller.md) 
- [Alta disponibilidade do controlador de rede](../technologies/network-controller/network-controller-high-availability.md) 
- [Implantar controlador de rede usando o Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Instalar a função de servidor do Controlador de Rede usando o Gerenciador do Servidor](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
