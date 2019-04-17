---
title: "Personalizar declarações para ser emitido em id_token quando usando OAuth ou OpenID se conectar com o AD FS 2016"
description: "Uma visão geral técnica das conecpts de token a identificação personalizada no AD FS 2016"
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c17d50bb6d90726eafc3e585f16a7a8189a68392
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personalizar declarações para ser emitido em id_token quando usando OAuth ou OpenID se conectar com o AD FS 2016

## <a name="overview"></a>Visão geral
O artigo [aqui](enabling-openId-connect-with-ad-fs.md) mostra como criar um aplicativo que usa o AD FS para conectar OpenID logon. No entanto, por padrão há apenas um conjunto fixo de declarações disponíveis no id_token. AD FS 2016 tem a funcionalidade de personalizar o id_token em cenários OpenID se conectar.

## <a name="when-are-custom-id-token-used"></a>Quando são identificação personalizada token usado?
Em determinados cenários, é possível que o aplicativo cliente não tem um recurso que está tentando acessar. Portanto, ele não precisa realmente um token de acesso. Nesses casos, o aplicativo cliente precisa essencialmente apenas um ID token, mas com algumas declarações adicionais para ajudar na funcionalidade.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quais são as restrições sobre a obtenção declarações personalizadas em ID token?

### <a name="scenario-1"></a>Cenário 1

![Restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode é definido como form_post
2.  Somente os clientes públicos podem obter declarações personalizadas na ID token
3.  Terceira identificador de terceiros deve ser igual ao identificador de cliente

### <a name="scenario-2"></a>Cenário 2

![Restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Com [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instaladas nos servidores do AD FS
1.  response_mode é definido como form_post
2.  Atribua allatclaims escopo para o cliente – par RP.
Você pode atribuir o escopo usando o cmdlet Grant-ADFSApplicationPermission conforme indicado no exemplo a seguir:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Criando um aplicativo de OAuth para lidar com declarações personalizadas no token de ID
Use o artigo [aqui](Enabling-OpenId-Connect-with-AD-FS-2016.md) para criar um aplicativo de aplicativo que usa o AD FS para se conectar OpenID logon. Siga as etapas abaixo para configurar o aplicativo no AD FS para recebimento de token de ID com declarações personalizadas.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Criar o grupo de aplicativos no AD FS 2016

1.  Crie um grupo de aplicativos com base no novo modelo, mostrado abaixo, chamado CustomTokenClient.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Esse modelo cria um cliente confidencial. Observe o identificador e especifique o URI de retorno como a URL de SSL do projeto do VS.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  Na próxima etapa, selecione **gerar um segredo compartilhado** criar credenciais de cliente e copiar as credenciais de cliente geradas.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Clique em **próxima** e vá para concluir o assistente.

![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Criar o terceiro
Para adicionar declarações personalizadas na ID token, você precisa criar uma RP cujos requerimentos judiciais ou Extrajudiciais serão adicionados no token do ID. Use o Assistente para Adicionar dependência terceiros confiança para criar um novo terceiro, como mostrado a seguir:
 
![Terceiro](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Depois de terceiro é criado, clique com o botão direito na terceira entrada de terceiros e selecione **Editar reivindicar a política de emissão** adicionar regras de emissão de declarações. Adicione as declarações necessárias personalizadas para o token de ID, como mostrado a seguir:

![Terceiro](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Atribuir o escopo de "allatclaims" ao par de cliente e o terceiro
Usando o PowerShell no servidor do AD FS, atribua o novo escopo allatclaims conforme indicado no exemplo a seguir (alterar o clientID e o servidor:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Alterar a ClientRoleIdentifier e ServerRoleIdentifier acordo com as configurações do aplicativo

## <a name="test-the-custom-claims-in-id-token"></a>Testar as declarações personalizadas no token de ID

Em seguida, usando o mesmo trecho de código que você sempre tenha usado para acessar requerimentos judiciais ou Extrajudiciais, você pode ver as declarações adicionais que se tornarão parte do id_token.
Por exemplo, em um aplicativo de amostra MVC .NET, abra um dos arquivos de controlador e insira o código como o indicado a seguir:


``` code
    [Authorize]
    public ActionResult About()
    {

        ClaimsPrincipal cp = ClaimsPrincipal.Current;

        string userName = cp.FindFirst(ClaimTypes.GivenName).Value;
        ViewBag.Message = String.Format("Hello {0}!", userName);
        return View();

    }

```

>[!NOTE]
>Esteja ciente de que o parâmetro de recurso é necessário na solicitação Oauth2.
>
>Exemplo ruim:
>
>**HTTPS & #58; / / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7**
>
>Bom exemplo:
>
>**HTTPS & #58; / / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=https&#58;//customidrp1/**
>
>Se o parâmetro de recurso não estiver na solicitação que você pode receber um erro ou um token sem qualquer declarações personalizadas.

## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  