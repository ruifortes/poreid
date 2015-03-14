# Utilização de cartões de teste #

O POReID não contempla a utilização de cartões de teste na sua forma atual. Considerámos duas razões para tal opção: a primeira razão é a disponibilização do componente que como está pode ser usado diretamente em ambientes produtivos e a segunda é que um cartão real é consideravelmente menos dispendioso que um de testes (podem ser adquiridos em http://www.kitcc.pt/ccidadao/kits).

No entanto, é possível a utilização de cartões de teste. Para tal bastará que se substitua a _keystore_ **poreid.cc.ks** localizada em **org/poreid/cc/keystores**. Na keystore substituta deverão ser adicionados os certificados de testes disponíveis em https://pki.teste.cartaodecidadao.pt/. Desta forma é possivel assinar com cartões de teste.