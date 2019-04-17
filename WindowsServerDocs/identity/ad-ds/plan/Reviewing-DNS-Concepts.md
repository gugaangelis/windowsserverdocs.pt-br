---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Revisando os conceitos do DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>Revisando os conceitos do DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema de nome de domínio (DNS) é um banco de dados distribuído que representa um namespace. O namespace contém todas as informações necessárias para qualquer cliente pesquisar qualquer nome. Qualquer servidor DNS pode responder a consultas sobre qualquer nome dentro de seu namespace. Um servidor DNS responde consultas em uma das seguintes maneiras:  
  
-   Se a resposta for no cache, ele responde à consulta do cache.  
  
-   Se a resposta está em uma zona hospedada pelo servidor DNS, ele responde à consulta da sua região. Uma zona é uma parte da árvore DNS armazenado em um servidor DNS. Quando um servidor DNS hospeda uma zona, é autoritativo para os nomes nessa região (ou seja, o servidor DNS pode responder a consultas para qualquer nome na zona). Por exemplo, um servidor que hospeda a zona contoso.com pode responder a consultas para qualquer nome no contoso.com.  
  
-   Se o servidor não pode responder a consulta da cache ou zonas, ele consulta outros servidores para a resposta.  
  
É importante entender os principais recursos do DNS, como delegação, resolução recursiva de nomes e zonas DNS integradas ao Active Directory, porque eles têm um impacto direto sobre seu design de estrutura lógica do Active Directory.  
  
Para obter mais informações sobre DNS e os serviços de domínio do Active Directory (AD DS), consulte [DNS e o AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Delegação  
Para um servidor DNS responder a consultas sobre qualquer nome, ele deve ter um caminho direto ou indireto para cada região no namespace. Esses caminhos são criados por meio de delegação. Uma delegação é um registro em uma zona pai que um servidor de nomes autoritativo para a zona no próximo nível da hierarquia de lista. Delegações tornam possível para os servidores em uma região para consultar os clientes de servidores em outras zonas. A ilustração a seguir mostra um exemplo de delegação.  
  
![Conceitos DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
O servidor DNS raiz hospeda a zona de raiz representada como um ponto (. ). A zona de raiz contém uma delegação para uma zona no próximo nível da hierarquia, a zona com. A delegação na zona da raiz informa o servidor DNS raiz que, para localizar a zona de com, ele deve entrar em contato com o servidor Com. Da mesma forma, a delegação na zona com informa o servidor Com isso, para localizar a zona contoso.com, ele deve entrar em contato com o servidor da Contoso.  
  
> [!NOTE]  
> Uma delegação usa dois tipos de registros. O registro de recurso de servidor (NS) de nome fornece o nome de um servidor de autorização. Host (A) e registros de recurso de host (AAAA) fornecem IP versão 4 (IPv4) e protocolo IP versão 6 (IPv6) endereços de um servidor de autorização.  
  
Esse sistema de zonas e delegações cria uma árvore hierárquica que representa o namespace DNS. Cada zona representa uma camada na hierarquia, e cada delegação representa uma ramificação da árvore.  
  
Usando a hierarquia das zonas e delegações, um servidor DNS raiz pode encontrar qualquer nome no namespace DNS. A zona raiz inclui delegações que levam direta ou indiretamente para todas as outras regiões na hierarquia. Qualquer servidor que pode consultar o servidor DNS raiz pode usar as informações nas delegações para encontrar qualquer nome no namespace.  
  
## <a name="recursive-name-resolution"></a>Resolução recursiva de nomes  
A resolução recursiva nome é o processo pelo qual um servidor DNS usa a hierarquia das zonas e delegações para responder a consultas para o qual não é autoritativo.  
  
Em algumas configurações, servidores DNS incluem as dicas de raiz (ou seja, uma lista de nomes e endereços IP) que habilitá-los consultar dos servidores DNS raiz. Em outras configurações, servidores encaminham todas as consultas que eles não podem responder ao outro servidor. Dicas de encaminhamento e raiz são ambos os métodos que podem usar servidores DNS para resolver consultas para o qual ele não tem autorização.  
  
### <a name="resolving-names-by-using-root-hints"></a>Resolvendo nomes usando dicas de raiz  
Dicas de raiz permitem que os servidores DNS localizar os servidores de raiz DNS. Depois que um servidor DNS localiza o servidor DNS raiz, ele pode resolver qualquer consulta para esse namespace. A ilustração a seguir descreve como o DNS resolve um nome usando dicas de raiz.  
  
![Conceitos DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
Neste exemplo, os seguintes eventos ocorrem:  
  
1.  Um cliente envia uma consulta recursiva para um servidor DNS para solicitar o endereço IP que corresponde ao nome do ftp.contoso.com. Uma consulta recursiva indica que o cliente quer uma resposta definitiva para sua consulta. A resposta para a consulta recursiva deve ser um endereço válido ou uma mensagem indicando que o endereço não foi encontrado.  
  
2.  Como o servidor DNS não está autorizado para o nome e não tiver a resposta em seu cache, o servidor DNS usa as dicas de raiz para localizar o endereço IP do servidor DNS raiz.  
  
3.  O servidor DNS usa uma consulta iterativa para solicitar que o servidor DNS raiz para resolver o nome ftp.contoso.com. Uma consulta iterativa indica que o servidor aceitará uma referência ao outro servidor no lugar de uma resposta definitiva para a consulta. Como o nome ftp.contoso.com termina com o rótulo com, o servidor DNS raiz retorna uma referência para o servidor Com que hospeda a zona com.  
  
4.  O servidor DNS usa uma consulta iterativa para solicitar que o servidor Com para resolver o nome ftp.contoso.com. Porque o nome ftp.contoso.com termina com o nome contoso.com, o servidor Com retorna uma referência para o servidor da Contoso zona que hospeda o contoso.com.  
  
5.  O servidor DNS usa uma consulta iterativa para solicitar que o servidor da Contoso para resolver o nome ftp.contoso.com. O servidor da Contoso localiza a resposta em seus dados de zona e, em seguida, retorna a resposta do servidor.  
  
6.  O servidor, em seguida, retorna o resultado para o cliente.  
  
### <a name="resolving-names-by-using-forwarding"></a>Resolvendo nomes usando encaminhamento  
Encaminhamento permite a resolução de nome de rota através de servidores específicos em vez de usar dicas de raiz. A ilustração a seguir descreve como o DNS resolve um nome usando o encaminhamento.  
  
![Conceitos DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
Neste exemplo, os seguintes eventos ocorrem:  
  
1.  Um cliente consulta um servidor DNS para o nome ftp.contoso.com.  
  
2.  O servidor DNS encaminha a consulta para outro servidor DNS, conhecido como um encaminhador.  
  
3.  Como o encaminhador não está autorizado para o nome e não tiver a resposta em seu cache, ele usa as dicas de raiz para localizar o endereço IP do servidor DNS raiz.  
  
4.  O encaminhador usa uma consulta iterativa para solicitar que o servidor DNS raiz para resolver o nome ftp.contoso.com. Como o nome ftp.contoso.com termina com o nome com, o servidor DNS raiz retorna uma referência para o servidor Com que hospeda a zona com.  
  
5.  O encaminhador usa uma consulta iterativa para solicitar que o servidor Com para resolver o nome ftp.contoso.com. Porque o nome ftp.contoso.com termina com o nome contoso.com, o servidor Com retorna uma referência para o servidor da Contoso zona que hospeda o contoso.com.  
  
6.  O encaminhador usa uma consulta iterativa para solicitar que o servidor da Contoso para resolver o nome ftp.contoso.com. O servidor da Contoso encontra a resposta em seus arquivos de zona e, em seguida, retorna a resposta do servidor.  
  
7.  O encaminhador, em seguida, retorna o resultado para o servidor DNS original.  
  
8.  O servidor DNS original, em seguida, retorna o resultado para o cliente.  
  


