---
title: Solução de problemas com URLs de investigação da Web
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6dfffd1e-f4f4-43b6-9e3c-49015ce34338
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 132db4811ee135d2ebff99efed6f53b5db1356ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404433"
---
# <a name="troubleshooting-web-probe-urls"></a>Solução de problemas com URLs de investigação da Web

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico contém informações sobre como solucionar problemas relacionados ao comando `Set-DAEntryPointDC`. Para confirmar que o erro recebido está relacionado à configuração do controlador de domínio de ponto de entrada, procure a ID de evento 10065 no Log de Eventos do Windows.  
  
## <a name="SaveGPOSettings"></a>Salvando configurações de GPO do servidor  
**Erro recebido**. Ocorreu um erro ao salvar as configurações de acesso remoto ao GPO < GPO_name >.  
  
Para solucionar esse erro, consulte Salvando configurações de GPO do servidor.  
  
## <a name="remote-access-is-not-configured"></a>O Acesso Remoto não está configurado  
**Erro recebido**. O acesso remoto não está configurado em < server_name >. Especifique o nome de um servidor que pertença a uma implantação multissite.  
  
Ou  
  
O acesso remoto não está configurado no servidor < server_name >. Especifique um computador com DirectAccess habilitado.  
  
**Causa**  
  
O Acesso Remoto não está configurado no computador especificado pelo parâmetro *ComputerName*.  
  
O cmdlet `Set-DaEntryPointDC` só está disponível nos servidores que fazem parte de uma implantação multissite configurada.  
  
**Solução**  
  
Execute o comando especificando o parâmetro *ComputerName* com o nome do servidor que já está configurado como parte da implantação multissite.  
  
## <a name="multisite-is-not-enabled"></a>A funcionalidade multissite não está habilitada  
**Erro recebido**. Você deve habilitar uma implantação multissite antes de executar esta operação. Para fazer isso, use o cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
A funcionalidade multissite não está habilitada no servidor especificado pelo parâmetro *ComputerName*.  
  
O cmdlet `Set-DaEntryPointDC` só está disponível nos servidores que fazem parte de uma implantação multissite configurada.  
  
**Solução**  
  
Execute o comando especificando o parâmetro *ComputerName* com o nome do servidor que já está configurado como parte da implantação multissite.  
  
## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>Ponto de entrada e controlador de domínio não fornecidos em cmdlet  
O cmdlet `Set-DaEntryPointDC` permite alterar o controlador de domínio que está associado a diversos pontos de entrada, por exemplo, quando um determinado controlador de domínio não está mais disponível. Você pode atualizar um ponto de entrada específico de modo a usar outro controlador de domínio, ou pode atualizar todos os pontos de entrada que utilizam um controlador de domínio específico de modo a usar um novo controlador. No primeiro caso, recomenda-se usar o parâmetro *EntryPointName* para especificar qual ponto de entrada deve ser atualizado. No segundo caso, recomenda-se usar o parâmetro *ExistingDC* para especificar qual controlador de domínio deve ser substituído. Só é possível especificar um desses parâmetros.  
  
**Erro recebido**. Nenhum parâmetro obrigatório foi especificado. Forneça um nome de ponto de entrada ou o nome de um controlador de domínio existente.  
  
Ou  
  
Especifique um valor Enabled ou Disabled para o parâmetro `Set-DaEntryPointDC`.  
  
**Causa**  
  
Nenhum dos parâmetros *EntryPointName* ou *ExistingDC* foi especificado, ou os dois parâmetros foram especificados, para o cmdlet `Set-DaEntryPointDC`.  
  
**Solução**  
  
Execute o comando especificando ou o parâmetro *EntryPointName* ou o parâmetro *ExistingDC*.  
  
## <a name="could-not-locate-domain-controller"></a>Não foi possível localizar o controlador de domínio  
**Erro recebido**. Não é possível localizar um novo controlador de domínio automaticamente. Tente mais tarde ou verifique as configurações do controlador de domínio.  
  
**Causa**  
  
O computador especificado com o parâmetro *ComputerName* não pode ser acessado através do RPC, ou não há controladores de domínio graváveis disponíveis no domínio.  
  
**Solução**  
  
Assegure-se de que o computador remoto possa ser acessado através do RPC e de que haja um controlador de domínio gravável disponível para o domínio. Se houver um controlador gravável disponível para o domínio, você também poderá especificar o nome dele explicitamente usando o parâmetro *NewDC*.  
  
## <a name="could-not-connect-to-domain-controller"></a>Não foi possível se conectar ao controlador de domínio  
  
-   **Problema 1**  
  
    **Erro recebido**. O controlador de domínio < domain_controller > não pode ser acessado. Verifique a conectividade da rede e a disponibilidade do controlador de domínio.  
  
    **Causa**  
  
    Não é possível acessar o controlador de domínio. Isso só ocorre quando o administrador especifica um controlador de domínio nos parâmetros *NewDC* ou *ExistingDC*.  
  
    **Solução**  
  
    Certifique-se de que o nome do controlador de domínio esteja escrito corretamente. Caso você tenha usado um nome curto para especificá-lo, use o FQDN e tente novamente.  
  
-   **Problema 2**  
  
    **Erro recebido**. O controlador de domínio < domain_controller > não pode ser contatado.  
  
    **Causa**  
  
    Pode haver um problema com a rede que impede o acesso ao controlador de domínio especificado no parâmetro *NewDC* ou a qualquer outro controlador de domínio existente na configuração.  
  
    **Solução**  
  
    Certifique-se de que o nome do controlador de domínio esteja escrito corretamente; verifique se o controlador existe, está em execução e é gravável; e confira se há uma relação de confiança entre o controlador e o domínio.  
  
-   **Problema 3**  
  
    **Erro recebido**. O controlador de domínio < domain_controller > não pode ser acessado para %2! s!.  
  
    **Causa**  
  
    Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Quando o controlador de domínio que gerencia o GPO do servidor de um ponto de entrada não está disponível, as definições de configuração de acesso remoto não podem ser lidas ou modificadas.  
  
    **Solução**  
  
    Siga o procedimento "para alterar o controlador de domínio que gerencia GPOs de servidor", descrito em [2,4. Configurar GPOs](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problema 4**  
  
    **Erro recebido**. O controlador de domínio primário no domínio < domain_name > não pode ser acessado.  
  
    **Causa**  
  
    Para manter a consistência da configuração em uma implantação multissite, é importante assegurar que cada GPO seja gerenciado por um único controlador de domínio. Os GPOs de cliente são gerenciados no controlador de domínio primário. Se esse controlador não estiver disponível, não será possível ler nem modificar as configurações do Acesso Remoto.  
  
    **Solução**  
  
    Siga o procedimento "para transferir a função de emulador de PDC", descrito em [2,4. Configurar GPOs](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
## <a name="read-only-domain-controller"></a>Controlador de domínio somente leitura  
**Erro recebido**. O controlador de domínio < domain_controller > é somente leitura. Especifique um controlador de domínio que não seja somente leitura.  
  
**Causa**  
  
O controlador de domínio especificado com o parâmetro *NewDC* é somente leitura.  
  
**Solução**  
  
Quando o cmdlet `Set-DAEntryPointDC` é utilizado, o parâmetro *NewDC* é usado para atualizar o controlador de domínio associado a um determinado ponto de entrada ou para atualizar todos os pontos de entrada associados a um controlador de domínio. Por isso, o novo controlador deve ser gravável. Especifique um controlador de domínio gravável no parâmetro *NewDC* e tente novamente.  
  
## <a name="cannot-retrieve-gpo"></a>Não é possível recuperar um GPO  
  
-   **Problema 1**  
  
    **Erro recebido**. O GPO < GPO_name > no controlador de domínio < previous_domain_controller > não pode ser recuperado do controlador de domínio < replacement_domain_controller do > porque eles não estão no mesmo domínio.  
  
    **Causa**  
  
    O servidor de acesso remoto e o controlador de domínio não estão no mesmo domínio; em consequência, o GPO não pode ser recuperado.  
  
    **Solução**  
  
    Se você tentou atualizar um ponto de entrada específico, verifique se o novo controlador de domínio está no mesmo domínio do servidor do ponto de entrada. Se você tentou atualizar um controlador de domínio específico, verifique se o novo controlador está no mesmo domínio daquele que você está tentando substituir.  
  
-   **Problema 2**  
  
    **Erro recebido**. O GPO < GPO_name > no controlador de domínio < previous_domain_controller > não podem ser recuperados do controlador de domínio < replacement_domain_controller do. Aguarde até a conclusão da replicação do domínio e tente de novo.  
  
    **Causa**  
  
    Ao tentar atualizar um controlador de domínio de ponto de entrada, o cmdlet tenta ler o GPO de servidor no novo controlador de domínio; porém, o GPO não se encontra no novo controlador porque ainda não foi replicado.  
  
    **Solução**  
  
    O GPO de servidor não existe no novo controlador de domínio. Verifique se os GPOs foram replicados com êxito no novo controlador e tente novamente.  
  
-   **Problema 3**  
  
    **Erro recebido**. Você não tem permissões para acessar o GPO < GPO_name >.  
  
    **Causa**  
  
    Ao tentar atualizar um controlador de domínio de ponto de entrada, o cmdlet tenta ler o GPO de servidor no novo controlador de domínio; porém, o GPO não pode ser lido no novo controlador porque você não tem as permissões corretas.  
  
    **Solução**  
  
    O GPO existe no controlador de domínio, mas não pode ser lido. Certifique-se de ter as permissões necessárias e tente novamente.  
  
## <a name="entry-point-not-part-of-multisite-deployment"></a>Ponto de entrada não faz parte da implantação multissite  
**Erro recebido**. O ponto de entrada < entry_point_name > não faz parte da implantação multissite. Especifique um valor alternativo.  
  
**Causa**  
  
O nome de ponto de entrada que você especificou não foi encontrado.  
  
**Solução**  
  
Certifique-se de que o nome do ponto de entrada esteja escrito corretamente e de que os GPOs sejam replicados para os controladores de domínio necessários e tente novamente. Para ver o controlador de domínio atribuído a cada ponto de entrada, use `Get-DAEntryPointDC`.  
  
## <a name="remote-access-server-settings"></a>Configurações do servidor de acesso remoto  
  
-   **Problema 1**  
  
    **Erro recebido**. O servidor < server_name > no ponto de entrada < entry_point_name > não pode ser acessado.  
  
    **Causa**  
  
    Ao tentar atualizar um controlador de domínio de ponto de entrada, o cmdlet tenta ler e gravar nesse controlador a partir de todos os servidores de acesso remoto relevantes. O cmdlet não pôde ler os dados de um ou mais servidores de acesso remoto.  
  
    **Solução**  
  
    Assegure-se de que todos os servidores de acesso remoto relevantes estejam em execução e de que você tenha permissões de administrador local em todos eles, e tente novamente.  
  
-   **Problema 2**  
  
    **Erro recebido**. Não é possível salvar as configurações no registro no servidor < server_name > no ponto de entrada < entry_point_name >.  
  
    **Causa**  
  
    Ao tentar atualizar um controlador de domínio de ponto de entrada, o cmdlet tenta ler e gravar nesse controlador a partir de todos os servidores de acesso remoto relevantes. O cmdlet não pôde gravar os dados em um ou mais servidores de acesso remoto.  
  
    **Solução**  
  
    Assegure-se de que todos os servidores de acesso remoto relevantes estejam em execução e de que você tenha permissões de administrador local em todos eles, e tente novamente.  
  
-   **Problema 3**  
  
    **Erro recebido**. As atualizações de GPO não podem ser aplicadas em < server_name >. As alterações só terão efeito após a próxima atualização de política.  
  
    **Causa**  
  
    Ao usar o cmdlet `Set-DAEntryPointDC`, o parâmetro *ComputerName* especificado é um servidor de acesso remoto em um ponto de entrada diferente do último adicionado à implantação multissite.  
  
    **Solução**  
  
    É possível ver os servidores que não foram atualizados usando o **Status da Configuração** no **PAINEL** do Console de Gerenciamento de Acesso Remoto. Isso não causa problemas funcionais; porém, você pode executar `gpupdate /force` nos servidores que não foram atualizados para que o status da configuração seja atualizado imediatamente.  
  
## <a name="problem-resolving-fqdn"></a>Problema ao resolver um FQDN  
**Erro recebido**. O servidor < server_name > no ponto de entrada < entry_point_name > não pode ser acessado.  
  
**Causa**  
  
Enquanto obtia a lista de servidores do DirectAccess a serem modificados, o cmdlet não pôde resolver o FQDN (nome de domínio totalmente qualificado) de um dos servidores com base na respectiva SID de computador.  
  
**Solução**  
  
O ponto de entrada especificado na mensagem de erro está associado a um controlador de domínio. Assegure-se de que esse controlador esteja disponível para o ponto de entrada. Se o computador ao qual pertence a SID especificada tiver sido removido do domínio, ignore essa mensagem e remova o servidor da implantação multissite.  
  
## <a name="no-entry-points-to-update"></a>Não há pontos de entrada para atualizar  
**Aviso recebido**. As configurações do controlador de domínio não foram modificadas. Se você achar que alterações são necessárias, verifique se os parâmetros do cmdlet estão configurados corretamente e se os GPOs foram replicados nos controladores de domínio necessários.  
  
**Causa**  
  
Ao chamar o cmdlet `Set-DaEntryPointDC` com o parâmetro *ExistingDC*, o DirectAccess verifica todos os pontos de entrada e atualiza aqueles que estão associados ao controlador de domínio especificado. No entanto, nenhum ponto de entrada usa o *ExistingDC*especificado.  
  
**Solução**  
  
Para ver a lista dos pontos de entrada com os respectivos controladores de domínio associados, use o cmdlet `Get-DAEntryPointDC`. Se era o caso de terem ocorrido alterações, certifique-se de que os parâmetros do cmdlet estejam escritos corretamente e de que os GPOs sejam replicados para os controladores de domínio necessários e tente novamente.  
  


