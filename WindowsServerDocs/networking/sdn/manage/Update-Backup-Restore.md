---
title: Atualizar, fazer backup e restaurar a infraestrutura SDN
description: Neste tópico, você aprenderá a atualizar, fazer backup e restaurar uma infraestrutura de SDN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7f385e094ca70027d1b036bf53af23c1fc4a1bd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406056"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>Atualizar, fazer backup e restaurar a infraestrutura SDN

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, você aprenderá a atualizar, fazer backup e restaurar uma infraestrutura de SDN. 

## <a name="upgrade-the-sdn-infrastructure"></a>Atualizar a infraestrutura de SDN
A infraestrutura de SDN pode ser atualizada do Windows Server 2016 para o Windows Server 2019. Para a ordenação de atualização, siga a mesma sequência de etapas, conforme mencionado na seção "atualizar a infraestrutura de SDN". Antes da atualização, é recomendável fazer um backup do banco de dados do controlador de rede.

Para computadores do controlador de rede, use o cmdlet Get-NetworkControllerNode para verificar o status do nó após a conclusão da atualização. Verifique se o nó retorna ao status "para cima" antes de atualizar os outros nós. Depois de atualizar todos os nós do controlador de rede, o controlador de rede atualiza os microserviços em execução no cluster do controlador de rede dentro de uma hora. Você pode disparar uma atualização imediata usando o cmdlet Update-networkcontroller. 

Instale as mesmas atualizações do Windows em todos os componentes do sistema operacional do sistema de rede definida pelo software (SDN), que inclui:

- Hosts Hyper-V habilitados para SDN
- VMs do controlador de rede
- VMs do software Load Balancer MUX
- VMs de gateway de RAS 

>[!IMPORTANT]
>Se você usar o System Center Virtual Manager, deverá atualizá-lo com os pacotes cumulativos de atualizações mais recentes.

Ao atualizar cada componente, você pode usar qualquer um dos métodos padrão para instalar atualizações do Windows. No entanto, para garantir o tempo de inatividade mínimo para cargas de trabalho e a integridade do banco de dados do controlador de rede, siga estas etapas:

1. Atualize os consoles de gerenciamento.<p>Instale as atualizações em cada um dos computadores em que você usa o módulo do PowerShell do controlador de rede.  Incluindo em qualquer lugar que você tenha a função RSAT-NetworkController instalada por si só. Excluindo as VMs do controlador de rede propriamente ditas; Você os atualiza na próxima etapa.

2. Na primeira VM do controlador de rede, instale todas as atualizações e reinicie o.

3. Antes de prosseguir para a próxima VM do controlador de rede, `get-networkcontrollernode` use o cmdlet para verificar o status do nó que você atualizou e reiniciou.

4. Durante o ciclo de reinicialização, aguarde até que o nó do controlador de rede fique inativo e volte novamente.<p>Após a reinicialização da VM, pode levar vários minutos antes de retornar ao status de **_backup_** . Para obter um exemplo da saída, consulte 

5. Instale atualizações em cada VM MUX do SLB, uma de cada vez, para garantir a disponibilidade contínua da infraestrutura do balanceador de carga.

6. Atualize os hosts e gateways RAS do Hyper-V, começando com os hosts que contêm os gateways RAS que estão no modo de **espera** .<p>As VMs de gateway de RAS não podem ser migradas ao vivo sem perder as conexões de locatário. Durante o ciclo de atualização, você deve ter cuidado para minimizar o número de vezes que o failover de conexões de locatário para um novo gateway de RAS. Ao coordenar a atualização de hosts e gateways de RAS, cada locatário falha ao longo de uma vez, no máximo.

    a. Evacuar o host de VMs que são capazes de migração dinâmica.<p>As VMs de gateway de RAS devem permanecer no host.

    b. Instale atualizações em cada VM de gateway neste host.

    c. Se a atualização exigir que a VM de gateway seja reinicializada, reinicialize a VM.  

    d. Instale atualizações no host que contém a VM de gateway que acabou de ser atualizada.

    e. Reinicialize o host se exigido pelas atualizações.

    f. Repita para cada host adicional que contém um gateway em espera.<p>Se nenhum gateway em espera permanecer, siga estas mesmas etapas para todos os hosts restantes.


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>Exemplo: Usar o cmdlet Get-networkcontrollernode 

Neste exemplo, você vê a saída para a `get-networkcontrollernode` execução do cmdlet de dentro de uma das VMs do controlador de rede.  

O status dos nós que você vê na saída de exemplo é:

- NCNode1.contoso.com = down
- NCNode2.contoso.com = up
- NCNode3.contoso.com = up

>[!IMPORTANT]
>Você deve aguardar vários minutos até que o status do nó seja alterado para _**ativo**_ antes de atualizar qualquer nó adicional, um de cada vez.

Depois de atualizar todos os nós do controlador de rede, o controlador de rede atualiza os microserviços em execução no cluster do controlador de rede dentro de uma hora. 

>[!TIP]
>Você pode disparar uma atualização imediata usando o `update-networkcontroller` cmdlet.


```Powershell
PS C:\> get-networkcontrollernode
Name            : NCNode1.contoso.com
Server          : NCNode1.Contoso.com
FaultDomain     : fd:/NCNode1.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Down

Name            : NCNode2.Contoso.com
Server          : NCNode2.contoso.com
FaultDomain     : fd:/ NCNode2.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up

Name            : NCNode3.Contoso.com
Server          : NCNode3.Contoso.com
FaultDomain     : fd:/ NCNode3.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up
```

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>Exemplo: Usar o cmdlet Update-networkcontroller
Neste exemplo, você vê a saída do `update-networkcontroller` cmdlet para forçar a atualização do controlador de rede. 

>[!IMPORTANT]
>Execute este cmdlet quando não houver mais atualizações a serem instaladas.


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>Fazer backup da infraestrutura de SDN

Os backups regulares do banco de dados do controlador de rede garantem a continuidade dos negócios em caso de desastre ou perda de dados.  O backup das VMs do controlador de rede não é suficiente porque não garante que a sessão continue nos vários nós do controlador de rede.

**Requirement**
* Um compartilhamento SMB e credenciais com permissões de leitura/gravação para o sistema de arquivos e compartilhamento.
* Opcionalmente, você pode usar uma conta de serviço gerenciado de grupo (GMSA) se o controlador de rede tiver sido instalado usando um GMSA também.

**Procedure**

1. Use o método de backup da VM de sua escolha ou use o Hyper-V para exportar uma cópia de cada VM do controlador de rede.<p>Fazer backup da VM do controlador de rede garante que os certificados necessários para descriptografar o banco de dados estejam presentes.  

2. Se estiver usando o System Center Virtual Machine Manager (SCVMM), interrompa o serviço do SCVMM e faça o backup por meio de SQL Server.<p>A meta aqui é garantir que nenhuma atualização seja feita no SCVMM durante esse tempo, o que poderia criar uma inconsistência entre o backup do controlador de rede e o SCVMM.  

   >[!IMPORTANT]
   >Não reinicie o serviço do SCVMM até que o backup do controlador de rede seja concluído.

3. Faça backup do banco de dados do `new-networkcontrollerbackup` controlador de rede com o cmdlet.

4. Verifique a conclusão e o êxito do backup com o `get-networkcontrollerbackup` cmdlet.

5. Se estiver usando o SCVMM, inicie o serviço do SCVMM.



### <a name="example-backing-up-the-network-controller-database"></a>Exemplo: Fazendo backup do banco de dados do controlador de rede

```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

# Get or Create Credential object for File share user

$ShareUserResourceId = "BackupUser"

$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }
If ($ShareCredential -eq $null) {
    $CredentialProperties = New-Object Microsoft.Windows.NetworkController.CredentialProperties
    $CredentialProperties.Type = "usernamePassword"
    $CredentialProperties.UserName = "contoso\alyoung"
    $CredentialProperties.Value = "<Password>"

    $ShareCredential = New-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential -Properties $CredentialProperties -ResourceId $ShareUserResourceId -Force
}

# Create backup

$BackupTime = (get-date).ToString("s").Replace(":", "_")

$BackupProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerBackupProperties
$BackupProperties.BackupPath = "\\fileshare\backups\NetworkController\$BackupTime"
$BackupProperties.Credential = $ShareCredential

$Backup = New-NetworkControllerBackup -ConnectionURI $URI -Credential $Credential -Properties $BackupProperties -ResourceId $BackupTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Exemplo: Verificando o status de uma operação de backup do controlador de rede

```Powershell
PS C:\ > Get-NetworkControllerBackup -ConnectionUri $URI -Credential $Credential -ResourceId $Backup.ResourceId
| ConvertTo-JSON -Depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerBackup/2017-04-25T16_53_13",
    "InstanceId":  "c3ea75ae-2892-4e10-b26c-a2243b755dc8",
    "Etag":  "W/\"0dafea6c-39db-401b-bda5-d2885ded470e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-25T16_53_13",
    "Properties":  {
                    "BackupPath":  "\\\\fileshare\backups\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  "",
                    "FailedResourcesList":  [

                                            ],
                    "SuccessfulResourcesList":  [
                                                    "/networking/v1/credentials/11ebfc10-438c-4a96-a1ee-8a048ce675be",
                                                    "/networking/v1/credentials/41229069-85d4-4352-be85-034d0c5f4658",
                                                    "/networking/v1/credentials/b2a82c93-2583-4a1f-91f8-232b801e11bb",
                                                    "/networking/v1/credentials/BackupUser",
                                                    "/networking/v1/credentials/fd5b1b96-b302-4395-b6cd-ed9703435dd1",
                                                    "/networking/v1/virtualNetworkManager/configuration",
                                                    "/networking/v1/virtualSwitchManager/configuration",
                                                    "/networking/v1/accessControlLists/f8b97a4c-4419-481d-b757-a58483512640",
                                                    "/networking/v1/logicalnetworks/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8",
                                                    "/networking/v1/logicalnetworks/48610528-f40b-4718-938e-99c2be76f1e0",
                                                    "/networking/v1/logicalnetworks/89035b49-1ee3-438a-8d7a-f93cbae40619",
                                                    "/networking/v1/logicalnetworks/a9c8eaa0-519c-4988-acd6-11723e9efae5",
                                                    "/networking/v1/logicalnetworks/d4ea002c-c926-4c57-a178-461d5768c31f",
                                                    "/networking/v1/macPools/11111111-1111-1111-1111-111111111111",
                                                    "/networking/v1/loadBalancerManager/config",
                                                    "/networking/v1/publicIPAddresses/2c502b2d-b39a-4be1-a85a-55ef6a3a9a1d",
                                                    "/networking/v1/GatewayPools/Default",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8056-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8057-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5910-8056-b4c04f395931",
                                                    "/networking/v1/networkInterfaces/058430d3-af43-4328-a440-56540f41da50",
                                                    "/networking/v1/networkInterfaces/08756090-6d55-4dec-98d5-80c4c5a47db8",
                                                    "/networking/v1/networkInterfaces/2175d74a-aacd-44e2-80d3-03f39ea3bc5d",
                                                    "/networking/v1/networkInterfaces/2400c2c3-2291-4b0b-929c-9bb8da55851a",
                                                    "/networking/v1/networkInterfaces/4c695570-6faa-4e4d-a552-0b36ed3e0962",
                                                    "/networking/v1/networkInterfaces/7e317638-2914-42a8-a2dd-3a6d966028d6",
                                                    "/networking/v1/networkInterfaces/834e3937-f43b-4d3c-88be-d79b04e63bce",
                                                    "/networking/v1/networkInterfaces/9d668fe6-b1c6-48fc-b8b1-b3f98f47d508",
                                                    "/networking/v1/networkInterfaces/ac4650ac-c3ef-4366-96e7-d9488fb661ba",
                                                    "/networking/v1/networkInterfaces/b9f23e35-d79e-495f-a1c9-fa626b85ae13",
                                                    "/networking/v1/networkInterfaces/fdd929f1-f64f-4463-949a-77b67fe6d048",
                                                    "/networking/v1/virtualServers/15a891ee-7509-4e1d-878d-de0cb4fa35fd",
                                                    "/networking/v1/virtualServers/57416993-b410-44fd-9675-727cd4e98930",
                                                    "/networking/v1/virtualServers/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a",
                                                    "/networking/v1/virtualServers/6c812217-5931-43dc-92a8-1da3238da893",
                                                    "/networking/v1/virtualServers/d78b7fa3-812d-4011-9997-aeb5ded2b431",
                                                    "/networking/v1/virtualServers/d90820a5-635b-4016-9d6f-bf3f1e18971d",
                                                    "/networking/v1/loadBalancerMuxes/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d78b7fa3-812d-4011-9997-aeb5ded2b431_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d90820a5-635b-4016-9d6f-bf3f1e18971d_suffix",
                                                    "/networking/v1/Gateways/15a891ee-7509-4e1d-878d-de0cb4fa35fd_suffix",
                                                    "/networking/v1/Gateways/57416993-b410-44fd-9675-727cd4e98930_suffix",
                                                    "/networking/v1/Gateways/6c812217-5931-43dc-92a8-1da3238da893_suffix",
                                                    "/networking/v1/virtualNetworks/b3dbafb9-2655-433d-b47d-a0e0bbac867a",
                                                    "/networking/v1/virtualNetworks/d705968e-2dc2-48f2-a263-76c7892fb143",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.2",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.3",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.4"
                                                ],
                    "InProgressResourcesList":  [

                                                ],
                    "ProvisioningState":  "Succeeded",
                    "Credential":  {
                                        "Tags":  null,
                                        "ResourceRef":  "/credentials/BackupUser",
                                        "InstanceId":  "00000000-0000-0000-0000-000000000000",
                                        "Etag":  null,
                                        "ResourceMetadata":  null,
                                        "ResourceId":  null,
                                        "Properties":  null
                                    }
                }
}
```

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>Restaurar a infraestrutura de SDN de um backup

Quando você restaura todos os componentes necessários do backup, o ambiente de SDN retorna para um estado operacional.  

>[!IMPORTANT]
>As etapas variam dependendo do número de componentes restaurados.


1. Se necessário, reimplante os hosts Hyper-V e o armazenamento necessário.

2. Se necessário, restaure as VMs do controlador de rede, as VMs do gateway de RAS e as VMs do MUX do backup. 

3. Interrompa o agente de host NC e o agente de host SLB em todos os hosts Hyper-V:

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Interrompa as VMs do gateway de RAS.

5. Interrompa as VMs MUX SLB.

6. Restaure o controlador de rede com `new-networkcontrollerrestore` o cmdlet.

7. Verifique o **ProvisioningState** de restauração para saber quando a restauração foi concluída com êxito.

8. Se estiver usando o SCVMM, restaure o banco de dados do SCVMM usando o backup que foi criado ao mesmo tempo que o backup do controlador de rede.

9. Se você quiser restaurar VMs de carga de trabalho do backup, faça isso agora.

10. Verifique a integridade do sistema com o cmdlet Debug-networkcontrollerconfigurationstate.

```Powershell
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController "https://NC.contoso.com" -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

### <a name="example-restoring-a-network-controller-database"></a>Exemplo: Restaurando um banco de dados do controlador de rede
 
```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

$ShareUserResourceId = "BackupUser"
$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }

$RestoreProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerRestoreProperties
$RestoreProperties.RestorePath = "\\fileshare\backups\NetworkController\2017-04-25T16_53_13"
$RestoreProperties.Credential = $ShareCredential

$RestoreTime = (Get-Date).ToString("s").Replace(":", "_")
New-NetworkControllerRestore -ConnectionURI $URI -Credential $Credential -Properties $RestoreProperties -ResourceId $RestoreTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Exemplo: Verificando o status de uma restauração de banco de dados do controlador de rede

```PowerShell
PS C:\ > get-networkcontrollerrestore -connectionuri $uri -credential $cred -ResourceId $restoreTime | convertto-json -depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerRestore/2017-04-26T15_04_44",
    "InstanceId":  "22edecc8-a613-48ce-a74f-0418789f04f6",
    "Etag":  "W/\"f14f6b84-80a7-4b73-93b5-59a9c4b5d98e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-26T15_04_44",
    "Properties":  {
                    "RestorePath":  "\\\\sa18fs\\sa18n22\\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  null,
                    "FailedResourcesList":  null,
                    "SuccessfulResourcesList":  null,
                    "ProvisioningState":  "Succeeded",
                    "Credential":  null
                }
}
```


Para obter informações sobre as mensagens de estado de configuração que podem aparecer, consulte [solucionar problemas da pilha de rede definida pelo software do Windows Server 2016](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).