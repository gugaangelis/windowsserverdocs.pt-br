---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Examinando conceitos do DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d077ecc30caca9b8f3b382af624121fec729afae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890497"
---
# <a name="reviewing-dns-concepts"></a>Examinando conceitos do DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema de nome de domínio (DNS) é um banco de dados distribuído que representa um namespace. O namespace contém todas as informações necessárias para qualquer cliente pesquisar qualquer nome. Qualquer servidor DNS pode responder a consultas sobre qualquer nome dentro de seu namespace. Um servidor DNS responde a consultas em uma das seguintes maneiras:  
  
- Se a resposta está no seu cache, ele responde à consulta do cache.  
- Se a resposta estiver em uma zona hospedada pelo servidor DNS, ele responde à consulta de sua zona. Uma zona é uma parte da árvore DNS armazenado em um servidor DNS. Quando um servidor DNS hospeda uma zona, ele tem autoridade para os nomes de zona (ou seja, o servidor DNS pode responder a consultas para qualquer nome de zona). Por exemplo, um servidor que hospeda a zona contoso.com pode responder a consultas para qualquer nome em contoso.com.  
- Se o servidor não pode responder à consulta do seu cache ou de zonas, ele consulta os outros servidores para a resposta.  

É importante entender os principais recursos do DNS, como a delegação, resolução de nomes recursivos e zonas DNS integradas ao Active Directory, porque eles têm um impacto direto no seu design de estrutura lógica do Active Directory.  
  
Para obter mais informações sobre DNS e os serviços de domínio do Active Directory (AD DS), consulte [DNS e AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Delegação

Para um servidor DNS Responder consultas sobre qualquer nome, ele deve ter um caminho direto ou indireto para cada zona no namespace. Esses caminhos são criados por meio de delegação. Uma delegação é um registro em uma zona pai que lista a um servidor de nomes autoritativo para a zona no próximo nível da hierarquia. As delegações tornam possível para servidores em uma zona para referir-se a clientes para servidores em outras zonas. A ilustração a seguir mostra um exemplo de delegação.  
  
![Conceitos DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
O servidor DNS raiz hospeda a zona raiz representada como um ponto (. ). A zona raiz contém uma delegação para uma zona no próximo nível da hierarquia, a zona de com. A delegação de zona raiz informa o servidor DNS raiz, que, para localizar a zona de com, ele deve contatar o servidor Com. Da mesma forma, a delegação de zona com informa ao servidor Com que, para localizar a zona contoso.com, ele deve contatar o servidor da Contoso.  
  
> [!NOTE]  
> Uma delegação usa dois tipos de registros. O registro de recurso de NS (servidor) de nome fornece o nome de um servidor autoritativo. Host (A) e registros de recursos de host (AAAA) fornecem IP versão 4 (IPv4) e IP versão 6 (IPv6) endereços de um servidor autoritativo.  
  
Este sistema de zonas e delegações cria uma árvore hierárquica que representa o namespace do DNS. Cada zona representa uma camada na hierarquia e, cada delegação representa uma ramificação da árvore.  
  
Usando a hierarquia de zonas e delegações, um servidor DNS raiz pode encontrar qualquer nome do namespace DNS. A zona raiz inclui as delegações que levam direta ou indiretamente a todas as outras regiões na hierarquia. Qualquer servidor que pode consultar o servidor DNS raiz pode usar as informações nas delegações para localizar qualquer nome no namespace.  
  
## <a name="recursive-name-resolution"></a>Resolução de nomes recursivos

Resolução de nomes recursivos é o processo pelo qual um servidor DNS usa a hierarquia de zonas e delegações para responder a consultas para o qual não está autorizado.  
  
Em algumas configurações, servidores DNS incluem dicas de raiz (ou seja, uma lista de nomes e endereços IP) que permitem que os servidores DNS raiz de consulta. Em outras configurações, os servidores encaminhar todas as consultas que eles não podem responder a outro servidor. Dicas de raiz e encaminhamento são ambos os métodos que os servidores DNS podem usar para resolver consultas para os quais eles não sejam autoritativos.  
  
### <a name="resolving-names-by-using-root-hints"></a>Resolvendo nomes usando dicas de raiz

Dicas de raízes permitem que os servidores DNS localizar os servidores DNS raiz. Depois que um servidor DNS localiza o servidor DNS raiz, ele pode resolver qualquer consulta para esse namespace. A ilustração a seguir descreve como o DNS resolve um nome usando as dicas de raiz.  
  
![Conceitos DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
Neste exemplo, os seguintes eventos ocorrem:  
  
1. Um cliente envia uma consulta recursiva para um servidor DNS para o endereço IP que corresponde ao ftp.contoso.com nome de solicitação. Uma consulta recursiva indica que o cliente deseja uma resposta definitiva para a sua consulta. A resposta para a consulta recursiva deve ser um endereço válido ou uma mensagem indicando que o endereço não pode ser encontrado.  
2. Como o servidor DNS não está autorizado para o nome e não tem a resposta em seu cache, o servidor DNS usa as dicas de raiz para localizar o endereço IP do servidor DNS raiz.  
3. O servidor DNS usa uma consulta iterativa para perguntar ao servidor DNS raiz para resolver o nome ftp.contoso.com. Uma consulta iterativa indica que o servidor aceitará uma referência a outro servidor no lugar de uma resposta definitiva para a consulta. Como o nome ftp.contoso.com termina com o rótulo com, o servidor DNS raiz retorna uma referência para o servidor Com que hospeda a zona de com.  
4. O servidor DNS usa uma consulta iterativa para perguntar ao servidor para resolver o nome ftp.contoso.com Com. Como o nome ftp.contoso.com termina com o nome contoso.com, o servidor Com retorna uma referência para o servidor da Contoso que hospeda a zona contoso.com.  
5. O servidor DNS usa uma consulta iterativa para perguntar ao servidor para resolver o nome ftp.contoso.com Contoso. O servidor Contoso localiza a resposta em seus dados de zona e, em seguida, retorna a resposta do servidor.  
6. O servidor, em seguida, retorna o resultado para o cliente.  
  
### <a name="resolving-names-by-using-forwarding"></a>Resolvendo nomes usando encaminhamento

Encaminhamento permite a resolução de nome de rota por meio de servidores específicos em vez de usar as dicas de raiz. A ilustração a seguir descreve como o DNS resolve um nome usando o encaminhamento.  
  
![Conceitos DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
Neste exemplo, os seguintes eventos ocorrem:  
  
1. Um cliente consulta um servidor DNS para o nome ftp.contoso.com.  
2. O servidor DNS encaminha a consulta para outro servidor DNS, conhecido como um encaminhador.  
3. Como o encaminhador não está autorizado para o nome e não tem a resposta em seu cache, ele usa as dicas de raiz para localizar o endereço IP do servidor DNS raiz.  
4. O encaminhador usa uma consulta iterativa para perguntar ao servidor DNS raiz para resolver o nome ftp.contoso.com. Como o ftp.contoso.com nome termina com o nome com, o servidor DNS raiz retorna uma referência para o servidor Com que hospeda a zona de com.  
5. O encaminhador usa uma consulta iterativa para solicitar que o servidor de Com para resolver o nome ftp.contoso.com. Como o nome ftp.contoso.com termina com o nome contoso.com, o servidor Com retorna uma referência para o servidor da Contoso que hospeda a zona contoso.com.  
6. O encaminhador usa uma consulta iterativa para perguntar ao servidor para resolver o nome ftp.contoso.com Contoso. O servidor Contoso localiza a resposta em seus arquivos de zona e, em seguida, retorna a resposta do servidor.  
7. O encaminhador, em seguida, retorna o resultado para o servidor DNS original.  
8. O servidor DNS original, em seguida, retorna o resultado para o cliente.  
