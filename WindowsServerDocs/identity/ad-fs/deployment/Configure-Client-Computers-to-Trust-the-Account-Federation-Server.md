---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurar computadores cliente para confiar no servidor de federação de conta
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886747"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar computadores cliente para confiar no servidor de federação de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que computadores cliente possam acessar com êxito os aplicativos federados usando serviços de Federação do Active Directory \(do AD FS\), primeiro você deve configurar as configurações do Internet Explorer em cada computador cliente para que o navegador confie o servidor de federação de conta. Você pode fazer isso manualmente ou por meio de diretiva de grupo, dependendo de sua preferência administrativa, executando um dos procedimentos a seguir.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Definindo as configurações do Internet Explorer manualmente  
Você pode usar o procedimento a seguir para configurar manualmente as configurações do Internet Explorer de cada usuário para dar suporte à Federação por meio do AD FS. Se vários usuários usam um único computador, concluir este procedimento várias vezes — uma vez para cada perfil de usuário.  
  
Para executar este procedimento, faça logon como o usuário que terá acesso a aplicativos federados. Este é um perfil\-configuração específica. Portanto, ele requer que você adicione manualmente essa configuração para cada perfil que existe em um computador específico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Para configurar manualmente os computadores cliente para confiar no servidor de federação de conta  
  
1.  No computador cliente, inicie o Internet Explorer.  
  
2.  Sobre o **ferramentas** menu, clique em **opções da Internet**.  
  
3.  Sobre o **segurança** , clique no **intranet Local** ícone e clique **Sites**.  
  
4.  Clique em **Advanced**e, na **adicionar este site à zona**, digite o sistema de nome de domínio completo \(DNS\) nome do servidor de federação de conta \(por exemplo, https :\/\/fs1.fabrikam.com\)e, em seguida, clique em **Add**.  
  
5.  Clique em **OK** três vezes.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configurar configurações do Internet Explorer usando a diretiva de grupo  
Na maioria das implantações, é recomendável que você use Diretiva de grupo para enviar por push as configurações apropriadas do Internet Explorer para cada computador cliente.  
  
Associação na **Admins. do domínio** ou **administradores de empresa**, ou equivalente, nos serviços de domínio do Active Directory \(AD DS\) é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Para configurar computadores cliente para confiar no servidor de federação de conta usando a diretiva de grupo  
  
1.  Em um controlador de domínio na floresta da organização do parceiro de conta, inicie o **gerenciamento de política de grupo** encaixar\-no.  
  
2.  Localizar o objeto de diretiva de grupo apropriadas \(GPO\)certo\-clicar nele e, em seguida, clique em **editar**.  
  
3.  Na árvore de console, abra **configuração do usuário\\preferências\\configurações do Windows\\manutenção do Internet Explorer**e, em seguida, clique em **segurança**.  
  
4.  No painel de detalhes, clique duas vezes\-clique em **zonas de segurança e classificações de conteúdo**.  
  
5.  Sob **zonas de segurança e privacidade**, clique em **importar as zonas de segurança atuais e as configurações de privacidade**e, em seguida, clique em **modificar configurações**.  
  
6.  Clique em **intranet Local**e, em seguida, clique em **Sites**.  
  
7.  Na **adicionar este site à zona**, digite o nome DNS completo do servidor de federação de conta \(por exemplo, https:\/\/fs1.fabrikam.com\), clique em **adicionar**e, em seguida, clique em **fechar**.  
  
8.  Clique em **Okey** duas vezes para aplicar essas alterações à política de grupo.  
  
