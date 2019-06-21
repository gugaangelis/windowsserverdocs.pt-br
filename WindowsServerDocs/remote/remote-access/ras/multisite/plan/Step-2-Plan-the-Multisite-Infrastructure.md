---
title: Etapa 2 de plano a infraestrutura de multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2655f4de83576ef62b113419a69badaacc868f9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281003"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Etapa 2 de plano a infraestrutura de multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A próxima etapa na implantação do acesso remoto em uma topologia de multissite é concluir o planejamento da infraestrutura multissite; inclusive, Active Directory, grupos de segurança e objetos de diretiva de grupo.  
## <a name="bkmk_2_1_AD"></a>2.1 planejar o Active Directory  
Uma implantação multissite do acesso remoto pode ser configurada em várias topologias de:  
  
-   **Único site do Active Directory, vários pontos de entrada**-nessa topologia, você tem um único site do Active Directory para toda a organização com links rápidos de intranet em todo o site, mas se você tiver vários servidores de acesso remoto implantados em toda a organização, cada atuando como um ponto de entrada. Um exemplo dessa topologia geográfico é ter um único site do Active Directory para os Estados Unidos com pontos de entrada na Costa Leste e Costa Oeste.  
  
    ![Infraestrutura de multissite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **Vários sites do Active Directory, vários pontos de entrada**-nessa topologia, você tem dois ou mais sites do Active Directory com um servidor de acesso remoto implantado como um ponto de entrada para cada site. Cada servidor de acesso remoto está associado com o controlador de domínio do Active Directory para o site. Um exemplo dessa topologia geográfico é ter um site do Active Directory para os Estados Unidos e outra para a Europa com um único ponto de entrada para cada site. Observe que, se você tiver vários sites do Active Directory você não precisará ter um ponto de entrada associado a cada site. Além disso, alguns sites do Active Directory podem ter mais de um ponto de entrada associado a ele.  
  
    ![Topologia de multissite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
Em um ponto de entrada multissite, você pode configurar um único servidor de acesso remoto, vários servidores de acesso remoto, ou clusters de servidor de acesso remoto.   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Recomendações e práticas recomendadas do active Directory  
Observe as seguintes recomendações e restrições para a implantação do Active Directory em um cenário multissite:  
  
1.  Cada site do Active Directory pode conter um ou mais servidores de acesso remoto, ou um cluster de servidor, funcionando como pontos de entrada multissite para computadores cliente. No entanto, um site do Active Directory não é necessário ter um ponto de entrada.  
  
2.  Um ponto de entrada multissite só pode ser associado com um único site do Active Directory. Quando os computadores cliente executando o Windows 8 se conectar a um ponto de entrada específico, eles são considerados como pertencentes ao site do Active Directory associado ao ponto de entrada.  
  
3.  É recomendável que cada site do Active Directory tem um controlador de domínio. O controlador de domínio pode ser somente leitura.  
  
4.  Se cada site do Active Directory contém um controlador de domínio, o GPO de um servidor no ponto de entrada é gerenciada por um dos controladores de domínio no site do Active Directory associado com o ponto de extremidade. Se não houver nenhum controlador de domínio habilitado para gravação no site, o GPO para um servidor é gerenciado em um controlador de domínio habilitado para gravação que é encontrado mais próximo para o primeiro servidor de acesso remoto está configurado no ponto de entrada. Mais próximo é determinado por um link de cálculo de custo. Observe que, nesse cenário, depois de fazer alterações de configuração, pode haver um atraso ao replicar entre o controlador de domínio, gerenciar GPO e o controlador de domínio somente leitura no site do Active Directory do servidor.  
  
5.  GPOs de cliente e servidor de aplicativo opcional que GPOs são gerenciados no controlador de domínio em execução como o emulador do controlador de domínio primário (PDC). Isso significa que o cliente GPOs não são necessariamente gerenciados no site do Active Directory que contém o ponto de entrada no qual os clientes se conectar.  
  
6.  Se o controlador de domínio para um site do Active Directory está inacessível, o servidor de acesso remoto se conectar a um controlador de domínio alternativo no site (se disponível). Caso contrário, ele se conecta ao controlador de domínio para outro site para recuperar um GPO atualizado e para autenticar clientes. Em ambos os casos, o console de gerenciamento de acesso remoto e os cmdlets do PowerShell não pode ser usado para recuperar ou modificar as definições de configuração até que o controlador de domínio esteja disponível. Observe o seguinte:  
  
    1.  Se o servidor que executa como o emulador do PDC não estiver disponível, você deve designar um controlador de domínio disponível que atualizou GPOs como o emulador do PDC.  
  
    2.  Se o controlador de domínio que gerencia um GPO de servidor estiver disponível, use o cmdlet do PowerShell Set-DAEntryPointDC para associar um novo controlador de domínio com o ponto de entrada. O novo controlador de domínio deve ter GPOs atualizados antes de executar o cmdlet.  
  
## <a name="bkmk_2_2_SG"></a>2.2 planejar grupos de segurança  
Durante a implantação de um único servidor com configurações avançadas, todos os computadores cliente acessem a rede interna via DirectAccess foram reunidos em um grupo de segurança. Em uma implantação multissite, esse grupo de segurança é usado para apenas computadores com cliente Windows 8. Para uma implantação multissite, os computadores cliente Windows 7 serão reunidos em grupos de segurança separados para cada ponto de entrada na implantação multissite. Por exemplo, se você agrupou anteriormente todos os computadores cliente no grupo clientes_da, você agora deve remover todos os computadores Windows 7 do grupo e colocá-los em um grupo de segurança diferentes. Por exemplo, em vários sites do Active Directory, topologia de pontos de entrada de vários, você cria um grupo de segurança para o ponto de entrada dos Estados Unidos (DA_Clients_US) e outro para o ponto de entrada da Europa (DA_Clients_Europe). Coloque nenhum localizados no United States no grupo DA_Clients_US e qualquer localizado na Europa no grupo DA_Clients_Europe dos computadores cliente Windows 7. Se você não tiver nenhum dos computadores cliente Windows 7, você não precisará planejar grupos de segurança para computadores com Windows 7.  
  
Grupos de segurança necessários são da seguinte maneira:  
  
-   Um grupo de segurança para todos os computadores de cliente do Windows 8. É recomendável criar um grupo de segurança exclusivos para esses clientes para cada domínio.  
  
-   Um grupo de segurança exclusivo que contém os computadores de cliente do Windows 7 para cada ponto de entrada. É recomendável criar um grupo exclusivo para cada domínio. Por exemplo:  Domain1\DA_Clients_Europe; Domain2\DA_Clients_Europe; Domain1\DA_Clients_US; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2.3 objetos de diretiva de grupo do plano  
Configurações do DirectAccess configuradas durante a implantação do acesso remoto são coletadas nos GPOs. Sua implantação de servidor único já usa GPOs para clientes do DirectAccess, o servidor de acesso remoto e, opcionalmente, para servidores de aplicativos. Uma implantação multissite requer os seguintes GPOs:  
  
-   Um GPO de servidor para cada ponto de entrada.  
  
-   Um GPO para cada domínio que contém computadores cliente que executam o Windows 8.  
  
-   Um GPO para cada ponto de entrada e de cada domínio que contém computadores cliente do Windows 7.  
  
GPOs podem ser configurados da seguinte maneira:  
  
-   **Automaticamente**-você pode especificar que os GPOs são criados automaticamente pelo acesso remoto. Um nome padrão é especificado para cada GPO e pode ser modificado.  
  
-   **Manualmente**-você pode usar GPOs criados manualmente pelo administrador do Active Directory.  
  
> [!NOTE]  
> Depois que o DirectAccess é configurado para usar GPOs específicos, ele não pode ser configurado para usar GPOs diferentes.  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 GPOs criados automaticamente  
Observe o seguinte quando usar GPOs criados automaticamente:  
  
-   GPOs criados automaticamente são aplicados de acordo com o local e o parâmetro de destino do link, da seguinte maneira:  
  
    -   GPO de servidor, os parâmetros de local e de link apontam para o domínio que contém o servidor de acesso remoto.  
  
    -   Para GPOs de cliente, o destino do link é definido como a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente e o GPO será vinculado à raiz de cada domínio. .  
  
-   Para GPOs criados automaticamente, para aplicar as configurações do DirectAccess o acesso remoto, administrador de servidor requer as seguintes permissões:  
  
    -   Permissões para domínios necessários de criação de GPO.  
  
    -   Permissões de links para todas as raízes de domínio de cliente selecionadas.  
  
    -   Permissão para criar o filtro WMI para GPOs é necessária se o DirectAccess foi configurado para funcionar somente em computadores portáteis.  
  
    -   Permissões de links para as raízes de domínios associados com os pontos de entrada (os domínios de GPO de servidor)  
  
    -   É recomendado que o administrador do Acesso Remoto possua permissões de leitura do GPO para cada domínio necessário. Isso permite o acesso remoto verificar se os GPOs com nomes duplicados não existe durante a criação de GPOs para a implantação multissite.  
  
    -   Permissões de replicação do Active Directory em domínios associados com pontos de entrada. Isso ocorre porque quando inicialmente adicionando pontos de entrada, o GPO do servidor para o ponto de entrada é criado no controlador de domínio mais próximo ao ponto de entrada. No entanto, uma vez que a criação do link é compatível com o emulador PDC somente, o GPO deve ser replicado do controlador de domínio no qual ele foi criado para o controlador de domínio em execução como o emulador do PDC antes de criar o link.  
  
Observe que, se as permissões corretas para replicação e vincular GPOs não existirem, um aviso será emitido. A operação de acesso remoto continuará, mas a replicação e a vinculação não ocorrerá. Se um link de aviso é emitido links não serão criados automaticamente, mesmo depois que as permissões estão em vigor. Em vez disso, o administrador precisará criar os links manualmente.  
  
### <a name="232-manually-created-gpos"></a>2.3.2 GPOs criados manualmente  
Observe o seguinte quando usar GPOs criados manualmente:  
  
-   Os seguintes GPOs devem ser criados manualmente para uma implantação multissite:  
  
    -   **GPO do servidor**- um GPO de servidor para cada ponto de entrada (no domínio em que o ponto de entrada está localizado). Esse GPO será aplicado em cada servidor de acesso remoto no ponto de entrada.  
  
    -   **(Windows 7) do GPO do cliente**- um GPO para cada ponto de entrada e cada domínio que contém computadores cliente Windows 7 que irão se conectar a pontos de entrada na implantação multissite. Por exemplo, Domain1\DA_W7_Clients_GPO_Europe; Domain2\DA_W7_Clients_GPO_Europe; Domain1\DA_W7_Clients_GPO_US; Domain2\DA_W7_Clients_GPO_US. Se não há computadores cliente com Windows 7 irá se conectar a pontos de entrada, os GPOs não são necessários.  
  
-   Não há nenhum requisito para criar computadores de cliente adicionais de GPOs para o Windows 8. Um GPO para cada domínio que contêm computadores cliente já foi criado quando o servidor de acesso remoto único foi implantado. Esses GPOs de cliente funcionará como os clientes de GPOs para o Windows 8 em uma implantação multissite.  
  
-   Os GPOs devem existir antes de clicar no botão de confirmação nos assistentes de implantação multissite.  
  
-   Ao usar GPOs criados manualmente, uma pesquisa é feita para um link para o GPO no domínio inteiro. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.  
  
-   Quando usar GPOs criados manualmente, para aplicar as configurações do DirectAccess o administrador do servidor de acesso remoto requer permissões completas de GPO (Editar, excluir, modificar segurança) nos GPOs criados manualmente.  
  
Observe que, se as permissões corretas para replicação e vincular GPOs não existirem, um aviso será emitido. A operação de acesso remoto continuará, mas a replicação e a vinculação não ocorrerá. Se um link de aviso é emitido links não serão criados automaticamente, mesmo depois que as permissões estão em vigor. Em vez disso, o administrador precisará criar os links manualmente.  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 gerenciar GPOs em um ambiente de controlador de múltiplos domínios  
Cada GPO será gerenciado por um controlador de domínio específico, da seguinte maneira:  
  
-   O GPO do servidor é gerenciado por um dos controladores de domínio no site do Active Directory associado ao servidor, ou se os controladores de domínio nesse site são somente leitura, por um controlador de domínio habilitado para gravação mais próximo para o primeiro servidor na entrada de ponto.  
  
-   GPOs de cliente serão gerenciados pelo controlador de domínio em execução como o emulador do PDC.  
  
Se você quiser manualmente modificar Observação de configurações de GPO o seguinte:  
  
-   Para GPOs, do servidor para identificar qual controlador de domínio está associado com um ponto de entrada específico, execute o cmdlet do PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>`.  
  
-   Por padrão, quando você fizer alterações com cmdlets do PowerShell de rede ou o console de gerenciamento de diretiva de grupo, o controlador de domínio atuando como o emulador PDC é usado.  
  
-   Se você modificar as configurações em um controlador de domínio que não é o controlador de domínio associado com o ponto de entrada (para GPOs do servidor) ou o emulador PDC (para GPOs de cliente), observe o seguinte:  
  
    1.  Antes de modificar as configurações, certifique-se de que o controlador de domínio seja replicado com um GPO atualizado, e [fazer backup de configurações de GPO](https://go.microsoft.com/fwlink/?LinkID=257928), antes de fazer alterações. Se o GPO não for atualizado, conflitos de mesclagem durante a replicação podem ocorrer, resultando em uma configuração corrompida do acesso remoto.  
  
    2.  Depois de modificar as configurações, você deve aguardar as alterações replicar para o controlador de domínio que está associado com os GPOs. Não faça alterações adicionais usando o console de gerenciamento de acesso remoto ou cmdlets do PowerShell de acesso remoto até que a replicação seja concluída. Se um GPO for editado em dois controladores de domínio diferente antes de replicação for concluída, mesclagem poderão ocorrer conflitos, resultando em uma configuração corrompida  
  
-   Como alternativa, você pode alterar a configuração padrão usando o **Alterar controlador de domínio** diálogo caixa no console de gerenciamento de diretiva de grupo, ou usando o **Open-NetGPO** cmdlet do PowerShell, para que as alterações feita usando o console ou os cmdlets de rede para usar o controlador de domínio que você especificar.  
  
    1.  Para fazer isso no console de gerenciamento de diretiva de grupo, o contêiner de sites ou de domínio com o botão direito e clique em **Alterar controlador de domínio**.  
  
    2.  Para fazer isso no PowerShell, especifique o parâmetro para o cmdlet Open-NetGPO DomainController. Por exemplo, para habilitar os perfis públicos e privados no Firewall do Windows em um GPO chamado Europe domain1\DA_Server_GPO usando um controlador de domínio chamado Europe-DC.corp.contoso.com, faça o seguinte:  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>Modificando associação do controlador de domínio  
Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Em alguns cenários, pode ser necessário atribuir outro controlador de domínio para um GPO:  
  
-   **Manutenção do controlador de domínio e o tempo de inatividade**-quando o controlador de domínio que gerencia um GPO não estiver disponível, pode ser necessário para gerenciar o GPO em um controlador de domínio diferente.  
  
-   **Otimização de distribuição de configuração**-depois de alterações de infraestrutura de rede, pode ser necessário para gerenciar o GPO do servidor de um ponto de entrada em um controlador de domínio no mesmo site do Active Directory que o ponto de entrada.   
  
## <a name="bkmk_2_4_DNS"></a>2.4 planejar DNS  
Ao planejar DNS para uma implantação multissite, observe o seguinte:  
  
1.  Computadores cliente usam o endereço ConnectTo para se conectar ao servidor de acesso remoto. Cada ponto de entrada em sua implantação requer um endereço ConnectTo diferente. Cada endereço ConnectTo do ponto de entrada deve estar disponível no DNS público e o endereço que você escolher deve corresponder ao nome de assunto do certificado IP-HTTPS implantado para a conexão IP-HTTPS.   
  
2.  Além disso, o acesso remoto impõe o roteamento simétrico; Portanto, somente os clientes Teredo e IP-HTTPS podem se conectar quando uma implantação multissite está habilitada. Para permitir que clientes IPv6 nativos para se conectar, o endereço ConnectTo (a URL IP-HTTPS) deve resolver os dois os externo (para a Internet) endereços IPv6 e IPv4 no servidor de acesso remoto.  
  
3.  O Acesso Remoto cria uma sonda da Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Durante a configuração do servidor único os seguintes nomes devem foi registrados no DNS:  
  
    1.  DirectAccess-WebProbeHost deve resolver para o endereço IPv4 interno do servidor de acesso remoto, ou o endereço IPv6 em um ambiente somente IPv6.  
  
    2.  DirectAccess-CorpConnectivityHost deve resolver para um endereço localhost (loopback) (tanto:: 1 ou 127.0.0.1, dependendo se o IPv6 foi implantado na rede corporativa).  
  
    Em uma implantação multissite, uma entrada DNS adicional para o directaccess-WebProbeHost deve ser criada para cada ponto de entrada. Ao adicionar um ponto de entrada, o acesso remoto tenta criar automaticamente essa entrada do directaccess-WebProbeHost adicionais. No entanto, se ele falhar um aviso será exibido e você deve criar manualmente a entrada.  
  
    > [!NOTE]  
    > Quando a eliminação de DNS é habilitada na sua infraestrutura DNS, é recomendável desabilitar a limpeza nas entradas do DNS criadas automaticamente pelo acesso remoto.  
  

  



