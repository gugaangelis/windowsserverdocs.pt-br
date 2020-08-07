---
title: Etapa 2 planejar a infraestrutura multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e411db1eef92b26a2d44051123361a5ac674bb3c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937077"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Etapa 2 planejar a infraestrutura multissite

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A próxima etapa na implantação do acesso remoto em uma topologia multissite é concluir o planejamento da infraestrutura multissite; incluindo, Active Directory, grupos de segurança e objetos Política de Grupo.
## <a name="21-plan-active-directory"></a><a name="bkmk_2_1_AD"></a>Active Directory de plano de 2,1
Uma implantação multissite de acesso remoto pode ser configurada em várias topologias:

-   **Site de Active Directory único, vários pontos de entrada**-nessa topologia, você tem um único site de Active Directory para toda a sua organização com links rápidos de intranet em todo o site, mas você tem vários servidores de acesso remoto implantados em toda a sua organização, cada um agindo como um ponto de entrada. Um exemplo geográfico dessa topologia é ter um único site de Active Directory para o Estados Unidos com pontos de entrada na costa leste e na costa oeste.

    ![Infraestrutura multissite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)

-   **Vários sites Active Directory, vários pontos de entrada**-nessa topologia, você tem dois ou mais Active Directory sites com um servidor de acesso remoto implantado como um ponto de entrada para cada site. Cada servidor de acesso remoto é associado ao controlador de domínio Active Directory para o site. Um exemplo geográfico dessa topologia é ter um Active Directory site para o Estados Unidos e outro para a Europa com um único ponto de entrada para cada site. Observe que, se você tiver vários sites Active Directory, não precisará ter um ponto de entrada associado a cada site. Além disso, alguns Active Directory sites podem ter mais de um ponto de entrada associado a ele.

    ![Topologia multissite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)

Em um ponto de entrada multissite, você pode configurar um único servidor de acesso remoto, vários servidores de acesso remoto ou um cluster de servidores de acesso remoto.

### <a name="active-directory-best-practices-and-recommendations"></a>Práticas recomendadas e recomendações do Active Directory
Observe as recomendações e restrições a seguir para Active Directory implantação em um cenário multissite:

1.  Cada site Active Directory pode conter um ou mais servidores de acesso remoto, ou um cluster de servidores, funcionando como pontos de entrada multissite para computadores cliente. No entanto, um site de Active Directory não precisa ter um ponto de entrada.

2.  Um ponto de entrada multissite só pode ser associado a um único site Active Directory. Quando os computadores cliente que executam o Windows 8 se conectam a um ponto de entrada específico, eles são considerados como pertencentes ao site de Active Directory associado a esse ponto de entrada.

3.  É recomendável que cada site de Active Directory tenha um controlador de domínio. O controlador de domínio pode ser somente leitura.

4.  Se cada site de Active Directory contiver um controlador de domínio, o GPO para um servidor no ponto de entrada será gerenciado por um dos controladores de domínio no site Active Directory associado ao ponto de extremidade. Se não houver nenhum controlador de domínio habilitado para gravação nesse site, o GPO para um servidor será gerenciado em um controlador de domínio habilitado para gravação que é encontrado mais próximo ao primeiro servidor de acesso remoto configurado no ponto de entrada. O mais próximo é determinado por um cálculo de custo de link. Observe que, nesse cenário, depois de fazer alterações de configuração, pode haver um atraso ao replicar entre o controlador de domínio que gerencia o GPO e o controlador de domínio somente leitura no site de Active Directory do servidor.

5.  Os GPOs de cliente e os GPOs de servidor de aplicativos opcionais são gerenciados no controlador de domínio em execução como emulador de controlador de domínio primário (PDC). Isso significa que os GPOs de cliente não são necessariamente gerenciados no site de Active Directory que contém o ponto de entrada ao qual os clientes se conectam.

6.  Se o controlador de domínio de um site Active Directory estiver inacessível, o servidor de acesso remoto se conectará a um controlador de domínio alternativo no site (se disponível). Caso contrário, ele se conecta ao controlador de domínio para outro site para recuperar um GPO atualizado e para autenticar clientes. Em ambos os casos, o console de gerenciamento de acesso remoto e os cmdlets do PowerShell não podem ser usados para recuperar ou modificar as definições de configuração até que o controlador de domínio esteja disponível. Observe o seguinte:

    1.  Se o servidor em execução como emulador de PDC não estiver disponível, você deverá designar um controlador de domínio disponível que tenha GPOs atualizados como o emulador de PDC.

    2.  Se o controlador de domínio que gerencia um GPO de servidor não estiver disponível, use o cmdlet Set-DAEntryPointDC do PowerShell para associar um novo controlador de domínio ao ponto de entrada. O novo controlador de domínio deve ter GPOs atualizados antes de executar o cmdlet.

## <a name="22-plan-security-groups"></a><a name="bkmk_2_2_SG"></a>grupos de segurança do plano 2,2
Durante a implantação de um único servidor com configurações avançadas, todos os computadores cliente que acessam a rede interna por meio do DirectAccess foram coletados em um grupo de segurança. Em uma implantação multissite, esse grupo de segurança é usado somente para computadores cliente com Windows 8. Para uma implantação multissite, os computadores cliente do Windows 7 serão coletados em grupos de segurança separados para cada ponto de entrada na implantação multissite. Por exemplo, se você já tiver agrupado todos os computadores cliente no grupo DA_Clients, agora deverá remover todos os computadores com Windows 7 desse grupo e colocá-los em um grupo de segurança diferente. Por exemplo, na topologia vários sites Active Directory, vários pontos de entrada, você cria um grupo de segurança para o ponto de entrada Estados Unidos (DA_Clients_US) e outro para o ponto de entrada da Europa (DA_Clients_Europe). Coloque todos os computadores cliente do Windows 7 localizados na Estados Unidos no grupo de DA_Clients_US e quaisquer localizados na Europa no grupo de DA_Clients_Europe. Se você não tiver computadores cliente com o Windows 7, não será necessário planejar grupos de segurança para computadores com Windows 7.

Os grupos de segurança necessários são os seguintes:

-   Um grupo de segurança para todos os computadores cliente do Windows 8. É recomendável criar um grupo de segurança exclusivo para esses clientes para cada domínio.

-   Um grupo de segurança exclusivo que contém computadores cliente do Windows 7 para cada ponto de entrada. É recomendável criar um grupo exclusivo para cada domínio. Por exemplo: domain1 \ DA_Clients_Europe; Domain2 \ DA_Clients_Europe; Domain1 \ DA_Clients_US; Domain2 \ DA_Clients_US.

## <a name="23-plan-group-policy-objects"></a><a name="bkmk_2_3_GPO"></a>2,3 planejar Política de Grupo objetos
As configurações do DirectAccess definidas durante a implantação de acesso remoto são coletadas em GPOs. Sua implantação de servidor único já usa GPOs para clientes DirectAccess, o servidor de acesso remoto e, opcionalmente, para servidores de aplicativos. Uma implantação multissite requer os seguintes GPOs:

-   Um GPO de servidor para cada ponto de entrada.

-   Um GPO para cada domínio que contém computadores cliente que executam o Windows 8.

-   Um GPO para cada ponto de entrada e cada domínio que contém computadores cliente com o Windows 7.

Os GPOs podem ser configurados da seguinte maneira:

-   **Automaticamente**-você pode especificar que os GPOs sejam criados automaticamente pelo acesso remoto. Um nome padrão é especificado para cada GPO e pode ser modificado.

-   **Manualmente**-você pode usar GPOs que foram criados manualmente pelo administrador do Active Directory.

> [!NOTE]
> Depois que o DirectAccess estiver configurado para usar GPOs específicos, ele não poderá ser configurado para usar GPOs diferentes.

### <a name="231-automatically-created-gpos"></a>2.3.1 GPOs criados automaticamente
Observe o seguinte quando usar GPOs criados automaticamente:

-   GPOs criados automaticamente são aplicados de acordo com o local e o parâmetro de destino do link, da seguinte maneira:

    -   Para o GPO do servidor, os parâmetros local e link apontam para o domínio que contém o servidor de acesso remoto.

    -   Para GPOs de cliente, o destino do link é definido como a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente e o GPO será vinculado à raiz de cada domínio. .

-   Para GPOs criados automaticamente, para aplicar as configurações do DirectAccess, o administrador do servidor de acesso remoto requer as seguintes permissões:

    -   Permissões de criação de GPO para domínios necessários.

    -   Permissões de links para todas as raízes de domínio de cliente selecionadas.

    -   A permissão para criar o filtro WMI para GPOs será necessária se o DirectAccess tiver sido configurado para funcionar somente em computadores móveis.

    -   Permissões de link para as raízes de domínios associados aos pontos de entrada (os domínios de GPO do servidor)

    -   É recomendado que o administrador do Acesso Remoto possua permissões de leitura do GPO para cada domínio necessário. Isso permite que o acesso remoto Verifique se os GPOs com nomes duplicados não existem ao criar GPOs para a implantação multissite.

    -   Active Directory permissões de replicação em domínios associados a pontos de entrada. Isso ocorre porque, ao adicionar inicialmente pontos de entrada, o GPO do servidor para o ponto de entrada é criado no controlador de domínio mais próximo desse ponto de entrada. No entanto, como a criação de link tem suporte apenas no emulador de PDC, o GPO deve ser replicado do controlador de domínio no qual ele foi criado para o controlador de domínio em execução como emulador PDC antes de criar o link.

Observe que, se as permissões corretas para replicação e vinculação de GPOs não existirem, um aviso será emitido. A operação de acesso remoto continuará, mas a replicação e a vinculação não ocorrerão. Se um aviso de link for emitido, os links não serão criados automaticamente, mesmo depois que as permissões estiverem em vigor. Em vez disso, o administrador precisará criar os links manualmente.

### <a name="232-manually-created-gpos"></a>2.3.2 GPOs criados manualmente
Observe o seguinte quando usar GPOs criados manualmente:

-   Os GPOs a seguir devem ser criados manualmente para a implantação multissite:

    -   **GPO de servidor**-um GPO de servidor para cada ponto de entrada (no domínio no qual o ponto de entrada está localizado). Esse GPO será aplicado em cada servidor de acesso remoto no ponto de entrada.

    -   **GPO do cliente (Windows 7)**– um GPO para cada ponto de entrada e cada domínio que contém computadores cliente do Windows 7 que se conectarão a pontos de entrada na implantação multissite. Por exemplo, domain1 \ DA_W7_Clients_GPO_Europe; Domain2 \ DA_W7_Clients_GPO_Europe; Domain1 \ DA_W7_Clients_GPO_US; Domain2 \ DA_W7_Clients_GPO_US. Se nenhum computador cliente do Windows 7 se conectar a pontos de entrada, os GPOs não serão necessários.

-   Não há nenhum requisito para criar GPOs adicionais para computadores cliente do Windows 8. Um GPO para cada domínio que contém computadores cliente já foi criado quando o único servidor de acesso remoto foi implantado. Em uma implantação multissite, esses GPOs de cliente funcionarão como os GPOs para clientes do Windows 8.

-   Os GPOs devem existir antes de clicar no botão confirmar nos assistentes de implantação multissite.

-   Ao usar GPOs criados manualmente, uma pesquisa é feita para um link para o GPO no domínio inteiro. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.

-   Ao usar GPOs criados manualmente, para aplicar configurações do DirectAccess, o administrador do servidor de acesso remoto requer permissões de GPO completas (editar, excluir, modificar a segurança) nos GPOs criados manualmente.

Observe que, se as permissões corretas para replicação e vinculação de GPOs não existirem, um aviso será emitido. A operação de acesso remoto continuará, mas a replicação e a vinculação não ocorrerão. Se um aviso de link for emitido, os links não serão criados automaticamente, mesmo depois que as permissões estiverem em vigor. Em vez disso, o administrador precisará criar os links manualmente.

### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 Gerenciando GPOs em um ambiente de controlador de vários domínios
Cada GPO será gerenciado por um controlador de domínio específico, da seguinte maneira:

-   O GPO do servidor é gerenciado por um dos controladores de domínio no site Active Directory associado ao servidor, ou se os controladores de domínio nesse site são somente leitura, por um controlador de domínio habilitado para gravação mais próximo do primeiro servidor no ponto de entrada.

-   Os GPOs do cliente serão gerenciados pelo controlador de domínio em execução como emulador de PDC.

Se você quiser modificar manualmente as configurações do GPO, observe o seguinte:

-   Para GPOs de servidor, para identificar qual controlador de domínio está associado a um ponto de entrada específico, execute o cmdlet do PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>` .

-   Por padrão, quando você faz alterações com os cmdlets do PowerShell de rede ou o console de gerenciamento do Política de Grupo, o controlador de domínio agindo como o emulador de PDC é usado.

-   Se você modificar as configurações em um controlador de domínio que não seja o controlador de domínio associado ao ponto de entrada (para GPOs de servidor) ou ao emulador de PDC (para GPOs de cliente), observe o seguinte:

    1.  Antes de modificar as configurações, verifique se o controlador de domínio é replicado com um GPO atualizado e [faça backup das configurações de GPO](https://go.microsoft.com/fwlink/?LinkID=257928)antes de fazer alterações. Se o GPO não for atualizado, poderão ocorrer conflitos de mesclagem durante a replicação, resultando em uma configuração de acesso remoto corrompida.

    2.  Depois de modificar as configurações, você deve aguardar até que as alterações sejam replicadas no controlador de domínio associado aos GPOs. Não faça alterações adicionais usando o console de gerenciamento de acesso remoto ou os cmdlets do PowerShell de acesso remoto até que a replicação seja concluída. Se um GPO for editado em dois controladores de domínio diferentes antes da conclusão da replicação, poderão ocorrer conflitos de mesclagem, resultando em uma configuração corrompida

-   Como alternativa, você pode alterar a configuração padrão usando a caixa de diálogo **alterar controlador de domínio** no console de gerenciamento do política de grupo ou usando o cmdlet do PowerShell **Open-NetGPO** , para que as alterações feitas usando o console ou os cmdlets de rede usem o controlador de domínio que você especificar.

    1.  Para fazer isso no console de gerenciamento de Política de Grupo, clique com o botão direito do mouse no contêiner de domínio ou sites e clique em **alterar controlador de domínio**.

    2.  Para fazer isso no PowerShell, especifique o parâmetro DomainController para o cmdlet Open-NetGPO. Por exemplo, para habilitar os perfis privado e público no firewall do Windows em um GPO denominado domain1 \ DA_Server_GPO _Europe usando um controlador de domínio chamado europe-dc.corp.contoso.com, faça o seguinte:

        ```
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True
        Save-NetGPO -GpoSession $gpoSession
        ```

#### <a name="modifying-domain-controller-association"></a>Modificando a associação do controlador de domínio
Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Em alguns cenários, pode ser necessário atribuir um controlador de domínio diferente para um GPO:

-   **Manutenção e tempo de inatividade do controlador de domínio**– quando o controlador de domínio que gerencia um GPO não está disponível, pode ser necessário gerenciar o GPO em um controlador de domínio diferente.

-   **Otimização da distribuição de configuração**– após a alteração da infraestrutura de rede, pode ser necessário gerenciar o GPO do servidor de um ponto de entrada em um controlador de domínio no mesmo site Active Directory que o ponto de entrada.

## <a name="24-plan-dns"></a><a name="bkmk_2_4_DNS"></a>DNS do plano 2,4
Observe o seguinte ao planejar o DNS para uma implantação multissite:

1.  Os computadores cliente usam o endereço connectto para se conectar ao servidor de acesso remoto. Cada ponto de entrada em sua implantação requer um endereço connectto diferente. Cada endereço de conexão de ponto de entrada deve estar disponível no DNS público e o endereço que você escolher deve corresponder ao nome da entidade do certificado IP-HTTPS implantado para a conexão IP-HTTPS.

2.  Além disso, o acesso remoto impõe o roteamento simétrico; Portanto, somente clientes Teredo e IP-HTTPS podem se conectar quando uma implantação multissite está habilitada. Para permitir que clientes IPv6 nativos se conectem, o endereço connectto (a URL de IP-HTTPS) deve ser resolvido para os endereços IPv6 e IPv4 externos (voltados para a Internet) no servidor de acesso remoto.

3.  O Acesso Remoto cria uma sonda da Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Durante a configuração do servidor único, os seguintes nomes devem ter sido registrados no DNS:

    1.  DirectAccess-WebProbeHost-devem ser resolvidos para o endereço IPv4 interno do servidor de acesso remoto ou para o endereço IPv6 em um ambiente somente IPv6.

    2.  DirectAccess-CorpConnectivityHost-deve resolver para um endereço de localhost (loopback) (ou:: 1 ou 127.0.0.1, dependendo se o IPv6 está implantado na rede corporativa).

    Em uma implantação multissite, uma entrada DNS adicional para DirectAccess-WebProbeHost deve ser criada para cada ponto de entrada. Ao adicionar um ponto de entrada, o acesso remoto tenta criar automaticamente essa entrada adicional do DirectAccess-WebProbeHost. No entanto, se falhar, um aviso será mostrado e você deverá criar a entrada manualmente.

    > [!NOTE]
    > Quando a limpeza de DNS está habilitada em sua infraestrutura de DNS, é recomendável desabilitar a eliminação nas entradas de DNS criadas automaticamente pelo acesso remoto.






