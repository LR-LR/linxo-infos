# **Resumé règles STET API**


- [**Resumé règles STET API**](#resum%c3%a9-r%c3%a8gles-stet-api)
  - [**AISP**](#aisp)
    - [**1. GET /accounts**](#1-get-accounts)
    - [**2. GET /accounts/{accountResourceId}/balances**](#2-get-accountsaccountresourceidbalances)
    - [**3. GET /accounts/{accountResourceId}/transactions**](#3-get-accountsaccountresourceidtransactions)
    - [**4. PUT /consents**](#4-put-consents)
    - [**5. GET /end-user-identity**](#5-get-end-user-identity)
    - [**6. GET /trusted-beneficiaries**](#6-get-trusted-beneficiaries)
  - [**PISP**](#pisp)
    - [**1. POST /payment-requests**](#1-post-payment-requests)
    - [**2. GET /payment-requests/{paymentRequestResourceId}**](#2-get-payment-requestspaymentrequestresourceid)
    - [**3. PUT /payment-requests/{paymentRequestResourceId}**](#3-put-payment-requestspaymentrequestresourceid)
    - [**4.1 POST /payment-requests/{paymentRequestResourceId}/confirmation**](#41-post-payment-requestspaymentrequestresourceidconfirmation)
    - [**4.2 POST /payment-requests/{paymentRequestResourceId}/o-confirmation**](#42-post-payment-requestspaymentrequestresourceido-confirmation)


## **AISP**
### **1. GET /accounts**
*Récupération des comptes du client (PSU)*
**Paramètres requis :**
| Nom          | Type   | Description                                                             |
| ------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton | Header | L'access token                                                          |
| Signature    | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID | Header | À définir dans la requête et à récupérer dans la réponse correspondante |

### **2. GET /accounts/{accountResourceId}/balances**
*Récupération d'un rapport sur le solde d'un compte*
**Paramètres requis :**
| Nom               | Type   | Description                                                             |
| ----------------- | ------ | ----------------------------------------------------------------------- |
| Authorization     | Header | L'access token                                                          |
| accountResourceId | Path   | Id du compte                                                            |
| Signature         | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID      | Header | À définir dans la requête et à récupérer dans la réponse correspondante |

### **3. GET /accounts/{accountResourceId}/transactions**
*Récupération de la liste des transactions d'un compte*
**Paramètres requis :**
| Nom               | Type   | Description                                                             |
| ----------------- | ------ | ----------------------------------------------------------------------- |
| Authorization     | Header | L'access token                                                          |
| accountResourceId | Path   | Id du compte                                                            |
| Signature         | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID      | Header | À définir dans la requête et à récupérer dans la réponse correspondante |

### **4. PUT /consents**
*Transmission du(des) consentement(s) du client (PSU)*
**Paramètres requis :**
| Nom          | Type   | Description                                                             |
| ------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton | Header | L'access token                                                          |
| Signature    | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID | Header | À définir dans la requête et à récupérer dans la réponse correspondante |
| Body         | Body   | Liste des consentements accordés à l'AISP par le PSU                    |
**Exemple de Body :**
```json
{
  "balances": [ // Liste des comptes dont on change le(s) consentement(s)
    {
      "iban": "YY64COJH41059545330222956960771321"
    }
  ],
  "trustedBeneficiaries": true, // Liste des bénéficiaires
  "psuIdentity": true // Identité du client
}
```

### **5. GET /end-user-identity**
*Récupération de l'identité de l'utilisateur*
**Paramètres requis :**
| Nom          | Type   | Description                                                             |
| ------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton | Header | L'access token                                                          |
| Signature    | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID | Header | À définir dans la requête et à récupérer dans la réponse correspondante |

### **6. GET /trusted-beneficiaries**
*Récupération de la liste des bénéficiaires de confiance*
**Paramètres requis :**
| Nom          | Type   | Description                                                             |
| ------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton | Header | L'access token                                                          |
| Signature    | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID | Header | À définir dans la requête et à récupérer dans la réponse correspondante |


## **PISP**
### **1. POST /payment-requests**
*Initiation d'une requête de paiement (PISP)*
**Paramètres requis :**
| Nom          | Type   | Description                                                             |
| ------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton | Header | L'access token                                                          |
| Signature    | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID | Header | À définir dans la requête et à récupérer dans la réponse correspondante |
| Body         | Body   | Informations de la requête de paiement                                  |
**Exemple de Body :**
```json
{
  "paymentInformationId": "MyPmtInfId", // [Obligatoire] ID paiement
  "creationDateTime": "2020-01-23T10:56:10+01:00", // [Obligatoire] Date de création du paiement
  "numberOfTransactions": 1, // Nombre de transactions
  "paymentTypeInformation": { // [Obligatoire]
    "serviceLevel": "SEPA", // [Voir doc] Règles en vertu desquelles la transaction doit être traitée, seul "SEPA" est autorisé
    "localInstrument": "INST", // [Voir doc] Instrument spécifique à la communauté des utilisateurs, "INST" pour demander un paiement instantané SEPA
    "categoryPurpose": "DVPM" // [Voir doc] Spécifie l'objectif de haut niveau de l'instruction. (CASH, CORT, DVPM, INTC, TREA)
  },
  "debtor": { // [Obligatoire] Identification d'une partie débiteur
    "name": "MyCustomer",
    "postalAddress": {
      "country": "FR",
      "addressLine": [
        "18 rue de la DSP2",
        "75008 PARIS"
      ]
    },
    "privateId": {
      "identification": "FD37G",
      "schemeName": "BANK",
      "issuer": "BICXYYTTZZZ"
    }
  },
  "creditor": { // [Obligatoire] Identification d'une partie créditeur [Obligatoire]
    "name": "myMerchant",
    "postalAddress": {
      "country": "FR",
      "addressLine": [
        "18 rue de la DSP2",
        "75008 PARIS"
      ]
    },
    "organisationId": {
      "identification": "852126789",
      "schemeName": "SIREN",
      "issuer": "FR"
    }
  },
  "creditorAccount": { // [Obligatoire] Identification d'un compte
    "iban": "YY64COJH41059545330222956960771321" // International Bank Account Number
  },
  "chargeBearer": "SLEV", // [Obligatoire] Code qui spécifie quelle(s) partie(s) supporteront les frais de traitement de l'opération de paiement (DEBT, CRED, SHAR, SLEV)
  "creditTransferTransaction": [ // [Obligatoire]
    {
      "paymentId": { // [Obligatoire] Éléments utilisés pour faire référence à une instruction de paiement
        "instructionId": "MyInstrId", // [Obligatoire] Id de l'instruction de paiement partagé entre le PISP et l'ASPSP
        "endToEndId": "MyEndToEndId" // Id de la transaction attribué par la partie initiatrice 
      },
      "requestedExecutionDate": "2016-12-31T00:00:00.000+01:00", // [Obligatoire] Date de débit du compte du débiteur
      "instructedAmount": { // [Obligatoire]
        "currency": "EUR", // [Obligatoire] Devise
        "amount": ".01" // [Obligatoire] Montant
      }
    }
  ],
  "supplementaryData": { // Informations additionnelles (URLs renvoie du rapport de situation au PISP et spécifie les approches d'authentification acceptées par le PISP et celles qui ont été choisies par l'ASPSP)
    "acceptedAuthenticationApproach": [ // combinaison de valeurs possibles pour les approches d'authentification (REDIRECT, DECOUPLED, EMBEDDED-1-FACTOR)
      "REDIRECT",
      "DECOUPLED"
    ],
    "successfulReportUrl": "http://myPisp/PaymentSuccess", // URL à utiliser par l'ASPSP afin d'informer le PISP de la finalisation du processus d'authentification et de consentement dans l'approche REDIRECT et DECOUPLED
    "unsuccessfulReportUrl": "http://myPisp/PaymentFailure" // URL à utiliser par le PISP afin de notifier le PISP de l'échec du processus d'authentification et de consentement dans l'approche REDIRECT et DECOUPLED Si cet URL n'est pas fourni par le PISP, le PISP utilisera le "successfulReportUrl" même en cas d'échec du traitement de la demande de paiement
  }
}
```
Les valeurs à changer sont :
- creationDateTime
- creditorAccount -> iban
- creditTransferTransaction -> requestedExecutionDate

### **2. GET /payment-requests/{paymentRequestResourceId}**
*Récupération d'une requête de paiement*
**Paramètres requis :**
| Nom                      | Type   | Description                                                             |
| ------------------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton             | Header | L'access token                                                          |
| paymentRequestResourceId | Path   | Id de la requête de paiement                                            |
| Signature                | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID             | Header | À définir dans la requête et à récupérer dans la réponse correspondante |

### **3. PUT /payment-requests/{paymentRequestResourceId}**
*Modification d'une requête de paiement*
**Paramètres requis :**
| Nom                      | Type   | Description                                                             |
| ------------------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton             | Header | L'access token                                                          |
| paymentRequestResourceId | Path   | Id de la requête de paiement                                            |
| Signature                | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID             | Header | À définir dans la requête et à récupérer dans la réponse correspondante |
| Body                     | Body   | Informations de la requête de paiement                                  |
**Exemple de body : [voir POST /payment-requests](#1-post-payment-requests)**

### **4.1 POST /payment-requests/{paymentRequestResourceId}/confirmation**
*Confirmation d'une requête de paiement ou de modification à l'aide de l'authentification standard du PSU*
**Paramètres requis :**
| Nom                      | Type   | Description                                                             |
| ------------------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton             | Header | L'access token                                                          |
| paymentRequestResourceId | Path   | Id de la requête de paiement                                            |
| Signature                | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID             | Header | À définir dans la requête et à récupérer dans la réponse correspondante |
| Body                     | Body   | Informations de la requête de paiement                                  |
**Exemple de body :**
```json
{ // [Obligatoire, même si vide]
  "psuAuthenticationFactor": "JJKJKJ788GKJKJBK"
}
```

### **4.2 POST /payment-requests/{paymentRequestResourceId}/o-confirmation**
*Confirmation d'une requête de paiement ou de modification à l'aide d'un code d'autorisation OAUTH2*
**Paramètres requis :**
| Nom                      | Type   | Description                                                             |
| ------------------------ | ------ | ----------------------------------------------------------------------- |
| Authorizaton             | Header | L'access token                                                          |
| paymentRequestResourceId | Path   | Id de la requête de paiement                                            |
| Signature                | Header | Signature de la requête (gérée par le proxy)                            |
| X-Request-ID             | Header | À définir dans la requête et à récupérer dans la réponse correspondante |
| Body                     | Body   | Informations de la requête de paiement                                  |
**Exemple de body :**
```json
{ // [Obligatoire, même si vide]
  "psuAuthenticationFactor": "JJKJKJ788GKJKJBK"
}
```
