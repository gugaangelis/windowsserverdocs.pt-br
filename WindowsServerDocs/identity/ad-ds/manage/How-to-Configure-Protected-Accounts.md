---
ms.assetid: 70c99703-ff0d-4278-9629-b8493b43c833
title: Como configurar contas protegidas
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4de7b1c9e3d12556f67c4515467bccd124e5e73e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-configure-protected-accounts"></a>Como configurar contas protegidas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Por meio de ataques Pass-the-hash (PtH), um invasor pode autenticar um servidor remoto ou serviço usando o hash NTLM subjacente de senha do usuário (ou outros elementos derivados de credenciais). A Microsoft tem anteriormente [publicado orientações](https://www.microsoft.com/download/details.aspx?id=36036) para reduzir ataques pass-the-hash.  Windows Server 2012 R2 inclui novos recursos para ajudar a mitigar tais ataques ainda mais. Para saber mais sobre outros recursos de segurança que ajudam a proteger contra roubo de credenciais, consulte [proteção de credenciais e gerenciamento](https://technet.microsoft.com/library/dn408190.aspx). Este tópico explica como configurar os seguintes novos recursos:

-   [Usuários protegidos](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_AddtoProtectedUsers)

-   [Políticas de autenticação](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicies)

-   [Silos de política de autenticação](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicySilos)

Há mitigações adicionais integradas ao Windows 8.1 e Windows Server 2012 R2 para ajudar a proteger contra o roubo de credenciais, que são abordados nos tópicos a seguir:

-   [Modo de administrador restrito para área de trabalho remota](http://blogs.technet.com/b/kfalde/archive/2013/08/14/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)

-   [Proteção da LSA](https://technet.microsoft.com/library/dn408187)

## <a name="BKMK_AddtoProtectedUsers"></a>Usuários protegidos
Os usuários protegidos é um novo grupo de segurança global para o qual você pode adicionar usuários novos ou existentes. Dispositivos do Windows 8.1 e Windows Server 2012 R2 hosts têm um comportamento especial com os membros deste grupo para fornecer melhor proteção contra roubo de credenciais. Um membro do grupo, um dispositivo Windows 8.1 ou um host do Windows Server 2012 R2 não armazena em cache as credenciais que não há suporte para os usuários protegido. Os membros deste grupo têm sem proteção adicional se estiverem conectados a um dispositivo que executa uma versão do Windows anteriores ao Windows 8.1.

Membros dos usuários protegido de grupo que são assinados em dispositivos Windows 8.1 e Windows Server 2012 R2 hosts podem *não é mais* usar:

-   Padrão de delegação de credenciais (CredSSP) - texto sem formatação não são armazenadas em cache as credenciais mesmo quando o **permitir delegação de credenciais padrão** política está habilitada

-   Resumo do Windows - texto sem formatação credenciais não são armazenadas em cache mesmo quando estiverem ativadas

-   Não é armazenado em cache NTLM - NTOWF

-   Chaves de longo prazo de Kerberos - Kerberos concessão de tíquete (TGT) é adquirido no logon e não pode ser novamente adquirido automaticamente

-   Logon offline - o verificador de logon em cache não é criado

Se o nível funcional do domínio for Windows Server 2012 R2, membros do grupo não poderão mais:

-   Autenticar usando autenticação NTLM

-   Usar pacotes de codificação Data Encryption Standard (DES) ou RC4 na pré-autenticação Kerberos

-   Ser recebido através da delegação restrita ou irrestrita

-   Renovar tíquetes de usuário (TGTs) além do tempo de vida de 4 horas inicial

Para adicionar usuários ao grupo, você pode usar [ferramentas de interface do usuário](https://technet.microsoft.com/library/cc753515.aspx) como Active Directory administrativas central (ADAC) ou Active Directory usuários e computadores ou uma ferramenta de linha de comando como [Dsmod grupo](https://technet.microsoft.com/library/cc732423.aspx), ou o Windows PowerShell[adicionar ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet. Contas para serviços e computadores *não devem* ser membros do grupo usuários protegido. A associação para essas contas fornece sem proteções locais porque a senha ou certificado sempre está disponível no host.

> [!WARNING]
> As restrições de autenticação não tem nenhuma solução alternativa, o que significa que os membros dos grupos altamente privilegiados como do grupo Administradores corporativos ou do grupo Admins. do domínio estão sujeitos às mesmas restrições que outros membros do grupo usuários protegido. Se todos os membros desses grupos são adicionados ao grupo usuários protegidos, é possível para todas essas contas seja bloqueada. Você nunca deve adicionar todas as contas altamente privilegiadas ao grupo usuários protegidos até que você testar exaustivamente ao impacto potencial.

Membros do grupo usuários protegido devem ser capazes de autenticar usando Kerberos com padrões AES (Advanced Encryption). Esse método requer AES chaves para a conta no Active Directory. O administrador interno não tem uma chave AES, a menos que a senha foi alterada em um controlador de domínio que executa o Windows Server 2008 ou posterior. Além disso, qualquer conta, que possui uma senha que foi alterada em um controlador de domínio que executa uma versão anterior do Windows Server, está bloqueada. Portanto, siga estas práticas recomendadas:

-   Não testar em domínios, a menos que **todos os controladores de domínio executam o Windows Server 2008 ou posterior**.

-   **Alterar senha** para todas as contas de domínio que foram criadas *antes de* o domínio foi criado. Caso contrário, essas contas não podem ser autenticadas.

-   **Alterar senha** para cada usuário antes de adicionar a conta aos usuários protegido de grupo ou certifique-se de que a senha foi alterado recentemente em um controlador de domínio que executa o Windows Server 2008 ou posterior.

### <a name="BKMK_Prereq"></a>Requisitos para usar contas protegidas
Contas protegidas tem os seguintes requisitos de implantação:

-   Para fornecer as restrições de cliente para os usuários protegido, hosts devem executar o Windows 8.1 ou Windows Server 2012 R2. Um usuário só precisa logon com uma conta que seja um membro de um grupo de usuários protegido. Nesse caso, o grupo usuários protegidos pode ser criado com [transferindo a função de emulador do domínio primário (PDC) do controlador](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) para um controlador de domínio que executa o Windows Server 2012 R2. Depois que esse objeto de grupo é replicado para outros controladores de domínio, a função de emulador do PDC pode ser hospedada em um controlador de domínio que executa uma versão anterior do Windows Server.

-   Para fornecer as restrições de lado de controlador de domínio para usuários protegido, o que é restringir o uso da autenticação NTLM, e outras restrições, o nível funcional do domínio deve ser Windows Server 2012 R2. Para obter mais informações sobre níveis funcionais, consulte [níveis funcionais de Noções básicas sobre serviços Active Directory domínio (AD DS)](../active-directory-functional-levels.md).

### <a name="BKMK_TrubleshootingEvents"></a>Solucionar problemas de eventos relacionados aos usuários protegido
Esta seção aborda novos logs para ajudar a solucionar problemas de eventos que estão relacionados aos usuários protegidos e como os usuários protegidos podem afetar alterações para solucionar qualquer um dos problemas de expiração ou delegação tíquetes (TGT) da concessão de tíquete.

#### <a name="new-logs-for-protected-users"></a>Logs de novo para os usuários protegido

Dois novos logs administrativos operacionais estão disponíveis para ajudar a solucionar problemas de eventos que estão relacionados aos usuários protegidos: usuário protegido - Log de cliente e protegido por falhas de usuário - Log de controlador de domínio. Esses novos logs estão localizados no Visualizador de eventos e são desabilitados por padrão. Para habilitar um log, clique em **Logs de aplicativos e serviços**, clique em **Microsoft**, clique em **Windows**, clique em **autenticação**e, em seguida, clique no nome do log e clique em **ação** (ou clique com botão direito no log) e clique em **Habilitar Log**.

Para obter mais informações sobre eventos em logs desses, consulte [políticas de autenticação e Silos de política de autenticação](https://technet.microsoft.com/library/dn486813.aspx).

#### <a name="troubleshoot-tgt-expiration"></a>Solucionar problemas de expiração TGT
Normalmente, o controlador de domínio define o tempo de vida do TGT e renovação de acordo com a política de domínio conforme mostrado na janela do Editor de gerenciamento de política de grupo seguinte.

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

Para **protegido por usuários**, as seguintes configurações são embutidas:

-   Tempo de vida máximo para o tíquete de usuário: 240 minutos

-   Vida útil máxima para renovação de tíquete de usuário: 240 minutos

#### <a name="troubleshoot-delegation-issues"></a>Solucionar problemas de delegação
Anteriormente, se uma tecnologia que usa a delegação Kerberos foi falhando, a conta do cliente foi verificada para ver se **conta é confidencial e não pode ser delegada** foi definido. No entanto, se a conta é um membro do **protegido por usuários**, ele talvez não tenha essa configuração definida no Active Directory administrativas central (ADAC). Como resultado, verifique a configuração e a associação ao grupo ao solucionar problemas de delegação.

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

### <a name="BKMK_AuditAuthNattempts"></a>Auditar tentativas de autenticação
Para auditar tentativas de autenticação explicitamente os membros do **protegido por usuários** grupo, você pode continuar a coletar os eventos de auditoria de log de segurança ou coletar os dados no novo logs operacionais administrativos. Para saber mais sobre esses eventos, consulte [políticas de autenticação e Silos de política de autenticação](https://technet.microsoft.com/library/dn486813.aspx)

### <a name="BKMK_ProvidePUdcProtections"></a>Fornecer proteções de DC para serviços e computadores
Contas para serviços e computadores não podem ser membros de **protegido por usuários**. Esta seção explica quais proteções de baseadas no controlador de domínio podem ser oferecidas para essas contas:

-   Rejeitar a autenticação NTLM: somente configurável por meio de [políticas de bloco NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

-   Rejeitar Data Encryption Standard (DES) na pré-autenticação Kerberos: controladores de domínio do Windows Server 2012 R2 não aceitam DES para contas de computador, a menos que estejam configurados para DES apenas porque todas as versões do Windows lançadas com Kerberos também dá suporte a RC4.

-   Rejeitar RC4 na pré-autenticação Kerberos: não é configurável.

    > [!NOTE]
    > Embora seja possível [alterar a configuração dos tipos de criptografia permitidos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx), não é recomendado para alterar essas configurações para criar contas de computador sem testes no ambiente de destino.

-   Restringir a permissão do usuário (TGTs) para um tempo de vida de 4 horas inicial: políticas de autenticação de uso.

-   Negar delegação com a delegação restrita ou irrestrita: para impedir que uma conta, abra o centro administrativo de diretório ativo (ADAC) e selecione o **conta é confidencial e não pode ser delegada** caixa de seleção.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

## <a name="BKMK_CreateAuthNPolicies"></a>Políticas de autenticação
Políticas de autenticação é um contêiner de novo no AD DS que contém objetos de política de autenticação. Políticas de autenticação podem especificar configurações que ajudam a reduzir a exposição de roubo de credenciais, como restringir o tempo de vida TGT para contas ou adicionando outras condições requerimentos judiciais ou Extrajudiciais relacionados.

No Windows Server 2012, o controle de acesso dinâmico introduziu uma classe de objeto de escopo da floresta do Active Directory chamada de política de acesso Central para fornecer uma maneira fácil de configurar servidores de arquivos em toda uma organização. No Windows Server 2012 R2, uma nova classe de objeto chamada política de autenticação (classe msDS-AuthNPolicies) pode ser usada para aplicar a configuração de autenticação para classes de conta em domínios do Windows Server 2012 R2. Classes de conta do Active Directory são:

-   Usuário

-   Computador

-   Conta de serviço gerenciado e grupo conta de serviço gerenciado (GMSA)

### <a name="quick-kerberos-refresher"></a>Rápido atualizador de Kerberos
O protocolo de autenticação Kerberos consiste em três tipos de bolsas de valores, também conhecidos como protocolos:

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbRefresher.gif)

-   O Exchange (AS) do serviço de autenticação (KRB_AS_ *)

-   A troca de serviço (TGS) concessão de tíquete (KRB_TGS_ *)

-   A troca de cliente/servidor (ponto de acesso) (KRB_AP_ *)

O exchange como é onde o cliente usa a senha da conta ou a chave particular para criar um Pre-autenticador para solicitar um tíquete de concessão de tíquete (TGT). Isso acontece ao logon do usuário ou na primeira vez em que um tíquete de serviço é necessária.

O exchange TGS é onde TGT da conta é usada para criar um autenticador para solicitar um tíquete de serviço. Isso ocorre quando uma conexão autenticado é necessária.

O exchange PA ocorre como normalmente como dados dentro do protocolo de aplicativo e não é afetado pelas políticas de autenticação.

Para obter mais informações, consulte [como Kerberos versão 5 autenticação protocolo funciona o](https://technet.microsoft.com/library/cc772815(v=WS.10).aspx).

### <a name="overview"></a>Visão geral
Políticas de autenticação complementam protegido por usuários, fornecendo uma maneira de aplicar as restrições configuráveis contas e fornecendo as restrições para contas para serviços e computadores. Políticas de autenticação são impostas durante a troca de AS ou o TGS exchange.

Você pode restringir a autenticação inicial ou o exchange como por meio da configuração:

-   Um tempo de vida do TGT

-   Condições de controle de acesso para restringir o logon do usuário, que deve ser atendido por dispositivos da qual o exchange como está chegando

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictAS.gif)

Você pode restringir as solicitações de tíquete de serviço por meio de uma troca de concessão de tíquete de serviço (TGS) por meio da configuração:

-   Condições de controle de acesso que devem ser atendidas pelo cliente (usuário, serviço, computador) ou o dispositivo do qual o exchange TGS está chegando

### <a name="BKMK_ReqForAuthnPolicies"></a>Requisitos para usar políticas de autenticação

|Política|Requisitos|
|----------|----------------|
|Fornecer tempos de vida TGT personalizados| Domínios de conta de nível funcional de domínio do Windows Server 2012 R2|
|Restringir o logon do usuário|-Domínios de conta de nível funcional de domínio Windows Server 2012 R2 com suporte de controle de acesso dinâmico<br />-Suportam a Windows 8, Windows 8.1, Windows Server 2012 ou Windows Server 2012 R2 dispositivos com controle de acesso dinâmico|
|Restringir emissão de tíquete de serviço com base em grupos de segurança e de conta de usuário| Domínios de recursos do nível funcional de domínio do Windows Server 2012 R2|
|Restringir emissão de tíquete de serviço com base em declarações do usuário ou conta de dispositivo, grupos de segurança ou requerimentos judiciais ou Extrajudiciais| Domínios de recursos do nível funcional de domínio do Windows Server 2012 R2 com suporte de controle de acesso dinâmico|

### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>Restringir uma conta de usuário para dispositivos específicos e hosts
Uma conta de alto valor com privilégios administrativos deve ser um membro do **protegido por usuários** grupo. Por padrão, nenhuma conta é membros do **protegido por usuários** grupo. Antes de adicionar contas ao grupo, configure o suporte ao controlador de domínio e criar uma política de auditoria para garantir que não há nenhum problema de bloqueio.

#### <a name="configure-domain-controller-support"></a>Configurar o suporte ao controlador de domínio

Domínio da conta do usuário deve ser no nível funcional do domínio do Windows Server 2012 R2 (DFL). Certifique-se de todos os controladores de domínio são Windows Server 2012 R2 e, em seguida, usam domínios do Active Directory e relações de confiança para [emitir o DFL](https://technet.microsoft.com/library/cc753104.aspx) ao Windows Server 2012 R2.

**Para configurar o suporte para controle de acesso dinâmico**

1.  Na política de controladores de domínio padrão, clique em **Enabled** para habilitar **suporte de cliente do Centro de distribuição de chaves (KDC) para declarações, autenticação composta e proteção Kerberos** em configuração do computador | Modelos administrativos | Sistema | KDC.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)

2.  Em **opções**, na caixa de listagem suspensa, selecione **sempre fornecer declarações**.

    > [!NOTE]
    > **Suporte** também podem ser configuradas, mas porque o domínio está no Windows Server 2012 R2 DFL, tendo as controladores de domínio sempre fornecer declarações permitirá que acesso baseada em declarações do usuário verifica para ocorrer quando estiver usando dispositivos de reconhecimento não requerimentos judiciais ou Extrajudiciais e hospeda para se conectar a serviços de reconhecimento de declarações.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)

    > [!WARNING]
    > Configurando **falhar solicitações de autenticação unarmored** irá resultar em falhas de autenticação de qualquer sistema operacional que não dá suporte a proteção Kerberos, como o Windows 7 e sistemas operacionais anteriores, ou sistemas operacionais a partir do Windows 8, que não foram configurados explicitamente para dar suporte a ele.

#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>Criar uma auditoria de conta de usuário para política de autenticação com ADAC

1.  Abra o Centro Administrativo do Active Directory (ADAC).

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_OpenADAC.gif)

    > [!NOTE]
    > Selecionadas **autenticação** nó está visível para domínios que estão no Windows Server 2012 R2 DFL. Se o nó não aparecer, tente novamente usando uma conta de administrador do domínio de um domínio que esteja no Windows Server 2012 R2 DFL.

2.  Clique em **políticas de autenticação**e clique em **nova** para criar uma nova política.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)

    Políticas de autenticações devem ter um nome de exibição e são impostas por padrão.

3.  Para criar uma política somente auditoria, clique em **restrições de política de auditoria somente**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)

    Políticas de autenticação são aplicadas com base no tipo de conta do Active Directory. Uma única política pode aplicar a todos os três tipos de conta definindo configurações para cada tipo. Tipos de conta são:

    -   Usuário

    -   Computador

    -   Conta de serviço gerenciado e grupo gerenciado conta de serviço

    Se você estendeu o esquema com novos objetos que podem ser usados pelo Centro de distribuição de chaves (KDC), o novo tipo de conta é classificado o mais próximo derivado do tipo de conta.

4.  Para configurar um tempo de vida do TGT para contas de usuário, selecione o **especificar uma vida útil de tíquete de concessão de contas de usuário** caixa de seleção e digite a hora em minutos.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTLifetime.gif)

    Por exemplo, se você quiser um tempo de vida TGT máximo 10 horas, insira **600** conforme mostrado. Se nenhum ciclo de vida do TGT estiver configurado, em seguida, se a conta é um membro do **protegido por usuários** de grupo, o tempo de vida do TGT e renovação é 4 horas. Caso contrário, a renovação e a vida útil do TGT são baseadas em política de domínio conforme visto na janela seguinte do Editor de gerenciamento de política de grupo para um domínio com as configurações padrão.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

5.  Para restringir a conta de usuário para selecionar dispositivos, clique em **editar** para definir as condições que são necessárias para o dispositivo.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)

6.  No **editar condições de controle de acesso** janela, clique em **adicionar uma condição**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCondition.png)

##### <a name="add-computer-account-or-group-conditions"></a>Adicione condições de grupo ou uma conta de computador

1.  Para configurar contas de computadores ou grupos, na lista suspensa, marque a caixa de lista suspensa **membro de cada** e altere para **membro de qualquer**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompMember.png)

    > [!NOTE]
    > Esse controle de acesso define as condições do dispositivo ou do host do qual o usuário entra. Na terminologia de controle de acesso, a conta de computador para o dispositivo ou host é o usuário, que é por isso que **usuário** é a única opção.

2.  Clique em **adicionar itens**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddItems.png)

3.  Para alterar os tipos de objeto, clique em **tipos de objeto**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjects.gif)

4.  Para selecionar objetos de computador no Active Directory, clique em **computadores**e clique em **Okey**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)

5.  Digite o nome dos computadores para impedir que o usuário e, em seguida, clique em **verificar nomes**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)

6.  Clique em Okey e crie quaisquer outras condições para a conta de computador.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddConditions.png)

7.  Quando terminar, clique em **Okey** e as condições definidas serão exibida para a conta do computador.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompDone.png)

##### <a name="add-computer-claim-conditions"></a>Adicione condições de declaração do computador

1.  Para configurar o computador requerimentos judiciais ou Extrajudiciais, lista suspensa grupo para selecionar a declaração.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaim.gif)

    Requerimentos judiciais ou Extrajudiciais estão disponíveis somente se eles já estão provisionados na floresta.

2.  Digite o nome de UO, a conta de usuário deve ser restrita ao logon.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimOUName.gif)

3.  Quando terminar, clique em Okey e a caixa mostrará as condições definidas.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimComplete.gif)

##### <a name="troubleshoot-missing-computer-claims"></a>Solucionar problemas de declarações ausentes do computador
Se a declaração foi provisionada, mas não está disponível, ele só pode ser configurado para **computador** classes.

Digamos que você quisesse restringir a autenticação com base na unidade organizacional (UO) do computador, que já foi configurado, mas apenas para **computador** classes.

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictComputers.gif)

Para a declaração de estar disponível para restringir o logon do usuário para o dispositivo, selecione o **usuário** caixa de seleção.

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)

#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>Provisione uma conta de usuário com uma política de autenticação com ADAC

1.  Do **usuário** conta, clique em **política**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicy.gif)

2.  Selecione o **atribuir uma política de autenticação para essa conta** caixa de seleção.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)

3.  Selecione a política de autenticação para aplicar ao usuário.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicySelect.png)

#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>Configurar o suporte de controle de acesso dinâmico em dispositivos e hosts
Você pode configurar os tempos de vida do TGT sem configurar o controle de acesso dinâmico (DAC). DAC só é necessário para a verificação de AllowedToAuthenticateFrom e AllowedToAuthenticateTo.

Usando política de grupo ou o Editor de política de Grupo Local, habilitar **suporte do cliente Kerberos declarações, autenticação composta e proteção Kerberos** em configuração do computador | Modelos administrativos | Sistema | Kerberos:

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)

### <a name="BKMK_TroubleshootAuthnPolicies"></a>Solucionar problemas de políticas de autenticação

#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>Determine as contas que são atribuídas diretamente uma política de autenticação
A seção contas na política de autenticação mostra as contas que aplicaram diretamente a política.

![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AccountsAssigned.gif)

#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>Use as falhas de política de autenticação - log administrativa do controlador de domínio
Um novo **falhas de política de autenticação - controlador de domínio** log administrativa em **Logs de aplicativos e serviços** > **Microsoft** > **Windows** > **autenticação** foi criado para facilitar ainda mais descobrir falhas devido a políticas de autenticação. O log é desabilitado por padrão. Para habilitá-lo, clique com botão direito o nome do log e clique em **Habilitar Log**. Os novos eventos são muito semelhantes em conteúdo para o tíquete de serviço eventos de auditoria e TGT Kerberos existente. Para saber mais sobre esses eventos, consulte [políticas de autenticação e Silos de política de autenticação](https://technet.microsoft.com/library/dn486813.aspx).

### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>Gerenciar políticas de autenticação usando o Windows PowerShell
Esse comando cria uma política de autenticação chamada **TestAuthenticationPolicy**. O **UserAllowedToAuthenticateFrom** parâmetro especifica os dispositivos da qual os usuários podem se autenticar por uma cadeia de caracteres SDDL no arquivo chamado txt.

```
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl
```

Esse comando obtém todas as políticas de autenticação que correspondam ao filtro que o **filtro** parâmetro especifica.

```
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com

```

Esse comando modifica a descrição e a **UserTGTLifetimeMins** propriedades da política de autenticação especificado.

```
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45
```

Esse comando remove a política de autenticação que o **identidade** parâmetro especifica.

```
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1
```

Esse comando usa o **Get-ADAuthenticationPolicy** cmdlet com o **filtro** parâmetro para obter todas as políticas de autenticação que não são impostas. O conjunto de resultados é enviada por pipe para o **ADAuthenticationPolicy remover** cmdlet.

```
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy
```

## <a name="BKMK_CreateAuthNPolicySilos"></a>Silos de política de autenticação
Silos de política de autenticação é um contêiner de novo (classe msDS-AuthNPolicySilos) no AD DS para o usuário, computador e contas de serviço. Eles ajudam a proteger as contas de alto valor. Embora todas as organizações precisam proteger membros dos grupos Administradores corporativos, Admins. do domínio e administradores de esquema porque essas contas podem ser usadas por um invasor para acessar qualquer recurso na floresta, outras contas também podem ser necessário proteção.

Algumas organizações isolam as cargas de trabalho, criando contas que são exclusivas-los e aplicando configurações de política de grupo para limitar o logon interativo local e remoto e privilégios administrativos. Silos de política de autenticação complementam esse trabalho através da criação de uma maneira de definir um relacionamento entre o usuário, computador e contas de serviço gerenciadas. Contas só podem pertencer a uma silo. Você pode configurar a política de autenticação para cada tipo de conta para controlar:

1.  Tempo de vida TGT não renovável

2.  Acessar condições de controle para retornar TGT (Observação: não é possível aplicar aos sistemas porque proteção Kerberos é necessária)

3.  Condições de controle de acesso para retornar o tíquete de serviço

Além disso, contas em um silo de política de autenticação tem uma reivindicação silo, que pode ser usada pelos recursos de reconhecimento de declarações como servidores de arquivos para controlar o acesso.

Um novo descritor de segurança pode ser configurado para controlar a emissão de tíquete de serviço com base em:

-   Usuário, grupos de segurança do usuário e/ou declarações do usuário

-   Declarações do dispositivo, dispositivo e/ou grupo de segurança do dispositivo

Obter essas informações para controladores de domínio do recurso requer o controle de acesso dinâmico:

-   Declarações do usuário:

    -   Windows 8 e posteriores clientes suporte ao controle de acesso dinâmico

    -   Controle de acesso dinâmico e requerimentos judiciais ou Extrajudiciais dá suporte a domínio da conta

-   Dispositivo e/ou grupo de segurança do dispositivo:

    -   Windows 8 e posteriores clientes suporte ao controle de acesso dinâmico

    -   Recurso configurado para a autenticação composta

-   Declarações de dispositivo:

    -   Windows 8 e posteriores clientes suporte ao controle de acesso dinâmico

    -   Domínio do dispositivo compatível com o controle de acesso dinâmico e requerimentos judiciais ou Extrajudiciais

    -   Recurso configurado para a autenticação composta

Políticas de autenticação podem ser aplicadas a todos os membros de um silo de política de autenticação, em vez de contas individuais ou autenticação separado políticas podem ser aplicadas a diferentes tipos de contas dentro de um silo. Por exemplo, uma política de autenticação pode ser aplicada a contas de usuário privilegiado e uma política de diferente pode ser aplicada a contas de serviços. Política de autenticação pelo menos um deve ser criada antes de um silo de política de autenticação pode ser criado.

> [!NOTE]
> Uma política de autenticação pode ser aplicada aos membros de um silo de política de autenticação, ou ele pode ser aplicado independentemente silos para restringir o escopo de conta específica. Por exemplo, para proteger uma única conta ou um pequeno conjunto de contas, uma política pode ser definida nessas contas sem adicionar as contas para um silo.

Você pode criar um silo de política de autenticação usando o Centro Administrativo do Active Directory ou o Windows PowerShell. Por padrão, um silo de política de autenticação somente auditorias silo políticas, que é equivalente ao especificar o **WhatIf** parâmetro em cmdlets do Windows PowerShell. Nesse caso, as restrições de silo de política não se aplicam, mas são geradas auditorias para indicar se ocorrem falhas se as restrições serão aplicadas.

#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>Para criar um silo de política de autenticação usando o Centro Administrativo do Active Directory

1.  Abrir **Centro Administrativo do Active Directory**, clique em **autenticação**, clique com botão direito **Silos de política de autenticação**, clique em **nova**e clique em **Silo de política de autenticação**.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)

2.  Em **nome de exibição**, digite um nome para silo. Em **permitido contas**, clique em **adicionar**, digite os nomes das contas e, em seguida, clique em **Okey**. Você pode especificar os usuários, computadores ou contas de serviço. Em seguida, especifica se deseja usar uma única política para todos os objetos ou uma diretiva separada para cada tipo de entidade de segurança e o nome da política ou políticas.

    ![contas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)

### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>Gerenciar silos de política de autenticação usando o Windows PowerShell
Esse comando cria um objeto de silo de política de autenticação e impõe-lo.

```
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce
```

Esse comando obtém todos os a autenticação silos de política que correspondam ao filtro especificado pelo **filtro** parâmetro. A saída é então passada para o **Format-Table** cmdlet para exibir o nome da política e o valor para **impor** em cada política.

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize

Name  Enforce
----  -------
silo     True
silos   False

```

Esse comando usa o **Get-ADAuthenticationPolicySilo** cmdlet com o **filtro** parâmetro para obter todos os silos de política de autenticação que não são impostos e pipe o resultado do filtro para o **ADAuthenticationPolicySilo remover** cmdlet.

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo
```

Esse comando concede acesso a autenticação política silo denominado *Silo* à conta de usuário chamado *User01*.

```
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01
```

Esse comando revoga o acesso às silo de política de autenticação chamado *Silo* da conta de usuário chamado *User01*. Porque o **confirmar** parâmetro é definido como **$False**, nenhuma mensagem de confirmação será exibida.

```
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False
```

Este exemplo usa primeiro o **Get-ADComputer** cmdlet para obter todas as contas de computador que correspondam ao filtro que o **filtro** parâmetro especifica. A saída desse comando é passada para **conjunto ADAccountAuthenticatinPolicySilo** atribuir o silo de política de autenticação chamado *Silo* e a política de autenticação chamado *AuthenticationPolicy02* -los.

```
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02
```



