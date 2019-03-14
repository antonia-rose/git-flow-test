# Parallele Major Releases mit git flow

See: https://gitversion.readthedocs.io/en/latest/git-branching-strategies/gitflow-examples/#support-branches

## Testszenario:
Wir haben zwei Branches `develop` und `master` in der Version `1.0.0`. Die Version liegt ebenfalls als Tag vor.

Durch permanente Weiterentwicklung wurde Version `2.0.0` als Tag erstellt und in `master` und `develop` gemerged.

Um weiterhin Version `1.x` zu supporten wurde ein Support-Branch `1.x` erstellt.

## Umgang mit Bugs
Bei einem Bug muss geprüft werden, ob dieser alle Versionen betrifft. Wenn es nur Version `2.x` betrifft können die Änderungen aus `develop` heraus erstellt werden.
Betrifft der Bug auch Version `1.x`, so muss die Änderung aus `1.x` heraus erstellt werden. Der Zielbranch ist dabei `1.x` aus dem heraus ein neues Tag `1.0.1` erstellt wird. Nach dem das Release veröffentlicht ist, wird der Branch `1.x` in `develop` gemerged und die Änderungen stehen für die aktuelle Major Version zur Verfügung. 

## Umgang mit neuen Features
Es muss geklärt sein, ob das Feature in alle Versionen implementiert werden muss. Ist dem so, muss das Feature aus `1.x` erstellt werden. Der Zielbranch ist dabei `1.x` aus dem heraus ein neues Tag `1.0.1` erstellt wird. Nach dem das Release veröffentlicht ist, wird der Branch `1.x` in `develop` gemerged und die Änderungen stehen für die aktuelle Major Version zur Verfügung. 

Betrifft das Feature nur die aktuelle Version, kann aus `develop` abgezweigt werden und wie gewohnt released werden.

Der `develop` wird nicht in den `1.x` Branch gemerged!

## Vorgehen mit Git Flow
See: https://danielkummer.github.io/git-flow-cheatsheet/

`develop` und `master` sind immer auf dem aktuellen Entwicklungsstand. Für Releases, die weiter unterstützt werden müssen, kann man mit `git flow support start 1.x 1.0.0` einen Support-Branch auf Basis des Tags `1.0.0` erstellen.

Entwicklungen, die auch Version `1.x` betreffen müssen aus dem Support-Branch erstellt werden. Git Flow unterstützt nur für Hotfixes die Angabe von einer Branch-Basis. Daher müssen alle Entwicklungen, die auch für Version `1.x` gelten als Hotfix erstellt werden: `git flow hotfix start 1.0.1 support/1.x`. 

Bei Merge Requests muss immer drauf geachtet werden, welcher Zielbranch definiert ist. Für Änderungen, die aus `support/1.x` erstellt wurden, muss dies dementsprechend der Zielbranch werden.

Hotfixes sind nicht dafür ausgelegt mittels Merge Requests wieder in die Zielbranches gesetzt zu werden. In Git Flow wird beim Beenden eines Hotfixes Folgendes gemacht:

```
Summary of actions:
- Hotfix branch 'hotfix/1.0.1' has been merged into 'support/1.x'
- The hotfix was tagged '1.0.1'
- Hotfix branch 'hotfix/1.0.1' has been locally deleted
- 'support/1.x' and tags have been pushed to 'origin'
- You are now on branch 'support/1.x'
```

Mit einem Merge Request muss das Erstellen des Tags manuell erfolgen.

Nachdem der Hotfix zurück in `support/1.x` gemerged und das Release erstellt wurde, kann man `support/1.x` in `develop` mergen und hat die Änderungen auch in Version `2.x` aus der heraus das entsprechende Release erstellt werden muss.