---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Atualizar para o AD FS no Windows Server 2016
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ce07398a2d624a1e9b004cd35eb9228d59dc2b5b
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2018
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Atualizar para o AD FS no Windows Server 2016 usando um banco de dados de trabalho

>Aplica-se a: Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Mudando de um farm de Windows Server 2012 R2 AD FS para um farm AD FS do Windows Server 2016  
Este documento descreve como atualizar seu farm do AD FS Windows Server 2012 R2 para AD FS no Windows Server 2016 quando você estiver usando um banco de dados de trabalho.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Atualização do AD FS para o Windows Server 2016 FBL  
Novo no AD FS para o Windows Server 2016 é o recurso de nível do comportamento de farm (FBL).   Esse recurso é farm ampla e determina os recursos que pode usar o farm AD FS.   Por padrão, o FBL em um farm de Windows Server 2012 R2 AD FS está no Windows Server 2012 R2 FBL.  

Um servidor Windows Server 2016 AD FS pode ser adicionado a um farm de Windows Server 2012 R2 e ele funcionará na mesma FBL como um Windows Server 2012 R2.  Quando você tiver um servidor Windows Server 2016 AD FS operacional dessa maneira, seu farm diz "mixed".  No entanto, você não poderá tirar proveito dos novos recursos do Windows Server 2016 até que o FBL é gerado para o Windows Server 2016.  Com um farm misto:  

-   Os administradores podem adicionar novas, servidores de Federação do Windows Server 2016 a um farm existente do Windows Server 2012 R2.  Como resultado, o farm estiver no "modo misto" e opera o nível de comportamento de farm do Windows Server 2012 R2.  Para garantir um comportamento consistente no farm, novos recursos do Windows Server 2016 não podem ser configurados ou usados nesse modo.  

-   Depois que todos os servidores de Federação do Windows Server 2012 R2 foram removidos da Fazenda modo misto e, no caso de um farm de trabalho, um dos novos servidores de Federação do Windows Server 2016 foi promovido para a função de nó principal, o administrador pode, em seguida, gere FBL do Windows Server 2012 R2 para o Windows Server 2016.  Como resultado, os novos recursos de 2016 do AD FS Windows Server podem ser configurados e usados.  

-   Como resultado do recurso farm misto, AD FS Windows Server 2012 R2 organizações pensando em atualizar para o Windows Server 2016 não precisará implantar um farm totalmente novo, exportar e importar dados de configuração.  Em vez disso, eles podem adicionar nós de Windows Server 2016 a um farm existente enquanto ele estiver online e gerar somente o tempo de inatividade relativamente breve envolvidas na eleve FBL.  

Lembre-se de que no modo misto farm, sítio AD FS não é capaz de quaisquer novos recursos ou funcionalidade introduzida no AD FS no Windows Server 2016.  Isso significa que as organizações que desejam experimentar novos recursos não podem fazer isso até que o FBL é acionado.  Então se sua organização estiver buscando para testar os novos recursos antes da rasing o FBL, você precisará implantar um farm separado para fazer isso.  

O restante do documento fornece as etapas para adicionar um servidor de Federação do Windows Server 2016 para um ambiente do Windows Server 2012 R2 e, em seguida, acionar o FBL para Windows Server 2016.  Essas etapas foram executadas em um ambiente de teste descrito no diagrama da arquitetura abaixo.  

> [!NOTE]  
> Antes de você pode mover para o AD FS no Windows Server 2016 FBL, você deve remover todos os nós do Windows 2012 R2.  Você não pode atualizar um sistema de operacional do Windows Server 2012 R2 para o Windows Server 2016 e fazer com que ele se tornar um nó de 2016.  Você precisará removê-lo e substituí-lo com um novo nó de 2016.
>
> Atualizar o FBL ao usar SQL para armazenar a configuração do AD FS criar um novo banco de dados do gerenciamento "AdfsConfigurationV3".

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2016-farm-behavior-level"></a>Para atualizar seu farm AD FS para o Windows Server 2016 Farm comportamento nível  

1.  Usando o Gerenciador do servidor instalar a função de serviços do Active Directory federação no Windows Server 2016  

2.  Usando o Assistente de configuração do AD FS, participe do novo servidor Windows Server 2016 ao farm AD FS existente.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  No servidor de Federação do Windows Server 2016, abra o gerenciamento do AD FS.    Observe que nada é exibido para cima, como este servidor de Federação não é o servidor primário.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  Quando a associação for concluída, no servidor Windows Server 2016, abra o PowerShell e execute o seguinte cmdlt: conjunto AdfsSyncProperties-PrimaryComputer de função  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  No servidor original do AD FS Windows Server 2012 R2, abra o PowerShell e execute o seguinte cmdlt: conjunto AdfsSyncProperties-função SecondaryComputer - PrimaryComputerName {FQDN}  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  No seu Proxy de aplicativo Web abrir o PowerShell e execute o cmdlt followoing: Install-WebApplicationProxy - CertificateThumbprint {SSLCert} - fsname fsname - TrustCred $trustcred  

7.  Agora no servidor de Federação do Windows Server 2016 abra o gerenciamento do AD FS.  Observe que, agora todos os nós aparecem porque a função principal foi transferida para esse servidor.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  Com a mídia de instalação do Windows Server 2016, abra um prompt de comando e navegue até o diretório support\adprep.  Execute o seguinte: adprep /forestprep.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

9. Após a conclusão da que executar adprep/domainprep  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

10. Agora no servidor Windows Server 2016 abrir o PowerShell e execute o seguinte cmdlt: AdfsFarmBehaviorLevelRaise invocar  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

11. Quando solicitado, digite Y. Isso iniciará a aumentar o nível.  Depois de concluída a FBL aumentaram com êxito.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

> [!NOTE]  
> Se os servidores do AD FS usarem SQL para a configuração, um novo banco de dados manamgement agora foi criado com o nome "AdfsConfiguraionV3". 

12. Agora, acesse AD FS gerenciamento, você verá os novos nós que foram adicionados ao AD FS no Windows Server 2016  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. Da mesma forma, você pode usar o cmdlt PowerShell: Get-AdfsFarmInformation para mostrar a você o FBL atual.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
