---
title: Solução de problemas de clientes DNS
description: Este artigo apresenta como solucionar problemas do problema de DNS do lado do cliente.
manager: dcscontentpm
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 2a9b44807ae6bc9f4c446d4af2150caf09955899
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182332"
---
# <a name="troubleshooting-dns-clients"></a>Solução de problemas de clientes DNS

Este artigo discute como solucionar problemas de clientes DNS.

## <a name="check-ip-configuration"></a>Verificar configuração de IP

1. Abra uma janela de prompt de comando como administrador no computador cliente.

2. Execute o comando a seguir:

   ```cmd
   ipconfig /all
   ```

3. Verifique se o cliente tem um endereço IP, uma máscara de sub-rede e um gateway padrão válidos para a rede à qual ele está conectado e sendo usado.

4. Verifique os servidores DNS listados na saída e verifique se os endereços IP listados estão corretos.

5. Verifique o sufixo DNS específico da conexão na saída e verifique se ele está correto.

Se o cliente não tiver uma configuração de TCP/IP válida, use um dos seguintes métodos:

* Para clientes configurados dinamicamente, use o `ipconfig /renew` comando para forçar manualmente o cliente a renovar sua configuração de endereço IP com o servidor DHCP.

* Para clientes configurados estaticamente, modifique as propriedades de TCP/IP do cliente para usar definições de configuração válidas ou conclua sua configuração de DNS para a rede.

## <a name="check-network-connection"></a>Verificar conexão de rede

### <a name="ping-test"></a>Teste de ping

Verifique se o cliente pode contatar um servidor DNS preferencial (ou alternativo) ao executar ping no servidor DNS preferencial por seu endereço IP.

Por exemplo, se o cliente usar um servidor DNS preferencial de 10.0.0.1, execute este comando em um prompt de comando:

```cmd
ping 10.0.0.1
```

Se nenhum servidor DNS configurado responder a um ping direto de seu endereço IP, isso indica que a origem do problema é mais provável que a conectividade de rede entre o cliente e os servidores DNS. Se esse for o caso, siga as etapas básicas de solução de problemas de rede TCP/IP para corrigir o problema. Tenha em mente que o tráfego ICMP deve ser permitido por meio do firewall para que o comando ping funcione.

### <a name="dns-query-tests"></a>Testes de consulta DNS

Se o cliente DNS puder executar ping no computador do servidor DNS, tente usar os `nslookup` comandos a seguir para testar se o servidor pode responder a clientes DNS. Como o nslookup não usa o cache DNS do cliente, a resolução de nomes usará o servidor DNS configurado do cliente.

#### <a name="test-a-client"></a>Testar um cliente

```cmd
nslookup <client>
```

Por exemplo, se o computador cliente for chamado de **CLIENT1**, execute este comando:

```cmd
nslookup client1
```

Se uma resposta bem-sucedida não for retornada, tente executar o seguinte comando:

```cmd
nslookup <fqdn of client>
```

Por exemplo, se o FQDN for **CLIENT1.Corp.contoso.com**, execute este comando:

```cmd
nslookup client1.corp.contoso.com.
```

> [!NOTE]
> Você deve incluir o ponto à direita ao executar esse teste.

Se o Windows encontrar o FQDN com êxito, mas não conseguir encontrar o nome curto, verifique a configuração do sufixo DNS na guia DNS das configurações avançadas de TCP/IP da NIC. Para obter mais informações, consulte [Configuring DNS Resolution](/previous-versions/tn-archive/dd163570(v=technet.10)#configuring-dns-resolution).

#### <a name="test-the-dns-server"></a>Testar o servidor DNS

```cmd
nslookup <DNS Server>
```

Por exemplo, se o servidor DNS for denominado DC1, execute este comando:

```cmd
nslookup dc1
```
Se os testes anteriores tiverem sido bem-sucedidos, esse teste também deverá ser bem-sucedido. Se esse teste não for bem-sucedido, verifique a conectividade com o servidor DNS.

#### <a name="test-the-failing-record"></a>Testar o registro com falha

```cmd
nslookup <failed internal record>
```

Por exemplo, se o registro com falha for **App1.Corp.contoso.com**, execute este comando:

```cmd
nslookup app1.corp.contoso.com
```

#### <a name="test-a-public-internet-address"></a>Testar um endereço de Internet público

```cmd
nslookup <external name>
```

Por exemplo:
```cmd
nslookup bing.com
```

Se todos os quatro testes tiverem sido bem-sucedidos, execute `ipconfig /displaydns` e verifique a saída do nome que falhou. Se você vir "o nome não existe" sob o nome com falha, uma resposta negativa foi retornada de um servidor DNS e armazenada em cache no cliente.

Para resolver o problema, limpe o cache executando `ipconfig /flushdns` .

## <a name="next-step"></a>Próxima etapa

Se a resolução de nomes ainda estiver falhando, vá para a seção [Solucionando problemas de servidores DNS](troubleshoot-dns-server.md) .
