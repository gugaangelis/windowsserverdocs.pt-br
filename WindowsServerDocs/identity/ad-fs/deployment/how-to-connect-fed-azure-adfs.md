---
title: Serviços de Federação do Active Directory (AD FS) no Azure | Microsoft Docs
description: Neste documento, você aprenderá a implantar o AD FS no Azure para alta disponibilidade.
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
ms.openlocfilehash: 15ebf21973f78ad705aca77e178005dfa9529cf2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868221"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Implantando Serviços de Federação do Active Directory (AD FS) no Azure
O AD FS fornece recursos de SSO (logon único) da Web e de Federação de identidades simplificados e seguros. A Federação com o Azure AD ou o O365 permite que os usuários se autentiquem usando credenciais locais e acessem todos os recursos na nuvem. Como resultado, é importante ter uma infraestrutura de AD FS altamente disponível para garantir o acesso a recursos locais e na nuvem. A implantação de AD FS no Azure pode ajudar a alcançar a alta disponibilidade necessária com esforços mínimos.
Há várias vantagens de implantar AD FS no Azure, alguns deles estão listados abaixo:

* **Alta disponibilidade** -com o poder dos conjuntos de disponibilidade do Azure, você garante uma infraestrutura altamente disponível.
* **Fácil de dimensionar** – precisa de mais desempenho? Migre facilmente para computadores mais potentes com apenas alguns cliques no Azure
* **Redundância entre regiões geográficas** – com a redundância geográfica do Azure, você pode ter certeza de que sua infraestrutura está altamente disponível em todo o mundo
* **Fácil de gerenciar** – com opções de gerenciamento altamente simplificadas no portal do Azure, gerenciar sua infraestrutura é muito fácil e sem complicações 

## <a name="design-principles"></a>Princípios de design
![Design de implantação](./media/how-to-connect-fed-azure-adfs/deployment.png)

O diagrama acima mostra a topologia básica recomendada para iniciar a implantação de sua infraestrutura de AD FS no Azure. Os princípios por trás dos vários componentes da topologia estão listados abaixo:

* **Servidores DC/ADFS**: Se você tiver menos de 1.000 usuários, poderá simplesmente instalar AD FS função em seus controladores de domínio. Se você não quiser nenhum impacto no desempenho nos controladores de domínio ou se tiver mais de 1.000 usuários, implante AD FS em servidores separados.
* **Servidor WAP** – é necessário implantar servidores de proxy de aplicativo Web, para que os usuários possam acessar o AD FS quando não estiverem na rede da empresa também.
* **DMZ**: Os servidores proxy de aplicativo Web serão colocados na DMZ e somente o acesso TCP/443 será permitido entre o DMZ e a sub-rede interna.
* **Balanceadores de carga**: Para garantir a alta disponibilidade de AD FS e de servidores proxy de aplicativo Web, é recomendável usar um balanceador de carga interno para servidores AD FS e Azure Load Balancer para servidores proxy de aplicativo Web.
* **Conjuntos de disponibilidade**: Para fornecer redundância à sua implantação de AD FS, é recomendável agrupar duas ou mais máquinas virtuais em um conjunto de disponibilidade para cargas de trabalho semelhantes. Essa configuração garante que, durante um evento de manutenção planejada ou não planejada, pelo menos uma máquina virtual estará disponível
* **Contas de armazenamento**: É recomendável ter duas contas de armazenamento. Ter uma única conta de armazenamento pode levar à criação de um único ponto de falha e pode fazer com que a implantação se torne indisponível em um cenário improvável em que a conta de armazenamento fique inativa. Duas contas de armazenamento ajudarão a associar uma conta de armazenamento a cada linha de falha.
* **Segregação de rede**:  Os servidores proxy de aplicativo Web devem ser implantados em uma rede DMZ separada. Você pode dividir uma rede virtual em duas sub-redes e, em seguida, implantar o (s) servidor (es) de proxy de aplicativo Web em uma sub-rede isolada. Você pode simplesmente definir as configurações de grupo de segurança de rede para cada sub-rede e permitir apenas a comunicação necessária entre as duas sub-redes. Mais detalhes são fornecidos por cenário de implantação abaixo

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Etapas para implantar AD FS no Azure
As etapas mencionadas nesta seção descrevem o guia para implantar o descrito a seguir, representado AD FS infraestrutura no Azure.

### <a name="1-deploying-the-network"></a>1. Implantando a rede
Conforme descrito acima, você pode criar duas sub-redes em uma única rede virtual ou então criar duas redes virtuais completamente diferentes (VNet). Este artigo se concentrará na implantação de uma única rede virtual e a dividirá em duas sub-redes. Atualmente, essa é uma abordagem mais fácil, pois duas VNets separadas exigirão uma VNet para o gateway de VNet para comunicações.

**1,1 criar rede virtual**

![Criar rede virtual](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

No portal do Azure, selecione rede virtual e você pode implantar a rede virtual e uma sub-rede imediatamente com apenas um clique. A sub-rede INT também é definida e está pronta agora para que as VMs sejam adicionadas.
A próxima etapa é adicionar outra sub-rede à rede, ou seja, a sub-rede DMZ. Para criar a sub-rede DMZ, simplesmente

* Selecione a rede recém-criada
* Nas propriedades, selecione sub-rede
* No painel sub-rede, clique no botão Adicionar
* Forneça o nome da sub-rede e as informações de espaço de endereço para criar a sub-rede

![Subnet](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![DMZ da sub-rede](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1,2. Criando os grupos de segurança de rede**

Um NSG (grupo de segurança de rede) contém uma lista de regras de ACL (lista de controle de acesso) que permitem ou negam o tráfego de rede para suas instâncias de VM em uma rede virtual. NSGs pode ser associado a sub-redes ou a instâncias de VM individuais dentro dessa sub-rede. Quando um NSG é associado a uma sub-rede, as regras de ACL se aplicam a todas as instâncias de VM nessa sub-rede.
Para fins deste guia, criaremos dois NSGs: um para uma rede interna e uma DMZ. Eles serão rotulados NSG_INT e NSG_DMZ, respectivamente.

![Criar NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Depois que o NSG for criado, haverá 0 regras de saída e 0. Depois que as funções nos respectivos servidores estiverem instaladas e funcionais, as regras de entrada e saída poderão ser feitas de acordo com o nível de segurança desejado.

![Inicializar NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Depois que os NSGs forem criados, associe NSG_INT à sub-rede INT e NSG_DMZ com a rede DMZ. Uma captura de tela de exemplo é fornecida abaixo:

![Configurar NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Clique em sub-redes para abrir o painel de sub-redes
* Selecione a sub-rede a ser associada ao NSG 

Após a configuração, o painel para sub-redes deve ter a seguinte aparência:

![Sub-redes após NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1,3. Criar conexão com o local**

Precisaremos de uma conexão local para implantar o controlador de domínio (DC) no Azure. O Azure oferece várias opções de conectividade para conectar sua infraestrutura local à sua infraestrutura do Azure.

* Ponto a site
* Site a site de rede virtual
* ExpressRoute

É recomendável usar o ExpressRoute. O ExpressRoute permite criar conexões privadas entre os data centers do Azure e a infraestrutura que está em seu local ou em um ambiente de colocalização. As conexões do ExpressRoute não passam pela Internet pública. Eles oferecem mais confiabilidade, velocidades mais rápidas, latências menores e maior segurança do que as conexões típicas pela Internet.
Embora seja recomendável usar o ExpressRoute, você pode escolher qualquer método de conexão mais adequado para sua organização. Para saber mais sobre o ExpressRoute e as várias opções de conectividade usando o ExpressRoute, leia [visão geral técnica do expressroute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Criar contas de armazenamento
Para manter a alta disponibilidade e evitar a dependência em uma única conta de armazenamento, você pode criar duas contas de armazenamento. Divida os computadores em cada conjunto de disponibilidade em dois grupos e atribua a cada grupo uma conta de armazenamento separada.

![Criar contas de armazenamento](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Criar conjuntos de disponibilidade
Para cada função (DC/AD FS e WAP), crie conjuntos de disponibilidade que conterão 2 máquinas, cada um no mínimo. Isso ajudará a obter maior disponibilidade para cada função. Ao criar os conjuntos de disponibilidade, é essencial decidir o seguinte:

* **Domínios de falha**: As máquinas virtuais no mesmo domínio de falha compartilham a mesma fonte de alimentação e o mesmo comutador de rede física. Um mínimo de 2 domínios de falha são recomendados. O valor padrão é 3 e você pode deixá-lo como está para a finalidade desta implantação
* **Domínios de atualização**: Os computadores que pertencem ao mesmo domínio de atualização são reiniciados juntos durante uma atualização. Você deseja ter no mínimo 2 domínios de atualização. O valor padrão é 5 e você pode deixá-lo como está para a finalidade desta implantação

![Conjuntos de disponibilidade](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Criar os seguintes conjuntos de disponibilidade

| Conjunto de disponibilidade | Role | Domínios de falhas | Atualizar domínios |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Implantar máquinas virtuais
A próxima etapa é implantar máquinas virtuais que hospedarão as diferentes funções em sua infraestrutura. Um mínimo de duas máquinas é recomendado em cada conjunto de disponibilidade. Crie quatro máquinas virtuais para a implantação básica.

| Machine | Role | Subnet | Conjunto de disponibilidade | Conta de armazenamento | Endereço IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INTEIRO |contosodcset |contososac1 |Estático |
| contosodc2 |DC/ADFS |INTEIRO |contosodcset |contososac2 |Estático |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Estático |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Estático |

Como você deve ter notado, nenhum NSG foi especificado. Isso ocorre porque o Azure permite que você use NSG no nível de sub-rede. Em seguida, você pode controlar o tráfego de rede do computador usando o NSG individual associado à sub-rede ou ao objeto NIC. Leia mais sobre [o que é um NSG (grupo de segurança de rede)](https://aka.ms/Azure/NSG).
O endereço IP estático é recomendado se você estiver gerenciando o DNS. Você pode usar o DNS do Azure e, em vez disso, nos registros DNS para seu domínio, consulte os novos computadores por seus FQDNs do Azure.
O painel da máquina virtual deve ser semelhante ao seguinte depois que a implantação for concluída:

![Máquinas virtuais implantadas](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configurando os servidores do controlador de domínio/AD FS
 Para autenticar qualquer solicitação de entrada, AD FS precisará entrar em contato com o controlador de domínio. Para economizar a viagem dispendiosa do Azure para o DC local para autenticação, é recomendável implantar uma réplica do controlador de domínio no Azure. Para obter alta disponibilidade, é recomendável criar um conjunto de disponibilidade de no mínimo 2 controladores de domínio.

| Controlador de domínio | Role | Conta de armazenamento |
|:---:|:---:|:---:|
| contosodc1 |Réplica |contososac1 |
| contosodc2 |Réplica |contososac2 |

* Promover os dois servidores como controladores de domínio de réplica com DNS
* Configure os servidores de AD FS instalando a função de AD FS usando o Gerenciador de servidores.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Implantando Load Balancer internas (ILB)
**6,1. Criar o ILB**

Para implantar um ILB, selecione balanceadores de carga na portal do Azure e clique em Adicionar (+).

> [!NOTE]
> Se você não vir **balanceadores de carga** no menu, clique em **procurar** no canto inferior esquerdo do portal e role até ver **balanceadores de carga**.  Em seguida, clique na estrela amarela para adicioná-la ao seu menu. Agora, selecione o novo ícone do balanceador de carga para abrir o painel para iniciar a configuração do balanceador de carga.
> 
> 

![Procurar balanceador de carga](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nome**: Forneça um nome adequado para o balanceador de carga
* **Esquema**: Como esse balanceador de carga será colocado na frente dos servidores de AD FS e for destinado somente a conexões de rede internas, selecione "interno"
* **Rede virtual**: Escolha a rede virtual na qual você está implantando o AD FS
* **Sub-rede**: Escolha a sub-rede interna aqui
* **Atribuição de endereço IP**: Estático

![Balanceador de carga interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Depois de clicar em criar e o ILB for implantado, você deverá vê-lo na lista de balanceadores de carga:

![Balanceadores de carga após ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

A próxima etapa é configurar o pool de back-end e a investigação de back-end.

**6,2. Configurar pool de back-end ILB**

Selecione o ILB criado recentemente no painel balanceadores de carga. O painel configurações será aberto. 

1. Selecione pools de back-end no painel configurações
2. No painel Adicionar pool de back-end, clique em Adicionar máquina virtual
3. Você verá um painel em que é possível escolher o conjunto de disponibilidade
4. Escolha o conjunto de disponibilidade de AD FS

![Configurar pool de back-end ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6,3. Configurando investigação**

No painel configurações do ILB, selecione investigações de integridade.

1. Clique em Adicionar
2. Forneça detalhes para a investigação a. **Nome**: Nome da investigação b. **Protocol**: HTTP c. **Port**: 80 (HTTP) d. **Caminho**:/ADFS/Probe e. **Intervalo**: 5 (valor padrão) – esse é o intervalo no qual o ILB investigará as máquinas no pool de back-end f. **Limite de limite não íntegro**: 2 (valor padrão) – esse é o limite de falhas de investigação consecutivas após o qual ILB irá declarar um computador no pool de back-end não responsivo e pare de enviar tráfego para ele.

![Configurar investigação ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Estamos usando o ponto de extremidade/ADFS/Probe que foi criado explicitamente para verificações de integridade em um ambiente AD FS em que uma verificação de caminho HTTPS completa não pode ocorrer.  Isso é substancialmente melhor do que uma verificação básica de porta 443, que não reflete com precisão o status de uma implantação de AD FS moderna.  Mais informações sobre isso podem ser encontradas em https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6,4. Criar regras de balanceamento de carga**

Para balancear efetivamente o tráfego, o ILB deve ser configurado com regras de balanceamento de carga. Para criar uma regra de balanceamento de carga, 

1. Selecione regra de balanceamento de carga no painel configurações do ILB
2. Clique em Adicionar no painel de regras de balanceamento de carga
3. No painel Adicionar regra de balanceamento de carga a. **Nome**: Forneça um nome para a regra b. **Protocol**: Selecione TCP c. **Port**: 443 d. **Porta de back-end**: 443 e. **Pool de back-end**: Selecione o pool que você criou para o cluster de AD FS anterior f. **Investigação**: Selecione a investigação criada para os servidores de AD FS anteriores

![Configurar regras de balanceamento de ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6,5. Atualizar o DNS com ILB**

Vá para o servidor DNS e crie um CNAME para o ILB. O CNAME deve ser para o serviço de Federação com o endereço IP apontando para o endereço IP do ILB. Por exemplo, se o endereço DIP ILB for 10.3.0.8 e o serviço de Federação instalado for fs.contoso.com, crie um CNAME para fs.contoso.com apontando para 10.3.0.8.
Isso garantirá que toda a comunicação em relação ao fs.contoso.com acabe na ILB e seja adequadamente roteada.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configurando o servidor proxy de aplicativo Web
**7,1. Configurando os servidores de proxy de aplicativo Web para alcançar servidores AD FS**

Para garantir que os servidores proxy de aplicativo Web sejam capazes de alcançar os servidores de AD FS por trás do ILB, crie um registro no%SystemRoot%\system32\drivers\etc\hosts para o ILB. Observe que o DN (nome distinto) deve ser o nome do serviço de Federação, por exemplo, fs.contoso.com. E a entrada IP deve ser a do endereço IP do ILB (10.3.0.8 como no exemplo).

**7,2. Instalando a função de proxy de aplicativo Web**

Depois de garantir que os servidores proxy de aplicativo Web sejam capazes de alcançar os servidores de AD FS por trás do ILB, você pode instalar os servidores de proxy de aplicativo Web. Os servidores proxy de aplicativo Web não precisam ingressar no domínio. Instale as funções de proxy de aplicativo Web nos dois servidores proxy de aplicativo Web selecionando a função acesso remoto. O Gerenciador de servidores vai guiá-lo para concluir a instalação do WAP.
Para obter mais informações sobre como implantar o WAP, leia [instalar e configurar o servidor proxy de aplicativo Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  Implantando a Internet (pública) Load Balancer
**8,1.  Criar Load Balancer voltados para a Internet (público)**

No portal do Azure, selecione balanceadores de carga e clique em Adicionar. No painel criar balanceador de carga, insira as informações a seguir

1. **Nome**: Nome para o balanceador de carga
2. **Esquema**: Público – essa opção informa ao Azure que esse balanceador de carga precisará de um endereço público.
3. **Endereço IP**: Criar um novo endereço IP (dinâmico)

![Balanceador de carga voltado para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Após a implantação, o balanceador de carga aparecerá na lista de balanceadores de carga.

![Lista de balanceadores de carga](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8,2. Atribuir um rótulo DNS ao IP público**

Clique na entrada de balanceador de carga recém-criada no painel balanceadores de carga para exibir o painel para configuração. Siga as etapas abaixo para configurar o rótulo DNS para o IP público:

1. Clique no endereço IP público. Isso abrirá o painel para o IP público e suas configurações
2. Clique em configuração
3. Forneça um rótulo DNS. Isso se tornará o rótulo DNS público que você pode acessar de qualquer lugar, por exemplo contosofs.westus.cloudapp.azure.com. Você pode adicionar uma entrada no DNS externo para o serviço de Federação (como fs.contoso.com) que é resolvido para o rótulo DNS do contosofs.westus.cloudapp.azure.com (balanceador de carga externo).

![Configurar balanceador de carga voltado para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurar o balanceador de carga (DNS) voltado para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8,3. Configurar o pool de back-end para Internet (público) Load Balancer** 

Siga as mesmas etapas de criação do balanceador de carga interno para configurar o pool de back-end para Internet (público) Load Balancer como o conjunto de disponibilidade para os servidores WAP. Por exemplo, contosowapset.

![Configurar o pool de back-end de Load Balancer voltados para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8,4. Configurar investigação**

Siga as mesmas etapas de configuração do balanceador de carga interno para configurar a investigação para o pool de back-end de servidores WAP.

![Configurar a investigação de Load Balancer voltadas para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8,5. Criar regra (s) de balanceamento de carga**

Siga as mesmas etapas do ILB para configurar a regra de balanceamento de carga para TCP 443.

![Configurar regras de balanceamento de Load Balancer voltadas para a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Protegendo a rede
**9,1. Protegendo a sub-rede interna**

Em geral, você precisa das seguintes regras para proteger com eficiência sua sub-rede interna (na ordem, conforme listado abaixo)

| Regra | Descrição | Fluxo |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Permitir a comunicação HTTPS do DMZ |Entrada |
| DenyInternetOutbound |Sem acesso à Internet |Saída |

![Regras de acesso INT (entrada)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9,2. Protegendo a sub-rede DMZ**

| Regra | Descrição | Fluxo |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Permitir HTTPS da Internet para a DMZ |Entrada |
| DenyInternetOutbound |Qualquer coisa exceto HTTPS para Internet é bloqueado |Saída |

![Regras de acesso EXT (entrada)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> Se a autenticação de certificado de usuário cliente (autenticação clientTLS usando certificados de usuário X509) for necessária, AD FS exigirá que a porta TCP 49443 seja habilitada para acesso de entrada.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. Testar a entrada AD FS
A maneira mais fácil é testar AD FS é usando a página IdpInitiatedSignon. aspx. Para poder fazer isso, é necessário habilitar o IdpInitiatedSignOn nas propriedades de AD FS. Siga as etapas abaixo para verificar sua configuração de AD FS

1. Execute o cmdlet abaixo no servidor de AD FS, usando o PowerShell, para defini-lo como habilitado.
   Set-Adfsproperties-EnableIdPInitiatedSignonPage $true 
2. Em qualquer computador externo, Acesse https\/:/ADFS-Server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Você deve ver a página AD FS como abaixo:

![Página testar logon](./media/how-to-connect-fed-azure-adfs/test1.png)

Após a entrada bem-sucedida, ele fornecerá uma mensagem de êxito, conforme mostrado abaixo:

![Teste de sucesso](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Modelo para implantação de AD FS no Azure
O modelo implanta uma configuração de 6 computadores, 2 cada para controladores de domínio, AD FS e WAP.

[AD FS no modelo de implantação do Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Você pode usar uma rede virtual existente ou criar uma nova VNET ao implantar este modelo. Os vários parâmetros disponíveis para personalizar a implantação estão listados abaixo com a descrição do uso do parâmetro no processo de implantação. 

| Parâmetro | Descrição |
|:--- |:--- |
| Location |A região na qual os recursos são implantados, por exemplo, leste dos EUA. |
| StorageAccountType |O tipo da conta de armazenamento criada |
| VirtualNetworkUsage |Indica se uma nova rede virtual será criada ou usará uma existente |
| VirtualNetworkName |O nome da rede virtual a ser criada, obrigatória no uso de rede virtual existente ou nova |
| VirtualNetworkResourceGroupName |Especifica o nome do grupo de recursos no qual reside a rede virtual existente. Ao usar uma rede virtual existente, isso se torna um parâmetro obrigatório para que a implantação possa encontrar a ID da rede virtual existente |
| VirtualNetworkAddressRange |O intervalo de endereços da nova VNET, obrigatório se estiver criando uma nova rede virtual |
| InternalSubnetName |O nome da sub-rede interna, obrigatório em ambas as opções de uso de rede virtual (novas ou existentes) |
| InternalSubnetAddressRange |O intervalo de endereços da sub-rede interna, que contém os controladores de domínio e os servidores ADFS, obrigatórios se a criação de uma nova rede virtual for criada. |
| DMZSubnetAddressRange |O intervalo de endereços da sub-rede DMZ, que contém os servidores de proxy de aplicativo do Windows, obrigatórios se a criação de uma nova rede virtual for criada. |
| DMZSubnetName |O nome da sub-rede interna, obrigatório em ambas as opções de uso de rede virtual (novas ou existentes). |
| ADDC01NICIPAddress |O endereço IP interno do primeiro controlador de domínio, esse endereço IP será atribuído estaticamente ao DC e deve ser um endereço IP válido dentro da sub-rede interna |
| ADDC02NICIPAddress |O endereço IP interno do segundo controlador de domínio, esse endereço IP será atribuído estaticamente ao DC e deve ser um endereço IP válido dentro da sub-rede interna |
| ADFS01NICIPAddress |O endereço IP interno do primeiro servidor ADFS, esse endereço IP será atribuído estaticamente ao servidor ADFS e deverá ser um endereço IP válido dentro da sub-rede interna |
| ADFS02NICIPAddress |O endereço IP interno do segundo servidor ADFS, esse endereço IP será atribuído estaticamente ao servidor ADFS e deverá ser um endereço IP válido dentro da sub-rede interna |
| WAP01NICIPAddress |O endereço IP interno do primeiro servidor WAP, esse endereço IP será atribuído estaticamente ao servidor WAP e deve ser um endereço IP válido dentro da sub-rede DMZ |
| WAP02NICIPAddress |O endereço IP interno do segundo servidor WAP, esse endereço IP será atribuído estaticamente ao servidor WAP e deve ser um endereço IP válido dentro da sub-rede DMZ |
| ADFSLoadBalancerPrivateIPAddress |O endereço IP interno do balanceador de carga do ADFS, esse endereço IP será atribuído estaticamente ao balanceador de carga e deve ser um endereço IP válido dentro da sub-rede interna |
| ADDCVMNamePrefix |Prefixo do nome da máquina virtual para controladores de domínio |
| ADFSVMNamePrefix |Prefixo do nome da máquina virtual para servidores ADFS |
| WAPVMNamePrefix |Prefixo do nome da máquina virtual para servidores WAP |
| ADDCVMSize |O tamanho da VM dos controladores de domínio |
| ADFSVMSize |O tamanho da VM dos servidores ADFS |
| WAPVMSize |O tamanho da VM dos servidores WAP |
| AdminUserName |O nome do administrador local das máquinas virtuais |
| adminPassword |A senha da conta de administrador local das máquinas virtuais |

## <a name="additional-resources"></a>Recursos adicionais
* [Conjuntos de disponibilidade](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Load Balancer interno](https://aka.ms/Azure/ILB/Internal)
* [Load Balancer voltadas para a Internet](https://aka.ms/Azure/ILB/Internet)
* [Contas de armazenamento](https://aka.ms/Azure/Storage)
* [Redes virtuais do Azure](https://aka.ms/Azure/VNet)
* [AD FS e links de proxy de aplicativo Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Próximas etapas
* [Integrando suas identidades locais com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configurando e gerenciando seu AD FS usando Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Implantação AD FS de alta disponibilidade entre regiões no Azure com o Gerenciador de tráfego do Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
