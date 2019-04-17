---
title: Instalação e requisitos de preparação para a implantação de controlador de rede
description: Você pode usar este tópico para preparar seu datacenter para implantação de controlador de rede.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c50a2167a894871ea76fb96523c19531100648ce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="installation-and-preparation-requirements-for-deploying-network-controller"></a>Instalação e requisitos de preparação para a implantação de controlador de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para preparar seu datacenter para implantação de controlador de rede.  
  
> [!NOTE]  
> Além de neste tópico, a seguinte documentação de controlador de rede está disponível.  
> 
> - [Controlador de rede](../technologies/network-controller/Network-Controller.md)
> - [Alta disponibilidade do controlador de rede](../technologies/network-controller/network-controller-high-availability.md)
> - [Implantar o controlador de rede usando o Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar a função de servidor de controlador de rede usando o Gerenciador do servidor](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)  

A seguir estão as etapas de instalação, software e outros requisitos e preparação, que você deve tomar antes de implantar o controlador de rede.

## <a name="installation-requirements"></a>Requisitos de instalação

A seguir é os requisitos de instalação para o controlador de rede.

- Para implantações do Windows Server 2016, você pode implantar o controlador de rede em um ou mais computadores, um ou mais VMs ou uma combinação de computadores e VMs. Todas as VMs e computadores planejados como nós de controlador de rede devem estar executando o Windows Server 2016 Datacenter edition.

## <a name="software-requirements"></a>Requisitos de software

Implantação de controlador de rede requer um ou mais computadores ou VMs que atuará como o controlador de rede e um computador ou VM para servir como um cliente de gerenciamento de controlador de rede. Esses computadores ou VMs execute os seguintes sistemas operacionais.  

- Qualquer computador ou uma máquina virtual (VM) no qual você instala o controlador de rede deve estar executando a edição de Datacenter do Windows Server 2016.  
  
- O computador do cliente de gerenciamento ou VM para o controlador de rede deve estar executando o Windows 8, Windows 8.1 ou Windows 10.  
  
## <a name="additional-requirements"></a>Requisitos adicionais

A seguir é etapas adicionais que você deve seguir antes de implantar o controlador de rede.
  
### <a name="configure-security-groups"></a>Definir grupos de segurança
  
Se os computadores ou VMs para o controlador de rede e o cliente de gerenciamento ingressados em domínio, configure os seguintes grupos de segurança para autenticação Kerberos.

- Crie um grupo de segurança e adicionar todos os usuários que têm permissão para configurar o controlador de rede. Por exemplo, crie um grupo chamado **administradores de controlador de rede**. Todos os usuários que você adicionar a esse grupo também devem ser membros do **usuários do domínio** grupo no Active Directory Users and Computers.  
  
    > [!NOTE]  
    > Para obter mais informações sobre como criar um grupo no Active Directory Users and Computers, consulte [criar um novo grupo](https://technet.microsoft.com/en-us/library/cc783256(v=ws.10).aspx).  

- Crie um grupo de segurança e adicionar todos os usuários que têm permissão para configurar e gerenciar a rede usando o controlador de rede.  Por exemplo, criar um novo grupo chamado **os usuários da rede controlador**. Todos os usuários que você adicionar ao novo grupo também devem ser membros do **usuários do domínio** grupo no Active Directory Users and Computers. Toda a configuração de controlador de rede e gerenciamento é realizada usando a transferência de estado representacional \(REST\).

### <a name="configure-log-file-locations-if-needed"></a>Configurar locais de arquivos de log, se necessário

Você pode armazenar os logs de depuração do controlador de rede no computador do controlador de rede ou VM ou em um compartilhamento de arquivos remoto. Se você quiser armazenar os logs em um compartilhamento de arquivo remoto, certifique-se de que o compartilhamento está acessível a partir do controlador de rede.

### <a name="configure-dynamic-dns-registration-for-network-controller"></a>Configurar registro DNS dinâmico para o controlador de rede
  
Você pode implantar nós de cluster de controlador de rede na mesma sub-rede ou em várias sub-redes. 

>[!NOTE]
>Se os nós de controlador de rede estão na mesma sub-rede, você deve fornecer o endereço IP de REST de controlador de rede quando você configura registro DNS dinâmico para o controlador de rede. Se os nós forem em várias sub-redes, você deve fornecer o nome DNS de REST de controlador de rede quando você configurar registro DNS dinâmico.

Se nós de controlador de rede em várias sub-redes, você deve executar a seguinte configuração DNS adicional:

- Criar um nome DNS para o controlador de rede durante o processo de implantação

- Configurar atualizações dinâmicas para o nome DNS de controlador de rede no servidor DNS

- Restringir as atualizações dinâmicas do DNS para somente nós de controlador de rede

Você pode usar os procedimentos a seguir para configurar atualizações dinâmicas e restringir a atualização dinâmica de registro de nome do controlador de rede.

> [!NOTE]
> A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para executar esses procedimentos.
  
#### <a name="to-allow-dns-dynamic-updates-for-a-zone"></a>Para permitir atualizações dinâmicas do DNS para uma zona

1. Abra o Gerenciador DNS.

2. Na árvore de console, clique com botão direito na zona aplicável e, em seguida, clique em **propriedades**. A zona **propriedades** caixa de diálogo é aberta.

3. No **geral** , verifique se que o tipo de zona está **principal** ou **integrada do Active Directory**.

4. Em **atualizações dinâmicas**, verifique **segura somente** é selecionado. Se não estiver selecionada, altere o valor da **atualizações dinâmicas** para **segura somente**e clique em **Okey**.

#### <a name="to-configure-dns-zone-security-permissions-for-network-controller-nodes"></a>Configurar permissões de segurança de zona DNS para nós de controlador de rede

1.  Abra o Gerenciador DNS.

2.  Na árvore de console, clique com botão direito na zona aplicável e, em seguida, clique em **propriedades**. A zona **propriedades** caixa de diálogo é aberta.

3.  Clique no **segurança** guia e, em seguida, clique em **avançado**. O **configurações de segurança avançadas** caixa de diálogo é aberta.

4. Em **configurações de segurança avançadas**, clique em **adicionar**. O **entrada de permissão** caixa de diálogo é aberta.
  
5. Clique em **selecionar uma entidade de segurança**. O **Selecionar usuário, computador, conta de serviço ou grupo** caixa de diálogo é aberta.

6. No **Selecionar usuário, computador, conta de serviço ou grupo** caixa de diálogo, clique em **tipos de objeto**. O **tipos de objeto** caixa de diálogo é aberta. 

7. Em **tipos de objeto**, selecione **computadores**e clique em **Okey**.

8. No **Selecionar usuário, computador, conta de serviço ou grupo** caixa de diálogo, digite o nome NetBIOS de um de nós de controlador de rede na sua implantação e clique em **Okey**.

9. Em **entrada de permissão**, certifique-se de que o valor de **tipo** é **permitir**e o valor de **se aplica a** é **esse objeto e todos os objetos descendentes**.
  
10. Em permissões, selecione **gravar todas as propriedades** e **excluir**e clique em **Okey**.

11. Repita as etapas **5** por meio de **10** para todos os computadores e VMs do cluster de controlador de rede.

Para obter mais informações, consulte [planejar uma infraestrutura de rede definidos do Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).
