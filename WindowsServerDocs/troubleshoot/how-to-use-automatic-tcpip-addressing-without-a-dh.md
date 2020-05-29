---
title: Como usar o endereçamento TCP/IP automático sem um servidor DHCP
description: Apresente como usar o endereçamento TCP/IP automático sem um servidor DHCP.
ms.date: 5/26/2020
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.reviewer: robsmi
ms.openlocfilehash: 8fbde77381141b76959f70e824eac22ee2121fa3
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150184"
---
# <a name="how-to-use-automatic-tcpip-addressing-without-a-dhcp-server"></a>Como usar o endereçamento TCP/IP automático sem um servidor DHCP

Este artigo descreve como usar o endereçamento do protocolo TCP/IP (protocolo de controle de transmissão) automático sem que um servidor DHCP esteja presente na rede. As versões do sistema operacional listadas na seção "aplica-se a" deste artigo têm um recurso chamado APIPA (endereçamento IP privado automático). Com esse recurso, um computador com Windows pode atribuir a si mesmo um endereço IP (Internet Protocol) caso um servidor DHCP não esteja disponível ou não exista na rede. Esse recurso torna a configuração e o suporte a uma pequena rede local (LAN) que executa TCP/IP com menos dificuldade.

## <a name="more-information"></a>Mais informações

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/help/322756) em caso de problemas.

Um computador baseado no Windows configurado para usar o DHCP pode automaticamente atribuir a si próprio um endereço IP (Internet Protocol) se um servidor DHCP não estiver disponível. Por exemplo, isso pode ocorrer em uma rede sem um servidor DHCP, ou em uma rede se um servidor DHCP estiver temporariamente inativo para manutenção.

A IANA (Internet Assigned Numbers Authority) reservou o 169.254.0.0-169.254.255.255 para o endereçamento IP privado automático. Como resultado, o APIPA fornece um endereço que está garantido para não entrar em conflito com endereços roteáveis.

Depois que o adaptador de rede tiver sido atribuído um endereço IP, o computador poderá usar TCP/IP para se comunicar com qualquer outro computador que esteja conectado à mesma LAN e que também esteja configurado para APIPA ou que tenha o endereço IP definido manualmente como 169.254. x. y (em que x. y é o intervalo de endereços do cliente) com uma máscara de sub-rede de 255.255.0.0. Observe que o computador não pode se comunicar com computadores em outras sub-redes ou com computadores que não usam o endereçamento IP privado automático. O endereçamento IP privado automático é habilitado por padrão.

Talvez você queira desabilitá-lo em qualquer um dos seguintes casos:

- Sua rede usa roteadores.

- Sua rede está conectada à Internet sem um servidor NAT ou de proxy.

A menos que você tenha desabilitado mensagens relacionadas ao DHCP, as mensagens DHCP fornecem notificações quando você altera entre o endereçamento DHCP e o endereçamento IP privado automático. Se o sistema de mensagens DHCP for desabilitado acidentalmente, você poderá ativar as mensagens DHCP novamente alterando o valor do valor PopupFlag na seguinte chave do registro de 00 para 01:  
`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP` 

Observe que você deve reiniciar o computador para que a alteração entre em vigor. Você também pode determinar se o computador está usando o APIPA usando a ferramenta winipcfg no Windows Millennium Edition, no Windows 98 ou no Windows 98 Second Edition:

Clique em Iniciar, executar, digite "winipcfg" (sem as aspas) e, em seguida, clique em OK. Clique em mais informações. Se a caixa endereço de configuração automática de IP contiver um endereço IP dentro do intervalo 169.254. x. x, o endereçamento IP privado automático será habilitado. Se a caixa de endereço IP existir, o endereçamento IP privado automático não estará habilitado no momento.
Para o Windows 2000, Windows XP ou Windows Server 2003, você pode determinar se o computador está usando APIPA usando o comando IPconfig em um prompt de comando:

Clique em Iniciar, executar, digite "cmd" (sem as aspas) e clique em OK para abrir uma janela de linha de comando do MS-DOS. Digite "ipconfig/all" (sem as aspas) e, em seguida, pressione a tecla ENTER. Se a linha ' autoconfiguração habilitada ' disser "Sim" e o ' endereço IP de configuração automática ' for 169.254. x. y (em que x. y é o identificador exclusivo do cliente), o computador estará usando APIPA. Se a linha ' autoconfiguração habilitada ' disser "não", o computador não está usando APIPA no momento.
Você pode desabilitar o endereçamento IP privado automático usando qualquer um dos métodos a seguir.

Você pode configurar as informações de TCP/IP manualmente, o que desabilita completamente o DHCP. Você pode desabilitar o endereçamento IP privado automático (mas não o DHCP) editando o registro. Você pode fazer isso adicionando a entrada de Registro DWORD "IPAutoconfigurationEnabled" com um valor de 0x0 para a seguinte chave do registro para Windows Millennium Edition, Windows98 ou Windows 98 Second Edition:`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

Para o Windows 2000, Windows XP e Windows Server 2003, o APIPA pode ser desabilitado adicionando a entrada de Registro DWORD "IPAutoconfigurationEnabled" com um valor de 0x0 para a seguinte chave do registro:  
`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\<Adapter GUID>`  
> [!NOTE]
> A subchave do **GUID do adaptador** é um GUID (identificador global exclusivo) para o adaptador de LAN do computador.

A especificação de um valor de 1 para a entrada DWORD IPAutoconfigurationEnabled habilitará o APIPA, que é o estado padrão quando esse valor é omitido do registro.

## <a name="examples-of-where-apipa-may-be-useful"></a>Exemplos de onde o APIPA pode ser útil

### <a name="example-1-no-previous-ip-address-and-no-dhcp-server"></a>Exemplo 1: nenhum endereço IP anterior e nenhum servidor DHCP

Quando o computador baseado no Windows (configurado para DHCP) estiver sendo inicializado, ele difundirá três ou mais mensagens de "descoberta". Se um servidor DHCP não responder depois que várias mensagens de descoberta forem transmitidas, o computador Windows atribuirá a si mesmo um endereço de classe B (APIPA). Em seguida, o computador com Windows exibirá uma mensagem de erro para o usuário do computador (fornecendo a ele nunca foi atribuído um endereço IP de um servidor DHCP no passado). O computador com Windows enviará uma mensagem de descoberta a cada três minutos em uma tentativa de estabelecer comunicações com um servidor DHCP.

### <a name="example-2-previous-ip-address-and-no-dhcp-server"></a>Exemplo 2: endereço IP anterior e nenhum servidor DHCP

O computador verifica o servidor DHCP e, se nenhum for encontrado, será feita uma tentativa de entrar em contato com o gateway padrão. Se o gateway padrão responder, o computador Windows manterá o endereço IP concedido anteriormente. No entanto, se o computador não receber uma resposta do gateway padrão ou se nenhum for atribuído, ele usará o recurso de endereçamento IP privado automático para atribuir a si mesmo um endereço IP. Uma mensagem de erro é apresentada ao usuário e as mensagens de descoberta são transmitidas a cada 3 minutos. Quando um servidor DHCP entra na linha, uma mensagem é gerada informando que as comunicações foram restabelecidas com um servidor DHCP.

### <a name="example-3-lease-expires-and-no-dhcp-server"></a>Exemplo 3: a concessão expira e nenhum servidor DHCP

O computador baseado no Windows tenta restabelecer a concessão do endereço IP. Se o computador com Windows não encontrar um servidor DCHP, ele atribuirá a si mesmo um endereço IP depois de gerar uma mensagem de erro. Em seguida, o computador transmite quatro mensagens de descoberta e, após a cada 5 minutos, ele repete o procedimento inteiro até que um servidor DHCP venha a ficar online. Uma mensagem é gerada informando que as comunicações foram restabelecidas com o servidor DHCP.
