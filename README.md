# Github
## Repository erstellen
![](images/create_repo.png)
```bash
echo "# test-2" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:thomaselmer/test-2.git
git push -u origin main
```

## Hinzufügen von Dateien zum Repository
Befehl: 
- `git add <dateiname>` um eine Datei hinzuzufügen
- `git add *` um alle Dateien in einem Repository hinzuzufügen

## Gitignore
erstellen einer Datei mit dem Namen: `.gitignore` damit können Dateien oder Ordner von der Versions-verwaltung ausgenommen werden. Wichtig für Environment-Variablen (`.env`), Datenbanken oder Media-Ordner auszunehmen.

dabei einfach den gewünschten Dateinamen oder Ordner angeben
```py
.env
.db
*.png
folder/*
!datei       # um eine Ausnahme zu machen
```

somit würde eine solche `.gitignore` alle Dateien ignorieren ausser die `README.MD` und alle Dateien in `src`
```
*
!README.md
!src
```


## Commits
ist sozusagen das Speichern eines aktuellen Standes, dabei soll eine kurze Beschreibung der Änderung angeführt werden

```bash
git commit -m “<message”>
```
![](images/commit-vscode.png)
## Push
ist das hochladen auf die Platform (Github, Gitlab, …)

```bash
git push
```

## Tag
damit wird eine bestimmter commit als Version gespeichert und muss mit einem `git push —tags` hochgeladen werden. Benennung idealerweise `v0.0.0` mit fortlaufender Nummer, ist aber kein muss. Somit ist eine Versionierung gegeben und es sofort ersichtlich welche die letzte stabile Version ist.

```bash
git tag <tag-name> 
git push --tags
```

## Branch
ein Branch wird mit dem Befehl `git checkout -b <branch-name>` erstellt.
das `-b`Flag muss nur dann gesetzt werden wenn ein **neuer Branch** erstellt werden soll. Mit `git checkout main` kann man auf einen bestehenden Branch wechseln
![](images/checkout-vscode.png)

### Empfehlungen für Branches
Name | Beschreibung
--- | ---
main | Haupbranch in dem nur getestete Versionen existieren und gegebenenfalls direkt mit der **Live-Umgebung** syncronisiert werden
development | Zweiter Hauptbranch für die **Test-Umgebung** hier sollen Versionen getestet werden befor sie in den **main** Branch integriert werden
*entweder*
manuel | Branch von Manuel zur Entwicklung
rupert | Branch von Rupert zur Entwicklung
*oder*
feature_xy | Branch zur Entwicklung de Features "XY"
bugfix_ab | Branch zur Behebung des Problemes "AB"

## Merge
1) in den Branch wechseln der weiter exestieren soll
2) `git merge <zweiter Branch>`
3) für gewönlich wird der zweite Branch in den ersten integriert
4) wenn nicht entsteht ein Merge Konflikt, welcher gelöst werden muss
  4.1) Nach dem lösen des Merge Konfliktes mit einem erneuten `git commit`speichern

![](images/merge.png)


## Workflows
Mit Workflows können Automatisierungen erstellt werden die beispielsweise bei einem **Push** auf den *Main Branch* oder mit dem Taggen einer neuen Version ausgelöst werden. 

Es können auch Workflows angelegt werden die automatische Tests durchführen.


```yml
name: Deploy on version tag

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ${{ secrets.SSH_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/myapp
            git fetch --tags
            git checkout ${{ github.ref_name }}
            docker compose up -d --build
```


## Sonstige hilfreiche Webseiten
In diesen [Link](https://learngitbranching.js.org) ist ein Tutorial für forgeschrittene Methoden um das Repository sauber zu halten

