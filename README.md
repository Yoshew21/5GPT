# 5GPT : AI-Powered Dev Toolkit - Rendu de Projet

**Étudiant :** Yoshew GRIBALDO  
**Cours :** 5GPT (M2)  
**Date :** Janvier 2026

---

## 📄 Vue d'ensemble

Ce dépôt contient le rendu final du projet **5GPT**, qui consiste en la conception, l'itération et la validation d'une suite d'outils assistés par IA pour le cycle de vie de développement logiciel (SDLC).

L'objectif principal était de démontrer une maîtrise du **Prompt Engineering** avancé en passant de simples instructions (Zero-Shot) à des architectures robustes et sécurisées (Chain-of-Thought, Few-Shot, Grounding).

## 🛠️ Contenu du Rendu

Le projet s'articule autour de 5 outils distincts, chacun adressant une problématique spécifique du développement :

| Outil | Catégorie | Objectif | Technique Clé |
| :--- | :--- | :--- | :--- |
| **1. AI Pull Request Reviewer** | Code | Revue de code automatisée et sécurisée | Persona Expert + CoT Guidé |
| **2. Analyseur Intelligent de Logs** | Debug | Diagnostic de cause racine (RCA) | Grounding (Preuve par le log) |
| **3. Générateur de Dockerfile** | DevOps | Conteneurisation optimisée et sécurisée | Implicit Few-Shot + Checklist |
| **4. Générateur de README** | Docs | Standardisation de la documentation | Template Injection |
| **5. Commit Message Generator** | Productivité | Messages de commit sémantiques | Few-Shot Learning |

## 📂 Structure des Fichiers

Les fichiers suivants ont été extraits du rapport principal pour faciliter la correction :

- **`prompts.md`** : Un recueil exhaustif de tous les prompts (V1, V2, VFinal) pour chaque outil.
- **`docs.md`** : La documentation technique approfondie expliquant les choix de conception et les stratégies de prompting.
- **`analyse.md`** : Les analyses de sécurité détaillées (Injection de Prompt & Log Poisoning) et les mesures de protection implémentées.

## 🔍 Points Clés pour la Correction

Ce travail met l'accent sur :

1.  **L'itération Scientifique :** Chaque outil a suivi un cycle d'amélioration (V1 -> V2 -> VFinal) justifié par des analyses critiques.
2.  **La Sécurité (Security by Design) :** Une attention particulière a été portée aux risques d'injection de prompt et d'empoisonnement de données, avec des contre-mesures concrètes (délimiteurs XML, règles de non-obéissance).
3.  **L'Utilisabilité :** Les prompts finaux sont conçus pour être intégrés dans des pipelines CI/CD réels, avec des formats de sortie structurés (Markdown, JSON-like).

---
*Ce projet a été réalisé dans le cadre du module 5GPT.*
