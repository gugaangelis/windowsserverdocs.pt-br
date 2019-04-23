---
title: Implantação de FS de alta disponibilidade entre fronteiras geográficas AD no Azure com o Gerenciador de tráfego do Azure | Microsoft Docs
description: Neste documento, você aprenderá como implantar o AD FS no Azure para obter alta disponibilidade.
keywords: O AD fs com o Gerenciador de tráfego do Azure, adfs com Gerenciador de tráfego do Azure, geográfico, multi datacenter, datacenters geográficas, vários centros de dados geográficos, implantar o AD FS no azure, implantar adfs do azure, adfs do azure, azure ad fs, implantar AD FS, implantar o ad fs, adfs no azure, implantar o AD FS no azure, implantar o AD FS no azure, adfs do azure, Introdução ao AD FS, Azure, AD FS no Azure, iaas, ADFS, mover adfs para o azure
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 4af0386ef97694baa9ba7f1e5e7163554d79734a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874117"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Implantação de FS de alta disponibilidade entre fronteiras geográficas AD no Azure com o Gerenciador de tráfego do Azure
[Implantação do AD FS no Azure](how-to-connect-fed-azure-adfs.md) fornece orientação passo a passo sobre como você pode implantar uma infraestrutura do AD FS simples para sua organização no Azure. Este artigo fornece as próximas etapas para criar uma implantação entre fronteiras geográficas do AD FS no Azure usando [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/). O Gerenciador de tráfego do Azure ajuda a criar uma disponibilidade entre fronteiras geográficas e a infraestrutura do AD FS de alto desempenho para sua organização, fazendo uso da gama de métodos de roteamento disponíveis para atender às diferentes necessidades da infraestrutura.

Uma infraestrutura do AD FS entre fronteiras geográficas altamente disponível permite:

* **Eliminação de pontos únicos de falha:** Com recursos de failover do Gerenciador de tráfego do Azure, você pode obter uma infraestrutura altamente disponível do AD FS, mesmo quando um dos data centers em uma parte de todo o mundo fica inativo
* **Desempenho aprimorado:** Você pode usar a implantação sugerida neste artigo para fornecer uma infraestrutura do AD FS de alto desempenho que pode ajudar os usuários a autenticar com mais rapidez. 

## <a name="design-principles"></a>Princípios de design
![Design geral](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Princípios básicos de design serão os mesmos, conforme listado nos princípios de Design no artigo de implantação do AD FS no Azure. O diagrama acima mostra uma extensão simples da implantação básica para outra região geográfica. Abaixo estão alguns pontos a considerar ao estender a implantação para a nova região geográfica

* **Rede virtual:** Você deve criar uma nova rede virtual na região geográfica que você deseja implantar uma infraestrutura do AD FS adicional. No diagrama acima, você vê Geo1 VNET e Geo2 VNET como as duas redes virtuais em cada região geográfica.
* **Controladores de domínio e servidores do AD FS na nova rede virtual geográfica:** É recomendável implantar controladores na nova região geográfica para que os servidores do AD FS na nova região não precise contatar um controlador de domínio em outra longe de rede para concluir uma autenticação e, assim, melhorar o desempenho do domínio.
* **Contas de armazenamento:** Contas de armazenamento estão associadas uma região. Uma vez que você implantará máquinas em uma nova região geográfica, você terá que criar novas contas de armazenamento a ser usado na região.  
* **Grupos de segurança de rede:** Como contas de armazenamento, grupos de segurança de rede criados em uma região não podem ser usadas em outra região geográfica. Portanto, você precisará criar novos grupos de segurança semelhantes na primeira região geográfica para a sub-rede INT e DMZ na nova região geográfica.
* **Rótulos de DNS para endereços IP públicos:** O Azure Traffic Manager pode se referir a pontos de extremidade somente por meio de rótulos de DNS. Portanto, você precisa criar rótulos de DNS para endereços IP públicos dos balanceadores de carga externos.
* **Gerenciador de tráfego do Azure:** Gerenciador de tráfego do Microsoft Azure permite que você controle a distribuição de tráfego do usuário para seus pontos de extremidade de serviço em execução em diferentes datacenters ao redor do mundo. O Azure Traffic Manager funciona no nível do DNS. Ele usa respostas DNS para direcionar o tráfego do usuário final a pontos de extremidade globalmente distribuídos. Os clientes, em seguida, conecte-se os pontos de extremidade diretamente. Com opções diferentes de roteamento de desempenho, ponderado e prioridade, você pode facilmente escolher a opção de roteamento mais adequada às necessidades de sua organização. 
* **Conectividade de V-net para V-net entre duas regiões:** Você não precisa ter conectividade entre as redes virtuais em si. Como cada rede virtual tem acesso aos controladores de domínio e tem o servidor AD FS e WAP em si, eles podem funcionar sem nenhuma conectividade entre as redes virtuais em regiões diferentes. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Etapas para integrar o Gerenciador de tráfego do Azure
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Implantar o AD FS na nova região geográfica
Siga as etapas e diretrizes [implantação do AD FS no Azure](how-to-connect-fed-azure-adfs.md) para implantar a mesma topologia na nova região geográfica.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Rótulos de DNS para endereços IP públicos dos balanceadores de carga de Internet (público)
Conforme mencionado acima, o Gerenciador de tráfego do Azure só pode se referir aos rótulos DNS como pontos de extremidade e, portanto, é importante criar rótulos de DNS para endereços IP públicos dos balanceadores de carga externos. Captura de tela abaixo mostra como você pode configurar seu rótulo de DNS para o endereço IP público. 

![Rótulo de DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Implantando o Azure Traffic Manager
Siga as etapas abaixo para criar um perfil do Gerenciador de tráfego. Para obter mais informações, você pode também consultar [gerenciar um perfil do Gerenciador de tráfego do Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Crie um perfil do Gerenciador de tráfego:** Dê um nome exclusivo de seu perfil do Gerenciador de tráfego. Esse nome do perfil é parte do nome DNS e atua como um prefixo para o rótulo de nome de domínio do Gerenciador de tráfego. O nome / prefixo é adicionado ao. trafficmanager.net para criar um rótulo DNS para o tráfego de Gerenciador. Captura de tela abaixo mostra o Gerenciador de tráfego que está sendo definido como mysts de prefixo DNS e o rótulo de DNS resultante será mysts.trafficmanager.net. 
   
    ![Criação de perfil do Gerenciador de tráfego](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Método de roteamento de tráfego:** Há três opções de roteamento disponíveis no Gerenciador de tráfego:
   
   * Priority 
   * Desempenho
   * Weighted
     
     **Desempenho** é a opção recomendada para alcançar a infraestrutura do AD FS altamente responsiva. No entanto, você pode escolher qualquer método de roteamento mais adequado para suas necessidades de implantação. A funcionalidade do AD FS não é afetada pela opção de roteamento selecionada. Ver [métodos de roteamento de tráfego do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) para obter mais informações. No exemplo de captura de tela acima, você pode ver os **desempenho** método selecionado.
3. **Configure pontos de extremidade:** Na página do Gerenciador de tráfego, clique em pontos de extremidade e selecione Adicionar. Isso abrirá uma página de ponto de extremidade de adicionar semelhante à captura de tela abaixo
   
   ![Configurar pontos de extremidade](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Para as diferentes entradas, siga a diretriz abaixo:
   
   **Tipo:** Selecione o ponto de extremidade do Azure conforme apontaremos para um endereço IP público do Azure.
   
   **Nome:** Crie um nome que você deseja associar o ponto de extremidade. Isso não é o nome DNS e registros de DNS não tem nenhuma relevância.
   
   **Tipo de recurso de destino:** Selecione o endereço IP público como o valor para essa propriedade. 
   
   **Recurso de destino:** Isso lhe dará uma opção para escolher os rótulos DNS diferentes que têm disponíveis em sua assinatura. Escolha o rótulo DNS correspondente ao ponto de extremidade que você está configurando.
   
   Adicione ponto de extremidade para cada região geográfica que você deseja que o Gerenciador de tráfego do Azure para rotear o tráfego.
   Para obter mais informações e etapas detalhadas sobre como adicionar e configurar pontos de extremidade no Gerenciador de tráfego, consulte [adicionar, desabilitar, habilitar ou excluir pontos de extremidade](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **Configure investigação:** Na página do Gerenciador de tráfego, clique em configuração. Na página de configuração, você precisa alterar as configurações de monitor de investigação em HTTP na porta 80 e caminho relativo /adfs/probe
   
    ![Configurar investigação](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Certifique-se de que o status dos pontos de extremidade é ONLINE quando a configuração estiver concluída**. Se todos os pontos de extremidade estão em estado 'degradado', o Azure Traffic Manager fará tentará encaminhar o tráfego, supondo que o diagnóstico esteja incorreto e todos os pontos de extremidade estão acessíveis.
   > 
   > 
5. **Modificar registros DNS do Azure Traffic Manager:** O serviço de federação deve ser um CNAME para o nome DNS do Gerenciador de tráfego do Azure. Crie um CNAME nos registros de DNS públicos para que a pessoa que está tentando acessar o serviço de Federação realmente atinge o Gerenciador de tráfego do Azure.
   
    Por exemplo, para apontar o fs.fabidentity.com do serviço de federação para o Gerenciador de tráfego, você precisaria atualizar o registro de recurso DNS para o seguinte:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Testar o roteamento e a entrada do AD FS
### <a name="routing-test"></a>Teste de roteamento
Um teste muito básico para o roteamento seria tentar executar o ping do nome do DNS do serviço de federação de um computador em cada região geográfica. Dependendo do método de roteamento escolhido, o ponto de extremidade que realmente executa ping será refletido na exibição do ping. Por exemplo, se você tiver selecionado o roteamento de desempenho, em seguida, o ponto de extremidade mais próximo à região do cliente será atingido. Abaixo está o instantâneo de dois pings de dois computadores cliente de região diferente, um na região Leste da Ásia e outra no Oeste dos EUA. 

![Teste de roteamento](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS sign-in de teste
É a maneira mais fácil de testar o AD FS usando a página Idpinitiatedsignon. Para que seja possível fazer isso, é necessário habilitar IdpInitiatedSignOn nas propriedades do AD FS. Siga as etapas abaixo para verificar a instalação do AD FS

1. Execute o cmdlet abaixo no servidor do AD FS, usando o PowerShell, para defini-lo como habilitado. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. De qualquer computador externo acesse https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Você deve ver a página do AD FS como abaixo:
   
    ![Teste de ADFS - desafio de autenticação](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    e entrar com êxito, ele fornecerá uma mensagem de êxito conforme mostrado abaixo:
   
    ![Teste de ADFS - êxito da autenticação](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Links relacionados
* [Implantação básica do AD FS no Azure](how-to-connect-fed-azure-adfs.md)
* [Gerenciador de tráfego do Microsoft Azure](https://docs.microsoft.com/azure/traffic-manager/)
* [Métodos de roteamento de tráfego do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Próximas etapas
* [Gerenciar um perfil do Gerenciador de tráfego do Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Adicionar, desabilitar, habilitar ou excluir pontos de extremidade](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

