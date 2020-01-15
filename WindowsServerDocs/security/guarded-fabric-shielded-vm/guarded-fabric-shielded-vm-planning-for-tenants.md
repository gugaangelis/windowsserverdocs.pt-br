---
title: Malha protegida e guia de planejamento de VM blindada para hosters
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.openlocfilehash: 255429960735d70ff3a4d260bd9090b95882b6bd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949778"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Malha protegida e guia de planejamento de VM blindada para locatários

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico se concentra em proprietários de VM que desejam proteger suas VMs (máquinas virtuais) para fins de conformidade e segurança. Independentemente de as VMs serem executadas em uma malha protegida do provedor de hospedagem ou em uma malha protegida privada, os proprietários da VM precisam controlar o nível de segurança de suas VMs blindadas, o que inclui a manutenção da capacidade de descriptografá-las, se necessário.

Há três áreas a serem consideradas ao usar VMs blindadas:

- O nível de segurança para as VMs
- As chaves de criptografia usadas para protegê-las
- Blindagem de dados — informações confidenciais usadas para criar VMs blindadas 

## <a name="security-level-for-the-vms"></a>Nível de segurança para as VMs

Ao implantar VMs blindadas, um dos dois níveis de segurança deve ser selecionado:

- Blindadas 
- Criptografia com suporte

As VMs blindadas e com suporte à criptografia têm um TPM virtual anexado a elas e aquelas que executam o Windows são protegidas pelo BitLocker. A principal diferença é que as VMs blindadas bloqueiam o acesso por administradores de malha enquanto as VMs com suporte à criptografia permitem que os administradores de malha tenham o mesmo nível de acesso que teriam em uma VM regular. Para obter mais detalhes sobre essas diferenças, consulte [visão geral de malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms.md). 

Escolha **VMs blindadas** se você estiver procurando proteger a VM de uma malha comprometida (incluindo administradores comprometidos). Eles devem ser usados em ambientes em que os administradores de malha e a própria malha não são confiáveis. Escolha **criptografia VMs com suporte** se você estiver procurando atender a uma barra de conformidade que possa exigir criptografia em repouso e criptografia da VM na conexão (por exemplo, durante a migração ao vivo).

As VMs com suporte de criptografia são ideais em ambientes em que os administradores de malha são totalmente confiáveis, mas a criptografia continua sendo um requisito.

Você pode executar uma mistura de VMs regulares, VMs blindadas e VMs com suporte à criptografia em uma malha protegida e até mesmo no mesmo host do Hyper-V. 

Se uma VM é blindada ou com suporte para criptografia é determinada pelos dados de blindagem selecionados durante a criação da VM. Os proprietários da VM configuram o nível de segurança ao criar os dados de blindagem (consulte a seção [dados de blindagem](#shielding-data) ).
Observe que, depois que essa opção for feita, ela não poderá ser alterada enquanto a VM permanecer na malha de virtualização.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Chaves de criptografia usadas para VMs blindadas

As VMs blindadas são protegidas de vetores de ataque de malha de virtualização usando discos criptografados e vários outros elementos criptografados que só podem ser descriptografados pelo:

- Uma chave de proprietário – essa é uma chave criptográfica mantida pelo proprietário da VM que normalmente é usada para recuperação ou solução de problemas de última resolução. Os proprietários da VM são responsáveis por manter as chaves do proprietário em um local seguro.
- Um ou mais Guardiões (chaves do guardião de host) – cada guardião representa uma malha de virtualização na qual um proprietário autoriza VMs blindadas a serem executadas. As empresas geralmente têm uma malha de virtualização de DR (recuperação de desastres) e primária e, normalmente, autorizariam que suas VMs blindadas fossem executadas em ambos. Em alguns casos, a malha secundária (DR) pode ser hospedada por um provedor de nuvem pública. As chaves privadas de qualquer malha protegida são mantidas somente na malha de virtualização, enquanto suas chaves públicas podem ser baixadas e estão contidas em seu guardião. 

**Como fazer criar uma chave de proprietário?** Uma chave de proprietário é representada por dois certificados. Um certificado para criptografia e um certificado para assinatura. Você pode criar esses dois certificados usando sua própria infraestrutura PKI ou obter certificados SSL de uma autoridade de certificação pública (CA). Para fins de teste, você também pode criar um certificado autoassinado em qualquer computador que comece com o Windows 10 ou o Windows Server 2016.

**Quantas chaves do proprietário você deve ter?** Você pode usar uma única chave de proprietário ou várias chaves de proprietário. As práticas recomendadas recomendam uma chave de proprietário único para um grupo de VMs que compartilham o mesmo nível de segurança, de confiança ou de risco e para o controle administrativo. Você pode compartilhar uma única chave de proprietário para todas as suas VMs blindadas ingressadas no domínio e caução que a chave de proprietário seja gerenciada pelos administradores de domínio.

**Posso usar minhas próprias chaves para o guardião de host?** Sim, você pode "Traga sua própria chave" para o provedor de hospedagem e usar essa chave para suas VMs blindadas. Isso permite que você use suas chaves específicas (vs. usando a chave do provedor de hospedagem) e pode ser usado quando você tem uma segurança específica ou regulamentos que você precisa obedecer. Para fins de higiene de chave, as chaves do guardião de host devem ser diferentes da chave do proprietário.

## <a name="shielding-data"></a>Dados de blindagem

Os dados de blindagem contêm os segredos necessários para implantar VMs blindadas ou com suporte de criptografia. Ele também é usado ao converter VMs regulares em VMs blindadas.

Os dados de blindagem são criados usando o assistente de arquivo de dados de blindagem e são armazenados em arquivos PDK que os proprietários da VM carregam na malha protegida.

As VMs blindadas ajudam a proteger contra ataques de uma malha de virtualização comprometida, portanto, precisamos de um mecanismo seguro para passar dados de inicialização confidenciais, como a senha do administrador, as credenciais de ingresso no domínio ou os certificados RDP, sem revela-los para a malha de virtualização em si ou para seus administradores. Além disso, os dados de blindagem contêm o seguinte:

1. Nível de segurança – com suporte blindado ou criptografia
2. Proprietário e lista de guardiões de host confiáveis em que a VM pode ser executada
3. Dados de inicialização da máquina virtual (Unattend. xml, certificado RDP)
4. Lista de discos de modelo assinados confiáveis para criar a VM no ambiente de virtualização 

Ao criar uma VM blindada ou com suporte de criptografia ou converter uma VM existente, você será solicitado a selecionar os dados de blindagem em vez de serem solicitados a fornecer as informações confidenciais.

**De quantos arquivos de dados de blindagem eu preciso?** Um único arquivo de dados de blindagem pode ser usado para criar cada VM blindada. Se, no entanto, uma determinada VM blindada exigir que qualquer um dos quatro itens seja diferente, um arquivo de dados de blindagem adicional será necessário. Por exemplo, você pode ter um arquivo de dados de blindagem para seu departamento de ti e um arquivo de dados de blindagem diferente para o departamento de RH porque sua senha de administrador inicial e certificados RDP diferem.

Embora seja possível usar arquivos de dados de blindagem separados para cada VM blindada, ela não é necessariamente a opção ideal e deve ser feita por motivos certos. Por exemplo, se cada VM blindada precisar ter uma senha de administrador diferente, considere usar um serviço de gerenciamento de senha ou uma ferramenta como a [solução de senha de administrador local da Microsoft (LAPs)](https://www.microsoft.com/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Criando uma VM blindada em uma malha de virtualização

Há várias opções para criar uma VM blindada em uma malha de virtualização (o seguinte é relevante para VMs blindadas e com suporte para criptografia):

1. Criar uma VM blindada em seu ambiente e carregá-la na malha de virtualização
2. Criar uma nova VM blindada com base em um modelo assinado na malha de virtualização
3. Blindar uma VM existente (a VM existente deve ser a geração 2 e deve estar executando o Windows Server 2012 ou posterior)

A criação de novas VMs a partir de um modelo é uma prática normal. No entanto, como o disco de modelo que é usado para criar uma nova VM blindada reside na malha de virtualização, são necessárias medidas adicionais para garantir que elas não tenham sido adulteradas por um administrador de malha mal-intencionado ou por malware em execução na malha. Esse problema é resolvido usando discos de modelo assinados – discos de modelo assinados e suas assinaturas de disco são criadas por administradores confiáveis ou pelo proprietário da VM. Quando uma VM blindada é criada, a assinatura do disco do modelo é comparada com as assinaturas contidas no arquivo de dados de blindagem especificado. Se qualquer uma das assinaturas do arquivo de dados de blindagem corresponder à assinatura do disco do modelo, o processo de implantação continuará. Se nenhuma correspondência puder ser encontrada, o processo de implantação será anulado, garantindo que os segredos da VM não serão comprometidos devido a um disco de modelo não confiável.

Ao usar discos de modelo assinados para criar VMs blindadas, duas opções estão disponíveis:

1. Use um disco de modelo assinado existente que seja fornecido pelo seu provedor de virtualização. Nesse caso, o provedor de virtualização mantém os discos de modelo assinados.
2. Carregue um disco de modelo assinado na malha de virtualização. O proprietário da VM é responsável por manter os discos de modelo assinados. 


