---
title: Personalizar declarações a serem emitidos no id_token ao usar o OAuth ou OpenID Connect com o AD FS 2016
description: Uma visão geral técnica de conecpts de token de id personalizada no AD FS 2016
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 8c8e14f22a4bca5b6d32e841814a58a4b4ddca01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820637"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>Personalizar declarações a serem emitidos no id_token ao usar o OAuth ou OpenID Connect com o AD FS 2016

## <a name="overview"></a>Visão geral
O artigo [aqui](enabling-openId-connect-with-ad-fs.md) mostra como criar um aplicativo que usa o AD FS para o OpenID Connect do logon. No entanto, por padrão existem apenas um conjunto fixo de declarações disponíveis no id_token. AD FS 2016 tem a capacidade de personalizar o id_token em cenários de OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Quando são ID personalizada token usado?
Em determinados cenários, é possível que o aplicativo cliente não tem um recurso que está tentando acessar. Portanto, ele não precisa realmente um token de acesso. Nesses casos, o aplicativo cliente precisa basicamente apenas um ID token, mas com algumas declarações adicionais para ajudá-lo a funcionalidade.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quais são as restrições sobre como obter declarações personalizadas na ID de token?

### <a name="scenario-1"></a>Cenário 1

![restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode é definido como form_post
2.  Somente os clientes públicos podem obter declarações personalizadas na ID de token
3.  Identificador de terceira parte confiável deve ser o mesmo que o identificador de cliente

### <a name="scenario-2"></a>Cenário 2

![restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Com o [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instalado nos servidores do AD FS
1.  response_mode é definido como form_post
2.  Atribua allatclaims de escopo para o cliente – par da RP.
Você pode atribuir o escopo usando o cmdlet Grant ADFSApplicationPermission conforme indicado no exemplo a seguir:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Criando um aplicativo OAuth para lidar com declarações personalizadas no token de ID
Use o artigo [aqui](Enabling-OpenId-Connect-with-AD-FS-2016.md) para criar um aplicativo do aplicativo que usa o AD FS para o OpenID Connect de logon. Em seguida, siga as etapas abaixo para configurar o aplicativo no AD FS para receber o token de ID com declarações personalizadas.

### <a name="create-the-application-group-in-ad-fs-2016"></a>Crie o grupo de aplicativos no AD FS 2016

1.  Crie um grupo de aplicativos com base no novo modelo, mostrado abaixo, chamado CustomTokenClient.

![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. Este modelo cria um cliente confidencial. Observe o identificador e especifique o URI de retorno como a URL do SSL do projeto VS.

![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  Na próxima etapa, selecione **gerar um segredo compartilhado** para criar as credenciais de cliente e copiar as credenciais do cliente geradas.

![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. Clique em **próxima** e continue para concluir o assistente.

![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>Criar a terceira parte confiável
Para adicionar declarações personalizadas na ID de token, você precisa criar uma RP cujas declarações serão adicionadas no token de ID. Use o assistente Adicionar terceira parte confiável para criar uma nova parte confiável, como mostrado abaixo:
 
![Terceira parte confiável](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

Depois de terceira parte confiável for criada, clique com o botão direito na entrada de terceiros de terceira parte confiável e selecione **Editar política de emissão de declaração** para adicionar regras de emissão de declarações. Adicione as declarações personalizadas necessárias para o token de ID, conforme mostrado abaixo:

![Terceira parte confiável](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>Atribuir o escopo de "allatclaims" para o par de cliente e a terceira parte confiável
Usando o PowerShell no servidor do AD FS, atribua o novo escopo allatclaims conforme indicado no exemplo a seguir (altere o clientID e o servidor:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>Alterar o ClientRoleIdentifier e ServerRoleIdentifier acordo com as configurações de aplicativo

## <a name="test-the-custom-claims-in-id-token"></a>Testar as declarações personalizadas no token de ID

Em seguida, usando o mesmo trecho de código que você sempre usou para acessar as declarações, você pode ver as declarações adicionais que se tornará parte do id_token.
Por exemplo, em um aplicativo de exemplo .NET MVC, abra os arquivos do controlador e insira o código como o abaixo:


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
>Esteja ciente de que o parâmetro de recurso é necessário na solicitação de Oauth2.
>
>Exemplo incorreto:
>
>**HTTPS&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token & escopo = openid & redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 & client_id = 6db3ec2a-075a - 4c 72 9b22 ca7ab60cb4e7 & estado = 42c2c156aef47e8d0870 & resource = 6db3ec2a-075a - 4c 72 9b22 ca7ab60cb4e7**
>
>Bom exemplo:
>
>**HTTPS&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token & escopo = openid & redirect_uri = https&#58;//myportal/auth & nonce = b3ceb943fc756d927777 & client_id = 6db3ec2a-075a - 4c 72 9b22 ca7ab60cb4e7 & estado = 42c2c156aef47e8d0870 & resource = https&#58;//customidrp1/ & response_mode = form_post**
>
>Se o parâmetro de recurso não estiver na solicitação, que você pode receber um erro ou um token sem quaisquer declarações personalizadas.

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
