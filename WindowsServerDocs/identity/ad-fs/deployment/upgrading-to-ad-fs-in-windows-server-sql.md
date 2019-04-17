---
title: Atualizar para o AD FS no Windows Server 2016 com o SQL Server
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 034d4f1f8d81cf105ba94bff34b180555702acde
ms.sourcegitcommit: 33c1f4965cd2eed7d384a096cfa5c883467b16a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2017
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Atualizar para o AD FS no Windows Server 2016 com o SQL Server

>Aplica-se a: Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Mudando de um farm de Windows Server 2012 R2 AD FS para um farm AD FS do Windows Server 2016  
Este documento descreve como atualizar seu farm do AD FS Windows Server 2012 R2 para AD FS no Windows Server 2016 quando você estiver usando um SQL Server para o banco de dados do AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Atualização do AD FS para o Windows Server 2016 FBL  
Novo no AD FS para o Windows Server 2016 é o recurso de nível do comportamento de farm (FBL).   Esse recurso é farm ampla e determina os recursos que pode usar o farm AD FS.   Por padrão, o FBL em um farm de Windows Server 2012 R2 AD FS está no Windows Server 2012 R2 FBL.  

Um servidor Windows Server 2016 AD FS pode ser adicionado a um farm de Windows Server 2012 R2 e ele funcionará na mesma FBL como um Windows Server 2012 R2.  Quando você tiver um servidor Windows Server 2016 AD FS operacional dessa maneira, seu farm diz "mixed".  No entanto, você não poderá tirar proveito dos novos recursos do Windows Server 2016 até que o FBL é gerado para o Windows Server 2016.  Com um farm misto:  

-   Os administradores podem adicionar novas, servidores de Federação do Windows Server 2016 a um farm existente do Windows Server 2012 R2.  Como resultado, o farm estiver no "modo misto" e opera o nível de comportamento de farm do Windows Server 2012 R2.  Para garantir um comportamento consistente no farm, novos recursos do Windows Server 2016 não podem ser configurados ou usados nesse modo.  

-   Depois que todos os servidores de Federação do Windows Server 2012 R2 foram removidos da Fazenda modo misto e, no caso de um farm de trabalho, um dos novos servidores de Federação Windows servir 2016 foi promovido para a função de nó principal, o administrador pode, em seguida, gere FBL do Windows Server 2012 R2 para o Windows Server 2016.  Como resultado, os novos recursos de 2016 do AD FS Windows Server podem ser configurados e usados.  

-   Como resultado do recurso farm misto, AD FS Windows Server 2012 R2 organizações pensando em atualizar para o Windows Server 2016 não precisará implantar um farm totalmente novo, exportar e importar dados de configuração.  Em vez disso, eles podem adicionar nós de Windows Server 2016 a um farm existente enquanto ele estiver online e gerar somente o tempo de inatividade relativamente breve envolvidas na eleve FBL.  

Lembre-se de que no modo misto farm, sítio AD FS não é capaz de quaisquer novos recursos ou funcionalidade introduzida no AD FS no Windows Server 2016.  Isso significa que as organizações que desejam experimentar novos recursos não podem fazer isso até que o FBL é acionado.  Então se sua organização estiver buscando para testar os novos recursos antes da rasing o FBL, você precisará implantar um farm separado para fazer isso.  

O restante do documento fornece as etapas para adicionar um servidor de Federação do Windows Server 2016 para um ambiente do Windows Server 2012 R2 e, em seguida, acionar o FBL para Windows Server 2016.  Essas etapas foram executadas em um ambiente de teste descrito no diagrama da arquitetura abaixo.  

> [!NOTE]  
> Antes de você pode mover para o AD FS no Windows Server 2016 FBL, você deve remover todos os nós do Windows 2012 R2.  Você não pode atualizar um sistema de operacional do Windows Server 2012 R2 para o Windows Server 2016 e fazer com que ele se tornar um nó de 2016.  Você precisará removê-lo e substituí-lo com um novo nó de 2016.  

A arquitetura diagrama a seguir mostra a configuração que foi usada para validar e gravar as etapas abaixo.

![Arquitetura](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png) 


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Ingresse o servidor do Windows 2016 AD FS sítio AD FS

1.  Usando o Gerenciador do servidor instalar a função de serviços do Active Directory federação no Windows Server 2016  

2.  Usando o Assistente de configuração do AD FS, participe do novo servidor Windows Server 2016 ao farm AD FS existente.  Sobre o **boas-vindas** tela click **próxima**.
 ![Participe farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Sobre o **conectar-se aos serviços de domínio do Active Directory** tela, s**especificar uma conta de administrador** com permissões para realizar a configuração de serviços de Federação e clique em **próxima**.
4.  Sobre o **especificar Farm** de tela, insira o nome do servidor SQL e instância e, em seguida, clique em **próxima**.
![Participe farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Sobre o **especificar o certificado SSL** de tela, especifique o certificado e clique em **próxima**.
![Participe farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Sobre o **especificar a conta de serviço** de tela, especifique a conta de serviço e clique em **próxima**. 
7.  Sobre o **revisar opções** de tela, examine as opções e clique em **próxima**. 
8.  Sobre o **pré-requisitos verifica** de tela, certifique-se de que todas as verificações de pré-requisito passaram e clique em **configurar**.
9.  Sobre o **resultados** de tela, certifique-se de que esse servidor foi configurado com êxito e clique em **fechar**.
 
   
#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Remover o servidor Windows Server 2012 R2 AD FS

>[!NOTE]
>Você não precisa configurar o servidor do AD FS principal usando o conjunto AdfsSyncProperties-função usando o SQL como o banco de dados.  Isso ocorre porque todos os nós são considerados principais nesta configuração.

1.  No servidor Windows Server 2012 R2 AD FS em uso do Gerenciador do servidor **remover funções e recursos** em **gerenciar**. 
![Remover servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Sobre o **antes de começar** de tela, clique em **próxima**.
3.  Sobre o **seleção de servidor** tela, clique em **próxima**.
4.  Sobre o **funções de servidor** tela, remover a marca de seleção ao lado de **serviços de Federação do Active Directory** e clique em **próxima**.
![Remover servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Sobre o **recursos** tela, clique em **próxima**.
6.  Sobre o **confirmação** tela, clique em **remover**.
7.  Quando isso for concluída, reinicie o servidor.
     
#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentar o nível de comportamento de Farm (FBL)
Antes desta etapa, você precisa garantir que forestprep e domainprep foram executados no ambiente do Active Directory e Active Directory tem o esquema do Windows Server 2016.  Este documento começou com um controlador de domínio do Windows 2016 e não exigem a executando estes porque eles foram executados quando o anúncio foi instalado.

1. Agora no servidor Windows Server 2016 abrir o PowerShell e execute o seguinte: **$cred = Get-Credential** e pressione enter.
2. Insira credenciais com privilégios de administrador no SQL Server.
3. Agora no PowerShell, digite o seguinte: **AdfsFarmBehaviorLevelRaise Invoke-$cred de credenciais**
2. Quando solicitado, digite **Y**. Isso iniciará a aumentar o nível.  Depois de concluída a FBL aumentaram com êxito.  
![Concluir a atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Agora, acesse AD FS gerenciamento, você verá os novos nós que foram adicionados ao AD FS no Windows Server 2016  
4. Da mesma forma, você pode usar o cmdlt PowerShell: Get-AdfsFarmInformation para mostrar a você o FBL atual.  
![Concluir a atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)
