---
title: Cruzar a migração de domínio de Cluster no Windows Server 2016/2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Este artigo descreve a migração de um cluster do Windows Server 2019 de um domínio para outro
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 21677706eb85cb1396c1f40bf443146c09ef1b0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/31/2019
ms.locfileid: "9042030"
---
# Migração de domínio de Cluster de failover

> Aplicável a: Windows Server 2019, Windows Server 2016

Este tópico fornece que uma visão geral para se movendo failover do Windows Server clusters de um domínio para outro.

## Por que migrar entre domínios

Há vários cenários em que é necessário migrar um cluster de um doamin para outro.

- EmpresaA mescla com EmpresaB e deve mover todos os clusters no domínio EmpresaA
- Clusters são criados no principal datacenter e enviados para locais remotos
- Cluster foi criado como um cluster de grupo de trabalho e agora deve ser parte de um domínio
- Cluster foi criado como um cluster de domínio e agora precisa fazer parte de um grupo de trabalho
- Cluster está sendo movido para uma área da empresa para outro e é um subdomínio diferente

Microsoft não oferece suporte aos administradores que tentam mover recursos de um domínio para outro, se a operação de aplicativo subjacente está sem suporte. Por exemplo, a Microsoft não fornece suporte para os administradores que tentam mover um Microsoft Exchange server de um domínio para outro.

   > [!WARNING]
   > Recomendamos que você execute um backup completo de armazenamento todos compartilhado do cluster antes de mover o cluster.

## Windows Server 2016 e versões anterior

No Windows Server 2016 e versões anteriores, o serviço de Cluster não têm a capacidade de migração de um domínio para outro.  Isso foi devido a dependência maior em serviços de domínio do Active Directory e os nomes virtuais criados.   

## Opções

Para fazer essas mudanças, há duas opções.

A primeira opção envolve destruir o cluster e recriando-lo no novo domínio.

![Destruir e recriar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

Conforme mostra a animação, essa opção é destrutiva com as etapas que está sendo:

1. Destrua o Cluster.
2. Altere a participação no domínio de nós para o novo domínio.
3. Recrie o Cluster como novo no domínio atualizado.  Isso seria envolvem a necessidade de recriar todos os recursos.

A segunda opção é menos destrutiva, mas exige hardware adicional, como um novo cluster precisaria ser criado no novo domínio.  Depois que o cluster é no novo domínio, execute o Assistente de migração de Cluster para migrar os recursos. Observe que isso não migrar dados - você precisará usar outra ferramenta de migração de dados, como o [Serviço de migração de armazenamento](../storage/storage-migration-service/overview.md)(depois de adicionado suporte a cluster).

![Compilar e migrar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

Conforme mostra a animação, essa opção não é destrutiva, mas exige um hardware diferente ou um nó do cluster existente que foi removido.

1. Crie um novo clusterin o novo domínio ao mesmo tempo, o cluster antigo disponível.
2. Use o [Assistente de migração de Cluster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) para migrar todos os recursos para o novo cluster. Lembrete, isso não copiar dados, portanto, precisará ser feito separadamente.
3. Encerrar ou destruir o cluster antigo.

Em ambas as opções, o novo cluster seria necessário ter todos os [aplicativos com reconhecimento de cluster](https://technet.microsoft.com/aa369082(v=vs.90)) instalados, drivers atualizadas, e possivelmente testes para garantir que todos os serão executado corretamente.  Isso é um processo demorado se dados também precisam ser movido.

## Windows Server 2019

No Windows Server 2019, apresentamos cluster entre recursos de migração de domínio.  Agora, os cenários listados acima podem ser feitos facilmente e a necessidade de recriar não é mais necessária.  

Mover um cluster de um domínio é um processo simples. Para fazer isso, há dois novos cmdlets do PowerShell.

**New-ClusterNameAccount** – cria uma conta de nome do Cluster no Active Directory **Remove-ClusterNameAccount** – remove as contas de nome do Cluster do Active Directory

O processo para fazer isso é alterar o cluster de um domínio para um grupo de trabalho e voltar para o novo domínio.  A necessidade de destruir um cluster, recriar um cluster, instalar aplicativos, etc não é um requisito. Por exemplo, ele teria esta aparência:

![Migrar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## Migrar um cluster para um novo domínio

Nas etapas a seguir, um cluster está sendo movido do domínio Contoso.com para o novo domínio Fabrikam.com.  O nome do cluster é *CLUSCLUS* e com uma função de servidor de arquivo chamado *CLUSCLUS FS*.

1. Crie uma conta de administrador local com o mesmo nome e senha em todos os servidores no cluster.  Isso pode ser necessário para fazer logon enquanto os servidores que estão mudando entre domínios.
2. Login para o primeiro servidor com uma conta de usuário ou administrador de domínio que tenha permissões do Active Directory para o Cluster nome do objeto (CNO), objetos de computador Virtual (VCO) tem acesso ao Cluster e PowerShell aberto.
3. Certifique-se todos os recursos de nome de rede do Cluster em um Offline estado e execute o abaixo comando.  Esse comando removerá os objetos do Active Directory que o cluster pode ter.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Use o Active Directory Users and Computers para garantir que o computador CNO e VCO objetos associados a todos os nomes de clusters foram removidos.

   > [!NOTE]
   > É uma boa ideia para parar o serviço de Cluster em todos os servidores no cluster e definir o tipo de inicialização do serviço manual para que o serviço de Cluster não for iniciado quando os servidores são reiniciar durante a alteração de domínios.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Alterar a associação de domínio dos servidores em um grupo de trabalho, reinicie os servidores, ingresse os servidores do novo domínio e reinicie novamente.
6. Depois que os servidores estiverem no novo domínio, entre um servidor com uma conta de usuário ou administrador de domínio que tem permissões do Active Directory para criar objetos, tem acesso ao Cluster e abra o PowerShell. Inicie o serviço de Cluster e defini-lo de volta para automático.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Coloque o nome do Cluster e outro cluster todos os recursos de nome de rede para um estado Online.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Altere o cluster para ser uma parte do novo domínio com objetos associados do active directory. Para fazer isso, o comando está abaixo e os recursos de nome de rede devem estar em um estado online.  O que fará esse comando é recriar os objetos de nome no Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Observação: Se você não tiver qualquer grupos adicionais com nomes de rede (ou seja, um Cluster Hyper-V com apenas das máquinas virtuais), a opção de parâmetro - UpgradeVCOs não é necessário.

9. Use o Active Directory Users and Computers para verificar o novo domínio e garantir que os objetos de computador associado foram criados. Se eles tiverem, em seguida, coloque os recursos restantes nos grupos on-line.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## Problemas conhecidos

Se você estiver usando o novo recurso de testemunha USB, não será possível adicionar o cluster para o novo domínio.  O raciocínio é que o tipo de testemunha de compartilhamento de arquivo deve utilizar o Kerberos para autenticação.  Altere a testemunha none antes de adicionar o cluster no domínio.  Depois de concluído, recrie a testemunha USB.  O erro que você verá é:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

