---
title: Melhorias de segurança do SMB
description: Uma explicação do recurso de criptografia SMB no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7221d3ea94ff9f2d7fca8e95cee66597e2dc6270
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402067"
---
# <a name="smb-security-enhancements"></a>Melhorias de segurança do SMB

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tópico explica os aprimoramentos de segurança SMB no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2016.

## <a name="smb-encryption"></a>Criptografia SMB

A criptografia SMB fornece criptografia de ponta a ponta de dados SMB e protege dados contra ocorrências de espionagem em redes não confiáveis. Você pode implantar a criptografia SMB com esforço mínimo, mas pode exigir pequenos custos adicionais para hardware ou software especializado. Ele não tem requisitos de IPsec (Internet Protocol Security) ou de WAN Accelerators. A criptografia SMB pode ser configurada por compartilhamento ou para todo o servidor de arquivos, e pode ser habilitada para uma variedade de cenários em que os dados atravessam redes não confiáveis.

>[!NOTE]
>A criptografia SMB não abrange a segurança em repouso, que normalmente é manipulada pelo Criptografia de Unidade de Disco BitLocker.

A criptografia SMB deve ser considerada para qualquer cenário no qual os dados confidenciais precisem ser protegidos contra ataques man-in-the-Middle. Os cenários possíveis incluem:

- Os dados confidenciais de um operador de informações são movidos usando o protocolo SMB. A criptografia SMB oferece uma garantia de integridade e privacidade de ponta a ponta entre o servidor de arquivos e o cliente, independentemente das redes atravessadas, como conexões de WAN (rede de longa distância) mantidas por provedores que não são da Microsoft.
- O SMB 3,0 permite que os servidores de arquivos forneçam armazenamento continuamente disponível para aplicativos de servidor, como o SQL Server ou o Hyper-V. A habilitação da criptografia SMB oferece uma oportunidade de proteger essas informações contra ataques de espionagem. A criptografia SMB é mais simples de usar do que as soluções de hardware dedicadas que são necessárias para a maioria das SANs (redes de área de armazenamento).

>[!IMPORTANT]
>Você deve observar que há um custo operacional de desempenho notável com qualquer proteção de criptografia de ponta a ponta em comparação com o não criptografado.

## <a name="enable-smb-encryption"></a>Habilitar criptografia SMB

Você pode habilitar a criptografia SMB para todo o servidor de arquivos ou apenas para compartilhamentos de arquivos específicos. Use um dos procedimentos a seguir para habilitar a criptografia SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitar a criptografia SMB com o Windows PowerShell

1. Para habilitar a criptografia SMB para um compartilhamento de arquivos individual, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar a criptografia SMB para todo o servidor de arquivos, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para criar um novo compartilhamento de arquivos SMB com a criptografia SMB habilitada, digite o seguinte script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar a criptografia SMB com Gerenciador do Servidor

1. No Gerenciador do Servidor, abra **serviços de arquivo e armazenamento**.
2. Selecione **compartilhamentos** para abrir a página de gerenciamento de compartilhamentos.
3. Clique com o botão direito do mouse no compartilhamento no qual você deseja habilitar a criptografia SMB e selecione **Propriedades**.
4. Na página **configurações** do compartilhamento, selecione **criptografar acesso a dados**. O acesso ao arquivo remoto para esse compartilhamento é criptografado.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerações sobre a implantação de criptografia SMB

Por padrão, quando a criptografia SMB está habilitada para um compartilhamento de arquivos ou servidor, somente clientes SMB 3,0 têm permissão para acessar os compartilhamentos de arquivos especificados. Isso impõe a intenção do administrador de proteger os dados para todos os clientes que acessam os compartilhamentos. No entanto, em algumas circunstâncias, um administrador pode querer permitir o acesso não criptografado para clientes que não dão suporte a SMB 3,0 (por exemplo, durante um período de transição quando versões mistas do sistema operacional do cliente estão sendo usadas). Para permitir o acesso não criptografado para clientes que não dão suporte a SMB 3,0, digite o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

O recurso de negociação de dialeto seguro descrito na próxima seção impede que um ataque man-in-the-Middle reutilize uma conexão do SMB 3,0 para o SMB 2,0 (que usaria acesso não criptografado). No entanto, ele não impede um downgrade para SMB 1,0, o que também resultaria em acesso não criptografado. Para garantir que os clientes SMB 3,0 sempre usem a criptografia SMB para acessar compartilhamentos criptografados, você deve desabilitar o servidor SMB 1,0. (Para obter instruções, consulte a seção [desabilitando SMB 1,0](#disabling-smb-10).) Se a configuração **– RejectUnencryptedAccess** for deixada com sua configuração padrão de **$true**, somente clientes SMB com capacidade de criptografia 3,0 poderão acessar os compartilhamentos de arquivos (os clientes SMB 1,0 também serão rejeitados).

>[!NOTE]
>* A criptografia SMB usa o algoritmo AES (criptografia AES)-CCM para criptografar e descriptografar os dados. O AES-CCM também fornece validação de integridade de dados (assinatura) para compartilhamentos de arquivos criptografados, independentemente das configurações de assinatura SMB. Se você quiser habilitar a assinatura SMB sem criptografia, poderá continuar a fazer isso. Para obter mais informações, consulte [noções básicas de assinatura SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Você poderá encontrar problemas ao tentar acessar o compartilhamento de arquivos ou o servidor se sua organização usa dispositivos de aceleração de WAN (rede de longa distância).
>* Com uma configuração padrão (em que não há nenhum acesso não criptografado permitido para compartilhamentos de arquivos criptografados), se os clientes que não dão suporte ao SMB 3,0 tentarem acessar um compartilhamento de arquivos criptografados, a ID de evento 1003 será registrada no log de eventos Microsoft-Windows-SmbServer/Operational e o cliente receberá uma mensagem de erro de **acesso negado** .
>* A criptografia SMB e o Encrypting File System (EFS) no sistema de arquivos NTFS não estão relacionados e a criptografia SMB não exige ou depende do uso do EFS.
>* A criptografia SMB e a Criptografia de Unidade de Disco BitLocker não estão relacionadas e a criptografia SMB não exige ou depende do uso de Criptografia de Unidade de Disco BitLocker.

## <a name="secure-dialect-negotiation"></a>Negociação de dialeto seguro

O SMB 3,0 é capaz de detectar ataques man-in-the-Middle que tentam fazer o downgrade do protocolo SMB 2,0 ou SMB 3,0 ou os recursos que o cliente e o servidor negociam. Quando esse ataque é detectado pelo cliente ou pelo servidor, a conexão é desconectada e a ID de evento 1005 é registrada no log de eventos Microsoft-Windows-SmbServer/Operational. A negociação de dialeto seguro não pode detectar ou impedir Downgrades de SMB 2,0 ou 3,0 para SMB 1,0. Por isso, e para aproveitar os recursos completos da criptografia SMB, é altamente recomendável que você desabilite o servidor SMB 1,0. Para obter mais informações, consulte [desabilitando o SMB 1,0](#disabling-smb-10).

O recurso de negociação de dialeto seguro descrito na próxima seção impede um ataque man-in-the-Middle de fazer downgrade de uma conexão do SMB 3 para o SMB 2 (que usaria acesso não criptografado); no entanto, ele não impede o downgrade para o SMB 1, o que também resultaria em acesso não criptografado. Para obter mais informações sobre possíveis problemas com implementações anteriores do SMB que não são do Windows, consulte a [base de dados de conhecimento Microsoft](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Novo algoritmo de assinatura

O SMB 3,0 usa um algoritmo de criptografia mais recente para assinatura: Criptografia AES (AES)-código de autenticação de mensagem com base em codificação (CMAC). O SMB 2,0 usou o algoritmo de criptografia HMAC-SHA256 mais antigo. AES-CMAC e AES-CCM podem acelerar significativamente a criptografia de dados na maioria das CPUs modernas com suporte de instruções AES. Para obter mais informações, consulte [noções básicas de assinatura SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Desabilitando o SMB 1,0

O serviço Pesquisador de computadores herdado e os recursos do protocolo de administração remota no SMB 1,0 agora são separados e podem ser eliminados. Esses recursos ainda estão habilitados por padrão, mas se você não tiver clientes SMB mais antigos, como computadores que executam o Windows Server 2003 ou o Windows XP, poderá remover os recursos SMB 1,0 para aumentar a segurança e potencialmente reduzir a aplicação de patches.

>[!NOTE]
>O SMB 2,0 foi introduzido no Windows Server 2008 e no Windows Vista. Clientes mais antigos, como computadores que executam o Windows Server 2003 ou o Windows XP, não dão suporte a SMB 2,0; Portanto, eles não poderão acessar compartilhamentos de arquivos ou compartilhamentos de impressão se o servidor SMB 1,0 estiver desabilitado. Além disso, alguns clientes SMB que não são da Microsoft podem não conseguir acessar compartilhamentos de arquivos SMB 2,0 ou compartilhamentos de impressão (por exemplo, impressoras com a funcionalidade "Scan-to-share").

Antes de começar a desabilitar o SMB 1,0, você precisará descobrir se os clientes SMB estão conectados no momento ao servidor que executa o SMB 1,0. Para fazer isso, insira o seguinte cmdlet no Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Você deve executar esse script repetidamente no decorrer de uma semana (várias vezes por dia) para criar uma trilha de auditoria. Você também pode executar isso como uma tarefa agendada.

Para desabilitar o SMB 1,0, digite o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se uma conexão de cliente SMB for negada porque o servidor que está executando o SMB 1,0 foi desabilitado, a ID de evento 1001 será registrada no log de eventos Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Mais informações

Aqui estão alguns recursos adicionais sobre SMB e tecnologias relacionadas no Windows Server 2012.

- [Bloco de mensagens do servidor](file-server-smb-overview.md)
- [Armazenamento no Windows Server](../storage.md)
- [Servidor de Arquivos de Escalabilidade Horizontal para dados de aplicativo](../../failover-clustering/sofs-overview.md)