# Publier le nouveau prototype live sur Netlify

Cette version conserve les profils intéressés en direct avec Netlify Blobs.
Elle commence par l'histoire de Never Go Alone, puis propose un onboarding
local (prénom, nom, âge, sexe et centres d'intérêt). Les centres d'intérêt
priorisent le catalogue sans masquer les autres sorties ; le thème sombre est
actif par défaut.

## Publication

1. Créer un **nouveau projet Netlify** depuis ce dossier Git. Ne pas publier
   seulement `index.html`, car les Functions ne seraient pas publiées.
2. Laisser le **Base directory** vide si le contenu de ce dossier est à la
   racine du dépôt. Netlify utilisera `netlify.toml`, la commande
   `npm run build` et le dossier publié `dist`.
3. Déclencher le déploiement. La commande est `npm run build` et le dossier
   publié est `dist`.
4. Dans **Site configuration → Environment variables**, ajouter de préférence
   `INTEREST_HASH_SECRET` avec une longue valeur aléatoire privée. Ne pas
   modifier cette valeur après les premières inscriptions, car elle protège
   les liens de retrait des profils.

Le dossier `dist` est généré automatiquement par la commande de build et est
volontairement ignoré par Git : il n'est pas nécessaire de l'ajouter au dépôt.

Les routes `/api/*` sont traitées par `netlify/functions/live-interest.mjs`.
Elles enregistrent seulement le prénom, l'âge, la nationalité, l'activité,
la courte description, les passions et un avatar illustré qui peuvent être
affichés publiquement.
Le genre choisi peut aussi être affiché publiquement après consentement.
L'e-mail et le nom ne sont pas conservés dans le stockage partagé.

Les informations saisies avant la participation restent uniquement en mémoire
pendant la visite : elles ne sont pas incluses dans l'URL ni enregistrées dans
le navigateur. Les anciennes données locales des premières versions sont
supprimées au chargement. Une vérification par pièce d'identité est prévue pour
la version finale, mais ce prototype ne collecte ni ne conserve aucun document
d'identité.

Le formulaire de participation est intégré à l'application : il comprend le
profil public et les trois questions du questionnaire de lancement. Un seul
bouton **Envoyer** transmet les réponses au Google Form puis ajoute le profil
au groupe. Il n'y a pas d'iframe Google, de case « j'ai envoyé » ni de second
bouton pour rejoindre le groupe.

La Function envoie les réponses au Google Form avant d'enregistrer le profil
public. Si le questionnaire n'est pas disponible, le profil n'est donc pas
ajouté par erreur. L'URL de réponse Google par défaut est incluse dans la
Function ; tu peux la remplacer dans Netlify avec la variable d'environnement
`GOOGLE_FORM_RESPONSE_URL` si tu changes de formulaire.

Le Google Form doit rester public et conserver ses quatre questions actuelles
(e-mail, niveau d'intérêt, retour libre et disponibilité pour un échange).
Les réponses sont envoyées depuis la Function afin que le bouton **Envoyer**
de l'application reste l'unique action du visiteur.

Pour protéger l'accès au prototype, activer la protection du site dans les
réglages Netlify plutôt que de mettre un mot de passe dans le code source.
