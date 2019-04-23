---
title: Serviços de Federação do Active Directory no Azure | Microsoft Docs
description: Neste documento, você aprenderá como implantar o AD FS no Azure para obter alta disponibilidade.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 588bc3f87c78feccac47d18d31d37be3b1a02d2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835097"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Implantando serviços de Federação do Active Directory no Azure
O AD FS fornece federação de identidade simplificada e segura e recursos de logon único (SSO) da Web. Federação com o Azure AD ou o O365 habilita os usuários a autenticar usando credenciais locais e acessar todos os recursos na nuvem. Como resultado, é importante ter uma infra-estrutura altamente disponível do AD FS para garantir o acesso aos recursos locais e na nuvem. Implantar o AD FS no Azure pode ajudar a atingir a alta disponibilidade necessária com esforço mínimo.
Há várias vantagens de implantar o AD FS no Azure, algumas delas estão listadas abaixo:

* **Alta disponibilidade** -com o poder dos conjuntos de disponibilidade do Azure, você certifique-se de uma infra-estrutura altamente disponível.
* **Fácil de dimensionar** – precisar de mais desempenho? Migre facilmente para computadores mais eficientes com apenas alguns cliques no Azure
* **Redundância geográfica cruzada** – com redundância geográfica do Azure você pode ter certeza de que sua infraestrutura é altamente disponível em todo o mundo
* **Fácil de gerenciar** – com opções de gerenciamento altamente simplificado no portal do Azure, gerenciar sua infraestrutura é muito fácil e sem complicações 

## <a name="design-principles"></a>Princípios de design
![Design de implantação](./media/how-to-connect-fed-azure-adfs/deployment.png)

O diagrama acima mostra a topologia básica recomendada para começar a implantar a infraestrutura do AD FS no Azure. Os princípios por trás de vários componentes da topologia são listados abaixo:

* **Controlador de domínio / servidores ADFS**: Se você tiver menos de 1.000 usuários, que você pode simplesmente instale a função do AD FS em seus controladores de domínio. Se você não quiser que algum impacto no desempenho nos controladores de domínio ou se você tiver mais de 1.000 usuários, em seguida, implante o AD FS em servidores separados.
* **Servidor WAP** – é necessário implantar servidores de Proxy de aplicativo Web, para que os usuários possam acessar o AD FS quando não estiverem na rede da empresa também.
* **DMZ**: Os servidores de Proxy de aplicativo Web serão colocados na rede de Perímetro e o acesso TCP/443 só é permitido entre a rede de Perímetro e a sub-rede interna.
* **Balanceadores de carga**: Para garantir a alta disponibilidade dos servidores do AD FS e Proxy de aplicativo Web, é recomendável usar um balanceador de carga interno para servidores do AD FS e Azure Load Balancer para servidores de Proxy de aplicativo Web.
* **Conjuntos de disponibilidade**: Para fornecer redundância para sua implantação do AD FS, é recomendável que você agrupe duas ou mais máquinas virtuais em um conjunto de disponibilidade para cargas de trabalho semelhantes. Essa configuração garante que durante a um evento de manutenção planejada ou não planejada, pelo menos uma máquina virtual estará disponível
* **Contas de armazenamento**: É recomendável ter duas contas de armazenamento. Ter uma única conta de armazenamento pode levar à criação de um ponto único de falha e pode fazer com que a implantação fique indisponível em um cenário improvável, em que a conta de armazenamento fica inativa. Duas contas de armazenamento ajudarão a associar uma conta de armazenamento para cada linha de falha.
* **Segregação de rede**:  Servidores de Proxy de aplicativo da Web devem ser implantados em uma rede de Perímetro separada. Você pode dividir uma rede virtual em duas sub-redes e implantar os servidores de Proxy de aplicativo Web em uma sub-rede isolada. Você pode simplesmente definir as configurações de grupo de segurança de rede para cada sub-rede e permitir somente a comunicação necessária entre as duas sub-redes. Mais detalhes são fornecidos para cada cenário de implantação abaixo

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Etapas para implantar o AD FS no Azure
As etapas mencionadas nesta seção descrevem o guia para implantar o abaixo de infraestrutura do AD FS descrita no Azure.

### <a name="1-deploying-the-network"></a>1. Implantar a rede
Conforme descrito acima, você pode tanto criar duas sub-redes em uma única rede virtual ou então criar duas redes virtuais completamente diferentes (VNet). Este artigo concentre-se sobre como implantar uma única rede virtual e dividi-la em duas sub-redes. Atualmente, isso é uma abordagem mais fácil, pois duas redes virtuais separadas exigiria uma rede virtual para o gateway de rede virtual para comunicações.

**1.1 Criar rede virtual**

![Criar rede virtual](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

No portal do Azure, selecione uma rede virtual e você pode implantar a rede virtual e uma sub-rede imediatamente com apenas um clique. A sub-rede INT também é definida e agora está pronta para VMs a serem adicionados.
A próxima etapa é adicionar outra sub-rede na rede, ou seja, a sub-rede de Perímetro. Para criar a sub-rede de rede de Perímetro, simplesmente

* Selecione a rede recém-criada
* Nas propriedades, selecione subrede
* Na sub-rede do painel, clique no botão Adicionar
* Forneça os nome e endereço espaço informações de sub-rede para criar a sub-rede

![Sub-rede](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![Subnet DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2. Criar grupos de segurança de rede**

Um grupo de segurança de rede (NSG) contém uma lista de regras de lista de controle de acesso (ACL) que permitem ou negam o tráfego de rede para suas instâncias de VM em uma rede Virtual. Os NSGs podem ser associados a sub-redes ou instâncias de VM individuais dentro dessa sub-rede. Quando um NSG está associado uma sub-rede, as regras ACL se aplicam a todas as instâncias VM na sub-rede.
Com a finalidade deste guia, vamos criar dois NSGs: um para uma rede interna e uma rede de Perímetro. Eles serão ser rotulados como NSG_INT e NSG_DMZ, respectivamente.

![Criar NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Depois que o NSG é criado, haverá 0 regra de saída de entrada e de 0. Depois que as funções nos respectivos servidores estiverem instaladas e funcionais, as regras de entrada e saídas podem ser feitas acordo com o nível de segurança desejado.

![Inicializar NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Depois que os NSGs forem criados, associe NSG_INT à sub-rede INT e NSG_DMZ à sub-rede DMZ. Uma captura de tela de exemplo é fornecida abaixo:

![Configurar NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Clique em sub-redes para abrir o painel para sub-redes
* Selecione a sub-rede para associar o NSG 

Após a configuração, o painel para sub-redes deve ser semelhante a:

![Subredes após NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3. Criar Conexão local**

Precisaremos uma conexão para o local para implantar o controlador de domínio (DC) no azure. O Azure oferece várias opções de conectividade para conectar-se a sua infraestrutura local para sua infraestrutura do Azure.

* Point-to-site
* Site a site da rede virtual
* ExpressRoute

É recomendável usar o ExpressRoute. O ExpressRoute permite criar conexões privadas entre datacenters do Azure e infraestrutura que está em suas instalações ou em um ambiente de colocalização. As conexões do ExpressRoute não passam pela Internet pública. Eles oferecem mais confiabilidade, velocidades mais rápidas, latências menores e segurança maior do que conexões típicas pela Internet.
Embora seja recomendável usar o ExpressRoute, você pode escolher qualquer método de conexão mais adequado para sua organização. Para saber mais sobre o ExpressRoute e as várias opções de conectividade usando o ExpressRoute, leia [visão geral técnica do ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Criar contas de armazenamento
Para manter a alta disponibilidade e evitar a dependência de uma única conta de armazenamento, você pode criar duas contas de armazenamento. Divida os computadores em cada conjunto de disponibilidade em dois grupos e, em seguida, atribuir a cada grupo de uma conta de armazenamento separada.

![Criar contas de armazenamento](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Criar conjuntos de disponibilidade
Para cada função (DC/AD FS e WAP), crie conjuntos de disponibilidade que conterão os 2 computadores cada, no mínimo. Isso ajudará a obter maior disponibilidade para cada função. Embora criar a conjuntos de disponibilidade, é essencial para decidir o seguinte:

* **Domínios de falha**: Máquinas virtuais no mesmo domínio de falha compartilham a mesma fonte de energia e o comutador de rede física. Um mínimo de 2 domínios de falha são recomendados. O valor padrão é 3 e você pode deixá-la como está para os fins desta implantação
* **Domínios de atualização**: As máquinas que pertencem ao mesmo domínio de atualização são reiniciadas juntas durante uma atualização. Você deseja ter no mínimo 2 domínios de atualização. O valor padrão é 5 e você pode deixá-la como está para os fins desta implantação

![Conjuntos de disponibilidade](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Criar os seguintes conjuntos de disponibilidade

| Conjunto de disponibilidade | Função | Domínios de falhas | Domínios de atualização |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Implantar máquinas virtuais
A próxima etapa é implantar máquinas virtuais que hospedam as diferentes funções na sua infraestrutura. Um mínimo de duas máquinas são recomendadas em cada conjunto de disponibilidade. Crie quatro máquinas virtuais para a implantação básica.

| Computador | Função | Sub-rede | Conjunto de disponibilidade | Conta de armazenamento | Endereço IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Estático |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Estático |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Estático |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Estático |

Como você deve ter notado, nenhum NSG foi especificado. Isso ocorre porque o azure permite que você use NSG no nível de sub-rede. Em seguida, você pode controlar o tráfego de rede de máquina usando o NSG individual associado com a sub-rede ou então o objeto NIC. Leia mais sobre [o que é um grupo de segurança de rede (NSG)](https://aka.ms/Azure/NSG).
Endereço IP estático é recomendável se você estiver gerenciando o DNS. Você pode usar o DNS do Azure e, em vez disso, nos registros de DNS para seu domínio, consulte novas máquinas por seus FQDNs do Azure.
O painel da máquina virtual deve ter aparência abaixo após a conclusão da implantação:

![Máquinas virtuais implantadas](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configurar o controlador de domínio / servidores do AD FS
 Para autenticar qualquer solicitação de entrada, será necessário contatar o controlador de domínio do AD FS. Para evitar a viagem dispendiosa do Azure para o controlador de domínio local para autenticação, é recomendável implantar uma réplica do controlador de domínio no Azure. Para alcançar alta disponibilidade, é recomendável criar um conjunto de disponibilidade pelo menos 2 controladores de domínio.

| Controlador de domínio | Função | Conta de armazenamento |
|:---:|:---:|:---:|
| contosodc1 |Réplica |contososac1 |
| contosodc2 |Réplica |contososac2 |

* Promova os dois servidores como controladores de domínio de réplica com DNS
* Configure os servidores do AD FS instalando a função do AD FS usando o Gerenciador do servidor.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Implantar o balanceador de carga interno (ILB)
**6.1. Criar ILB**

Para implantar um ILB, selecione balanceadores de carga no portal do Azure e clique em Adicionar (+).

> [!NOTE]
> Se você não vir **balanceadores de carga** no menu, clique em **procurar** no canto inferior esquerdo do portal e role até ver **balanceadores de carga**.  Em seguida, clique na estrela amarela para adicioná-lo ao menu. Agora, selecione o novo ícone de Balanceador de carga para abrir o painel para iniciar a configuração do balanceador de carga.
> 
> 

![Procurar balanceador de carga](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nome**: Atribua qualquer nome adequado ao balanceador de carga
* **Esquema de**: Uma vez que este balanceador de carga será colocado na frente de servidores do AD FS e destina-se apenas a conexões de rede interna, selecione "Interno"
* **Rede virtual**: Escolha a rede virtual em que você está implantando o AD FS
* **Subrede**: Selecione a sub-rede interna aqui
* **Atribuição de endereço IP**: Estático

![Balanceador de carga interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Depois de clicar em criar e o ILB é implantado, você verá na lista de balanceadores de carga:

![Balanceadores de carga após ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

Próxima etapa é configurar o pool de back-end e a investigação de back-end.

**6.2. Configurar o pool de back-end do ILB**

Selecione o ILB recém-criado no painel balanceadores de carga. Ele será aberto o painel de configurações. 

1. Selecione pools de back-end no painel de configurações
2. No painel Adicionar pool de back-end, clique em Adicionar máquina virtual
3. Você verá um painel onde você pode escolher o conjunto de disponibilidade
4. Escolha o conjunto de disponibilidade do AD FS

![Configurar o pool de back-end do ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3. Configurando investigação**

No painel de configurações de ILB, selecione as investigações de integridade.

1. Clique em Adicionar
2. Forneça detalhes para investigação de um. **Nome**: Nome de investigação b. **Protocol**: C HTTP. **Port**: 80 (HTTP) d. **Caminho**: / adfs/investigação eletrônico. **Intervalo de**: 5 (valor padrão) – é o intervalo no qual o ILB investigará as máquinas no pool de back-end f. **Limite não íntegro**: 2 (valor padrão) – isso é o limite de falhas de investigação consecutivas após o qual o ILB declarará uma máquina no pool de back-end não está respondendo e interromperá o envio de tráfego a ele.

![Configurar investigação ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Estamos usando o ponto de extremidade /adfs/probe que foi criado explicitamente para verificações de integridade em um ambiente do AD FS em que uma verificação completa do caminho HTTPS não pode ocorrer.  Isso é consideravelmente melhor que uma verificação básica porta 443, que não refletem com precisão o status de uma implantação do AD FS modernos.  Para obter mais informações sobre isso podem ser encontradas em https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6.4. Criar regras de balanceamento de carga**

Para efetivamente equilibrar o tráfego, o ILB deve ser configurado com regras de balanceamento de carga. Para criar uma regra de balanceamento de carga 

1. Selecione a regra no painel de configurações do ILB de balanceamento de carga
2. Clique em Adicionar no painel de regra de balanceamento de carga
3. No painel de regra de balanceamento de carga de adicionar um. **Nome**: Forneça um nome para a regra b. **Protocol**: Selecione TCP c. **Port**: 443 d. **Porta de back-end**: 443 e. **Pool de back-end**: Selecione o pool que você criou para o AD FS anteriormente f do cluster. **Investigação**: Selecione a investigação criada anteriormente para servidores do AD FS

![Configurar regras de balanceamento ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5. Atualizar DNS com ILB**

Vá para o servidor DNS e crie um CNAME para o ILB. O CNAME deve ser para o serviço de federação com o endereço IP que aponta para o endereço IP do ILB. Por exemplo, se o endereço DIP ILB for 10.3.0.8 e o serviço de Federação instalado for fs.contoso.com, em seguida, crie um CNAME para fs.contoso.com apontando para 10.3.0.8.
Isso garantirá que todas as comunicações relacionadas a FS.contoso.com direcionadas para o ILB e sejam roteadas adequadamente.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configurando o servidor de Proxy de aplicativo Web
**7.1. Configurando os servidores de Proxy de aplicativo Web para acessar os servidores do AD FS**

Para garantir que os servidores de Proxy de aplicativo Web capazes de alcançar os servidores do AD FS por trás do ILB, crie um registro no %systemroot%\system32\drivers\etc\hosts para o ILB. Observe que o nome diferenciado (DN) deve ser o nome de serviço de federação, por exemplo, fs.contoso.com. E a entrada IP deve ser de endereço IP do ILB (10.3.0.8, como no exemplo).

**7.2. Instalar a função do Proxy de aplicativo Web**

Depois de garantir que os servidores de Proxy de aplicativo Web sejam capazes de acessar os servidores do AD FS por trás do ILB, você pode, em seguida, instale os servidores de Proxy de aplicativo Web. Servidores de Proxy de aplicativo Web não precisam estar associados ao domínio. Instale as funções de Proxy de aplicativo Web em dois servidores de Proxy de aplicativo Web, selecionando a função acesso remoto. O Gerenciador do servidor irá guiá-lo para concluir a instalação do WAP.
Para obter mais informações sobre como implantar o WAP, leia [instalar e configurar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  Implantar a balanceador de carga (públicos) de Internet
**8.1.  Criar balanceador de carga (públicos) para a Internet**

No portal do Azure, selecione balanceadores de carga e, em seguida, clique em Adicionar. No painel de Balanceador de carga de criar, insira as seguintes informações

1. **Nome**: Nome para o balanceador de carga
2. **Esquema de**: Público – essa opção informa ao Azure que este balanceador de carga precisará de um endereço público.
3. **Endereço IP**: Criar um novo endereço IP (dinâmico)

![Balanceador de carga para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Após a implantação, o balanceador de carga será exibido na lista de balanceadores de carga.

![Lista de balanceadores de carga](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2. Atribuir um rótulo DNS ao IP público**

Clique na entrada do balanceador de carga recém-criado no painel de balanceadores de carga para abrir o painel de configuração. Siga as etapas abaixo para configurar o rótulo DNS para o IP público:

1. Clique no endereço IP público. Isso abrirá o painel para o IP público e suas configurações
2. Clique em configuração
3. Forneça um rótulo DNS. Isso se tornará o rótulo DNS público que você pode acessar de qualquer lugar, por exemplo, contosofs.westus.cloudapp.azure.com. Você pode adicionar uma entrada no DNS externo para o serviço de Federação (como fs.contoso.com) que resolve para o rótulo DNS do balanceador de carga externo (contosofs.westus.cloudapp.azure.com).

![Configurar balanceador de carga para a internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurar balanceador de carga (DNS) para a internet](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3. Configurar o pool de back-end para o balanceador de carga de Internet (público)** 

Siga as mesmas etapas usadas para criar o balanceador de carga interno para configurar o pool de back-end para a Internet (público) balanceador de carga como a conjunto de disponibilidade para os servidores WAP. Por exemplo, contosowapset.

![Configurar o pool de back-end do balanceador de carga voltado para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4. Configurar investigação**

Siga as mesmas etapas usadas para configurar o balanceador de carga interno para configurar a investigação para o pool de back-end de servidores WAP.

![Configurar investigação do balanceador de carga voltado para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5. Criar regra (s) de balanceamento de carga**

Siga as mesmas etapas usadas no ILB para configurar a balanceamento de carga regra para TCP 443.

![Configurar regras de balanceamento do balanceador de carga voltado para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Segurança da rede
**9.1. Proteger a sub-rede interna**

Em geral, você precisa de regras a seguir para proteger com eficiência sua sub-rede interna (na ordem listada abaixo)

| Regra | Descrição | Fluxo |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Permitir a comunicação HTTPS de rede de Perímetro |Entrada |
| DenyInternetOutbound |Sem acesso à internet |Saída |

![Regras de acesso INT (entrada)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

<!--
[comment]: <> (![INT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgintinbound.png))
[comment]: <> (![INT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgintoutbound.png))
-->

**9.2. Proteger a sub-rede de rede de Perímetro**

| Regra | Descrição | Fluxo |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Permitir HTTPS da internet para a rede de Perímetro |Entrada |
| DenyInternetOutbound |Qualquer coisa, exceto HTTPS para a internet está bloqueada |Saída |

![Regras de acesso EXT (entrada)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

<!--
[comment]: <> (![EXT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzinbound.png))
[comment]: <> (![EXT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzoutbound.png))
-->

> [!NOTE]
> Se a autenticação de certificado de usuário do cliente (autenticação do clientTLS usando X509 certificados de usuário) for necessária, o AD FS requer TCP porta 49443 seja habilitada para acesso de entrada.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. Testar a entrada do AD FS
A maneira mais fácil é testar o AD FS usando a página Idpinitiatedsignon. Para que seja possível fazer isso, é necessário habilitar IdpInitiatedSignOn nas propriedades do AD FS. Siga as etapas abaixo para verificar a instalação do AD FS

1. Execute o cmdlet abaixo no servidor do AD FS, usando o PowerShell, para defini-lo como habilitado.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Em qualquer computador externo, acesse https:\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Você deve ver a página do AD FS como abaixo:

![Página de logon de teste](./media/how-to-connect-fed-azure-adfs/test1.png)

Entrar com êxito, ele fornecerá uma mensagem de êxito conforme mostrado abaixo:

![Êxito do teste](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Modelo de implantação do AD FS no Azure
O modelo implanta uma configuração de 6 computador, 2 para controladores de domínio, o AD FS e WAP.

[AD FS no modelo de implantação do Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Você pode usar uma rede virtual existente ou criar uma nova rede virtual durante a implantação desse modelo. Os diversos parâmetros disponíveis para personalizar a implantação estão listados abaixo com a descrição do uso do parâmetro no processo de implantação. 

| Parâmetro | Descrição |
|:--- |:--- |
| Location |A região para implantar os recursos, por exemplo, Leste-nos. |
| StorageAccountType |O tipo de conta de armazenamento criada |
| VirtualNetworkUsage |Indica se uma nova rede virtual será criado ou usar um existente |
| VirtualNetworkName |O nome da rede Virtual para criar, obrigatória no uso da rede virtual nova ou existente |
| VirtualNetworkResourceGroupName |Especifica o nome do grupo de recursos onde reside a rede virtual existente. Ao usar uma rede virtual existente, isso se torna um parâmetro obrigatório para que a implantação possa encontrar a ID da rede virtual existente |
| VirtualNetworkAddressRange |O intervalo de endereços da nova VNET, obrigatória se criando uma nova rede virtual |
| InternalSubnetName |O nome da sub-rede interna, obrigatório em ambas as opções de uso de rede virtual (novas ou existentes) |
| InternalSubnetAddressRange |O intervalo de endereços da sub-rede interna, que contém os servidores de controladores de domínio e o ADFS, obrigatórios se criando uma nova rede virtual. |
| DMZSubnetAddressRange |O intervalo de endereços da sub-rede dmz, que contém os Windows servidores proxy de aplicativo, obrigatórios, se criar uma nova rede virtual. |
| DMZSubnetName |O nome da sub-rede interna, obrigatório em ambas as opções de uso de rede virtual (novas ou existentes). |
| ADDC01NICIPAddress |O endereço IP interno do primeiro controlador de domínio, esse endereço IP será atribuído estaticamente para o controlador de domínio e deve ser um endereço ip válido dentro da sub-rede interna |
| ADDC02NICIPAddress |O endereço IP interno do segundo controlador de domínio, esse endereço IP será atribuído estaticamente para o controlador de domínio e deve ser um endereço ip válido dentro da sub-rede interna |
| ADFS01NICIPAddress |O endereço IP interno do primeiro servidor ADFS; esse endereço IP será atribuído estaticamente para o servidor ADFS e deve ser um endereço ip válido dentro da sub-rede interna |
| ADFS02NICIPAddress |O endereço IP interno do segundo servidor ADFS; esse endereço IP será atribuído estaticamente para o servidor ADFS e deve ser um endereço ip válido dentro da sub-rede interna |
| WAP01NICIPAddress |O endereço IP interno do primeiro servidor WAP; esse endereço IP será atribuído estaticamente para o servidor WAP e deve ser um endereço ip válido dentro da sub-rede DMZ |
| WAP02NICIPAddress |O endereço IP interno do segundo servidor WAP; esse endereço IP será atribuído estaticamente para o servidor WAP e deve ser um endereço ip válido dentro da sub-rede DMZ |
| ADFSLoadBalancerPrivateIPAddress |O endereço IP interno do balanceador de carga do ADFS, esse endereço IP será atribuído estaticamente ao balanceador de carga e deve ser um endereço ip válido dentro da sub-rede interna |
| ADDCVMNamePrefix |Prefixo de nome de máquina virtual para controladores de domínio |
| ADFSVMNamePrefix |Prefixo de nome de máquina virtual para servidores ADFS |
| WAPVMNamePrefix |Prefixo de nome de máquina virtual para os servidores do WAP |
| ADDCVMSize |O tamanho da vm dos controladores de domínio |
| ADFSVMSize |O tamanho da vm dos servidores ADFS |
| WAPVMSize |O tamanho da vm dos servidores do WAP |
| AdminUserName |O nome do administrador local das máquinas virtuais |
| AdminPassword |A senha para a conta de administrador local das máquinas virtuais |

## <a name="additional-resources"></a>Recursos adicionais
* [Conjuntos de disponibilidade](https://aka.ms/Azure/Availability) 
* [Balanceador de carga do Azure](https://aka.ms/Azure/ILB)
* [Balanceador de carga interno](https://aka.ms/Azure/ILB/Internal)
* [Balanceador de carga para Internet](https://aka.ms/Azure/ILB/Internet)
* [Contas de armazenamento](https://aka.ms/Azure/Storage)
* [Redes virtuais do Azure](https://aka.ms/Azure/VNet)
* [O AD FS e Links de Proxy de aplicativo da Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Próximas etapas
* [Integrando suas identidades locais ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configurando e gerenciando o AD FS usando o Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Implantação de FS de alta disponibilidade entre fronteiras geográficas AD no Azure com o Gerenciador de tráfego do Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
