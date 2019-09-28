---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: Novidades na instalação e na remoção dos Serviços de Domínio Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 286d3ee6e9c2b9959a4cc60a710b1cb078612201
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369556"
---
# <a name="whats-new-in-active-directory-domain-services-installation-and-removal"></a>Novidades na instalação e na remoção dos Serviços de Domínio Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A implantação de Active Directory Domain Services (AD DS) no Windows Server 2012 é mais simples e mais rápida do que as versões anteriores do Windows Server. O processo de instalação do AD DS agora está embutido no Windows PowerShell e foi integrado ao Gerenciador do Servidor. O número das etapas necessárias para introduzir controladores de domínio em um ambiente Active Directory existente foi reduzido. Isso simplifica e torna mais eficiente o processo de criação de um novo ambiente Active Directory. O novo processo de implantação de AD DS minimiza as possibilidades de erros que, de outra forma, bloqueariam a instalação.  
  
Além disso, é possível instalar os binários da função de servidor AD DS (ou seja, a função de servidor AD DS) em vários servidores ao mesmo tempo. Também é possível executar remotamente o assistente de instalação do AD DS em um servidor individual. Esses aprimoramentos fornecem mais flexibilidade para a implantação de controladores de domínio que executam o Windows Server 2012, especialmente para implantações globais de grande escala em que muitos controladores de domínio precisam ser implantados em escritórios em regiões diferentes.  
  
A instalação do AD DS inclui os seguintes recursos:  
  
- **Integração do Adprep.exe no processo de instalação do AD DS.** As etapas complicadas necessárias à preparação de um Active Directory existente, como a necessidade de usar uma variedade de credenciais diferentes, copiar os arquivos Adprep.exe ou fazer logon em controladores de domínio específicos, foram todas simplificadas ou ocorrem automaticamente. Isso reduz o tempo necessário para instalar o AD DS e diminui as possibilidades de erros que, de outra forma, bloqueariam a promoção do controlador de domínio.  

   Nos ambientes em que é preferível executar os comandos adprep.exe antes da instalação de um novo controlador de domínio, ainda é possível executar os comandos adprep.exe separadamente da instalação do AD DS. A versão 2012 do Windows Server do adprep. exe é executada remotamente, para que você possa executar todos os comandos necessários de um servidor que execute uma versão de 64 bits do Windows Server 2008 ou posterior.  

- **A nova instalação do AD DS se baseia no Windows PowerShell e pode ser chamada remotamente.** A nova instalação do AD DS foi integrada ao Gerenciador do Servidor; portanto, para instalar o AD DS, você pode usar a mesma interface que é utilizada na instalação de outras funções de servidor. Para os usuários do Windows PowerShell, os cmdlets de implantação do AD DS fornecem mais funcionalidade e flexibilidade. Há uma paridade funcional entre as opções de instalação por linha de comando e GUI.  
- **A nova instalação do AD DS inclui validação de pré-requisitos.** Todos os possíveis erros são identificados antes do início da instalação. Você pode corrigir antecipadamente as condições de erro, sem a preocupação resultante de uma atualização parcialmente concluída. Por exemplo, se for preciso executar adprep /domainprep, o assistente de instalação verificará se o usuário tem direitos suficientes para executar a operação.  
- **As páginas de configuração são agrupadas em uma sequência que espelha os requisitos das opções de promoção mais comuns com as opções relacionadas agrupadas em menos páginas de assistente.** Isso fornece um melhor contexto para fazer escolhas de instalação.  
- **Você pode exportar um script do Windows PowerShell contendo todas as opções que foram especificadas durante a instalação gráfica.** No final de uma instalação ou remoção, você pode exportar as configurações para um script do Windows PowerShell para uso com a automação da mesma operação.  
- **Somente a replicação essencial ocorre antes da reinicialização.** Nova opção para permitir a replicação de dados não críticos antes da reinicialização. Para obter mais informações, consulte [ADDSDeployment cmdlet arguments](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  

## <a name="BKMK_ADConfigurationWizard"></a>O assistente de configuração do Active Directory Domain Services

A partir do Windows Server 2012, o assistente de configuração do Active Directory Domain Services substitui o Assistente para Instalação do Active Directory Domain Services herdado como a opção da interface do usuário (IU) para especificar as configurações ao instalar um controlador de domínio. O Assistente de Configuração dos Serviços de Domínio Active Directory é iniciado após a conclusão do Assistente para Adicionar Funções.  

> [!WARNING]  
> O Assistente para Instalação do Active Directory Domain Services herdado (Dcpromo. exe) foi preterido a partir do Windows Server 2012.  

Em [instalar o &#40;nível de&#41;Active Directory Domain Services 100](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), os procedimentos de interface do usuário mostram como iniciar o assistente para adicionar funções para instalar os binários de função de servidor AD DS e executar o assistente de configuração de Active Directory Domain Services para concluir a instalação do controlador de domínio. Os exemplos do Windows PowerShell mostram como concluir as duas etapas usando um cmdlet de implantação do AD DS.  
  
## <a name="BKMK_NewAdprep"></a>Integração de adprep. exe

A partir do Windows Server 2012, há apenas uma versão do adprep. exe (não há nenhuma versão de 32 bits, adprep32. exe). Os comandos da Adprep são executados automaticamente conforme necessário quando você instala um controlador de domínio que executa o Windows Server 2012 em um domínio ou floresta Active Directory existente.  
  
Embora as operações de adprep sejam executadas automaticamente, é possível executar o Adprep.exe em separado. Por exemplo, se o usuário que instala o AD DS não for membro do grupo Administradores Corporativos, o que é exigido para a execução de Adprep /forestprep, poderá ser necessário executar o comando separadamente. Mas, você só precisa executar adprep. exe se estiver planejando fazer a atualização in-loco seu primeiro controlador de domínio do Windows Server 2012 (em outras palavras, você planeja atualizar no local o sistema operacional de um controlador de domínio que executa o Windows Server 2012).  
  
O Adprep. exe está localizado na pasta \support\adprep do disco de instalação do Windows Server 2012. A versão 2012 do Windows Server do adprep é capaz de executar remotamente.  
  
A versão 2012 do Windows Server do adprep. exe pode ser executada em qualquer servidor que executa uma versão de 64 bits do Windows Server 2008 ou posterior. O servidor precisa de conectividade de rede para o mestre de esquema da floresta e para o mestre de infraestrutura do domínio em que o controlador de domínio será adicionado. Se uma dessas funções estiver hospedada em um servidor que executa o Windows Server 2003, a Adprep deverá ser executada remotamente. O servidor em que o adprep é executado não precisa ser um controlador de domínio. Ele pode ser um domínio associado ou estar em um grupo de trabalho.  

> [!NOTE]  
> Se você tentar executar a versão 2012 do Windows Server do adprep. exe em um servidor que executa o Windows Server 2003, o seguinte erro será exibido:  
>   
> Adprep.exe não é um aplicativo Win32 válido.  

![Novidades](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  

Para saber mais sobre como resolver outros erros retornados pelo Adprep.exe, consulte [Known issues](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  

### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Verificação de associação a grupos com base nas funções de mestre de operações do Windows Server 2003

Para cada comando (/forestprep, /domainprep ou /rodcprep), o Adprep executa uma verificação de associação a grupos para determinar se a credencial especificada representa uma conta em determinados grupos. Para fazer essa verificação, o Adprep contata o proprietário da função de mestre de operações. Se o mestre de operações executar o Windows Server 2003 e o Adprep.exe for executado para garantir a execução da verificação de associação a grupos em todos os casos, você precisará especificar os parâmetros de linha de comando /user e /userdomain.  
  
O/User e/UserDomain são novos parâmetros para adprep. exe no Windows Server 2012. Esses parâmetros especificam o nome da conta de usuário e o domínio do usuário, respectivamente, do usuário que executa o comando adprep. O utilitário de linha de comando Adprep.exe bloqueia a especificação de /userdomain e /user, porém omitindo um deles.  
  
Contudo, as operações Adprep também podem ser executadas como parte da instalação do AD DS via Windows PowerShell ou Gerenciador do Servidor. Essas experiências compartilham a mesma implementação subjacente (Adprep. dll) como adprep. exe. As experiências do Windows PowerShell e do Gerenciador do Servidor têm entradas separadas de credenciais, o que não impõe os mesmos requisitos do adprep.exe. Com o Windows PowerShell ou com o Gerenciador do Servidor, é possível passar um valor para /user, mas não para /userdomain no adprep.dll. Se/user for especificado, mas/UserDomain não for especificado, o domínio do computador local será usado para executar a verificação. Se o computador não for um domínio associado, a associação a grupos não poderá ser verificada.  
  
Quando não é possível verificar a associação a grupos, o Adprep mostra uma mensagem de aviso nos arquivos de log do adprep e continua:  

```
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```

Se o Adprep.exe for executado sem a especificação dos parâmetros /user e /userdomain e o mestre de operações executar o Windows Server 2003, o Adprep.exe contatará um controlador de domínio no domínio do atual usuário de logon. Se o usuário de logon atual não for uma conta de domínio, o Adprep.exe não poderá executar a verificação de associação a grupos. O Adprep.exe também não poderá executar a verificação de associação a grupos se forem usadas as credenciais de cartão inteligente, mesmo que você tenha especificado /user e /userdomain.  
  
Quando o Adprep é concluído com êxito, nenhuma outra ação é necessária. Se o Adprep falhar durante a execução e apresentar erros de acesso, forneça uma conta com a associação correta. Para obter mais informações, consulte [Requisitos de credenciais para executar o Adprep.exe e instalar os Serviços de Domínio Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
### <a name="syntax-for-adprep-in-windows-server-2012"></a>Sintaxe do Adprep no Windows Server 2012

Use a sintaxe a seguir para executar o adprep separadamente em uma instalação do AD DS:  

```
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```

Use /logdsid no comando para gerar mais logs detalhados. O adprep.log fica localizado em %windir%\System32\Debug\Adprep\Logs.  

### <a name="running-adprep-using-smartcard"></a>Executando o adprep com cartão inteligente

A versão 2012 do Windows Server do adprep. exe funciona usando o cartão inteligente como credenciais, mas não há uma maneira fácil de especificar a credencial do cartão inteligente por meio da linha de comando. Uma forma de fazer isso é obter a credencial de cartão inteligente usando o cmdlet Get-Credential, do PowerShell. Depois, use o nome de usuário do objeto PSCredential retornado, que aparece como `@@...`. A senha é o PIN do cartão inteligente.  

O Adprep.exe exigirá /userdomain se /user for especificado. Para credenciais de cartão inteligente, o /userdomain deve ser o domínio da conta de usuário subjacente, representada pelo cartão inteligente.  

### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>O comando adprep /domainprep /gpprep não é executado automaticamente

O comando adprep /domainprep /gpprep não é executado como parte da instalação do AD DS. Esse comando define as permissões exigidas para a funcionalidade de modo de planejamento RSOP (Conjunto de Políticas Resultante). Para saber mais sobre esse comando, consulte o [artigo 324392 da Base de Dados de Conhecimento Microsoft](https://support.microsoft.com/kb/324392). Se for preciso executar o comando no seu domínio Active Directory, execute-o separadamente da instalação do AD DS. Se o comando já tiver sido executado na preparação de controladores de domínio de implantação que executam o Windows Server 2003 SP1 ou posterior, não será necessário executar o comando novamente.  

Você pode adicionar com segurança controladores de domínio que executam o Windows Server 2012 a um domínio existente sem executar adprep/domainprep/gpprep, mas o modo de planejamento do RSOP não funcionará corretamente.  

## <a name="BKMK_PrereqCheck"></a>Validação do pré-requisito de instalação do AD DS

O assistente de instalação do AD DS verifica se os seguintes pré-requisitos foram cumpridos antes de a instalação começar. Isso dá a você a chance de solucionar problemas que podem bloquear a instalação.  
  
Por exemplo, os pré-requisitos relacionados ao Adprep incluem:  

- Verificação de credencial do Adprep: Se for preciso executar o adprep, o assistente de instalação verificará se o usuário tem direitos suficientes para executar as operações do Adprep necessárias.  
- Verificação de disponibilidade do mestre de esquema: Se o assistente de instalação determinar que o adprep /forestprep deve ser executado, o assistente verificará se o mestre de esquema está online; se não estiver, o processo falhará.  
- Verificação de disponibilidade do mestre de infraestrutura: Se o assistente de instalação determinar que o adprep /forestprep deve ser executado, o assistente verificará se o mestre de infraestrutura está online; se não estiver, o processo falhará.

Outras verificações de pré-requisitos que foram transferidas do Assistente de Instalação do Active Directory herdado (dcpromo.exe) incluem:  

- Verificação de nome da floresta: Verifica se o nome da floresta é válido e não existe no momento.  
- Verificação de nome NetBIOS: Verifica se o nome NetBIOS fornecido é válido e se não há conflitos com nomes existentes.  
- Verificação de caminho de componente: Verifica se os caminhos para banco de dados Active Directory, logs e SYSVOL são válidos e se há espaço em disco suficiente a ser disponibilizado para eles.  
- Verificação de nome de domínio filho: Verifica se os nomes de domínio pai e dos novos domínios filho são válidos e se não há conflitos com domínios existentes.  
- Verificação de nome de domínio de árvore: Verifica se o nome da árvore especificado é válido e não existe no momento.  

## <a name="BKMK_SystemReqs"></a>Requisitos do sistema

Os requisitos do sistema para o Windows Server 2012 não foram alterados no Windows Server 2008 R2. Para obter mais informações, consulte [requisitos de sistema do Windows Server 2008 R2 com SP1](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  

Alguns recursos podem ter requisitos adicionais. Por exemplo, o recurso de clonagem do controlador de domínio virtual requer que o emulador de PDC execute o Windows Server 2012 e um computador executando o Windows Server 2012 com a função Hyper-V instalada.  

## <a name="BKMK_KnownIssues"></a>Problemas conhecidos

Esta seção lista alguns dos problemas conhecidos que afetam AD DS instalação no Windows Server 2012. Para ver mais problemas conhecidos, consulte [Solucionando problemas na implantação do controlador de domínio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  

- Se o acesso WMI ao mestre de esquema for bloqueado pelo Firewall do Windows durante a execução remota de adprep /forestprep, o erro a seguir será registrado no log de adprep, em %systemroot%\system32\debug\adprep:  

   ```
   Adprep encountered a Win32 error.
   Error code: 0x6ba Error message: The RPC server is unavailable.  
   ```

   Nesse caso, uma solução alternativa é executar o adprep /forestprep diretamente no mestre de esquema ou então executar um dos seguintes comandos para permitir o tráfego de WMI pelo Firewall do Windows.

   Para Windows Server 2008 ou posterior:

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes
   ```

   Para Windows Server 2003:

   ```
   netsh firewall set service RemoteAdmin enable
   ```

   Depois de concluído o adprep, você pode executar um destes comandos para bloquear novamente o tráfego de WMI:

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no
   ```

   ```
   netsh firewall set service remoteadmin disable
   ```

- Digite Ctrl + C para cancelar o cmdlet Install-ADDSForest. O cancelamento interrompe a instalação e todas as alterações feitas no estado do servidor serão revertidas. No entanto, após a emissão do comando de cancelamento, o controle não é devolvido ao Windows PowerShell e o cmdlet pode travar indefinidamente.  
- **A instalação de um controlador de domínio adicional usando credenciais de cartão inteligente falhará se o servidor de destino não tiver ingressado no domínio antes da instalação.**  

   A mensagem de erro apresentada nesse caso é a seguinte:  

   Não é possível se conectar ao controlador de domínio de origem de replicação *nome do controlador de domínio de origem*. (Exceção: Longonfailure: nome de usuário desconhecido ou senha incorreta)  

   Se você ingressar o servidor de destino no domínio e depois executar a instalação usando um cartão inteligente, a instalação terá êxito.  
  
- **O módulo ADDSDeployment não é executado em processos de 32 bits.** Se você estiver automatizando a implantação e a configuração do Windows Server 2012 usando um script que inclui um cmdlet ADDSDeployment e qualquer outro cmdlet que não ofereça suporte a processos nativos de 64 bits, o script poderá falhar com um erro que indica o ADDSDeployment Não é possível encontrar o cmdlet.  

   Nesse caso, é preciso executar o cmdlet ADDSDeployment separadamente do cmdlet que não dá suporte a processos nativos de 64 bits.  

- Há um novo sistema de arquivos no Windows Server 2012 chamado sistema de arquivos resiliente. Não armazene o banco de dados Active Directory, nem os arquivos de log ou o SYSVOL em um volume de dados formatado com ReFS (Sistema de Arquivos Resiliente). Para obter mais informações sobre ReFS, consulte [Building o sistema de arquivos da próxima geração para Windows: ReFS @ no__t-0.  
- No Gerenciador do Servidor, os servidores que executam AD DS ou outras funções de servidor em uma instalação Server Core e foram atualizados para o Windows Server 2012, a função de servidor pode aparecer com status vermelho, mesmo que os eventos e status sejam coletados conforme o esperado. Os servidores que executam uma instalação do Server Core de uma versão preliminar do Windows Server 2012 também podem ser afetados.  

### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>A instalação dos Serviços de Domínio Active Directory será interrompida se um erro impedir a replicação crítica

Se a instalação do AD DS encontrar um erro durante a fase de replicação crítica, a instalação poderá ser interrompida indefinidamente. Por exemplo, se erros de rede impedirem a conclusão da replicação crítica, a instalação não continuará.  
  
Se fizer a instalação usando o Gerenciador do Servidor, você poderá ver a página de andamento da instalação, que permanecerá aberta, mas nenhum erro será relatado em tela e o andamento poderá ficar inalterado por aproximadamente 15 minutos. Se usar o Windows PowerShell, o andamento mostrado na janela do Windows PowerShell não mudará durante mais de 15 minutos.  
  
Caso ocorra esse problema, verifique o arquivo dcpromo.log na pasta %systemroot%/debug, no servidor de destino. O arquivo de log geralmente indica falhas repetidas para replicação. Algumas causas conhecidas desse problema são:  

- Problemas de rede impedem a replicação crítica entre o servidor de destino que está sendo promovido e o controlador de domínio de origem da replicação.  

   Por exemplo, o dcpromo.log mostra:  

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

   Como o processo de instalação faz novas tentativas de replicação crítica indefinidamente, a instalação do controlador de domínio continuará se os problemas de rede subjacentes forem resolvidos. Investigue o problema de rede usando ferramentas como ipconfig, nslookup e netmon conforme a necessidade. Verifique se há conectividade entre o controlador de domínio que você está promovendo e o parceiro de replicação selecionado durante a instalação do AD DS. Verifique também se a resolução de nomes está funcionando.  

   Os requisitos da instalação do AD DS para conectividade de rede e resolução de nomes são validados durante a verificação de pré-requisitos, antes do início da instalação. Porém, podem surgir algumas condições de erro no período entre a ocorrência da validação de pré-requisitos e a conclusão da instalação, como se o parceiro de replicação se tornar indisponível durante a instalação.  

- Durante a instalação do controlador de domínio replicado, a conta local de Administrador do servidor de destino é especificada para as credenciais de instalação e a senha da conta local de Administrador coincide com a senha de uma conta Admin. do Domínio. Nesse caso, você pode concluir o assistente de instalação e iniciar a instalação antes de encontrar a falha de "acesso negado".  

   Por exemplo, o dcpromo.log mostra:  

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

   Se o erro for provocado pela especificação de uma conta e senha de Administrador local, para recuperar será necessário reinstalar o sistema operacional, [executar uma limpeza de metadados](https://technet.microsoft.com/library/cc816907(WS.10).aspx) da conta referente ao controlador de domínio que não pôde concluir a instalação e depois tentar instalar o AD DS novamente usando as credenciais de Administrador do Domínio. A reinicialização do servidor não corrigirá essa condição de erro porque o servidor indicará que o AD DS está instalado, embora a instalação não tenha sido concluída com êxito.  

### <a name="BKMK_nonnormalDNSNameWarning"></a>Active Directory Domain Services assistente de configuração avisa quando um nome DNS não normalizado é especificado

Se você criar um novo domínio ou floresta e especificar um nome de domínio DNS incluindo caracteres internacionalizados, mas não normalizados, o Assistente de Configuração dos Serviços de Domínio Active Directory exibirá um aviso de que as consultas DNS para o nome poderão falhar. Embora o nome de domínio DNS seja especificado na página Configuração de Implantação, o aviso aparece na página Verificação de Pré-requisitos, posteriormente no assistente.  

Se um nome de domínio DNS for especificado usando um nome não normalizado como füßball. com ou ' ΣΤ '. com (as versões normalizadas são: füssball.com e βστα. com), os aplicativos cliente que tentarem acessá-lo com o WinHTTP normalizarão o nome antes de chamar APIs de resolução de nomes. Se o usuário digitar "' ΣΤ '. com" em alguma caixa de diálogo, a consulta DNS será enviada como "βστα. com" e nenhum servidor DNS fará a correspondência com um registro de recurso para "' ΣΤ '. com". O usuário não poderá resolver o nome.  

O exemplo a seguir explica um dos problemas que podem surgir durante o uso de um nome IDN não normalizado:  

1. O domínio que usa um nome não normalizado é criado e registrado no servidor DNS: füßball. com  
2. O computador "NPS" é Unido ao domínio e obtém seu nome registrado: NPS. füßball. com  
3. Um aplicativo cliente tenta se conectar ao servidor NPS. füßball. com  
4. O aplicativo cliente tenta resolver o nome NPS. füßball. com chamando APIs de resolução de nomes.  
5. Devido à normalização, o nome é convertido em nps.füssball.com e é consultado pela conexão como NPS. füßball. com  
6. O aplicativo cliente não pode resolver o nome, pois o nome registrado é NPS. füßball. com  

Se o aviso for exibido na página Verificação de Pré-requisitos, no Assistente de Configuração dos Serviços de Domínio Active Directory, volte para a página Configuração de Implantação e especifique um nome de domínio DNS normalizado. Ao instalar um novo domínio usando o Windows PowerShell, especifique um nome DNS para a opção -DomainName.  

Para saber mais sobre IDNs, consulte [Lidando com IDNs (Nomes de Domínio Internacionalizados)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
