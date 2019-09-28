---
title: Migração de cluster entre domínios no Windows Server 2016/2019
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Este artigo descreve como mover um cluster do Windows Server 2019 de um domínio para outro
ms.localizationpriority: medium
ms.openlocfilehash: 68f49795124dedf0655726853a4d865686f6d697
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361412"
---
# <a name="failover-cluster-domain-migration"></a>Migração de domínio de cluster de failover

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece uma visão geral para mover clusters de failover do Windows Server de um domínio para outro.

## <a name="why-migrate-between-domains"></a>Por que migrar entre domínios

Há vários cenários em que é necessário migrar um cluster de um doamin para outro.

- As mesclagens da empresa com o CompanyB e devem mover todos os clusters para o domínio da empresaA
- Os clusters são criados no datacenter principal e enviados para locais remotos
- O cluster foi criado como um cluster de grupo de trabalho e agora precisa fazer parte de um domínio
- O cluster foi criado como um cluster de domínio e agora precisa fazer parte de um grupo de trabalho
- O cluster está sendo movido para uma área da empresa para outra e é um subdomínio diferente

A Microsoft não oferece suporte a administradores que tentem mover recursos de um domínio para outro se a operação subjacente do aplicativo não for suportada. Por exemplo, a Microsoft não oferece suporte a administradores que tentam mover um servidor do Microsoft Exchange de um domínio para outro.

   > [!WARNING]
   > Recomendamos que você execute um backup completo de todo o armazenamento compartilhado no cluster antes de mover o cluster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 e anterior

No Windows Server 2016 e versões anteriores, o Serviço de cluster não tinha a capacidade de mudar de um domínio para outro.  Isso ocorreu devido à maior dependência no Active Directory Domain Services e nos nomes virtuais criados.   

## <a name="options"></a>Opções

Para fazer essa movimentação, há duas opções.

A primeira opção envolve destruir o cluster e recriá-lo no novo domínio.

![Destruir e recompilar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

Como mostra a animação, essa opção é destrutiva com as etapas que estão sendo:

1. Destrua o cluster.
2. Altere a associação de domínio dos nós para o novo domínio.
3. Recrie o cluster como novo no domínio atualizado.  Isso envolveria a recriação de todos os recursos.

A segunda opção é menos destrutiva, mas requer hardware adicional, pois um novo cluster precisaria ser compilado no novo domínio.  Quando o cluster estiver no novo domínio, execute o assistente de migração de cluster para migrar os recursos. Observe que isso não migra dados – você precisará usar outra ferramenta para migrar dados, como o [serviço de migração de armazenamento](../storage/storage-migration-service/overview.md)(quando o suporte ao cluster for adicionado).

![Criar e migrar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

Como mostra a animação, essa opção não é destrutiva, mas requer um hardware diferente ou um nó do cluster existente do que foi removido.

1. Crie um novo cluster no novo domínio enquanto ainda tiver o cluster antigo disponível.
2. Use o [Assistente de migração de cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) para migrar todos os recursos para o novo cluster. Lembrete, isso não copia dados, portanto, precisará ser feito separadamente.
3. Descomissionar ou destruir o cluster antigo.

Em ambas as opções, o novo cluster precisaria ter todos os [aplicativos com reconhecimento de cluster](https://technet.microsoft.com/aa369082(v=vs.90)) instalados, os drivers atualizados e possivelmente os testes para garantir que todos sejam executados corretamente.  Esse é um processo demorado se os dados também precisarem ser movidos.

## <a name="windows-server-2019"></a>Windows Server 2019

No Windows Server 2019, introduzimos recursos de migração de domínio entre clusters.  Agora, os cenários listados acima podem ser facilmente executados e a necessidade de recompilação não é mais necessária.  

Mover um cluster de um domínio é um processo de avanço direto. Para fazer isso, há dois novos commandlets do PowerShell.

**New-ClusterNameAccount** – cria uma conta de nome de cluster em Active Directory **Remove-ClusterNameAccount** – remove as contas de nome de cluster de Active Directory

O processo para fazer isso é alterar o cluster de um domínio para um grupo de trabalho e voltar para o novo domínio.  A necessidade de destruir um cluster, recriar um cluster, instalar aplicativos, etc não é um requisito. Por exemplo, ele ficaria assim:

![Migrar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migrando um cluster para um novo domínio

Nas etapas a seguir, um cluster está sendo movido do domínio Contoso.com para o novo domínio Fabrikam.com.  O nome do cluster é *CLUSCLUS* e com uma função de servidor de arquivos chamada *FS-CLUSCLUS*.

1. Crie uma conta de administrador local com o mesmo nome e senha em todos os servidores no cluster.  Isso pode ser necessário para fazer logon enquanto os servidores estão se movendo entre domínios.
2. Entre no primeiro servidor com uma conta de administrador ou de usuário de domínio que tenha Active Directory permissões para o CNO (objeto de nome de cluster), VCO (objetos de computador virtual), tenha acesso ao cluster e abra o PowerShell.
3. Verifique se todos os recursos de nome de rede de cluster estão em estado offline e execute o comando abaixo.  Esse comando removerá os objetos de Active Directory que o cluster pode ter.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Use Active Directory usuários e computadores para garantir que os objetos de computador CNO e VCO associados a todos os nomes clusterizados tenham sido removidos.

   > [!NOTE]
   > É uma boa ideia parar o Serviço de cluster em todos os servidores do cluster e definir o tipo de inicialização do serviço como manual para que o Serviço de cluster não seja iniciado quando os servidores estiverem sendo reiniciados durante a alteração de domínios.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Altere a associação de domínio dos servidores para um grupo de trabalho, reinicie os servidores, ingresse os servidores no novo domínio e reinicie-os.
6. Depois que os servidores estiverem no novo domínio, entre em um servidor com uma conta de usuário ou administrador de domínio que tenha Active Directory permissões para criar objetos, tenha acesso ao cluster e abra o PowerShell. Inicie o serviço de cluster e defina-o novamente como automático.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Coloque o nome do cluster e todos os outros recursos de nome de rede de cluster para um estado online.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Altere o cluster para que ele faça parte do novo domínio com objetos associados do Active Directory. Para fazer isso, o comando está abaixo e os recursos de nome de rede devem estar em um estado online.  O que esse comando fará é recriar os objetos de nome em Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    OBSERVAÇÃO:  Se você não tiver grupos adicionais com nomes de rede (ou seja, um cluster Hyper-V com apenas máquinas virtuais), a opção de parâmetro-UpgradeVCOs não será necessária.

9. Use Active Directory usuários e computadores para verificar o novo domínio e garantir que os objetos de computador associados foram criados. Se tiverem, coloque os recursos restantes nos grupos online.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problemas conhecidos

Se você estiver usando o novo recurso de testemunha USB, não será possível adicionar o cluster ao novo domínio.  O raciocínio é que o tipo de testemunha de compartilhamento de arquivos deve utilizar o Kerberos para autenticação.  Altere a testemunha para nenhuma antes de adicionar o cluster ao domínio.  Após a conclusão, recrie a testemunha USB.  O erro que você verá é:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

