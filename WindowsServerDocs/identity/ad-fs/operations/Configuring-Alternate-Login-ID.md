---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configurando o ID de logon alternativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb4c98ff1090a9d3c35654614a43e12db99d691d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-alternate-login-id"></a>Configurando o ID de logon alternativo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Os usuários podem fazer logon aplicativos habilitados serviços do Active Directory federação (AD FS) usando qualquer forma de identificador do usuário que é aceito pelos serviços de domínio do Active Directory (AD DS). Isso inclui os nomes de entidade de segurança do usuário (UPNs) (johndoe@contoso.com) ou domínio qualificado nomes de conta sam (contoso\johndoe ou contoso.com\johndoe #).

Em alguns ambientes, devido a política corporativa ou dependências de aplicativos de linha de negócios no local, os usuários finais podem apenas Lembre-se dos seus nomes de endereço e não o nome UPN ou conta sam de email. Em alguns casos, o UPN também é não pode ser roteado (jdoe@contoso.local) e só é usada para autenticação em aplicativos na rede corporativa.

Desde não pode ser roteado domínios (ex.: Propriedade contoso. local) não pode ser verificada, Office 365 exige que todos os IDs para ser totalmente internet pode ser roteado de logon do usuário. Se o UPN local usa um domínio não pode ser roteado (ex.: Contoso. local), ou o UPN existente não pode ser modificado devido a dependências do aplicativo local, recomendamos que configurar a ID de logon alternativo. ID de logon alternativo permite que você configure um log na experiência onde os usuários podem fazer logon com um atributo que não seja seu UPN, como mail.

Um dos benefícios desse recurso é que ele permite que você adotar SaaS provedores, como o Office 365 sem modificar seu UPNs local. Também permite que você dê suporte a aplicativos de serviço de linha de negócios com identidades provisionados consumidor.

> [!IMPORTANT]
> Usando o ID alternativo em ambientes híbridos com Exchange e/ou Skype for Business é aceito, mas não é recomendado. Usando o mesmo conjunto de credenciais (por exemplo, o UPN) local e online fornece a melhor experiência de usuário em um ambiente híbrido.  A Microsoft recomenda que os clientes alterar seus UPNs se possível para evitar a necessidade de ID alternativo Usando o ID alternativo com o Lync ou Skype for business requer o Lync Server 2013 ou posterior. Os clientes que usam a ID alternativo devem considerar a ativação [autenticação modernos](https://support.office.com/article/using-office-365-modern-authentication-with-office-clients-776c0036-66fd-41cb-8928-5495c0f9168a) Exchange no Office 365 para uma melhor experiência de usuário. Além disso, os clientes que usam o Skype para empresas com clientes móveis devem garantir que o endereço SIP é idêntico ao endereço de email do usuário (e ID alternativo). 

Consulte a tabela abaixo para a experiência do usuário com ID alternativo usando vários clientes do Office 365 com autenticação normal, autenticação modernos e autenticação baseada em certificado (requer a habilitação da autenticação moderno).

|**Tipos de cliente**|**Informações adicionais**|**Suporte à declaração - autenticação normal e moderno**|**Descrição**|
|--------------------|------------------------------|------------------------------|------------------------------|
|Outlook|Autenticação normal: Você deve estar em uma máquina de domínio associado e conectado à rede corporativa<br /><br />Autenticação moderna: compatíveis|Você só pode usar o ID alternativo em ambientes que não permitir acesso externo para usuários de caixa de correio. Isso significa que os usuários podem apenas autenticar a caixa de correio há uma maneira com suporte quando forem conectados e associados à rede corporativa, em uma VPN ou conectados via acesso direto. Se você aderir para configurar a autenticação modernos (conhecida como ADAL) você pode usar o Outlook de máquinas de ingressar/conectados sem domínio, mas você receberá algumas solicitações extras ao configurar seu perfil do Outlook.<br /><br />Veja a primeira imagem abaixo da tabela para demonstração de experiência do usuário.|[Autenticação moderna no Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Pastas públicas híbrida|Autenticação normal: Não suporte<br /><br />Autenticação moderna: compatíveis|Pastas públicas híbrido não poderá expandir se IDs alternativas são usados e, portanto, não deve ser usado hoje com métodos de autenticação normal. Se você quiser ser capaz de usar uma pasta pública no híbrido, você precisará configurar a autenticação modernos (conhecida como ADAL).<br /><br />Veja a primeira imagem abaixo da tabela para demonstração de experiência do usuário.|[Autenticação moderna no Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Transpor local delegação|Sem suporte|Transpor atualmente local permissões não têm suporte em uma configuração híbrida, mas eles também não funcionará se você usar AltID.||
|Acesso de caixa de correio de arquivo morto (caixa de correio local - arquivo morto na nuvem)|Com suporte|Os usuários receberão um prompt para credenciais extra ao acessar o arquivo morto, ele terá que fornecer lá alternativo ID quando solicitado.<br /><br />Veja a primeira imagem abaixo da tabela para demonstração de experiência do usuário.||
|Página do Office 365 Pro Plus ativação|Suporte - chave de registro do lado cliente recomendado|Com ID alternativo configurado, você verá que o UPN local é previamente preenchido no campo de verificação. Isso precisa ser alterado para a identidade alternativa que está sendo usada. É recomendável usar a chave do registro lado cliente observada na coluna do link.<br /><br />Veja a segunda imagem abaixo da tabela para demonstração de experiência do usuário.|[Office 2013 e o Lync 2013 periodicamente solicitar credenciais SharePoint Online, OneDrive e Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|Equipes da Microsoft|Com suporte|Microsoft Teams compatível com o AD FS (SAML P, WS-alimentadas, WS-Trust e OAuth) e autenticação modernos.<br/><br/> Core Teams da Microsoft, como as funcionalidades de canais, bate-papos e arquivos funciona com o ID de logon Altnernate. </br></br>Aplicativos de terceiros 1º e 3º precisam ser investigada separadamente pelo cliente. Isso ocorre porque cada aplicativo tem suas própria protocolos de autenticação de suporte.| 
|Skype para empresas / Lync|Com suporte (exceto conforme observado), mas há a possibilidade de confusão do usuário.  No clientes móveis, Id alternativa é compatível somente se o endereço SIP = endereço de email = ID alternativo|Os usuários talvez seja necessário entrar duas vezes para o Skype para o cliente de desktop de negócios, pela primeira vez usando o UPN no local e, em seguida, usando a ID alternativo. (Observe que o "endereço Sign-in" é, na verdade, o endereço SIP que pode não ser o mesmo que o "nome de usuário", mas muitas vezes é).  Quando solicitado pela primeira vez para um nome de usuário, o usuário deve inserir o UPN, mesmo se ele incorretamente é previamente preenchido com o endereço SIP ou ID alternativo. Depois que o usuário clicar em entrar com o UPN, o usuário prompt nome reaparecerá, desta vez previamente populado com o UPN. Neste momento, o usuário deve substituir isso com a ID alternativo e clique em login para concluir o processo de login. Nos clientes móveis, os usuários devem inserir a ID de usuário local na página avançados, usando o formato de estilo SAM (domínio \ usuário), não o formato UPN.<br /><br />Depois de entrar com êxito, se o Skype for Business ou Lync disser "Exchange precisa de suas credenciais", você precisa fornecer as credenciais que são válidas para onde a caixa de correio está localizada. Se a caixa de correio está na nuvem que você precisa fornecer a ID alternativo. Se a caixa de correio for local, você precisa fornecer o UPN no local.|[Autenticação moderna no Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|O Outlook Web Access|Com suporte|||
|Aplicativos móveis do Outlook para Android, IOS e Windows Phone|Com suporte|||
|OneDrive para empresas|Suporte - chave de registro do lado cliente recomendado|Com ID alternativo configurado, você verá que o UPN local é previamente preenchido no campo de verificação. Isso precisa ser alterado para a identidade alternativa que está sendo usada. É recomendável usar a chave do registro lado cliente observada na coluna do link.<br /><br />Veja a segunda imagem abaixo da tabela para demonstração de experiência do usuário.|[Office 2013 e o Lync 2013 periodicamente solicitar credenciais SharePoint Online, OneDrive e Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|OneDrive para o cliente de negócios móvel|Com suporte|||

![Logon alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID1.png)

![Logon alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID2.png)

![Logon alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID3.png)

Abaixo, as capturas de tela a seguir são um exemplo adicional usando o Skype for Business.  No exemplo, as informações a seguir são usadas


- SIP:userA@contoso.com 
- UPN:userA@contoso.local
- Email:userA@contoso.com
- AltId:userA@contoso.com 

Insira o endereço SIP no campo de entrada.

![Skype](media/Configure-Alternate-Login-ID/SkypeA.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeB.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeC.png)

## <a name="to-configure-alternate-login-id"></a>Para configurar a ID de logon alternativo
Para definir a ID de logon alternativo, você deve executar as seguintes tarefas:

Configurar seu relações de confiança do AD FS requerimentos judiciais ou Extrajudiciais provedor para habilitar o ID de logon alternativo

1.  Instalar [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590).  Você pode obtê-lo por meio de serviços de atualização do Windows ou baixá-lo diretamente.

2.  Atualize a configuração do AD FS executando o seguinte cmdlet do PowerShell em qualquer um dos servidores de federação no farm (se você tiver um farm de trabalho, você deve executar esse comando no servidor do AD FS principal no seu farm):

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>

    ```

    **AlternateLoginID** é o nome LDAP do atributo que você deseja usar para logon.

    **LookupForests** é a lista de floresta DNS que seus usuários pertencem a.

    Para habilitar o recurso de ID de logon alternativo, configure parâmetros AlternateLoginID - e - LookupForests com um valor diferente de nulo, válido.

    No exemplo a seguir, você está ativando a funcionalidade de ID de logon alternativo forma que os usuários com contas em florestas contoso.com e fabrikam.com podem fazer logon aplicativos do AD FS habilitado com o atributo "email".

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
    ```

3.  Para desabilitar esse recurso, defina o valor para ambos os parâmetros ser null.

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
    ```

4.  Para habilitar o ID de logon alternativo com o Azure AD, não há etapas de configurações adicionais são necessárias ao usar o Azure AD Connect.   ID alternativa pode ser configurado diretamente do assistente.  Consulte identifica os usuários na seção [conectar-se ao Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/#connect-to-azure-ad).

## <a name="additional-details--considerations"></a>Considerações e detalhes adicionais

-   O recurso de ID de logon alternativo só está disponível para ambientes agrupados com o AD FS implantado.  Não há suporte nos seguintes cenários:
    -   Domínios não pode ser roteado (por exemplo, contoso. local) que não podem ser verificados pelo Azure AD.
    -   Ambientes que não têm o AD FS implantado gerenciados.


-   Quando habilitada, o recurso de ID de logon alternativo só está disponível para autenticação de nome de usuário/senha em todos os usuário nome/senha protocolos de autenticação com suporte pelo AD FS (SAML P, WS-alimentadas, WS-Trust e OAuth).


-   Quando a autenticação integrada do Windows (WIA) é executada (por exemplo, quando os usuários tentam acessar um aplicativo corporativo em um computador ingressado no domínio da intranet e do AD FS administrador tiver configurado a política de autenticação para usar WIA para intranet), UPN será usada para autenticação. Se você tiver configurado qualquer reivindicação regras para os participantes terceira para recurso de ID de logon alternativo, você deve verificar se que essas regras ainda são válidas no caso de WIA.

-   Quando habilitada, o recurso de ID de logon alternativo requer pelo menos um servidor de catálogo global pode ser alcançado de servidor do AD FS para cada floresta de conta de usuário compatíveis com o AD FS. Falha para alcançar um servidor de catálogo global na floresta de conta do usuário resultará no AD FS voltavam a usar o nome UPN. Por padrão, todos os controladores de domínio são servidores de catálogo global.

-   Quando habilitada, se o servidor do AD FS localizar mais de um objeto de usuário com o mesmo valor de ID de logon alternativo especificado em todas as florestas de conta de usuário configurado, ele irá falhar o logon.

-   Quando o recurso de ID de logon alternativo é habilitado, o AD FS tentará autenticar o usuário final com ID de logon alternativo primeiro e depois retornar para usar o UPN se não encontrar uma conta que pode ser identificada pelo ID de logon alternativo. Você deve verificar se que há sem conflitos entre a ID de logon alternativo e o UPN se quiser ainda oferece suporte a logon UPN. Por exemplo, o atributo de email de configuração de um com UPN da outra bloqueará o outro usuário de logon com seu UPN.

-   Se uma das florestas que esteja configurado pelo administrador for pressionada, o AD FS continuará procurar a conta de usuário com ID de logon alternativo em outras florestas que estão configurados. Se o servidor do AD FS encontrar um único usuário objetos florestas que ele pesquisou, um usuário fará logon com êxito.

-   Além disso convém personalizar a página de entrada do AD FS para dar aos usuários finais alguns dica sobre a ID de logon alternativo. Você pode fazer isso adicionando a descrição da página de entrada personalizada (para obter mais informações, consulte [Personalizando as páginas do AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) ou personalizando a cadeia de caracteres "faça logon com contas da organização" acima do campo de nome de usuário (para obter mais informações, consulte [personalização avançada do AD FS Sign-in páginas](https://technet.microsoft.com/library/dn636121.aspx).

-   O novo tipo de declaração que contém o valor de ID de logon alternativo é **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Eventos e contadores de desempenho
Os seguintes contadores de desempenho foram adicionados para medir o desempenho dos servidores do AD FS quando o ID de logon alternativo é ativado:

-   Alternativas autenticações de Id de logon: número de autenticações realizada por meio da ID de logon alternativo

-   Alternativo autenticações/s de Id de logon: o número de autenticações realizada por meio da ID de logon alternativo por segundo

-   Média de latência de pesquisa para o ID de logon alternativo: média de latência de pesquisa florestas que um administrador tiver configurado para o ID de logon alternativo

A seguir estão diversos casos de erro e correspondente impacto sobre a experiência do usuário entrar com eventos registrados pelo AD FS:



**Casos de erro**|**Impacto na experiência de entrada**|**Evento**|
---------|---------|---------
Não é possível obter um valor SAMAccountName para o objeto de usuário|Falha de logon|ID de evento 364 com mensagem de exceção MSIS8012: não é possível localizar o samAccountName para o usuário: '{0}'.|
O atributo CanonicalName não é acessível|Falha de logon|ID de evento 364 com mensagem de exceção MSIS8013: CanonicalName: '{0}' do usuário: '{1}' está no formato incorreto.|
Vários objetos de usuário são encontrados no one florestas|Falha de logon|ID de evento 364 com mensagem de exceção MSIS8015: encontradas várias contas de usuário com identidade '{0}' na floresta '{1}' com identidades: {2}|
Vários objetos de usuário são encontrados em várias florestas|Falha de logon|ID de evento 364 com mensagem de exceção MSIS8014: encontradas várias contas de usuário com identidade '{0}' em florestas: {1}|

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)


