---
title: Solução de problemas de multicanal do SMB
description: Apresenta os métodos de solução de problemas de Multichannel do SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 662a58fdeb3cda14a0e54c8d0ab7bd0b85387fd7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960128"
---
# <a name="smb-multichannel-troubleshooting"></a>Solução de problemas de multicanal do SMB

Este artigo descreve como solucionar problemas relacionados ao SMB multicanal.

## <a name="check-the-network-interface-status"></a>Verificar o status da interface de rede

Verifique se a associação para a interface de rede está definida como **true** no cliente SMB (MS \_ Client) e no servidor SMB (MS \_ Server). Quando você executar o comando a seguir, a saída deverá mostrar **true** em **habilitado** para ambas as interfaces de rede:

```PowerShell
Get-NetAdapterBinding -ComponentID ms_server,ms_msclient
```

Depois disso, verifique se a interface de rede está listada na saída dos seguintes comandos:

```PowerShell
Get-SmbServerNetworkInterface
```

```PowerShell
Get-SmbClientNetworkInterface
```

Você também pode executar o comando **Get-netadapter** para exibir o índice de interface para verificar o resultado. O índice de interface mostra todos os adaptadores SMB ativos que estão ativamente associados à interface apropriada.

## <a name="check-the-firewall"></a>Verificar o firewall

Se houver apenas um endereço IP de link local e nenhum endereço roteável publicamente, o perfil de rede provavelmente será definido como **público**. Isso significa que o SMB é bloqueado no firewall por padrão.

O comando a seguir revela qual perfil de conexão está sendo usado. Você também pode usar o centro de rede e compartilhamento para recuperar essas informações.

**Get-NetConnectionProfile**

No grupo **compartilhamento de arquivos e impressoras** , verifique as regras de entrada do firewall para certificar-se de que "SMB-in" esteja habilitado para o perfil correto.

![Regras de SMB](media/smb-multichannel-troubleshooting-1.png)

Você também pode habilitar o **compartilhamento de arquivos e impressoras** na janela **central de rede e compartilhamento** . Para fazer isso, selecione **alterar configurações de compartilhamento avançadas** no menu à esquerda e, em seguida, selecione **ativar o compartilhamento de arquivos e impressoras** para o perfil. Essa opção habilita as regras de firewall de compartilhamento de arquivos e impressoras.

![Alterar configurações de compartilhamento avançadas](media/smb-multichannel-troubleshooting-2.png)

## <a name="capture-client-and-server-sided-traffic-for-troubleshooting"></a>Capturar o tráfego do cliente e do servidor para solução de problemas

Você precisa das informações de rastreamento de conexão SMB que começam do handshake TCP de três vias. Recomendamos que você feche todos os aplicativos (especialmente o Windows Explorer) antes de iniciar a captura. Reinicie o serviço de **estação de trabalho** no cliente SMB, inicie a captura de pacote e, em seguida, reproduza o problema.

Verifique se a conexão SMBv3.*x* está sendo negociada e se nada entre o servidor e o cliente está afetando a negociação de dialeto. SMBv2 e versões anteriores não oferecem suporte a multicanal.

Procure os pacotes de \_ informações de interface de rede \_ . É aí que o cliente SMB solicita uma lista de adaptadores do servidor SMB. Se esses pacotes não forem trocados, o multicanal não funcionará.

O servidor responde retornando uma lista de interfaces de rede válidas. Em seguida, o cliente SMB adiciona-os à lista de adaptadores disponíveis para multicanal. Neste ponto, o multicanal deve iniciar e, pelo menos, tentar iniciar a conexão.

Para obter mais informações, consulte os seguintes artigos:

- [Solicitações de aplicativos 3.2.4.20.10 consultando interfaces de rede do servidor](/openspecs/windows_protocols/ms-smb2/147adde4-d936-4597-924a-8caa3429c6b0)

- [Resposta de \_ informações de interface de rede 2.2.32.5 \_](/openspecs/windows_protocols/ms-smb2/fcd862d1-1b85-42df-92b1-e103199f531f)

- [3.2.5.14.11 manipulando uma resposta de interfaces de rede](/openspecs/windows_protocols/ms-smb2/5459722b-1eaa-4ead-b465-284363264cad)

Nos cenários a seguir, um adaptador não pode ser usado:

- Há um problema de roteamento no cliente. Isso geralmente é causado por uma tabela de roteamento incorreta que força o tráfego na interface errada.

- Restrições de multicanal foram definidas. Para obter mais informações, consulte [New-SmbMultichannelConstraint](/powershell/module/smbshare/new-smbmultichannelconstraint).

- Algo bloqueou os pacotes de solicitação e resposta da interface de rede.

- O cliente e o servidor não podem se comunicar por meio da interface de rede extra. Por exemplo, o handshake de três vias TCP falhou, a conexão é bloqueada por um firewall, a instalação da sessão falhou e assim por diante.

Se o adaptador e seu endereço IPv6 estiverem na lista enviada pelo servidor, a próxima etapa será ver se as comunicações são tentadas nessa interface. Filtre o rastreamento pelo endereço de link local e pelo tráfego SMB e procure uma tentativa de conexão. Se esse for um rastreamento de NetConnection, você também poderá examinar os eventos da WFP (plataforma de filtragem do Windows) para ver se a conexão está sendo bloqueada.
