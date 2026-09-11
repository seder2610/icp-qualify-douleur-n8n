# ICP Qualify — Douleur ops agence (v1)

Workflow n8n **ultra-léger** pour qualifier des prospects par **type de douleur**, pas par secteur.

## Principe

| Douleur | Poids | Signaux détectés |
|---------|-------|------------------|
| **Reporting client** | 45 | rapport hebdo/mensuel, dashboard client, Looker, copier-coller data… |
| **Onboarding client** | 35 | nouveau client, kick-off, checklist, brief, setup… |
| **Intégrations outils** | 20 | CRM, Notion, Slack, Sheets, Zapier/Make, process manuel… |

**Bonus structure** (pas secteur) : taille 5–60, ≥8 clients, décideur, rapports récurrents.

## Usage

1. Importer `icp-qualify-douleur-v1.json` dans n8n
2. **Manual trigger** → `demo_mode: true` lance 5 prospects fictifs (SEO, social, full-service, SEA, solopreneur exclu)
3. Sortie : `icp_score`, tier A/B/C, `icp_primary_pain`, `icp_angle`, `icp_message_hook`, CTA **DEMO**

## Config (node ⚙️ Configuration ICP)

| Param | Défaut | Description |
|-------|--------|-------------|
| `min_score` | 55 | Seuil qualification |
| `weight_reporting` | 45 | Poids douleur #1 |
| `weight_onboarding` | 35 | Poids douleur #2 |
| `weight_integrations` | 20 | Poids extension |
| `demo_mode` | true | false = passe-through depuis node précédent |
| `cta_keyword` | DEMO | Mot-clé DM LinkedIn |

## Brancher sur V7CE Prospect

En prod : remplacer le node demo par import Notion/Apify, puis POST vers `POST /api/leads/import` avec :
- `niche`: `ops-agence`
- `icp_score`: score calculé
- `angle`: `icp_angle`

Voir `v7ce-prospect/data/icp.json` pour la config ICP active.

## Tiers

- **A** : score ≥ 75
- **B** : score ≥ min_score
- **C** : sous seuil (filtré)
