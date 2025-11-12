# Guide des Agents - AstroJS Boilerplate

## 🎯 Objectif de ce guide

Ce document guide les agents IA (comme Cursor Agent, Claude, etc.) sur la manière d'interagir avec ce projet AstroJS et d'appliquer correctement les règles définies.

## 🧭 Navigation dans les règles

### Détection du contexte

Analysez les fichiers demandés ou modifiés pour déterminer quelle règle appliquer :

| Pattern de fichier          | Règle à appliquer | Fichier AGENTS.md                 |
| --------------------------- | ----------------- | --------------------------------- |
| `src/components/**/*.astro` | UI                | `.cursor/rules/ui/AGENTS.md`      |
| `src/lib/services/**/*.ts`  | Logic             | `.cursor/rules/logic/AGENTS.md`   |
| `src/lib/utils/**/*.ts`     | Logic             | `.cursor/rules/logic/AGENTS.md`   |
| `src/lib/adapters/**/*.ts`  | Data              | `.cursor/rules/data/AGENTS.md`    |
| `src/content/config.ts`     | Data              | `.cursor/rules/data/AGENTS.md`    |
| `src/tests/**/*.test.ts`    | Testing           | `.cursor/rules/testing/AGENTS.md` |

### Règles multiples

**Si plusieurs contextes sont impliqués**, appliquez les règles dans cet ordre :

1. Data (schéma, validation)
2. Logic (traitement)
3. UI (affichage)
4. Testing (validation du tout)

**Exemple :** Créer une page de blog avec API

1. Data : Adapter API + schéma Zod
2. Logic : Service de récupération des articles
3. UI : Composants Astro pour l'affichage
4. Testing : Tests de chaque couche

## 📋 Checklist générale avant toute génération

Avant de générer ou modifier du code, vérifiez :

- [ ] Le fichier suit la convention de nommage (kebab-case)
- [ ] TypeScript est utilisé avec typage strict
- [ ] Les imports sont organisés correctement
- [ ] Les variables d'environnement sensibles ne sont pas hardcodées
- [ ] Le code suit les standards ES2024+
- [ ] La documentation JSDoc est présente pour les exports publics
- [ ] Les erreurs sont correctement gérées et typées
- [ ] L'accessibilité est prise en compte (pour UI)
- [ ] Les performances sont optimisées (lazy loading, images...)

## 🎨 Méthodologie de travail

### 1. Analyse de la demande

**Questions à se poser :**

- Quel est l'objectif principal de la demande ?
- Quels fichiers seront créés/modifiés ?
- Quelles règles spécifiques s'appliquent ?
- Y a-t-il des dépendances entre composants ?

### 2. Planification

**Avant de coder :**

1. Identifier la structure Atomic Design (pour UI)
2. Déterminer les types TypeScript nécessaires
3. Vérifier les schémas Zod à créer/utiliser
4. Lister les tests à écrire

### 3. Génération progressive

**Ne pas tout générer d'un coup :**

1. Commencer par les types et interfaces
2. Créer les schémas de validation
3. Implémenter la logique métier
4. Construire les composants UI
5. Ajouter les tests

### 4. Validation

**Après génération :**

- Vérifier la cohérence avec les règles
- S'assurer que le code compile (TypeScript)
- Confirmer que les imports sont résolubles
- Valider que les patterns sont suivis

## 🔍 Patterns de détection

### Demandes de création de composant

**Indices :** "créer un composant", "nouveau composant", "ajouter un bouton"
**Action :**

1. Consulter `.cursor/rules/ui/AGENTS.md`
2. Déterminer le niveau Atomic Design
3. Vérifier les exemples dans `.cursor/rules/ui/examples/`

### Demandes de logique métier

**Indices :** "service", "utilitaire", "fonction", "traitement", "logique"
**Action :**

1. Consulter `.cursor/rules/logic/AGENTS.md`
2. Déterminer si c'est un service ou un utilitaire
3. Vérifier les patterns de validation Zod

### Demandes liées aux données

**Indices :** "API", "fetch", "collection", "données", "schéma", "adapter"
**Action :**

1. Consulter `.cursor/rules/data/AGENTS.md`
2. Identifier la source de données (API, Collection, etc.)
3. Créer les schémas Zod appropriés

### Demandes de tests

**Indices :** "test", "tester", "coverage", "mock"
**Action :**

1. Consulter `.cursor/rules/testing/AGENTS.md`
2. Déterminer le type de test (unitaire, intégration, e2e)
3. Suivre les patterns de mock/stub

## 💡 Principes de génération de code

### Clarté > Concision

```typescript
// ✅ Préférer ceci (clair)
const user = await fetchUser(userId);
if (!user) {
  throw new UserNotFoundError(userId);
}
return user;

// ❌ Éviter ceci (trop concis)
return await fetchUser(userId) ?? throw new UserNotFoundError(userId);
```

### Types explicites

```typescript
// ✅ Types explicites
function calculateTotal(items: CartItem[]): number {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// ❌ Types implicites
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

### Décomposition fonctionnelle

**Si une fonction dépasse 50 lignes :**

1. La décomposer en sous-fonctions
2. Extraire la logique réutilisable
3. Documenter chaque fonction

### Éviter la duplication

**Avant de créer une nouvelle fonction/composant :**

1. Vérifier si un équivalent existe
2. Évaluer si l'existant peut être étendu
3. Ne créer que si vraiment nécessaire

## 🚨 Erreurs courantes à éviter

### ❌ Utiliser `any` en TypeScript

**Solution :** Utiliser `unknown` ou typer correctement

### ❌ Ignorer la validation Zod

**Solution :** Toujours valider les données externes (API, formulaires)

### ❌ Mélanger logique et présentation

**Solution :** Séparer en services/utils et composants

### ❌ Hardcoder des URLs/clés API

**Solution :** Variables d'environnement

### ❌ Oublier l'accessibilité

**Solution :** Attributs ARIA, sémantique HTML, contraste

### ❌ Ne pas gérer les erreurs

**Solution :** try/catch, classes d'erreur personnalisées

### ❌ Tests insuffisants

**Solution :** Couvrir les cas nominaux ET les cas d'erreur

## 📚 Exemples de demandes et réponses

### Exemple 1 : "Crée un composant Card pour afficher un article"

**Analyse :**

- Type : Composant UI
- Niveau : Molecule (groupe d'atoms)
- Règle : `.cursor/rules/ui/`

**Plan :**

1. Créer l'interface `CardProps` en TypeScript
2. Créer `card.astro` dans `src/components/molecules/`
3. Ajouter les styles CSS standards
4. Documenter les props
5. Créer un test `card.test.ts`

### Exemple 2 : "Ajoute une fonction pour formater les dates"

**Analyse :**

- Type : Utilitaire
- Règle : `.cursor/rules/logic/`

**Plan :**

1. Créer `date-utils.ts` dans `src/lib/utils/`
2. Typer les paramètres et retour
3. Ajouter JSDoc
4. Gérer les cas d'erreur
5. Créer des tests unitaires

### Exemple 3 : "Configure l'API pour récupérer les produits"

**Analyse :**

- Type : Données / API
- Règle : `.cursor/rules/data/`

**Plan :**

1. Créer le schéma Zod `ProductSchema` dans `src/types/`
2. Créer l'adapter `products-adapter.ts` dans `src/lib/adapters/`
3. Configurer les variables d'environnement
4. Gérer les erreurs API
5. Ajouter les tests d'intégration

## 🔄 Workflow d'amélioration continue

### Quand suggérer des améliorations

**Soyez proactif si vous détectez :**

- Code dupliqué (DRY)
- Fonctions trop longues (>50 lignes)
- Absence de gestion d'erreur
- Types `any` utilisés
- Composants non accessibles
- Performance sous-optimale
- Absence de tests

**Comment suggérer :**

1. Expliquer le problème
2. Proposer une solution conforme aux règles
3. Montrer un exemple de code amélioré
4. Quantifier le bénéfice si possible

## 🎓 Auto-apprentissage

### Consulter les exemples

Chaque règle spécialisée contient un dossier `examples/` :

- Utilisez-les comme référence
- Adaptez-les au contexte
- Maintenez leur qualité

### Mettre à jour les règles

Si un pattern se répète souvent :

1. Documenter le pattern
2. Ajouter un exemple
3. Mettre à jour AGENTS.md
4. Incrémenter la version

## 📞 Escalade et limites

### Quand demander une clarification

- Ambiguïté dans les requirements
- Conflit entre règles
- Choix architecturaux majeurs
- Modifications de schéma de données
- Changements de structure de projet

### Format de demande de clarification

```
🤔 Clarification nécessaire

**Contexte :** [Décrire la situation]
**Question :** [Question précise]
**Options envisagées :**
1. Option A : [description + pros/cons]
2. Option B : [description + pros/cons]

**Recommandation :** [Votre avis avec justification]
```

## ✅ Checklist de sortie

Avant de finaliser une génération de code :

- [ ] Code compilé sans erreur TypeScript
- [ ] Linter passé sans erreur
- [ ] Imports organisés correctement
- [ ] JSDoc présente pour exports publics
- [ ] Gestion d'erreur implémentée
- [ ] Tests créés et passants
- [ ] Accessibilité vérifiée (si UI)
- [ ] Performance optimisée
- [ ] Pas de secrets hardcodés
- [ ] Convention de nommage respectée
- [ ] Règles spécifiques appliquées

---

**Version :** 1.0.0  
**Compatibilité :** Cursor Agent, Claude, GitHub Copilot  
**Dernière mise à jour :** 2025-10-28
