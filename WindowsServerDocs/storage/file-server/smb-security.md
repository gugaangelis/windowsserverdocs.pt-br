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
ms.openlocfilehash: 658875f132712d34a2c59967ebd316e8c5edca7c
ms.sourcegitcommit: 568b924d32421256f64abfee171304f1daf320d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85070547"
---
# <a name="smb-security-enhancements"></a>Melhorias de segurança do SMB

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tópico explica os aprimoramentos de segurança do SMB no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2016.

## <a name="smb-encryption"></a>Criptografia SMB

A Criptografia SMB fornece criptografia de ponta a ponta dos dados do SMB e protege os dados contra ocorrências de interceptação em redes não confiáveis. Você pode implantar a Criptografia SMB com esforço mínimo, mas pode exigir pequenos custos adicionais para hardware ou software especializado. Não tem requisitos de IPsec (Internet Protocol Security) ou de aceleradores de WAN. A Criptografia SMB pode ser configurada a cada compartilhamento ou para todo o servidor de arquivos e pode ser habilitado para uma variedade de cenários em que os dados percorrem redes não confiáveis.

>[!NOTE]
>A criptografia SMB não abrange a segurança em repouso, que normalmente é manipulada pela Criptografia de Unidade de Disco BitLocker.

A criptografia SMB deve ser considerada para qualquer cenário no qual os dados confidenciais precisem ser protegidos contra ataques man-in-the-middle. Os cenários possíveis incluem:

- Os dados confidenciais de um profissional da informação são movidos usando o protocolo SMB. A criptografia SMB oferece uma garantia de integridade e privacidade de ponta a ponta entre o servidor de arquivos e o cliente, independentemente das redes que são cruzadas, como conexões de WAN (rede de longa distância) mantidas por provedores que não são da Microsoft.
- O SMB 3.0 permite que os servidores de arquivos forneçam armazenamento continuamente disponível para aplicativos de servidor, como o SQL Server ou o Hyper-V. A habilitação da criptografia SMB oferece uma oportunidade de proteger essas informações contra ataques de espionagem. A criptografia SMB é mais simples de usar do que as soluções de hardware dedicadas exigidas pela maioria das SANs (redes de área de armazenamento).

>[!IMPORTANT]
>Você deve observar que há um custo operacional de desempenho considerável com qualquer proteção de criptografia de ponta a ponta em comparação com a opção não criptografada.

## <a name="enable-smb-encryption"></a>Habilitar Criptografia SMB

Você pode habilitar a Criptografia SMB para todo o servidor de arquivos ou apenas para compartilhamentos de arquivos específicos. Use um dos seguintes procedimentos para habilitar a Criptografia SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitar a criptografia SMB com o Windows PowerShell

1. Para habilitar a Criptografia SMB para um compartilhamento de arquivo individual, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar a Criptografia SMB para todo o servidor de arquivos, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para criar um compartilhamento de arquivo SMB com a Criptografia SMB habilitada, digite o seguinte script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar a Criptografia SMB com Gerenciador do Servidor

1. No Gerenciador do Servidor, abra **Serviços de Arquivo e Armazenamento**.
2. Selecione **Compartilhamentos** para abrir a página de gerenciamento de compartilhamentos.
3. Clique com o botão direito do mouse no compartilhamento no qual você deseja habilitar a Criptografia SMB e selecione **Propriedades**.
4. Na página **Configurações** do compartilhamento, selecione **Criptografar o acesso a dados**. O acesso ao arquivo remoto para esse compartilhamento é criptografado.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerações para implantar a Criptografia SMB

Por padrão, quando a criptografia SMB está habilitada para um compartilhamento de arquivo ou servidor, somente clientes SMB 3.0 podem acessar os compartilhamentos de arquivos especificados. Isso impõe a intenção do administrador de proteger os dados para todos os clientes que acessam os compartilhamentos. No entanto, em algumas circunstâncias, um administrador pode querer permitir o acesso não criptografado para clientes que não dão suporte a SMB 3.0 (por exemplo, durante um período de transição quando versões mistas do sistema operacional do cliente estão sendo usadas). Para permitir o acesso não criptografado para clientes que não dão suporte a SMB 3.0, digite o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

A funcionalidade de negociação de dialeto seguro descrita na próxima seção impede que um ataque man-in-the-middle faça o downgrade de uma conexão do SMB 3.0 para o SMB 2.0 (que usaria acesso não criptografado). No entanto, não impede o downgrade para SMB 1.0, o que também resultaria em acesso não criptografado. Para garantir que os clientes SMB 3.0 sempre usem a criptografia SMB para acessar compartilhamentos criptografados, você deve desabilitar o servidor SMB 1.0. (Para obter instruções, confira a seção [Como desabilitar SMB 1.0](#disabling-smb-10).) Se a configuração **–RejectUnencryptedAccess** for deixada em sua configuração padrão de **$true**, somente os clientes SMB 3.0 com capacidade de criptografia poderão acessar os compartilhamentos de arquivos (os clientes SMB 1.0 também serão rejeitados).

>[!NOTE]
>* A Criptografia SMB usa o algoritmo de criptografia AES-CCM para criptografar e descriptografar os dados. O AES-CCM também fornece validação de integridade de dados (assinatura) para compartilhamentos de arquivos criptografados, independentemente das configurações de assinatura SMB. Se você quiser habilitar a assinatura SMB sem criptografia, poderá continuar fazendo isso. Para saber mais, confira [As noções básicas da assinatura SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Você poderá encontrar problemas ao tentar acessar o compartilhamento de arquivo ou o servidor se a sua organização usar dispositivos de aceleração de WAN (rede de longa distância).
>* Com uma configuração padrão (em que não há nenhum acesso não criptografado permitido para compartilhamentos de arquivos criptografados), se os clientes que não dão suporte ao SMB 3.0 tentarem acessar um compartilhamento de arquivo criptografados, a ID de evento 1003 será registrada no log de eventos Microsoft-Windows-SmbServer/Operational e o cliente receberá uma mensagem de erro de **Acesso negado**.
>* A criptografia SMB e o EFS (Encrypting File System) no sistema de arquivos NTFS não estão relacionados e a criptografia SMB não exige nem depende do uso do EFS.
>* A criptografia SMB e a Criptografia de Unidade de Disco BitLocker não estão relacionadas e a criptografia SMB não exige nem depende do uso de Criptografia de Unidade de Disco BitLocker.

## <a name="secure-dialect-negotiation"></a>Negociação de dialeto seguro

O SMB 3.0 é capaz de detectar ataques man-in-the-middle que tentam fazer downgrade do protocolo SMB 2.0 ou SMB 3.0 ou os recursos que o cliente e o servidor negociam. Quando esse ataque é detectado pelo cliente ou pelo servidor, a conexão é desconectada e a ID de evento 1005 é registrada no log de eventos Microsoft-Windows-SmbServer/Operational. A negociação de dialeto seguro não pode detectar nem impedir downgrades do SMB 2.0 ou 3.0 para o SMB 1.0. Por isso e para aproveitar os recursos completos da criptografia SMB, é altamente recomendável desabilitar o servidor SMB 1.0. Para obter mais informações, confira [Como desabilitar SMB 1.0](#disabling-smb-10).

A funcionalidade de negociação de dialeto seguro descrito na próxima seção impede um ataque man-in-the-middle de fazer downgrade de uma conexão do SMB 3 para o SMB 2 (que usaria acesso não criptografado). No entanto, não impede o downgrade para o SMB 1.0, o que também resultaria em acesso não criptografado. Para obter mais informações sobre possíveis problemas com implementações anteriores não Windows do SMB, confira a [Base de dados de conhecimento Microsoft](https://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Novo algoritmo de assinatura

O SMB 3.0 usa um algoritmo de criptografia mais recente para assinatura: CMCA (Message Authentication Code baseado em criptografia)-criptografia AES. O SMB 2.0 usou o algoritmo de criptografia HMAC-SHA256 mais antigo. O AES-CMAC e o AES-CCM podem acelerar significativamente a criptografia de dados na maioria das CPUs modernas compatíveis com instruções AES. Para saber mais, confira [As noções básicas da assinatura SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Como desabilitar o SMB 1.0

Os recursos do Protocolo de Administração Remota e o serviço de navegador de computador herdado no SMB 1.0 agora são separados e podem ser eliminados. Esses recursos ainda são habilitados por padrão, mas se você não tiver nenhum cliente SMB mais antigo, como Windows XP ou Windows Server 2003, poderá remover os recursos do SMB 1.0 para aumentar a segurança e reduzir potencialmente a aplicação de patches.

>[!NOTE]
>O SMB 2.0 foi introduzido no Windows Server 2008 e no Windows Vista. Clientes mais antigos, como computadores que executam o Windows Server 2003 ou o Windows XP, não dão suporte a SMB 2.0. Portanto, não poderão acessar compartilhamentos de arquivos nem compartilhamentos de impressão se o servidor SMB 1.0 estiver desabilitado. Além disso, alguns clientes SMB que não são da Microsoft podem não conseguir acessar compartilhamentos de arquivos SMB 2.0 ou compartilhamentos de impressão (por exemplo, impressoras com a funcionalidade "digitalizar para compartilhamento").

Antes de começar a desabilitar o SMB 1.0, você precisará descobrir se os clientes SMB estão conectados no momento ao servidor que executa o SMB 1.0. Para fazer isso, insira o seguinte cmdlet no Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Você deve executar esse script repetidamente no decorrer de uma semana (várias vezes por dia) para criar uma trilha de auditoria. Você também pode executar isso como uma tarefa agendada.

Para desabilitar o SMB 1.0, insira o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se uma conexão de cliente SMB for negada porque o servidor que está executando o SMB 1.0 foi desabilitado, a ID de evento 1001 será registrada no log de eventos Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Mais informações

Aqui estão alguns recursos adicionais sobre SMB e tecnologias relacionadas no Windows Server 2012.

- [Protocolo SMB](file-server-smb-overview.md)
- [Armazenamento no Windows Server](../storage.yml)
- [Servidor de Arquivos de Escalabilidade Horizontal para Dados de Aplicativos](../../failover-clustering/sofs-overview.md)