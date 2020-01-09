---
title: Velocidade de transferência de arquivos SMB lentos
description: Apresenta como solucionar problemas de desempenho de transferência de arquivos SMB.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 0e6c049404f464eba872075a8ef5060b303920c8
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654557"
---
# <a name="slow-smb-files-transfer-speed"></a>Velocidade de transferência de arquivos SMB lentos

Este artigo fornece procedimentos de solução de problemas sugeridos para velocidades de transferência de arquivos lentas por meio de SMB.

## <a name="large-file-transfer-is-slow"></a>A transferência de arquivo grande está lenta

Se você observar transferências lentas de arquivos grandes, considere as seguintes etapas:

- Experimente o comando de cópia de arquivo para e/s não armazenada em buffer (**xcopy/j** ou **Robocopy/j**).

- Teste a velocidade de armazenamento. Isso ocorre porque as velocidades de cópia de arquivo são limitadas pela velocidade de armazenamento.

- As cópias de arquivos às vezes começam rapidamente e ficam lentas. Siga estas diretrizes para verificar essa situação:
    
  - Isso geralmente ocorre quando a cópia inicial é armazenada em cache ou armazenado em buffer (na memória ou no cache de memória do controlador RAID) e o cache é executado. Isso força os dados a serem gravados diretamente no disco (write-through). Esse é um processo mais lento.
    
  - Use os contadores do monitor de desempenho de armazenamento para determinar se o desempenho do armazenamento é prejudicado ao longo do tempo. Para obter mais informações, consulte [ajuste de desempenho para servidores de arquivos SMB](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/file-server/smb-file-server).

- Use o RAMMap (SysInternals) para determinar se o uso de "arquivo mapeado" na memória pára de crescer devido a esgotamento de memória livre.

- Procure a perda de pacotes no rastreamento. Isso pode causar limitação pelo provedor de congestionamento de TCP.

- Para SMBv3 e versões posteriores, certifique-se de que o SMB multicanal está habilitado e funcionando.

- No cliente SMB, habilite o MTU grande no SMB e desabilite a limitação da largura de banda. Para fazer isso, execute o comando a seguir:  
  
  ```PowerShell
  Set-SmbClientConfiguration -EnableBandwidthThrottling 0 -EnableLargeMtu 1
  ```

## <a name="small-file-transfer-is-slow"></a>A transferência de arquivo pequena está lenta

A transferência lenta de arquivos pequenos por meio de SMB ocorre com mais frequência se houver muitos arquivos. Este é um comportamento esperado.

Durante a transferência de arquivos, a criação de arquivos causa sobrecarga de protocolo alto e sobrecarga do sistema de arquivos alta. Para transferências de arquivos grandes, esses custos ocorrem apenas uma vez. Quando um grande número de arquivos pequenos for transferido, o custo será repetitivo e causará transferências lentas.

Veja a seguir os detalhes técnicos sobre esse problema:

- O SMB chama um comando Create para solicitar que o arquivo seja criado. Algum código verificará se o arquivo existe e, em seguida, criará o arquivo. Ou alguma variação do comando Create cria o arquivo real.

- Cada comando Create gera atividade no sistema de arquivos.

- Depois que os dados são gravados, o arquivo é fechado.

- Todos os tempos, o processo sofre de latência de rede e latência de servidor SMB. Isso ocorre porque a solicitação SMB é primeiro convertida para um comando do sistema de arquivos e, em seguida, para a latência real do sistema de arquivos para concluir a operação.

- Se algum programa antivírus estiver em execução, a transferência diminuirá ainda mais. Isso ocorre porque os dados geralmente são verificados uma vez pelo farejador de pacotes e uma segunda vez quando eles são gravados no disco. Em alguns cenários, essas ações são repetidas milhares de tempo. Você pode observar velocidades de menos de 1 MB/s.

## <a name="opening-office-documents-is-slow"></a>A abertura de documentos do Office está lenta

Esse problema geralmente ocorre em uma conexão WAN. Isso é comum e normalmente é causado pela maneira como os aplicativos do Office (Microsoft Excel, em particular) acessam e lêem dados.

É recomendável que você verifique se os binários do Office e do SMB estão atualizados e, em seguida, teste ao conceder o leasing desabilitado no servidor SMB. Para fazer isso, execute estas etapas:
   
1. Execute o seguinte comando do PowerShell no Windows 8 e no Windows Server 2012 ou versões posteriores do Windows:
      
   ```PowerShell
   Set-SmbServerConfiguration -EnableLeasing $false  
   ```
      
   Ou execute o seguinte comando em uma janela de prompt de comandos com privilégios elevados:  

   ```cmd
   REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v DisableLeasing /t REG\_DWORD /d 1 /f  
   ```
      
   > [!NOTE]
   > Depois de definir essa chave do registro, as concessões SMB2 não são mais concedidas, mas oplocks ainda estão disponíveis. Essa configuração é usada principalmente para solução de problemas.
    
2. Reinicie o servidor de arquivos ou reinicie o serviço do **servidor** . Para reiniciar o serviço, execute os seguintes comandos:

   ```cmd  
   NET STOP SERVER 
   NET START SERVER
   ```

Para evitar esse problema, você também pode replicar o arquivo para um servidor de arquivos local. Para obter mais informações, consulte [documentos do AVING Office para um servidor de rede é lento ao usar o EFS](https://docs.microsoft.com/office/troubleshoot/office/saving-file-to-network-server-slow).
