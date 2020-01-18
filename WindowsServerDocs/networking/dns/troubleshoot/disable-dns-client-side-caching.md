---
title: Desabilitar o cache do lado do cliente DNS em clientes DNS
description: Este artigo apresenta como desabilitar o cache do lado do cliente DNS em clientes DNS.
manager: dcscontentpm
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 51a9dbfd05402a9d018aec3bfea8a5c89e9e5d5e
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265838"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>Desabilitar o cache do lado do cliente DNS em clientes DNS

O Windows contém um cache DNS do lado do cliente. O recurso de cache DNS do lado do cliente pode gerar uma falsa impressão de que o balanceamento de carga do DNS "round robin" não está ocorrendo do servidor DNS para o computador cliente do Windows. Quando você usa o comando ping para pesquisar o mesmo nome de domínio de registro a, o cliente pode usar o mesmo endereço IP.  

## <a name="how-to-disable-client-side-caching"></a>Como desabilitar o armazenamento em cache do lado do cliente

Para parar o cache DNS, execute um dos seguintes comandos:

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


Para desabilitar o cache DNS permanentemente no Windows, use a ferramenta do controlador de serviço ou a ferramenta serviços para definir o tipo de inicialização do serviço cliente DNS como **desabilitado**. Observe que o nome do serviço cliente DNS do Windows também pode aparecer como "dnscache". 

> [!NOTE]
> Se o cache do resolvedor de DNS for desativado, o desempenho geral do computador cliente diminuirá e o tráfego de rede para consultas DNS aumentará. 

O serviço cliente DNS otimiza o desempenho da resolução de nomes DNS armazenando nomes anteriormente resolvidos na memória. Se o serviço cliente DNS estiver desativado, o computador ainda poderá resolver nomes DNS usando os servidores DNS da rede. 

Quando o resolvedor do Windows recebe uma resposta, positiva ou negativa, para uma consulta, ele adiciona essa resposta ao seu cache e, portanto, cria um registro de recurso DNS. O resolvedor sempre verifica o cache antes de consultar qualquer servidor DNS. Se um registro de recurso DNS estiver no cache, o resolvedor usará o registro do cache em vez de consultar um servidor. Esse comportamento acelera as consultas e diminui o tráfego de rede para consultas DNS. 

Você pode usar a ferramenta ipconfig para exibir e liberar o cache do resolvedor de DNS. Para exibir o cache do resolvedor de DNS, execute o seguinte comando no prompt de comando:

```cmd
ipconfig /displaydns 
```

Esse comando exibe o conteúdo do cache do resolvedor de DNS, incluindo os registros de recursos de DNS pré-carregados do arquivo de hosts e quaisquer nomes consultados recentemente que foram resolvidos pelo sistema. Depois de algum tempo, o resolvedor descarta o registro do cache. O período de tempo é especificado pelo valor **TTL (vida útil)** associado ao registro de recurso DNS. Você também pode liberar o cache manualmente. Depois de liberar o cache, o computador deve consultar os servidores DNS novamente para todos os registros de recursos DNS que foram resolvidos anteriormente pelo computador. Para excluir as entradas no cache do resolvedor de DNS, execute `ipconfig /flushdns` em um prompt de comando.

## <a name="using-the-registry-to-control-the-caching-time"></a>Usando o registro para controlar o tempo de cache

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/help/322756) em caso de problemas.

O período de tempo para o qual uma resposta positiva ou negativa é armazenada em cache depende dos valores das entradas na seguinte chave do registro:

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DNSCache\Parameters**

O TTL para respostas positivas é o menor dos seguintes valores: 

- O número de segundos especificado na resposta de consulta que o resolvedor recebeu

- O valor da configuração do registro **MaxCacheTtl** .

>[!Note]
>- O TTL padrão para respostas positivas é de 86.400 segundos (1 dia).
>- O TTL para respostas negativas é o número de segundos especificado na configuração do registro MaxNegativeCacheTtl.
>- O TTL padrão para respostas negativas é de 900 segundos (15 minutos).
Se você não quiser que respostas negativas sejam armazenadas em cache, defina a configuração do registro MaxNegativeCacheTtl como 0.

Para definir o tempo de cache em um computador cliente:

1. Inicie o editor do registro (regedit. exe).

2. Localize e clique na seguinte chave no registro:

   **HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\Dnscache\Parameters**

3. No menu Editar, aponte para novo, clique em valor DWORD e adicione os seguintes valores de registro:

   - Nome do valor: MaxCacheTtl

     Tipo de dados: REG_DWORD

     Dados do valor: valor padrão de 86400 segundos. 
     
     Se você reduzir o valor máximo de TTL no cache DNS do cliente para 1 segundo, isso dará a aparência de que o cache DNS do lado do cliente foi desabilitado.    

   - Nome do valor: MaxNegativeCacheTtl

     Tipo de dados: REG_DWORD

     Dados do valor: valor padrão de 900 segundos. 
     
     Defina o valor como 0 se você não quiser que respostas negativas sejam armazenadas em cache.

4. Digite o valor que você deseja usar e clique em OK.

5. Feche o Editor do Registro.