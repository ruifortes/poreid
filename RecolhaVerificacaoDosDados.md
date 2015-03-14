# Problema da utilização dos dados numa aplicação desligada da aplicação que acede ao Cartão de Cidadão #

Tipicamente esta situação ocorre em contexto web. A recolha individual dos elementos pretendidos, mesmo que assinados, não oferece garantias relativamente à autenticidade dos elementos recebidos. O objectivo deste documento visa fornecer uma solução para a recolha e validação de dados de identificação e morada.

## Recolha de dados de um Cartão de Cidadão ##

O Cartão de Cidadão, à semelhança dos passaportes biométricos contêm um mecanismo de segurança designado por Autenticação Passiva (termo original _Passive Authentication_). Este mecanismo é utilizado para verificar se os dados existentes no chip são autênticos e não foram alterados.

O mecanismo é concretizado na forma de um _Signed CMS_ que contêm os resumos criptográficos dos seguintes elementos do cartão: dados de identificação do cidadão, fotografia do cidadão, chave pública do cartão e morada do cidadão. Este _Signed CMS_ está armazenado num ficheiro tipicamente designado por _SOD_ (Security Object Descriptor)

A verificação da autenticidade dos dados não oferece garantias de que estes não foram lidos a partir de outra qualquer fonte. É necessário relembrar que os dados são públicos, podendo ser lidos livremente (apenas a morada está protegida por pin). Torna-se necessário adicionar um factor de segurança extra. Tal factor poderá ser concretizado sob a forma de uma assinatura com a adição de um token gerado por um serviço externo. No caso de uma autenticação em que há um serviço que a solicita, este serviço pode gerar um _nonce_ que é enviado para ser assinado. Tal assinatura sobre este token impede ataques de repetição.


Sendo assim a recolha de dados num contexto de autenticação consistiria no seguinte:


  * **ASSINATURA(RESUMO(**dados do cidadão<sup>1</sup> + _nonce_**))**
  * dados de identificação do cidadão<sup>1</sup> (obrigatório/opcional - ver nota)
  * morada do cidadão<sup>1</sup> (obrigatório/opcional - ver nota)
  * fotografia do cidadão<sup>1</sup> (obrigatório/opcional - ver nota)

  * certificado de autenticação (obrigatório)
  * _SOD_ (obrigatório/opcional - ver nota)
  * _nonce_ (obrigatório)


## Verificação dos dados recolhidos ##

O procedimento de verificação inicia-se com a verificação da validade do _nonce_. Se for válido, procede-se à verificação da assinatura produzida pelo cidadão.

Se a assinatura produzida pelo cidadão for válida, gera-se o resumo criptográfico e compara-se com o resumo assinado, desta forma garante-se que o conjunto de dados recebidos não foi modificado até chegar ao destino.

Posteriormente verifica-se o _Signed CMS_, se for válido, geram-se os resumos criptográficos relativos aos dados do cidadão e comparam-se com os existentes no _Signed CMS_, desta forma garante-se que dados supostamente lidos do Cartão de Cidadão são autênticos e não foram alterados.

Falta apenas uma validação adicional, que consiste em verificar se o nome/nic existente no certificado de autenticação coincide com o nome/nic existente no bloco de dados designado por identificação do cidadão. Esta ultima verificação garante que os dados fornecidos constam no mesmo Cartão de Cidadão que produziu a assinatura.

**Exemplo disponível no repositório [aqui](http://code.google.com/p/poreid/source/browse/#svn%2Ftrunk%2Fverifysample), para executar necessita do projecto [sod.verify](http://code.google.com/p/poreid/source/browse/#svn%2Ftrunk%2Fsod.verify) igualmente disponível no mesmo repositório.**

---

**Nota:** A leitura dos dados de identificação do cidadão é opcional se os dados pretendidos constarem no certificado de autenticação (nome e número de identificação civil). Desta forma o o procedimento poderia ser reduzido a:

  * **ASSINATURA(RESUMO(**_nonce_**))**
  * certificado de autenticação (obrigatório)
  * _nonce_ (obrigatório)