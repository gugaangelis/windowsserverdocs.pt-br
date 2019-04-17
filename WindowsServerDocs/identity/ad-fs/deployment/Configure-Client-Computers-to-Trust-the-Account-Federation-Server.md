---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: "Configurar computadores cliente para confiar no servidor de federação de conta"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar computadores cliente para confiar no servidor de federação de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que os computadores cliente com êxito podem acessar aplicativos federados usando os serviços de Federação do Active Directory \(AD FS\), primeiro você deve configurar as configurações do Internet Explorer em cada computador cliente para que o navegador confia no servidor de federação de conta. Você pode fazer isso manualmente ou por meio da política de grupo, dependendo da sua preferência administrativa concluindo um dos procedimentos a seguir.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Definir configurações do Internet Explorer manualmente  
Você pode usar o procedimento a seguir para definir configurações do Internet Explorer de cada usuário para dar suporte a federação por meio do AD FS manualmente. Se vários usuários usarem um único computador, concluir este procedimento várias vezes — uma vez para cada perfil de usuário.  
  
Para executar este procedimento, faça logon como o usuário que acessará aplicativos federados. Essa é uma configuração profile\ específicos. Portanto, ele requer que você adicione manualmente essa configuração para cada perfil que existe em um computador específico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Configurar manualmente os computadores cliente para confiar no servidor de federação de conta  
  
1.  No computador cliente, inicie o Internet Explorer.  
  
2.  Sobre o **ferramentas** menu, clique em **opções da Internet**.  
  
3.  Sobre o **segurança**, clique no **intranet Local** ícone e clique **Sites**.  
  
4.  Clique em **avançado**e em **adicionar este site à zona**, digite o nome completo do sistema de nomes de domínio \(DNS\) do servidor de federação de conta \ (por exemplo, https:\/\/fs1.fabrikam.com\) e, em seguida, clique em **adicionar**.  
  
5.  Clique em **Okey** três vezes.  
  
## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>Definir configurações do Internet Explorer usando a política de grupo  
Para a maioria das implantações, recomendamos que você use a política de grupo para enviar por push as configurações do Internet Explorer apropriadas para cada computador cliente.  
  
A associação ao grupo **Admins. do domínio** ou **administradores corporativos**, ou equivalente, no Active Directory Domain Services \(AD DS\) é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>Para configurar computadores cliente para confiar no servidor de federação de conta usando a política de grupo  
  
1.  Em um controlador de domínio na floresta de organização do parceiro de conta, inicie o **Group Policy Management** snap\-in.  
  
2.  Encontre \(GPO\) o objeto de política de grupo apropriado, clique right\ e clique **editar**.  
  
3.  Na árvore de console, abra **Configuration\\Preferences\\Windows usuário Settings\\Internet manutenção do Internet Explorer**e clique em **segurança**.  
  
4.  No painel de detalhes, clique em double\ **zonas de segurança e classificações de conteúdo**.  
  
5.  Em **zonas de segurança e privacidade**, clique em **importar as zonas de segurança atuais e as configurações de privacidade**e clique em **modificar configurações**.  
  
6.  Clique em **intranet Local**e clique em **Sites**.  
  
7.  Em **adicionar este site à zona**, digite o nome completo do DNS do servidor de federação de conta \ (por exemplo, https:\/\/fs1.fabrikam.com\), clique em **adicionar**e clique em **fechar**.  
  
8.  Clique em **Okey** duas vezes para aplicá-las à política de grupo.  
  
