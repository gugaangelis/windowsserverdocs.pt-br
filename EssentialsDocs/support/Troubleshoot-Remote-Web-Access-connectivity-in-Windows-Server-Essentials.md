---
title: Solucionar problemas de conectividade do Acesso Remoto via Web no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02563f56cad211d5aa842d7607fa9f6005f18026
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470161"
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Solucionar problemas de conectividade do Acesso Remoto via Web no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Normalmente, o Windows Server Essentials pode configurar automaticamente um roteador de banda larga se o roteador for um dispositivo UPnP certificado e se a configuração de UPnP estiver habilitada no roteador.

## <a name="possible-issues"></a>Possíveis problemas
 Você pode encontrar os seguintes problemas de conectividade no Acesso Remoto via Web:

-   Seu roteador não está ativado ou não está conectado à sua rede.

-   A configuração UPnP está desativada no seu roteador.

-   Seu roteador talvez não dê suporte total ao padrão UPnP. A Microsoft mantém uma lista de roteadores que trabalham com os sistemas operacionais Windows. Para ver uma lista de roteadores (incluindo roteadores sem fio) compatíveis com o Windows Server Essentials, visite o [Centro de Compatibilidade do Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).

## <a name="possible-fixes"></a>Soluções possíveis
 Os procedimentos a seguir podem solucionar esses problemas:

- Verifique se o seu roteador está ativado e funcionando corretamente.

- Assegure que o adaptador de rede local esteja diretamente conectado ao seu roteador ou a um comutador que esteja conectado ao seu roteador.

- Confirme que o dispositivo de banda larga que se conecta a seu ISP (provedor de serviço de Internet) está ligado e funcionando adequadamente; confirme também que seu roteador está conectado ao dispositivo de banda larga.

- Ative a configuração UPnP no roteador. Conecte-se à página da Web de configuração do seu roteador e ative a configuração de UPnP. Para obter mais informações sobre como efetuar logon no seu roteador e como ativar a configuração de UPnP, consulte a documentação para seu roteador. Depois de ativar a configuração de UPnP, execute o assistente para ativar Acesso via Web remota novamente para configurar o roteador.

- Se o seu roteador não dá suporte total ao padrão UPnP, ele não pode ser configurado automaticamente. Você deve configurar manualmente o seu roteador ou adquirir um roteador que dê suporte ao padrão UPnP.

   Para configurar manualmente o seu roteador, conclua as tarefas a seguir:

  - Criar reserva de endereço IP para seu servidor do Windows Server Essentials.

     Antes de configurar manualmente o roteador para encaminhar as portas requeridas para o Windows Server Essentials, você deve configurar uma reserva de protocolo DHCP para o servidor que está executando o Windows Server Essentials no roteador. Isso garante que o endereço IP que você estará encaminhando às portas não mude.

     Para obter informações sobre como configurar manualmente uma reserva DHCP para seu servidor em seu roteador, consulte a documentação do fabricante do seu roteador.

  - Configure o encaminhamento de porta no seu roteador para as portas a seguir:

    |Serviço ou protocolo|Porta|
    |-------------------------|----------|
    |HTTP|TCP 80|
    |HTTPS|TCP 443|

    Para obter informações sobre como configurar manualmente o encaminhamento de porta em seu roteador, consulte a documentação do fabricante.

    Uma página típica de configuração de roteador inclui uma tabela com a aparência demonstrada a seguir.

  > [!NOTE]
  >  Nessa tabela, o endereço IP do computador que executa o Windows Server Essentials é 192.168.0.100. Você deve determinar o endereço IP do seu computador e substituir esse endereço IP por aquele mostrado na tabela.

  |Endereço IP|Protocolo (TCP/UDP)|Agenda|Filtro de entrada|
  |----------------|---------------------------|--------------|--------------------|
  |192.168.0.100|TCP 80|Sempre|Permitir todos|
  |192.168.0.100|TCP 443|Sempre|Permitir todos|

   Depois de configurar manualmente o roteador, execute o assistente para ativar Acesso via Web remoto, assegurando que você selecione a opção **ignorar a instalação do roteador** na página **introdução** .

- Se o seu roteador não der total suporte ao padrão UPnP, adquira um novo.

> [!TIP]
>  Certifique-se que o seu roteador tem o firmware de BIOS mais recente instalado. Você pode atualizar o firmware de BIOS para seu roteador na página da Web de configuração para ele. Para obter mais informações, veja a documentação do seu roteador. Após seu roteador ser atualizado, execute o Assistente de Configuração do Acesso em Qualquer Local.

## <a name="see-also"></a>Veja também

-   [Usar o Acesso via Web Remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Acesso via Web Remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Acesso em Qualquer Local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

