---
title: Melhorias de segurança do SMB
description: Uma explicação sobre o recurso de criptografia do SMB no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b1586c8c63e46452075b4106c944670395734142
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034407"
---
# <a name="smb-security-enhancements"></a>Melhorias de segurança do SMB

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tópico explica os aprimoramentos de segurança do SMB no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

## <a name="smb-encryption"></a>Criptografia SMB

Criptografia SMB fornece criptografia de ponta a ponta dos dados SMB e protege os dados contra ocorrências de interceptação em redes não confiáveis. Você pode implantar a criptografia SMB com um mínimo de esforço, mas ele pode exigir pequeno custos adicionais de software ou hardware especializado. Ele não tem nenhum requisito para o Internet Protocol security (IPsec) ou aceleradores de WAN. Criptografia SMB pode ser configurada em uma base por compartilhamento ou servidor de arquivos inteiro, e ele pode ser habilitado para uma variedade de cenários onde os dados percorrem redes não confiáveis.

>[!NOTE]
>Criptografia SMB não aborda a segurança em repouso, que normalmente é manipulada pela criptografia de unidade de disco BitLocker.

Criptografia SMB devem ser considerada para qualquer cenário em que os dados confidenciais precisam ser protegidos contra ataques man-in-the-middle. Os cenários possíveis incluem:

- Os dados confidenciais do operador de informações são movidos usando o protocolo SMB. Criptografia SMB oferece uma garantia de privacidade e integridade de ponta a ponta entre o servidor de arquivos e o cliente, independentemente das redes percorrido, como de longa distância (WAN) conexões de rede que são mantidas por provedores de terceiros.
- SMB 3.0 permite que os servidores de arquivo fornecer armazenamento continuamente disponível para aplicativos de servidor, como SQL Server ou Hyper-V. Habilitar a criptografia SMB fornece uma oportunidade para proteger essas informações contra ataques de espionagem. Criptografia SMB é mais simples de usar do que as soluções de hardware dedicado que são necessárias para a maioria das redes de área de armazenamento (SANs).

>[!IMPORTANT]
>Você deve observar que há um custo com qualquer proteção de criptografia de ponta a ponta, quando comparado a não-criptografado operacional notável de desempenho.

## <a name="enable-smb-encryption"></a>Habilitar a criptografia SMB

Você pode habilitar a criptografia SMB para o servidor de arquivos inteiro ou apenas para compartilhamentos de arquivos específicos. Use um dos procedimentos a seguir para habilitar a criptografia SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitar a criptografia SMB com o Windows PowerShell

1. Para habilitar a criptografia SMB para um compartilhamento de arquivos individuais, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar a criptografia SMB para o servidor de arquivos inteiro, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para criar um novo compartilhamento de arquivos SMB com a criptografia SMB habilitada, digite o seguinte script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar a criptografia SMB com o Gerenciador do servidor

1. No Gerenciador do servidor, abra **serviços de arquivo e armazenamento**.
2. Selecione **compartilhamentos** para abrir a página de gerenciamento de compartilhamentos.
3. Clique com botão direito no compartilhamento no qual você deseja habilitar a criptografia SMB e, em seguida, selecione **propriedades**.
4. Sobre o **as configurações** página do compartilhamento, selecione **criptografar o acesso a dados**. Acesso a esse compartilhamento de arquivos remoto é criptografado.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerações sobre a implantação da criptografia SMB

Por padrão, quando a criptografia SMB é habilitada para um compartilhamento de arquivos ou servidor, somente os clientes SMB 3.0 têm permissão para acessar os compartilhamentos de arquivos especificado. Isso reforça a intenção do administrador de proteger os dados para todos os clientes que acessam os compartilhamentos. No entanto, em algumas circunstâncias, um administrador talvez queira permitir o acesso não criptografado para clientes que não dão suporte a SMB 3.0 (por exemplo, durante um período de transição quando as versões de sistemas operacionais combinados do cliente estão sendo usadas). Para permitir o acesso não criptografado para clientes que não dão suporte a SMB 3.0, digite o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

O recurso de negociação de dialeto seguro descrito na próxima seção impede que um ataque man-in-the-middle fazer o downgrade de uma conexão do SMB 3.0 para o SMB 2.0 (que usaria o acesso sem criptografia). No entanto, ele não impede que um downgrade para o SMB 1.0, que também poderia resultar no acesso sem criptografia. Para garantir que os clientes SMB 3.0 sempre usam criptografia SMB para acessar os compartilhamentos criptografados, você deve desabilitar o servidor SMB 1.0. (Para obter instruções, consulte a seção [desabilitando o SMB 1.0](#disabling-smb-10).) Se o **– RejectUnencryptedAccess** configuração é deixada em sua configuração padrão de **$true**, clientes somente com capacidade de criptografia do SMB 3.0 têm permissão para acessar os compartilhamentos de arquivos (clientes SMB 1.0 também serão rejeitados).

>[!NOTE]
>* Criptografia SMB usa a criptografia AES (padrão avançado) – algoritmo CCM para criptografar e descriptografar os dados. AES-CCM também fornece validação de integridade de dados (assinatura) para compartilhamentos de arquivos criptografados, independentemente das configurações de assinatura SMB. Se você quiser habilitar a assinatura sem criptografia SMB, você pode continuar a fazer isso. Para obter mais informações, consulte [as Noções básicas da assinatura SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Você pode encontrar problemas ao tentar acessar o compartilhamento de arquivos ou o servidor se sua organização usa dispositivos de aceleração de rede de (longa distância WAN) de longa distância.
>* Com uma configuração padrão (em que não há nenhum acesso sem criptografia permitido para compartilhamentos de arquivos criptografados), se os clientes que não dão suporte a SMB 3.0 tentar acessar um compartilhamento de arquivos criptografados, 1003 de ID de evento é registrado no log de eventos Microsoft-Windows-SmbServer/Operational , e o cliente receberá um **acesso negado** mensagem de erro.
>* Criptografia SMB e o Encrypting File System (EFS) no sistema de arquivos NTFS não estão relacionados e criptografia SMB não exige ou dependem do uso do EFS.
>* Criptografia SMB e o BitLocker Drive Encryption não estão relacionados e criptografia SMB não exige ou dependem usando o BitLocker Drive Encryption.

## <a name="secure-dialect-negotiation"></a>Negociação de dialeto seguro

O SMB 3.0 é capaz de detectar ataques man-in-the-middle que tentam fazer o downgrade do protocolo SMB 2.0 ou SMB 3.0 ou os recursos que o cliente e servidor negociar. Quando um ataque desse tipo é detectado pelo cliente ou servidor, a conexão é desconectada e ID de evento 1005 é registrada no log de eventos Microsoft-Windows-SmbServer/operacional. Proteger o dialeto negociação não pode detectar nem prevenir downgrades do SMB 2.0 ou 3.0 para o SMB 1.0. Por isso e para aproveitar a todos os recursos da criptografia SMB, é altamente recomendável que você desative o servidor SMB 1.0. Para obter mais informações, consulte [desabilitando o SMB 1.0](#disabling-smb-10).

O recurso de negociação de dialeto seguro que é descrito na próxima seção impede que um ataque man-in-the-middle fazer o downgrade de uma conexão do SMB 3 para SMB 2 (que usaria o acesso sem criptografia); No entanto, ele não impede a downgrades para protocolos SMB 1, que também poderia resultar no acesso sem criptografia. Para obter mais informações sobre problemas potenciais com implementações de não-Windows anteriormente do SMB, consulte a [da Base de dados de Conhecimento Microsoft](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Novo algoritmo de assinatura

SMB 3.0 usa um algoritmo de criptografia mais recente para a assinatura: Padrão de criptografia avançada (AES) - cipher - com base em código de autenticação de mensagem (CMAC). SMB 2.0 usado o algoritmo de criptografia HMAC-SHA256 mais antigo. CMAC AES e AES-CCM podem acelerar significativamente a criptografia de dados em CPUs mais modernas que tem a instrução de AES de suporte. Para obter mais informações, consulte [as Noções básicas da assinatura SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Desabilitando o SMB 1.0

O serviço de navegador do computador herdado e recursos de protocolo de administração remota do SMB 1.0 agora estão separados, e eles podem ser eliminados. Esses recursos ainda estão habilitados por padrão, mas se você tiver clientes SMB mais antigos, como computadores que executam o Windows Server 2003 ou Windows XP, você pode remover os recursos do SMB 1.0 para aumentar a segurança e reduzir potencialmente a aplicação de patch.

>[!NOTE]
>SMB 2.0 foi introduzido no Windows Server 2008 e Windows Vista. Clientes mais antigos, como computadores que executam o Windows Server 2003 ou Windows XP, não dão suporte a SMB 2.0; e, portanto, eles não poderão acessar compartilhamentos de arquivos ou compartilhamentos de impressão, se o servidor SMB 1.0 está desabilitado. Além disso, alguns clientes SMB não Microsoft podem não ser capazes de acessar compartilhamentos de arquivos do SMB 2.0 ou imprimir compartilhamentos (por exemplo, impressoras com a funcionalidade de "verificação para compartilhamento").

Antes de começar a desabilitar o SMB 1.0, você precisará descobrir se os clientes SMB atualmente estão conectados ao servidor executando o SMB 1.0. Para fazer isso, insira o seguinte cmdlet no Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Esse script deve ser executado repetidamente ao longo de uma semana (várias vezes por dia) para criar uma trilha de auditoria. Você também pode executar isso como uma tarefa agendada.

Para desabilitar o SMB 1.0, insira o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se uma conexão de cliente SMB é negada porque o servidor que executa o SMB 1.0 foi desabilitado, o evento ID 1001 será registrado no log de eventos Microsoft-Windows-SmbServer/operacional.

## <a name="more-information"></a>Mais informações

Aqui estão alguns recursos adicionais sobre o SMB e as tecnologias relacionadas no Windows Server 2012.

- [Protocolo SMB](file-server-smb-overview.md)
- [Armazenamento no Windows Server](../storage.md)
- [Servidor de arquivos de escalabilidade horizontal para dados de aplicativo](../../failover-clustering/sofs-overview.md)