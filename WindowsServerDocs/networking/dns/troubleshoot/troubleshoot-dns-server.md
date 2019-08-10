---
title: Solucionando problemas de servidores DNS
description: Este artigo apresenta como solucionar problemas do problema de DNS do lado do servidor.
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: b0547436cfa0f07ba9cbc4e3dd1825f8d33bc093
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917763"
---
# <a name="troubleshooting-dns-servers"></a>Solucionando problemas de servidores DNS

Este artigo discute como solucionar problemas em servidores DNS.

## <a name="check-ip-configuration"></a>Verificar configuração de IP

1. Execute `ipconfig /all` em um prompt de comando e verifique o endereço IP, a máscara de sub-rede e o gateway padrão.

2. Verifique se o servidor DNS é autoritativo para o nome que está sendo pesquisado. Nesse caso, consulte [verificando problemas com dados](#checking-for-problems-with-authoritative-data)autoritativos.

3. Execute o seguinte comando:

   ```cmd
   nslookup <name> <IP address of the DNS server>
   ```
   Por exemplo: 
   ```cmd
   nslookup app1 10.0.0.1
   ```
   Se você receber uma resposta de falha ou de tempo limite, consulte [verificando problemas de recursão](#checking-for-recursion-problems).

4. Libere o cache do resolvedor. Para fazer isso, execute o seguinte comando em uma janela de prompt de comando administrativo:

   ```cmd
   dnscmd /clearcache
   ```
   Ou, em uma janela administrativa do PowerShell, execute o seguinte cmdlet:
   ```powershell
   Clear-DnsServerCache
   ```

5. Repita a etapa 3. 

## <a name="check-dns-server-problems"></a>Verificar problemas do servidor DNS

### <a name="event-log"></a>Log de eventos

Verifique os seguintes logs para ver se há erros gravados:

- Aplicativo

- Sistema

- Servidor DNS

### <a name="test-by-using-nslookup-query"></a>Testar usando a consulta NSLOOKUP

Execute o comando a seguir e verifique se o servidor DNS pode ser acessado de computadores cliente.

```cmd
nslookup <client name> <server IP address>
```

- Se o resolvedor retornar o endereço IP do cliente, o servidor não terá nenhum problema.

- Se o resolvedor retornar uma resposta "falha do servidor" ou "consulta recusada", a zona provavelmente será pausada ou o servidor provavelmente será sobrecarregado. Você pode saber se ele está em pausa verificando a guia Geral das propriedades de zona no console DNS.

Se o resolvedor retornar uma resposta "solicitação ao servidor com tempo limite" ou "sem resposta do servidor", o serviço DNS provavelmente não estará em execução. Tente reiniciar o serviço do servidor DNS digitando o seguinte em um prompt de comando no servidor:

```cmd
net start DNS
```

Se o problema ocorrer quando o serviço estiver em execução, talvez o servidor não esteja escutando no endereço IP que você usou em sua consulta nslookup. Na guia **interfaces** da página Propriedades do servidor no console DNS, os administradores podem restringir um servidor DNS para escutar somente os endereços selecionados. Se o servidor DNS tiver sido configurado para limitar o serviço a uma lista específica de seus endereços IP configurados, é possível que o endereço IP usado para contatar o servidor DNS não esteja na lista. Você pode tentar um endereço IP diferente na lista ou adicionar o endereço IP à lista.

Em casos raros, o servidor DNS pode ter uma configuração de firewall ou segurança avançada. Se o servidor estiver localizado em outra rede acessível somente por meio de um host intermediário (como um roteador de filtragem de pacotes ou servidor proxy), o servidor DNS poderá usar uma porta não padrão para escutar e receber solicitações de clientes. Por padrão, o nslookup envia consultas para servidores DNS na porta UDP 53. Portanto, se o servidor DNS usar qualquer outra porta, as consultas nslookup falharão. Se você considerar que isso pode ser o problema, verifique se um filtro intermediário é intencionalmente usado para bloquear o tráfego em portas DNS conhecidas. Se não estiver, tente modificar os filtros de pacote ou as regras de porta no firewall para permitir o tráfego na porta UDP/TCP 53.

## <a name="checking-for-problems-with-authoritative-data"></a>Verificando problemas com dados autoritativos

Verifique se o servidor que retorna a resposta incorreta é um servidor primário para a zona (o servidor primário padrão para a zona ou um servidor que usa a integração Active Directory para carregar a zona) ou um servidor que hospeda uma cópia secundária da zona. 

### <a name="if-the-server-is-a-primary-server"></a>Se o servidor for um servidor primário

O problema pode ser causado por um erro do usuário quando os usuários inserem dados na zona. Ou pode ser causado por um problema que afeta Active Directory replicação ou atualização dinâmica.

### <a name="if-the-server-is-hosting-a-secondary-copy-of-the-zone"></a>Se o servidor estiver hospedando uma cópia secundária da zona

1. Examine a zona no servidor mestre (o servidor do qual esse servidor efetua pull das transferências de zona).

   > [!NOTE]
   >Você pode determinar qual servidor é o servidor mestre examinando as propriedades da zona secundária no console DNS.

   Se o nome não estiver correto no servidor mestre, vá para a etapa 4.

2. Se o nome estiver correto no servidor mestre, verifique se o número de série no servidor mestre é menor ou igual ao número de série no servidor secundário. Se for, modifique o servidor mestre ou o servidor secundário para que o número de série no servidor mestre seja maior que o número de série no servidor secundário. 
  
3. No servidor secundário, Force uma transferência de zona de dentro do console DNS ou executando o seguinte comando:
  
   ```cmd
   dnscmd /zonerefresh <zone name>
   ```
  
   Por exemplo, se a zona for corp.contoso.com, digite: `dnscmd /zonerefresh corp.contoso.com`.
  
4. Examine o servidor secundário novamente para ver se a zona foi transferida corretamente. Caso contrário, você provavelmente terá um problema de transferência de zona. Para obter mais informações, consulte [problemas de transferência de zona](#zone-transfer-problems).

5. Se a zona foi transferida corretamente, verifique se os dados estão agora corretos. Caso contrário, os dados estão incorretos na zona primária. O problema pode ser causado por um erro do usuário quando os usuários inserem dados na zona. Ou pode ser causado por um problema que afeta Active Directory replicação ou atualização dinâmica.

## <a name="checking-for-recursion-problems"></a>Verificando problemas de recursão

Para que a recursão funcione com êxito, todos os servidores DNS que são usados no caminho de uma consulta recursiva devem ser capazes de responder e encaminhar os dados corretos. Se não puderem, uma consulta recursiva poderá falhar por qualquer um dos seguintes motivos:

- A consulta atinge o tempo limite antes que possa ser concluída.

- Um servidor que é usado durante a consulta não responde.

- Um servidor usado durante a consulta fornece dados incorretos.

Inicie a solução de problemas no servidor que foi usado em sua consulta original. Verifique se esse servidor encaminha as consultas para outro servidor examinando a guia encaminhadores nas propriedades do servidor no console DNS. Se a caixa de seleção **habilitar encaminhadores** estiver marcada e um ou mais servidores estiverem listados, esse servidor encaminhará as consultas.

Se esse servidor encaminhar consultas para outro servidor, verifique se há problemas que afetam o servidor ao qual esse servidor encaminha as consultas. Para verificar se há problemas, consulte [verificar problemas do servidor DNS](#check-dns-server-problems). Quando essa seção instrui você a executar uma tarefa no cliente, em vez disso, execute-a no servidor.

Se o servidor estiver íntegro e puder encaminhar consultas, repita essa etapa e examine o servidor ao qual esse servidor encaminha as consultas.

Se este servidor não encaminhar consultas para outro servidor, teste se este servidor pode consultar um servidor raiz. Para fazer isso, execute o comando:

```cmd
nslookup
server <IP address of server being examined>
set q=NS
 ```

- Se o resolvedor retornar o endereço IP de um servidor raiz, você provavelmente terá uma delegação quebrada entre o servidor raiz e o nome ou endereço IP que você está tentando resolver. Siga o procedimento [testar uma delegação quebrada](#test-a-broken-delegation) para determinar onde você tem uma delegação interrompida.

- Se o resolvedor retornar uma resposta "solicitação ao servidor atingiu o tempo limite", verifique se as dicas de raiz apontam para servidores raiz em funcionamento. Para fazer isso, use o [para exibir o procedimento de dicas de raiz atual](#to-view-the-current-root-hints) . Se as dicas de raiz apontarem para servidores raiz em funcionamento, você poderá ter um problema de rede ou o servidor poderá usar uma configuração de firewall avançada que impeça o resolvedor de consultar o servidor, conforme descrito na seção [verificar problemas do servidor DNS](#check-dns-server-problems) . Também é possível que o padrão de tempo limite recursivo seja muito curto.

### <a name="test-a-broken-delegation"></a>Testar uma delegação interrompida

Inicie os testes no procedimento a seguir consultando um servidor raiz válido. O teste orienta você pelo processo de consulta de todos os servidores DNS da raiz para o servidor que você está testando em uma delegação quebrada.

1. No prompt de comando no servidor que você está testando, insira o seguinte:

   ```cmd
   nslookup
   server <server IP address>
   set norecursion
   set querytype= <resource record type>
   <FQDN>
   ```
   > [!NOTE]
   >Tipo de registro de recurso é o tipo de registro de recurso que você estava consultando em sua consulta original e FQDN é o FQDN para o qual você estava consultando (encerrado por um ponto).
 
2. Se a resposta incluir uma lista de registros de recursos "NS" e "A" para servidores delegados, repita a etapa 1 para cada servidor e use o endereço IP dos registros de recurso "A" como o endereço IP do servidor.

   - Se a resposta não contiver um registro de recurso "NS", você terá uma delegação interrompida.
   
   - Se a resposta contiver registros de recursos "NS", mas nenhum registro de recurso "A", insira **definir recursão**e consulta individualmente para os registros de recurso "a" dos servidores listados nos registros "NS". Se você não encontrar pelo menos um endereço IP válido de um registro de recurso "A" para cada registro de recurso NS em uma zona, você terá uma delegação interrompida.

3. Se você determinar que tem uma delegação quebrada, corrija-a adicionando ou atualizando um registro de recurso "a" na zona pai usando um endereço IP válido para um servidor DNS correto para a zona delegada.

### <a name="to-view-the-current-root-hints"></a>Para exibir as dicas de raiz atuais

1. Inicie o console DNS.

2. Adicione ou conecte-se ao servidor DNS que falhou em uma consulta recursiva.

3. Clique com o botão direito do mouse no servidor e selecione **Propriedades**.

4. Clique em dicas de raiz.

Verifique a conectividade básica com os servidores raiz.

- Se as dicas de raiz parecerem estar configuradas corretamente, verifique se o servidor DNS usado em uma resolução de nome com falha pode executar ping nos servidores raiz pelo endereço IP.

- Se os servidores raiz não responderem ao ping pelo endereço IP, os endereços IP para os servidores raiz poderão ter sido alterados. No entanto, é incomum ver uma reconfiguração de servidores raiz.

## <a name="zone-transfer-problems"></a>Problemas de transferência de zona

Execute as seguintes verificações:

- Verifique Visualizador de Eventos tanto para o servidor DNS primário quanto para o secundário.

- Verifique o servidor mestre para ver se ele está se recusando a enviar a transferência para segurança. 

- Verifique a guia **transferências de zona** das propriedades de zona no console DNS. Se o servidor restringe as transferências de zona para uma lista de servidores, como aqueles listados na guia **servidores de nomes** das propriedades de zona, verifique se o servidor secundário está nessa lista. Verifique se o servidor está configurado para enviar transferências de zona.

- Verifique se há problemas no servidor mestre seguindo as etapas na seção [verificar problemas do servidor DNS](#check-dns-server-problems) . Quando você for solicitado a executar uma tarefa no cliente, execute a tarefa no servidor secundário em vez disso.

- Verifique se o servidor secundário está executando outra implementação de servidor DNS, como BIND. Se for, o problema poderá ter uma das seguintes causas:

  - O servidor mestre do Windows pode ser configurado para enviar transferências de zona rápidas, mas o servidor secundário de terceiros pode não oferecer suporte a transferências de zona rápida. Se esse for o caso, desabilite as transferências de zona rápida no servidor mestre de dentro do console DNS marcando a caixa de seleção **habilitar secundários de associação** na guia **avançado** das propriedades do servidor.

  - Se uma zona de pesquisa direta no Windows Server contiver um tipo de registro (por exemplo, um registro SRV) ao qual o servidor secundário não oferece suporte, o servidor secundário poderá ter problemas para obter a zona.

Verifique se o servidor mestre está executando outra implementação de servidor DNS, como BIND. Nesse caso, é possível que a zona no servidor mestre inclua registros de recursos incompatíveis que o Windows não reconheça.
 
Se o servidor mestre ou secundário estiver executando outra implementação de servidor DNS, verifique ambos os servidores para garantir que eles ofereçam suporte aos mesmos recursos. Você pode verificar o Windows Server no console DNS na guia **avançado** da página Propriedades do servidor. Além da caixa habilitar associações secundárias, essa página inclui a lista suspensa de **verificação de nome** . Isso permite que você selecione a imposição de conformidade RFC estrita para caracteres em nomes DNS.

