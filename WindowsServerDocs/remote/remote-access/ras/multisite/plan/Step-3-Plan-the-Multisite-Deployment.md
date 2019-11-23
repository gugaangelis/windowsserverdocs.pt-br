---
title: Etapa 3 planejar a implantação multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1320a5c8b8c267f270dae43e764533d9289006a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404458"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Etapa 3 planejar a implantação multissite

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Depois de planejar a infraestrutura multissite, planeje os requisitos de certificado adicionais, como os computadores cliente selecionam pontos de entrada e endereços IPv6 atribuídos em sua implantação.  

As seções a seguir fornecem informações detalhadas de planejamento.
  
## <a name="bkmk_3_1_IPHTTPS"></a>3,1 planejar certificados IP-HTTPS  
Ao configurar os pontos de entrada, você configura cada ponto de entrada com um endereço connectto específico. O certificado IP-HTTPS para cada ponto de entrada deve corresponder ao endereço connectto. Observe o seguinte ao obter o certificado:  
  
-   Não é possível usar certificados autoassinados em uma implantação multissite.  
  
-   É recomendável usar uma AC pública, para que as CRLs estejam prontamente disponíveis.  
  
-   No campo assunto, especifique o endereço IPv4 do adaptador externo do servidor de acesso remoto (se o endereço connectto tiver sido especificado como um endereço IP e não um nome DNS) ou o FQDN da URL IP-HTTPS.  
  
-   O nome comum do certificado deve corresponder ao nome do site IP-HTTPS. Também há suporte para o uso de uma URL curinga que corresponda ao nome DNS connectto.  
  
-   Os certificados IP-HTTPS podem usar curingas no nome da entidade. O mesmo certificado curinga pode ser usado para todos os pontos de entrada.  
  
-   No campo Uso Avançado de Chave, use o OID (identificador de objeto) da Autenticação do Servidor.  
  
-   Se você estiver dando suporte a computadores cliente que executam o Windows 7 na implantação multissite, no campo pontos de distribuição da CRL, especifique um ponto de distribuição da CRL que seja acessível por clientes do DirectAccess que estão conectados à Internet. Isso não é necessário para clientes que executam o Windows 8 (por padrão, a verificação de revogação de CRL está desabilitada para IP-HTTPS nesses clientes).  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal do computador e não do usuário.  
  
## <a name="bkmk_3_2_NLS"></a>3,2 planejar o servidor do local de rede  
O site do servidor do local de rede pode ser hospedado no servidor de acesso remoto ou em outro servidor em sua organização. Se você hospedar o servidor de local de rede no servidor de acesso remoto, o site será criado automaticamente quando você implantar o acesso remoto. Se você hospedar o servidor de local de rede em outro servidor que executa um sistema operacional Windows em sua organização, deverá verificar se o Serviços de Informações da Internet (IIS) está instalado para criar o site.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 requisitos de certificado para o servidor de local de rede  
Verifique se o site do servidor do local de rede atende aos seguintes requisitos para a implantação de certificado:  
  
-   Ele requer um certificado de servidor HTTPS.  
  
-   Se o servidor de local de rede estiver localizado no servidor de acesso remoto e você tiver selecionado para usar um certificado autoassinado ao implantar o único servidor de acesso remoto, será necessário reconfigurar a implantação de servidor único para usar um certificado emitido por uma autoridade de certificação interna.  
  
-   Os computadores cliente do DirectAccess devem confiar na AC que emitiu o certificado do servidor para o site do servidor do local de rede.  
  
-   Os computadores cliente do DirectAccess na rede interna devem ser capazes de resolver o nome do site do servidor do local de rede.  
  
-   O site do servidor do local de rede deve estar altamente disponível para computadores na rede interna.  
  
-   O servidor do local de rede não deve ser acessível aos computadores cliente do DirectAccess pela Internet.  
  
-   O certificado do servidor deve ser verificado em relação a uma CRL (lista de certificados revogados).  
  
-   Não há suporte para certificados curinga quando o servidor de local de rede está hospedado no servidor de acesso remoto.  
  
Ao obter o certificado de site a ser usado para o servidor de local de rede, observe o seguinte:  
  
1.  No campo Assunto, especifique o endereço IP da interface da intranet do servidor de local de rede ou o FQDN da URL de local de rede. Observe que você não deve especificar um endereço IP se o servidor de local de rede estiver hospedado no servidor de acesso remoto. Isso ocorre porque o servidor de local de rede deve usar o mesmo nome de entidade para todos os pontos de entrada e nem todos os pontos de entrada têm o mesmo endereço IP.  
  
2.  No campo Uso Avançado de Chave, use o OID da Autenticação do Servidor.  
  
3.  Para o campo pontos de distribuição da CRL, use um ponto de distribuição de CRL acessível por clientes DirectAccess que estão conectados à intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2 DNS para o servidor de local de rede  
Se você hospedar o servidor de local de rede no servidor de acesso remoto, deverá adicionar uma entrada DNS para o site do servidor do local de rede para cada ponto de entrada em sua implantação. Observe o seguinte:  
  
-   O nome da entidade do primeiro certificado do servidor do local de rede na implantação multissite é usado como a URL do servidor do local de rede para todos os pontos de entrada, portanto, o nome da entidade e a URL do servidor do local de rede não podem ser iguais ao nome do computador do primeiro servidor de acesso remoto na implantação. Ele deve ser um FQDN dedicado para o servidor de local de rede.  
  
-   O serviço fornecido pelo tráfego do servidor de local de rede é balanceado entre pontos de entrada usando DNS e, portanto, deve haver uma entrada DNS com a mesma URL para cada ponto de entrada, configurado com o endereço IP interno do ponto de entrada.  
  
-   Todos os pontos de entrada devem ser configurados com um certificado de servidor de local de rede com o mesmo nome de entidade (que corresponde à URL do servidor de local de rede).  
  
-   A infraestrutura de servidor do local de rede (configurações de DNS e certificado) para um ponto de entrada deve ser criada antes de adicionar o ponto de entrada.  
  
## <a name="bkmk_3_3_IPsec"></a>3,3 planejar o certificado raiz IPsec para todos os servidores de acesso remoto  
Observe o seguinte ao planejar a autenticação de cliente IPsec em uma implantação multissite:  
  
1.  Se você optou por usar o proxy Kerberos interno para autenticação do computador ao configurar o único servidor de acesso remoto, será necessário alterar a configuração para usar certificados de computador emitidos por uma AC interna, pois o proxy Kerberos não tem suporte em um multissite planta.  
  
2.  Se você usou um certificado autoassinado, deverá reconfigurar a implantação de servidor único para usar um certificado emitido por uma autoridade de certificação interna.  
  
3.  Para que a autenticação IPsec tenha sucesso durante a autenticação do cliente, todos os servidores de acesso remoto devem ter um certificado emitido pela AC raiz ou intermediária do IPsec e com o OID de autenticação do cliente para uso avançado de chave.  
  
4.  A mesma raiz IPsec ou certificado intermediário deve ser instalado em todos os servidores de acesso remoto na implantação multissite.  
  
## <a name="bkmk_3_4_GSLB"></a>balanceamento de carga do servidor global do 3,4 plano  
Em uma implantação multissite, você também pode configurar um balanceador de carga de servidor global. Um balanceador de carga de servidor global poderá ser útil para sua organização se sua implantação abranger uma grande distribuição geográfica, pois ela pode distribuir a carga de tráfego entre os pontos de entrada.  O balanceador de carga do servidor global pode ser configurado para fornecer aos clientes DirectAccess as informações do ponto de entrada mais próximo. O processo funciona da seguinte maneira:  
  
1.  Os computadores cliente que executam o Windows 10 ou o Windows 8 têm uma lista de endereços IP do balanceador de carga de servidor global, cada um associado a um ponto de entrada.  
  
2.  O computador cliente Windows 10 ou Windows 8 tenta resolver o FQDN do balanceador de carga do servidor global no DNS público para um endereço IP. Se o endereço IP resolvido estiver listado como o endereço IP do balanceador de carga do servidor global de um ponto de entrada, o computador cliente selecionará automaticamente esse ponto de entrada e se conectará à sua URL IP-HTTPS (endereço connectto) ou ao seu endereço IP do servidor Teredo. Observe que o endereço IP do balanceador de carga do servidor global não precisa ser idêntico ao endereço connectto ou ao endereço do servidor Teredo do ponto de entrada, já que os computadores cliente nunca tentam se conectar ao endereço IP do balanceador de carga do servidor global.  
  
3.  Se o computador cliente estiver atrás de um proxy da Web (e não puder usar a resolução DNS) ou se o FQDN do balanceador de carga do servidor global não resolver para nenhum endereço IP do balanceador de carga global do servidor configurado, um ponto de entrada será selecionado automaticamente usando uma investigação de HTTPS para as URLs de IP-HTTPS de todos os pontos de entrada. O cliente se conectará ao servidor que responde primeiro.  
  
Para obter uma lista de dispositivos de balanceamento de carga de servidor global que dão suporte ao acesso remoto, vá para a página localizar um parceiro no [servidor e na plataforma de nuvem da Microsoft](https://www.microsoft.com/server-cloud/).  
  
## <a name="bkmk_3_5_EP_Selection"></a>seleção de ponto de entrada do cliente do plano 3,5 do DirectAccess  
Quando você configura uma implantação multissite, por padrão, os computadores cliente com Windows 10 e Windows 8 são configurados com as informações necessárias para se conectar a todos os pontos de entrada na implantação e para se conectar automaticamente a um único ponto de entrada com base em uma seleção algoritmo. Você também pode configurar sua implantação para permitir que computadores cliente com Windows 10 e Windows 8 selecionem manualmente o ponto de entrada ao qual se conectarão. Se um computador cliente Windows 10 ou Windows 8 estiver conectado atualmente à Estados Unidos ponto de entrada e a seleção de ponto de entrada automático estiver habilitada, se o ponto de entrada de Estados Unidos ficar inacessível, depois de alguns minutos o computador cliente tentará se conectar por meio do ponto de entrada da Europa. É recomendável usar a seleção de ponto de entrada automático; no entanto, permitir a seleção de ponto de entrada manual permite que os usuários finais se conectem a um ponto de entrada diferente com base nas condições de rede atuais. Por exemplo, se um computador estiver conectado ao ponto de entrada de Estados Unidos e a conexão com a rede interna se tornar muito mais lenta do que o esperado. Nessa situação, o usuário final pode selecionar manualmente para se conectar ao ponto de entrada da Europa para melhorar a conexão com a rede interna.  
  
> [!NOTE]  
> Depois que um usuário final selecionar um ponto de entrada manualmente, o computador cliente não será revertido para a seleção de ponto de entrada automática. Ou seja, se o ponto de entrada selecionado manualmente ficar inacessível, o usuário final deverá reverter para a seleção de ponto de entrada automática ou selecionar manualmente outro ponto de entrada.  
  
 Os computadores cliente do Windows 7 são configurados com as informações necessárias para se conectar a um único ponto de entrada na implantação multissite. Eles não podem armazenar as informações para vários pontos de entrada simultaneamente. Por exemplo, um computador cliente do Windows 7 pode ser configurado para se conectar ao ponto de entrada do Estados Unidos, mas não ao ponto de entrada da Europa. Se o ponto de entrada de Estados Unidos estiver inacessível, o computador cliente do Windows 7 perderá a conectividade com a rede interna até que o ponto de entrada seja acessível. O usuário final não pode fazer nenhuma alteração para tentar se conectar ao ponto de entrada da Europa.  
  
## <a name="bkmk_3_6_IPv6"></a>prefixos e roteamentos do plano 3,6  
  
### <a name="internal-ipv6-prefix"></a>Prefixo IPv6 interno  
Durante a implantação do servidor de acesso remoto único, você planejou os prefixos IPv6 da rede interna, observe o seguinte em uma implantação multissite:  
  
1.  Se você incluiu todos os sites Active Directory quando configurou a implantação de acesso remoto de servidor único, os prefixos IPv6 da rede interna já estarão definidos no console de gerenciamento de acesso remoto.  
  
2.  Se você criar sites Active Directory adicionais para implantação multissite, deverá planejar novos prefixos IPv6 para os sites adicionais e defini-los no acesso remoto. Observe que os prefixos IPv6 só podem ser configurados usando o console de gerenciamento de acesso remoto ou cmdlets do PowerShell se o IPv6 for implantado na rede corporativa interna.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Prefixo IPv6 para computadores cliente do DirectAccess (prefixo IP-HTTPS)  
  
1.  Se o IPv6 for implantado na rede corporativa interna, você deverá planejar um prefixo IPv6 para atribuir a computadores cliente do DirectAccess em quaisquer pontos de entrada adicionais em sua implantação.  
  
2.  Verifique se os prefixos IPv6 a serem atribuídos aos computadores cliente do DirectAccess em cada ponto de entrada são distintos e se não há sobreposição nos prefixos IPv6.  
  
3.  Se o IPv6 não estiver implantado na rede corporativa, um prefixo IP-HTTPS para cada ponto de entrada será selecionado automaticamente ao adicionar o ponto de entrada.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Prefixo IPv6 para clientes VPN  
Se você implantou a VPN no servidor de acesso remoto único, observe o seguinte:  
  
1.  A adição de um prefixo VPN IPv6 a um ponto de entrada só será necessária se você quiser permitir a conectividade IPv6 do cliente VPN para a rede corporativa.  
  
2.  O prefixo de VPN só poderá ser configurado em um ponto de entrada usando o console de gerenciamento de acesso remoto ou o cmdlet do PowerShell se o IPv6 estiver implantado na rede corporativa interna e se a VPN estiver habilitada no ponto de entrada.  
  
3.  O prefixo de VPN deve ser exclusivo em cada ponto de entrada e não deve se sobrepor a outros prefixos VPN ou IP-HTTPS.  
  
4.  Se o IPv6 não estiver implantado na rede corporativa, os clientes VPN que se conectarem ao ponto de entrada não serão atribuídos a um endereço IPv6.  
  
### <a name="routing"></a>Roteamento  
Em um roteamento simétrico de implantação multissite é imposta usando Teredo e IP-HTTPS. Quando o IPv6 for implantado na rede corporativa, observe o seguinte:  
  
1. Os prefixos Teredo e IP-HTTPS de cada ponto de entrada devem ser roteáveis pela rede corporativa para seu servidor de acesso remoto associado.  
  
2. As rotas devem ser configuradas na infraestrutura de roteamento de rede corporativa.  
  
3. Para cada ponto de entrada, deve haver uma a três rotas na rede interna:  
  
   1. Prefixo IP-HTTPS-esse prefixo é escolhido pelo administrador no assistente Adicionar um ponto de entrada.  
  
   2. Prefixo de VPN IPv6 (opcional). Esse prefixo pode ser escolhido depois de habilitar a VPN para um ponto de entrada  
  
   3. Prefixo Teredo (opcional). Esse prefixo só será relevante se o servidor de acesso remoto estiver configurado com dois endereços IPv4 públicos consecutivos no adaptador externo. O prefixo é baseado no primeiro endereço IPv4 público do par de endereços. Por exemplo, se os endereços externos forem:  
  
      1. www\.xxx. yyy. zzz  
  
      2. www\.xxx. yyy. zzz + 1  
  
      Em seguida, o prefixo Teredo a ser configurado é 2001:0: WWXX: YYZZ::/64, em que WWXX: YYZZ é a representação hexadecimal do endereço IPv4 www\.xxx. yyy. zzz.  
  
      Observe que você pode usar o script a seguir para calcular o prefixo Teredo:  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Todas as rotas acima devem ser roteadas para o endereço IPv6 no adaptador interno do servidor de acesso remoto (ou para o endereço VIP (IP virtual) interno de um ponto de entrada com balanceamento de carga).  
  
> [!NOTE]  
> Quando o IPv6 é implantado na rede corporativa e a administração do servidor de acesso remoto é executada remotamente pelo DirectAccess, as rotas para os prefixos Teredo e IP-HTTPS de todos os outros pontos de entrada devem ser adicionadas a cada servidor de acesso remoto para que o tráfego seja encaminhado para a rede interna.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory prefixos IPv6 específicos do site  
Quando um computador cliente que executa o Windows 10 ou o Windows 8 está conectado a um ponto de entrada, o computador cliente é imediatamente associado ao site de Active Directory do ponto de entrada e é configurado com prefixos IPv6 associados ao ponto de entrada. A preferência é que os computadores cliente se conectem aos recursos usando esses prefixos IPv6, pois eles são configurados dinamicamente na tabela de diretiva de prefixo IPv6 com maior precedência ao se conectar a um ponto de entrada.  
  
Se sua organização usa uma topologia de Active Directory com prefixos IPv6 específicos do site (por exemplo, um app.corp.com de recurso interno FQDN é hospedado em América do Norte e na Europa com um endereço IP específico do site em cada local), isso não é configurado pelo o padrão é usar o console de acesso remoto, e os prefixos IPv6 específicos do site não são configurados para cada ponto de entrada. Se você quiser habilitar esse cenário opcional, precisará configurar cada ponto de entrada com os prefixos IPv6 específicos que devem ser preferidos por computadores cliente que se conectam a um ponto de entrada específico. Faça isso da seguinte maneira:  
  
1.  Para cada GPO usado para computadores cliente Windows 10 ou Windows 8, execute o cmdlet Set-DAEntryPointTableItem do PowerShell  
  
2.  Defina o parâmetro EntryPointRange para o cmdlet com os prefixos IPv6 específicos do site. Por exemplo, para adicionar os prefixos específicos do site 2001: DB8:1: 1::/64 e 2001: DB: 1:2::/64 a um ponto de entrada chamado Europa, execute o seguinte  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Ao modificar o parâmetro EntryPointRange, certifique-se de não remover os prefixos de 128 bits existentes que pertencem aos pontos de extremidade de túnel IPsec e ao endereço DNS64.  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3,7 planejar a transição para o IPv6 quando o acesso remoto multissite for implantado  
Muitas organizações usam o protocolo IPv4 na rede corporativa. Com o esgotamento de prefixos IPv4 disponíveis, muitas organizações estão fazendo a transição de somente IPv4 para redes IPv6.  
  
É mais provável que essa transição ocorra em dois estágios:  
  
1.  De uma rede corporativa somente IPv4 a um IPv6 + IPv4.  
  
2.  De um IPv6 + IPv4 para uma rede corporativa somente IPv6.  
  
Em cada parte, a transição pode ser executada em estágios. Em cada estágio, apenas uma sub-rede da rede pode ser alterada para a nova configuração de rede. Portanto, uma implantação multissite do DirectAccess é necessária para dar suporte a uma implantação híbrida, em que, por exemplo, alguns dos pontos de entrada pertencem a uma sub-rede somente IPv4 e outros pertencem a uma sub-rede IPv6 + IPv4. Além disso, as alterações de configuração durante os processos de transição não devem interromper a conectividade do cliente via DirectAccess.  
  
### <a name="TransitionIPv4toMixed"></a>Transição de uma rede corporativa somente IPv4 para um IPv6 + IPv4  
Ao adicionar endereços IPv6 a uma rede corporativa somente IPv4, talvez você queira adicionar um endereço IPv6 a um servidor DirectAccess já implantado. Além disso, talvez você queira adicionar um ponto de entrada ou um nó a um cluster de balanceamento de carga com endereços IPv4 e IPv6 à implantação do DirectAccess.  
  
O acesso remoto permite que você adicione servidores com endereços IPv4 e IPv6 a uma implantação que foi originalmente configurada com apenas endereços IPv4. Esses servidores são adicionados como servidores somente IPv4 e seus endereços IPv6 são ignorados pelo DirectAccess; Consequentemente, sua organização não pode aproveitar os benefícios da conectividade IPv6 nativa nesses novos servidores.  
  
Para transformar a implantação em uma implantação IPv6 + IPv4 e aproveitar os recursos IPv6 nativos, você deve reinstalar o DirectAccess. Para manter a conectividade do cliente durante a reinstalação, consulte transição de uma implantação somente IPv4 para uma única IPv6 usando implantações de DirectAccess duplas.  
  
> [!NOTE]  
> Assim como acontece com uma rede somente IPv4, em uma rede IPv4 + IPv6 mista, o endereço do servidor DNS que é usado para resolver as solicitações de DNS do cliente deve ser configurado com o DNS64 que é implantado nos próprios servidores de acesso remoto e não com um DNS corporativo.  
  
### <a name="TransitionMixedtoIPv6"></a>Transição de um IPv6 + IPv4 para uma rede corporativa somente IPv6  
O DirectAccess permite que você adicione pontos de entrada somente IPv6 somente se o primeiro servidor de acesso remoto na implantação tiver originalmente endereços IPv4 e IPv6 ou apenas um endereço IPv6. Ou seja, você não pode fazer a transição de uma rede somente IPv4 para uma rede somente IPv6 em uma única etapa sem reinstalar o DirectAccess. Para fazer a transição diretamente de uma rede somente IPv4 para uma rede somente IPv6, consulte transição de uma implantação somente IPv4 para uma única IPv6 usando implantações de DirectAccess duplas.  
  
Depois de concluir a transição de uma implantação somente IPv4 para uma implantação IPv6 + IPv4, você poderá fazer a transição para uma rede somente IPv6. Durante e após a transição, observe o seguinte:  
  
-   Se qualquer servidor de back-end somente IPv4 permanecer na rede corporativa, eles não poderão ser acessados por clientes que se conectam por meio de pontos de entrada somente IPv6.  
  
-   Ao adicionar pontos de entrada somente IPv6 a uma implantação IPv4 + IPv6, o DNS64 e o NAT64 não serão habilitados nos novos servidores. Os clientes que se conectam a esses pontos de entrada serão automaticamente configurados para usar os servidores DNS corporativos.  
  
-   Se precisar excluir endereços IPv4 de um servidor implantado, você deverá remover o servidor da implantação do DirectAccess, remover seu endereço de rede corporativa IPv4 e adicioná-lo novamente à implantação.  
  
Para dar suporte à conectividade de cliente com a rede corporativa, você deve garantir que o servidor de local de rede possa ser resolvido pelo DNS corporativo para seu endereço IPv6. Um endereço IPv4 adicional também pode ser definido, mas não é necessário.  
  
### <a name="DualDeployment"></a>Transição de um IPv4-somente para uma implantação somente IPv6 usando implantações de DirectAccess duplas  
A transição de um somente IPv4 para uma rede corporativa somente IPv6 não pode ser feita sem reinstalar a implantação do DirectAccess. Para manter a conectividade do cliente durante a transição, você pode usar outra implantação do DirectAccess. A implantação dupla é necessária quando o primeiro estágio de transição é concluído (rede somente IPv4 atualizada para IPv4 + IPv6) e você pretende se preparar para uma transição futura para uma rede corporativa somente IPv6 orto aproveitar os benefícios de conectividade IPv6 nativos. A implantação dupla é descrita nas seguintes etapas gerais:  
  
1.  Instale uma segunda implantação do DirectAccess. Você pode instalar o DirectAccess em novos servidores ou remover servidores da primeira implantação e usá-los para a segunda implantação.  
  
    > [!NOTE]  
    > Ao instalar uma implantação adicional do DirectAccess junto com uma atual, verifique se não há dois pontos de entrada que compartilham o mesmo prefixo de cliente.  
    >   
    > Se você instalar o DirectAccess usando o assistente de Introdução ou com o cmdlet `Install-RemoteAccess`, o acesso remoto definirá automaticamente o prefixo de cliente do primeiro ponto de entrada na implantação para um valor padrão de <\_prefixo de sub-rede IPv6 >: 1000::/64. Se necessário, você deve alterar o prefixo.  
  
2.  Remova os grupos de segurança de cliente escolhidos da primeira implantação.  
  
3.  Adicione os grupos de segurança do cliente à segunda implantação.  
  
    > [!IMPORTANT]  
    > Para manter a conectividade do cliente durante todo o processo, você deve adicionar os grupos de segurança à segunda implantação imediatamente após removê-los do primeiro. Isso garante que os clientes não serão atualizados com dois ou zero GPOs do DirectAccess. Os clientes começarão a usar a segunda implantação depois de recuperarem e atualizarem o GPO do cliente.  
  
4.  Opcional: Remova os pontos de entrada do DirectAccess da primeira implantação e adicione esses servidores como novos pontos de entrada no segundo.  
  
Quando tiver concluído a transição, você poderá desinstalar a primeira implantação do DirectAccess. Ao desinstalar o, os seguintes problemas podem ocorrer:  
  
-   Se a implantação tiver sido configurada para dar suporte apenas a clientes em computadores móveis, o filtro WMI será excluído. Se os grupos de segurança do cliente da segunda implantação incluírem computadores desktop, o GPO do cliente DirectAccess não filtrará computadores desktop e poderá causar problemas neles. Se um filtro de computadores móveis for necessário, recrie-o seguindo as instruções em [criar filtros WMI para o GPO](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Se ambas as implantações foram originalmente criadas no mesmo domínio Active Directory, a entrada de investigação de DNS que aponta para localhost será excluída e poderá causar problemas de conectividade do cliente. Por exemplo, os clientes podem se conectar usando IP-HTTPS em vez de Teredo ou alternar entre pontos de entrada multissite do DirectAccess. Nesse caso, você deve adicionar a seguinte entrada DNS ao DNS corporativo:  
  
    -   Zona: nome de domínio  
  
    -   Nome: DirectAccess-corpConnectivityHost  
  
    -   Endereço IP::: 1  
  
    -   Tipo: AAAA  
  
  
  


