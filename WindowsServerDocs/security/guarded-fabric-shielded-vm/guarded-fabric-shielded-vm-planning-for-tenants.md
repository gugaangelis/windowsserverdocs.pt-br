---
title: Malha protegida e VM Blindada, guia de planejamento para Hosters
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0a43cedf8ce0138e89624ff3df5bc7088f583252
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875297"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Malha protegida e VM Blindada, guia de planejamento para locatários

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico enfoca os proprietários da VM que deseja proteger suas máquinas virtuais (VMs) para fins de segurança e conformidade. Independentemente se as VMs são executadas em malha protegida de um provedor de hospedagem ou uma malha protegida privada, os proprietários da VM precisam controlar o nível de segurança de suas VMs blindadas, que inclui a manter a capacidade de descriptografá-las se necessário.

Há três áreas a considerar ao usar VMs blindadas:

- O nível de segurança para as VMs
- As chaves de criptografia usadas para protegê-los
- Os dados de blindagem — usadas para criar as informações confidenciais de VMs blindadas 

## <a name="security-level-for-the-vms"></a>Nível de segurança para as VMs

Ao implantar VMs blindadas, um dos dois níveis de segurança deve ser selecionado:

- Blindado 
- Criptografia com suporte

Ambos blindadas e suporte para criptografia de VMs têm um TPM virtual anexado e aquelas que a execução do Windows é protegido pelo BitLocker. A principal diferença é que blindado VMs bloquear o acesso por administradores de malha enquanto VMs com suporte de criptografia permitem administradores da malha de mesmo nível de acesso que teriam a uma VM regular. Para obter mais detalhes sobre essas diferenças, consulte [protegidos malha e blindadas visão geral de VMs](guarded-fabric-and-shielded-vms.md). 

Escolher **VMs Blindadas** se você estiver procurando para proteger a VM de uma malha comprometida (incluindo administradores comprometidos). Eles devem ser usados em ambientes em que os administradores de malha e a estrutura em si não são confiáveis. Escolher **VMs com suporte de criptografia** se você estiver procurando para atender a uma barra de conformidade que pode exigir criptografia em repouso e criptografia da VM durante a transmissão (por exemplo, durante a migração ao vivo).

Suporte para criptografia de VMs são ideais em ambientes em que os administradores de malha são totalmente confiáveis, mas criptografia permanece um requisito.

Você pode executar uma mistura de VMs comuns, as VMs blindadas e suporte para criptografia de VMs em uma malha protegida e até mesmo no mesmo host do Hyper-V. 

Se uma VM está blindada ou suporte para a criptografia é determinado pelos dados de blindagem ao criar a VM selecionada. Os proprietários da VM para configurar o nível de segurança ao criar os dados de blindagem (consulte a [os dados de blindagem](#shielding-data) seção).
Observe que quando essa opção é feita, ela não pode ser alterada enquanto a VM permanece na malha de virtualização.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Chaves criptográficas usadas para as VMs blindadas

VMs blindadas são protegidas contra virtualization fabric vetores de ataque usando discos criptografados e vários outros elementos criptografados que podem ser descriptografados apenas por:

- Uma chave do proprietário – isso é uma chave de criptografia mantida pelo proprietário da VM-que é normalmente usado para recuperação de último recurso ou solução de problemas. Os proprietários da VM são responsáveis por manter as chaves de proprietário em um local seguro.
- Um ou mais guardiões (chaves de guardião de Host) – cada guardião representa uma malha de virtualização em que um proprietário autoriza VMs blindadas para ser executado. As empresas geralmente têm um principal e uma malha de virtualização de DR (recuperação) de desastres e normalmente seriam autorizar seus VMs blindadas para execução em ambos. Em alguns casos, a malha (DR) secundária pode ser hospedada por um provedor de nuvem pública. As chaves privadas para qualquer malha protegida são mantidas somente na malha de virtualização, embora suas chaves públicas podem ser baixados e estão contidos dentro de seus pais. 

**Como criar uma chave de proprietário?** Uma chave de proprietário é representada por dois certificados. Um certificado para criptografia e um certificado para assinar. Você pode criar esses dois certificados usando sua própria infraestrutura PKI ou obter certificados SSL de uma autoridade de certificação pública (CA). Para fins de teste, você também pode criar um certificado autoassinado no início qualquer computador com Windows 10 ou Windows Server 2016.

**Quantas chaves proprietário deve ter?** Você pode usar uma chave única de proprietário ou várias chaves de proprietário. Práticas recomendadas recomendam uma chave única de proprietário para um grupo de VMs que compartilham a mesma segurança, o nível de confiança ou risco e para o controle administrativo. Você pode compartilhar uma chave única de proprietário para todas as suas ingressado no domínio VMs blindadas e efetue a caução de chave proprietário a ser gerenciado pelos administradores do domínio.

**Posso usar minhas próprias chaves de guardião de Host?** Sim, você pode "Traga seu próprio" chave para a hospedagem provedor e o uso que a chave para suas VMs blindadas. Isso permite que você use suas chaves específicas (em vez de usar a chave do provedor de hospedagem) e pode ser usado quando você tiver específicas de segurança ou que você precisa cumprir regulamentos. Para fins de higiene de troca de chave, as chaves de guardião de Host devem ser diferentes do que a chave do proprietário.

## <a name="shielding-data"></a>Os dados de blindagem

Os dados de blindagem contém segredos necessários para implantar VMs blindadas ou suporte para criptografia. Ele também é usado ao converter VMs regulares para VMs blindadas.

Os dados de blindagem é criado usando o Assistente de arquivo de dados de blindagem e é armazenado em arquivos PDK que os proprietários das VM carregar para a malha protegida.

Ajudam a proteger contra ataques de uma malha de virtualização comprometido, portanto, precisamos de um mecanismo seguro para passar dados confidenciais de inicialização, como senha, credenciais de ingresso no domínio ou certificados RDP, o administrador sem revelá-las para VMs blindadas a malha de virtualização ou para seus administradores. Além disso, os dados de blindagem contém o seguinte:

1. Segurança de nível – blindado ou suporte para criptografia
2. Proprietário e a lista de confiáveis guardiões de Host em que a VM pode ser executada
3. Dados de inicialização da máquina virtual (Unattend. XML, o certificado RDP)
4. Lista de discos de modelo assinado confiável para a criação da VM no ambiente de virtualização 

Quando criar uma VM blindada ou suporte para a criptografia ou converter uma VM existente, você será solicitado para selecionar os dados de blindagem, em vez de inserir as informações confidenciais.

**Quantos arquivos de dados de blindagem precisa?** Um único arquivo de dados de blindagem pode ser usado para criar cada VM blindada. Se, no entanto, uma determinada VM blindada requer que qualquer um dos quatro itens sejam diferentes, um arquivo de dados de blindagem adicional é necessário. Por exemplo, você pode ter um arquivo de dados de blindagem para seu departamento de TI e um arquivo de dados de blindagem diferente para o departamento de RH porque sua senha de administrador inicial e certificados RDP eram diferentes.

Embora seja possível usar arquivos de dados de blindagem separado para cada VM blindada, ele não é necessariamente a melhor opção e deve ser feito pelos motivos certos. Por exemplo, se todas as VMs blindadas precisa ter uma senha de administrador diferente, considere em vez de usar o gerenciamento de senhas de serviço ou ferramenta, como [solução de senha de Administrador Local (LAPS) da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Criar uma VM blindada em uma malha de virtualização

Há várias opções para criar uma VM blindada em uma malha de virtualização (a seguir é relevante para VMs blindadas e suporte para a criptografia):

1. Criar uma VM blindada em seu ambiente e carregá-lo na malha de virtualização
2. Criar uma nova VM blindada de um modelo assinado na malha de virtualização
3. Blindagem de uma VM existente (a VM existente deve ser da geração 2 e deve estar executando o Windows Server 2012 ou posterior)

Criando novas VMs de um modelo é a prática normal. No entanto, já que reside o disco de modelo que é usado para criar novas VMs Blindadas na malha de virtualização, as medidas adicionais são necessárias para garantir que ele não foi violado por um administrador de malha mal-intencionados ou malware em execução na malha. Esse problema é resolvido usando discos de modelo assinado — discos de modelo assinado e suas assinaturas de disco são criadas por administradores confiáveis ou o proprietário da VM. Quando uma máquina virtual blindada é criada, assinatura do disco de modelo é comparada com as assinaturas contidas no arquivo de dados de blindagem especificado. Se qualquer uma das assinaturas do arquivo de dados blindagem corresponder a assinatura do disco de modelo, continua o processo de implantação. Se nenhuma correspondência for encontrada, o processo de implantação é interrompido, garantindo que os segredos de VM não serão comprometidos devido a um disco de modelo não confiáveis.

Ao usar discos de modelo assinado para criar VMs blindadas, duas opções estão disponíveis:

1. Use um disco de modelo assinado existente que é fornecido pelo seu provedor de virtualização. Nesse caso, o provedor de virtualização mantém discos de modelo assinado.
2. Carregar um disco de modelo assinado na malha de virtualização. O proprietário da VM é responsável por manter os discos de modelo assinado. 


