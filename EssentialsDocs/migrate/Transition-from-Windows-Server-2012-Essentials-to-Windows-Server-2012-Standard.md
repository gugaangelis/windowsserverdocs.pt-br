---
title: "Transição do Windows Server Essentials para Windows Server 2012 Standard"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d2005b72adede72b718fa5b49b93435f5fbac1bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Transição do Windows Server Essentials para Windows Server 2012 Standard

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials dá suporte a até 25 usuários e 50 dispositivos. Quando sua empresa precisa de exceder o limite, você pode executar uma transição de licença in-loco do Windows Server Essentials para o Windows Server 2012 Standard permaneça licença compatível.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Como a transição afeta usuários e dispositivos limita  
 Depois que você faça a transição para o Windows Server 2012 Standard, os limites de conta e seus dispositivos do usuário são removidos, mas os recursos que são exclusivos para Windows Server Essentials (por exemplo, o painel, acesso via Web remoto e backup do computador cliente), ainda permanecem disponíveis. No entanto, limitações técnicas para que esses recursos dão suporte a um máximo de 75 contas de usuário e 75 dispositivos. Se for necessário adicionar mais de 75 contas de usuário ou dispositivos, você deve desativar os recursos do Windows Server Essentials e usar as ferramentas do Windows Server 2012 Standard nativas para gerenciar dispositivos e contas de usuário.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard requer uma licença de acesso para cliente (CAL) para cada usuário ou dispositivo em seu ambiente. Isso é diferente do Windows Server Essentials, que não usa o modelo de CAL e não vem com qualquer CALs.  Durante a transição do Windows Server Essentials para o Windows Server 2012 Standard, você precisará comprar o número apropriado e o tipo de CALs para seu ambiente (a maioria dos clientes adquirir CALs de usuário).  
  
## <a name="before-the-transition"></a>Antes da transição  
  
-   Antes de fazer a transição do Windows Server Essentials para Windows Server 2012 Standard, você deve fazer o backup completo os dados do servidor.  
  
    > [!IMPORTANT]
    >  Sem um backup completo do servidor, você não poderá restaurar o servidor para o estado em que estava antes da transição.  
  
-   Além disso, certifique-se de que você leia e entenda o contrato de licença de usuário final (EULA) para Windows Server 2012 Standard. Para exibir o EULA:  
  
    1.  Abra uma janela de comando como administrador.  
  
    2.  Execute o seguinte comando:  
  
         **DISM / online/definido-edition: ServerStandard /geteula: caminho do eula**  
  
         Onde **caminho de eula** representa o local ao qual você deseja salvar o arquivo de EULA. Por exemplo; C:\ws8std_eula.rtf.  Certifique-se de usar. rtf como a extensão de nome de arquivo.  
  
    3.  Abra o local onde você salvou o arquivo e, em seguida, clique duas vezes no arquivo para abri-lo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transição para o Windows Server 2012 Standard  
 Depois que você decidiu fazer a transição do Windows Server Essentials para o Windows Server 2012 Standard, completa essas duas etapas:  
  
1.  Compre uma licença para o Windows Server 2012 Standard e o número apropriado de usuário e/ou dispositivo licenças de acesso para cliente para seu ambiente.  
  
     Você pode adquirir uma licença para o Windows Server 2012 Standard de uma tomada de varejo, um distribuidor, ou com a Ajuda de um [Microsoft Partner](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Se você adquiriu o Windows Server 2012 Standard inicialmente e exercidos seus direitos de downgrade para instalar uma de suas duas instâncias virtuais como Windows Server Essentials, você não precisa adquirir algum outro item.  
    >   
    >  Se você comprar o Windows Server 2012 Standard através do canal de licenciamento por Volume, você pode baixar uma imagem ISO e uma chave do produto para o Windows Server 2012 Standard do Volume Licensing Service Center (VLSC).  
    >   
    >  Se você adquirir Windows Server 2012 Standard de todos os outros canais pode baixar uma imagem ISO e uma chave de produto de avaliação do Windows Server Essentials do [Centro de avaliação TechNet](https://technet.microsoft.com/evalcenter/jj659306.aspx). Realizar a transição, conforme descrito na próxima etapa converterá o produto de avaliação em um produto totalmente licenciado e com suporte.  
  
2.  Abra o Windows PowerShell como administrador e execute o comando a seguir.  
  
     **DISM / online/definido-edition: ServerStandard /accepteula /productkey:** *chave do produto*  
  
     Onde *chave do produto* é a chave do produto para sua cópia do Windows Server 2012 Standard.  
  
     O servidor for reiniciado para concluir o processo de transição.  
  
 Após a transição, os recursos do Windows Server Essentials permanecem no servidor e têm suporte para até 75 usuários e 75 dispositivos. Se você exceder qualquer um desses limites, você deve usar as ferramentas do Windows Server 2012 Standard nativas para gerenciar dispositivos e contas de usuário.  
  
 Além disso, depois que você faça a transição para o Windows Server 2012 Standard, os recursos de mídia do Windows Server Essentials não estão mais disponíveis. Isso inclui os recursos de mídia do acesso via Web remoto e as configurações de mídia no painel.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Desativar recursos do Windows Server Essentials  
 Se você não precisa mais o Windows Server Essentials Dashboard ou outros recursos de valor agregado para gerenciar o servidor, você pode desativar os recursos e removê-los do seu servidor.  
  
 O **desativar o Assistente de recursos do Windows Server Essentials** ajuda você a desinstala os recursos. Ele também limpa o servidor de arquivos que foram criados pelo software de servidor Windows Server Essentials.  Algumas operações de limpeza são executadas imediatamente, enquanto outros são iniciados depois que o servidor for reiniciado.  
  
 O **desativar o Assistente de recursos do Windows Server Essentials** requer que você desinstalar manualmente todos os suplementos antes de concluir o assistente. Para ver uma lista de suplementos instalados, abra a página de aplicativo no painel. O assistente irá alertá-lo a se ele detecta suplementos instalados e pede para desinstalá-los.  
  
 O **desativar o Assistente de recursos do Windows Server Essentials** permite que você escolha se deseja manter os arquivos de backup para o cliente computadores após desativar os recursos do Windows Server Essentials.  
  
 Há duas maneiras de executar o **desativar o Assistente de recursos do Windows Server Essentials** do painel:  
  
#### <a name="from-the-alert"></a>Do alerta  
  
1.  No painel, abra o Visualizador de alerta.  
  
2.  Na lista de organizar, selecione o alerta que relata informações sobre como desativar recursos do Windows Server Essentials depois de transição.  
  
3.  Em alerta, clique em **desativar recursos do Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>No painel obter ajuda e suporte  
  
1.  Sobre a Home page, clique em obter ajuda e suporte.  
  
2.  Clique em **desativar o Assistente de recursos do Windows Server Essentials**.  
  
 É possível que algumas tarefas executadas pelo **desativar o Assistente de recursos do Windows Server Essentials** não será concluída com êxito. Em alguns casos, isso pode evitar que o painel sejam executados. Se isso ocorrer, você pode iniciá-lo manualmente, executando o arquivo:  
  
 **%SystemDrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Consulte também  
  

-   [Transição para o Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar dados do servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transição para o Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Migrar dados do servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

