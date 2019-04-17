---
title: "Infraestrutura de rede definida Update, o Backup e restauração do Software"
description: "Este tópico faz parte do Software de rede definidos guia sobre como a atualização, Backup e restauração SDN infraestrutura no Windows Server 2016."
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: grcusanz
ms.openlocfilehash: bb7194ec865db980962853b87d68a84a5446269e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="update-backup-and-restore-software-defined-networking-infrastructure"></a>Infraestrutura de rede definida Update, o Backup e restauração do Software

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico contém as seções a seguir.

- [Atualizando a infraestrutura SDN](#bkmk_Updating)
- [A infraestrutura SDN backup](#bkmk_backup)
- [Restaurar a infraestrutura SDN de um backup](#bkmk_restore)

## <a name="bkmk_Updating"></a>Atualizando a infraestrutura SDN

A atualização é o processo de instalação de atualizações do Windows em todos os componentes do sistema operacional do sistema de rede definidos Software (SDN).  Isso inclui o SDN habilitado hosts, VMs do controlador de rede, Software Load balanceador multiplexador VMs e RAS Gateway VMs do Hyper-V.  É fundamental que todos esses componentes têm exatamente o mesmo conjunto de atualizações instaladas.  Se for usado o System Center Virtual Machine Manager também é recomendável que você também atualizá-lo com as últimas atualizar pacotes cumulativos também.

Atualização de cada componente é executada usando qualquer um dos métodos padrão para instalar as atualizações do windows, mas as etapas descritas abaixo devem ser seguidas para garantir um mínimo de tempo de inatividade para cargas de trabalho e para garantir a integridade do banco de dados do controlador de rede.

### <a name="step-1-update-the-management-consoles"></a>Etapa 1: Atualizar os consoles de gerenciamento
Instale as atualizações necessárias em cada um dos computadores em que você usa o módulo Powershell de controlador de rede.  Isso inclui qualquer lugar que você tenha a função de NetworkController RSAT instalada por si só.  Isso não não incluindo as VMs do controlador de rede propriamente ditos como eles serão atualizados na etapa 2.

### <a name="step-2-update-the-network-controllers"></a>Etapa 2: Atualizar os controladores de rede
Isso é a etapa mais importante no ciclo de atualização, já que cada VM do controlador de rede deve ser atualizada e ser totalmente online no cluster controlador de rede antes de prosseguir para o próximo.

Inicie com uma VM do controlador de rede e instale todas as atualizações necessárias.  Se necessário, reinicie a VM.

Antes de prosseguir para a próxima VM do controlador de rede usar get-networkcontrollernode para verificar o status do nó que foi atualizado e reinicializado.  Aguarde até o nó de controlador de rede ficar inativo durante o ciclo de reinicialização e, em seguida, volte para cima novamente.  Após a VM reinicialização, ainda pode levar vários minutos para que ele voltar ao estado pressionado.

#### <a name="example-using-get-networkcontrollernode-to-check-the-status-of-network-controller-nodes"></a>Exemplo: Usando get-networkcontrollernode para verificar o status de nós de controlador de rede

Este exemplo mostra a saída da execução get-networkcontrollernode de dentro de uma das VMs do controlador de rede.  Ele mostra que NCNode1.contoso.com está inativo enquanto os outros dois nós são íntegros.  Você deve esperar para cima para vários minutos até o status para o nó muda para backup antes de prosseguir com a atualização de nós adicionais.

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

Somente depois que todos os nós de controlador de rede estão no estado Up pode você repetir essas etapas para cada nó de controlador de rede adicional.  Continue atualizando cada nó um por vez.

Depois que todos os nós de controlador de rede são atualizados, o controlador de rede atualizará os microsserviços em execução dentro do cluster de controlador de rede dentro de uma hora.  Você pode disparar uma atualização imediata usando o cmdlet de atualização networkcontroller.

#### <a name="example-using-update-networkcontroller-to-force-network-controller-to-update"></a>Exemplo: Usando a atualização networkcontroller para forçar o controlador de rede para atualizar

Esse comando mostra o resultado da atualização networkcontroller quando não houver atualizações restante para ser instalado.

```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

### <a name="step-3-update-slb-muxes"></a>Etapa 3: Atualização SLB Muxes

Instale atualizações em cada VM de multiplexador SLB um por vez para garantir a disponibilidade contínua da infraestrutura balanceador load.

### <a name="step-4-update-hyper-v-hosts-and-ras-gateways"></a>Etapa 4: Atualização Hyper-V Hosts e RAS Gateways

Porque RAS Gateway VMs não podem ser dinâmicas migrados sem perder as conexões de locatário, tome cuidado para minimizar o número de vezes que locatário conexões serão failover para um novo gateways RAS durante o ciclo de atualização.  Por coordenar atualização dos hosts e gateways RAS cada locatário será apenas failover no máximo uma vez.  

Siga estas etapas para cada host, começando com os hosts que contêm os Gateways RAS que estão no modo de espera:

1.  Remover o host de VMs que são capazes de migração em tempo real.  VMs do Gateway RAS deve permanecer no host.
2.  Instale as atualizações em cada VM Gateway nesse host.
3.  Se a atualização requer o gateway VM reinicializar reinicialize a VM.  
4.  Instale atualizações no host que contém o gateway VM que apenas foi atualizada.
5.  Reinicialize o host se exigido pelas atualizações.
6.  Repita para cada host adicional que contém um gateway no modo de espera.  Se nenhum gateways espera permanecerem, siga as mesmas etapas para todos os hosts restantes.

## <a name="bkmk_backup"></a>A infraestrutura SDN backup

Backups regulares do banco de dados do controlador de rede são fundamentais para garantir a continuidade de negócios no caso de uma desastre ou perda de dados.  Fazer backup de VMs do controlador de rede é insuficiente, porque ela não garante que quorum é mantido em vários nós de controlador de rede.
Requisitos:
 * Um compartilhamento SMB e credenciais com permissões de leitura/gravação para o compartilhamento e sistema de arquivos.
 * Opcionalmente, você pode usar uma conta de serviço gerenciado do grupo (GMSA) se o controlador de rede foi instalado usando um GMSA também.

Siga estas etapas para executar um backup:

1. Backup VMs do controlador de rede usando o método de backup de VM de sua preferência, ou use o Hyper-V para exportar uma cópia de cada VM do controlador de rede.  Isso garante que, se uma recompilação completa, incluindo a restauração da infraestrutura VMs for executada, os certificados necessários para descriptografar o banco de dados estão presentes.
2. Se você estiver usando o sistema Center Virtual Machine Manager (SCVMM), interrompa o serviço SCVMM e faça backup por meio do SQL Server para garantir que nenhuma atualização é feitas em SCVMM durante esse período que poderia criar uma inconsistência entre o backup do controlador de rede e SCVMM.  Não reinicie o serviço SCVMM até que o backup do controlador de rede seja concluído.
3. Backup o banco de dados do controlador de rede usando o novo networkcontrollerbackup.

 #### <a name="example-backing-up-the-network-controller-database"></a>Exemplo: Backup do banco de dados do controlador de rede
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

4. Use get-networkcontrollerbackup para verificar se há sucesso do backup e a conclusão.

 #### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Exemplo: Verificar o status de uma operação de backup do controlador de rede

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

5.  Se usando SCVMM agora você pode começar a serviço SCVMM.

## <a name="bkmk_restore"></a>Restaurar a infraestrutura SDN de um backup

A restauração é o processo de restauração todos os componentes necessários de backup para retornar um ambiente SDN para um estado operacional.  As etapas um pouco varia dependendo da quantidade de componentes que estão sendo restaurados.

1. Se necessário, reimplante hosts Hyper-V e o armazenamento necessários.

2. Se necessário, restaure as VMs do controlador de rede, VMs do Gateway RAS e multiplexador VMs de backup. 

3. Interrompa o agente de Host do NC e SLB Host Agent em todos os hosts Hyper-V

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Parar RAS Gateway VMs

5. Parar SLB multiplexador VMs

6. Restaure o controlador de rede usando o cmdlet new-networkcontrollerrestore.

 #### <a name="example-restoring-a-network-controller-database"></a>Exemplo: Restauração de um banco de dados do controlador de rede
 
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

7. Verifique a restauração ProvisioningState saber quando a restauração tinha concluída com êxito.

 #### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Exemplo: Verificando o status de uma restauração de banco de dados do controlador de rede

    ```
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

8. Se usar SCVMM, restaure o banco de dados SCVMM usando o backup que foi criado ao mesmo tempo em que o backup do controlador de rede.

9. Se a carga de trabalho VMs estiverem sendo restaurada de backup, você pode fazer isso agora.

10. Use o cmdlet networkcontrollerconfigurationstate de depuração para verificar a integridade do seu sistema.

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

Para obter informações sobre as mensagens de estado de configuração que podem aparecer, consulte [solucionar problemas do Windows Server 2016 Software definido de rede pilha](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).