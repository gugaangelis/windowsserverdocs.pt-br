---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurar computadores cliente para confiar no servidor de Federação da conta
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8c047f69cb0cb2db57ea33697c49e53a212fe823
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359864"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar computadores cliente para confiar no servidor de Federação da conta

Para que os computadores cliente possam acessar com êxito os aplicativos federados usando Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1, você deve primeiro configurar as configurações do Internet Explorer em cada computador cliente para que o navegador confie na conta servidor de Federação. Você pode fazer isso manualmente ou por meio de Política de Grupo, dependendo da sua preferência administrativa, concluindo um dos procedimentos a seguir.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Definindo manualmente as configurações do Internet Explorer  
Você pode usar o procedimento a seguir para definir manualmente as configurações do Internet Explorer de cada usuário para dar suporte à Federação por meio de AD FS. Se vários usuários usarem um único computador, conclua esse procedimento várias vezes — uma vez para cada perfil de usuário.  
  
Para executar esse procedimento, faça logon como o usuário que acessará aplicativos federados. Essa é uma configuração de perfil @ no__t-0specific. Portanto, é necessário adicionar manualmente essa configuração para cada perfil existente em um computador específico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Para configurar manualmente os computadores cliente para confiar no servidor de Federação da conta  
  
1.  No computador cliente, inicie o Internet Explorer.  
  
2.  No menu **ferramentas** , clique em **Opções da Internet**.  
  
3.  Na guia **segurança** , clique no ícone **intranet local** e, em seguida, clique em **sites**.  
  
4.  Clique em **avançado**e, em **Adicionar este site à zona**, digite o nome completo do sistema de nome de domínio \(DNS @ no__t-3 do servidor de Federação da conta \(for exemplo, https: @no__t -5\/fs1.fabrikam.com @ no__t-7 e clique em **Adicionar** .  
  
5.  Clique em **OK** três vezes.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Definindo as configurações do Internet Explorer usando Política de Grupo  
Para a maioria das implantações, recomendamos que você use Política de Grupo para enviar por push as configurações apropriadas do Internet Explorer para cada computador cliente.  
  
A associação em **Administradores de domínio** ou administradores de **empresa**, ou equivalente, em Active Directory Domain Services \(AD DS @ no__t-3 é o mínimo necessário para concluir este procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em \( [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? \=LinkId 83477.\)   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Para configurar computadores cliente para confiar no servidor de Federação da conta usando Política de Grupo  
  
1.  Em um controlador de domínio na floresta da organização do parceiro de conta, inicie o snap do **política de grupo Management** @ no__t-1in.  
  
2.  Localize o objeto Política de Grupo apropriado \(GPO @ no__t-1, Right @ no__t-2Click-o e clique em **Editar**.  
  
3.  Na árvore de console, abra **configuração de usuário @ no__t-1Preferences @ no__t-2Windows configurações @ no__t-3Internet Explorer Maintenance**e clique em **segurança**.  
  
4.  No painel de detalhes, clique duas vezes nas **zonas de segurança e nas classificações de conteúdo**do @ no__t-0click.  
  
5.  Em **zonas de segurança e privacidade**, clique em **importar as configurações de privacidade e zonas de segurança atuais**e clique em **Modificar configurações**.  
  
6.  Clique em **intranet local**e em **sites**.  
  
7.  Em **Adicionar este site à zona**, digite o nome DNS completo do servidor de Federação de conta \(Para exemplo, https: @no__t -2\/fs1.fabrikam.com @ no__t-4, clique em **Adicionar**e em **Fechar**.  
  
8.  Clique em **OK** duas vezes para aplicar essas alterações ao política de grupo.  
  
