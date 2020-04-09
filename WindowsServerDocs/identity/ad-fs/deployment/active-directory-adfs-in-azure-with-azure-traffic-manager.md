---
title: Implantação AD FS de alta disponibilidade entre regiões no Azure com o Gerenciador de tráfego do Azure | Microsoft Docs
description: Como implantar AD FS no Azure para alta disponibilidade.
services: active-directory
author: anandyadavmsft
manager: mtillman
ms.prod: windows-server
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 9bfb59fadd2cf6b07d3c47ab69f0fe67974706a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855199"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Implantação AD FS de alta disponibilidade entre regiões no Azure com o Gerenciador de tráfego do Azure
[AD FS implantação no Azure](how-to-connect-fed-azure-adfs.md) fornece diretrizes passo a passo sobre como você pode implantar uma infraestrutura de AD FS simples para sua organização no Azure. Este artigo fornece as próximas etapas para criar uma implantação entre as áreas geográficas do AD FS no Azure usando o [Gerenciador de tráfego do Azure](https://docs.microsoft.com/azure/traffic-manager/). O Gerenciador de tráfego do Azure ajuda a criar uma infraestrutura de alto AD FS desempenho e alta disponibilidade geograficamente difundida para sua organização, utilizando a variedade de métodos de roteamento disponíveis para atender a diferentes necessidades da infraestrutura.

Uma infraestrutura de AD FS entre as áreas geográficas altamente disponível permite:

* **Eliminação de ponto único de falha:** Com os recursos de failover do Gerenciador de tráfego do Azure, você pode obter uma infraestrutura de AD FS altamente disponível, mesmo quando um dos data centers de uma parte do globo fica inativo
* **Desempenho aprimorado:** Você pode usar a implantação sugerida neste artigo para fornecer uma infraestrutura de AD FS de alto desempenho que pode ajudar os usuários a se autenticarem mais rapidamente. 

## <a name="design-principles"></a>Princípios de design
![Design geral](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Os princípios básicos de design serão os mesmos listados em princípios de design no artigo AD FS implantação no Azure. O diagrama acima mostra uma extensão simples da implantação básica para outra região geográfica. Abaixo estão alguns pontos a serem considerados ao estender sua implantação para a nova região geográfica

* **Rede virtual:** Você deve criar uma nova rede virtual na região geográfica em que deseja implantar a infraestrutura de AD FS adicional. No diagrama acima, você vê Geo1 VNET e Geo2 VNET como as duas redes virtuais em cada região geográfica.
* **Controladores de domínio e servidores de AD FS em Nova VNET geográfica:** É recomendável implantar controladores de domínio na nova região geográfica para que os servidores AD FS na nova região não precisem entrar em contato com um controlador de domínio em outra rede para concluir uma autenticação e, assim, melhorar o desempenho.
* **Contas de armazenamento:** As contas de armazenamento são associadas a uma região. Como você estará implantando computadores em uma nova região geográfica, precisará criar novas contas de armazenamento a serem usadas na região.  
* **Grupos de segurança de rede:** Como contas de armazenamento, os grupos de segurança de rede criados em uma região não podem ser usados em outra região geográfica. Portanto, será necessário criar novos grupos de segurança de rede semelhantes aos da primeira região geográfica para a sub-rede INT e DMZ na nova região geográfica.
* **Rótulos de DNS para endereços IP públicos:** O Gerenciador de tráfego do Azure pode se referir a pontos de extremidade somente por meio de rótulos DNS. Portanto, é necessário criar rótulos de DNS para os endereços IP públicos dos balanceadores de carga externos.
* **Gerenciador de tráfego do Azure:** Gerenciador de Tráfego do Microsoft Azure permite que você controle a distribuição do tráfego do usuário para seus pontos de extremidade de serviço executados em diferentes data centers em todo o mundo. O Gerenciador de tráfego do Azure funciona no nível do DNS. Ele usa respostas DNS para direcionar o tráfego do usuário final para pontos de extremidade distribuídos globalmente. Em seguida, os clientes se conectam a esses pontos de extremidade diretamente. Com opções de roteamento diferentes de desempenho, peso e prioridade, você pode escolher facilmente a opção de roteamento mais adequada para as necessidades da sua organização. 
* **Conectividade v-net para v-net entre duas regiões:** Você não precisa ter conectividade entre as redes virtuais em si. Como cada rede virtual tem acesso aos controladores de domínio e tem AD FS e servidor WAP em si, eles podem trabalhar sem qualquer conectividade entre as redes virtuais em regiões diferentes. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Etapas para integrar o Gerenciador de tráfego do Azure
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Implantar AD FS na nova região geográfica
Siga as etapas e diretrizes em [AD FS implantação no Azure](how-to-connect-fed-azure-adfs.md) para implantar a mesma topologia na nova região geográfica.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Rótulos de DNS para endereços IP públicos dos balanceadores de carga voltados para a Internet (público)
Conforme mencionado acima, o Gerenciador de tráfego do Azure só pode fazer referência a rótulos DNS como pontos de extremidade e, portanto, é importante criar rótulos de DNS para os endereços IP públicos dos balanceadores de carga externos. A captura de tela abaixo mostra como você pode configurar o rótulo DNS para o endereço IP público. 

![Rótulo DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Implantando o Gerenciador de tráfego do Azure
Siga as etapas abaixo para criar um perfil do Gerenciador de tráfego. Para obter mais informações, você também pode consultar [gerenciar um perfil do Gerenciador de tráfego do Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Criar um perfil do Gerenciador de tráfego:** Dê um nome exclusivo ao seu perfil do Gerenciador de tráfego. Esse nome do perfil faz parte do nome DNS e atua como um prefixo para o rótulo de nome de domínio do Traffic Manager. O nome/prefixo é adicionado a. trafficmanager.net para criar um rótulo DNS para o Gerenciador de tráfego. A captura de tela abaixo mostra o prefixo DNS do Gerenciador de tráfego que está sendo definido como mysts e o rótulo DNS resultante será mysts.trafficmanager.net. 
   
    ![Criação do perfil do Gerenciador de tráfego](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Método de roteamento de tráfego:** Há três opções de roteamento disponíveis no Gerenciador de tráfego:
   
   * Prioridade 
   * Desempenho
   * Ponderada
     
     **Desempenho** é a opção recomendada para obter uma infraestrutura de AD FS altamente responsiva. No entanto, você pode escolher qualquer método de roteamento mais adequado para suas necessidades de implantação. A funcionalidade de AD FS não é afetada pela opção de roteamento selecionada. Consulte [métodos de roteamento de tráfego do Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) para obter mais informações. Na captura de tela de exemplo acima, você pode ver o método de **desempenho** selecionado.
3. **Configurar pontos de extremidade:** Na página Gerenciador de tráfego, clique em pontos de extremidade e selecione Adicionar. Isso abrirá uma página Adicionar ponto de extremidade semelhante à captura de tela abaixo
   
   ![Configurar pontos de extremidade](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Para as diferentes entradas, siga a diretriz abaixo:
   
   **Tipo:** Selecione ponto de extremidade do Azure, pois vamos apontar para um endereço IP público do Azure.
   
   **Nome:** Crie um nome que você deseja associar ao ponto de extremidade. Esse não é o nome DNS e não tem nenhuma influência sobre os registros DNS.
   
   **Tipo de recurso de destino:** Selecione endereço IP público como o valor para essa propriedade. 
   
   **Recurso de destino:** Isso lhe dará a opção de escolher entre os diferentes rótulos de DNS disponíveis em sua assinatura. Escolha o rótulo DNS correspondente ao ponto de extremidade que você está configurando.
   
   Adicione o ponto de extremidade para cada região geográfica à qual você deseja que o Gerenciador de tráfego do Azure encaminhe o tráfego.
   Para obter mais informações e etapas detalhadas sobre como adicionar/configurar pontos de extremidade no Gerenciador de tráfego, consulte [Adicionar, desabilitar, habilitar ou excluir pontos de extremidade](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **Configurar investigação:** Na página Gerenciador de tráfego, clique em configuração. Na página configuração, você precisa alterar as configurações do monitor para investigação na porta HTTP 80 e no caminho relativo/ADFS/Probe
   
    ![Configurar investigação](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Verifique se o status dos pontos de extremidade está online quando a configuração estiver concluída**. Se todos os pontos de extremidade estiverem no estado ' degradado ', o Gerenciador de tráfego do Azure fará uma melhor tentativa de rotear o tráfego supondo que o diagnóstico está incorreto e que todos os pontos de extremidade estão acessíveis.
   > 
   > 
5. **Modificando registros DNS para o Gerenciador de tráfego do Azure:** O serviço de Federação deve ser um CNAME para o nome DNS do Gerenciador de tráfego do Azure. Crie um CNAME nos registros DNS públicos para que qualquer pessoa que esteja tentando acessar o serviço de Federação realmente alcance o Gerenciador de tráfego do Azure.
   
    Por exemplo, para apontar o serviço de Federação fs.fabidentity.com para o Gerenciador de tráfego, você precisa atualizar seu registro de recurso DNS para ser o seguinte:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Testar o roteamento e o AD FS de entrada
### <a name="routing-test"></a>Teste de roteamento
Um teste muito básico para o roteamento seria tentar executar ping no nome DNS do serviço de Federação de um computador em cada região geográfica. Dependendo do método de roteamento escolhido, o ponto de extremidade que realmente executa ping será refletido na exibição de ping. Por exemplo, se você selecionou o roteamento de desempenho, o ponto de extremidade mais próximo à região do cliente será alcançado. Abaixo está o instantâneo de dois pings de duas máquinas de cliente de região diferente, uma na região EastAsia e outra no oeste dos EUA. 

![Teste de roteamento](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>Teste de entrada AD FS
A maneira mais fácil de testar AD FS é usando a página IdpInitiatedSignon. aspx. Para poder fazer isso, é necessário habilitar o IdpInitiatedSignOn nas propriedades de AD FS. Siga as etapas abaixo para verificar sua configuração de AD FS

1. Execute o cmdlet abaixo no servidor de AD FS, usando o PowerShell, para defini-lo como habilitado. 
   Set-Adfsproperties-EnableIdPInitiatedSignonPage $true
2. Em qualquer computador externo, Acesse https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Você deve ver a página AD FS como abaixo:
   
    ![Teste do ADFS – desafio de autenticação](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    e, na entrada bem-sucedida, ele fornecerá uma mensagem de êxito, conforme mostrado abaixo:
   
    ![Teste do ADFS-êxito na autenticação](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Links relacionados
* [Implantação básica de AD FS no Azure](how-to-connect-fed-azure-adfs.md)
* [Gerenciador de Tráfego do Microsoft Azure](https://docs.microsoft.com/azure/traffic-manager/)
* [Métodos de roteamento de tráfego do Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}
* [Gerenciar um perfil do Gerenciador de tráfego do Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Adicionar, desabilitar, habilitar ou excluir pontos de extremidade](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

