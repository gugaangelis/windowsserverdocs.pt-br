---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar a proteção de bloqueio inteligente Extranet do AD FS
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2363e7cd696275de47c70c3ef3a2316d43b487db
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87863987"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>Bloqueio de Extranet do AD FS e Extranet Smart Lockout

## <a name="overview"></a>Visão geral

O ESL (bloqueio inteligente de extranet) protege os usuários de experimentarem o bloqueio de conta de extranet de atividades mal-intencionadas.  

O ESL permite que AD FS diferenciar entre as tentativas de entrada de um local familiar para um usuário e tentativas de entrada do que pode ser um invasor. AD FS pode bloquear os invasores enquanto permite que usuários válidos continuem a usar suas contas. Isso impede e protege contra a negação de serviço e determinadas classes de ataques de irrigação de senha no usuário. O ESL está disponível para AD FS no Windows Server 2016 e é incorporado ao AD FS no Windows Server 2019.

O ESL só está disponível para solicitações de autenticação de nome de usuário e senha que são provenientes da extranet com o proxy de aplicativo Web ou um proxy de terceiros. Qualquer proxy de terceiros deve dar suporte ao protocolo MS-ADFSPIP para ser usado no lugar do proxy de aplicativo Web, como [F5 Big-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191). Consulte a documentação de proxy de terceiros para determinar se o proxy dá suporte ao protocolo MS-ADFSPIP.   

## <a name="additional-features-in-ad-fs-2019"></a>Recursos adicionais no AD FS 2019
O bloqueio inteligente de extranet no AD FS 2019 adiciona as seguintes vantagens em comparação com AD FS 2016:
- Defina limites de bloqueio independentes para locais familiares e desconhecidos para que os usuários em locais bons conhecidos possam ter mais espaço para o erro do que solicitações de locais suspeitos
- Habilite o modo de auditoria para bloqueio inteligente enquanto continua a impor o comportamento de bloqueio flexível anterior. Isso permite que você saiba mais sobre os locais familiares do usuário e que ainda serão protegidos pelo recurso de bloqueio de extranet disponível em AD FS 2012R2.  

## <a name="how-it-works"></a>Como funciona
### <a name="configuration-information"></a>Informações da configuração
Quando ESL está habilitado, uma nova tabela no banco de dados de artefato, AdfsArtifactStore. AccountActivity, é criada e um nó é selecionado no farm de AD FS como o mestre de "atividade do usuário". Em uma configuração de WID, esse nó é sempre o nó primário. Em uma configuração do SQL, um nó é selecionado para ser o mestre de atividade do usuário.  

Para exibir o nó selecionado como mestre de atividade do usuário. (Get-AdfsFarmInformation). FarmRoles

Todos os nós secundários entrarão em contato com o nó mestre em cada logon novo pela porta 80 para saber o valor mais recente das contagens de senha inadequadas e novos valores de local familiares, e atualizará esse nó depois que o logon for processado.

![configuração](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Se o nó secundário não puder entrar em contato com o mestre, ele gravará eventos de erro no log do AD FS admin. As autenticações continuarão a ser processadas, mas AD FS gravará apenas o estado atualizado localmente. AD FS tentará entrar em contato com o mestre a cada 10 minutos e alternará de volta para o mestre quando o mestre estiver disponível.

### <a name="terminology"></a>Terminologia
- **FamiliarLocation**: durante uma solicitação de autenticação, o ESL verifica todos os IPs apresentados. Esses IPs serão uma combinação de IP de rede, IP encaminhado e o x-Forwarded-for IP opcional. Se a solicitação for bem-sucedida, todos os IPs serão adicionados à tabela de atividades da conta como "IPs familiares". Se a solicitação tiver todos os IPs presentes nos "IPs familiares", a solicitação será tratada como um local "familiar".
- **UnknownLocation**: se uma solicitação que venha tem pelo menos um IP não está presente na lista de "FamiliarLocation" existente, a solicitação será tratada como um local "desconhecido". Isso é para lidar com cenários de proxy, como a autenticação herdada do Exchange Online, na qual os endereços do Exchange Online lidam com solicitações bem-sucedidas e com falha.  
- **badPwdCount**: um valor que representa o número de vezes que uma senha incorreta foi enviada e a autenticação não foi bem-sucedida. Para cada usuário, os contadores separados são mantidos para locais familiares e locais desconhecidos.
- **UnknownLockout**: um valor booliano por usuário se o usuário estiver bloqueado do acesso de locais desconhecidos. Esse valor é calculado com base nos valores badPwdCountUnfamiliar e ExtranetLockoutThreshold.
- **ExtranetLockoutThreshold**: esse valor determina o número máximo de tentativas de senha inválidos. Quando o limite for atingido, o ADFS rejeitará as solicitações da extranet até que a janela de observação tenha passado.
- **ExtranetObservationWindow**: esse valor determina a duração em que as solicitações de nome de usuário e senha de locais desconhecidos são bloqueadas. Quando a janela for aprovada, o ADFS começará a executar a autenticação de nome de usuário e senha de locais desconhecidos novamente.
- **ExtranetLockoutRequirePDC**: quando habilitado, o bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado, o bloqueio de extranet fará fallback para outro controlador de domínio, caso o PDC não esteja disponível.  
- **ExtranetLockoutMode**: controla somente o modo de aplicação do bloqueio inteligente de extranet no log
    - **ADFSSmartLockoutLogOnly**: o bloqueio inteligente de extranet está habilitado, mas AD FS gravará apenas os eventos de administrador e auditoria, mas não rejeitará as solicitações de autenticação. Esse modo deve ser habilitado inicialmente para que FamiliarLocation seja preenchido antes de ' ADFSSmartLockoutEnforce ' ser habilitado.
    - **ADFSSmartLockoutEnforce**: suporte completo para bloquear solicitações de autenticação desconhecidas quando os limites são atingidos.

Há suporte para endereços IPv4 e IPv6.

### <a name="anatomy-of-a-transaction"></a>Anatomia de uma transação
- **Verificação de pré-autenticação**: durante uma solicitação de autenticação, o ESL verifica todos os IPs apresentados. Esses IPs serão uma combinação de IP de rede, IP encaminhado e o x-Forwarded-for IP opcional. Nos logs de auditoria, esses IPs são listados no <IpAddress> campo na ordem de x-MS-encaminhar-Client-IP, x-encaminhád-for, x-MS-proxy-Client-IP.

  Com base nesses IPs, o ADFS determina se a solicitação é de um local familiar ou desconhecido e, em seguida, verifica se o respectivo badPwdCount é menor que o limite de limite definido ou se a última tentativa de **falha** ocorreu mais do que o intervalo de tempo da janela de observação. Se uma dessas condições for verdadeira, o ADFS permitirá essa transação para processamento adicional e validação de credenciais. Se ambas as condições forem falsas, a conta já estará em um estado bloqueado até que a janela de observação passe. Após a janela de observação passar, o usuário tem permissão para se autenticar. Observe que, em 2019, o ADFS verificará o limite de limite apropriado com base em se o endereço IP corresponder a um local familiar ou não.
- **Logon bem-sucedido**: se o logon for bem-sucedido, os IPs da solicitação serão adicionados à lista de IPS de local familiar do usuário.  
- **Logon com falha**: se o logon falhar, o badPwdCount será aumentado. O usuário entrará em um estado de bloqueio se o invasor enviar mais senhas inadequadas ao sistema do que o limite permite. (badPwdCount > ExtranetLockoutThreshold)  

![configuração](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

O valor "UnknownLockout" será igual a true quando a conta for bloqueada. Isso significa que o badPwdCount do usuário está acima do limite, ou seja, alguém tentou mais senhas do que o permitido pelo sistema. Nesse estado, há duas maneiras pelas quais um usuário válido pode fazer logon.
- O usuário deve aguardar o tempo de ObservationWindow ser decorrido ou
- Para redefinir o estado de bloqueio, redefina o badPwdCount para zero com ' Reset-ADFSAccountLockout '.

Se nenhuma redefinição ocorrer, a conta será permitida uma única tentativa de senha no AD para cada janela de observação. A conta voltará ao estado bloqueado após essa tentativa e a janela de observação será reiniciada. O valor de badPwdCount só será redefinido automaticamente após um logon de senha bem-sucedido.

### <a name="log-only-mode-versus-enforce-mode"></a>Modo somente de log versus modo ' impor '
A tabela AccountActivity é populada durante o modo ' somente log ' e o modo ' impor '. Se o modo ' somente log ' for ignorado e ESL for movido diretamente para o modo ' impor ' sem o período de espera recomendado, os IPs familiares dos usuários não serão conhecidos pelo ADFS. Nesse caso, ESL se comportaria como ' ADBadPasswordCounter ', potencialmente bloqueando o tráfego de usuários legítimos se a conta de usuário estiver sob um ataque de força bruta ativo. Se o modo ' somente log ' for ignorado e o usuário inserir um estado bloqueado com "UnknownLockout" = TRUE e tentar entrar com uma senha de boa qualidade de um IP que não esteja na lista de IPs "familiar", ele não poderá se conectar. O modo somente de log é recomendado por 3-7 dias para evitar esse cenário. Se as contas estiverem ativamente sob ataque, no mínimo 24 horas do modo ' somente log ' serão necessárias para evitar bloqueios para usuários legítimos.  

## <a name="extranet-smart-lockout-configuration"></a>Configuração de bloqueio inteligente da extranet  

### <a name="prerequisites-for-ad-fs-2016"></a>Pré-requisitos para o AD FS 2016

1. **Instalar atualizações em todos os nós no farm**

   Primeiro, verifique se todos os servidores de AD FS do Windows Server 2016 estão atualizados a partir das atualizações do Windows de junho de 2018 e se o AD FS 2016 farm está em execução no nível de comportamento do farm 2016.
1. **Verificar permissões**

   O bloqueio inteligente de extranet requer que o gerenciamento remoto do Windows esteja habilitado em cada servidor de AD FS.
3. **Atualizar permissões de banco de dados de artefato**

   O bloqueio inteligente de extranet exige que a conta de serviço AD FS tenha permissões para criar uma nova tabela no banco de dados de artefato AD FS. Faça logon em qualquer servidor de AD FS como um administrador de AD FS e conceda essa permissão executando os seguintes comandos em uma janela de prompt de comando do PowerShell:

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >O espaço reservado $cred é uma conta que tem permissões de administrador de AD FS. Isso deve fornecer as permissões de gravação para criar a tabela.

   Os comandos acima podem falhar devido à falta de permissões suficientes porque seu farm de AD FS está usando SQL Server, e a credencial fornecida acima não tem permissão de administrador no SQL Server. Nesse caso, você pode configurar as permissões de banco de dados manualmente no banco de dados SQL Server executando o comando a seguir quando estiver conectado ao banco de dados AdfsArtifactStore.
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

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Verifique se AD FS log de auditoria de segurança está habilitado
Esse recurso usa os logs de auditoria de segurança, portanto, a auditoria deve ser habilitada em AD FS, bem como a política local em todos os servidores de AD FS.

### <a name="configuration-instructions"></a>Instruções de configuração
O bloqueio inteligente de extranet usa a propriedade **ExtranetLockoutEnabled**do ADFS. Essa propriedade foi usada anteriormente para controlar "bloqueio de software de extranet" no servidor 2012R2. Se o bloqueio flexível de extranet tiver sido habilitado, para exibir a configuração da propriedade atual, execute ` Get-AdfsProperties` .

### <a name="configuration-recommendations"></a>Recomendações de configuração
Ao configurar o bloqueio inteligente da extranet, siga as práticas recomendadas para definir limites:  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: Half of AD Threshold Value`

Valor do AD: 20, ExtranetLockoutThreshold: 10

O bloqueio de Active Directory funciona independentemente do bloqueio inteligente de extranet. No entanto, se Active Directory bloqueio estiver habilitado, ExtranetLockoutThreshold em AD FS < limite de bloqueio de conta no AD

`ExtranetLockoutRequirePDC - $false`

Quando habilitado, o bloqueio de extranet requer um controlador de domínio primário (PDC). Quando desabilitado e configurado como falso, o bloqueio de extranet fará fallback para outro controlador de domínio, caso o PDC não esteja disponível.

Para definir essa propriedade, execute:

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>Habilitar o modo somente de log

No modo somente log, AD FS popula os usuários com informações de localização familiares e grava eventos de auditoria de segurança, mas não bloqueia nenhuma solicitação. Esse modo é usado para validar que o bloqueio inteligente está em execução e para permitir que AD FS "Aprenda" locais familiares para os usuários antes de habilitar o modo "impor". Como AD FS aprende, ele armazena a atividade de logon por usuário (seja no modo somente log ou no modo impor).
Defina o comportamento de bloqueio como somente log executando o cmdlet a seguir.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

O modo somente log destina-se a ser um estado temporário para que o sistema possa aprender o comportamento de logon antes de introduzir a imposição de bloqueio com o comportamento de bloqueio inteligente. A duração recomendada para o modo somente de log é de 3-7 dias. Se as contas estiverem ativamente sob ataque, o modo somente de log deverá ser executado por um mínimo de 24 horas.

No AD FS 2016, se o comportamento do 2012R2 ' extranet Soft Lock ' for habilitado antes de habilitar o bloqueio inteligente da extranet, o modo somente de log desabilitará o comportamento ' extranet Soft Lock '. AD FS bloqueio inteligente não bloqueará os usuários no modo somente de log. No entanto, o AD local pode bloquear o usuário com base na configuração do AD. Examine as políticas de bloqueio do AD para saber como o AD local pode bloquear os usuários.

No AD FS 2019, uma vantagem adicional é poder habilitar o modo somente de log para bloqueio inteligente e, ao mesmo tempo, continuar a impor o comportamento de bloqueio flexível anterior usando o PowerShell abaixo.

`Set-AdfsProperties -ExtranetLockoutMode 3`

Para que o novo modo tenha efeito, reinicie o serviço AD FS em todos os nós no farm

`Restart-service adfssrv`

Quando o modo estiver configurado, você poderá habilitar o bloqueio inteligente usando o parâmetro EnableExtranetLockout

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>Habilitar modo de imposição

Depois de se sentir confortável com o limite de bloqueio e a janela de observação, ESL pode ser movido para o modo "impor" usando o seguinte cmdlet PSH:

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

Para que o novo modo tenha efeito, reinicie o serviço AD FS em todos os nós no farm usando o comando a seguir.

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>Gerenciar atividade de conta de usuário
AD FS fornece três cmdlets para gerenciar dados de atividade da conta. Esses cmdlets se conectam automaticamente ao nó no farm que mantém a função mestre.
>[!NOTE]
>A JEA (administração suficiente) pode ser usada para delegar AD FS commandlets para redefinir os bloqueios de conta. Por exemplo, a equipe de suporte técnico pode ser delegada a permissões para usar ESL commandlets. Para obter informações sobre como delegar permissões para usar esses cmdlets, consulte [delegar AD FS acesso do commandlet do PowerShell a usuários não administradores](delegate-ad-fs-pshell-access.md)

Esse comportamento pode ser substituído passando o parâmetro-Server.

- Get-ADFSAccountActivity-UserPrincipalName

  Ler a atividade da conta atual para uma conta de usuário. O cmdlet sempre se conecta automaticamente ao mestre do farm usando o ponto de extremidade REST da atividade de conta. Portanto, todos os dados sempre devem ser consistentes.

`Get-ADFSAccountActivity user@contoso.com`

  Propriedades:
    - BadPwdCountFamiliar: incrementado quando uma autenticação é bem-sucedida em um local conhecido.
    - BadPwdCountUnknown: incrementado quando uma autenticação não é bem-sucedida em um local desconhecido
    - LastFailedAuthFamiliar: se a autenticação não foi bem-sucedida em um local familiar, o LastFailedAuthUnknown está definido para a hora da autenticação malsucedida
    - LastFailedAuthUnknown: se a autenticação não foi bem-sucedida em um local desconhecido, LastFailedAuthUnknown é definido para a hora da autenticação malsucedida
    - FamiliarLockout: valor booliano que será "true" se o "BadPwdCountFamiliar" > ExtranetLockoutThreshold
    - UnknownLockout: valor booliano que será "true" se o "BadPwdCountUnknown" > ExtranetLockoutThreshold  
    - FamiliarIPs: máximo de 20 IPs que são familiares para o usuário. Quando isso for excedido, o IP mais antigo na lista será removido.
-    Set-ADFSAccountActivity

     Adiciona novos locais familiares. A lista de IP familiar tem, no máximo, 20 entradas, se isso for excedido, o IP mais antigo na lista será removido.

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- Redefinir-ADFSAccountLockout

  Redefine o contador de bloqueios para uma conta de usuário para cada local familiar (badPwdCountFamiliar) ou contadores de local desconhecidos (badPwdCountUnfamiliar). Ao redefinir um contador, o valor "FamiliarLockout" ou "UnfamiliarLockout" será atualizado, pois o contador de redefinição será menor que o limite.  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>Log de eventos & informações de atividade do usuário para AD FS bloqueio de extranet

### <a name="connect-health"></a>Connect Health
A maneira recomendada para monitorar a atividade de conta de usuário é por meio do Connect Health. O Connect Health gera relatórios para download em IPs arriscados e tentativas de senha inadequadas. Cada item no Relatório de IP arriscado mostra informações agregadas sobre atividades de entrada do AD FS com falha que excedem o limite designado. As notificações por email podem ser definidas para alertar os administradores assim que isso ocorrer com as configurações de email personalizáveis. Para obter informações adicionais e instruções de instalação, visite a [documentação do Connect Health](/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS eventos de bloqueio inteligente da extranet.

>[!NOTE]
> Solucionar problemas de bloqueio inteligente de extranet com o [AD FS ajudar a proteger o guia de solução de problemas](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/a73d5843-9939-4c03-80a1-adcbbf3ccec8)

Para que os eventos de bloqueio inteligente da extranet sejam gravados, o ESL deve ser habilitado no modo ' somente log ' ou ' impor ' e a auditoria de segurança do ADFS está habilitada.
AD FS gravará eventos de bloqueio de extranet no log de auditoria de segurança:
- Quando um usuário é bloqueado (atinge o limite de bloqueio para tentativas de logon malsucedidas)
- Quando AD FS recebe uma tentativa de logon para um usuário que já está no estado de bloqueio

No modo somente log, você pode verificar o log de auditoria de segurança para eventos de bloqueio. Para todos os eventos encontrados, você pode verificar o estado do usuário usando o cmdlet Get-ADFSAccountActivity para determinar se o bloqueio ocorreu de endereços IP conhecidos ou desconhecidos e para verificar novamente a lista de endereços IP familiares para esse usuário.


|ID do evento|Descrição|
|-----|-----|
|1203|Esse evento é gravado para cada tentativa de senha inadequada. Assim que o badPwdCount atingir o valor especificado em ExtranetLockoutThreshold, a conta será bloqueada no ADFS durante a duração especificada em ExtranetObservationWindow.</br>ID da atividade: %1</br>XML: %2|
|1201|Esse evento é gravado cada vez que um usuário é bloqueado. </br>ID da atividade: %1</br>XML: %2|
|557 (ADFS 2019)| Ocorreu um erro ao tentar se comunicar com o serviço REST do repositório de contas no nó %1. Se esse for um farm WID, o nó primário poderá estar offline. Se este for um farm do SQL, o ADFS selecionará automaticamente um novo nó para hospedar a função mestre de armazenamento do usuário.|
|562 (ADFS 2019)|Ocorreu um erro ao communcating com o ponto de extremidade de repositório de conta no servidor %1.</br>Mensagem de exceção: %2|
|563 (ADFS 2019)|Ocorreu um erro ao calcular o status de bloqueio da extranet. Devido ao valor da configuração %1, a autenticação será permitida para esse usuário e a emissão do token continuará. Se esse for um farm WID, o nó primário poderá estar offline. Se este for um farm do SQL, o ADFS selecionará automaticamente um novo nó para hospedar a função mestre de armazenamento do usuário.</br>Nome do servidor de repositório de conta: %2</br>ID de usuário: %3</br>Mensagem de exceção: %4|
|512|A conta para o usuário a seguir está bloqueada. Uma tentativa de logon está sendo permitida devido à configuração do sistema.</br>ID da atividade: %1 </br>Usuário: %2 </br>IP do cliente: %3 </br>Contagem de senha inadequada: %4  </br>Última tentativa de senha inadequada: %5|
|515|A seguinte conta de usuário estava em um estado bloqueado e a senha correta acabou de ser fornecida. Essa conta pode estar comprometida.</br>Dados adicionais </br>ID da atividade: %1 </br>Usuário: %2 </br>IP do cliente: %3 |
|516|A seguinte conta de usuário foi bloqueada devido a muitas tentativas de senha inadequadas.</br>ID da atividade: %1  </br>Usuário: %2  </br>IP do cliente: %3  </br>Contagem de senha inadequada: %4  </br>Última tentativa de senha inadequada: %5|

## <a name="esl-frequently-asked-questions"></a>Perguntas frequentes sobre o ESL

**Um farm do ADFS usando o bloqueio inteligente de extranet no modo impor já vê bloqueios de usuários mal-intencionados?** 

R: se o bloqueio inteligente do ADFS estiver definido para o modo "impor", você nunca verá a conta do usuário legítima bloqueada por força bruta ou negação de serviço. A única maneira de um bloqueio de conta mal-intencionada pode impedir que um usuário entre é se o ator incorreto tiver a senha do usuário ou puder enviar solicitações de um endereço IP válido (conhecido) para esse usuário. 

**O que acontece ESL está habilitado e o ator inadequado tem uma senha de usuário?** 

R: o objetivo típico do cenário de ataque de força bruta é adivinhar uma senha e entrar com êxito.Se um usuário for Phish ou se uma senha for adivinhada, o recurso ESL não bloqueará o acesso, já que a entrada atenderá aos critérios "bem-sucedidos" de senha correta mais novo IP. O IP dos atores defeituosos seria exibido como "familiar".A melhor mitigação nesse cenário é limpar a atividade do usuário no ADFS e exigir a autenticação multifator para os usuários. É altamente recomendável instalar a proteção de senha do AAD, que garante que senhas adivinhes não cheguem ao sistema.

**Se meu usuário nunca tiver se conectado com êxito de um IP e tentar com uma senha incorreta algumas vezes, eles poderão fazer logon depois de digitarem a senha corretamente?** 

R: se um usuário enviar várias senhas incorretas (isto é, legitimamente digitar incorretamente) e, na seguinte tentativa, a senha for correta, o usuário terá a conexão imediatamente. Isso limpará a contagem de senha incorreto e adicionará esse IP à lista FamiliarIPs.No entanto, se eles ultrapassarem o limite de logons com falha do local desconhecido, eles entrarão no estado de bloqueio e precisarão aguardar a janela de observação e entrar com uma senha válida ou exigir a intervenção do administrador para redefinir sua conta.  
 
**O ESL também funciona na intranet?**

R: se os clientes se conectarem diretamente aos servidores ADFS e não por meio de servidores proxy de aplicativo Web, o comportamento de ESL não será aplicado.  

**Estou vendo os endereços IP da Microsoft no campo IP do cliente. O ESL bloqueia ataques de força bruta EXO com proxy?**  

R: o ESL funcionará bem para evitar cenários de ataque de força bruta do Exchange Online ou de outras autenticações herdadas. Uma autenticação herdada tem uma "ID da atividade" de 00000000-0000-0000-0000-000000000000.Nesses ataques, o ator inadequado é tirar proveito da autenticação básica do Exchange Online (também conhecida como autenticação herdada) para que o endereço IP do cliente seja exibido como um Microsoft One. Os servidores do Exchange Online no proxy de nuvem a verificação de autenticação em nome do cliente do Outlook. Nesses cenários, o endereço IP do emissor mal-intencionado estará no IP x-MS-encaminhar-Client-IP e o Microsoft Exchange Online Server estará no valor x-MS-Client-IP.
O bloqueio inteligente de extranet verifica IPs de rede, IPs encaminhados, o x-Forwarded-Client-IP e o valor x-MS-Client-IP. Se a solicitação for bem-sucedida, todos os IPs serão adicionados à lista conhecida. Se uma solicitação chegar e qualquer um dos IPs apresentados não estiver na lista conhecida, a solicitação será marcada como desconhecida. O usuário familiar poderá entrar com êxito enquanto as solicitações dos locais desconhecidos serão bloqueadas.  

**Posso estimar o tamanho do ADFSArtifactStore antes de habilitar o ESL?**

R: com o ESL habilitado, AD FS rastreia a atividade da conta e os locais conhecidos para os usuários no banco de dados ADFSArtifactStore. Esse banco de dados é dimensionado em tamanho em relação ao número de usuários e localizações conhecidas acompanhados. Ao planejar a habilitação do ESL, você pode estimar o aumento do tamanho do banco de dados ADFSArtifactStore a uma taxa de até 1 GB a cada 100.000 usuários. Se o farm de AD FS estiver usando o banco de dados interno do Windows (WID), o local padrão para os arquivos de banco de dados será C:\Windows\WID\Data\. Para evitar o preenchimento desta unidade, verifique se você tem um mínimo de 5 GB de armazenamento livre antes de habilitar o ESL. Além do armazenamento em disco, planeje um aumento da memória total do processo depois de habilitar o ESL em até um 1 GB de RAM adicional para populações de usuário de 500.000 ou menos.


## <a name="additional-references"></a>Referências adicionais  
[Práticas recomendadas para proteger Serviços de Federação do Active Directory (AD FS)](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-Adfsproperties](/powershell/module/adfs/set-adfsproperties?view=win10-ps)

[Operações do AD FS](../ad-fs-operations.md)
