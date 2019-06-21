---
title: Etapa 3 plano implantação multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 16a2dcdc573fac2631b5a9890ee04f2efb08d90a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282535"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Etapa 3 plano implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Após planejar a infraestrutura de multissite, planejar quaisquer requisitos de certificado adicionais, como computadores cliente selecionar pontos de entrada e endereços IPv6 atribuídos em sua implantação.  

As seções a seguir fornecem informações detalhadas de planejamento.
  
## <a name="bkmk_3_1_IPHTTPS"></a>Certificados de IP-HTTPS planejar 3.1  
Quando você configura os pontos de entrada, você configura cada ponto de entrada com um endereço ConnectTo específico. O certificado IP-HTTPS para cada ponto de entrada deve corresponder ao endereço ConnectTo. Observe o seguinte ao obter o certificado:  
  
-   Não é possível usar certificados autoassinados em uma implantação multissite.  
  
-   É recomendável usar uma AC pública, para que as CRLs estejam prontamente disponíveis.  
  
-   No campo assunto, especifique o endereço IPv4 do adaptador externo do servidor de acesso remoto (se o endereço ConnectTo foi especificado como um endereço IP e não é um nome DNS), ou o FQDN da URL IP-HTTPS.  
  
-   O nome comum do certificado deve corresponder ao nome do site da Web IP-HTTPS. Também há suporte para o uso de uma URL curinga que corresponda ao nome DNS ConnectTo.  
  
-   Certificados IP-HTTPS podem usar curingas no nome da entidade. O mesmo certificado curinga pode ser usado para todos os pontos de entrada.  
  
-   No campo Uso Avançado de Chave, use o OID (identificador de objeto) da Autenticação do Servidor.  
  
-   Se você estiver dando suporte a computadores cliente que executam o Windows 7 na implantação multissite, no campo pontos de distribuição da CRL, especifique um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess que estão conectados à Internet. Isso não é necessário para clientes que executam o Windows 8 (por padrão, que a verificação de revogação CRL está desabilitada para IP-HTTPS nesses clientes).  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal do computador e não do usuário.  
  
## <a name="bkmk_3_2_NLS"></a>3.2 planejar o servidor de local de rede  
O site de servidor de local de rede pode ser hospedado no servidor de acesso remoto ou em outro servidor na sua organização. Se você hospedar o servidor de local de rede no servidor de acesso remoto, o site será criado automaticamente quando você implanta o acesso remoto. Se você hospedar o servidor de local de rede em outro servidor que executa um sistema de operacional Windows em sua organização, certifique-se de que os serviços de informações da Internet (IIS) é instalado para criar o site.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 requisitos de certificado para o servidor de local de rede  
Certifique-se de que o site de servidor de local de rede atende aos seguintes requisitos para implantação de certificado:  
  
-   Ele requer um certificado de servidor HTTPS.  
  
-   Se o servidor de local de rede está localizado no servidor de acesso remoto e você optou por usar um certificado autoassinado, quando você implantou o servidor de acesso remoto, você deve reconfigurar a implantação de servidor único para usar um certificado emitido por uma CA interna.  
  
-   Os computadores cliente do DirectAccess devem confiar na AC que emitiu o certificado do servidor para o site do servidor do local de rede.  
  
-   Os computadores cliente DirectAccess na rede interna devem ser capazes de resolver o nome do site de servidor de local de rede.  
  
-   O site de servidor de local de rede deve estar altamente disponível para computadores na rede interna.  
  
-   O servidor do local de rede não deve ser acessível aos computadores cliente do DirectAccess pela Internet.  
  
-   O certificado do servidor deve ser verificado em relação a uma lista (certificados Revogados).  
  
-   Não há suporte para certificados curinga quando o servidor de local de rede está hospedado no servidor de acesso remoto.  
  
Ao obter o certificado de site a ser usado para o servidor de local de rede, observe o seguinte:  
  
1.  No campo Assunto, especifique o endereço IP da interface da intranet do servidor de local de rede ou o FQDN da URL de local de rede. Observe que você não deve especificar um endereço IP, se o servidor de local de rede estiver hospedado no servidor de acesso remoto. Isso ocorre porque o servidor de local de rede deve usar o mesmo nome de assunto para todos os pontos de entrada, e nem todos os pontos de entrada têm o mesmo endereço IP.  
  
2.  No campo Uso Avançado de Chave, use o OID da Autenticação do Servidor.  
  
3.  No campo pontos de distribuição da CRL, use um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess que estão conectados à intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2DNS para o servidor de local de rede  
Se você hospedar o servidor de local de rede no servidor de acesso remoto, você deve adicionar uma entrada DNS para o site de servidor de local de rede para cada ponto de entrada em sua implantação. Observe o seguinte:  
  
-   Nome do assunto do primeiro certificado de servidor de local de rede na implantação multissite é usado como a URL de servidor de local de rede para todos os pontos de entrada, portanto o nome da entidade e a URL de servidor de local de rede não podem ser o mesmo que o nome do computador das primeiro servidor de acesso remoto na implantação. Ele deve ser um FQDN para o servidor de local de rede dedicado.  
  
-   O serviço fornecido pelo tráfego de servidor de local de rede é balanceado entre os pontos de entrada usando o DNS e, portanto, deve haver uma entrada DNS com a mesma URL para cada ponto de entrada, configurado com o endereço IP interno do ponto de entrada.  
  
-   Todos os pontos de entrada devem ser configurados com um certificado de servidor de local de rede com o mesmo nome de assunto (que corresponde à URL do servidor de rede local).  
  
-   A infraestrutura de servidor de local de rede (configurações de DNS e o certificado) para um ponto de entrada deve ser criada antes de adicionar o ponto de entrada.  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 planejar o certificado raiz do IPsec para todos os servidores de acesso remoto  
Ao planejar para a autenticação IPsec de cliente em uma implantação multissite, observe o seguinte:  
  
1.  Se você optou por usar o proxy Kerberos interno para autenticação do computador quando você configura o servidor de acesso remoto, você deve alterar a configuração para usar certificados de computador emitidos por uma CA interna, uma vez que não há suporte para o proxy Kerberos para um multissite implantação.  
  
2.  Se você usou um certificado autoassinado, você deve reconfigurar a implantação de servidor único para usar um certificado emitido por uma CA interna.  
  
3.  Para a autenticação IPsec ser bem-sucedido durante a autenticação de cliente, todos os servidores de acesso remoto devem ter um certificado emitido pela autoridade de certificação intermediária ou raiz do IPsec e com o OID de autenticação de cliente para uso avançado de chave.  
  
4.  A mesma raiz do IPsec ou certificado intermediário deve ser instalado em todos os servidores de acesso remoto na implantação multissite.  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 planejar o balanceamento de carga do servidor global  
Em uma implantação multissite, você pode configurar adicionalmente um balanceador de carga do servidor global. Um balanceador de carga do servidor global pode ser útil para sua organização, se sua implantação abrange uma grande distribuição geográfica porque ele pode distribuir carga de tráfego entre os pontos de entrada.  O balanceador de carga do servidor global pode ser configurado para fornecer os clientes DirectAccess com as informações de ponto de entrada do ponto de entrada mais próximo. O processo funciona da seguinte maneira:  
  
1.  Computadores cliente que executam o Windows 10 ou Windows 8 tem uma lista de endereços IP do balanceador de carga do servidor global, cada um associado a um ponto de entrada.  
  
2.  O computador cliente Windows 10 ou Windows 8 tenta resolver o FQDN do balanceador de carga global do servidor no DNS público para um endereço IP. Se o endereço IP resolvido é listado como o endereço IP de Balanceador de carga do servidor global de um ponto de entrada, o computador cliente automaticamente seleciona o ponto de entrada e se conecta a sua URL IP-HTTPS (endereço ConnectTo), ou para seu endereço IP do servidor Teredo. Observe que o endereço IP do balanceador de carga global do servidor não precisam ser idênticos para o endereço ConnectTo ou para o endereço do servidor Teredo do ponto de entrada, uma vez que os computadores cliente nunca tentam conectar-se para o endereço IP do balanceador de carga do servidor global.  
  
3.  Se o computador cliente estiver atrás de um proxy da web (e não é possível usar a resolução de DNS), ou se o FQDN do balanceador de carga global do servidor não resolver para algum configurado o endereço IP do balanceador de carga do servidor global e, em seguida, um ponto de entrada será selecionado automaticamente usando uma investigação HTTPS para as URLs de IP-HTTPS de todos os pontos de entrada. O cliente se conectará ao servidor que responder primeiro.  
  
Para obter uma lista de dispositivos que dão suporte ao acesso remoto de balanceamento de carga do servidor global, vá para localizar uma página de parceiro no [Microsoft Server e plataforma de nuvem](https://www.microsoft.com/server-cloud/).  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 planejar a seleção de ponto de entrada de cliente do DirectAccess  
Quando você configura uma implantação multissite, por padrão, o Windows 10 e computadores cliente com Windows 8 são configurados com as informações necessárias para se conectar a todos os pontos de entrada na implantação e para conectar automaticamente a um único ponto de entrada com base em uma seleção algoritmo. Você também pode configurar sua implantação para permitir que o Windows 10 e computadores cliente com Windows 8 para selecionar manualmente a entrada de apontam para que eles se conectarão. Se um computador cliente Windows 10 ou Windows 8 está atualmente conectado ao ponto de entrada dos Estados Unidos e seleção de ponto de entrada automática está habilitada, se o ponto de entrada dos Estados Unidos se torna inacessível, alguns minutos após o computador cliente tentará se conectar por meio do ponto de entrada na Europa. É recomendável usar a seleção de ponto de entrada automática; No entanto, permitindo que o ponto de entrada manual de seleção permite que os usuários finais para se conectar a um ponto de entrada diferentes com base nas condições de rede atual. Por exemplo, se um computador é conectado ao ponto de entrada dos Estados Unidos e a conexão à rede interna se torna muito mais lento do que o esperado. Nessa situação, o usuário final pode selecionar manualmente para se conectar ao ponto de entrada da Europa para melhorar a conexão à rede interna.  
  
> [!NOTE]  
> Depois que um usuário final seleciona um ponto de entrada manualmente, o computador cliente não será revertido para seleção de ponto de entrada automática. Ou seja, se o ponto de entrada selecionados manualmente ficar inacessível, o usuário final deve seja revertido para a seleção de ponto de entrada automática ou manualmente, selecione outro ponto de entrada.  
  
 Computadores cliente do Windows 7 são configurados com as informações necessárias para se conectar a um único ponto de entrada na implantação multissite. Eles não é possível armazenar as informações de vários pontos de entrada simultaneamente. Por exemplo, um computador de cliente do Windows 7 pode ser configurado para se conectar ao ponto de entrada dos Estados Unidos, mas não o ponto de entrada na Europa. Se o ponto de entrada dos Estados Unidos está inacessível, o computador de cliente do Windows 7 perderá a conectividade à rede interna até que o ponto de entrada está acessível. O usuário final não é possível fazer alterações para tentar se conectar ao ponto de entrada na Europa.  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 planejar prefixos e roteamento  
  
### <a name="internal-ipv6-prefix"></a>Prefixo IPv6 interno  
Durante a implantação do acesso remoto único servidor que você planejou os prefixos de IPv6 da rede interna, observe o seguinte em uma implantação multissite:  
  
1.  Se você incluiu todos os seus sites do Active Directory quando você configurou sua implantação do acesso remoto de servidor único, os prefixos de IPv6 da rede interna já serão definidos no console de gerenciamento de acesso remoto.  
  
2.  Se você criar sites adicionais do Active Directory para a implantação multissite, você deve planejar novos prefixos IPv6 para os sites adicionais e defini-los no acesso remoto. Observe que os prefixos IPv6 só podem ser configurados usando o console de gerenciamento de acesso remoto ou cmdlets do PowerShell se IPv6 está implantado na rede corporativa interna.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Prefixo IPv6 para os computadores cliente DirectAccess (prefixo IP-HTTPS)  
  
1.  Se o IPv6 está implantado na rede corporativa interna, você deve planejar um prefixo IPv6 para atribuir a computadores cliente do DirectAccess em qualquer ponto de entrada adicionais em sua implantação.  
  
2.  Certifique-se de que os prefixos IPv6 para atribuir a computadores cliente do DirectAccess em cada ponto de entrada são diferentes e que não haja nenhuma sobreposição nos prefixos de IPv6.  
  
3.  Se o IPv6 não está implantado na rede corporativa, um prefixo IP-HTTPS para cada ponto de entrada será selecionado automaticamente ao adicionar o ponto de entrada.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Prefixo IPv6 para clientes VPN  
Se você implantou o VPN no servidor de acesso remoto único, observe o seguinte:  
  
1.  Adição de uma VPN de IPv6 prefixo para um ponto de entrada só é necessária se você deseja permitir que o cliente VPN conectividade IPv6 com a rede corporativa.  
  
2.  O prefixo da VPN só pode ser configurado em um ponto de entrada usando o console de gerenciamento de acesso remoto ou o cmdlet do PowerShell se IPv6 está implantado na rede corporativa interna e se a VPN estiver habilitada no ponto de entrada.  
  
3.  O prefixo da VPN deve ser exclusivo em cada ponto de entrada e não deve se sobrepor com outros prefixos VPN ou IP-HTTPS.  
  
4.  Se o IPv6 não está implantado na rede corporativa, os clientes VPN conectando-se ao ponto de entrada não serão atribuídos um endereço IPv6.  
  
### <a name="routing"></a>Roteamento  
Em uma implantação multissite o roteamento simétrico é imposto usando Teredo e IP-HTTPS. Quando o IPv6 está implantado na rede corporativa, observe o seguinte:  
  
1. Os prefixos de Teredo e IP-HTTPS de cada ponto de entrada devem ser roteáveis em seu servidor de acesso remoto associado à rede corporativa.  
  
2. As rotas devem ser configuradas na infraestrutura de roteamento de rede corporativa.  
  
3. Para cada ponto de entrada, deverá haver um a três rotas na rede interna:  
  
   1. Prefixo de IP-HTTPS prefixo – isso é escolhido pelo administrador no adicionar um Assistente de ponto de entrada.  
  
   2. Prefixo IPv6 de VPN (opcional). Esse prefixo pode ser escolhido depois de habilitar a VPN para um ponto de entrada  
  
   3. Prefixo de Teredo (opcional). Esse prefixo é relevante apenas se o servidor de acesso remoto está configurado com dois endereços consecutivos sejam públicos IPv4 no adaptador externo. O prefixo baseia-se o primeiro endereço IPv4 público do par de endereço. Por exemplo, se os endereços externos são:  
  
      1. www.xxx.yyy.zzz  
  
      2. www.xxx.yyy.zzz+1  
  
      Em seguida, o prefixo de Teredo para configurar é 2001:0:WWXX:YYZZ::/ 64, onde WWXX: YYZZ é a representação hexadecimal do IPv4 endereço www.xxx.yyy.zzz.  
  
      Observe que você pode usar o script a seguir para calcular o prefixo de Teredo:  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Todas as rotas acima devem ser roteadas para o endereço IPv6 no adaptador interno do servidor de acesso remoto (ou para o endereço IP (VIP) virtual interno para uma carga balanceada ponto de entrada).  
  
> [!NOTE]  
> Quando o IPv6 está implantado na rede corporativa e administração de servidor de acesso remoto é executado remotamente pelo DirectAccess, as rotas para o Teredo e prefixos de IP-HTTPS de todos os outros pontos de entrada devem ser adicionados a cada servidor de acesso remoto para que o tráfego será encaminhado à rede interna.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Prefixos IPv6 específicas do site do Active Directory  
Quando um computador cliente executando o Windows 10 ou Windows 8 é conectado a um ponto de entrada, o computador cliente é imediatamente associados ao site do Active Directory do ponto de entrada e é configurada com prefixos IPv6 associados com o ponto de entrada. A preferência é para computadores cliente para se conectar aos recursos usando esses prefixos IPv6, já que eles sejam configurados dinamicamente na tabela de diretivas de prefixo IPv6 com maior precedência ao se conectar a um ponto de entrada.  
  
Se sua organização usa uma topologia do Active Directory com prefixos específicos do site do IPv6 (por exemplo um app.corp.com FQDN interno do recurso é hospedado na América do Norte e na Europa com um endereço IP específico do site em cada local) não for configurada por padrão usando o console de acesso remoto e prefixos de IPv6 específicos do site não está configurada para cada ponto de entrada. Se você quiser habilitar esse cenário opcional, você precisará configurar cada ponto de entrada com os prefixos IPv6 específicos que devem ser preferencial por computadores cliente se conectam a um ponto de entrada específica. Fazer isso da seguinte maneira:  
  
1.  Para cada GPO usado para computadores de cliente do Windows 8 ou Windows 10, execute o cmdlet do PowerShell Set-DAEntryPointTableItem  
  
2.  Defina o parâmetro EntryPointRange para o cmdlet com os prefixos IPv6 específico do site. Por exemplo adicionar o específicos do site prefixos 2001:db8:1:1:::/ 64 e 2001:db:1:2::/ / 64 para um ponto de entrada chamado Europa, execute o seguinte  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Ao modificar o parâmetro EntryPointRange, certifique-se de que você não remover os prefixos de 128 bits existentes que pertencem aos pontos de extremidade do túnel de IPsec e o endereço do DNS64.  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 planejar a transição para IPv6 quando o acesso remoto multissite é implantado  
Muitas organizações usam o protocolo IPv4 na rede corporativa. Com o esgotamento de prefixos de IPv4 disponíveis, muitas organizações estão fazer a transição de somente IPv4 para redes somente IPv6.  
  
Essa transição é mais provável entrar em vigor em dois estágios:  
  
1.  De um IPv4 somente a uma rede corporativa do IPv4 + IPv6.  
  
2.  De um IPv6 + IPv4 para uma rede corporativa somente IPv6.  
  
Cada parte, a transição pode ser executada em estágios. Em cada estágio apenas uma sub-rede da rede pode ser alterada para a nova configuração de rede. Portanto, uma implantação multissite do DirectAccess é necessária para dar suporte a uma implantação híbrida, onde, por exemplo, alguns dos pontos de entrada pertencem a uma sub-rede somente com IPv4 e outras pessoas pertencem a uma sub-rede IPv4 + IPv6. Além disso, as alterações de configuração durante os processos de transição não devem interromper a conectividade de cliente por meio do DirectAccess.  
  
### <a name="TransitionIPv4toMixed"></a>Fazer a transição de um somente com IPv4 para uma rede corporativa do IPv4 + IPv6  
Ao adicionar endereços IPv6 a uma rede corporativa somente IPv4, você talvez queira adicionar um endereço IPv6 a um servidor DirectAccess já implantado. Além disso, você talvez queira adicionar um ponto de entrada ou um nó a um cluster com balanceamento de carga com endereços IPv4 e IPv6 para a implantação do DirectAccess.  
  
Acesso remoto permite que você adicione servidores com endereços IPv4 e IPv6 com uma implantação que foi originalmente configurada somente com endereços IPv4. Esses servidores são adicionados como servidores somente IPv4 e seus endereços IPv6 são ignorados pelo DirectAccess; Consequentemente, sua organização não pode tirar proveito dos benefícios de conectividade IPv6 nativa com esses novos servidores.  
  
Para transformar a implantação a uma implantação de IPv4 + IPv6 e aproveitar os recursos do IPv6 nativos, você deverá reinstalar o DirectAccess. Para manter a conectividade de cliente em toda a reinstalação consulte transição de um somente com IPv4 para uma implantação somente com IPv6 usando duas implantações do DirectAccess.  
  
> [!NOTE]  
> Assim como acontece com uma rede somente IPv4, em uma rede IPv4 + IPv6 mista, o endereço do servidor DNS que é usado para resolver as solicitações do cliente DNS deve ser configurado com o DNS64 que é implantado nos próprios servidores de acesso remoto e não com um DNS corporativo.  
  
### <a name="TransitionMixedtoIPv6"></a>Fazer a transição de IPv4 + IPv6 a uma rede corporativa somente IPv6  
O DirectAccess permite que você adicione pontos de entrada somente IPv6 somente se o primeiro servidor de acesso remoto na implantação tinha originalmente em ambos os IPv4 e os endereços IPv6 ou apenas um endereço IPv6. Ou seja, você não pode fazer a transição de uma rede somente IPv4 para uma rede somente IPv6 em uma única etapa sem reinstalar o DirectAccess. Para fazer a transição diretamente de uma rede somente IPv4 para uma rede somente IPv6, consulte transição de um somente com IPv4 para uma implantação somente com IPv6 usando duas implantações do DirectAccess.  
  
Depois de concluir a transição de uma implantação somente IPv4 para uma implantação IPv4 + IPv6, você pode fazer a transição para uma rede somente IPv6. Durante e após a transição, observe o seguinte:  
  
-   Se todos os servidores back-end somente IPv4 permanecerem na rede corporativa, eles não estarão acessíveis aos clientes que se conectam por meio de pontos de entrada somente IPv6.  
  
-   Ao adicionar pontos de entrada somente IPv6 a uma implantação de IPv4 + IPv6, o DNS64 e NAT64 não serão habilitada nos novos servidores. Clientes que se conectam a esses pontos de entrada serão automaticamente configurados para usar os servidores DNS corporativos.  
  
-   Se você precisar excluir os endereços IPv4 de um servidor implantado, remova o servidor de implantação do DirectAccess, remova o endereço de rede corporativa IPv4 e adicioná-lo novamente para a implantação.  
  
Para dar suporte à conectividade de cliente à rede corporativa, você deve garantir que o servidor de local de rede pode ser resolvido pelo DNS corporativo para seu endereço IPv6. Um endereço IPv4 adicional pode ser bem definido, mas não é necessário.  
  
### <a name="DualDeployment"></a>Fazer a transição de um somente com IPv4 para uma implantação somente com IPv6 usando duas implantações do DirectAccess  
A transição de um somente com IPv4 para uma rede corporativa somente IPv6 não pode ser feita sem reinstalar a implantação do DirectAccess. Para manter a conectividade de cliente durante a transição, você pode usar outra implantação do DirectAccess. Implantação dupla é necessária quando o primeiro estágio de transição for concluída (rede somente IPv4, atualizado para o IPv4 + IPv6) e você pretende se preparar para uma transição futuras para um somente com IPv6 rede corporativa orto tirar proveito dos benefícios de conectividade de IPv6 nativos. A implantação dupla é descrita nas etapas gerais a seguir:  
  
1.  Instale uma segunda implantação do DirectAccess. Você pode instalar o DirectAccess em novos servidores, ou remover servidores da primeira implantação e usá-los para a segunda implantação.  
  
    > [!NOTE]  
    > Ao instalar uma implantação do DirectAccess adicional junto com uma atual, certifique-se de que não há dois pontos de entrada compartilham o mesmo prefixo de cliente.  
    >   
    > Se você instalar o DirectAccess usando o Assistente de introdução ou com o cmdlet `Install-RemoteAccess`, acesso remoto define automaticamente o prefixo do cliente do primeiro ponto de entrada na implantação para um valor padrão de < subrede IPv6\_prefixo >: 1000::/ / 64. Se necessário, você deve alterar o prefixo.  
  
2.  Remova os grupos de segurança de cliente escolhido desde a primeira implantação.  
  
3.  Adicione os grupos de segurança do cliente para a segunda implantação.  
  
    > [!IMPORTANT]  
    > Para manter a conectividade de cliente em todo o processo, você deve adicionar os grupos de segurança para a segunda implantação imediatamente após removê-los a partir da primeira. Isso garante que os clientes não serão atualizados com duas ou zero GPOs do DirectAccess. Os clientes começará a usar a segunda implantação depois que eles recuperar e atualizar seu GPO do cliente.  
  
4.  Opcional: Remova os pontos de entrada do DirectAccess da primeira implantação e adicionar esses servidores como novos pontos de entrada no segundo.  
  
Quando você tiver concluído a transição, você pode desinstalar a primeira implantação do DirectAccess. Ao desinstalar, os seguintes problemas podem ocorrer:  
  
-   Se a implantação foi configurada para dar suporte apenas a clientes em computadores móveis, o filtro WMI será excluído. Se os grupos de segurança do cliente da segunda implantação incluem computadores desktop, o GPO do cliente DirectAccess não filtrará computadores desktop e pode causar problemas neles. Se for necessário um filtro de computadores móveis, recrie-o seguindo as instruções [criar filtros de WMI para o GPO](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Se ambas as implantações foram originalmente criadas no mesmo domínio do Active Directory, o DNS de investigação que aponta para o host local será excluída e pode causar problemas de conectividade de cliente de entrada. Por exemplo, os clientes podem se conectar usando IP-HTTPS em vez de Teredo ou alternar entre os pontos de entrada multissite do DirectAccess. Nesse caso, você deve adicionar a seguinte entrada DNS ao DNS corporativo:  
  
    -   Zone: domain name  
  
    -   Name: directaccess-corpConnectivityHost  
  
    -   Endereço IP::: 1  
  
    -   Digite: AAAA  
  
  
  


