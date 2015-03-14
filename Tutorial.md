# Exemplos de utilização #

> ### Leitura do nome da rua, do apelido e do campo de notas pessoais ###
```
CitizenCard cc = CardFactory.getCard();
System.out.println("nome da rua: "+cc.getAddress().getStreetName());
System.out.println("apelido: "+cc.getID().getSurname());
System.out.println("notas pessoais: "+cc.readPersonalNotes().toString());
cc.close();
```

> ### Leitura da fotografia ###
```
CitizenCard cc = CardFactory.getCard();
byte[] foto = cc.getPhotoData().getPhoto(); //formato jpeg2000
cc.close();
```

> ### Leitura dos dados do cidadão e utilização da keystore, reutilizando a mesma instância ###
```
Security.addProvider(new POReIDProvider());
        
CitizenCard cc = CardFactory.getCard();
System.out.println("apelido: "+cc.getID().getCitizenFullName());
System.out.println("NIF: "+cc.getID().getTaxNo());
        
POReIDKeyStoreParameter ksParam = new POReIDKeyStoreParameter();
ksParam.setCard(cc);
        
KeyStore ks = KeyStore.getInstance(POReIDConfig.POREID);
ks.load(ksParam);
        
Signature signature = Signature.getInstance("SHA1withRSA");
PrivateKey pk = (PrivateKey) ks.getKey(POReIDConfig.AUTENTICACAO, null);
signature.initSign(pk);
signature.update("01234567890".getBytes());
byte[] signatureBytes = signature.sign();
cc.close();
```
  * **Este cenário é util na eventualidade de existir mais do que um par cartão/leitor no sistema, assim evita-se selecionar duas vezes o cartão pretendido.**