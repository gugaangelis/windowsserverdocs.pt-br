---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar a proteção de bloqueio de Extranet do AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 03/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dd6dea2fb8a16bfdbe93f93fbdd1dc5ac47af4be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189900"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>Bloqueio de Extranet e bloqueio inteligente de Extranet do AD FS

## <a name="overview"></a>Visão geral

Bloqueio de extranet inteligente (ESL) protege os usuários experimentem o bloqueio de conta de extranet de atividades mal-intencionadas.  

ESL permite que o AD FS diferenciar entre tentativas de logon de uma localização familiar para um usuário e tentativas de logon do que pode ser um invasor. O AD FS pode bloquear os invasores, permitindo que os usuários válidos continuam a usar suas contas. Isso impede e protege contra determinadas classes de ataques spray de senha de logon do usuário e de negação de serviço. ESL está disponível para o AD FS no Windows Server 2016 e é incorporado ao AD FS no Windows Server 2019. 

ESL só está disponível para o nome de usuário e quais são fornecidos por meio da extranet com Proxy de aplicativo Web ou um suporte as solicitações de autenticação de senha 3ª proxy de terceiros.   

## <a name="additional-features-in-ad-fs-2019"></a>Recursos adicionais do AD FS de 2019 
Bloqueio inteligente de extranet do AD FS 2019 adiciona as seguintes vantagens em comparação comparadas o AD FS 2016: 
- Definir limites de bloqueio independentes para familiarizados e locais para que usuários em locais bons conhecidos possam ter mais espaço para erro que as solicitações de locais suspeitos 
- Habilite o modo de auditoria para o bloqueio inteligente enquanto continua a impor o comportamento de bloqueio flexível anterior. Isso permite que você saiba mais sobre locais familiares do usuário e ainda estará protegido pelo recurso de bloqueio de extranet que está disponível a partir do AD FS 2012R2.  

## <a name="how-it-works"></a>Como ele funciona 
### <a name="configuration-information"></a>informações de configuração 
Quando ESL está habilitada, uma nova tabela no banco de dados de artefato, AdfsArtifactStore.AccountActivity, é criada e um nó é selecionado no farm do AD FS como o mestre de "Atividade de usuário". Em uma configuração de WID, esse nó é sempre o nó primário. Em uma configuração de SQL, um nó é selecionado para ser o mestre de atividade do usuário.  

Para exibir o nó selecionado como o mestre de atividade do usuário. Get-AdfsFarmInformation.FarmRoles 

Todos os nós secundários entre em contato com o nó mestre em cada novo logon por meio da porta 80 para saber o valor mais recente das contagens de senha incorreta e novos valores de local familiar e atualizar esse nó após o logon é processado. 

![configuração](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Se o nó secundário não pode contatar o mestre, ele gravará eventos de erro no log do administrador do AD FS. Autenticações continuarão a ser processada, mas o AD FS irá gravar apenas o estado atualizado localmente. AD FS tentará entrar em contato com o mestre de cada 10 minutos e alternará para o mestre depois que o mestre está disponível. 

### <a name="terminology"></a>Terminologia 
- **FamiliarLocation**: Durante uma solicitação de autenticação, ESL verifica que todos os IPs de apresentada. Esses IPs será uma combinação de IP de rede, encaminhado IP e o IP de x-forwarded-for opcional. Se a solicitação for bem-sucedida, todos os IPs são adicionados à tabela de atividades de conta como "IPs familiares". Se a solicitação tiver todos os IPs presentes no "IPs familiar", a solicitação será tratada como um local de "Familiar".
- **UnknownLocation**: Se uma solicitação que chega tem pelo menos um IP não está presente na lista de "FamiliarLocation" existente, a solicitação será tratada como um local de "Desconhecido". Isso serve para lidar com cenários de proxy como a autenticação herdado do Exchange Online em que o Exchange Online endereços lidar com solicitações bem-sucedidas e com falha.  
- **badPwdCount**: Um valor que representa o número de vezes que uma senha incorreta foi enviada e a autenticação foi bem-sucedida. Para cada usuário, os contadores separados são mantidos por locais familiares e de locais desconhecidos. 
- **UnknownLockout**: Um valor booliano por usuário se o usuário será impedido de acessar de locais desconhecidos. Esse valor é calculado com base nos valores de ExtranetLockoutThreshold e badPwdCountUnfamiliar. 
- **ExtranetLockoutThreshold**: Esse valor determina o número máximo de tentativas de senha incorreta. Quando o limite for atingido, o ADFS rejeitará as solicitações da extranet até que a janela de Observação tenha passado.
- **ExtranetObservationWindow**: Esse valor determina a duração em que as solicitações de nome de usuário e senha de locais desconhecidos estão bloqueadas. Quando a janela tiver passado, o ADFS começará a realizar a autenticação de nome de usuário e senha de locais desconhecidos novamente. 
- **ExtranetLockoutRequirePDC**: Quando habilitada, o bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado, o bloqueio de extranet fará fallback para outro controlador de domínio caso o controlador de domínio primário não está disponível.  
- **ExtranetLockoutMode**: Controles de log somente modo de vs impostas inteligente de bloqueio de Extranet 
    - **ADFSSmartLockoutLogOnly**: Extranet bloqueio inteligente está habilitado, mas o AD FS será apenas escrever admin e eventos de auditoria, mas será rejeitado não as solicitações de autenticação. Esse modo destina-se a ser habilitada inicialmente para FamiliarLocation sejam preenchidos antes de 'ADFSSmartLockoutEnforce' está habilitada.
    - **ADFSSmartLockoutEnforce**: Suporte completo para o bloqueio de solicitações de autenticação desconhecido quando os limites são atingidos. 

Há suporte para endereços IPv4 e IPv6. 

### <a name="anatomy-of-a-transaction"></a>Anatomia de uma transação 
- **Verificação de pré-autenticação**: Durante uma solicitação de autenticação, ESL verifica que todos os IPs de apresentada. Esses IPs será uma combinação de IP de rede, encaminhado IP e o IP de x-forwarded-for opcional. Nos logs de auditoria, esses IPs estão listados no <IpAddress> campo na ordem de x-ms-forwarded-client-ip, x-forwarded-for, x-ms-proxy-cliente-ip. 
 
  ADFS com base nesses IPs, determina se a solicitação é de uma localização familiar e não familiar e, em seguida, verifica se o respectivo badPwdCount é menor que o limite de conjunto ou se a última **falha** tentativa é aconteceu maior que o intervalo de tempo da janela de observação. Se uma das seguintes condições for verdadeira, o ADFS permite que essa transação para processamento adicional e validação de credenciais. Se ambas as condições forem falsas, a conta estiver em um estado bloqueado até que a janela de Observação passa. Depois que a janela de Observação passa, o usuário é permitido uma tentativa de autenticar. Observe que de 2019, ADFS verificará com relação ao limite de limite apropriado com base em se o endereço IP corresponde a uma localização familiar ou não.
- **Logon bem-sucedido**: Se o log-in for bem-sucedida, os IPs da solicitação são adicionados à lista IP do usuário local familiar.  
- **Falha no logon**: Se o log-in falha o badPwdCount é aumentada. O usuário entra em um estado de bloqueio se o invasor envia mais senhas incorretas no sistema que o limite permite. (badPwdCount > ExtranetLockoutThreshold)  

![configuração](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

O valor de "UnknownLockout" será igual a true quando a conta está bloqueada. Isso significa que badPwdCount do usuário em que o limite ou seja, alguém tentou mais senhas que foram permitido pelo sistema. Nesse estado, existem 2 maneiras de um usuário válido que pode fazer logon. 
- O usuário deve aguardar o tempo de ObservationWindow decorrer ou
- Para redefinir o estado de bloqueio, redefina o badPwdCount como zero com 'Redefinição ADFSAccountLockout'. 

Se nenhum redefinições ocorrerem, a conta poderão ser uma tentativa de senha única no AD para cada janela de observação. A conta retornará ao estado bloqueado depois disso serão reiniciado tentativa e a janela de observação. O valor de badPwdCount só será redefinido automaticamente após um logon de senha com êxito. 

### <a name="log-only-mode-versus-enforce-mode"></a>Modo somente log versus modo 'Aplicar' 
A tabela AccountActivity é preenchida durante o modo 'Somente Log' e 'Aplicar' modo. Se o modo 'Somente Log' é ignorado e ESL é movido diretamente para o modo 'Aplicar' sem o período de espera recomendada, os IPs familiarizar os usuários não será conhecido ao ADFS. Nesse caso, ESL seria se comportam como 'ADBadPasswordCounter', bloqueando o tráfego de usuário legítimo potencialmente se a conta de usuário estiver sob um ataque de força bruta de Active Directory. Se o modo 'Somente Log' é ignorado e o usuário insere um bloqueado estado com "UnknownLockout" = TRUE e tenta entrar com uma senha válida de um IP que não está na lista IP "familiar", em seguida, eles não poderão entrar. É recomendado somente para log de 3 a 7 dias para evitar esse cenário. Se as contas são ativamente sob ataque, um mínimo de 24 horas de modo 'Log ' somente é necessário para evitar bloqueios para que os usuários legítimos.  

## <a name="extranet-smart-lockout-configuration"></a>Configuração de extranet de bloqueio inteligente  
 
### <a name="prerequisites-for-ad-fs-2016"></a>Pré-requisitos para o AD FS 2016 
 
1. **Instalar atualizações em todos os nós no farm**

   Primeiro, verifique se todos os servidores do AD FS do Windows Server 2016 são atualizados a partir das atualizações do Windows de junho de 2018 e se o farm do AD FS 2016 está em execução no nível de comportamento de farm 2016.
1. **Verifique as permissões** 

   Bloqueio de extranet do Smart exige que o gerenciamento remoto do Windows esteja habilitado em todos os servidores do AD FS.
3. **Atualizar as permissões de banco de dados de artefato** 
 
   Bloqueio de extranet do smart requer a conta de serviço do AD FS tenha permissões para criar uma nova tabela no banco de dados de artefato do AD FS. Fazer logon em qualquer servidor do AD FS como um administrador do AD FS e, em seguida, conceda essa permissão, executando os seguintes comandos em uma janela de Prompt de comando do PowerShell: 

   ``` powershell
   PS C:\>$cred = Get-Credential 
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred 
   ``` 
   >[!NOTE]
   >O espaço reservado $cred é uma conta que tenha permissões de administrador do AD FS. Isso deve fornecer as permissões de gravação para criar a tabela. 

   Os comandos acima podem falhar devido à falta de permissão suficiente porque seu farm do AD FS usando o SQL Server e a credencial fornecida acima não tem permissão de administrador no SQL server. Nesse caso, você pode configurar permissões de banco de dados manualmente no banco de dados do SQL Server executando o comando a seguir quando você está conectado ao banco de dados AdfsArtifactStore. 
    ```  
    # when prompted with “Are you sure you want to perform this action?”, enter Y. 

    [CmdletBinding(SupportsShouldProcess=$true,ConfirmImpact = 'High')] 
    Param() 

    $fileLocation = "$env:windir\ADFS\Microsoft.IdentityServer.Servicehost.exe.config" 

    if (-not [System.IO.File]::Exists($fileLocation)) 
    { 
    write-error "Unable to open ADFS configuration file." 
    return 
    } 

    $doc = new-object Xml 
    $doc.Load($fileLocation) 
    $connString = $doc.configuration.'microsoft.identityServer.service'.policystore.connectionString 
    $connString = $connString -replace "Initial Catalog=AdfsConfigurationV[0-9]*", "Initial Catalog=AdfsArtifactStore" 

    if ($PSCmdlet.ShouldProcess($connString, "Executing SQL command sp_addrolemember 'db_owner', 'db_genevaservice' ")) 
    { 
    $cli = new-object System.Data.SqlClient.SqlConnection 
    $cli.ConnectionString = $connString 
    $cli.Open() 

    try 
    {     

    $cmd = new-object System.Data.SqlClient.SqlCommand 
    $cmd.CommandText = "sp_addrolemember 'db_owner', 'db_genevaservice'" 
    $cmd.Connection = $cli 
    $rowsAffected = $cmd.ExecuteNonQuery()  
    if ( -1 -eq $rowsAffected ) 
    { 
    write-host "Success" 
    } 
    } 
    finally 
    { 
    $cli.CLose() 
    } 
    } 
    ``` 

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verifique se o que log de auditoria de segurança do AD FS está habilitado 
Este recurso faz uso de auditoria de segurança de logs, portanto, a auditoria devem ser habilitados no AD FS, bem como a política local em todos os servidores do AD FS. 
 
### <a name="configuration-instructions"></a>Instruções de configuração 
Bloqueio de extranet do inteligente usa a propriedade do ADFS **ExtranetLockoutEnabled**. Essa propriedade foi usada anteriormente para o controle de "Bloqueio de Extranet reversível" no Server 2012 R2. Se o bloqueio de Extranet do reversível foi habilitado, para exibir a configuração da propriedade atual, execute ` Get-AdfsProperties` . 

### <a name="configuration-recommendations"></a>Recomendações de configuração 
Ao configurar o bloqueio inteligente de Extranet, siga as práticas recomendadas para configuração de limites:  

`ExtranetObservationWindow (new-timespan -Minutes 30)` 

`ExtranetLockoutThreshold: – 2x AD Threshold Value` 

Valor do AD: 20, ExtranetLockoutThreshold: 10 

Bloqueio do Active Directory funciona independentemente do bloqueio inteligente de Extranet. No entanto, se o bloqueio do Active Directory estiver ativado, ExtranetLockoutThreshold no AD FS < limite de bloqueio de conta do AD 

`ExtranetLockoutRequirePDC - $false`
 
Quando habilitada, o bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado e configurado como false, o bloqueio de extranet fará fallback para outro controlador de domínio caso o controlador de domínio primário não está disponível. 
 
Para definir essa propriedade executar: 

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false 
```
### <a name="enable-log-only-mode"></a>Habilitar o modo de Log 
 
No modo somente de log, o AD FS preenche as informações de localização familiar de usuários e grava eventos de auditoria de segurança, mas não bloqueia todas as solicitações. Esse modo é usado para validar que o bloqueio inteligente está em execução e para habilitar o AD FS para "saber" locais familiares para os usuários antes de habilitar o "modo de imposição". Como o AD FS aprende, ele armazena a atividade de logon por usuário (em modo somente de log ou modo de imposição). Defina o comportamento de bloqueio para fazer logon apenas executando o cmdlet a seguir.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly` 

Modo de log somente destina-se a ser um estado temporário para que o sistema possa aprender o comportamento de logon antes de introduzir a imposição de bloqueio com o comportamento de bloqueio inteligente. A duração recomendada para o modo de log é de 3 a 7 dias. Se as contas são ativamente sob ataque, o modo de log deve ser executado por um mínimo de 24 horas. 

No AD FS 2016, se o comportamento de 'Bloqueio de Extranet reversível' 2012R2 estiver habilitado antes de habilitar o bloqueio de Extranet inteligente, o modo de Log desabilitará o comportamento de 'Bloqueio de Extranet reversível'. Bloqueio inteligente do AD FS não bloqueará os usuários no modo de Log. No entanto, no local AD pode bloquear o usuário com base na configuração do AD. Examine as políticas de bloqueio do AD para saber como local AD pode usuários de bloqueio. 

No AD FS 2019, uma vantagem adicional deve ser capaz de habilitar o modo de log somente para o bloqueio inteligente enquanto continua a impor o comportamento flexível bloqueio anterior usando o Powershell abaixo. 

`Set-AdfsProperties -ExtranetLockoutMode 3` 
 
Para o novo modo entrem em vigor, reinicie o serviço do AD FS em todos os nós no farm 

`Restart-service adfssrv` 
 
Depois que o modo é configurado, você pode habilitar o bloqueio inteligente usando o parâmetro EnableExtranetLockout 
 
`Set-AdfsProperties -EnableExtranetLockout $true` 

### <a name="enable-enforce-mode"></a>Habilitar o modo de imposição 
 
Depois que você estiver familiarizado com a janela de observação e de limite de bloqueio, ESL pode ser movido para "modo de imposição" usando o seguinte cmdlet PSH: 

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce` 

Para o novo modo entrem em vigor, reinicie o serviço do AD FS em todos os nós no farm usando o comando a seguir. 

`Restart-service adfssrv` 

## <a name="manage-user-account-activity"></a>Gerenciar a atividade de conta de usuário 
O AD FS fornece três cmdlets para gerenciar os dados de atividade de conta. Esses cmdlets se conectam automaticamente para o nó no farm que mantém a função principal. 
>[!NOTE] 
>Administração JEA (Just Enough) pode ser usado para delegar commandlets do AD FS para redefinir os bloqueios de conta. Por exemplo, o pessoal de assistência técnica pode ser permissões delegadas para usar os commandlets ESL. Para obter informações sobre como delegar permissões para usar esses cmdlets, consulte [delegado AD FS Powershell Commandlet acesso a usuários não-administrador](delegate-ad-fs-pshell-access.md)

Esse comportamento pode ser substituído, passando o parâmetro - Server. 

- Get-ADFSAccountActivity 

  Leia a atividade da conta atual para uma conta de usuário. O cmdlet sempre automaticamente se conecta ao mestre do farm usando o ponto de extremidade REST da atividade de conta. Portanto, todos os dados devem sempre ser consistentes. 

  Exemplo: Get-ADFSAccountActivity user@contoso.com 

  Propriedades: 
    - BadPwdCountFamiliar: Incrementado quando uma autenticação é bem-sucedida de um local conhecido.
    - BadPwdCountUnknown: Incrementado quando uma autenticação for bem-sucedida de um local desconhecido
    - LastFailedAuthFamiliar: Se a autenticação foi bem-sucedida de uma localização familiar, LastFailedAuthUnknown é definida para hora de autenticação sem êxito 
    - LastFailedAuthUnknown: Se a autenticação foi bem-sucedida de um local desconhecido, LastFailedAuthUnknown é definida para hora de autenticação sem êxito 
    - FamiliarLockout: Valor booliano que será "True" se "BadPwdCountFamiliar" > ExtranetLockoutThreshold 
    - UnknownLockout: Valor booliano que será "True" se "BadPwdCountUnknown" > ExtranetLockoutThreshold  
    - FamiliarIPs: o máximo de 20 IPs que são familiares para o usuário. Quando isso for excedido o IP na lista de mais antigo será removido. 
-    Set-ADFSAccountActivity 
     
     Adiciona novos locais familiares. A conhecida lista IP tem um máximo de 20 entradas, se isso for excedido, o IP na lista de mais antigo será removido. 

     Exemplo: Conjunto ADFSAccountActivity user@contoso.com - AdditionalFamiliarIps "1.2.3.4"

- Reset-ADFSAccountLockout 
   
  Redefine o contador de bloqueios de conta de usuário para cada local Familiar (badPwdCountFamiliar) ou contadores de um local desconhecido (badPwdCountUnfamiliar). Redefinindo um contador, o valor "FamiliarLockout" ou "UnfamiliarLockout" será atualizado, pois a contador será inferior ao limite.  

   Exemplo: Redefinição ADFSAccountLockout user@contoso.com -exemplo de localização Familiar:  Redefinição ADFSAccountLockout user@contoso.com -local desconhecido 

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>O log de eventos e informações de atividade de usuário para o bloqueio de Extranet do AD FS 

### <a name="connect-health"></a>Connect Health 
É a maneira recomendada para monitorar a atividade de conta de usuário por meio do Connect Health. Connect Health gera relatórios que pode ser baixado em IPs arriscados e tentativas de senha incorreta. Cada item no relatório IP arriscado mostra informações agregadas sobre atividades com falha do AD FS sign-in que excedem o limite designado. Notificações de email podem ser definidas para alertar os administradores assim que isso ocorre com as configurações de email personalizáveis. Para obter mais informações e instruções de instalação, visite o [documentação do Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-adfs). 

### <a name="ad-fs-extranet-smart-lockout-events"></a>Eventos do AD FS o bloqueio inteligente Extranet. 
Para eventos de bloqueio inteligente de Extranet a serem gravados, ESL deve estar habilitado no modo 'somente log' ou 'Aplicar' e a auditoria de segurança do AD FS está habilitada. O AD FS gravará eventos de bloqueio de extranet para o log de auditoria de segurança: 
- Quando um usuário está bloqueado out (atingir o limite de bloqueio de tentativas de logon malsucedida) 
- Quando o AD FS recebe uma tentativa de logon para um usuário que já está em estado de bloqueio 

No modo somente de log, você pode verificar o log de auditoria de segurança para eventos de bloqueio. Para todos os eventos encontrados, você pode verificar o estado do usuário usando o cmdlet Get-ADFSAccountActivity para determinar se o bloqueio ocorreu de endereços IP familiares ou desconhecidos e verifique a lista de endereços IP familiares para o usuário. 


|ID de evento|Descrição|
|-----|-----| 
|1203|Esse evento é criado para cada tentativa de senha incorreta. Assim que o badPwdCount atinge o valor especificado em ExtranetLockoutThreshold, a conta será bloqueada no AD FS para a duração especificada no ExtranetObservationWindow.</br>ID da atividade: %1</br>XML: %2|
|1201|Esse evento é gravado cada vez que um usuário está bloqueado. </br>ID da atividade: %1</br>XML: %2| 
|557 (2019 ADFS)| Ocorreu um erro ao tentar se comunicar com o serviço de rest do armazenamento de conta no nó %1. Quando se trata de um farm de WID de nó primário pode estar offline. Quando se trata de um farm de SQL AD FS selecionará automaticamente um novo nó para hospedar a função de mestre do repositório de usuário.| 
|562 (2019 ADFS)|Ocorreu um erro quando communcating com a conta armazena o ponto de extremidade no servidor %1.</br>Mensagem de exceção: %2| 
|563 (2019 ADFS)|Ocorreu um erro ao calcular o status de bloqueio de extranet. Porque o valor da %1 Configurando a autenticação será permitida para esse usuário e a emissão de token continuará. Quando se trata de um farm de WID de nó primário pode estar offline. Quando se trata de um farm de SQL AD FS selecionará automaticamente um novo nó para hospedar a função de mestre do repositório de usuário.</br>Nome da conta de armazenamento de servidor: %2</br>Id de usuário: %3</br>Mensagem de exceção: %4|
|512|A conta para o seguinte usuário está bloqueada. Uma tentativa de logon está sendo permitida por causa da configuração do sistema.</br>ID da atividade: %1 </br>Usuário: %2 </br>Cliente IP: %3 </br>Contagem de senha inválida: %4  </br>Última tentativa de senha incorreta: %5|
|515|A seguinte conta de usuário estava em um estado bloqueado e a senha correta apenas foi fornecida. Essa conta pode ser comprometida.</br>Dados adicionais </br>ID da atividade: %1 </br>Usuário: %2 </br>Cliente IP: %3 |
|516|A seguinte conta de usuário foi bloqueada devido a muitas tentativas de senha incorreta.</br>ID da atividade: %1  </br>Usuário: %2  </br>Cliente IP: %3  </br>Contagem de senha inválida: %4  </br>Última tentativa de senha incorreta: %5|

## <a name="esl-frequently-asked-questions"></a>Perguntas frequentes sobre o ESL 
 
**Será um farm de AD FS usando o bloqueio de Extranet inteligente no modo de imposição topar com bloqueios de usuário mal-intencionado?**  

R: Se o bloqueio inteligente do ADFS é definido como 'modo de imposição' e em seguida, você nunca verá a conta do usuário legítimo bloqueada por força bruta ou de negação de serviço. A única maneira de um bloqueio de conta mal-intencionado pode impedir que um usuário de entrada é se o ator mal-intencionado tem a senha do usuário ou pode enviar solicitações de um endereço IP (familiar) bom conhecido para o usuário.  
 
**O que acontece ESL está habilitado e o ator mal-intencionado tem uma senha de usuário?**  

R: O objetivo típico do cenário de ataque de força bruta é adivinhar uma senha e entrar com êxito.  Se um usuário de captura ou se uma senha é acertada, em seguida, o recurso ESL não bloqueará o acesso, pois a entrada atenderá aos critérios de "êxito" da senha correta mais novo IP. O IP de atores ruins, em seguida, será exibido como um "familiar". A melhor atenuação neste cenário é para limpar a atividade do usuário no AD FS e para exigir autenticação multifator para os usuários. É altamente recomendável instalar a proteção de senha do AAD que garante que adivinhar senhas não entrar em um sistema. 
 
**Se meu usuário nunca tiver entrado com êxito de um IP e, em seguida, tentativas com senha incorreta algumas vezes eles poderão fazer logon depois de eles, por fim, digitem a senha corretamente?**  

R: Se um usuário envia várias senhas incorretas (incorretamente digitando ou seja, legitimamente) e na tentativa a seguir obtém a senha correta, o usuário terá êxito imediatamente para entrar no aplicativo.  Isso limpará a contagem de senha incorreta e adicionar esse IP à lista FamiliarIPs.  No entanto, se eles ficarem acima do limite de logons com falha do local desconhecido, eles entrará no estado de bloqueio e será necessário esperar após a janela de observação e entrar com uma senha válida ou exigem a intervenção do administrador para redefinir a sua conta.  
 
**ESL funciona na intranet muito?**    R: Se os clientes se conectarem diretamente aos servidores do AD FS e não por meio de servidores de Proxy de aplicativo Web, em seguida, o comportamento de ESL não se aplicará.  
 
**Estou vendo endereços IP da Microsoft no campo de IP do cliente. ESL bloco EXO transmitidas por proxy ataques de força bruta?**   

R: ESL funcionará bem para impedir que o Exchange Online ou outros cenários de ataque de força bruta de autenticação herdados. Uma autenticação herdada tem uma "ID de atividade" de 00000000-0000-0000-0000-000000000000. Esses ataques, o ator mal-intencionado está aproveitando o Exchange Online (também conhecida como autenticação herdados) de autenticação básica para que o endereço IP do cliente seja exibido como o Microsoft. Os servidores Exchange online no proxy de nuvem a verificação de autenticação em nome do cliente do Outlook. Nesses cenários, o endereço IP do remetente mal-intencionado será o x-ms-forwarded--ip do cliente e servidor Microsoft Exchange Online que IP será o valor de x-ms-client-ip. Bloqueio de extranet do Smart verifica os IPs de rede, encaminhado IPs, o x-forwarded--IP do cliente e o valor de x-ms-client-ip. Se a solicitação for bem-sucedida, todos os IPs são adicionados à lista de ferramentas familiar. Se uma solicitação chega e qualquer um dos IPs apresentada não estão na lista familiar, em seguida, a solicitação será marcada como desconhecida. O usuário familiar será capaz de entrar com êxito enquanto as solicitações de locais desconhecidos serão bloqueadas.  


## <a name="additional-references"></a>Referências adicionais  
[Práticas recomendadas para proteger os serviços de Federação do Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
