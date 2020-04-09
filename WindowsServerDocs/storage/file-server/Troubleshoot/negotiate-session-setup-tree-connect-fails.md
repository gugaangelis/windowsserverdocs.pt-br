---
title: Negociar, configuração de sessão e falhas de conexão de árvore
description: Apresenta como solucionar problemas de negociar, configuração de sessão e falhas de conexão de árvore.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 13124176e530aa7b74d18a38c906bf5297be511e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815379"
---
# <a name="negotiate-session-setup-and-tree-connect-failures"></a>Negociar, configuração de sessão e falhas de conexão de árvore

Este artigo descreve como solucionar problemas de falhas que ocorrem durante uma negociação SMB, configuração de sessão e solicitação de conexão de árvore.

## <a name="negotiate-fails"></a>A negociação falha

O servidor SMB recebe uma solicitação SMB NEGOTIAte de um cliente SMB. A conexão atinge o tempo limite e é redefinida após 60 segundos. Pode haver uma mensagem de confirmação após cerca de 200 microssegundos.

Esse problema é causado com mais frequência pelo programa antivírus.

Se você estiver usando o Windows Server 2008 R2, há hotfixes para esse problema. Verifique se o cliente SMB e o servidor SMB estão atualizados.

## <a name="session-setup-fails"></a>Falha na configuração da sessão

O servidor SMB recebe uma sessão SMB\_solicitação de instalação de um cliente SMB, mas falhou ao responder.

Se o nome de domínio totalmente qualificado (FQDN) ou o nome do sistema de entrada/saída básico (NetBIOS) do servidor for ' sed no caminho UNC (Convenção de nomenclatura universal), o Windows usará o Kerberos para autenticação.

Após a resposta de negociação, haverá uma tentativa de obter um tíquete Kerberos para o SPN (nome da entidade de serviço) do CIFS (sistema de arquivos de Internet comum) do servidor. Examine o tráfego Kerberos na porta TCP 88 para certificar-se de que não haja erros de Kerberos quando o cliente SMB estiver obtendo o token.

> [!NOTE]
> Os erros que ocorrem durante a pré-autenticação de Kerberos estão OK. Os erros que ocorrem após a pré-autenticação Kerberos (instâncias em que a autenticação não funciona) são os erros que causaram o problema de SMB.

Além disso, faça as seguintes verificações:

- Examine o blob de segurança na sessão SMB\_solicitação de instalação para certificar-se de que as credenciais corretas sejam enviadas.

- Tente desabilitar a proteção de nome do servidor SMB (**SmbServerNameHardeningLevel = 0**).

- Verifique se o servidor SMB tem um SPN quando ele é acessado por meio de um registro DNS CNAME.

- Verifique se a assinatura SMB está funcionando. (Isso é especialmente importante para dispositivos mais antigos e de terceiros.)

## <a name="tree-connect-fails"></a>Falha na conexão de árvore

Certifique-se de que as credenciais da conta de usuário tenham permissões de compartilhamento e do sistema de arquivos NT (NTFS) para a pasta.

A causa de erros comuns de conexão de árvore pode ser encontrada no [3.3.5.7 recebendo uma árvore de SMB2\_solicitação de conexão](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87). A seguir estão as soluções para dois códigos de status comuns.

STATUS de \[\_nome de\_de rede\_inadequado\]

Verifique se o compartilhamento existe no servidor e se está grafado corretamente na solicitação do cliente SMB.

STATUS de \[\_acesso\_NEGAdo\]

Verifique se o disco e a pasta que são usados pelo compartilhamento existem e estão acessíveis.

Se você estiver usando o SMBv3 ou posterior, verifique se o servidor e o compartilhamento exigem Criptografia, mas o cliente não dá suporte à criptografia. Para fazer isso, execute as seguintes ações:

- Verifique o servidor executando o comando a seguir.

  ```PowerShell
  Get-SmbServerConfiguration | select Encrypt*
  ```

  Se EncryptData e RejectUnencryptedAccess forem true, o servidor exigirá criptografia.

- Verifique o compartilhamento executando o seguinte comando:

  ```PowerShell
  Get-SmbShare | select name, EncryptData  
  ```

  Se EncryptData for true no compartilhamento e RejectUnencryptedAccess for true no servidor, a criptografia será exigida pelo compartilhamento

Siga estas diretrizes ao solucionar problemas:

- O Windows 8, o Windows Server 2012 e versões posteriores do Windows dão suporte à criptografia do lado do cliente (SMBv3 e posterior).

- O Windows 7, o Windows Server 2008 R2 e versões anteriores do Windows não oferecem suporte à criptografia do lado do cliente.

- O samba e o dispositivo de terceiros podem não dar suporte à criptografia. Talvez seja necessário consultar a documentação do produto para obter mais informações.

## <a name="references"></a>Referências

Para obter mais informações, consulte os artigos a seguir.

[3.3.5.4 recebendo uma solicitação de negociação SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/b39f253e-4963-40df-8dff-2f9040ebbeb1)

[3.3.5.5 recebendo uma sessão do SMB2\_solicitação de instalação](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/e545352b-9f2b-4c5e-9350-db46e4f6755e)

[3.3.5.7 recebendo uma árvore de SMB2\_solicitação de conexão](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87?redirectedfrom=MSDN)
