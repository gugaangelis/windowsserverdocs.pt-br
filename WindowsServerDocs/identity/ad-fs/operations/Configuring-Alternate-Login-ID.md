---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configurar a ID de logon alternativa
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5bc43717f37fb3b14ac7f384a061ee64c734222d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189662"
---
# <a name="configuring-alternate-login-id"></a>Configurar a ID de logon alternativa


## <a name="what-is-alternate-login-id"></a>O que é a ID de logon alternativo?
Na maioria dos cenários, os usuários usar seus UPNS (nomes da entidade de usuário) para fazer logon em suas contas. No entanto, em alguns ambientes devido a políticas corporativas ou de dependências do aplicativo de linha de negócios no local, os usuários podem estar usando alguma outra forma de entrar. 

>[!NOTE]
>Recomendada pela Microsoft são de práticas recomendadas corresponder ao UPN para o endereço SMTP primário. Este artigo aborda o percentual pequeno de clientes que não é possível corrigir o UPN do corresponder.

Por exemplo, eles podem estar usando sua id de email para entrar e que pode ser diferente do seu UPN. Isso é particularmente uma ocorrência comum em cenários em que seu UPN não for roteável. Considere um usuário Jane Doe com o UPN jdoe@contoso.local e o endereço de email jdoe@contoso.com. Jane pode não estar até mesmo ciente do UPN conforme ela sempre tenha usado sua id de email para entrar. Uso de qualquer outro método de entrada, em vez de UPN representa a ID alternativa. Para obter mais informações sobre como o UPN é cria, consulte [população UserPrincipalName do Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

ID do Active Directory Federation Services (AD FS) permite que aplicativos federados usando o AD FS para entrar usando alternativo. Isso permite que os administradores especifiquem uma alternativa para o padrão UPN a ser usado para entrar. O AD FS já oferece suporte ao uso de qualquer forma de identificador de usuário que é aceito pelos serviços de domínio Active Directory (AD DS). Quando configurado para a ID alternativa, o AD FS permite aos usuários entrar usando o valor de ID alternativo configurado, digamos, id de email. Usando a ID alternativa permite que você adote os provedores de SaaS, como o Office 365 sem modificar seu UPNs locais. Ele também permite que você oferecer suporte a aplicativos de serviço de linha de negócios com identidades de consumidor provisionado.

## <a name="alternate-id-in-azure-ad"></a>Id alternativa no Azure AD
Uma organização pode ter que usar a ID alternativa nos seguintes cenários:
1.  O nome de domínio local não for roteável, ex. Contoso. local e, consequentemente o nome principal de usuário padrão não for roteável (jdoe@contoso.local). UPN existente não pode ser alterado devido a dependências do aplicativo local ou as políticas da empresa. Azure AD e Office 365 exige todos os sufixos de domínio associados com o diretório do AD do Azure para ser totalmente roteável da internet. 
2.  O UPN local não é o mesmo que o endereço de email do usuário e usam o endereço de email para entrar no Office 365, os usuários e UPN não pode ser usado devido a restrições de organizacionais.
Nos cenários mencionados acima, a ID alternativa com AD FS permite aos usuários entrar no Azure AD sem modificar seu UPNs locais. 

## <a name="end-user-experience-with-alternate-login-id"></a>Experiência do usuário final com a ID de logon alternativo
A experiência do usuário final varia dependendo do método de autenticação usado com a id de logon alternativo.  Atualmente, há três maneiras diferentes nos quais usando a id de logon alternativo pode ser obtido.  São eles:

- **(Herdado) de autenticação regular**-usa o protocolo de autenticação básica.
- **Autenticação moderna** -traz baseada no Active Directory Authentication Library ADAL entrar aos aplicativos. Isso permite que os recursos de entrada, como a MFA (autenticação multifator), provedores de identidade de terceiros com base em SAML com aplicativos cliente do Office, cartão inteligente e autenticação baseada em certificado.
- **Autenticação moderna híbrida** - fornece todos os benefícios da autenticação moderna e fornece aos usuários a capacidade de acessar a aplicativos locais usando os tokens de autorização obtidos da nuvem.

>[!NOTE]
> Para a melhor experiência possível, a Microsoft recomenda autenticação moderna híbrida.



## <a name="configure-alternate-logon-id"></a>Defina a ID de logon alternativo
Usando o Azure AD Connect, é recomendável usar o Azure AD conectar-se para configurar a ID de logon alternativo para o seu ambiente.

- Para a nova configuração do Azure AD Connect, consulte conectar-se ao AD do Azure para obter instruções detalhadas sobre como configurar o farm alternativo do ID e o AD FS.
- Para as instalações existentes do Azure AD Connect, consulte Alterando o método de entrada do usuário para obter instruções sobre como alterar o método de entrada para o AD FS

Quando o Azure AD Connect é fornecido detalhes sobre o ambiente do AD FS, ele automaticamente verifica a presença do KB direito no seu AD FS e configura o AD FS para a ID alternativa, incluindo todas as regras de declaração de direito necessário para confiança de Federação do AD do Azure. Não há nenhuma etapa adicional necessária externa assistente configure ID alternativa.

>[!NOTE]
> A Microsoft recomenda usar o Azure AD Connect para configurar ID de logon alternativo.

### <a name="manually-configure-alternate-id"></a>Configurar manualmente a ID alternativa
Para configurar a ID de logon alternativo, você deve executar as seguintes tarefas: Configurar seu objetos de confiança do provedor de declarações do AD FS para habilitar a ID de logon alternativo

1.  Se você tiver o Server 2012 R2, verifique se que você tiver o KB2919355 instalado em todos os servidores do AD FS. Você pode obtê-lo por meio do Windows Update Services ou baixá-lo diretamente. 

2.  Atualizar a configuração do AD FS, executando o seguinte cmdlet do PowerShell em qualquer um dos servidores de federação no seu farm (se você tiver um farm de WID, você deve executar esse comando no servidor primário do AD FS no farm):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** é o nome LDAP do atributo que você deseja usar para o logon.

**LookupForests** é a lista de floresta DNS que seus usuários pertencerem a.

Para habilitar o recurso de ID de logon alternativo, você deve configurar os parâmetros de AlternateLoginID - e - LookupForests com um valor não nulo, válido.

No exemplo a seguir, você estiver habilitando a funcionalidade de ID de logon alternativo, de modo que os usuários com contas em florestas contoso.com e fabrikam.com podem fazer logon em aplicativos do AD FS habilitados com o atributo "email".

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3.  Para desabilitar esse recurso, defina o valor para ambos os parâmetros ser nulo.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Autenticação moderna híbrida com a ID alternativa

>[!IMPORTANT]
>A seguir só foi testado com o AD FS e não 3ª provedores de identidade de terceiros.

### <a name="exchange-and-skype-for-business"></a>Exchange e Skype for Business
Se você estiver usando a id de logon alternativo com o Exchange e Skype for Business, a experiência do usuário varia dependendo se você estiver usando HMA.

>[!NOTE]
>Para uma melhor experiência do usuário final, a Microsoft recomenda o uso de autenticação moderna híbrida.

ou obter mais informações, consulte [visão geral de autenticação moderna híbrida](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Pré-requisitos para o Exchange e Skype for Business
A seguir estão os pré-requisitos para a obtenção de SSO com a ID alternativa.

- O Exchange Online devem ter a autenticação moderna ativadas.
- Skype para Online de negócios (SFB) deve ter a autenticação moderna ativadas.
- Exchange local deve ter autenticação moderna ativadas.  Exchange 2013 CU19 ou Exchange 2016 CU18 e backup é necessário em todos os servidores do Exchange. Nenhum Exchange 2010 no ambiente.
- Skype for Business local deve ter autenticação moderna ativadas.
- Você deve usar os clientes Exchange e Skype que têm autenticação moderna habilitada. Todos os servidores devem estar executando SFB Server 2015 CU5.
- Skype para clientes de negócios que são compatíveis com a autenticação moderna
   - iOS, Android, Windows Phone
   - SFB 2016 (MA está ativado por padrão, mas verifique se ele não tiver sido desabilitado).
   - SFB 2013 (MA está desativado por padrão, portanto, certifique-se MA foi ativada.)
   - Área de trabalho Mac SFB
- Clientes do Exchange que são compatíveis com a autenticação moderna e dar suporte a AltID regkeys
    - Office Pro Plus 2016 apenas





#### <a name="supported-office-version"></a>Versão com suporte do Office

Configurar seu diretório para que o SSO com id alternativa usando id alternativa pode causar prompts extras para autenticação, se essas configurações adicionais não forem concluídas. Consulte o artigo para o possível impacto na experiência do usuário com a id alternativa.

Com a seguinte configuração adicional, a experiência do usuário foi aprimorada significativamente e você pode conseguir quase zero prompts de autenticação para usuários de id alternativa em sua organização.

##### <a name="step-1-update-to-required-office-version"></a>Etapa 1. Atualização necessária a versão do Office
Versão 1712 do Office (compilar sem 8827.2148) e acima atualizou a lógica de autenticação para lidar com o cenário de id alternativa. Para aproveitar a nova lógica, os computadores cliente precisam ser atualizados para a versão 1712 do Office (compilar sem 8827.2148) e superior.

##### <a name="step-2-update-to-required-windows-version"></a>Etapa 2. Atualização necessária a versão do Windows
Windows versão 1709 e posterior atualizou a lógica de autenticação para lidar com o cenário de id alternativa. Para aproveitar a nova lógica, os computadores cliente precisam ser atualizados para Windows versão 1709 e superior.

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>Etapa 3. Configurar o registro para os usuários afetados usando a diretiva de grupo
Os aplicativos do office se baseiam nas informações enviadas por push pelo administrador do diretório para identificar o ambiente de id alternativa. As seguintes chaves do registro precisam ser configurados para ajudar a autenticar o usuário com a id alternativa sem mostrar avisos adicionais de aplicativos do office

|RegKey para adicionar|Nome de dados de Regkey, tipo e valor|Windows 7/8|Windows 10|Descrição|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Obrigatório|Obrigatório|O valor desse regkey é um nome de domínio personalizado verificado no locatário da organização. Por exemplo, a Contoso corp pode fornecer um valor de Contoso.com nesse regkey se Contoso.com é um dos nomes de domínio personalizado verificado no locatário Contoso.onmicrosoft.com.|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Necessário para o Outlook 2016 ProPlus|Necessário para o Outlook 2016 ProPlus|O valor desse regkey pode ser de 1 / 0 para indicar ao aplicativo Outlook se deverá contar com a lógica de autenticação de id alternativa aprimorada.|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obrigatório|Obrigatório|Este regkey pode ser usado para definir o STS como uma zona confiável nas configurações de internet. Implantação do AD FS padrão recomenda adicionar o namespace do ADFS a zona da Intranet Local para o Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Novo fluxo de autenticação após a configuração adicional

![Fluxo de autenticação](media/Configure-Alternate-Login-ID/alt1a.png)

1. a: Usuário está provisionado no AD do Azure usando a id alternativa
   </br>b: Administrador do diretório envia por push configurações regkey necessária para computadores cliente afetados
2. Usuário é autenticado no computador local e abre um aplicativo do office
3. Aplicativo do Office utilizará as credenciais de sessão local
4. Aplicativo do Office se autentica no Azure AD usando a dica de domínio enviada pelo administrador e as credenciais locais
5. Com êxito o Azure AD autentica o usuário por meio do direcionamento para corrigir o realm de Federação e emitir um token

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Aplicativos e experiência do usuário após a configuração adicional

### <a name="non-exchange-and-skype-for-business-clients"></a>Não sejam do Exchange e Skype para clientes de negócios
|Remota|Instrução de suporte|Comentários|
| ----- | -----|-----|
|Microsoft Teams|Com suporte|<li>Microsoft Teams dá suporte ao AD FS (SAML-P, WS-Fed, WS-Trust e OAuth) e autenticação moderna.</li><li> Core Microsoft Teams, como as funcionalidades de canais, bate-papos e arquivos de trabalhar com a ID de logon alternativo.</li><li>aplicativos de terceiros 1º e o 3º devem ser investigados separadamente pelo cliente. Isso ocorre porque cada aplicativo tem seus próprios protocolos de autenticação de capacidade de suporte.</li>|     
|OneDrive for Business|Suporte - chave do registro do lado do cliente recomendado |Com a ID alternativa configurada, você vê que o UPN local é previamente preenchido no campo de verificação. Isso precisa ser alterado para a identidade alternativa que está sendo usada. Recomendamos usar a chave do registro do lado cliente observada neste artigo: Office 2013 e Lync 2013 periodicamente solicitam credenciais para o SharePoint Online, OneDrive e o Lync Online.|
|OneDrive for Business de cliente móvel|Com suporte|| 
|Página de ativação Pro Plus do Office 365|Suporte - chave do registro do lado do cliente recomendado|Com a ID alternativa configurada, você vê que o UPN local é previamente preenchido no campo de verificação. Isso precisa ser alterado para a identidade alternativa que está sendo usada. Recomendamos usar a chave do registro de cliente observada neste artigo: Office 2013 e Lync 2013 periodicamente solicitam credenciais para o SharePoint Online, OneDrive e o Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange e Skype para clientes de negócios

|Remota|Instrução de suporte - com HMA|Instrução de suporte - sem HMA|
| ----- |----- | ----- |
|Outlook|Prompts não extras, com suporte|Com suporte</br></br>Com o **autenticação moderna** para o Exchange Online: Com suporte</br></br>Com o **autenticação regular** para o Exchange Online: Suporte com as seguintes condições:</br><li>Você deve estar em um computador de ingressado no domínio e conectados à rede corporativa </li><li>Você só pode usar a ID alternativa em ambientes que não permitem acesso externo para usuários de caixa de correio. Isso significa que os usuários podem apenas autenticar sua caixa de correio de uma forma com suporte quando eles estão conectados e associados à rede corporativa, em uma VPN ou conectados por meio de máquinas de acesso direto, mas você obtém alguns prompts extras ao configurar seu perfil do Outlook.| 
|Pastas públicas do híbrido|Prompts com suporte, não extras.|Com o **autenticação moderna** para o Exchange Online: Com suporte</br></br>Com o **autenticação regular** para o Exchange Online: Sem suporte</br></br><li>Híbrido de pastas públicas não são capazes de expandir se IDs alternativos são usados e, portanto, não deve ser usado hoje em dia com métodos de autenticação regular.|
|Entre instalações a delegação|Consulte [configurar o Exchange para dar suporte a permissões delegadas de caixa de correio em uma implantação híbrida](https://technet.microsoft.com/library/mt784505.aspx)|Consulte [configurar o Exchange para dar suporte a permissões delegadas de caixa de correio em uma implantação híbrida](https://technet.microsoft.com/library/mt784505.aspx)|
|Arquivar o acesso de caixa de correio (caixa de correio local - arquivo-morto na nuvem)|Prompts não extras, com suporte|Com suporte, os usuários obtêm um extra pedir credenciais ao acessar o arquivo morto, eles precisarão fornecer sua ID alternativa quando solicitado.| 
|Outlook Web Access|Com suporte|Com suporte|
|Aplicativos móveis de Outlook para IOS, Android e Windows Phone|Com suporte|Com suporte|
|Skype for Business / Lync|Com suporte, sem prompts extras|Com suporte (exceto como observado), mas há um potencial de confusão do usuário.</br></br>Em clientes móveis, Id alternativa é suportada somente se o endereço SIP = endereço de email = ID alternativa.</br></br> Os usuários podem precisar entrar duas vezes para o Skype para cliente da área de trabalho de negócios, pela primeira vez usando o UPN local e, em seguida, usando a ID alternativa. (Observe que o "endereço de entrada" é, na verdade, o que pode não ser o mesmo que o "nome de usuário", porém muitas vezes é endereço SIP). Quando solicitado pela primeira vez para um nome de usuário, o usuário deve inserir o UPN, mesmo se ele incorretamente é previamente preenchido com o endereço SIP ou de ID alternativa. Depois que o usuário clica em entrar com o UPN, o usuário nome prompt será exibido novamente, desta vez preenchida previamente com o UPN. Neste momento, o usuário deve substituir isso com a ID alternativa e clique em entrar para concluir o processo de entrada. Em clientes móveis, os usuários devem inserir a ID de usuário local na página Avançado, usando o formato de estilo do SAM (domínio \ nomedeusuário), não o formato UPN.</br></br>Depois de entrar com êxito, se o Skype for Business ou Lync diz "Exchange precisa das suas credenciais", você precisa fornecer as credenciais que são válidas para onde a caixa de correio está localizada. Se a caixa de correio está na nuvem que você precisa fornecer a ID alternativa. Se a caixa de correio for local, você precisará fornecer o UPN local.| 
 
## <a name="additional-details--considerations"></a>Detalhes adicionais e considerações sobre

-   O recurso de ID de logon alternativa está disponível para ambientes agrupados com o AD FS implantado.  Não há suporte nos seguintes cenários:
    -   Domínios não roteáveis (por exemplo, contoso. local) que não podem ser verificados pelo Azure AD.
    -   Ambientes que não têm o AD FS implantado gerenciados.


-   Quando habilitado, o recurso de ID de logon alternativo só está disponível para autenticação de nome de usuário e senha em todos os usuário nome/senha protocolos de autenticação suportados pelo AD FS (SAML-P, WS-Fed, WS-Trust e OAuth).


-   Quando a autenticação integrada do Windows (WIA) é executada (por exemplo, quando os usuários tentam acessar um aplicativo corporativo em um computador ingressado no domínio da intranet e o administrador do AD FS tiver configurado a política de autenticação para usar WIA para intranet), usado UPN para autenticação. Se você tiver configurado todas as regras de declaração para as partes de terceira parte confiável para o recurso de ID de logon alternativo, certifique-se de que essas regras ainda são válidas no caso de WIA.

-   Quando habilitada, o recurso de ID de logon alternativo requer pelo menos um servidor de catálogo global para ser acessível a partir do servidor do AD FS para cada floresta de conta de usuário que dá suporte ao AD FS. Falha ao acessar um servidor de catálogo global na floresta de conta de usuário resulta em fazer fallback para usar o UPN do AD FS. Por padrão, todos os controladores de domínio são servidores de catálogo global.

-   Quando habilitada, se o servidor do AD FS localizar mais de um objeto de usuário com o mesmo valor de ID de logon alternativo especificado em todas as florestas de conta de usuário configurado, ele falha de logon.

-   Quando o recurso de ID de logon alternativo é habilitado, o AD FS tenta autenticar o usuário final com a ID de logon alternativo primeiro e, em seguida, fazer fallback para usar o UPN se ele não é possível localizar uma conta que pode ser identificada pela ID de logon alternativo. Você deve verificar se que há não há conflitos entre a ID de logon alternativo e o UPN, se você quiser ainda oferece suporte o logon UPN. Por exemplo, configuração de um atributo de email com o outro UPN bloqueia o outro usuário de entrar com seu UPN.

-   Se uma das florestas que é configurada pelo administrador estiver inativo, o AD FS continua a pesquisar a conta de usuário com ID de logon alternativo em outras florestas configuradas. Se o servidor do AD FS localiza um único usuário objetos entre as florestas que ele pesquisou, um usuário fizer logon com êxito.

-   Além disso você queira personalizar a página de entrada do AD FS para dar aos usuários finais alguma dica sobre a ID de logon alternativo. Você pode fazer isso adicionando a descrição de página de entrada personalizado (para obter mais informações, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) ou personalizar a cadeia de caracteres "Entrar com conta organizacional" acima do campo de nome de usuário (para obter mais informações obter informações, consulte [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).

-   O novo tipo de declaração que contém o valor de ID de logon alternativo é **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Eventos e contadores de desempenho
Os contadores de desempenho a seguir foram adicionados para medir o desempenho dos servidores do AD FS quando o ID de logon alternativo está habilitado:

-   Autenticações de Id de logon alternativos: número de autenticações executadas usando a ID de logon alternativo

-   Alternativo autenticações de Id de logon/s: o número de autenticações executadas usando a ID de logon alternativo por segundo

-   Média de latência de pesquisa para a ID de logon alternativo: média de latência de pesquisa nas florestas que um administrador tiver configurado para a ID de logon alternativo

Seguem diversos casos de erro e o impacto correspondente no experiência de entrada do usuário uma com eventos registrados em log pelo AD FS:



**Casos de erro**|**Impacto na experiência de entrada**|**Event**|
---------|---------|---------
Não é possível obter um valor para o SAMAccountName para o objeto de usuário|Falha no logon|ID do evento 364 com a mensagem de exceção MSIS8012: Não é possível localizar o samAccountName do usuário: '{0}'.|
O atributo CanonicalName não está acessível|Falha no logon|ID do evento 364 com a mensagem de exceção MSIS8013: CanonicalName: '{0}' do usuário:'{1}' está no formato incorreto.|
Vários objetos de usuário são encontrados em uma floresta|Falha no logon|ID do evento 364 com a mensagem de exceção MSIS8015: Encontradas várias contas de usuário com identidade '{0}'na floresta'{1}' com identidades: {2}|
Vários objetos de usuário são encontrados em várias florestas|Falha no logon|ID do evento 364 com a mensagem de exceção MSIS8014: Encontradas várias contas de usuário com identidade '{0}' em florestas: {1}|

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)


