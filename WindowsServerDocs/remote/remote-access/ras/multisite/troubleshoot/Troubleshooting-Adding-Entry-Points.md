---
title: Solução de problemas da adição de pontos de entrada
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2cb0566dc2cdc68c4653f85c06869585bc6454ef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313832"
---
# <a name="troubleshooting-adding-entry-points"></a>Solução de problemas da adição de pontos de entrada

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações sobre como solucionar problemas relacionados ao comando `Add-DAEntryPoint`. Para confirmar que o erro recebido está relacionado à adição de um ponto de entrada, procure a ID de evento 10067 no Log de Eventos do Windows.  
  
## <a name="missing-remoteaccessserver-parameter"></a>Parâmetro RemoteAccessServer ausente  
**Erro recebido**. Você deve fornecer um valor para o parâmetro RemoteAccessServer.  
  
**Causa**  
  
Ao adicionar um novo ponto de entrada a uma implantação multissite, você precisa especificar o parâmetro *RemoteAccessServer*, que é o nome do servidor que você deseja adicionar como o novo ponto de entrada.  
  
**Solução**  
  
Execute o comando especificando o parâmetro *RemoteAccessServer* com o nome do servidor a ser adicionado como um ponto de entrada.  
  
## <a name="remote-access-is-not-configured"></a>O Acesso Remoto não está configurado  
**Erro recebido**. O acesso remoto não está configurado em < server_name >. Especifique o nome de um servidor que pertença a uma implantação multissite.  
  
**Causa**  
  
O Acesso Remoto não está configurado no computador especificado pelo parâmetro *ComputerName* ou no computador em que você executa o comando.  
  
Ao adicionar um novo ponto de entrada a uma implantação multissite, você precisa especificar dois parâmetros: *ComputerName* e *RemoteAccessServer*. *ComputerName* é o nome de um servidor que já faz parte da implantação multissite, enquanto *RemoteAccessServer* é o nome do servidor que você deseja adicionar como o novo ponto de entrada. Se a execução for feita em um computador que faz parte da implantação multissite, o parâmetro ComputerName não será necessário.  
  
**Solução**  
  
Execute o comando especificando o parâmetro *ComputerName* com o nome do servidor que já está configurado como parte da implantação multissite, ou execute o comando em um computador que faça parte da implantação multissite.  
  
## <a name="multisite-not-enabled"></a>Funcionalidade multissite não habilitada  
**Erro recebido**. Você deve habilitar uma implantação multissite antes de executar esta operação. Para fazer isso, use o cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
A funcionalidade multissite não está habilitada no servidor especificado pelo parâmetro *ComputerName*. Para adicionar um novo ponto de entrada a uma implantação do Acesso Remoto, primeiro você precisa habilitar essa funcionalidade.  
  
**Solução**  
  
Habilite a funcionalidade multissite usando o cmdlet `Enable-DaMultiSite`. Para obter mais informações, consulte [implantar o acesso remoto multissite](https://technet.microsoft.com/library/hh831664.aspx).  
  
## <a name="ipv6-prefix-issues"></a>Problemas com o prefixo IPv6  
  
-   **Problema 1**  
  
    **Erro recebido**. O IPv6 é implantado na rede interna, mas você não especificou um prefixo IPv6 do cliente.  
  
    **Causa**  
  
    O IPv6 está implantado na rede corporativa e um prefixo IP-HTTPS é necessário. No entanto, não foi especificado um prefixo no parâmetro *ClientIPv6Prefix* para o novo ponto de entrada.  
  
    **Solução**  
  
    1.  Atribua um prefixo IP-HTTPS exclusivo ao novo ponto de entrada e assegure-se de que os pacotes direcionados para um endereço IP sob esse prefixo serão roteados para o servidor que você está adicionando.  
  
    2.  Execute o cmdlet `Add-DAEntryPoint` e especifique o prefixo IP-HTTPS no parâmetro *ClientIPv6Prefix*.  
  
-   **Problema 2**  
  
    **Erro recebido**. O prefixo IPv6 do cliente já está em uso por outro ponto de entrada. Especifique um valor alternativo.  
  
    **Causa**  
  
    O prefixo IP-HTTPS especificado no parâmetro *ClientIPv6Prefix* já está sendo usado por outro ponto de entrada.  
  
    **Solução**  
  
    1.  Atribua um prefixo IP-HTTPS exclusivo ao novo ponto de entrada e assegure-se de que os pacotes direcionados para um endereço IP sob esse prefixo serão roteados para o servidor que você está adicionando.  
  
    2.  Execute o cmdlet `Add-DAEntryPoint` e especifique o prefixo IP-HTTPS no parâmetro *ClientIPv6Prefix*.  
  
## <a name="connectto-address"></a>Endereço ConnectTo  
**Erro recebido**. O endereço (< connect_to_address >) ao qual os clientes DirectAccess se conectam no servidor RemoteAccess é o mesmo que o endereço do servidor do local de rede. Especifique um valor alternativo.  
  
**Causa**  
  
O endereço ConnectTo e o endereço do servidor de local de rede são iguais.  
  
**Solução**  
  
Deve ser possível resolver o endereço ConnectTo pela Internet para que os computadores cliente possam se conectar por IP-HTTPS. Deve ser possível resolver o endereço do servidor de local de rede pela rede corporativa, mas não pela Internet. Certifique-se de que o endereço do servidor de local de rede e o endereço ConnectTo não sejam iguais. Selecione endereços diferentes e tente novamente.  
  
## <a name="directaccess-or-vpn-already-installed"></a>VPN ou DirectAccess já instalado  
**Erro recebido**. Foi detectada uma instalação de VPN no servidor < server_name >. Especifique um servidor alternativo que não tenha Acesso Remoto instalado ou remova a configuração de VPN do servidor.  
  
Ou  
  
O acesso remoto já está instalado no servidor < server_name >. Especifique um servidor alternativo que não esteja executando o DirectAccess ou remova a configuração do DirectAccess existente do servidor.  
  
**Causa**  
  
O DirectAccess ou a VPN já estão configurados no novo ponto de entrada. Não é possível adicionar um ponto de entrada configurado a uma implantação multissite.  
  
**Solução**  
  
Para adicionar um servidor a uma implantação multissite, você deve instalar a função Acesso Remoto no servidor, mas o DirectAccess e a VPN não devem estar configurados.  
  
Execute o comando de modo que o servidor especificado no parâmetro *RemoteAccessServer* não tenha o DirectAccess ou a VPN configurados.  
  
## <a name="ipsec-root-certificate"></a>Certificado raiz IPsec  
**Erro recebido**. O certificado raiz IPsec configurado não pode ser localizado no servidor < server_name >.  
  
**Causa**  
  
O certificado da autoridade de certificação (AC) raiz ou intermediária que emite certificados de computador não foi encontrado no servidor que você está tentando adicionar à implantação.  
  
**Solução**  
  
No Console de Gerenciamento de Acesso Remoto, na **Etapa 2: Servidor de Acesso Remoto**, clique em **Editar** e, na página **Autenticação**, sob **Usar certificados de computador**, verifique se o certificado selecionado é válido. Se o certificado for válido, verifique se ele está localizado sob a AC raiz confiável no servidor que você deseja adicionar e tente novamente.  
  
> [!NOTE]  
> O certificado deve ser o mesmo certificado com a mesma impressão digital.  
  
Se o certificado não for válido, selecione um certificado válido que esteja configurado como a AC raiz confiável em todos os servidores de acesso remoto.  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>Mistura de pontos de entrada IPv6 e IPv4  
Quando o DirectAccess é instalado pela primeira vez, o adaptador de rede interno é inspecionado para determinar se a rede contém apenas endereços IPv4 (rede somente IPv4), endereços IPv6 e IPv4, ou apenas endereços IPv6 (rede somente IPv6). Essa informação é usada para determinar o tipo de implantação (Somente IPv4, IPv6+IPv4 ou Somente IPv6).  
  
-   **Problema 1**  
  
    **Aviso recebido**. O servidor de acesso remoto que está sendo adicionado é configurado com endereços IPv4 e IPv6. Esta é uma implantação exclusiva para IPv4, e o Remote Access irá ignorar os endereços IPv6.  
  
    **Causa**  
  
    Quando esta implantação foi instalada pela primeira vez, foi detectado que a rede interna era somente IPv4. Em uma implantação multissite, pressupõe-se que pontos de entrada diferentes estejam localizados em sub-redes diferentes com características diferentes. Portanto, embora a implantação esteja configurada como somente IPv4, ela pode conter um ponto de entrada localizado em uma sub-rede IPv6+IPv4. No entanto, embora o ponto de entrada seja adicionado à implantação, o DirectAccess irá ignorar os endereços IPv6 configurados na interface interna do novo ponto de entrada.  
  
    **Solução**  
  
    Se a rede interna inteira estiver configurada com endereços IPv6 e IPv4, considere a possibilidade de mudar para uma implantação IPv6+IPv4 a fim de aproveitar os benefícios das tecnologias IPv6. Consulte "transição de um IPv4 puro para uma rede corporativa IPv6 + IPv4" na [etapa 3: planejar a implantação](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d)multissite.  
  
-   **Problema 2**  
  
    **Erro recebido**. Os adaptadores de rede internos dos servidores de acessar remotos nesta implantação multissite são configurados com endereços IPv4. Todos os pontos de entrada adicionados à implantação devem ter um endereço IPv4 interno.  
  
    **Causa**  
  
    Quando esta implantação foi instalada pela primeira vez, foi detectado que a rede interna era somente IPv4. O Acesso Remoto detectou que o ponto de entrada que você está tentando adicionar está configurado somente com endereços IPv6 na rede interna. Isso não é permitido em uma implantação somente IPv4.  
  
    **Solução**  
  
    Se a rede inteira já estiver configurada com endereços IPv6, recomenda-se mudar para uma implantação IPv6+IPv4 ou somente IPv6. Consulte "planejar a transição para o IPv6 quando o acesso remoto multissite for implantado".  
  
-   **Problema 3**  
  
    **Erro recebido**. Esse ponto de entrada está localizado em uma rede IPv4, mas os pontos de entrada anteriores estão localizados em uma rede IPv6. Conecte esse ponto de entrada à rede IPv6 antes de adicioná-lo à mesma implantação multissite.  
  
    **Causa**  
  
    Quando esta implantação foi instalada pela primeira vez, foi detectado que a rede interna era IPv6+IPv4 ou somente IPv6. Foi detectado que somente endereços IPv4 estão configurados na rede interna no novo ponto de entrada que você está tentando adicionar. Isso não é permitido em implantações IPv6+IPv4 ou somente IPv6.  
  
    **Solução**  
  
    Configure o novo ponto de entrada com endereços IPv6 e adicione-o à implantação multissite.  
  
-   **Problema 4**  
  
    **Aviso recebido**. O adaptador de rede interno no servidor de acesso remoto não está configurado com um endereço IPv4. O DNS64 e o NAT64 não serão configurados neste servidor. Clientes do DirectAccess somente podem acessar servidores IPv6 internos.  
  
    **Causa**  
  
    Quando esta implantação foi instalada pela primeira vez, foi detectado que a rede interna era IPv6+IPv4. Nesse modo de implantação, o DNS64 e o NAT64 são habilitados para permitir que os computadores cliente acessem computadores da rede interna que estejam configurados somente com endereços IPv4.  
  
    Ao adicionar o novo ponto de entrada, o Acesso Remoto detectou que a interface interna do novo computador tem somente endereços IPv6. Para configurar o DNS64 e o NAT64, é necessário um endereço IPv4 para rotear os pacotes do servidor de acesso remoto para o computador somente IPv4. Como esse IP não existe no novo computador, o NAT64 e o DNS64 não serão configurados no servidor de acesso remoto. Portanto, os computadores cliente que acessarem a rede corporativa pelo DirectAccess usando esse ponto de entrada não conseguirão acessar os servidores somente IPv4 na rede interna. Para obter informações sobre como fazer a transição para uma rede IPv6 + IPv4 ou uma rede somente IPv6, consulte "planejar a transição para o IPv6 quando o acesso remoto multissite for implantado".  
  
    **Solução**  
  
    Adicione um endereço IPv4 ao novo servidor de acesso remoto para garantir o funcionamento correto do DNS64 e do NAT64.  
  
## <a name="domain-issues-with-the-servergponame"></a>Problemas de domínio com ServerGpoName  
  
-   **Problema 1**  
  
    **Erro recebido**. O domínio especificado no parâmetro ServerGpoName < server_GPO > não existe. Em vez disso, especifique o < de domínio domain_name >.  
  
    **Causa**  
  
    Não foi encontrada a parte do nome de domínio que consta do nome do GPO de servidor enviado pelo administrador.  
  
    **Solução**  
  
    Verifique se você digitou o nome do domínio corretamente. Se o nome do domínio estiver escrito certo, tente novamente usando o FQDN (nome de domínio totalmente qualificado).  
  
-   **Problema 2**  
  
    **Erro recebido**. O GPO do servidor deve estar localizado no domínio do servidor de acesso remoto. Especifique o < de domínio domain_name > no parâmetro ServerGpoName.  
  
    **Causa**  
  
    O domínio do GPO de servidor não é igual ao domínio ao qual pertence o servidor de acesso remoto.  
  
    **Solução**  
  
    O GPO de servidor deve estar localizado no mesmo domínio do servidor de acesso remoto. Use o nome do domínio do servidor para o GPO de servidor e tente novamente.  
  
## <a name="split-brain-dns"></a>DNS com partição de rede  
**Aviso recebido**. A entrada NRPT para o sufixo DNS < DNS_suffix > contém o nome público usado pelos computadores cliente para se conectar ao servidor de acesso remoto. Adicione o nome < connect_to_address > como uma isenção na NRPT.  
  
**Causa**  
  
Você está usando DNS com partição de rede. Para permitir que os clientes se conectem usando IP-HTTPS, é recomendável verificar se o endereço ConnectTo selecionado está isento das regras de NRPT.  
  
**Solução**  
  
Se você tiver uma implantação multissite, verifique se todos os endereços ConnectTo dos diversos pontos de entrada estão isentos das regras de NRPT.  
  
Para isentar um endereço das regras de NRPT:  
  
1.  No Console de Gerenciamento de Acesso Remoto, na **Etapa 3: Servidores de Infraestrutura**, clique em **Editar**.  
  
2.  No **Assistente de Instalação do Servidor de Infraestrutura**, na página **DNS**, clique duas vezes na tabela para especificar um novo sufixo de nome.  
  
3.  Na caixa de diálogo **Endereços do Servidor DNS**, no sufixo DNS, especifique o endereço ConnectTo do ponto de entrada e clique em **Aplicar**.  
  
Quando você adiciona sufixos de nome sem especificar um endereço de servidor, o sufixo é tratado como uma isenção de NRPT.  
  
## <a name="saving-server-gpo-settings"></a>Salvando configurações de GPO de servidor  
**Erro recebido**. Ocorreu um erro ao salvar as configurações de acesso remoto ao GPO < GPO_name >.  
  
Para solucionar esse erro, consulte Salvando configurações de GPO do servidor na [solução de problemas habilitando multissite](https://technet.microsoft.com/library/jj591658.aspx).  
  
## <a name="gpo-updates-cannot-be-applied"></a>Não é possível aplicar atualizações de GPO  
**Aviso recebido**. As atualizações de GPO não podem ser aplicadas em < server_name >. As alterações só terão efeito após a próxima atualização de política.  
  
**Causa**  
  
Ocorreu um erro na tentativa de atualizar políticas no computador especificado. Com isso, as alterações feitas só terão efeito após a próxima atualização de política.  
  
**Solução**  
  
Para forçar uma atualização de política, execute `gpupdate /force` no computador especificado.  
  


