Moip SDK JS
=============
[![Build Status](https://travis-ci.org/brunoosilva/moip-sdk-js.svg)](https://travis-ci.org/brunoosilva/moip-sdk-js) [![Coverage Status](https://coveralls.io/repos/github/brunoosilva/moip-sdk-js/badge.svg?branch=master)](https://coveralls.io/github/brunoosilva/moip-sdk-js?branch=master)

## O que é?

SDK Javascript que possibilita a criptografia de dados sensíveis de cartão no browser do cliente assim como identificação e validação de números de cartão de crédito. Para Web / React Native / Ionic 1 / Ionic 3 / NodeJS.

## Plataformas

* [Web e Ionic 1](brunoosilva/moip-sdk-js#web-e-ionic-1)
* [Ionic 3](brunoosilva/moip-sdk-js#ionic-3)
* [React Native](brunoosilva/moip-sdk-js#react-native)
* [NodeJS](brunoosilva/moip-sdk-js#nodejs)


## Observação

Para todas as plataformar, é necessário passar a sua public key como parâmetro para gerar o hash dos dados do cartão de crédito. Essa informação você pode obter pelo painel da moip, na seção de **Chave de acesso** https://conta.moip.com.br/configurations/api_credentials

```shell
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoBttaXwRoI1Fbcond5mS
7QOb7X2lykY5hvvDeLJelvFhpeLnS4YDwkrnziM3W00UNH1yiSDU+3JhfHu5G387
O6uN9rIHXvL+TRzkVfa5iIjG+ap2N0/toPzy5ekpgxBicjtyPHEgoU6dRzdszEF4
ItimGk5ACx/lMOvctncS5j3uWBaTPwyn0hshmtDwClf6dEZgQvm/dNaIkxHKV+9j
Mn3ZfK/liT8A3xwaVvRzzuxf09xJTXrAd9v5VQbeWGxwFcW05oJulSFjmJA9Hcmb
DYHJT+sG2mlZDEruCGAzCVubJwGY1aRlcs9AQc1jIm/l8JwH7le2kpk3QoX+gz0w
WwIDAQAB
-----END PUBLIC KEY-----
```

### Web e Ionic 1

Neste cenário, a lib de criptografia já está compilada junto com o código do SDK, sem ter necessidade de importar outro script.

```javascript
<script src="moip-sdk-js.js"></script>

<script>
MoipSdkJs.MoipCreditCard
    	.setPubKey(pubKey)
	.setCreditCard({
	    number  : '4012001037141112',
	    cvc     : '123',
	    expirationMonth: '05',
      	    expirationYear : '18'
    	})
	.hash()
	.then(hash => console.log('hash', hash));
</script>
```

### Ionic 3

Neste cenário, é necessário instalar e importar uma lib de criptografia de terceiro, para gerar o hash do carto de crédito. Após importar, lembrar de passar o contexto dele atravéz do método **setEncrypter**, como mostrado abaixo no exemplo.

#### Instalar

```
yarn add moip-sdk-js jsencrypt 
// or
npm i moip-sdk-js jsencrypt --save
```

#### Usar

```javascript
import jsencrypt from 'jsencrypt';
import { MoipCreditCard } from 'moip-sdk-js';

MoipCreditCard
	.setEncrypter(jsencrypt, 'ionic')
    	.setPubKey(pubKey)
	.setCreditCard({
	    number  : '4012001037141112',
	    cvc     : '123',
	    expirationMonth: '05',
      	    expirationYear : '18'
    	})
	.hash()
	.then(hash => console.log('hash', hash));
```

### React Native

Neste cenário, é necessário instalar e importar uma lib de criptografia de terceiro, para gerar o hash do carto de crédito. Após importar, lembrar de passar o contexto dele atravéz do método **setEncrypter**, como mostrado abaixo no exemplo.

#### Instalar

```
yarn add moip-sdk-js react-native-rsa-native
// or
npm i moip-sdk-js react-native-rsa-native --save
```

#### Usar

```javascript
import { RSA } from 'react-native-rsa-native';
import { MoipCreditCard } from 'moip-sdk-js';

MoipCreditCard
	.setEncrypter(RSA, 'react-native')
	.setPubKey(pubKey)
	.setCreditCard({
	    number  : '4012001037141112',
	    cvc     : '123',
	    expirationMonth: '05',
	    expirationYear : '18'
	})
	.hash()
	.then(hash => console.log('hash', hash));
```

### NodeJS

Neste cenário, é necessário instalar e importar uma lib de criptografia de terceiro, para gerar o hash do carto de crédito. Após importar, lembrar de passar o contexto dele atravéz do método **setEncrypter**, como mostrado abaixo no exemplo.

#### Instalar

```
yarn add moip-sdk-js node-rsa
// or
npm i moip-sdk-js node-rsa --save
```

#### Usar

```javascript
import NodeRSA from 'node-rsa';
import { MoipCreditCard } from 'moip-sdk-js';

MoipCreditCard
	.setEncrypter(NodeRSA, 'node')
	.setPubKey(pubKey)
	.setCreditCard({
	    number  : '4012001037141112',
	    cvc     : '123',
	    expirationMonth: '05',
	    expirationYear : '18'
	})
	.hash()
	.then(hash => console.log('hash', hash));
```


## Validação do Cartão de Crédito

Também é disponibilizado uma class com alguns métodos que faz as validações dos dados do cartão de crédito.

Para o uso Web ou Ionic 1, deve usar a classe da seguinte forma:

```javascript
MoipSdkJs.MoipValidator.isValidNumber(12345); // return true/false
```

Para o uso React Native ou Ionic 3, deve usar a classe da seguinte forma:
```javascript
import { MoipValidator } from 'moip-sdk-js';
MoipValidator.isValidNumber(12345); // return true/false
```

### Número do cartão de crédito
``` javascript
const creditCardNumber = '4111111111111111';
MoipValidator.isValidNumber(creditCardNumber);	//return true/false
```

### Código de segurança do cartão de crédito (CVC)
``` javascript
const creditCardNumber = '4111111111111111';
const cvc = '123';
MoipValidator.isSecurityCodeValid(creditCardNumber, cvc); //return true/false
```

### Data de expiração do cartão de crédito
``` javascript
const month = '10';
const year = '2020';
MoipValidator.isExpiryDateValid(month, years);	//return true/false
```

### Bandeira dos cartões
``` javascript
MoipValidator.cardType('5105105105105100');    //return [Object]MASTERCARD
MoipValidator.cardType('4111111111111111');    //return [Object]VISA
MoipValidator.cardType('341111111111111');     //return [Object]AMEX
MoipValidator.cardType('30569309025904');      //return [Object]DINERS
MoipValidator.cardType('3841001111222233334'); //return [Object]HIPERCARD
MoipValidator.cardType('4514160123456789');    //return [Object]ELO
MoipValidator.cardType('6370950000000005');    //return [Object]HIPER
MoipValidator.cardType('9191919191919191');    //return [Object]null
```

## Licença

MIT
