---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Examinando conceitos do DNS
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 20192d56aded75f5a178a600067b26c8c23286fb
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938576"
---
# <a name="reviewing-dns-concepts"></a>Examinando conceitos do DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O DNS (sistema de nomes de domínio) é um banco de dados distribuído que representa um namespace. O namespace contém todas as informações necessárias para qualquer cliente procurar qualquer nome. Qualquer servidor DNS pode responder a consultas sobre qualquer nome dentro de seu namespace. Um servidor DNS responde às consultas de uma das seguintes maneiras:

- Se a resposta estiver em seu cache, ela responderá à consulta do cache.
- Se a resposta estiver em uma zona hospedada pelo servidor DNS, ela responderá à consulta de sua zona. Uma zona é uma parte da árvore DNS armazenada em um servidor DNS. Quando um servidor DNS hospeda uma zona, ele é autoritativo para os nomes nessa zona (ou seja, o servidor DNS pode responder a consultas de qualquer nome na zona). Por exemplo, um servidor que hospeda a zona contoso.com pode responder a consultas de qualquer nome em contoso.com.
- Se o servidor não puder responder à consulta de seu cache ou zonas, ele consultará outros servidores para a resposta.

É importante entender os principais recursos do DNS, como delegação, resolução de nomes recursivos e zonas de DNS integradas Active Directory, pois eles têm um impacto direto sobre o design de estrutura lógica Active Directory.

Para obter mais informações sobre DNS e Active Directory Domain Services (AD DS), consulte [DNS e AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).

## <a name="delegation"></a>Delegação

Para que um servidor DNS responda a consultas sobre qualquer nome, ele deve ter um caminho direto ou indireto para cada zona no namespace. Esses caminhos são criados por meio de delegação. Uma delegação é um registro em uma zona pai que lista um servidor de nomes que é autoritativo para a zona no próximo nível da hierarquia. As delegações possibilitam que servidores em uma zona refiram clientes a servidores em outras zonas. A ilustração a seguir mostra um exemplo de delegação.

![Conceitos de DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)

O servidor raiz DNS hospeda a zona raiz representada como um ponto (. ). A zona raiz contém uma delegação para uma zona no próximo nível da hierarquia, a zona com. A delegação na zona raiz informa ao servidor raiz DNS que, para localizar a zona com, deve entrar em contato com o servidor com. Da mesma forma, a delegação na zona com informa ao servidor com que, para localizar a zona contoso.com, deve entrar em contato com o servidor da contoso.

> [!NOTE]
> Uma delegação usa dois tipos de registros. O registro de recurso do servidor de nomes (NS) fornece o nome de um servidor autoritativo. Os registros de recurso de host (A) e host (AAAA) fornecem endereços IP versão 4 (IPv4) e IP versão 6 (IPv6) de um servidor autoritativo.

Esse sistema de zonas e delegações cria uma árvore hierárquica que representa o namespace DNS. Cada zona representa uma camada na hierarquia, e cada delegação representa uma ramificação da árvore.

Usando a hierarquia de zonas e delegações, um servidor raiz DNS pode encontrar qualquer nome no namespace DNS. A zona raiz inclui delegações que levam direta ou indiretamente a todas as outras zonas na hierarquia. Qualquer servidor que possa consultar o servidor raiz DNS pode usar as informações nas delegações para localizar qualquer nome no namespace.

## <a name="recursive-name-resolution"></a>Resolução de nome recursiva

A resolução de nomes recursivas é o processo pelo qual um servidor DNS usa a hierarquia de zonas e delegações para responder a consultas para as quais não é autoritativo.

Em algumas configurações, os servidores DNS incluem dicas de raiz (isto é, uma lista de nomes e endereços IP) que os habilitam a consultar os servidores raiz DNS. Em outras configurações, os servidores encaminham todas as consultas que eles não podem responder a outro servidor. Dicas de encaminhamento e raiz são os dois métodos que os servidores DNS podem usar para resolver consultas para as quais eles não são autoritativos.

### <a name="resolving-names-by-using-root-hints"></a>Resolvendo nomes usando dicas de raiz

As dicas de raiz permitem que qualquer servidor DNS Localize os servidores raiz DNS. Depois que um servidor DNS localiza o servidor raiz DNS, ele pode resolver qualquer consulta para esse namespace. A ilustração a seguir descreve como o DNS resolve um nome usando dicas de raiz.

![Conceitos de DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)

Neste exemplo, ocorrem os seguintes eventos:

1. Um cliente envia uma consulta recursiva a um servidor DNS para solicitar o endereço IP que corresponde ao nome ftp.contoso.com. Uma consulta recursiva indica que o cliente deseja uma resposta definitiva para sua consulta. A resposta para a consulta recursiva deve ser um endereço válido ou uma mensagem indicando que o endereço não pode ser encontrado.
2. Como o servidor DNS não é autoritativo para o nome e não tem a resposta em seu cache, o servidor DNS usa dicas de raiz para localizar o endereço IP do servidor raiz DNS.
3. O servidor DNS usa uma consulta iterativa para solicitar que o servidor raiz DNS resolva o nome ftp.contoso.com. Uma consulta iterativa indica que o servidor aceitará uma referência a outro servidor no lugar de uma resposta definitiva para a consulta. Como o nome ftp.contoso.com termina com o rótulo com, o servidor raiz DNS retorna uma referência ao servidor com que hospeda a zona com.
4. O servidor DNS usa uma consulta iterativa para solicitar que o servidor com resolva o nome ftp.contoso.com. Como o nome ftp.contoso.com termina com o nome contoso.com, o servidor com retorna uma referência ao servidor Contoso que hospeda a zona contoso.com.
5. O servidor DNS usa uma consulta iterativa para solicitar que o servidor Contoso resolva o nome ftp.contoso.com. O servidor Contoso localiza a resposta em seus dados de zona e, em seguida, retorna a resposta para o servidor.
6. Em seguida, o servidor retorna o resultado para o cliente.

### <a name="resolving-names-by-using-forwarding"></a>Resolvendo nomes usando o encaminhamento

O encaminhamento permite que você roteie a resolução de nomes por meio de servidores específicos em vez de usar dicas de raiz. A ilustração a seguir descreve como o DNS resolve um nome usando o encaminhamento.

![Conceitos de DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)

Neste exemplo, ocorrem os seguintes eventos:

1. Um cliente consulta um servidor DNS para o nome ftp.contoso.com.
2. O servidor DNS encaminha a consulta para outro servidor DNS, conhecido como encaminhador.
3. Como o encaminhador não é autoritativo para o nome e não tem a resposta em seu cache, ele usa dicas de raiz para localizar o endereço IP do servidor raiz DNS.
4. O encaminhador usa uma consulta iterativa para solicitar que o servidor raiz DNS resolva o nome ftp.contoso.com. Como o nome ftp.contoso.com termina com o nome com, o servidor raiz DNS retorna uma referência ao servidor com que hospeda a zona com.
5. O encaminhador usa uma consulta iterativa para solicitar que o servidor com resolva o nome ftp.contoso.com. Como o nome ftp.contoso.com termina com o nome contoso.com, o servidor com retorna uma referência ao servidor Contoso que hospeda a zona contoso.com.
6. O encaminhador usa uma consulta iterativa para solicitar que o servidor Contoso resolva o nome ftp.contoso.com. O servidor Contoso localiza a resposta em seus arquivos de zona e, em seguida, retorna a resposta para o servidor.
7. Em seguida, o encaminhador retorna o resultado para o servidor DNS original.
8. O servidor DNS original retorna o resultado para o cliente.
