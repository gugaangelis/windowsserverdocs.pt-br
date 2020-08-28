---
title: ktpass
description: Artigo de referência para o comando ktpass, que configura o nome principal do servidor para o host ou serviço no AD DS e gera um arquivo. keytab que contém a chave secreta compartilhada do serviço.
ms.topic: reference
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ef7e2ba1aa84faa44cf4bf77e842e8d3bcdc235
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028234"
---
# <a name="ktpass"></a>ktpass

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura o nome principal do servidor para o host ou serviço no Active Directory Domain Services (AD DS) e gera um arquivo. keytab que contém a chave secreta compartilhada do serviço. O arquivo .keytab baseia-se na implementação do protocolo de autenticação Kerberos pelo MIT (Massachusetts Institute of Technology). A ferramenta de linha de comando ktpass permite que serviços não Windows que dão suporte à autenticação Kerberos usem os recursos de interoperabilidade fornecidos pelo serviço de centro de distribuição de chaves Kerberos (KDC).

## <a name="syntax"></a>Sintaxe

```
ktpass
[/out <filename>]
[/princ <principalname>]
[/mapuser <useraccount>]
[/mapop {add|set}] [{-|+}desonly] [/in <filename>]
[/pass {password|*|{-|+}rndpass}]
[/minpass]
[/maxpass]
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]
[/itercount]
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]
[/kvno <keyversionnum>]
[/answer {-|+}]
[/target]
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <password>]  [/?|/h|/help]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ------------|
| /out `<filename>` | Especifica o nome do arquivo Kerberos versão 5. keytab a ser gerado. **Observação:** Esse é o arquivo. keytab que você transfere para um computador que não está executando o sistema operacional Windows e, em seguida, substitui ou mescla com o arquivo. keytab existente, */etc/krb5.keytab*. |
| /princ `<principalname>` | Especifica o nome da entidade de segurança no formulário host/computer.contoso.com@CONTOSO.COM . **AVISO:** Esse parâmetro diferencia maiúsculas de minúsculas. |
| /mapuser `<useraccount>` | Mapeia o nome da entidade de segurança Kerberos, que é especificada pelo parâmetro **princ** , para a conta de domínio especificada. |
| /mapop `{add|set}` | Especifica como o atributo de mapeamento é definido.<ul><li>**Adicionar** – adiciona o valor do nome de usuário local especificado. Esse é o padrão.</li><li>**Set** – define o valor da criptografia somente des (padrão de criptografia de dados) para o nome de usuário local especificado.</li></ul> |
| `{-|+}`desonly | A criptografia somente DES é definida por padrão.<ul><li>**+** Define uma conta para criptografia somente DES.</li><li>**-** Libera a restrição em uma conta para criptografia somente DES. **Importante:** O Windows não dá suporte a DES por padrão.</li></ul> |
| /in `<filename>` | Especifica o arquivo. keytab para ler de um computador host que não esteja executando o sistema operacional Windows. |
| /pass `{password|*|{-|+}rndpass}` | Especifica uma senha para o nome de usuário principal que é especificado pelo parâmetro **princ** . Use `*` para solicitar uma senha. |
| /minpass | Define o comprimento mínimo da senha aleatória para 15 caracteres. |
| /maxpass | Define o comprimento máximo da senha aleatória como 256 caracteres. |
| /crypto `{DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}` | Especifica as chaves que são geradas no arquivo keytab:<ul><li>**Des-CBC-CRC** -usado para compatibilidade.</li><li>**Des-CBC-MD5** -segue mais de acordo com a implementação de MIT e é usada para compatibilidade.</li><li>**RC4-HMAC-NT** -emprega a criptografia de 128 bits.</li><li>**Aes256-SHA1** -emprega a criptografia aes256-CTS-HMAC-SHA1-96.</li><li>   **Aes128-SHA1** -emprega a criptografia aes128-CTS-HMAC-SHA1-96.</li><li>**Todos** -Estados que todos os tipos de criptografia com suporte podem ser usados.</li></ul><p>**Observação:** Como as configurações padrão se baseiam em versões mais antigas do MIT, você sempre deve usar o `/crypto` parâmetro. |
| /itercount | Especifica a contagem de iterações que é usada para criptografia AES. O padrão ignora **itercount** para criptografia não AES e define a criptografia aes como 4.096. |
| /ptype `{KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}` | Especifica o tipo de entidade de segurança.<ul><li>**KRB5_NT_PRINCIPAL** -o tipo de entidade de segurança geral (recomendado).</li><li>**KRB5_NT_SRV_INST** -a instância do serviço de usuário</li><li>  **KRB5_NT_SRV_HST** -a instância do serviço de host</li></ul> |
| /kvno `<keyversionnum>` | Especifica o número de versão da chave. O valor padrão é 1. |
| /answer `{-|+}` | Define o modo de resposta de segundo plano:<ul><li>**-** Respostas redefinem prompts de **senha automaticamente sem.**</li><li>**+** Respostas redefinem prompts de senha automaticamente com **Sim**.</li></ul> |
| /target | Define qual controlador de domínio usar. O padrão é que o controlador de domínio seja detectado, com base no nome principal. Se o nome do controlador de domínio não for resolvido, uma caixa de diálogo solicitará um controlador de domínio válido. |
| /rawsalt | força o ktpass a usar o algoritmo rawsalt ao gerar a chave. Esse parâmetro é opcional. |
| `{-|+}dumpsalt` | A saída desse parâmetro mostra o algoritmo parâmetro de Salt do MIT que está sendo usado para gerar a chave. |
| `{-|+}setupn` | Define o UPN (nome principal do usuário), além do SPN (nome da entidade de serviço). O padrão é definir ambos no arquivo. keytab. |
| `{-|+}setpass <password>` | Define a senha do usuário quando fornecida. Se rndpass for usado, uma senha aleatória será gerada em vez disso. |
| /? | Exibe a ajuda para este comando. |

#### <a name="remarks"></a>Comentários

- Os serviços em execução em sistemas que não estão executando o sistema operacional Windows podem ser configurados com contas de instância de serviço no AD DS. Isso permite que qualquer cliente Kerberos se autentique em serviços que não estejam executando o sistema operacional Windows usando o Windows KDCs.

- O parâmetro **/princ** não é avaliado por ktpass e é usado como fornecido. Não há nenhuma verificação para ver se o parâmetro corresponde ao caso exato do valor do atributo **userPrincipalName** ao gerar o arquivo keytab. As distribuições Kerberos que diferenciam maiúsculas de minúsculas usando esse arquivo keytab podem ter problemas se não houver correspondência exata de maiúsculas e minúsculas e até mesmo falharem durante a pré-autenticação. Para verificar e recuperar o valor correto do atributo **userPrincipalName** de um arquivo de exportação do LDifDE. Por exemplo:

    ```
    ldifde /f keytab_user.ldf /d CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com /p base /l samaccountname,userprincipalname
    ````

### <a name="examples"></a>Exemplos

Para criar um arquivo Kerberos. keytab para um computador host que não esteja executando o sistema operacional Windows, você deve mapear a entidade de segurança para a conta e definir a senha da entidade de segurança do host.

1. Use o snap-in de **usuários e computadores** do Active Directory para criar uma conta de usuário para um serviço em um computador que não esteja executando o sistema operacional Windows. Por exemplo, crie uma conta com o nome *Usuário1*.

2. Use o comando **ktpass** para configurar um mapeamento de identidade para a conta de usuário digitando:

    ```
    ktpass /princ host/User1.contoso.com@CONTOSO.COM /mapuser User1 /pass MyPas$w0rd /out machine.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set
    ```

    > [!NOTE]
    > Não é possível mapear várias instâncias de serviço para a mesma conta de usuário.

3. Mescle o arquivo. keytab com o arquivo */etc/krb5.keytab* em um computador host que não esteja executando o sistema operacional Windows.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
