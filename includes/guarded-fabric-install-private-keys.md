Se você não forneceu um arquivo PFX para a criptografia ou certificados de autenticação no primeiro servidor HGS, somente a chave pública será replicada para esse servidor.
Você precisará instalar a chave privada importando um arquivo PFX que contém a chave privada para o repositório de certificados local ou, no caso de chaves com suporte para HSM, configurando o provedor de armazenamento de chaves e associando-o a seus certificados de acordo com as instruções do fabricante do HSM.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->