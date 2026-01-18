# LeadFlow — Guide de déploiement Vercel (et workflow local)

## 1) Le vrai problème (avant même “quoi choisir”)

Le message rouge est **bloquant** :

> Your account has been suspended. To reactivate your subscription, add a valid payment method

👉 **Tu ne peux PAS déployer tant que ce point n’est pas réglé.**  
Aucun choix de framework ne changera ça.

**Action immédiate :**

- Soit tu ajoutes un moyen de paiement.
- Soit tu switches temporairement sur :
  - un autre compte Vercel, ou
  - un autre host (Railway / Render / Fly.io).

Une fois réglé → on parle config.

## 2) Ce que tu dois choisir dans Vercel (quand le compte est OK)

Je pars du principe que **LeadFlow = Next.js (App Router)**  
Logique : forms, auth légère, server actions, intégration CRM.

**✅ Framework Preset**

👉 Choisis : **Next.js**  
❌ **PAS** `Other`  
Sinon : build moins “smart”, détection moins fiable, friction inutile.

**✅ Root Directory**

👉 `./`  
(sauf si ton repo a un `/apps/web` ou équivalent)

**✅ Build and Output Settings**

👉 ne touche à rien (par défaut, Vercel gère `next build` nickel)

## 3) Variables d’environnement (À NE PAS OUBLIER)

Tu peux déployer sans, mais la plateforme sera bancale.

Minimum à prévoir (même placeholders au départ) :

- `DATABASE_URL`
- `NEXT_PUBLIC_BASE_URL`
- `CRM_API_KEY` (HubSpot ou autre)
- `PDF_STORAGE_BUCKET` (plus tard)
- `ACCESS_CODE_SECRET`
- `NEXTAUTH_SECRET` (si auth)

👉 Tu peux les ajouter après le premier deploy, mais prépare la liste dès maintenant.

## 4) Nom du projet

`lead-flow` → OK (propre, scalable, compatible futur “Monitor/SaaS”).

## 5) Workflow cible (Codex → GitHub → Vercel) pour itérer vite

Objectif : chaque amélioration = visible immédiatement, sans attendre le deploy.

**Boucle de travail standard :**

1. Codex modifie le code.
2. Codex te donne une preview locale immédiate.
3. Tu valides visuellement.
4. Push GitHub.
5. Vercel auto-deploy (preview + prod).

👉 Important : Vercel devient l’étape “partage / staging”, pas l’étape “découverte”.

## 6) Règle stricte à imposer à Codex (dans chaque message)

À chaque réponse Codex, il doit inclure un mini-runbook local pour que tu voies le site tout de suite.

**Exigence minimale à coller dans ton prompt Codex :**

Toujours fournir :

- commandes pour installer
- commandes pour lancer
- URL locale
- check rapide “ça tourne”

**Exemple attendu dans chaque message Codex :**

```bash
npm install
npm run dev
```

Ouvrir http://localhost:3000

Si erreur : commande + piste corrective.

**Option bonus (si tu veux être carré) :**

- `npm run lint`
- `npm run build` avant push
