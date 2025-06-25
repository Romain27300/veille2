name: Veille Emploi UT2J

on:
  schedule:
    - cron: '0 8 * * *'  # Tous les jours à 8h UTC
  workflow_dispatch:

jobs:
  veille:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Configurer Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Installer les dépendances
        run: pip install requests beautifulsoup4 pandas openpyxl

      - name: Exécuter le script
        run: python veille_ut2j.py

      - name: Ajouter le fichier Excel au dépôt
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git add veille_emploi_ut2j.xlsx
          git commit -m "Mise à jour automatique du fichier Excel"
          git push
