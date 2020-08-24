---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configurar a ID de logon alternativa
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.openlocfilehash: 549ba062a30ce3b2d1a9f06d60357c0199766d84
ms.sourcegitcommit: c6e2e545100bbbc4864088fd0d103bafc147fcbb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88785058"
---
# <a name="configuring-alternate-login-id"></a>Configurar a ID de logon alternativa


## <a name="what-is-alternate-login-id"></a>O que é a ID de logon alternativa?
Na maioria dos cenários, os usuários usam seus UPN (nomes de entidade de usuário) para fazer logon em suas contas. No entanto, em alguns ambientes devido a políticas corporativas ou dependências locais de aplicativos de linha de negócios, os usuários podem estar usando alguma outra forma de entrada.

> [!NOTE]
> As práticas recomendadas da Microsoft são fazer a correspondência entre o UPN e o endereço SMTP primário. Este artigo aborda a pequena porcentagem de clientes que não podem corrigir a correspondência de UPN.

Por exemplo, eles podem usar sua ID de email para entrar e que podem ser diferentes de seu UPN. Isso é particularmente uma ocorrência comum em cenários em que seu UPN não é roteável. Considere um usuário Janete Doe com UPN jdoe@contoso.local e endereço de email jdoe@contoso.com . Jane pode não estar até mesmo ciente do UPN, pois ela sempre usou sua ID de email para entrar. O uso de qualquer outro método de entrada em vez de UPN constitui a ID alternativa. Para obter mais informações sobre como o UPN é criado, consulte [população de userPrincipalName do Azure ad](/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

Serviços de Federação do Active Directory (AD FS) (AD FS) permite que aplicativos federados usando AD FS entrem usando uma ID alternativa. Isso permite que os administradores especifiquem uma alternativa ao UPN padrão a ser usado para entrar. O AD FS já oferece suporte ao uso de qualquer forma de identificador de usuário aceita pelo Active Directory Domain Services (AD DS). Quando configurado para a ID alternativa, AD FS permite que os usuários entrem usando o valor de ID alternativo configurado, digamos email-ID. O uso da ID alternativa permite que você adote provedores de SaaS, como o Office 365, sem modificar seus UPNs locais. Ele também permite que você ofereça suporte a aplicativos de serviço de linha de negócios com identidades provisionadas pelo consumidor.

## <a name="alternate-id-in-azure-ad"></a>ID alternativa no Azure AD

Uma organização pode ter que usar a ID alternativa nos seguintes cenários:
1. O nome de domínio local não é roteável, por exemplo, Contoso. local e, como resultado, o nome principal de usuário padrão é não roteável ( jdoe@contoso.local ). O UPN existente não pode ser alterado devido a dependências do aplicativo local ou políticas da empresa. O Azure AD e o Office 365 exigem que todos os sufixos de domínio associados ao diretório do AD do Azure sejam totalmente roteáveis pela Internet.
2. O UPN local não é o mesmo que o endereço de email do usuário e para entrar no Office 365, os usuários usam o endereço de email e o UPN não podem ser usados devido a restrições organizacionais.
   Nos cenários mencionados acima, a ID alternativa com AD FS permite que os usuários entrem no Azure AD sem modificar seus UPNs locais.

## <a name="configure-alternate-logon-id"></a>Configurar a ID de logon alternativa
Usando Azure AD Connect é recomendável usar o Azure AD Connect para configurar a ID de logon alternativa para seu ambiente.

- Para obter uma nova configuração de Azure AD Connect, consulte conectar-se ao Azure AD para obter instruções detalhadas sobre como configurar a ID alternativa e o farm de AD FS.
- Para instalações existentes do Azure AD Connect, consulte alterando o método de entrada do usuário para obter instruções sobre como alterar o método de entrada para AD FS

Quando Azure AD Connect é fornecido detalhes sobre o ambiente de AD FS, ele verifica automaticamente a presença dos KB corretos no seu AD FS e configura AD FS para ID alternativa, incluindo todas as regras de declaração corretas necessárias para a relação de confiança de Federação do Azure AD. Não há nenhuma etapa adicional necessária fora do assistente para configurar a ID alternativa.

> [!NOTE]
> A Microsoft recomenda o uso de Azure AD Connect para configurar a ID de logon alternativa.

### <a name="manually-configure-alternate-id"></a>Configurar manualmente a ID alternativa
Para configurar a ID de logon alternativa, você deve executar as seguintes tarefas: configurar seu AD FS relações de confiança do provedor de declarações para habilitar a ID de logon alternativa

1.  Se você tiver o servidor 2012R2, verifique se você tem o KB2919355 instalado em todos os servidores de AD FS. Você pode obtê-lo por meio de serviços Windows Update ou baixá-lo diretamente.

2.  Atualize a configuração de AD FS executando o seguinte cmdlet do PowerShell em qualquer um dos servidores de Federação em seu farm (se você tiver um farm de WID, deverá executar esse comando no servidor de AD FS primário em seu farm):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** é o nome LDAP do atributo que você deseja usar para o logon.

**LookupForests** é a lista de DNS de floresta à qual os usuários pertencem.

Para habilitar o recurso de ID de logon alternativo, você deve configurar os parâmetros-AlternateLoginID e-LookupForests com um valor não nulo e válido.

No exemplo a seguir, você está habilitando a funcionalidade de ID de logon alternativa, de modo que os usuários com contas em florestas contoso.com e fabrikam.com possam fazer logon em aplicativos habilitados para AD FS com o atributo "mail".

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. Para desabilitar esse recurso, defina o valor para ambos os parâmetros como NULL.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Autenticação moderna híbrida com ID alternativo

> [!IMPORTANT]
> O seguinte só foi testado em relação a AD FS e não a provedores de identidade de terceiros.

### <a name="exchange-and-skype-for-business"></a>Exchange e Skype for Business
Se você estiver usando uma ID de logon alternativo com o Exchange e o Skype for Business, a experiência do usuário varia dependendo se você está usando a HMA ou não.

> [!NOTE]
> Para a melhor experiência do usuário final, a Microsoft recomenda o uso da autenticação moderna híbrida.

ou mais informações, consulte [visão geral da autenticação moderna híbrida](https://support.office.com/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Pré-requisitos para o Exchange e Skype for Business
Veja a seguir os pré-requisitos para obter o SSO com a ID alternativa.

- O Exchange Online deve ter a autenticação moderna ativada.
- O Skype for Business (SFB) online deve ter a autenticação moderna ativada.
- O Exchange local deve ter a autenticação moderna ativada.  O Exchange 2013 CU19 ou o Exchange 2016 CU18 e o up são necessários em todos os servidores Exchange. Não há Exchange 2010 no ambiente.
- O Skype for Business local deve ter a autenticação moderna ativada.
- Você deve usar clientes do Exchange e Skype que tenham a autenticação moderna habilitada. Todos os servidores devem estar executando o SFB Server 2015 CU5.
- Clientes do Skype for Business que são capazes de autenticação moderna
   - iOS, Android Windows Phone
   - O SFB 2016 (MA está ativado por padrão, mas certifique-se de que ele não foi desabilitado.)
   - SFB 2013 (MA está desativado por padrão, portanto, certifique-se de que MA tenha sido ativado).
   - SFB Mac desktop
- Clientes do Exchange que são capazes de autenticação moderna e dão suporte a AltID regkeys
    - Office Pro Plus somente 2016

#### <a name="supported-office-version"></a>Versão do Office com suporte

Configurar seu diretório para SSO com a ID alternativa usando a ID alternativa pode causar prompts adicionais para autenticação se essas configurações adicionais não forem concluídas. Consulte o artigo para obter um possível impacto na experiência do usuário com a ID alternativa.

Com a configuração adicional a seguir, a experiência do usuário é significativamente melhor, e você pode obter prompts quase zero para autenticação para usuários de ID alternativa em sua organização.

##### <a name="step-1-update-to-required-office-version"></a>Etapa 1. Atualizar para a versão do Office necessária
A versão 1712 do Office (Build no 8827,2148) e superior atualizaram a lógica de autenticação para manipular o cenário de ID alternativo. Para aproveitar a nova lógica, os computadores cliente precisam ser atualizados para a versão 1712 do Office (Build no 8827,2148) e superior.

##### <a name="step-2-update-to-required-windows-version"></a>Etapa 2. Atualizar para a versão do Windows necessária
O Windows versão 1709 e superior atualizaram a lógica de autenticação para manipular o cenário de ID alternativo. Para aproveitar a nova lógica, os computadores cliente precisam ser atualizados para a versão 1709 e posteriores do Windows.

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>Etapa 3. Configurar o registro para usuários afetados usando a política de grupo
Os aplicativos do Office dependem das informações enviadas pelo administrador de diretório para identificar o ambiente de ID alternativo. As seguintes chaves do registro precisam ser configuradas para ajudar os aplicativos do Office a autenticar o usuário com uma ID alternativa sem mostrar prompts extras

|RegKey a adicionar|Nome de dados RegKey, tipo e valor|Windows 7/8|Windows 10|Description|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER \Software\Microsoft\AuthN|LoginHint</br>REG_SZ</br>contoso.com|Obrigatório|Obrigatório|O valor dessa RegKey é um nome de domínio personalizado verificado no locatário da organização. Por exemplo, a Contoso Corp pode fornecer um valor de Contoso.com nessa RegKey se Contoso.com for um dos nomes de domínio personalizado verificados no Contoso.onmicrosoft.com de locatário.|
HKEY_CURRENT_USER \Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Necessário para o Outlook 2016 ProPlus|Necessário para o Outlook 2016 ProPlus|O valor dessa RegKey pode ser 1/0 para indicar ao aplicativo do Outlook se ele deve envolver a lógica de autenticação de ID alternativo aprimorada.|
HKEY_CURRENT_USER \Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obrigatório|Obrigatório|Essa RegKey pode ser usada para definir o STS como uma zona confiável nas configurações da Internet. A implantação padrão do ADFS recomenda adicionar o namespace do ADFS à zona da intranet local para o Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Novo fluxo de autenticação após configuração adicional

![Fluxo de autenticação](media/Configure-Alternate-Login-ID/alt1a.png)

1. r: o usuário é provisionado no Azure AD usando a ID alternativa
   </br>b: o administrador de diretório envia as configurações de RegKey necessárias para computadores cliente impactados
2. O usuário é autenticado no computador local e abre um aplicativo do Office
3. O aplicativo do Office usa as credenciais da sessão local
4. O aplicativo do Office é autenticado no Azure AD usando a dica de domínio enviada por credenciais de administrador e locais
5. O Azure AD autentica com êxito o usuário direcionando para corrigir o realm de Federação e emitir um token

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Aplicativos e experiência do usuário após a configuração adicional

### <a name="non-exchange-and-skype-for-business-clients"></a>Clientes não Exchange e Skype for Business

|Cliente|Declaração de suporte|Comentários|
| ----- | -----|-----|
|Microsoft Teams|Com suporte|<li>O Microsoft Teams dá suporte a AD FS (SAML-P, WS-enalimentate, WS-Trust e OAuth) e à autenticação moderna.</li><li> As principais equipes da Microsoft, como canais, chats e arquivos funcionais, funcionam com a ID de logon alternativa.</li><li>os aplicativos de primeira e de terceiros devem ser investigados separadamente pelo cliente. Isso ocorre porque cada aplicativo tem seus próprios protocolos de autenticação de suporte.</li>|
|OneDrive for Business|Com suporte-chave do registro do lado do cliente recomendada |Com a ID alternativa configurada, você vê que o UPN local é preenchido previamente no campo de verificação. Isso precisa ser alterado para a identidade alternativa que está sendo usada. É recomendável usar a chave do registro do lado do cliente indicada neste artigo: Office 2013 e Lync 2013 solicitam credenciais para o SharePoint Online, OneDrive e Lync Online periodicamente.|
|Cliente móvel do OneDrive for Business|Com suporte||
|Página de ativação do Office 365 Pro Plus|Com suporte-chave do registro do lado do cliente recomendada|Com a ID alternativa configurada, você vê que o UPN local é preenchido previamente no campo de verificação. Isso precisa ser alterado para a identidade alternativa que está sendo usada. É recomendável usar a chave do registro do lado do cliente indicada neste artigo: o Office 2013 e Lync 2013 solicitam periodicamente credenciais para o SharePoint Online, OneDrive e Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Clientes do Exchange e Skype for Business

|Cliente|Instrução de suporte-com HMA|Instrução de suporte-sem HMA|
| ----- |----- | ----- |
|Outlook|Com suporte, sem prompts extras|Com suporte</br></br>Com a **autenticação moderna** para o Exchange Online: com suporte</br></br>Com a **autenticação regular** para o Exchange Online: com suporte com as seguintes advertências:</br><li>Você deve estar em um computador ingressado no domínio e conectado à rede corporativa </li><li>Você só pode usar a ID alternativa em ambientes que não permitem acesso externo para usuários de caixa de correio. Isso significa que os usuários só podem se autenticar em sua caixa de correio de forma compatível quando estão conectados e ingressados na rede corporativa, em uma VPN ou conectados por meio de máquinas de acesso direto, mas você obtém alguns prompts adicionais ao configurar seu perfil do Outlook.|
|Pastas públicas híbridas|Com suporte, sem prompts adicionais.|Com a **autenticação moderna** para o Exchange Online: com suporte</br></br>Com **autenticação regular** para o Exchange Online: sem suporte</br></br><li>As pastas públicas híbridas não poderão ser expandidas se forem usadas IDs alternativas e, portanto, não deverão ser usadas hoje com métodos de autenticação regulares.|
|Delegação entre locais|Consulte [Configurar o Exchange para dar suporte a permissões de caixa de correio delegadas em uma implantação híbrida](/exchange/hybrid-deployment/set-up-delegated-mailbox-permissions)|Consulte [Configurar o Exchange para dar suporte a permissões de caixa de correio delegadas em uma implantação híbrida](/exchange/hybrid-deployment/set-up-delegated-mailbox-permissions)|
|Arquivar o acesso à caixa de correio (caixa de correio local-arquivo na nuvem)|Com suporte, sem prompts extras|Com suporte-os usuários recebem um prompt extra para as credenciais ao acessar o arquivo morto, eles precisam fornecer sua ID alternativa quando solicitado.|
|Outlook Web Access|Com suporte|Com suporte|
|Aplicativos móveis do Outlook para Android, IOS e Windows Phone|Com suporte|Com suporte|
|Skype for Business/Lync|Com suporte, sem prompts extras|Com suporte (exceto conforme observado), mas há um potencial para confusão do usuário.</br></br>Em clientes móveis, há suporte para a ID alternativa somente se o endereço SIP = email address = ID alternativo.</br></br> Os usuários podem precisar entrar duas vezes no cliente do Skype for Business Desktop, primeiro usando o UPN local e, em seguida, usando a ID alternativa. (Observe que o "endereço de entrada" é, na verdade, o endereço SIP que pode não ser o mesmo que o "nome de usuário", embora geralmente seja). Quando um nome de usuário é solicitado pela primeira vez, o usuário deve inserir o UPN, mesmo que ele esteja previamente preenchido com a ID alternativa ou o endereço SIP. Depois que o usuário clicar em entrar com o UPN, o prompt de nome de usuário reaparecerá, desta vez com o UPN. Desta vez, o usuário deve substituir isso pela ID alternativa e clicar em entrar para concluir o processo de entrada. Em clientes móveis, os usuários devem inserir a ID de usuário local na página avançado, usando o formato de estilo SAM (domínio \ nomedeusuário), não o formato UPN.</br></br>Após a entrada bem-sucedida, se o Skype for Business ou o Lync disser "o Exchange precisa de suas credenciais", você precisará fornecer as credenciais que são válidas para onde a caixa de correio está localizada. Se a caixa de correio estiver na nuvem, você precisará fornecer a ID alternativa. Se a caixa de correio for local, você precisará fornecer o UPN local.|

## <a name="additional-details--considerations"></a>Detalhes adicionais & considerações

- O recurso de ID de logon alternativo está disponível para ambientes federados com AD FS implantado.  Não há suporte nos seguintes cenários:
    - Domínios não roteáveis (por exemplo, contoso. local) que não podem ser verificados pelo Azure AD.
    - Ambientes gerenciados que não têm AD FS implantados.


- Quando habilitada, o recurso de ID de logon alternativo só estará disponível para autenticação de nome de usuário/senha em todos os protocolos de autenticação de nome/senha com suporte do AD FS (SAML-P, WS-enalimentated, WS-Trust e OAuth).


- Quando a autenticação integrada do Windows (WIA) é executada (por exemplo, quando os usuários tentam acessar um aplicativo corporativo em um computador ingressado no domínio da intranet e AD FS administrador configurou a política de autenticação para usar o WIA para intranet), o UPN é usado para autenticação. Se você tiver configurado qualquer regra de declaração para as terceiras partes confiáveis para o recurso de ID de logon alternativo, verifique se essas regras ainda são válidas no caso WIA.

- Quando habilitado, o recurso de ID de logon alternativo exige que pelo menos um servidor de catálogo global seja acessível do servidor de AD FS para cada floresta de conta de usuário à qual o AD FS dá suporte. A falha ao acessar um servidor de catálogo global na floresta da conta de usuário resulta em AD FS voltando para usar o UPN. Por padrão, todos os controladores de domínio são servidores de catálogo global.

- Quando habilitado, se o servidor de AD FS encontrar mais de um objeto de usuário com o mesmo valor de ID de logon alternativo especificado em todas as florestas da conta de usuário configurada, ele falhará no logon.

- Quando o recurso de ID de logon alternativo é habilitado, AD FS tenta autenticar o usuário final com a ID de logon alternativa primeiro e, em seguida, retornar para usar o UPN se ele não conseguir encontrar uma conta que possa ser identificada pela ID de logon alternativa. Certifique-se de que não haja conflitos entre a ID de logon alternativa e o UPN se você quiser ainda dar suporte ao logon UPN. Por exemplo, a configuração de um atributo de email com o UPN do outro impede que o outro usuário entre com seu UPN.

- Se uma das florestas configuradas pelo administrador estiver inativa, AD FS continuará a procurar a conta de usuário com a ID de logon alternativa em outras florestas configuradas. Se AD FS Server encontrar objetos de usuário exclusivos nas florestas que ele pesquisou, um usuário fará logon com êxito.

- Além disso, você pode querer personalizar a página de entrada AD FS para dar aos usuários finais alguma dica sobre a ID de logon alternativa. Você pode fazer isso adicionando a descrição da página de entrada personalizada (para obter mais informações, consulte [Personalizando as páginas de entrada AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11)) ou Personalizando a cadeia de caracteres "entrar com a conta organizacional" acima do campo nome de usuário (para obter mais informações, consulte [personalização avançada de AD FS páginas de entrada](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn636121(v=ws.11)).

- O novo tipo de declaração que contém o valor de ID de logon alternativo é **http:schemas. Microsoft. com/WS/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Eventos e contadores de desempenho
Os seguintes contadores de desempenho foram adicionados para medir o desempenho de servidores de AD FS quando a ID de logon alternativa está habilitada:

- Autenticações de ID de logon alternativas: número de autenticações executadas usando a ID de logon alternativa

- Autenticações de ID de logon alternativas/s: número de autenticações executadas usando a ID de logon alternativa por segundo

- Latência média de pesquisa para a ID de logon alternativa: latência média de pesquisa nas florestas que um administrador configurou para a ID de logon alternativa

A seguir estão os vários casos de erro e o impacto correspondente na experiência de entrada de um usuário com eventos registrados por AD FS:



|                       **Casos de erro**                        | **Impacto na experiência de entrada** |                                                              **Evento**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Não é possível obter um valor para SAMAccountName para o objeto de usuário |          Falha de logon           |                  ID do evento 364 com a mensagem de exceção MSIS8012: não é possível encontrar samAccountName para o usuário: ' {0} '.                   |
|        O atributo Canôniconame não está acessível         |          Falha de logon           |               ID do evento 364 com a mensagem de exceção MSIS8013: Canônicaname: ' {0} ' do usuário: ' {1} ' está em formato inadequado.                |
|        Vários objetos de usuário são encontrados em uma floresta        |          Falha de logon           | ID do evento 364 com mensagem de exceção MSIS8015: encontrou várias contas de usuário com a identidade ' {0} ' na floresta ' {1} ' com identidades: {2} |
|   Vários objetos de usuário são encontrados em várias florestas    |          Falha de logon           |           ID do evento 364 com a mensagem de exceção MSIS8014: encontrou várias contas de usuário com a identidade ' {0} ' nas florestas: {1}            |

## <a name="see-also"></a>Consulte Também
[Operações do AD FS](../ad-fs-operations.md)
