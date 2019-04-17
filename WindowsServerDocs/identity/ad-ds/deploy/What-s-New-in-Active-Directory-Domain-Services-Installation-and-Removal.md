---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: "O que & #39; s Novidades no domínio do Active Directory serviços de instalação e remoção"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 814a6f1af9144db28e81163b21cb5da4b6e51b3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services-installation-and-removal"></a>O que & #39; s Novidades no domínio do Active Directory serviços de instalação e remoção

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda:  
  
-   [O Assistente de configuração de serviços de domínio do Active Directory](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_ADConfigurationWizard)  
  
-   [Integração de Adprep.exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)  
  
-   [Validação de pré-requisito de instalação do AD DS](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_PrereqCheck)  
  
-   [Requisitos do sistema](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_SystemReqs)  
  
-   [Problemas conhecidos](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)  
  
Implantação do Active Directory serviços de domínio (AD DS) no Windows Server 2012 é mais simples e mais rápidas do que as versões anteriores do Windows Server. O processo de instalação do AD DS baseia-se agora no Windows PowerShell e está integrado com o Gerenciador do servidor. O número de etapas necessárias para apresentar os controladores de domínio em um ambiente existente do Active Directory é reduzido. Isso torna o processo de criação de um novo ambiente do Active Directory mais simples e mais eficiente. O novo processo de implantação do AD DS minimiza a chance de erros que seriam contrário bloqueadas instalação.  
  
Além disso, você pode instalar os binários de função de servidor do AD DS (ou seja, a função de servidor do AD DS) em vários servidores ao mesmo tempo. Você também pode executar o Assistente de instalação do AD DS remotamente em um servidor específico. Essas melhorias fornecem mais flexibilidade para implantar controladores de domínio que executam o Windows Server 2012, especialmente para implantações em grande escala, globais onde muitos controladores de domínio precisam ser implantadas para escritórios em diferentes regiões.  
  
Instalação do AD DS inclui os seguintes recursos:  
  
-   **Adprep.exe integração com o processo de instalação do AD DS.** As etapas complicadas necessárias para preparar um Active Directory existentes, como a necessidade de usar diversas credenciais diferentes, copie os arquivos Adprep.exe ou fazer logon controladores de domínio específico, são todos simplificadas ou ocorrem automaticamente. Isso reduz o tempo necessário para instalar o AD DS e reduz as chances de erros que podem bloquear promoção de controlador de domínio.  
  
    Para ambientes em que é preferível para executar comandos adprep.exe antes da instalação de um novo controlador de domínio, você ainda pode executar comandos adprep.exe separadamente da instalação do AD DS. A versão do Windows Server 2012 do adprep.exe é executado remotamente, portanto, você pode executar todos os comandos necessários de um servidor que executa uma versão de 64 bits do Windows Server 2008 ou posterior.  
  
-   **Na nova instalação do AD DS é incorporada no Windows PowerShell e pode ser chamada remotamente.** Na nova instalação do AD DS é integrada com o Gerenciador do servidor, portanto, você pode usar a mesma interface para instalar o AD DS que você usa ao instalar outras funções de servidor. Para usuários do Windows PowerShell, os cmdlets de implantação do AD DS oferecem maior funcionalidade e flexibilidade. Há paridade funcional entre linha de comando e opções de instalação de GUI.  
  
-   **Na nova instalação do AD DS inclui validação de pré-requisito.** Qualquer possíveis erros são identificados antes de começa a instalação. Você pode corrigir as condições de erro antes que ocorram sem as preocupações resultante de uma parcialmente concluir a atualização. Por exemplo, se adprep /domainprep precisa ser executado, o Assistente para instalação verifica se o usuário tem direitos suficientes para executar a operação.  
  
-   **Páginas de configuração são agrupadas em uma sequência que reflete os requisitos das opções mais comuns de promoção com opções relacionadas agrupadas em menos páginas do assistente.** Isso proporciona melhor contexto para fazer escolhas de instalação.  
  
-   **Você pode exportar um script do Windows PowerShell que contém todas as opções especificadas durante a instalação gráfica.** No final de uma instalação ou a remoção, você pode exportar as configurações de um script do Windows PowerShell para uso com automatizando a mesma operação.  
  
-   **Somente replicação crítica ocorre antes da reinicialização.** Nova opção para permitir que a replicação de dados não críticos antes da reinicialização. Para obter mais informações, consulte [ADDSDeployment cmdlet argumentos](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  
  
### <a name="BKMK_ADConfigurationWizard"></a>O Assistente de configuração de serviços de domínio do Active Directory  
A partir do Windows Server 2012, o Assistente de configuração de serviços de domínio Active Directory substitui o legado Assistente domínio Active Directory Services instalação como a opção de interface (IU) do usuário para especificar as configurações quando você instala um controlador de domínio. O Assistente de configuração de serviços de domínio Active Directory começa após a conclusão do Assistente para adicionar funções.  
  
> [!WARNING]  
> O (dcpromo.exe) Assistente de instalação dos serviços do Active Directory domínio herdado foi preterido a partir do Windows Server 2012.  
  
Em [instalar serviços de domínio do Active Directory & #40; Nível de 100 & #41; ](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), os procedimentos de interface do usuário mostram como iniciar o Assistente para adicionar funções para instalar o servidor do AD DS binários de função e, em seguida, execute o assistente domínio Active Directory serviços de configuração para concluir a instalação do controlador de domínio. Os exemplos do Windows PowerShell mostram como concluir as duas etapas usando um cmdlet de implantação do AD DS.  
  
### <a name="BKMK_NewAdprep"></a>Integração de adprep.exe  
A partir do Windows Server 2012, existe apenas uma versão do Adprep.exe (não há nenhuma versão de 32 bits, adprep32.exe). Adprep comandos são executados automaticamente conforme necessário quando você instala um controlador de domínio que executa o Windows Server 2012 em um domínio do Active Directory existente ou floresta.  
  
Embora adprep operações serão executadas automaticamente, você pode executar Adprep.exe separadamente. Por exemplo, se o usuário que instala o AD DS não é um membro do grupo Administradores corporativos, que é necessário para executar Adprep /forestprep, em seguida, talvez seja necessário executar o comando separadamente. Mas, você só precisa executar adprep.exe se você estiver planejando atualização in-loco, o primeiro controlador de domínio do Windows Server 2012 (em outras palavras, você planeja in-loco atualizar o sistema operacional de um controlador de domínio que executa o Windows Server 2012).  
  
Adprep.exe está localizado na pasta \support\adprep do disco de instalação do Windows Server 2012. A versão do Windows Server 2012 do adprep é capaz de executar remotamente.  
  
A versão do Windows Server 2012 do adprep.exe pode executar em qualquer servidor que executa uma versão de 64 bits do Windows Server 2008 ou posterior. O servidor precisa conectividade de rede para o mestre de esquema de floresta e o mestre de infraestrutura do domínio em que você deseja adicionar um controlador de domínio. Se qualquer uma dessas funções está hospedado em um servidor que executa o Windows Server 2003, em seguida, adprep deve ser executada remotamente. O servidor em que você execute adprep não precisa ser um controlador de domínio. Ele pode ser domínio associado ou em um grupo de trabalho.  
  
> [!NOTE]  
> Se você tentar executar a versão do Windows Server 2012 do adprep.exe em um servidor que executa o Windows Server 2003, o seguinte erro é exibida:  
>   
> Adprep.exe não é um aplicativo Win32 válido.  
  
![Quais são as novidades](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  
  
Para obter informações sobre como resolver outros erros retornados pelo Adprep.exe, consulte [problemas conhecidos ](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  
  
#### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Verificação de associação de grupo contra funções mestre de operações do Windows Server 2003  
Para cada comando (/forestprep, /domainprep, ou /rodcprep), Adprep realiza uma verificação de associação de grupo para determinar se a credencial especificada representa uma conta em determinados grupos. Para executar essa verificação, Adprep entra em contato com o proprietário da função mestre de operações. Se o mestre de operações estiver executando o Windows Server 2003, você precisa especificar o /user e /userdomain parâmetros de linha de comando se você executar Adprep.exe para garantir que a associação ao grupo Verifique é realizado em todos os casos.  
  
O /user e /userdomain são novos parâmetros para Adprep.exe no Windows Server 2012. Esses parâmetros especificam o nome de conta de usuário e domínio do usuário, respectivamente, do usuário que executa o comando adprep. Os blocos de utilitário de linha de comando Adprep.exe especificando um dos /userdomain e /user mas omitindo o outro.  
  
No entanto, operações de Adprep também podem ser executadas como parte de uma instalação do AD DS usando o Windows PowerShell ou Gerenciador do servidor. Essas experiências compartilham o mesmo (adprep.dll) implementação subjacente como adprep.exe. As experiências do Windows PowerShell e Gerenciador do servidor têm suas credenciais separadas de entrada, que não impõe os mesmos requisitos pelo adprep.exe. Usando o Windows PowerShell ou Gerenciador do servidor, é possível passar um valor /user, mas não /userdomain para adprep.dll. Se /user for especificado, mas /userdomain não for especificado, domínio da máquina local é usado para executar a verificação. Se o computador não estiver ingressado no domínio, a associação de grupo não pode ser verificada.  
  
Quando a associação ao grupo não pode ser verificada, Adprep mostra uma mensagem de aviso nos arquivos de log e continua:  
  
```  
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```  
  
Se você executar Adprep.exe sem especificar a /user e /userdomain parâmetros e o mestre de operações está executando o Windows Server 2003, Adprep.exe entra em contato com um controlador de domínio no domínio do usuário de logon atual. Se o usuário de logon atual não for uma conta de domínio, Adprep.exe não é possível executar a verificação de associação de grupo. Adprep.exe também não é possível executar a verificação de associação de grupo se as credenciais de cartão inteligente são usadas, mesmo se você especificar ambos /user e /userdomain.  
  
Se Adprep for concluído com êxito, não há nenhuma ação é necessária. Se Adprep falhar durante a execução com erros de acesso, forneça uma conta com a associação correta. Para obter mais informações, consulte [requisitos para executar Adprep.exe e instalar os serviços de domínio do Active Directory de credenciais ](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
#### <a name="syntax-for-adprep-in-windows-server-2012"></a>Sintaxe de Adprep no Windows Server 2012  
Use a sintaxe a seguir para executar adprep separadamente de uma instalação do AD DS:  
  
```  
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```  
  
Use /logdsid no comando para gerar mais detalhadas de registro em log. O adprep.log está localizado em % windir%\System32\Debug\Adprep\Logs.  
  
#### <a name="running-adprep-using-smartcard"></a>Executando adprep usando smartcard  
A versão do Windows Server 2012 do adprep.exe funciona usando um cartão inteligente como credenciais, mas não há nenhuma maneira fácil para especificar as credenciais de cartão inteligente por meio da linha de comando. Uma maneira de fazer isso é para obter a credencial de cartão inteligente por meio do cmdlet do PowerShell Get-Credential. Em seguida, use o nome do usuário do objeto retornado PSCredential, que aparece como `@@...`. A senha é o PIN do cartão inteligente.  
  
Adprep.exe requer /userdomain se /user for especificado. Credenciais de cartão inteligente, o /userdomain deve ser o domínio da conta de usuário subjacente representado pelo cartão inteligente.  
  
#### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Adprep /domainprep /gpprep comando não é executado automaticamente  
O adprep /domainprep /gpprep comando não é executado como parte da instalação do AD DS. Esse comando define as permissões que são necessárias para conjunto resultante de diretivas (RSOP) funcionalidade do modo de planejamento. Para obter mais informações sobre esse comando, consulte [artigo da Base de Conhecimento Microsoft 324392](https://support.microsoft.com/kb/324392). Se o comando precisa ser executado no seu domínio do Active Directory, você pode executá-lo separadamente da instalação do AD DS. Se o comando foi executado na preparação de implantação de controladores de domínio que executam o Windows Server 2003 SP1 ou posterior, o comando não precisa ser executado novamente.  
  
Você pode adicionar com segurança controladores de domínio que executam o Windows Server 2012 a um domínio existente sem executar adprep /domainprep /gpprep, mas o modo de planejamento RSOP não funcionará corretamente.  
  
### <a name="BKMK_PrereqCheck"></a>Validação de pré-requisito de instalação do AD DS  
O Assistente de instalação do AD DS verifica se os pré-requisitos a seguir forem atendidos antes de começa a instalação. Isso fornece a chance de corrigir problemas que potencialmente podem bloquear a instalação.  
  
Por exemplo, relacionados à Adprep pré-requisitos incluem:  
  
-   Verificação de credencial Adprep: Se adprep precisa ser executado, o Assistente para instalação verifica se o usuário tem direitos suficientes para executar as operações de Adprep necessárias.  
  
-   Verificação do esquema disponibilidade mestre: se o Assistente de instalação determina que adprep /forestprep precisa ser executado, ele verifica se o mestre de esquema está online e falha caso contrário.  
  
-   Seleção de disponibilidade mestre infraestrutura: se o Assistente de instalação determina que adprep /domainprep precisa ser executado, ele verifica se o mestre de infraestrutura está online e falha caso contrário.  
  
Outras verificações de pré-requisito transferidas dos herdados (dcpromo.exe) Assistente de instalação do Active Directory incluem:  
  
-   Verificação de nome de floresta: garante que o nome da floresta é válido e ainda não exista.  
  
-   Verificação de nome NetBIOS: verificações que forneceu o nome NetBIOS é válido e não entre em conflito com nomes existentes.  
  
-   Verificação do componente de caminho: verifica se os caminhos para o banco de dados do Active Directory, logs e SYSVOL são válidos e que há espaço suficiente em disco disponível para eles.  
  
-   Verificação de nome de domínio do filho: garante que o pai e os novos nomes de domínio filho são válidos e que elas não entrem em conflito com domínios existentes.  
  
-   Árvore de verificação de nome de domínio: garante que o nome da árvore especificado é válido e que ele ainda não exista.  
  
## <a name="BKMK_SystemReqs"></a>Requisitos do sistema  
Requisitos do sistema para o Windows Server 2012 continuam como no Windows Server 2008 R2. Para obter mais informações, consulte [Windows Server 2008 R2 com SP1 requisitos do sistema](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  
  
Alguns recursos podem ter requisitos adicionais. Por exemplo, o recurso de clonagem de controlador de domínio virtual requer a que o emulador do PDC executar o Windows Server 2012 e um computador que executa o Windows Server 2012 com a função Hyper-V instalada.  
  
## <a name="BKMK_KnownIssues"></a>Problemas conhecidos  
Esta seção lista alguns dos problemas conhecidos que afetam a instalação do AD DS no Windows Server 2012. Para outros problemas conhecidos, consulte [solução de problemas de implantação de controlador de domínio ](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
-   Se o acesso WMI ao mestre de esquema é bloqueado pelo Firewall do Windows quando você executar remotamente adprep /forestprep, o seguinte erro é registrado no log de adprep em systemroot%\system32\debug\adprep %:  
  
    ```  
    Adprep encountered a Win32 error.   
    Error code: 0x6ba Error message: The RPC server is unavailable.  
    ```  
  
    Nesse caso, você pode contornar o erro por qualquer adprep em execução /forestprep diretamente no esquema mestre, ou você pode executar que um dos seguintes comandos para permitir o tráfego WMI pelo Firewall do Windows.  
  
    Para o Windows Server 2008 ou posterior:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes  
    ```  
  
    Para o Windows Server 2003:  
  
    ```  
    netsh firewall set service RemoteAdmin enable  
    ```  
  
    Depois que termina de adprep, você pode executar qualquer um dos seguintes comandos para bloquear o tráfego WMI novamente:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no  
    ```  
  
    ```  
    netsh firewall set service remoteadmin disable  
    ```  
  
-   Você pode digitar Ctrl + C para cancelar o cmdlet Install-ADDSForest. O cancelamento impede a instalação e todas as alterações que foram feitas para o estado do servidor são revertidas. Mas, depois que o comando de cancelamento é emitido, o controle não é retornado para o Windows PowerShell e o cmdlet pode travar indefinidamente.  
  
-   **Instalação de um controlador de domínio adicional usando credenciais de cartão inteligente falhará se o servidor de destino não tenha ingressado no domínio antes da instalação.**  
  
    Nesse caso, a mensagem de erro retornada é:  
  
    Não é possível conectar ao controlador de domínio de origem de replicação *nome do controlador de domínio de origem *. (Exceção: Logonfailure: nome de usuário desconhecido ou senha incorreta)  
  
    Se você adicionar o servidor de destino ao domínio e, em seguida, executar a instalação usando um cartão inteligente, a instalação seja bem-sucedido.  
  
-   **O módulo ADDSDeployment não é executado em processos de 32 bits.** Se você estiver automatizando de implantação e configuração do Windows Server 2012 usando um script que inclui um cmdlet ADDSDeployment e outro cmdlet que não oferece suporte a processos de 64 bits nativos, o script pode falhar com um erro que indica que o cmdlet ADDSDeployment não pôde ser encontrado.  
  
    Nesse caso, você precisa executar o cmdlet ADDSDeployment separadamente do cmdlet que não oferece suporte a processos de 64 bits nativos.  
  
-   Há um novo sistema de arquivos no Windows Server 2012 denominado sistema de arquivos resiliente. Não armazene o banco de dados do Active Directory, arquivos de log ou SYSVOL em um volume de dados formatado com o sistema de arquivos resiliente (ReFS). Para saber mais sobre ReFS, consulte [criando a próxima geração de sistema de arquivos para Windows: ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx).  
  
-   No Gerenciador do servidor, os servidores que executam o AD DS ou outras funções de servidor em uma instalação Server Core e foram atualizados para o Windows Server 2012, a função de servidor pode aparecer com status vermelho, mesmo que o status e eventos são coletados conforme o esperado. Servidores que executam uma instalação Server Core de uma versão preliminar do que Windows Server 2012 também pode ser afetada.  
  
### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>Travamentos de instalação de serviços de domínio do diretório ativos se um erro impedir a replicação crítica  
Se a instalação do AD DS encontra um erro durante a fase de replicação crítica, a instalação pode travar indefinidamente. Por exemplo, se erros de rede impedir a replicação crítica de concluir, a instalação não continua.  
  
Se você estiver instalando usando o Gerenciador do servidor, você pode ver a página installation progress permanece aberto, mas houver nenhum erro relatado na tela e não pode alterar o progresso por cerca de 15 minutos. Se você estiver usando o Windows PowerShell, o progresso mostrado na janela do Windows PowerShell não mudará para mais de 15 minutos.  
  
Se você enfrentar esse problema, verifique o arquivo dcpromo.log na pasta %SystemRoot%\Debug. no servidor de destino. O arquivo de log geralmente indicará falhas repetidas replicar. Algumas causas conhecidas para esse problema são:  
  
-   Problemas de rede impedir a replicação crítica entre o servidor de destino está sendo promovido e o controlador de domínio de origem de replicação.  
  
    Por exemplo, dcpromo.log mostra:  
  
    ```  
  
      05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963  
      Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    500  
    Reported error information:  
    Error value:   
    Could not find the domain controller for this domain. (1908)  
    directory service:   
    <domain>.com  
    Extensive error information:  
    Error value:   
    A security package specific error occurred. 1825  
    directory service:   
    <DC Name>  
    ```  
  
    Porque o processo de instalação repete indefinidamente replicação crítica, a instalação do controlador de domínio continua se os problemas de rede subjacente sejam resolvidos. Investigue o problema de rede usando ferramentas como ipconfig, nslookup e netmon conforme necessário. Verifique se há conectividade entre o controlador de domínio que você está promovendo e o parceiro de replicação selecionada durante a instalação do AD DS. Além disso, verifique se a resolução de nome está funcionando.  
  
    Requisitos de instalação do AD DS para resolução de nomes e conectividade de rede são validados durante a verificação de pré-requisitos antes de começa a instalação. Mas algumas condições de erro podem surgir no tempo após a validação de pré-requisito ocorre e antes que a instalação for concluída, por exemplo, se o parceiro de replicação se torna não disponível durante a instalação.  
  
-   Durante a instalação da réplica do controlador de domínio, a conta de administrador local do servidor de destino é especificada para as credenciais de instalação e a senha da conta do administrador local corresponde à senha de uma conta de administrador do domínio. Nesse caso, você pode concluir o Assistente de instalação e iniciar a instalação antes de você encontrar a falha de "Acesso negado".  
  
    Por exemplo, dcpromo.log mostra:  
  
    ```  
  
    03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...  
    03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    508  
    Reported error information:  
    Error value:   
    Access is denied. (5)  
    directory service:   
    DC2.contoso.com  
  
    ```  
  
    Se o erro é causado especificando uma conta de administrador local e uma senha, para recuperar você precisa reinstalar o sistema operacional, [executar a limpeza de metadados](https://technet.microsoft.com/library/cc816907(WS.10).aspx) da conta para o controlador de domínio que não conseguiu concluir a instalação e tente novamente a instalação do AD DS com credenciais de administrador do domínio. Reiniciando o servidor não corrigirão essa condição de erro porque o servidor indicará que AD DS é instalado, mesmo que a instalação não foi concluída com êxito.  
  
### <a name="BKMK_nonnormalDNSNameWarning"></a>Assistente de configuração dos serviços do Active Directory domínio avisa quando é especificado um nome DNS não normalizado  
Se você criar um novo domínio ou floresta e você especificar um nome de domínio DNS que inclui internacionalizados caracteres que não estão normalizados, o Assistente de configuração de serviços de domínio Active Directory exibe um aviso de que as consultas do DNS para o nome poderão falhar. Embora o nome de domínio DNS é especificado na página de configuração da implantação, o aviso aparece na página de verificação de pré-requisitos posteriormente no assistente.  
  
Se um nome de domínio DNS é especificado usando um nome cancelou normalizado como .com füßball.com ou 'ΣΤ' (as versões normalizadas são: füssball.com e βστα.com), aplicativos cliente que tentam acessá-la com WinHTTP serão normalizar o nome antes de chamar APIs de resolução de nome. Se o usuário digita "'ΣΤ'.com" na caixa de diálogo de alguns, a consulta DNS será enviada como "βστα.com" e nenhum servidor DNS corresponderá-lo com um registro de recurso para ".com 'ΣΤ'". O usuário será incapaz de resolver nome.  
  
O exemplo a seguir explica um dos problemas que podem ocorrer ao usar um nome de IDN que não é normalizado:  
  
1.  O domínio usando um nome não normalizado é criado e registrado no servidor dns: füßball.com  
  
2.  Nps"máquina" está associado ao domínio e obtém seu nome registrado: nps.füßball.com  
  
3.  Um aplicativo cliente tenta se conectar com o servidor nps.füßball.com  
  
4.  O aplicativo cliente tenta resolver o nome nps.füßball.com chamar APIs de resolução de nome.  
  
5.  Devido a normalização, o nome será convertido em nps.füssball.com e é consultado transmitido como nps.füßball.com  
  
6.  O aplicativo cliente é incapaz de resolver o nome, desde que o nome registrado é nps.füßball.com  
  
Se o aviso é exibido na página de verificação de pré-requisitos no Assistente de configuração de serviços de domínio Active Directory, retornar à página de configuração de implantação e especifique um nome de domínio DNS normalizado. Se você estiver instalando um novo domínio usando o Windows PowerShell, especifique um nome DNS normalizado para a opção - NomeDomínio.  
  
Para saber mais sobre IDNs, consulte [manipulando a nomes de domínio internacionalizado (IDNs)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
  

