# **Resumé règles BerlinGroup API**

- [**Resumé règles BerlinGroup API**](#resum%c3%a9-r%c3%a8gles-berlingroup-api)
  - [**AIS**](#ais)
    - [**1. GET /v1/accounts**](#1-get-v1accounts)
    - [**2. GET /v1/accounts/{account-id}**](#2-get-v1accountsaccount-id)
    - [**3. GET /v1/accounts/{account-id}/balances**](#3-get-v1accountsaccount-idbalances)
    - [**4. GET /v1/accounts/{account-id}/transactions**](#4-get-v1accountsaccount-idtransactions)
    - [**5. GET /v1/accounts/{account-id}/transactions/{resourceId}**](#5-get-v1accountsaccount-idtransactionsresourceid)
    - [**6. POST /v1/consents**](#6-post-v1consents)
    - [**7. GET /v1/consents/{consentId}**](#7-get-v1consentsconsentid)
    - [**8. DELETE /v1/consent/{consentId}**](#8-delete-v1consentconsentid)
    - [**9. GET /v1/consents/{consentId}/status**](#9-get-v1consentsconsentidstatus)
    - [**10. POST /v1/consents/{consentId}/authorisations**](#10-post-v1consentsconsentidauthorisations)
    - [**11. GET /v1/consents/{consentId}/authorisations/{authorisationId}**](#11-get-v1consentsconsentidauthorisationsauthorisationid)
    - [**12. PUT /v1/consents/{consentId}/authorisations/{authorisationId}**](#12-put-v1consentsconsentidauthorisationsauthorisationid)
  - [**PIS**](#pis)
    - [**1. POST /v1/{payment-service}/{payment-product}**](#1-post-v1payment-servicepayment-product)
    - [**2. GET /v1/{payment-service}/{payment-product}/{paymentId}**](#2-get-v1payment-servicepayment-productpaymentid)
    - [**3. DELETE /v1/{payment-service}/{payment-product}/{paymentId}**](#3-delete-v1payment-servicepayment-productpaymentid)
    - [**4. GET /v1/{payment-service}/{payment-product}/{paymentId}/status**](#4-get-v1payment-servicepayment-productpaymentidstatus)
    - [**5. POST /v1/{payment-service}/{payment-product}/{paymentId}/authorisations**](#5-post-v1payment-servicepayment-productpaymentidauthorisations)
    - [**6. GET /v1/{payment-service}/{payment-product}/{paymentId}/authorisations**](#6-get-v1payment-servicepayment-productpaymentidauthorisations)
    - [**7. GET /v1/{payment-service}/{payment-product}/{paymentId}/authorisations/{authorisationId}**](#7-get-v1payment-servicepayment-productpaymentidauthorisationsauthorisationid)
    - [**8. PUT /v1/{payment-service}/{payment-product}/{paymentId}/authorisations/{authorisationId}**](#8-put-v1payment-servicepayment-productpaymentidauthorisationsauthorisationid)
    - [**9. POST /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations**](#9-post-v1payment-servicepayment-productpaymentidcancellation-authorisations)
    - [**10. GET /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations**](#10-get-v1payment-servicepayment-productpaymentidcancellation-authorisations)
    - [**11. GET /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations/{cancellationId}**](#11-get-v1payment-servicepayment-productpaymentidcancellation-authorisationscancellationid)
    - [**12. PUT /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations/{cancellationId}**](#12-put-v1payment-servicepayment-productpaymentidcancellation-authorisationscancellationid)

## **AIS**
### **1. GET /v1/accounts**
*Lire la liste des comptes*
**Paramètres requis :**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| X-Request-ID | Header | ID de la requête   |
| Consent-ID   | Header | ID du consentement |

### **2. GET /v1/accounts/{account-id}**
*Lire les détails d'un compte*
**Paramètres requis :**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| account-id   | Path   | ID de la requête   |
| X-Request-ID | Header | ID de la requête   |
| Consent-ID   | Header | ID du consentement |

### **3. GET /v1/accounts/{account-id}/balances**
*Lire le solde d'un compte*
**Paramètres requis :**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| account-id   | Path   | ID de la requête   |
| X-Request-ID | Header | ID de la requête   |
| Consent-ID   | Header | ID du consentement |

### **4. GET /v1/accounts/{account-id}/transactions**
*Lire la liste des transactions d'un compte*
**Paramètres requis :**
| Nom           | Type   | Description                                                      |
| ------------- | ------ | ---------------------------------------------------------------- |
| account-id    | Path   | ID de la requête                                                 |
| bookingStatus | Query  | Statut des transactions retournées ("booked", "pending", "both") |
| X-Request-ID  | Header | ID de la requête                                                 |
| Consent-ID    | Header | ID du consentement                                               |

### **5. GET /v1/accounts/{account-id}/transactions/{resourceId}**
*Lire les détaile d'une transaction*
**Paramètres requis :**
| Nom          | Type   | Description          |
| ------------ | ------ | -------------------- |
| account-id   | Path   | ID de la requête     |
| resourceId   | Path   | Id de la transaction |
| X-Request-ID | Header | ID de la requête     |
| Consent-ID   | Header | ID du consentement   |

### **6. POST /v1/consents**
*Créer un consentement*
**Paramètres requis :**
| Nom          | Type   | Description      |
| ------------ | ------ | ---------------- |
| X-Request-ID | Header | ID de la requête |
| Body         | Body   | ID de la requête |
**Exemple de body :**
```json
{
  "access": { // Accès demandés pour un consentement
    "accounts": [ // Demande des informations des comptes détaillés
      {
        "iban": "FR7612345987650123456789014",
        "currency": "EUR"
      },
      {
        "bban": "BARC12345612345678",
        "currency": "EUR"
      }
    ],
    "balances": [ // Demande des soldes des comptes
      {
        "pan": "5409050000000000",
        "currency": "EUR"
      },
      {
        "maskedPan": "123456xxxxxx1234",
        "currency": "EUR"
      }
    ],
    "transactions": [ // Demande des transactions des comptes
      {
        "msisdn": "+49 170 1234567",
        "currency": "EUR"
      }
    ],
    "availableAccounts": "allAccounts", // Optionnel si supporté par l'API de la banque
    "allPsd2": "allAccounts" // Optionnel si supporté par l'API de la banque
  },
  "recurringIndicator": false, // Accès récurrent aux données d'un compte (true), ou un seul accès (false)
  "validUntil": "2020-12-31", // Date de fin de validité
  "frequencyPerDay": 4, // Fréquence maximale d'accès par jour
  "combinedServiceIndicator": false // Si c'est "true", ça veut dire qu'un service d'initiation de paiement sera adressé dans la même session
}
```

### **7. GET /v1/consents/{consentId}**
*Retourne un consentement*
**Paramètres requis :**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| consentId    | Path   | ID du consentement |
| X-Request-ID | Header | ID de la requête   |

### **8. DELETE /v1/consent/{consentId}**
*Supprime un consentement*
**Paramètres requis**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| consentId    | Path   | ID du consentement |
| X-Request-ID | Header | ID de la requête   |

### **9. GET /v1/consents/{consentId}/status**
*Lire le status d'un consentement d'un compte*
**Paramètres requis**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| consentId    | Path   | ID du consentement |
| X-Request-ID | Header | ID de la requête   |

### **10. POST /v1/consents/{consentId}/authorisations**
*Lance le processus d'autorisation d'un consentement*
**Paramètres requis :**
| Nom          | Type   | Description        |
| ------------ | ------ | ------------------ |
| X-Request-ID | Header | ID de la requête   |
| consentId    | Path   | ID du consentement |

### **11. GET /v1/consents/{consentId}/authorisations/{authorisationId}**
*Lire le status de l'authentification forte (SCA) de l'autorisation d'un consentement*
**Paramètres requis :**
| Nom             | Type   | Description          |
| --------------- | ------ | -------------------- |
| consentId       | Path   | ID du consentement   |
| authorisationId | Path   | ID de l'autorisation |
| X-Request-ID    | Header | ID de la requête     |

### **12. PUT /v1/consents/{consentId}/authorisations/{authorisationId}**
*Met à jour les données de l'utilisateur (PSU) dans les consentements*
**Paramètres requis :**
| Nom             | Type   | Description          |
| --------------- | ------ | -------------------- |
| consentId       | Path   | ID du consentement   |
| authorisationId | Path   | ID de l'autorisation |
| X-Request-ID    | Header | ID de la requête     |
| Body            | Body   | Body de la requête   |
**Exemple de body : [Voir POST /v1/consents](#post-v1consents)**

## **PIS**

### **1. POST /v1/{payment-service}/{payment-product}**
*Requête d'initiation de paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                  |
| --------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------- |
| payment-service | Path   | Service de paiement (payments, bulk-payments, periodic-payments)                                                             |
| payment-product | Path   | Service de paiement (sepa-credit-transfers, instant-sepa-credit-transfers, target-2-payments, cross-border-credit-transfers) |
| X-Request-ID    | Header | ID de la requête                                                                                                             |
| PSU-IP-Address  | Header | L'adresse IP du client (PSU)                                                                                                 |
| Body            | Body   | Body de la requête                                                                                                           |
**Exemple de body :**
```json
// Pour les paiements simple ou récurrents (payments, periodic-payments)
{
  "endToEndIdentification" : "MyEndToEndId",
  "debtorAccount" : { // Compte du débiteur
    "iban" : "FR7612345987650123456789014", // iban ou bban ou pan ou maskedPan ou msisdn ou currency
    "currency" : "EUR" // devise
  },
  "instructedAmount" : {
    "currency" : "USD",
    "amount" : ".01" // montant
  },
  "creditorAccount" : { // Compte du créditeur
    "bban" : "BARC12345612345678",
    "currency" : "EUR"
  },
  "creditorAgent" : "BICFI", // Créancier
  "creditorAgentName" : "Creditor Agent Name", // Nom du créancier
  "creditorName" : "Creditor Name", // Nom du créditeur
  "creditorAdress" : {
    "streetName" : "Rue de la PSD2",
    "buildingNumber" : "BAT 12",
    "townName" : "Aix-En-Provence",
    "postCode" : "13100",
    "country" : "FR"
  },
  // Seulement pour les paiements récurrents ("periodic-payments")
  "startDate" : "2020-01-24", // Date de début
  "endDate" : "2020-12-24", // Date de fin
  "executionRule" : "following", // Jour ouvré précédant ou suivant la date de paiement ? (following, preceding)
  "frequency" : "Monthly", // Fréquence de paiement (Daily, Weekly, EveryTwoWeeks, Monthly, EveryTwoMonths, Quarterly, SemiAnnual, Annual)
  "dayOfExecution" : "31" // Jour de l'exécution du paiement
}

// Pour les paiements de masse (bulk-payments)
{
  "batchBookingPreferred" : "true", // Le client (PSU) choisit une entrée pour toutes les transactions (true), ou une entrée par transaction (false)
  "debtorAccount" : { // Compte du débiteur
    "iban" : "FR7612345987650123456789014", // iban ou bban ou pan ou maskedPan ou msisdn ou currency
    "currency" : "EUR" // devise
  },
  "requestedExecutionDate" : "2020-01-24",
  "requestesExecutionTime" : "2020-01-24T14:41:59.000+01:00",
  "payments" : [ // Liste de paiements sans le débiteur, renseigné déjà avant
    {
      "endToEndIdentification" : "MyEndToEndId",
      "instructedAmount" : {
        "currency" : "USD",
        "amount" : ".01" // montant
      },
      "creditorAccount" : { // Compte du créditeur
        "bban" : "BARC12345612345678",
        "currency" : "EUR"
      },
      "creditorAgent" : "BICFI", // Créancier
      "creditorAgentName" : "Creditor Agent Name", // Nom du créancier
      "creditorName" : "Creditor Name", // Nom du créditeur
      "creditorAdress" : {
        "streetName" : "Rue de la PSD2",
        "buildingNumber" : "BAT 12",
        "townName" : "Aix-En-Provence",
        "postCode" : "13100",
        "country" : "FR"
      },
    } // ...
  ]
}
```

### **2. GET /v1/{payment-service}/{payment-product}/{paymentId}**
*Retourne le contenu d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **3. DELETE /v1/{payment-service}/{payment-product}/{paymentId}**
*Requête d'annulation de paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **4. GET /v1/{payment-service}/{payment-product}/{paymentId}/status**
*Retourne le statut d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **5. POST /v1/{payment-service}/{payment-product}/{paymentId}/authorisations**
*Lance le processus d'autorisation d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |
| Body            | Body   | Body de la requête                                                                                                                   |
**Exemple de body :**
```json
{
  "scaAuthenticationData" : "", // Données d'authentification forte (SCA), dépend de la méthode d'authentification. Si la donnée est binaire, alors elle sera encodé en base64
}
```

### **6. GET /v1/{payment-service}/{payment-product}/{paymentId}/authorisations**
*Retourne la liste de tous les IDs des autorisations de paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **7. GET /v1/{payment-service}/{payment-product}/{paymentId}/authorisations/{authorisationId}**
*Retourne le status de l'authentification forte (SCA) de l'autorisation de paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| authorisationId | Path   | ID de l'autorisation                                                                                                                 |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **8. PUT /v1/{payment-service}/{payment-product}/{paymentId}/authorisations/{authorisationId}**
*Met à jour les données de l'utilisateur (PSU) dans l'initiation d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| authorisationId | Path   | ID de l'autorisation                                                                                                                 |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |
| Body            | Body   | Body de la requête                                                                                                                   |
**Exemple de body : [Voir POST /v1/consents](#post-v1payment-servicepayment-productpaymentidauthorisations)**

### **9. POST /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations**
*Lance le processus d'autorisation d'annulation d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |
**Exemple de body :**
```json
{
  "scaAuthenticationData" : "", // Données d'authentification forte (SCA), dépend de la méthode d'authentification. Si la donnée est binaire, alors elle sera encodé en base64
}
```

### **10. GET /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations**
*Retourne la liste de tous les IDs de toutes les annulations de paiements*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **11. GET /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations/{cancellationId}**
*Retourne le status d'authentification forte (SCA) de l'annulation d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| cancellationId  | Path   | ID de l'annulation de paiement                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |

### **12. PUT /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations/{cancellationId}**
*Met à jour les données d'un utilisateur (PSU) pour l'annulation d'un paiement*
**Paramètres requis :**
| Nom             | Type   | Description                                                                                                                          |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| payment-service | Path   | Service de paiement ("payments", "bulk-payments", "periodic-payments")                                                               |
| payment-product | Path   | Service de paiement ("sepa-credit-transfers", "instant-sepa-credit-transfers", "target-2-payments", "cross-border-credit-transfers") |
| paymentId       | Path   | ID du paiement                                                                                                                       |
| cancellationId  | Path   | ID de l'annulation de paiement                                                                                                       |
| X-Request-ID    | Header | ID de la requête                                                                                                                     |
| Body            | Body   | Body de la requête                                                                                                                   |
**Exemple de body : [voir POST /v1/{payment-service}/{payment-product}/{paymentId}/cancellation-authorisations](#post-v1payment-servicepayment-productpaymentidcancellation-authorisations)**
