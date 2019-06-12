---
title: Migração de Cluster de domínio no Windows Server 2016/2019 entre
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Este artigo descreve a movimentação de um cluster do Windows Server 2019 de um domínio para outro
ms.localizationpriority: medium
ms.openlocfilehash: 1054de942e807f00586903683faeaf695ec2f033
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452933"
---
# <a name="failover-cluster-domain-migration"></a>Migração de domínio do Cluster de failover

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece que uma visão geral para o failover do Windows Server movendo clusters de um domínio para outro.

## <a name="why-migrate-between-domains"></a>Por que migrar entre domínios

Há vários cenários em que é necessário migrar um cluster de um doamin para outro.

- EmpresaA mescla com EmpresaB e deve mover todos os clusters no domínio EmpresaA
- Clusters são criados no data center principal e enviados para locais remotos
- Cluster tiver sido criado como um cluster do grupo de trabalho e agora precisa fazer parte de um domínio
- Cluster tiver sido criado como um cluster de domínio e agora precisa fazer parte de um grupo de trabalho
- Cluster está sendo movido para uma área da empresa para outro e é um subdomínio diferente

Microsoft não oferece suporte a administradores que tentam mover recursos de um domínio para outro, se não há suporte para a operação do aplicativo subjacente. Por exemplo, a Microsoft não fornece suporte para os administradores que tentam mover um servidor Microsoft Exchange de um domínio para outro.

   > [!WARNING]
   > É recomendável que você executar um backup completo de todo o armazenamento compartilhado no cluster antes de mover o cluster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 e anterior

No Windows Server 2016 e anteriores, o serviço de Cluster não tinha a capacidade de mover de um domínio para outro.  Isso ocorreu devido à dependência de aumento no Active Directory Domain Services e os nomes virtuais criados.   

## <a name="options"></a>Opções

Para fazer uma movimentação desse tipo, há duas opções.

A primeira opção envolve destruir o cluster e reconstruí-lo no novo domínio.

![Destruir e recriar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

Como mostra a animação, essa opção é destrutiva com as etapas que estão sendo:

1. Destrua o Cluster.
2. Altere a associação de domínio de nós no novo domínio.
3. Recrie o Cluster como novo no domínio atualizado.  Isso envolveria a necessidade de recriar todos os recursos.

A segunda opção é menos destrutiva, mas requer hardware adicional, como um novo cluster precisaria ser compilado no novo domínio.  Depois que o cluster está no novo domínio, execute o Assistente de migração de Cluster para migrar os recursos. Observe que isso não migrar dados – você precisará usar outra ferramenta de migração de dados, tais como [serviço de migração de armazenamento](../storage/storage-migration-service/overview.md)(assim que for adicionado suporte a cluster).

![Criar e migrar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

Como mostra a animação, essa opção não é destrutiva, mas exige um hardware diferente ou um nó do cluster existente que foi removido.

1. Crie um novo clusterin o novo domínio ao mesmo tempo, o cluster antigo disponível.
2. Use o [Assistente de migração de Cluster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) para migrar todos os recursos para o novo cluster. Como lembrete, isso não copia dados, portanto, precisará ser feito separadamente.
3. Encerrar ou destrua o cluster antigo.

Em ambas as opções, o novo cluster precisaria ter todos os [aplicativos com reconhecimento de cluster](https://technet.microsoft.com/aa369082(v=vs.90)) instalado drivers atualizadas, e possivelmente testes para garantir que tudo serão executado corretamente.  Isso é um processo demorado se também precisam de dados a ser movido.

## <a name="windows-server-2019"></a>Windows Server 2019

2019 do Windows Server, apresentamos os recursos de migração de domínio cruzado do cluster.  Portanto, agora, os cenários listados acima podem ser feitos facilmente e a necessidade de recriação não for mais necessário.  

Movendo um cluster de um domínio é um processo simples. Para fazer isso, há dois novos cmdlets do PowerShell.

**Novo ClusterNameAccount** – cria uma conta de nome de Cluster no Active Directory **ClusterNameAccount remover** – remove as contas de nome de Cluster do Active Directory

O processo de fazer isso é alterar o cluster de um domínio para um grupo de trabalho e de volta para o novo domínio.  A necessidade de destruir um cluster, recriar um cluster, instale aplicativos, etc não é um requisito. Por exemplo, ela teria esta aparência:

![Migrar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migrar um cluster para um novo domínio

Nas etapas a seguir, um cluster está sendo movido do domínio Contoso.com para o novo domínio Fabrikam.com.  É o nome do cluster *CLUSCLUS* e com uma função de servidor de arquivos chamado *FS CLUSCLUS*.

1. Crie uma conta de administrador local com o mesmo nome e senha em todos os servidores no cluster.  Isso pode ser necessário para fazer logon enquanto os servidores estão se movendo entre domínios.
2. Entrar para o primeiro servidor com uma conta de usuário ou administrador de domínio que tenha permissões do Active Directory para o Cluster nome CNO (objeto), objetos de computador Virtual (VCO) tem acesso ao Cluster e abra o PowerShell.
3. Verifique se todos os recursos de nome de rede do Cluster estão em um Offline, estado e execute o comando abaixo.  Este comando removerá os objetos do Active Directory que o cluster pode ter.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Use o Active Directory Users and Computers para garantir que o computador CNO e VCO objetos associados a todos os nomes de cluster foram removidos.

   > [!NOTE]
   > É uma boa ideia parar o serviço de Cluster em todos os servidores no cluster e definir o tipo de inicialização do serviço como Manual para que o serviço de Cluster não foi iniciada quando os servidores estão reiniciando durante a alteração de domínios.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Alterar a associação de domínio dos servidores para um grupo de trabalho, reinicie os servidores, ingressar nos servidores para o novo domínio e reiniciar novamente.
6. Depois que os servidores estão no novo domínio, entre em um servidor com uma conta de usuário ou administrador de domínio que tem permissões do Active Directory para criar objetos, tem acesso ao Cluster e abra o PowerShell. Inicie o serviço de Cluster e defini-lo de volta para automático.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Coloque o nome do Cluster e todos os outro cluster recursos de nome de rede para um estado Online.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Altere o cluster para ser uma parte do novo domínio com objetos do active directory associado. Para fazer isso, o comando está abaixo e os recursos de nome de rede devem estar no estado online.  O que esse comando fará é recriar os objetos de nome no Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    OBSERVAÇÃO: Se você não tiver todos os grupos adicionais com nomes de rede (ou seja, um Cluster Hyper-V com apenas as máquinas virtuais), a opção de parâmetro de UpgradeVCOs - não é necessária.

9. Use o Active Directory Users and Computers para verificar o novo domínio e verifique se que os objetos de computador associados foram criados. Se eles tiverem, em seguida, coloque os recursos restantes nos grupos online.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problemas conhecidos

Se você estiver usando o novo recurso de testemunha USB, não será possível adicionar o cluster para o novo domínio.  O raciocínio é que o tipo de testemunha de compartilhamento de arquivo deve utilizar o Kerberos para autenticação.  Altere a testemunha para nenhum antes de adicionar o cluster para o domínio.  Depois de concluído, recrie a testemunha USB.  É o erro que será exibida:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

