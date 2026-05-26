# Automatisation n8n pour l'inclusion de documents de facture

## Vue d'ensemble

Ce dépôt documente un workflow n8n exporté sous `workflow/workflow.sanitized.json`. L'automatisation traite les pièces jointes Gmail, analyse les factures avec Google Gemini, téléverse les fichiers dans Google Drive et ajoute les données structurées dans Google Sheets.

Le fichier du workflow n'a pas été modifié lors de cette organisation.

## Objectif du workflow

Automatiser l'entrée de factures reçues par e-mail : détecter les messages Gmail avec pièces jointes, extraire les données de facture, stocker le fichier original dans Google Drive et écrire les données dans Google Sheets.

## Problème résolu

Le workflow réduit les tâches manuelles : ouvrir les e-mails, télécharger les fichiers, lire les factures, renommer les documents, téléverser les fichiers et remplir une feuille de calcul.

## Architecture

Séquence principale :

1. `Gmail Trigger` surveille Gmail et télécharge les pièces jointes.
2. `If` vérifie l'existence d'une pièce jointe.
3. `Split Out` sépare les binaires.
4. `Loop Over Items` traite chaque pièce jointe.
5. `Analyze document` utilise Google Gemini pour extraire un JSON.
6. `Edit Fields` normalise l'objet retourné.
7. `If1` vérifie si le document est corporatif.
8. `Merge` et `Merge1` combinent les données structurées et binaires.
9. `Upload file` et `Upload file1` téléversent le fichier vers Google Drive.
10. `Append row in sheet` écrit les données dans Google Sheets.

Voir [assets/diagrams/workflow-architecture.mmd](../../assets/diagrams/workflow-architecture.mmd).

## Noeuds n8n utilisés

- `n8n-nodes-base.gmailTrigger`
- `n8n-nodes-base.if`
- `n8n-nodes-base.splitOut`
- `n8n-nodes-base.splitInBatches`
- `@n8n/n8n-nodes-langchain.googleGemini`
- `n8n-nodes-base.noOp`
- `n8n-nodes-base.set`
- `n8n-nodes-base.merge`
- `n8n-nodes-base.googleDrive`
- `n8n-nodes-base.googleSheets`

## Intégrations nécessaires

- Gmail OAuth2
- Google Gemini ou Google PaLM API
- Google Drive OAuth2
- Google Sheets OAuth2

## Configuration attendue

Utilisez `templates/env.example` comme référence de placeholders. Les valeurs réelles doivent être configurées uniquement dans n8n ou dans un gestionnaire de secrets.

La configuration attendue inclut les identifiants Gmail, Gemini/PaLM, Drive et Sheets, ainsi que la feuille cible et les dossiers Drive.

## Importation dans n8n

1. Ouvrez n8n.
2. Importez `workflow/workflow.sanitized.json`.
3. Vérifiez tous les noeuds avant activation.
4. Reconnectez les identifiants locaux.
5. Remplacez les références de feuille, d'onglet et de dossiers par celles de votre environnement.
6. Testez avec des pièces jointes fictives.
7. Activez uniquement après validation des sorties et des logs.

## Identifiants

Créez les identifiants directement dans n8n. N'ajoutez jamais de secrets dans le JSON sanitizé. Testez chaque identifiant avant d'activer le workflow.

## Exemples

- [Exemple d'entrée](../../examples/sample-input.md)
- [Exemple de sortie](../../examples/sample-output.md)

## Limites

- La qualité de sortie dépend du document et du modèle.
- Le prompt attend un JSON pur ; une réponse invalide peut interrompre les étapes suivantes.
- Les références spécifiques à l'environnement doivent être revues après importation.
- Consultez [SECURITY_NOTES.md](../../SECURITY_NOTES.md) pour les risques relevés.

## Prochaines étapes

- Remplacer les références spécifiques par des placeholders sûrs.
- Ajouter des captures d'écran actualisées.
- Tester des factures fictives corporatives et personnelles.
- Améliorer la gestion des erreurs pour IA, Drive et Sheets.
