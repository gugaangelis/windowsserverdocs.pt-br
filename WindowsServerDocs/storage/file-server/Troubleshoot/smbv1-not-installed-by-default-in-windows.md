---
title: O SMBv1 não é instalado por padrão no Windows 10 versão 1709, Windows Server versão 1709 e versões posteriores
description: Discute o comportamento do protocolo SMBv1 na atualização dos criadores de outono do Windows 10 e no Windows Server, versão 1709 e versões posteriores.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: fdc90c6e5d6790348fafc12079eec5ac7e387b3f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815309"
---
# <a name="smbv1-is-not-installed-by-default-in-windows-10-version-1709-windows-server-version-1709-and-later-versions"></a>O SMBv1 não é instalado por padrão no Windows 10 versão 1709, Windows Server versão 1709 e versões posteriores

## <a name="summary"></a>Resumo

No Windows 10 outono Creators Update e no Windows Server, versão 1709 (RS3) e versões posteriores, o protocolo de rede do SMBv1 (Server Message Block versão 1) não é mais instalado por padrão. Ele foi substituído por SMBv2 e protocolos posteriores a partir do 2007. a Microsoft preteriu publicamente o protocolo SMBv1 em 2014. 

O SMBv1 tem o seguinte comportamento na atualização dos criadores de outono do Windows 10 e no Windows Server, versão 1709 (RS3): 
 
- O SMBv1 agora tem os subrecursos de cliente e servidor que podem ser desinstalados separadamente.    
- O Windows 10 Enterprise e o Windows 10 Education não contêm mais o cliente ou servidor SMBv1 por padrão após uma instalação limpa.    
- O Windows Server 2016 não contém mais o cliente ou servidor SMBv1 por padrão após uma instalação limpa.    
- O Windows 10 Home e o Windows 10 Professional não contêm mais o servidor SMBv1 por padrão após uma instalação limpa.    
- O Windows 10 Home e o Windows 10 Professional ainda contêm o cliente SMBv1 por padrão após uma instalação limpa. Se o cliente SMBv1 não for usado por 15 dias no total (excluindo o computador que está sendo desligado), ele será desinstalado automaticamente.    
- As atualizações in-loco e os vôos do Insider do Windows 10 Home e do Windows 10 Professional não removem automaticamente o SMBv1 inicialmente. Se o cliente ou servidor SMBv1 não for usado por 15 dias no total (excluindo o tempo durante o qual o computador está desligado), cada um deles será desinstalado automaticamente.     
- As atualizações in-loco e os vôos insiders das edições Windows 10 Enterprise e Windows 10 Education não removem automaticamente o SMBv1. Um administrador deve decidir desinstalar o SMBv1 nesses ambientes gerenciados. No Windows 10, versão 1809 (RS5) e versões posteriores, um administrador pode ativar a remoção automática de SMBv1 ativando o recurso "remoção automática de SMB 1.0/CIFS".    
- A remoção automática de SMBv1 após 15 dias é uma operação única. Se um administrador reinstalar o SMBv1, nenhuma outra tentativa será feita para desinstalá-lo.
- Os recursos SMB versão 2, 2, 2,1, 3,0, 3, 2 e 3.1.1 ainda têm suporte completo e são incluídos por padrão como parte dos binários do SMBv2.    
- Como o serviço Pesquisador de computadores depende do SMBv1, o serviço será desinstalado se o cliente ou servidor do SMBv1 for desinstalado. Isso significa que o Explorer networkcan não exibe mais computadores Windows por meio do método de navegação de datagrama NetBIOS herdado.    
- O SMBv1 ainda pode ser reinstalado em todas as edições do Windows 10 e do Windows Server 2016.    
 
  > [!NOTE]
  > O Windows 10, versão 1803 (RS4) Professional lida com o SMBv1 da mesma maneira que o Windows 10, versão 1703 (RS2) e Windows 10, versão 1607 (RS1). Esse problema foi corrigido no Windows 10, versão 1809 (RS5). Você ainda pode desinstalar o SMBv1 manualmente. No entanto, o Windows não desinstalará automaticamente o SMBv1 após 15 dias nos seguintes cenários: 
 
-  Você faz uma instalação limpa do Windows 10, versão 1803.     
-  Você atualiza o Windows 10, versão 1607 ou Windows 10, versão 1703 para o Windows 10, versão 1803 diretamente, sem primeiro atualizar para o Windows 10, versão 1709.     
 
Se você tentar se conectar a dispositivos que dão suporte apenas a SMBv1, ou se esses dispositivos tentarem se conectar a você, você poderá receber uma das seguintes mensagens de erro:     

```
You can't connect to the file share because it's not secure. This share requires the obsolete SMB1 protocol, which is unsafe and could expose your system to attack.
Your system requires SMB2 or higher. For more info on resolving this issue, see: https://go.microsoft.com/fwlink/?linkid=852747  
```

```
The specified network name is no longer available.
```

```
Unspecified error 0x80004005
```

```
System Error 64
```

```
The specified server cannot perform the requested operation.
```

```
Error 58
```    

Os eventos a seguir são exibidos quando um servidor remoto precisa de uma conexão SMBv1 desse cliente, mas SMBv1 é desinstalado ou desabilitado no cliente.

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32002
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description:
 The local computer received an SMB1 negotiate response. 

Dialect: 
SecurityMode 
Server name: 

Guidance: 
SMB1 is deprecated and should not be installed nor enabled. For more information, see https://go.microsoft.com/fwlink/?linkid=852747.
```

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32000
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description: 
SMB1 negotiate response received from remote device when SMB1 cannot be negotiated by the local computer. 
Dialect: 
Server name: 

Guidance: 
The client has SMB1 disabled or uninstalled. For more information: https://go.microsoft.com/fwlink/?linkid=852747.     
```

Esses dispositivos provavelmente não estão executando o Windows. É mais provável que eles executem versões mais antigas do Linux, do samba ou de outros tipos de software de terceiros para fornecer serviços SMB. Muitas vezes, essas versões do Linux e do samba não têm mais suporte. 

> [!NOTE]
> O Windows 10, versão 1709, também é conhecido como "atualização de criadores de Outono".   

## <a name="more-information"></a>Mais informações

Para contornar esse problema, entre em contato com o fabricante do produto que dá suporte apenas a SMBv1 e solicite uma atualização de software ou firmware que ofereça suporte a SMBv 2.02 ou uma versão posterior. Para obter uma lista atual de fornecedores conhecidos e seus requisitos de SMBv1, consulte o seguinte blog da equipe de engenharia de armazenamento do Windows e do Windows Server: 

[Câmara de compensação do produto SMBv1](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/SMB1-Product-Clearinghouse/ba-p/426008) 
#### <a name="leasing-mode"></a>Modo de leasing

Se SMBv1 for necessário para fornecer compatibilidade de aplicativos para o comportamento de software herdado, como um requisito para desabilitar o oplocks, o Windows fornecerá um novo sinalizador de compartilhamento SMB conhecido como modo de concessão. Esse sinalizador especifica se um compartilhamento desabilita a semântica de SMB moderna, como concessões e oplocks.

Você pode especificar um compartilhamento sem usar oplocks ou leasing para permitir que um aplicativo herdado funcione com o SMBv2 ou uma versão posterior. Para fazer isso, use os cmdlets do PowerShell **New-SmbShare** ou **set-SmbShare** juntamente com o parâmetro **-LeasingMode None** .

> [!NOTE]
> Você deve usar essa opção somente em compartilhamentos que são exigidos por um aplicativo de terceiros para suporte herdado se o fornecedor indicar que ele é necessário. Não especifique o modo de concessão em compartilhamentos de dados do usuário ou compartilhamentos de CA que são usados por servidores de arquivos de escalabilidade horizontal. Isso ocorre porque a remoção de oplocks e concessões causa instabilidade e corrupção de dados na maioria dos aplicativos. O modo de leasing funciona apenas no modo de compartilhamento. Ele pode ser usado por qualquer sistema operacional cliente.

#### <a name="explorer-network-browsing"></a>Navegação na rede do Explorer

O serviço Pesquisador de computadores depende do protocolo SMBv1 para popular o nó de rede do Windows Explorer (também conhecido como "ambiente de rede"). Esse protocolo herdado é muito preterido, não é roteado e tem segurança limitada. Como o serviço não pode funcionar sem SMBv1, ele é removido ao mesmo tempo.

No entanto, se você ainda precisar usar os ambientes de grupo de trabalho Entrada na Rede Home e Small Business para localizar computadores baseados no Windows, poderá seguir estas etapas em seus computadores baseados no Windows que não usam mais o SMBv1: 
 
1. Inicie os serviços "host do provedor de descoberta de função" e "publicação de recurso de descoberta de função" e defina-os como **automático (início atrasado)** .

2. Quando você abrir a rede do Explorer, habilite a descoberta de rede quando solicitado.    
 
Todos os dispositivos Windows dentro dessa sub-rede que têm essas configurações agora aparecerão na rede para navegação. Isso usa o protocolo WS-DISCOVERY. Entre em contato com outros fornecedores e fabricantes se seus dispositivos ainda não aparecerem nesta lista de procura depois que os dispositivos Windows forem exibidos. É possível que eles tenham esse protocolo desabilitado ou que ofereçam suporte apenas a SMBv1.

> [!NOTE]
> recomendamos que você mapeie unidades e impressoras em vez de habilitar esse recurso, o que ainda requer pesquisa e navegação para seus dispositivos. Os recursos mapeados são mais fáceis de localizar, exigem menos treinamento e são mais seguros de usar. Isso será especialmente verdadeiro se esses recursos forem fornecidos automaticamente por meio de Política de Grupo. Um administrador pode configurar impressoras para localização por métodos diferentes do serviço Pesquisador de computadores herdado usando endereços IP, Active Directory Domain Services (AD DS), Bonjour, mDNS, uPnP e assim por diante.

Se você não puder usar nenhuma dessas soluções alternativas ou se o fabricante do aplicativo não puder fornecer versões com suporte do SMB, você poderá reabilitar o SMBv1 manualmente seguindo as etapas em [como detectar, habilitar e desabilitar o SMBv1, o SMBv2 e o SMBv3 no Windows](detect-enable-and-disable-smbv1-v2-v3.md).

> [!IMPORTANT]
> É altamente recomendável que você não reinstale o SMBv1. Isso ocorre porque esse protocolo mais antigo tem problemas de segurança conhecidos relacionados a ransomware e a outros malwares.  

#### <a name="windows-server-best-practices-analyzer-messaging"></a>Mensagens do analisador de práticas recomendadas do Windows Server

Os sistemas de operação de servidor do Windows Server 2012 e posterior contêm um analisador de práticas recomendadas (BPA) para servidores de arquivos. Se você seguiu as diretrizes online corretas para desinstalar o SMB1, a execução deste BPA retornará uma mensagem de aviso contraditória:

    Title: The SMB 1.0 file sharing protocol should be enabled
    Severity: Warning
    Date: 3/25/2020 12:38:47 PM
    Category: Configuration
    Problem: The Server Message Block 1.0 (SMB 1.0) file sharing protocol is disabled on this file server.
    Impact: SMB not in a default configuration, which could lead to less than optimal behavior.
    Resolution: Use Registry Editor to enable the SMB 1.0 protocol.

Você deve ignorar as diretrizes específicas da regra de BPA, mas ela foi preterida. Repetimos: não habilite o SMB 1,0.

## <a name="references"></a>Referências

[Parar de usar o SMB1](https://aka.ms/stopusingsmb1)
