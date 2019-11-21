# DSP2

## 1. Contexte

En décembre 2015, la Commission Européenne publie la deuxième directive sur les services de paiements : la directive DSP2 (ou PSD2 en anglais). L’établissement de ces nouvelles réglementations fait suite à différentes observations de l’évolution du secteur bancaire :

- Le manque de compétition empêchant une réelle évolution des services proposés par les banques dites « traditionnelles ». On trouve des monopoles bancaires dans plusieurs pays européens. En Angleterre 80% des activités bancaires sont contrôlées par les cinq banques principales.

- L’arrivée de nouveaux acteurs ; les fintechs, qui, grâce à la technologie proposent des services financiers.

- L’émergence du commerce en ligne a renforcé la demande de paiement par service tiers (l’e-commerce représente aujourd’hui 80 milliards d’euros) et la nécessité de sécuriser les transactions (Europol dénombre en 2013 à 1.44 milliards d’euros l’étendue des transactions bancaires frauduleuses).

Avec la DSP2, la Commission Européenne a de multiples objectifs : faire évoluer le secteur bancaire, stimuler son innovation, lisser les pratiques entre anciens et nouveaux acteurs, encourager les évolutions technologiques tout en les réglementant et proposer aux utilisateurs une meilleure expérience bancaire.

## 2. Les acteurs

- EBA (European Banking Authority) :
  L’Autorité bancaire européenne se charge de donner un cadre à la directive DSP2 qui, sur certains détails, reste relativement floue.

- ASPSP (Account Servicing Payment Service Providers) :
  Les banques ou les acteurs appelés « services de paiement gestionnaire du compte » ou « ASPSP » (Account Servicing Payment Service Providers) sont les institutions au cœur du projet de la DSP2.

- TPP (Third-Party Providers) :
  Les prestataires de services tiers sont l’autre partie essentielle de la directive DSP2. Ce sont des fintechs qui grâce à leur technologie proposent de nouveaux services aux consommateurs.
  On peut distinguer ces prestataires de services tiers en deux catégories :

  - AISP (Account Information Service Providers) :
    Ils permettent aux utilisateurs de regrouper leurs informations bancaires sur une même plateforme quand ils ont de multiples comptes.

  - PISP (Payment Initiation Service Providers) :
    Ils permettent d’initier un paiement directement depuis leur plateforme.

  - PIISP (Payment Istrument Issuer Service Provider):
    Fournissent des cartes de paiement qui sont reliées au compte bancaire.

- PSU (Payment Services User) :
  Il s’agit des utilisateurs de ces nouveaux services financiers.

- Les fournisseurs de technologies :
  Ils fournissent des API ouvertes, permettant aux acteurs financiers de :
  - donner **des accès aux tiers** (AISP et PISP) via un système OAuth2 (ou autre)
  - fournir une **authentification sécurisée** aux utilisateurs
  - permettre une gestion du **consentement** des utilisateurs chez les tiers TPP
  - contrôler la **validité d’accès du TPP** et son droit sur les différentes données des clients ou actions possibles

## 3. DSP2 et ses préconisations techniques

- Pour que tout paiement soit autorisé, il faut que l’utilisateur donne son **consentement explicite** au TPP (art. 64)
  Ce consentement explicite doit exposer clairement les données partagées au TPP ainsi que les actions qu’il peut réaliser.

- **Au dessous de 150€** : l’authentification et autres règles que doivent suivre les prestataires ne s’appliquent pas (art.63)

- Avec l’accord explicite du client, le TPP peut être en charge d’indiquer la **disponibilité des fonds** lors d’une transaction (art. 65)

- Les PISP sont garants du bon déroulement du paiement et sont responsables de la **sécurité des données**, ils doivent y avoir un accès limité qui respecte leur besoin de gestion sans plus ni moins (art. 66)

- Les AISP **doivent protéger les données récoltées**, mais ne peuvent les stocker ou les utiliser que dans des mesures de sécurité. Celles-ci doivent être collectées avec un consentement explicite de l’utilisateur (art. 67)

- Les TPP peuvent **émettre des instruments de paiement**. Ils doivent dans ce cadre supporter les charges et mesures de sécurité liées à l’utilisation de cet instrument par le client (art. 70)

- **Le TPP doit prouver le bon déroulement d’une transaction** lorsque l’utilisateur nie avoir autorisé un paiement ou qu’il s’est mal déroulé. Si le paiement n’était pas autorisé, il revient au TPP d’effectuer un remboursement immédiat et doit indemniser les différentes parties s’il se trouvait responsable de la faute. Si l’authentification demandée à l’utilisateur ce dernier ne doit alors subir aucune perte liée à l’utilisation d’un instrument de paiement (art. 72-73-74)

- Un refus de paiement par un prestataire se doit d’être toujours accompagné d’une justification à l’utilisateur (art. 79)

- **Un ordre de paiement est irrévocable** une fois que l’utilisateur a donné son ordre il ne peut revenir en arrière (art. 80)

- Les paiements doivent être perçus dans la limite d’un jour, le paiement doit respecter les conditions de l’utilisateur à savoir montant, bénéficiaire, etc (art. 84 à 87)

- Les TPP peuvent utiliser les données si **l’accord est explicitement exprimé** par les utilisateurs, ces données peuvent être utilisées, que dans le cadre de démarches sécuritaires (fraude, prévention) (art. 94)

- Les TPP se doivent de maintenir des **systèmes de sécurité élevés** afin de prévenir tout incident (art. 95)

- **Tout incident opérationnel doit être reporté aux autorités** puis à l’ABE, la BCE et aux utilisateurs des TPP (art. 96)

- Une **authentification forte** (2 facteurs d’authentification ou 2FA) est exigée pour l’accès au compte bancaire, le paiement électronique ou toute opération à risque de fraude

Ces articles ne donnent pas trop de détails concernant des points pratiques pourtant nécessaires à des applications concrètes. Avant qu’une jurisprudence fournie n’existe, des standards portés par plusieurs banques ont émergé pour tenter de concrétiser et détailler les articles de la directive : **Berlin Group, Open Banking, STET**

## 4. Les différents types de Strong Customer Authentication (SCA)

Dans le cadre de la directive DSP2, une authentification forte du client (SCA) est imposée afin de répondre à un besoin de sécurité face à de nombreuses transactions frauduleuses. Ce nouveau processus vise à s’assurer de l’identité du client au travers de deux éléments au moins qui correspondent aux critères suivants: être quelque chose que le seul le client sait (code PIN par exemple) ou que seul lui détient (un code envoyé sur le téléphone portable) ou que le client est (données biométriques). Certains systèmes existants sont déjà conformes avec cette norme comme 3DSecure par exemple. Au travers de DSP2 il est possible d’avoir différentes approches techniques de SCA. Voici les trois approches et leurs particularités :

- **Redirect** : l’approche « redirigée » consiste à procéder à l’étape d’authentification sur la plateforme de l’ASPSP, le client « quitte » donc la plateforme du TPP pour s’authentifier sur celle de l’ASPSP puis sera redirigé vers l’interface du TPP une fois l’authentification terminée

<p align="center">
  <image src="img/Redirect.jpg">
</p>

- **Decoupled** : ici l’authentification s’effectue directement au travers de l’ASPSP sans redirection. En effet, le client va donner ses identifiants d’authentification au TPP qui va par la suite le transmettre à l’ASPSP. L’ASPSP grâce à ces identifiants effectuera le processus d’authentification, sans que le client n’ai quitté pendant tout ce temps la plateforme du TPP.

<p align="center">
  <image src="img/Decoupled.jpg">
</p>

- **Embedded** : À travers de cette approche, le client donne non pas ses identifiants, mais les clés de son authentification comme quelque chose qu’il connait ou qu’il possède au TPP (un code éphémère envoyé par le ASPSP sur le téléphone, un code PIN uniquement en possession du client). Le TPP va par la suite transmettre ces réponses à l’ASPSP pour valider l’authentification.

<p align="center">
  <image src="img/Embedded.jpg">
</p>

## 3. Plus d'informations

- http://blog.particeep.com/analyse-tout-ce-que-vous-devez-savoir-sur-la-dsp2-et-linfrastructure-api/
