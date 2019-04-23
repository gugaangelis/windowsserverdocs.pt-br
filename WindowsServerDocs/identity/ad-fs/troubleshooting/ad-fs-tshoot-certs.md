---
title: Solucionando problemas do AD FS - certificados
description: Este documento descreve os problemas típicos de certificado.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 42fdce936bb4a6f25b456c4a96c7b99f650e6033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860577"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Solucionando problemas do AD FS - certificados
O AD FS requer os seguintes certificados para funcionar corretamente.  Se qualquer um deles não foi configurado ou configurado corretamente os problemas podem surgir.  

## <a name="required-certificates"></a>Certificados necessários
O AD FS requer os seguintes certificados:



- **Relação de confiança de Federação** – isso requer que um certificado encadeado a uma raiz de Internet (autoridade de certificação) mutuamente confiável está presente no repositório de raiz confiável do provedor de declarações e terceira servidores de federação, um design de certificação cruzada foi implementado no qual cada lado tem trocadas sua autoridade de certificação raiz com seu parceiro, ou auto-assinados certificados foram importados em cada lado onde for apropriado.
- **Autenticação de token** – o computador de cada serviço de Federação requer um certificado de autenticação de token.  O certificado de autenticação de token do provedor de declarações deve ser confiável pela terceira servidor de Federação. O certificado de autenticação de token de terceiros terceira parte confiável deve ser confiável por todos os aplicativos que recebem os tokens do servidor de federação de RP.
- **Secure Sockets Layer (SSL)** – o certificado SSL para o serviço de Federação devem estar presente em um repositório confiável no computador de proxy do servidor de Federação e tem uma cadeia válida para um repositório de autoridade de certificação (CA) confiável.
- **Lista de revogação (CRL) do certificado** – para qualquer certificado que tenha uma CRL publicada, a CRL deve ser acessível a todos os clientes e servidores que precisam acessar o certificado.

Se qualquer uma das opções acima não estão configuradas corretamente, o AD FS não funcionará.

## <a name="common-things-to-check-with-certificates"></a>Tarefas comuns para verificar com certificados
O exemplo a seguir é uma lista de coisas que podem surgir e devem ser verificados durante a tentativa de resolver um problema de certificado.

- Certifique-se de que o certificado é confiável
    - Certificados SSL precisam ser confiável para os clientes
    - Certificados de autenticação de token precisam ser confiável pelas partes confiantes
- Verifique a cadeia de confiança - cada certificado na cadeia, precisa ser válido.
- Verifique se a data de validade do certificado
- Verificar a acessibilidade de lista de revogação de certificados (CRL)
    - Verifique se que o campo de CPD é populado
    - Navegar manualmente para o CDP
- Verifique se que o certificado não foi revogado

## <a name="common-certificate-errors"></a>Erros comuns de certificado
A tabela a seguir é uma lista de erros comuns e as possíveis causas.

|Evento|Causa|Resolução
|-----|-----|-----|
|Evento 249 - um certificado não pôde ser encontrado no repositório de certificados. Em cenários de substituição do certificado, isso poderá causar uma falha quando o serviço de Federação é assinar ou descriptografar usando este certificado.|O certificado em questão não está presente no repositório de certificados local ou a conta de serviço não tem permissão para a chave privada do certificado.|Certifique-se de que o certificado está instalado no repositório LocalMAchine\My no servidor do AD FS. Certifique-se de que a conta de serviço do AD FS tem acesso de leitura à chave privada do certificado.|
|Evento 315 - Ocorreu um erro durante uma tentativa de criar a cadeia de certificados para a relação de confiança de provedor de declarações do certificado de autenticação.|O certificado não foi revogado.</br></br>A cadeia de certificados não pode ser verificada.</br></br>O certificado expirou ou ainda não é válido.|Certifique-se de que o certificado é válido e não foi revogado.</br></br>Certifique-se de que a CRL é acessível.|
|Evento 316 - Ocorreu um erro durante uma tentativa de criar a cadeia de certificados para a terceira parte confiável do certificado de autenticação.|O certificado não foi revogado.</br></br>A cadeia de certificados não pode ser verificada.</br></br>O certificado expirou ou ainda não é válido.|Certifique-se de que o certificado é válido e não foi revogado.</br></br>Certifique-se de que a CRL é acessível.|
|Evento 317 - Ocorreu um erro durante uma tentativa de criar a cadeia de certificados para o certificado de criptografia de relação de confiança de terceiros terceira parte confiável.|O certificado não foi revogado.</br></br>A cadeia de certificados não pode ser verificada.</br></br>O certificado expirou ou ainda não é válido.|Certifique-se de que o certificado é válido e não foi revogado.</br></br>Certifique-se de que a CRL é acessível.|
|Evento 319 - erro enquanto estava sendo construída a cadeia de certificados para o certificado do cliente.|O certificado não foi revogado.</br></br>A cadeia de certificados não pode ser verificada.</br></br>O certificado expirou ou ainda não é válido.|Certifique-se de que o certificado é válido e não foi revogado.</br></br>Certifique-se de que a CRL é acessível.|
|Evento 360 - foi feita uma solicitação para um ponto de extremidade de transporte de certificado, mas a solicitação não incluiu um certificado de cliente.|A raiz da autoridade de certificação que emitiu o certificado de cliente não é confiável.</br></br>O certificado do cliente expirou.</br></br>O certificado do cliente é autoassinado e não é confiável.|Certifique-se de que a raiz da autoridade de certificação que emitiu o certificado de cliente está presente no repositório de raiz confiável.</br></br>Certifique-se de que o certificado do cliente não expirou.</br></br>Se o certificado do cliente for autoassinado, certifique-se de que ele foi adicionado à lista de certificados confiáveis ou substituir o certificado autoassinado com um certificado confiável.|
|Evento 374 - Ocorreu um erro ao criar a cadeia de certificados para o provedor de declarações certificado de criptografia de relação de confiança.|O certificado não foi revogado.</br></br>A cadeia de certificados não pode ser verificada.</br></br>O certificado expirou ou ainda não é válido.|Certifique-se de que o certificado é válido e não foi revogado.</br></br>Certifique-se de que a CRL é acessível.|
|Evento 381 - Ocorreu um erro durante uma tentativa de criar a cadeia de certificados para o certificado de configuração.|Um dos certificados configurados para uso no servidor do AD FS expirou ou foi revogado.|Certifique-se de que todos os certificados configurados não foi revogados e não expiraram.|
|Evento 385 - o AD FS detectado que um ou mais certificados no banco de dados de configuração do AD FS precisa ser atualizado manualmente.|Um dos certificados configurados para uso no servidor do AD FS expirou ou está se aproximando da data de expiração.|Atualize o certificado expirado ou em breve para expirar com uma substituição. (Se você estiver usando certificados autoassinados e substituição do certificado automático estiver habilitada, esse erro pode ser ignorado pois ele será resolvido automaticamente.)|
|Evento 387 - o AD FS detectado que um ou mais dos certificados que são especificados no serviço de Federação não eram acessíveis à conta de serviço usada pelo serviço Windows do AD FS.|A conta de serviço do AD FS não tem permissões de leitura para a chave privada de um ou mais certificados configurados.|Certifique-se de que a conta de serviço do AD FS tem permissão de leitura para a chave privada de todos os certificados configurados.|
|Evento 389 - o AD FS detectado que um ou mais dos seus objetos de confiança exigem os certificados a ser atualizado manualmente porque eles são expirou ou expirarão em breve.|Um dos certificados do seu parceiro configurado expirou ou está prestes a expirar. Aplica-se para uma confiança de provedor de declarações ou para uma terceira parte confiável.|Se você criou manualmente essa relação de confiança, atualize a configuração do certificado manualmente. Se você usou os metadados de federação para criar a relação de confiança, o certificado será atualizado automaticamente, assim que o parceiro atualiza o certificado.|




## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
 