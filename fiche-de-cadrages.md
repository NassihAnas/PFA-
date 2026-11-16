# Fiche de suivi des projets agents IA

Master • Présentation de l’avancement et plan de correction

Groupe : Binôme NASSIH   Projet n° : 12   Date : 16/11/2026

Intitulé du projet : Plateforme intelligente de gestion événementielle et d'assistance IA dédiée au Mondial 2030 (Maroc – Espagne – Portugal)

Membres : Anas NASSIH, Houssam NASSIH

Jalon examiné : Jalon 1 — Démarrage & Cadrage   Prochain point : Jalon 2 — Architecture Data, RAG/LLM & Solveur OR-Tools

## Utilisation de la fiche

Avant chaque point, chaque groupe complète les réponses et rassemble les preuves. Présenter en 10 minutes : bilan, démonstration, résultats et blocages. Prévoir ensuite 5 minutes de questions. Chaque membre explique sa contribution et un choix technique.

Pour chaque rubrique, distinguer ce qui est prévu, commencé, démontré ou bloqué. Une fonction est démontrée lorsqu’un scénario reproductible et son résultat peuvent être vérifiés. Justifier toute question sans objet. Si un élément manque, indiquer la correction et son échéance.

## Bilan et périmètre

**1. Quel problème métier traitez-vous, pour quel utilisateur et avec quel résultat attendu ?**

Preuve à présenter : Un cas d’usage précis et un critère de réussite mesurable.

Réponse du groupe : 
* **Problème métier :** À l’horizon de la Coupe du Monde 2030 (Maroc, Espagne, Portugal), l’afflux massif de visiteurs et de professionnels génère une forte complexité pour coordonner les inscriptions, allouer les ressources limitées (salles, transports, guides, hébergements) sans conflits et orienter efficacement les utilisateurs selon leurs besoins.
* **Utilisateurs cibles :** Touristes et supporters internationaux (loisirs et packs confort), participants et organisateurs d’événements professionnels (conférences, forums B2B) et administrateurs de la plateforme.
* **Résultat attendu :** Une plateforme unifiée structurée autour de **3 piliers** (1. Sorties & guides touristiques au Maroc, 2. Événements professionnels, 3. Packs confort Mondial 2030), couplée à un moteur de planification sous contraintes (**Google OR-Tools**) et à un **Agent IA conversationnel (LLM)** dédié à chaque pilier pour conseiller, recommander et assister les utilisateurs.
* **Critère de réussite mesurable :** Génération de plannings et d'itinéraires sans aucun conflit de ressources ($0\%$ de double réservation) en moins de $3\text{ secondes}$, et taux de pertinence $> 85\%$ des réponses et recommandations fournies par l'agent IA sur un jeu de questions-réponses de test.

**2. Quel périmètre MVP avez-vous retenu et quelles fonctions avez-vous reportées ?**

Preuve à présenter : Liste des fonctions incluses et exclues, validée par le groupe.

Réponse du groupe : 
* **Périmètre MVP retenu (Prévu / Commencé) :**
  * Architecture multi-piliers (Tourisme Maroc, Événements Pro, Packs Mondial 2030) avec gestion des inscriptions et contrôle des capacités.
  * Visualisation interactive des plannings via **FullCalendar** (React).
  * Moteur d'optimisation et de recommandation sous contraintes via **Google OR-Tools** (Spring Boot).
  * Intégration d'un assistant conversationnel **LLM** spécialisé par pilier (via RAG / Fine-tuning léger sur notre dataset événementiel) capable de répondre aux questions et de déclencher des recherches de créneaux ou de packs.
* **Fonctions reportées (Hors MVP) :**
  * Passerelle de paiement bancaire réelle (remplacée par une simulation de réservation).
  * Gestion des déplacements transfrontaliers temps réel (vols/trains Espagne-Portugal).
  * Application mobile native et notifications SMS.

**3. Qu’aviez-vous prévu depuis le dernier point et qu’avez-vous réellement terminé ?**

Preuve à présenter : Fonctions terminées, en cours et bloquées, avec preuve pour chacune.

Réponse du groupe : 
* **Statut :** Jalon de démarrage (Point 0).
* **Terminé :** Définition du périmètre fonctionnel des 3 piliers, validation de l'architecture technique imposée (React, Spring Boot, PostgreSQL, Google OR-Tools, FullCalendar, Docker, JUnit, Playwright) et conception du schéma conceptuel de données.
* **En cours :** Structuration et collecte du dataset d'événements (sites hôtes Maroc 2030, circuits touristiques, événements B2B) et initialisation du dépôt Git avec environnement Docker.
* **Bloqué :** Aucun blocage technique à ce stade ; arbitrage en cours sur le choix entre RAG pur et Fine-tuning (LoRA) pour l'adaptation du LLM sur nos ressources matérielles.

**4. Que peut-on exécuter aujourd’hui sans intervention manuelle cachée ?**

Preuve à présenter : Démonstration de bout en bout sur un scénario reproductible.

Réponse du groupe : *(Phase de démarrage)* Aucune démonstration de bout en bout n'est encore exécutable. L'architecture de base (`docker-compose.yml` avec PostgreSQL et squelette Spring Boot / React) est en cours d'initialisation pour permettre une première démonstration au Jalon 2.

État général : conforme au jalon

Décision ou aide attendue de l’enseignant : Validation de la stratégie d'entraînement/adaptation du LLM (Fine-tuning LoRA vs RAG sur dataset structuré) et confirmation de l'articulation entre le LLM et le solveur déterministe Google OR-Tools.

## Données et architecture

**5. Quel dataset ou corpus utilisez-vous exactement et comment y accède-t-on ?**

Preuve à présenter : Nom, URL, version ou date, licence et fichiers retenus.

Réponse du groupe : Nous construisons un dataset hybride structuré (`dataset_mondial2030_v1.json` / `.csv`) stocké dans **PostgreSQL** et versionné dans le dépôt Git du projet. Il agrège :
1. **Données officielles publiques (Licence ouverte / Open Data) :** Calendrier prévisionnel et villes hôtes du Mondial 2030 (Casablanca, Rabat, Marrakech, Agadir, Fès, Tanger), infrastructures et données touristiques de l'ONMT (Office National Marocain du Tourisme).
2. **Corpus d'événements professionnels et touristiques :** Base de données structurée d'événements B2B, guides certifiés, salles et packs d'hébergement/transport générée et curatée pour couvrir les 3 piliers.

**6. Les données couvrent-elles réellement le problème posé ? Que simulez-vous ?**

Preuve à présenter : Variables utiles, labels, limites et distinction entre réel et synthétique.

Réponse du groupe : 
* **Variables utiles :** `pilier_id` (Tourisme, Pro, Pack Mondial), `ville`, `date_debut`, `date_fin`, `capacite_max`, `ressources_requises`, `tarif`, `langue_guide`, `contraintes_horaires`, ainsi que des paires `(question_utilisateur, contexte_evenement, reponse_attendue, appel_outil)` pour l'agent IA.
* **Réel vs Synthétique :** Les lieux géographiques, les stades/villes hôtes du Maroc 2030 et les attractions touristiques sont **réels**. Les plannings détaillés des conférences B2B, les disponibilités des guides, les stocks de packs confort et les historiques d'inscriptions sont **synthétiques** (générés par script Python) afin de simuler une charge réaliste pendant la compétition.

**7. Comment avez-vous nettoyé, annoté et séparé les données sans fuite ?**

Preuve à présenter : Pipeline, exemples avant/après, splits et règles anti-doublons.

Réponse du groupe : 
* **Pipeline de nettoyage :** Script de prétraitement normalisant les formats de dates (ISO 8601 UTC), dédupliquant les événements par clé composite `(titre, ville, date_debut)` et validant l'intégrité référentielle (capacité $> 0$).
* **Annotation et séparation (Anti-fuite) :** Pour l'évaluation du LLM et des recommandations, les scénarios utilisateurs sont séparés de manière stricte en **80 % entraînement / base de connaissances** et **20 % jeu de test isolé** (`test_queries_2030.json`), en veillant à ce qu'aucune reformulation d'une question de test ne figure dans le jeu d'entraînement.

**8. Quelle insuffisance des données risque de fausser votre conclusion ?**

Preuve à présenter : Biais identifié et mesure concrète pour en réduire l’effet.

Réponse du groupe : 
* **Biais identifié :** Une surreprésentation des événements dans les grandes villes (Casablanca/Rabat) ou des durées standardisées peut biaiser les recommandations du LLM et sous-estimer la complexité combinatoire du solveur OR-Tools.
* **Mesure corrective :** Équilibrage stratifié du dataset synthétique sur les 6 villes hôtes marocaines et injection volontaire de scénarios de saturation (pics d'affluence les jours de match) dans le jeu de test.

**9. Quels composants fonctionnent et comment échangent-ils les données ?**

Preuve à présenter : Schéma d’architecture à jour et exemple de requête API.

Réponse du groupe : *(Architecture cible en cours de déploiement)* :
* **Frontend (React + FullCalendar)** $\leftrightarrow$ **Backend (Spring Boot REST API)** en JSON.
* **Backend Spring Boot** $\leftrightarrow$ **PostgreSQL** via Spring Data JPA (stockage des événements, inscriptions, ressources et logs de chat).
* **Backend Spring Boot** $\leftrightarrow$ **Google OR-Tools (Java)** pour le calcul des plannings sans conflit et la recommandation sous contraintes.
* **Backend Spring Boot** $\leftrightarrow$ **Service Agent IA (LLM)** via API REST interne (injection du contexte dynamique issu de PostgreSQL et appel d'outils OR-Tools).
* *Exemple de requête API cible :* `POST /api/v1/chat/pilier-mondial` avec `{"userId": 42, "pilier": "PACK_MONDIAL", "message": "Je cherche un pack 3 jours à Marrakech autour d'un quart de finale avec guide francophone"}`.

**10. Pourquoi avez-vous choisi ces frameworks plutôt qu’une solution plus simple ?**

Preuve à présenter : Justification liée au besoin, aux compétences et aux ressources.

Réponse du groupe : Cette stack répond aux exigences techniques du cahier des charges et aux contraintes du domaine :
* **Spring Boot & PostgreSQL :** Indispensables pour garantir l'intégrité transactionnelle (ACID) lors des inscriptions simultanées sur des places limitées.
* **React & FullCalendar :** Nécessaires pour offrir une interface fluide et visuelle permettant de manipuler les plannings des 3 piliers.
* **Google OR-Tools + LLM :** Un LLM seul hallucine sur les disponibilités et échoue à résoudre des problèmes combinatoires de planning ; le coupler à OR-Tools garantit des solutions mathématiquement valides tout en conservant une interaction naturelle.

**11. Comment gérez-vous l’état, la mémoire, les erreurs et les reprises ?**

Preuve à présenter : Modèle d’état, journal d’une exécution et exemple d’échec traité.

Réponse du groupe : 
* **État & Mémoire :** L'état métier (réservations, ressources) est persisté dans PostgreSQL avec verrouillage optimiste (`@Version`). L'historique conversationnel de chaque pilier est sauvegardé par session utilisateur (`chat_sessions`).
* **Erreurs & Reprises :** Gestion centralisée des exceptions dans Spring Boot (`@ControllerAdvice`). Si le LLM est indisponible ou dépasse le délai imparti, un mécanisme de repli (*fallback*) bascule sur une recherche multicritères classique dans PostgreSQL. Si OR-Tools renvoie `INFEASIBLE`, l'état antérieur est préservé et l'utilisateur est invité à assouplir une contrainte.

Correction prioritaire sur les données ou la conception : Finaliser la génération et la validation du dataset `dataset_mondial2030_v1.json` (minimum 300 événements répartis sur les 3 piliers) d'ici le Jalon 2.

## Fonctionnement des agents et protocole

**12. Quelle décision l’agent prend-il et quels outils appelle-t-il ?**

Preuve à présenter : Trace d’un scénario avec entrées, décisions, appels et sorties.

Réponse du groupe : L'agent IA agit comme un conseiller spécialisé par pilier (Tourisme, Pro, Packs Mondial). Face à une requête utilisateur, il identifie l'intention, extrait les contraintes (dates, ville, budget, thématique) et appelle trois outils :
1. `SearchCatalogTool` : Interroge PostgreSQL sur le pilier concerné.
2. `ConstraintPlannerTool` : Appelle **Google OR-Tools** pour vérifier la faisabilité horaire et assembler un planning ou un pack sans chevauchement.
3. `BookingDraftTool` : Prépare une pré-inscription ou une recommandation structurée.

**13. Quel résultat obtenez-vous avec des règles ou une chaîne sans agent ?**

Preuve à présenter : Comparaison sur les mêmes cas, avec le même budget de calcul.

Réponse du groupe : *(Baseline prévue pour le Jalon 2)* Une recherche par filtres SQL classiques sans agent ni OR-Tools oblige l'utilisateur à vérifier manuellement les conflits horaires entre ses sorties touristiques, ses conférences B2B et les matchs du Mondial, entraînant un taux d'échec ou de chevauchement estimé à $> 30\%$ sur des séjours multi-activités.

**14. Si plusieurs agents collaborent, quel rôle distinct remplit chacun ?**

Preuve à présenter : Rôles, échanges et comparaison avec un agent unique. Sinon expliquer l’absence de multi-agents.

Réponse du groupe : Nous mettons en place une **architecture routée par pilier** : un routeur léger oriente la requête vers l'un des **3 agents spécialisés** (1. *Agent Guide Touristique Maroc*, 2. *Agent Événements Professionnels*, 3. *Agent Packs Confort Mondial 2030*), chacun disposant de son propre prompt système, de son sous-ensemble de connaissances et d'un accès aux outils communs (PostgreSQL + OR-Tools). Cette séparation évite la confusion de contexte entre des besoins touristiques et des impératifs logistiques B2B.

**15. Comment vérifiez-vous une sortie du LLM avant de l’utiliser ?**

Preuve à présenter : Validation du format, contrôle des sources, test métier et cas d’abstention.

Réponse du groupe : 
1. **Validation de format :** Parsing strict en JSON (Structured Outputs) validé côté Spring Boot avant tout appel d'outil.
2. **Contrôle métier (Grounding) :** Chaque identifiant d'événement, tarif ou créneau cité par le LLM est vérifié en temps réel dans PostgreSQL et validé par Google OR-Tools.
3. **Cas d'abstention :** Si un utilisateur demande un événement hors périmètre (ex : match hors Maroc/Espagne/Portugal ou ressource épuisée), l'agent a pour consigne stricte de s'abstenir d'inventer une offre et de proposer la meilleure alternative disponible en base.

**16. Quelles actions exigent une validation humaine et quelles limites imposez-vous ?**

Preuve à présenter : Permissions, plafond d’appels, coût maximal et scénario d’action refusée.

Réponse du groupe : 
* **Validation humaine :** L'agent ne peut jamais valider définitivement une inscription payante, modifier le planning global d'un événement ou supprimer une ressource. Il génère uniquement une *proposition/pré-réservation* que l'utilisateur (ou l'administrateur) doit confirmer explicitement via l'interface React.
* **Limites :** Plafond de $3$ appels d'outils par tour de conversation, timeout de $5\text{ secondes}$ sur OR-Tools et limitation de débit (*rate limiting*) à $20$ requêtes/minute par utilisateur.

**17. Quels tests prouvent que les fonctions annoncées marchent réellement ?**

Preuve à présenter : Tests automatisés, résultat daté et anomalies encore ouvertes.

Réponse du groupe : *(Plan de tests prévu)*
* **Tests unitaires & d'intégration (JUnit 5) :** Vérification des règles d'inscription, de la non-collision des ressources dans OR-Tools et du parsing des réponses de l'agent.
* **Tests End-to-End (Playwright) :** Scénarios automatisés simulant un utilisateur naviguant sur les 3 piliers, posant une question au chat IA, visualisant le créneau recommandé sur FullCalendar et validant son inscription.

**18. Quels indicateurs avez-vous choisis et pourquoi conviennent-ils au projet ?**

Preuve à présenter : Définition des métriques, baseline et protocole de mesure.

Réponse du groupe : 
1. **Taux de plannings/packs valides sans conflit ($\%$) :** Garantit la fiabilité mathématique du couple LLM + OR-Tools.
2. **Taux de fidélité aux données (Faithfulness / Absence d'hallucination, $\%$) :** Mesure que les prix, dates et lieux cités par le chat existent réellement en base.
3. **Latence bout-en-bout ($\text{ms}$) :** Garantit une expérience conversationnelle fluide ($< 3000\text{ ms}$).

Correction prioritaire sur l’agent ou les tests : Développer le prototype de l'agent spécialisé avec appels d'outils OR-Tools et rédiger les premiers tests unitaires JUnit dès la finalisation du dataset.

## Résultats et déploiement

**19. Quels résultats chiffrés obtenez-vous sur un jeu de test distinct ?**

Preuve à présenter : Tableau baseline/solution, taille du test, latence et coût par scénario.

Réponse du groupe : *(Non mesuré au Jalon 1 — Démarrage)*. Les premières mesures comparatives sur un jeu de test de $50$ scénarios multi-piliers seront présentées au Jalon 2.

**20. Quels sont vos trois échecs les plus importants et comment les expliquez-vous ?**

Preuve à présenter : Entrées, sorties observées, causes probables et correction envisagée.

Réponse du groupe : *(En phase de démarrage, aucun échec d'exécution en production n'est encore relevé. Trois risques majeurs ont été identifiés dès le cadrage pour être traités préventivement :)*
1. **Hallucination des disponibilités par le LLM :** Risque que le modèle invente un créneau libre. *Correction :* Interdiction de répondre sur une disponibilité sans appel préalable à `SearchCatalogTool` et `ConstraintPlannerTool`.
2. **Conflits de fuseaux horaires / dates :** Complexité de synchronisation entre les événements du Mondial et les événements locaux. *Correction :* Normalisation stricte en UTC dans PostgreSQL et Spring Boot.
3. **Temps de réponse trop long :** Cumul de la latence du LLM et de la résolution OR-Tools. *Correction :* Borne temporelle stricte sur OR-Tools et streaming de la réponse textuelle.

**21. Quelles entrées difficiles ou hostiles avez-vous testées ?**

Preuve à présenter : Données manquantes, outil indisponible, prompt malveillant, réponse non sourcée.

Réponse du groupe : *(Batterie de tests prévue pour le Jalon 2)* :
* Requêtes hors sujet ou tentatives de *prompt injection* visant à contourner les tarifs ou les règles de capacité.
* Demande d'un pack sur un événement complet (capacité maximale atteinte).
* Simulation d'indisponibilité de l'API LLM pour valider le basculement sur la recherche classique.

**22. Un autre groupe peut-il installer et lancer votre application ?**

Preuve à présenter : README vérifié, dépendances fixées et commande de lancement.

Réponse du groupe : L'architecture est conçue dès le départ pour être 100 % reproductible via **Docker** (`docker-compose up --build`) accompagnée d'un fichier `README.md` détaillant les variables d'environnement requises et le script d'initialisation de la base PostgreSQL.

**23. Quelle version est déployée et quel parcours avez-vous validé ?**

Preuve à présenter : Commit ou version, environnement, démonstration et résultat du test de bout en bout.

Réponse du groupe : Version `v0.1-init` (environnement local de développement). Aucun parcours complet n'est encore validé ; le premier parcours cible pour le Jalon 2 est : *Connexion $\rightarrow$ Question à l'agent IA Pilier Mondial 2030 $\rightarrow$ Proposition d'un pack sans conflit via OR-Tools $\rightarrow$ Affichage sur FullCalendar*.

**24. Comment protégez-vous les secrets, les accès et les données ?**

Preuve à présenter : Configuration sans secrets, contrôle d’accès et politique de journalisation.

Réponse du groupe : 
* Gestion des clés API (LLM) et identifiants PostgreSQL exclusivement via variables d'environnement (`.env` exclu du dépôt via `.gitignore`).
* Authentification et contrôle d'accès par rôles (RBAC) via Spring Security (`ADMIN`, `ORGANISATEUR`, `PARTICIPANT`).
* Journalisation sécurisée sans exposition des mots de passe ni des données personnelles sensibles dans les logs.

### Résultats à comparer

| Indicateur | Baseline | Solution | Jeu ou scénario |
| :--- | :--- | :--- | :--- |
| Taux de recommandation sans conflit horaire | À mesurer (Jalon 2) | Objectif : $100\%$ | Jeu de test `test_queries_2030.json` ($50$ cas) |
| Latence et coût par requête | À mesurer (Jalon 2) | Objectif : $< 3\text{ s}$ | Exécution locale Docker + API LLM |

## Contributions et corrections

**25. Quelle contribution chaque membre peut-il présenter et expliquer ?**

Preuve à présenter : Tâches, commits, résultats et revue croisée. Chaque membre explique au moins un choix technique.

Réponse du groupe : 
* **Anas NASSIH :** Architecture Backend **Spring Boot**, modélisation de la base **PostgreSQL**, implémentation du moteur d'optimisation sous contraintes **Google OR-Tools**, intégration du service **LLM / Agents par pilier** et tests unitaires **JUnit**.
* **Houssam NASSIH :** Architecture Frontend **React**, intégration interactive de **FullCalendar**, interfaces conversationnelles des 3 piliers, conteneurisation **Docker** et tests End-to-End **Playwright**.
* *(Revue croisée systématique via Pull Requests sur le dépôt Git du groupe).*

**26. Quel blocage menace le prochain jalon et de quelle aide avez-vous besoin ?**

Preuve à présenter : Cause précise, tentatives déjà faites et décision attendue.

Réponse du groupe : Le principal risque pour le Jalon 2 réside dans le temps de préparation et de nettoyage du dataset multi-piliers ainsi que le dimensionnement matériel pour l'exécution/adaptation du LLM. Nous souhaitons valider avec l'enseignant le volume minimal attendu du dataset pour que l'évaluation soit jugée représentative.

**27. Quelle affirmation ou fonctionnalité devez-vous corriger après cette revue ?**

Preuve à présenter : Écart entre prévu et observé, cause et preuve de correction attendue.

Réponse du groupe : Clarifier dès la conception la frontière entre le rôle du LLM (compréhension du besoin, génération de la réponse, synthèse des recommandations) et celui de Google OR-Tools (calcul exact des disponibilités et résolution des contraintes) afin de garantir zéro hallucination sur les réservations.

**28. Quelles sont vos trois prochaines actions prioritaires ?**

Preuve à présenter : Action observable, responsable nommé, échéance et test d’acceptation.

Réponse du groupe : 
1. Générer et valider le dataset `dataset_mondial2030_v1.json` couvrant les 3 piliers et l'importer dans PostgreSQL (Anas & Houssam).
2. Développer le socle Backend Spring Boot avec le modèle de contraintes Google OR-Tools et l'intégration du premier agent IA conversationnel (Anas).
3. Mettre en place l'interface React avec FullCalendar, le composant de chat par pilier et la configuration Docker Compose complète (Houssam).

**29. Faut-il réduire le périmètre ou revoir le choix des données ou des outils ?**

Preuve à présenter : Décision argumentée et effet sur le planning et les livrables.

Réponse du groupe : Le périmètre des 3 piliers (Tourisme Maroc, Événements Pro, Packs Mondial 2030) est conservé car il constitue la valeur ajoutée du projet, mais nous concentrons les données géographiques sur les **6 villes hôtes marocaines** afin de maintenir un volume de données maîtrisable et de respecter le planning jusqu'au rendu final.

**30. Quelle preuve présenterez-vous lors du prochain point d’avancement ?**

Preuve à présenter : Jalon vérifiable et scénario exact qui permettra de le valider.

Réponse du groupe : Une démonstration en direct lancée via `docker-compose up` montrant : (1) le dataset chargé dans PostgreSQL, (2) un utilisateur interrogeant l'agent IA du pilier *Packs Mondial 2030* pour composer un séjour sans conflit autour d'un match, et (3) l'affichage automatique du planning généré par OR-Tools dans FullCalendar, accompagné des premiers rapports de tests JUnit.

### Plan de correction à valider

| Action prioritaire | Responsable | Échéance | Preuve de correction |
| :--- | :--- | :--- | :--- |
| 1. Finaliser et importer le dataset structuré Mondial 2030 (3 piliers) | Anas & Houssam NASSIH | Jalon 2 - 7j | Fichier `dataset_mondial2030_v1.json` + script d'import SQL validé |
| 2. Implémenter l'API Spring Boot, le solveur OR-Tools et l'agent IA routé | Anas NASSIH | Jalon 2 - 3j | Tests JUnit passants sur les contraintes et les appels d'outils LLM |
| 3. Développer l'interface React/FullCalendar/Chat et conteneuriser via Docker | Houssam NASSIH | Jalon 2 - 1j | Lancement `docker-compose up` fonctionnel + test E2E Playwright |

Décision de la revue : [poursuivre / corriger avant le prochain jalon / réduire le périmètre]

Commentaire de l’enseignant : [à compléter]