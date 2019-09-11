---
title: Solução de problemas AD FS-certificados
description: Este documento descreve os problemas de certificado típicos.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cee87ce864e333b98e92fa64e939f2ead7edc156
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869193"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Solução de problemas AD FS-certificados
AD FS requer os seguintes certificados para funcionar corretamente.  Se qualquer um deles não tiver sido instalado ou configurado corretamente, os problemas podem surgir.  

## <a name="required-certificates"></a>Certificados necessários
AD FS requer os seguintes certificados:



- **Confiança de Federação** – isso requer que um certificado encadeado para uma AC (autoridade de certificação) raiz da Internet mutuamente confiável esteja presente no repositório de raiz confiável do provedor de declarações e dos servidores de Federação de terceira parte confiável, um o design de certificação cruzada foi implementado na qual cada lado trocou sua CA raiz com seu parceiro ou certificados autoassinados que foram importados em cada lado, quando apropriado.
- **Assinatura de token** – cada computador serviço de Federação requer um certificado de autenticação de tokens.  O certificado de autenticação de tokens do provedor de declarações deve ser confiável pelo servidor de Federação de terceira parte confiável. O certificado de autenticação de tokens de terceira parte confiável deve ser confiável para todos os aplicativos que recebem tokens do servidor de Federação RP.
- **Protocolo SSL (SSL)** – o certificado SSL para o serviço de Federação deve estar presente em um repositório confiável no computador proxy do servidor de Federação e tem uma cadeia válida para um repositório de AC (autoridade de certificação) confiável.
- **CRL (lista de certificados revogados)** – para qualquer certificado que tenha uma CRL publicada, a CRL deve estar acessível a todos os clientes e servidores que precisam acessar o certificado.

Se qualquer um dos itens acima não estiver configurado corretamente, AD FS não funcionará.

## <a name="common-things-to-check-with-certificates"></a>Coisas comuns para verificar com certificados
Veja a seguir uma lista de coisas que podem surgir e que devem ser verificadas ao tentar resolver um problema de certificado.

- Verifique se o certificado é confiável
    - Os certificados SSL precisam ser confiáveis pelos clientes
    - Os certificados de autenticação de tokens precisam ser confiáveis para as partes confiantes
- Verifique a cadeia de confiança-cada certificado na cadeia precisa ser válido.
- Verificar a data de validade do certificado
- Verificar a acessibilidade da CRL (lista de certificados revogados)
    - Verifique se o campo CDP está preenchido
    - Navegue manualmente até a CDP
- Verifique se o certificado não foi revogado

## <a name="common-certificate-errors"></a>Erros comuns de certificado
A tabela a seguir é uma lista de erros comuns e possíveis causas.

|evento|Causa|Resolução
|-----|-----|-----|
|Evento 249-não foi possível encontrar um certificado no repositório de certificados. Em cenários de substituição de certificado, isso pode potencialmente causar uma falha quando a Serviço de Federação está assinando ou descriptografando usando esse certificado.|O certificado em questão não está presente no repositório de certificados local ou a conta de serviço não tem permissão para a chave privada do certificado.|Verifique se o certificado está instalado no repositório do LocalMAchine\My no servidor de AD FS. Verifique se a conta de serviço AD FS tem acesso de leitura à chave privada do certificado.|
|Evento 315-ocorreu um erro durante uma tentativa de criar a cadeia de certificados para o certificado de assinatura de confiança do provedor de declarações.|O certificado não foi revogado.</br></br>Não é possível verificar a cadeia de certificados.</br></br>O certificado expirou ou ainda não é válido.|Verifique se o certificado é válido e se não foi revogado.</br></br>Verifique se a CRL está acessível.|
|Evento 316-ocorreu um erro durante uma tentativa de criar a cadeia de certificados para o certificado de assinatura de confiança de terceira parte confiável.|O certificado não foi revogado.</br></br>Não é possível verificar a cadeia de certificados.</br></br>O certificado expirou ou ainda não é válido.|Verifique se o certificado é válido e se não foi revogado.</br></br>Verifique se a CRL está acessível.|
|Evento 317-ocorreu um erro durante uma tentativa de criar a cadeia de certificados para o certificado de criptografia de confiança de terceira parte confiável.|O certificado não foi revogado.</br></br>Não é possível verificar a cadeia de certificados.</br></br>O certificado expirou ou ainda não é válido.|Verifique se o certificado é válido e se não foi revogado.</br></br>Verifique se a CRL está acessível.|
|Evento 319-ocorreu um erro enquanto a cadeia de certificados para o certificado do cliente estava sendo compilada.|O certificado não foi revogado.</br></br>Não é possível verificar a cadeia de certificados.</br></br>O certificado expirou ou ainda não é válido.|Verifique se o certificado é válido e se não foi revogado.</br></br>Verifique se a CRL está acessível.|
|Evento 360-uma solicitação foi feita a um ponto de extremidade de transporte de certificado, mas a solicitação não incluiu um certificado de cliente.|A AC raiz que emitiu o certificado do cliente não é confiável.</br></br>O certificado do cliente expirou.</br></br>O certificado do cliente é auto-assinado e não é confiável.|Verifique se a autoridade de certificação raiz que emitiu o certificado do cliente está presente no repositório raiz confiável.</br></br>Verifique se o certificado do cliente não expirou.</br></br>Se o certificado do cliente for auto-assinado, verifique se ele foi adicionado à lista de certificados confiáveis ou substitua o certificado autoassinado por um certificado confiável.|
|Evento 374-ocorreu um erro ao criar a cadeia de certificados para o certificado de criptografia de confiança do provedor de declarações.|O certificado não foi revogado.</br></br>Não é possível verificar a cadeia de certificados.</br></br>O certificado expirou ou ainda não é válido.|Verifique se o certificado é válido e se não foi revogado.</br></br>Verifique se a CRL está acessível.|
|Evento 381-ocorreu um erro durante uma tentativa de criar a cadeia de certificados para o certificado de configuração.|Um dos certificados configurados para uso no servidor de AD FS está expirado ou foi revogado.|Verifique se todos os certificados configurados não foram revogados e se não expiraram.|
|Evento 385-AD FS detectou que um ou mais certificados no banco de dados de configuração do AD FS precisam ser atualizados manualmente.|Um dos certificados configurados para uso no servidor de AD FS expirou ou está se aproximando da data de expiração.|Atualize o certificado expirado ou em breve-para-expirar com uma substituição. (Se você estiver usando certificados autoassinados e a substituição automática de certificado estiver habilitada, esse erro poderá ser ignorado, pois ele será resolvido automaticamente.)|
|Evento 387-AD FS detectado que um ou mais dos certificados especificados no Serviço de Federação não estavam acessíveis para a conta de serviço usada pelo serviço do Windows AD FS.|A conta de serviço de AD FS não tem permissões de leitura para a chave privada de um ou mais certificados configurados.|Verifique se a conta de serviço AD FS tem permissão de leitura para a chave privada de todos os certificados configurados.|
|Evento 389-AD FS detectou que uma ou mais das suas relações de confiança exigem que seus certificados sejam atualizados manualmente porque expiraram ou expirarão em breve.|Um dos certificados do seu parceiro configurado expirou ou está prestes a expirar. Isso pode se aplicar a uma confiança do provedor de declarações ou a uma terceira parte confiável.|Se você criou essa relação de confiança manualmente, atualize a configuração do certificado manualmente. Se você usou metadados de Federação para criar a relação de confiança, o certificado será atualizado automaticamente assim que o parceiro atualizar o certificado.|




## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
 