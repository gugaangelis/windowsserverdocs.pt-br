---
title: Etapa 1 configurar a infraestrutura do DirectAccess
description: Este tópico faz parte do guia adicionar DirectAccess a uma implantação de acesso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5dc529f7-7bc3-48dd-b83d-92a09e4055c4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: acdfdcc44a4166d23246098d4857a851cd2fa31e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836957"
---
# <a name="step-1-configure-the-directaccess-infrastructure"></a>Etapa 1 configurar a infraestrutura do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como configurar a infraestrutura requerida para habilitar o DirectAccess em uma implantação da VPN existente. Antes de começar as etapas de implantação, certifique-se de que você tenha concluído as etapas de planejamento descritas em [etapa 1: Planejar a infraestrutura do DirectAccess](Step-1-Plan-DirectAccess-Infrastructure.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|Definir configurações de rede do servidor|Definir as configurações de rede do servidor no servidor de Acesso Remoto.|  
|Configurar o roteamento da rede corporativa|Configurar o roteamento da rede corporativa para verificar se o tráfego está devidamente roteado.|  
|Configurar firewalls|Configurar firewalls adicionais, se necessário.|  
|Configurar autoridades de certificação (CAs) e certificados|O Assistente para Habilitar o DirectAccess configura um proxy Kerberos interno que permite autenticação usando nomes de usuário e senhas. Ele também configura um certificado IP-HTTPS no servidor de Acesso Remoto.|  
|Configurar o servidor DNS|Definir as configurações de DNS para o servidor de Acesso Remoto.|  
|Configurar o Active Directory|Ingressar os computadores cliente ao domínio do Active Directory.|  
|Configurar GPOs|Configurar os GPOs para implantação, se necessário.|  
|Configurar os grupos de segurança|Configurar os grupos de segurança que conterão os computadores cliente do DirectAccess e quaisquer outros grupos de segurança requeridos na implantação.|  
|Configurar o servidor de local de rede|O Assistente para Habilitar o DirectAccess configura o servidor de local da rede no servidor DirectAccess.|  
  
## <a name="ConfigNetworkSettings"></a>Definir configurações de rede do servidor  
As configurações de interface de rede a seguir são requeridas para uma implantação de servidor único em um ambiente com IPv4 e IPv6. Todos os endereços IP são configurados usando **Alterar configurações do adaptador** na **Central de Rede e Compartilhamento do Windows**.  
  
-   Topologia de borda  
  
    -   Um endereço público estático IPv4 ou IPv6 voltado para a Internet.  
  
    -   Um único endereço estático interno IPv4 ou IPv6.  
  
-   Atrás do dispositivo NAT (dois adaptadores de rede)  
  
    -   Um único endereço estático interno IPv4 ou IPv6 voltado para a rede.  
  
-   Atrás do dispositivo NAT (um adaptador de rede)  
  
    -   Um único endereço estático IPv4 ou IPv6.  
  
> [!NOTE]  
> Se o servidor de Acesso Remoto tiver dois adaptadores de rede (um classificado no perfil do domínio e outro em um perfil público/privado), porém uma topologia de NIC único for usada, veja as recomendações a seguir:  
>   
> 1.  Certifique-se que o 2º NIC também está classificado no perfil do domínio - Recomendado.  
> 2.  Se o 2º NIC não puder ser configurado para o perfil do domínio por alguma razão, a política de IPsec do DirectAccess deve ser manualmente estendida a todos os perfis usando os seguintes comandos do Windows PowerShell:  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
  
## <a name="ConfigRouting"></a>Configurar o roteamento na rede corporativa  
Configure o roteamento na rede corporativa da seguinte forma:  
  
-   Quando o IPv6 nativo for implantado na organização, adicione uma rota para que os roteadores no tráfego IPv6 da rota de rede interna retorne pelo servidor de Acesso Remoto.  
  
-   Configure manualmente as rotas IPv4 e IPv6 da organização nos servidores de Acesso Remoto. Adicione uma rota publicada para que todo o tráfego com um prefixo IPv6 (/48) da organização seja encaminhado à rede interna. Além disso, para tráfego IPv4, adicione rotas explícitas para que o tráfego IPv4 seja encaminhado para a rede interna.  
  
## <a name="ConfigFirewalls"></a>Configurar firewalls  
Ao usar firewalls adicionais na sua implantação, aplique as seguintes exceções de firewall voltado para a Internet do tráfego de Acesso Remoto quando o servidor de Acesso Remoto está na Internet IPv4:  
  
-   Protocolo 41 de entrada e saída do tráfego IP 6to4.  
  
-   Porta de destino 443 do IP-HTTPS-protocolo TCP (Transmission Control) e a porta de origem TCP 443 de saída. Quando o servidor de Acesso Remoto tem um único adaptador de rede e o servidor de local de rede está no servidor de acesso remoto, a porta TCP 62000 também é necessária.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções de firewall voltado para a Internet do tráfego de Acesso Remoto quando o servidor de Acesso Remoto está na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de Acesso Remoto:  
  
-   Protocolo ISATAP 41 de entrada e saída  
  
-   TCP/UDP para todo o tráfego IPv4/IPv6  
  
## <a name="ConfigCAs"></a>Configurar autoridades de certificação e certificados  
O Assistente para Habilitar o DirectAccess configura um proxy Kerberos interno que permite autenticação usando nomes de usuário e senhas. Ele também configura um certificado IP-HTTPS no servidor de Acesso Remoto.  
  
### <a name="ConfigCertTemp"></a>Configurar modelos de certificado  
Ao usar uma AC interna para emitir certificados, é necessário configurar um modelo de certificado para o certificado IP-HTTPS e o certificado de site do servidor de local de rede.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar um modelo de certificado  
  
1.  Na AC interna, crie um modelo de certificado conforme descrito em [Criando modelos de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Implante o modelo de certificado conforme descrito em [Deploying Certificate Templates (Implantando modelos de certificado)](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="configure-the-ip-https-certificate"></a>Configurar o certificado IP-HTTPS  
O Acesso Remoto requer um certificado IP-HTTPS para autenticar conexões IP-HTTPS para o servidor de Acesso Remoto. Existem três opções de certificado para o certificado IP-HTTPS:  
  
-   **Público**-fornecido por uma 3ª parte.  
  
    Um certificado usado para autenticação IP-HTTPS. Caso o nome da entidade do certificado não seja um curinga, ele deverá ser a URL de FQDN resolvível externamente, usada somente para conexões IP-HTTPS do servidor de Acesso Remoto.  
  
-   **Privada**-a seguir é necessária, se eles ainda não existirem:  
  
    -   Um certificado de site usado para autenticação IP-HTTPS. A entidade do certificado deve ser um FQDN (nome de domínio totalmente qualificado) externamente resolvível que pode ser obtido da Internet.  
  
    -   Um ponto de distribuição da lista de revogação de certificados (CRL) que está acessível por um FQDN resolvível publicamente.  
  
-   **Autoassinado**-a seguir é necessária, se eles ainda não existirem:  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
    -   Um certificado de site usado para autenticação IP-HTTPS. A entidade do certificado deve ser um FQDN externamente resolvível que pode ser obtido da Internet.  
  
    -   Um ponto de distribuição de CRL que pode ser obtido de um FQDN (nome de domínio totalmente qualificado) resolvível publicamente.  
  
Verifique se o certificado do site usado para autenticação IP-HTTPS atende aos seguintes requisitos:  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo Assunto, especifique o endereço IPv4 do adaptador externo do servidor de Acesso Remoto ou FQDN da URL IP-HTTPS.  
  
-   No campo Uso Avançado de Chave, use o OID (identificador de objeto) da Autenticação do Servidor.  
  
-   No campo Pontos de Distribuição da Lista de Certificados Revogados, especifique um ponto de distribuição da CRL que seja acessível pelos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem conter curingas no nome.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Para instalar o certificado IP-HTTPS de uma AC interna  
  
1.  No servidor de Acesso Remoto: Sobre o **inicie** tela, digite**mmc.exe**, e pressione ENTER.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar**, **Conta do computador**, **Avançar**, **Computador local**, **Concluir** e, por fim, em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Sobre o **solicitar certificados** página, marque a caixa de seleção para o modelo de certificado e, se necessário, clique em **obter mais informações são necessárias para se registrar neste certificado**.  
  
8.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
9. Em **Valor**, especifique o endereço IPv4 no adaptador externo do servidor de Acesso Remoto ou FQDN da URL IP-HTTPS e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **Valor**, especifique o endereço IPv4 no adaptador externo do servidor de Acesso Remoto ou FQDN da URL IP-HTTPS e clique em **Adicionar**.  
  
12. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
13. Na guia **Extensões**, ao lado de **Uso de Chave Estendida**, clique na seta e verifique se a Autenticação do Servidor está na lista **Opções selecionadas**.  
  
14. Clique em **OK**, **Registrar** e em **Concluir**.  
  
15. No painel de detalhes do snap-in Certificados, verifique se o novo certificado foi registrado com Finalidades de Autenticação de Servidor.  
  
## <a name="ConfigDNS"></a>Configurar o servidor DNS  
Você deve configurar manualmente uma entrada DNS para o site do servidor de local da rede interna da sua implantação.  
  
### <a name="NLS_DNS"></a>Para criar a investigação web e servidor do local de rede registros DNS  
  
1.  No servidor DNS da rede interna: Sobre o **iniciar** tela, digite * * dnsmgmt.msc**, e, em seguida, pressione ENTER.  
  
2.  No painel esquerdo do console **Gerenciador DNS**, expanda a zona de pesquisa direta para o seu domínio. Clique com o botão direito no domínio e clique em **Novo Host (A ou AAAA)**.  
  
3.  Na caixa de diálogo **Novo Host**, na caixa **Nome (usa o nome de domínio pai se deixado em branco)**, digite o nome de DNS para o site do servidor de local de rede (este é o nome que os clientes do DirectAccess usam para conectar ao servidor de local de rede). Na caixa **Endereço IP**, digite o endereço IPv4 no servidor de local de rede e clique em **Adicionar Host**. Na caixa de diálogo **DNS**, clique em **OK**.  
  
4.  Na caixa de diálogo **Novo Host**, na caixa **Nome (usa o nome de domínio pai se deixado em branco)**, digite o nome do DNS para a sonda da web (o nome para a sonda da web é directaccess-webprobehost). Na caixa **Endereço IP**, digite o endereço IPv4 da sonda da web e clique em **Adicionar Host**. Repita esse processo para o directaccess-corpconnectivityhost e quaisquer verificadores de conectividade criados manualmente. Na caixa de diálogo **DNS**, clique em **OK**.  
  
5.  Clique em **Concluído**.  

![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Você também deve configurar entradas DNS para o seguinte:  
  
-   **O servidor IP-HTTPS**clientes DirectAccess - devem ser capazes de resolver o nome DNS do servidor de acesso remoto da Internet.  
  
-   **Verificação de revogação de CRL**- DirectAccess usa para a conexão IP-HTTPS entre os clientes DirectAccess e o servidor de acesso remoto e para a conexão baseada em HTTPS entre o cliente DirectAccess de verificação de revogação de certificado e o servidor de local de rede. Em ambos os casos, os clientes do DirectAccess devem poder resolver e acessar o local do ponto de distribuição da CRL.  
  
## <a name="ConfigAD"></a>Configurar o Active Directory  
O servidor de Acesso Remoto e todos os computadores cliente do DirectAccess devem ser ingressados em um domínio do Active Directory. Os computadores cliente do DirectAccess devem ser membros de um dos seguintes tipos de domínio:  
  
-   Domínios pertencentes à mesma floresta que o servidor de Acesso Remoto.  
  
-   Domínios pertencentes a florestas com relações de confiança bidirecional com a floresta do servidor de Acesso Remoto.  
  
-   Domínios com relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Para ingressar computadores cliente no domínio  
  
1.  Sobre o **inicie** tela, digite **explorer.exe**, e pressione ENTER.  
  
2.  Clique com o botão direito do mouse no ícone Computador e em **Propriedades**.  
  
3.  Na página **Sistema**, clique em **Configurações avançadas do sistema**.  
  
4.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
5.  Em **Nome do computador**, digite o nome do computador se você também estiver alterando o nome do computador ao ingressar o servidor no domínio. Em **Membro de**, clique em **Domínio** e digite o nome do domínio em que você deseja ingressar o servidor; por exemplo, corp.contoso.com, e clique em **OK**.  
  
6.  Quando você for solicitado a informar um nome de usuário e uma senha, digite o nome de usuário e a senha de um usuário com direitos de ingressar computadores no domínio e clique em **OK**.  
  
7.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio, clique em **OK**.  
  
8.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
9. Na caixa de diálogo **Propriedades do Sistema**, clique em Fechar. Clique em **Reiniciar Agora** quando solicitado.  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Observe que você deve fornecer as credenciais do domínio depois de inserir o comando Add-Computer abaixo.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>Configurar GPOs  
Para implantar o acesso remoto, você precisará de pelo menos de dois objetos de diretiva de grupo: um objeto de diretiva de grupo contém configurações para o servidor de acesso remoto e o outro contém configurações para computadores cliente do DirectAccess. Quando você configurar o acesso remoto, o assistente cria automaticamente os objetos de diretiva de grupo necessários. No entanto, se sua organização impor uma convenção de nomenclatura, ou você não tenha as permissões necessárias para criar ou editar objetos de diretiva de grupo, eles devem ser criados antes de configurar o acesso remoto.  
  
Para criar objetos de diretiva de grupo, consulte [criar e editar um objeto de diretiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> O administrador pode vincular manualmente os objetos de diretiva de grupo do DirectAccess a uma unidade organizacional usando as seguintes etapas:  
>   
> 1.  Antes de configurar o DirectAccess, vincule os GPOs criados às respectivas Unidades Organizacionais.  
> 2.  Configure o DirectAccess, especificando um grupo de segurança para computadores cliente.  
> 3.  O administrador de acesso remoto pode ou não pode ter permissões para vincular objetos de diretiva de grupo no domínio. De qualquer maneira, os Objetos de Política de Grupo serão configurados automaticamente. Se os GPOs já estiverem vinculados a uma OU, os links não serão removidos, e os GPOs não serão vinculados ao domínio. Para um GPO de servidor, a OU deverá conter o objeto de computador do servidor, ou o GPO será vinculado à raiz do domínio.  
> 4.  Se a vinculação à OU não foi feito antes de executar o Assistente do DirectAccess, em seguida, depois que a configuração for concluída, o administrador do domínio poderá vincular os objetos de diretiva de grupo do DirectAccess às unidades organizacionais requeridas. O link para o domínio pode ser removido. As etapas para vincular um objeto de diretiva de grupo a uma unidade organizacional pode ser encontrada [aqui](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Se um objeto de diretiva de grupo foi criado manualmente, é possível durante a configuração do DirectAccess que o objeto de diretiva de grupo não estará disponível. O objeto de diretiva de grupo pode não ter sido replicado no controlador de domínio mais próximo ao computador de gerenciamento. Neste caso, o administrador pode aguardar a replicação ser concluída, ou forçá-la.  
  
## <a name="ConfigSGs"></a>Configurar grupos de segurança  
As configurações do DirectAccess contidas no objeto de diretiva de grupo do computador de cliente são aplicadas somente aos computadores que são membros dos grupos de segurança que você especificar ao configurar o acesso remoto. Além disso, se você está usando os grupos de segurança para gerenciar os servidores de aplicativo, crie um grupo de segurança para tais servidores.  
  
### <a name="Sec_Group"></a>Para criar um grupo de segurança para clientes do DirectAccess  
  
1.  Sobre o **inicie** tela, digite**DSA. msc**, e pressione ENTER. No console **Usuários e Computadores do Active Directory**, no painel esquerdo, expanda o domínio que conterá o grupo de segurança, clique com o botão direito do mouse em **Usuários**, aponte para **Novo** e clique em **Grupo**.  
  
2.  Na caixa de diálogo **Novo Objeto - Grupo**, em **Nome do grupo**, digite o nome do grupo de segurança.  
  
3.  Em **Escopo do grupo**, clique em **Global**, em **Tipo de grupo**, escolha **Segurança** e clique em **OK**.  
  
4.  Clique no grupo de segurança dos computadores cliente do DirectAccess e, na caixa de diálogo de propriedades, clique na guia **Membros**.  
  
5.  Na guia **Membros** , clique em **Adicionar**.  
  
6.  Na caixa de diálogo **Selecionar Usuários, Contatos, Computadores ou Contas de Serviço**, selecione os computadores cliente que você deseja habilitar para o DirectAccess e clique em **OK**.  
  
![Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)**comandos equivalentes do Windows PowerShell**  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>Configurar o servidor de local de rede  
O servidor de local de rede deve estar em um servidor com alta disponibilidade e possuir um certificado SSL válido confiável para os clientes do DirectAccess. Existem duas opções de certificado para o certificado do servidor de local de rede:  
  
-   **Privada**-a seguir é necessária, se eles ainda não existirem:  
  
    -   Um certificado de site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.   
  
    -   Um ponto de distribuição de CRL com alta disponibilidade na rede interna.  
  
-   **Autoassinado**-a seguir é necessária, se eles ainda não existirem:  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
    -   Um certificado de site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.  
  
> [!NOTE]  
> Se o site do servidor de local de rede encontrar-se no servidor de Acesso Remoto, um site será criado automaticamente ao configurar o Acesso Remoto, ligado ao certificado de servidor fornecido.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Para instalar o certificado de servidor de local de rede de uma AC interna  
  
1.  No servidor que hospedará o site do servidor de local de rede: Sobre o **inicie** tela, digite**mmc.exe**, e pressione ENTER.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar**, **Conta do computador**, **Avançar**, **Computador local**, **Concluir** e, por fim, em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Sobre o **solicitar certificados** página, marque a caixa de seleção para o modelo de certificado e, se necessário, clique em **obter mais informações são necessárias para se registrar neste certificado**.  
  
8.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
9. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
12. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in Certificados, verifique se o novo certificado foi registrado com Finalidades de Autenticação de Servidor.  
  

  


