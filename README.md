## ***Activité Pratique N°4 :  Sécurité des Systèmes Distribués*** 

### ***Objectifs*** 🎯
**Partie 1** :
1. Télécharger Keycloak 23.0.1
2. Démarrer Keycloak
3. Créer un compte Admin
4. Créer une Realm
5. Créer un client à sécuriser
6. Créer des utilisateurs
7. Créer des rôles
8. Affecter les rôles aux utilisateurs
9. Avec PostMan :
    - Tester l'authentification avec le mot de passe
    - Analyser les contenus des deux JWT Access Token et Refresh Token
    - Tester l'authentification avec le Refresh Token
    - Tester l'authentification avec Client ID et Client Secret
    - Changer les paramètres des Tokens Access Token et Refresh Token

**Partie  2** :
- Sécuriser avec Keycloak les applications Wallet App

## ***Partie 1*** 
***Keycloak*** est un serveur d'authentification et d'autorisation Open Source. Il permet de gérer les utilisateurs, les rôles, les groupes, les sessions, les politiques de sécurité, etc. Il est basé sur le standard OpenID Connect et s'appuie sur le standard OAuth 2.0. Il est compatible avec les standards SAML 2.0 et LDAP.

***Quarkus*** est un framework Java open source, conçu pour créer des applications Java natives et efficaces pour le cloud. Il est basé sur des normes Java bien établies et largement adoptées, notamment Eclipse MicroProfile, Hibernate et Apache Camel via une architecture réactive.
#### 1. Démarrer Keycloak en utilisant docker

- Lancer Keycloak avec la commande : `docker run -p 8080:8080 -e KEYCLOAK_ADMIN= -e KEYCLOAK_ADMIN_PASSWORD= quay.io/keycloak/keycloak:23.0.1 start-dev`

![img_1.png](captures/img_1.png)
- Keycloak est accessible à l'adresse : http://localhost:8080

![img_2.png](captures/img_2.png)
- Se connecter avec le compte Admin créé lors de l'installation

![img_3.png](captures/img_3.png)

#### 3. Créer un Realm

Un **Realm** est un espace de travail qui regroupe des utilisateurs, des applications, des rôles, des groupes, des politiques de sécurité, etc. Il est possible de créer plusieurs Realms. Par exemple, un Realm pour les utilisateurs, un Realm pour les applications, etc.

- Créer un Realm en cliquant sur le bouton Add realm
- Donner un nom au Realm : `wallet-realm`

![img_4.png](captures/img_4.png)

#### 4. Créer un client à sécuriser

Un **client** est une application qui va être sécurisée par Keycloak. Il peut s'agir d'une application Web, d'une application mobile, d'une API, etc.

- Créer un client
- Donner un nom au client : `wallet-client`

![img_5.png](captures/img_5.png)
- Choisir l'option Confidential

![img_6.png](captures/img_6.png)

![img_7.png](captures/img_7.png)

![img_8.png](captures/img_8.png)

#### 5. Créer des utilisateurs

Les **utilisateurs** sont les personnes qui vont utiliser les applications sécurisées par Keycloak.

- Créer un utilisateur
- Donner un nom à l'utilisateur : `douhichaimae`
- Donner un email à l'utilisateur : `chaimaedouhi7@gmail.com`
- Activer l'utilisateur

![img_9.png](captures/img_9.png)
![img_10.png](captures/img_10.png)

- Créer un mot de passe pour l'utilisateur

![img_11.png](captures/img_11.png)
![img_12.png](captures/img_12.png)

#### 6. Créer des rôles

Les **rôles** sont des permissions qui vont être affectées aux utilisateurs. Par exemple, un utilisateur peut avoir le rôle `user` et un autre utilisateur peut avoir le rôle `admin`.

- Créer un rôle
- Donner un nom au rôle : `user`
- Sauvegarder le rôle

![img_13.png](captures/img_13.png)
- Créer un autre rôle
- Donner un nom au rôle : `admin`
- Sauvegarder le rôle

![img_14.png](captures/img_14.png)

#### 7. Affecter les rôles aux utilisateurs
Dans **Role Mappings**, affecter le rôle `admin` à l'utilisateur `douhichaimae`

![img_15.png](captures/img_15.png)

#### 8. Avec PostMan :
##### Tester l'authentification avec le mot de passe
![img_16.png](captures/img_16.png)
![img_17.png](captures/img_17.png)

* Access Token: JWT qui contient les informations de l'utilisateur authentifié
* Refresh Token: JWT qui permet de générer un nouveau Access Token sans avoir à s'authentifier à nouveau
* Expires In: Durée de validité du Access Token en secondes
* Refresh Expires In: Durée de validité du Refresh Token en secondes
* Session State: Identifiant de la session de l'utilisateur
* Scope: Permissions de l'utilisateur
* Token Type: Type du token (ici Bearer)

Le token se présente sous la forme suivante : `header.payload.signature`

* Header: Contient le type du token et l'algorithme de signature
* Payload: Contient les informations de l'utilisateur authentifié
* Signature: Permet de vérifier l'intégrité du token 


##### Analyser les contenus des deux JWT Access Token et Refresh Token
- Analyser le contenu du Access Token sur le site https://jwt.io/

***HEADER***
```json
{
   "alg": "RS256",
   "typ": "JWT",
   "kid": "l8-8AWQIV0wUtdEd6GAXtUgfQxMmnTxBDCqF19lJUxE"
}
```

***PAYLOAD***
```json
{
   "exp": 1701551623,
   "iat": 1701551323,
   "jti": "ddcf75a3-fa96-4dcd-8b5d-45ae613e0d5a",
   "iss": "http://localhost:8080/realms/wallet-realm",
   "aud": "account",
   "sub": "662e2ffc-b0cf-4b98-b03f-e19f5b2a64f2",
   "typ": "Bearer",
   "azp": "wallet-client",
   "session_state": "a1924b36-b992-4e56-a017-b0d37c01e34e",
   "acr": "1",
   "allowed-origins": [
      "*"
   ],
   "realm_access": {
      "roles": [
         "offline_access",
         "default-roles-wallet-realm",
         "ADMIN",
         "uma_authorization"
      ]
   },
   "resource_access": {
      "account": {
         "roles": [
            "manage-account",
            "manage-account-links",
            "view-profile"
         ]
      }
   },
   "scope": "email profile",
   "sid": "a1924b36-b992-4e56-a017-b0d37c01e34e",
   "email_verified": false,
   "name": "Chaimae DOUHI",
   "preferred_username": "douhichaimae",
   "given_name": "Chaimae",
   "family_name": "DOUHI",
   "email": "chaimaedouhi7@gmail.com"
}
```
***Signature***

`Signature = HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

- Analyser le contenu du Refresh Token sur le site https://jwt.io/

***HEADER***
```json
{
   "alg": "HS256",
   "typ": "JWT",
   "kid": "6e7a2f70-b794-4e02-8eb3-a29642b61014"
}
```

***Payload***
```json
{
   "exp": 1701553123,
   "iat": 1701551323,
   "jti": "1ac7de38-e1e6-4589-94c1-1a3e79c0932f",
   "iss": "http://localhost:8080/realms/wallet-realm",
   "aud": "http://localhost:8080/realms/wallet-realm",
   "sub": "662e2ffc-b0cf-4b98-b03f-e19f5b2a64f2",
   "typ": "Refresh",
   "azp": "wallet-client",
   "session_state": "a1924b36-b992-4e56-a017-b0d37c01e34e",
   "scope": "email profile",
   "sid": "a1924b36-b992-4e56-a017-b0d37c01e34e"
}
```
***Signature***

`Signature = HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

##### Tester l'authentification avec le Refresh Token
- Récupérer le Refresh Token du précédent test

![img_25.png](captures/img_25.png)
Dans ce cas, le Access Token est généré à partir du Refresh Token, sans avoir à s'authentifier à nouveau.

##### Tester l'authentification avec Client ID et Client Secret
- Récupérer le Client ID et le Client Secret
On peut récupérer le Client ID et le Client Secret dans l'onglet Credentials du client

* On va activer l'option Client Autentication et Service Accounts roles

![img_28.png](captures/img_28.png)
![img_27.png](captures/img_27.png)
![img_29.png](captures/img_29.png)

##### Changer les paramètres des Tokens Access Token et Refresh Token
- Dans l'onglet Settings du Realm, on peut changer les paramètres des Tokens Access Token et Refresh Token
- On peut changer la durée de validité des Tokens Access Token et Refresh Token

## ***Partie 2 : Sécuriser avec Keycloak les applications Wallet App***
 
Dans cette partie, on va sécuriser les applications Wallet App avec Keycloak. On va utiliser le protocole OpenID Connect pour sécuriser les applications Wallet App. On va utiliser le client wallet-client créé dans la partie précédente.
### 1) ***ebank-service-unsecured project***
GET http://localhost:8084/currencyDeposits
![img_31.png](captures/img_31.png)

POST http://localhost:8084/currencyTransfer
![img_32.png](captures/img_32.png)

Ajout des dépendances suivantes dans le pom.xml
```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
   <groupId>org.keycloak</groupId>
   <artifactId>keycloak-spring-boot-starter</artifactId>
   <version>23.0.1</version>
</dependency>
```

Ajout des configurations suivantes dans le application.properties
```properties
keycloak.realm=ebank-realm
keycloak.resource=ebank-client
keycloak.bearer-only=true
keycloak.auth-server-url=http://localhost:8080
keycloak.ssl-required=none
```

Ajout de la classe KeycloakAdapterConfig
```java
@Configuration
public class KeycloakAdapterConfig {
    @Bean
    public KeycloakSpringBootConfigResolver keycloakConfigResolver(){
        return new KeycloakSpringBootConfigResolver();
    }
}
```

Ajout de la classe SecurityConfig
```java
@KeycloakConfiguration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter {

    @Override
    protected SessionAuthenticationStrategy sessionAuthenticationStrategy() {
        return new RegisterSessionAuthenticationStrategy(new SessionRegistryImpl());
    }
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.authenticationProvider(keycloakAuthenticationProvider());
    }
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
        http.csrf().disable();
        http.cors();
        http.headers().frameOptions().disable();
        http.authorizeRequests()
                .antMatchers("/h2-console/**", "/swagger-ui/**", "/v3/**",
                        "/public/**", "/actuator.**").permitAll();
        http.authorizeRequests().anyRequest().authenticated();
    }
}
```
![img_34.png](captures/img_34.png)

Protection des méthodes en utilisant les rôles
```java
@RestController
@CrossOrigin("*")
public class EBankRestController {
    @Autowired
    private EBankServiceImpl eBankService;
    @PostMapping("/currencyTransfer")
    @PreAuthorize("hasAuthority('ADMIN')")
    public CurrencyTransferResponse currencyTransfer(@RequestBody NewWalletTransferRequest request){
        return this.eBankService.newWalletTransaction(request);
    }
    @GetMapping("/currencyDeposits")
    @PreAuthorize("hasAuthority('USER')")  
    public List<CurrencyDeposit> currencyDepositList(){
        return eBankService.currencyDeposits();
    }
}
```

En utilisant Http client tool, on va tester les méthodes de l'API ebank-service-unsecured

```Text
POST http://localhost:8080/realms/wallet-realm/protocol/openid-connect/token
Accept: application/x-www-form-urlencoded
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=douhichaimae&password=123&client_id=wallet-client


###

GET http://localhost:8085/currencyDeposits
Accept: application/json
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJCbGVaWTlhd2MzMHJ0ajZpMXNqT2JUQzF3T242Q2FQSG9VTUtjUlNvVmh3In0.eyJleHAiOjE3MDE2MDc5NDgsImlhdCI6MTcwMTYwNzY0OCwianRpIjoiODA2ODBhMGQtMDcyMy00MThjLWJjNDctZmM5NDFhN2M1NmNkIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy93YWxsZXQtcmVhbG0iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiODBkYmM0NjktZTRkMy00NjVhLThlYzEtZmNhNWNkOTU0MmVjIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoid2FsbGV0LWNsaWVudCIsInNlc3Npb25fc3RhdGUiOiJkYmU1Y2MwNS01ZWNhLTQ5MDctODMwNy1kZTJmZmQ1Njk4MGEiLCJhY3IiOiIxIiwiYWxsb3dlZC1vcmlnaW5zIjpbIioiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iLCJBRE1JTiIsImRlZmF1bHQtcm9sZXMtd2FsbGV0LXJlYWxtIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIiwic2lkIjoiZGJlNWNjMDUtNWVjYS00OTA3LTgzMDctZGUyZmZkNTY5ODBhIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJuYW1lIjoiQ2hhaW1hZSBET1VISSIsInByZWZlcnJlZF91c2VybmFtZSI6ImRvdWhpY2hhaW1hZSIsImdpdmVuX25hbWUiOiJDaGFpbWFlIiwiZmFtaWx5X25hbWUiOiJET1VISSIsImVtYWlsIjoiY2hhaW1hZWRvdWhpN0BnbWFpbC5jb20ifQ.ArHTvPCgxvmE-GTTXVBGqNF99PgHLf3ILgN1ZiUsefUg4qNJTixUeRNmoV64lS2dOmluMPg1rohCvJf2Lfeccq4HTMRYvdcN-Z2szodfo99HrBoZcsMK0ypV8oJmOEDVGQm56F8gxPPBAn2GoDChoJszmQLDlFVzhl0qB5O6mACT-19bnqR5Rjnx51kcULVula_Ca8MdUNtTsAemcSCM2Rt3XP5Ou_mJJAyDCzwU71FhEWYYvvN8nswxn-GHeY6vDSbeAY7ZQz9qeRuYVwdMMUVS2RStsBvL-1C5fFOwoa2jwEUxgeLvAh0Od4dNGhWQzTSBKpvbf1Z65LIVLwmTsw


###

POST http://localhost:8085/currencyTransfer
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJCbGVaWTlhd2MzMHJ0ajZpMXNqT2JUQzF3T242Q2FQSG9VTUtjUlNvVmh3In0.eyJleHAiOjE3MDE2MDc5NDgsImlhdCI6MTcwMTYwNzY0OCwianRpIjoiODA2ODBhMGQtMDcyMy00MThjLWJjNDctZmM5NDFhN2M1NmNkIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy93YWxsZXQtcmVhbG0iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiODBkYmM0NjktZTRkMy00NjVhLThlYzEtZmNhNWNkOTU0MmVjIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoid2FsbGV0LWNsaWVudCIsInNlc3Npb25fc3RhdGUiOiJkYmU1Y2MwNS01ZWNhLTQ5MDctODMwNy1kZTJmZmQ1Njk4MGEiLCJhY3IiOiIxIiwiYWxsb3dlZC1vcmlnaW5zIjpbIioiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iLCJBRE1JTiIsImRlZmF1bHQtcm9sZXMtd2FsbGV0LXJlYWxtIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJlbWFpbCBwcm9maWxlIiwic2lkIjoiZGJlNWNjMDUtNWVjYS00OTA3LTgzMDctZGUyZmZkNTY5ODBhIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJuYW1lIjoiQ2hhaW1hZSBET1VISSIsInByZWZlcnJlZF91c2VybmFtZSI6ImRvdWhpY2hhaW1hZSIsImdpdmVuX25hbWUiOiJDaGFpbWFlIiwiZmFtaWx5X25hbWUiOiJET1VISSIsImVtYWlsIjoiY2hhaW1hZWRvdWhpN0BnbWFpbC5jb20ifQ.ArHTvPCgxvmE-GTTXVBGqNF99PgHLf3ILgN1ZiUsefUg4qNJTixUeRNmoV64lS2dOmluMPg1rohCvJf2Lfeccq4HTMRYvdcN-Z2szodfo99HrBoZcsMK0ypV8oJmOEDVGQm56F8gxPPBAn2GoDChoJszmQLDlFVzhl0qB5O6mACT-19bnqR5Rjnx51kcULVula_Ca8MdUNtTsAemcSCM2Rt3XP5Ou_mJJAyDCzwU71FhEWYYvvN8nswxn-GHeY6vDSbeAY7ZQz9qeRuYVwdMMUVS2RStsBvL-1C5fFOwoa2jwEUxgeLvAh0Od4dNGhWQzTSBKpvbf1Z65LIVLwmTsw

{
  "sourceWalletId": "W1",
  "sourceWalletCurrency": "EUR",
  "destinationWalletId": "W1",
  "destinationWalletCurrency": "MAD",
  "amount": 1000
}


```
![img_35.png](captures/img_35.png)
### 2) ***wallet-service project***

Cette application est basée sur GraphQL.
* Query : Permet de récupérer des données

On va récupérer les continents 
![img_40.png](captures/img_40.png)

Récupérer les pays d'un continent
![img_39.png](captures/img_39.png)

Récupérer les wallets
![img_42.png](captures/img_42.png)

* Mutation : Permet de modifier des données

![img_43.png](captures/img_43.png)

Ajout des dépendances suivantes dans le pom.xml
```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.keycloak</groupId>
    <artifactId>keycloak-spring-boot-starter</artifactId>
    <version>23.0.1</version>
</dependency>
```
Dans le fichier application.properties, on va ajouter les configurations suivantes
```properties
keycloak.realm=wallet-realm
keycloak.resource=wallet-client
keycloak.bearer-only=true
keycloak.auth-server-url=http://localhost:8080
keycloak.ssl-required=none
```

Dans la classe SecurityConfig, on va ajouter les configurations suivantes
```java
@Override
protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
        http.csrf().disable();
        http.cors();
        http.headers().frameOptions().disable();
        http.authorizeRequests()
        .antMatchers("/h2-console/**", "/graphql/**","/graphiql/**").permitAll();
        http.authorizeRequests().anyRequest().authenticated();
        }
        }
```

On va protéger les méthodes en utilisant les rôles
```java
@MutationMapping
@PreAuthorize("hasAuthority('ADMIN')")
public Wallet addWallet(@Argument String currencyCode, @Argument double initialBalance,
    Principal principal){
    KeycloakAuthenticationToken authenticationToken= (KeycloakAuthenticationToken) principal;
    AccessToken token = authenticationToken.getAccount().getKeycloakSecurityContext().getToken();
    String username = token.getPreferredUsername();
    return walletService.newWallet(currencyCode,initialBalance,username);
}
```

### 3) ***Wallet-Frontend project in Angular***
![img_44.png](captures/img_44.png)
On va installer ***keycloak-angular*** et ***keycloak-js***
```Text
npm install keycloak-angular keycloak-js --save
``` 

Dans AppModule, on va importer KeycloakAngularModule et KeycloakService et on va ajouter la méthode init
```javascript 
export function kcFactory(kcService : KeycloakService){
  return ()=>{
    kcService.init({
      config : {
        realm :"wallet-realm",
        clientId : "wallet-client",
        url : "http://localhost:8080"
      },
      initOptions : {
        onLoad : "login-required",
        checkLoginIframe : true
      }
    })
  }
}
```
Dans providers, on va ajouter la ligne suivante
```javascript

  providers: [
    {
      provide : APP_INITIALIZER,
      useFactory : kcFactory,
      multi : true
    }
  ]
```
Maintenant on va se rediriger automatiquement vers Keyclock pour s'authentifier

![img.png](captures/im.png)

On va maintenant protéger les routes de l'application en utilisant ***AuthGuardService***
```javascript
import { Injectable } from '@angular/core';
import {
  ActivatedRouteSnapshot,
  Router,
  RouterStateSnapshot
} from '@angular/router';
import { KeycloakAuthGuard, KeycloakService } from 'keycloak-angular';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard extends KeycloakAuthGuard {
  constructor(
    protected override readonly router: Router,
    protected readonly keycloak: KeycloakService
  ) {
    super(router, keycloak);
  }

  public async isAccessAllowed(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ) {
    if (!this.authenticated) {
      await this.keycloak.login({
        redirectUri: window.location.origin
      });
    }

    const requiredRoles = route.data['roles'];

    if (!(requiredRoles instanceof Array) || requiredRoles.length === 0) {
      return true;
    }

    return requiredRoles.every((role) => this.roles.includes(role));
  }
}
```
Dans ***app-routing.module.ts***, on va ajouter les rôles nécessaires pour accéder aux routes
```javascript
 {
    path : "wallets", component : WalletsComponent,
    canActivate : [AuthGuard], data : {roles : ['USER']}
  },
  {
    path : "transactions/:walletId", component : WalletTransactionsComponent,
    canActivate : [AuthGuard], data : {roles : ['USER','ADMIN']}
  }
```

On va ajouter un fichier security.service.ts
```javascript
import {Injectable} from "@angular/core";
import {KeycloakProfile} from "keycloak-js";
import {KeycloakEventType, KeycloakService} from "keycloak-angular";

@Injectable({providedIn : "root"})
export class SecurityService {
  public profile : KeycloakProfile;
  constructor (public kcService: KeycloakService) {
    this.init();
  }
  init(){
    this.kcService.keycloakEvents$.subscribe({
      next: (e) => {
        if (e.type == KeycloakEventType.OnAuthSuccess) {
          this.kcService.loadUserProfile().then(profile=>{
            this.profile=profile;
          });
        }
      }
    });
  }
  public hasRoleIn(roles:string[]):boolean{
    let userRoles = this.kcService.getUserRoles();
    for(let role of roles){
      if (userRoles.includes(role)) return true;
    } return false;
  }
}
```

Dans navbar.component.html

```html
<a class="nav-link"  *ngIf="securityService.profile; else loginTemplate">
    <a>{{securityService.profile.firstName}}</a>
</a>
```

* login method
```javascript
async login() {
    await this.securityService.kcService.login(
      {
        redirectUri: window.location.origin
      }
    );
  }
```

L'utilisateur connécté est affiché dans la navbar

![img_3.png](captures/im_3.png)


* logout method
```javascript
onLogout() {
    this.securityService.kcService.logout(
      {
        redirectUri: window.location.origin
      }
    );
  }
```


## Conclusion ✨
Dans cette activité pratique, on a pu découvrir Keycloak et comment sécuriser les applications Wallet App avec Keycloak.
Keycloak est un serveur d'authentification et d'autorisation Open Source. Il permet de gérer les utilisateurs, les rôles, les groupes, les sessions, les politiques de sécurité, etc. Il est basé sur le standard OpenID Connect et s'appuie sur le standard OAuth 2.0. Il est compatible avec les standards SAML 2.0 et LDAP.






















