---
title: Absturzberichte
description: Erfahre, wie mit Absturzberichten umzugehen ist, und wie man sie liest.
authors:
  - IMB11
---

# Absturzberichte {#crash-reports}

:::tip
Falls du Schwierigkeiten hast, den Grund für einen Absturz herauszufinden, frage im [Fabric Discord](https://discord.gg/v6v4pMv) im Kanal `#player-support` oder `#server-admin-support` nach Hilfe.
:::

Absturzberichte sind ein sehr wichtiger Teil, um Probleme mit deinem Spiel oder Server zu beheben. Sie enthalten viele Informationen über den Absturz und können beim Finden der Ursache für den Absturz hilfreich sein.

## Absturzberichte Absturzberichte finden {#finding-crash-reports}

Absturzberichte werden im `crash-reports`-Verzeichnis in deinem Spiel-Verzeichnis gespeichert. Falls du einen Server nutzt, sind sie im `crash-reports`-Verzeichnis im Server-Verzeichnis.

Für Launcher von Drittanbietern solltest du deren Dokumentation verwenden, um den Ort der Absturzberichte zu finden.

Absturzberichte befinden sich an den folgenden Orten:

::: code-group

```:no-line-numbers [Windows]
%appdata%\.minecraft\crash-reports
```

```:no-line-numbers [macOS]
~/Library/Application Support/minecraft/crash-reports
```

```:no-line-numbers [Linux]
~/.minecraft/crash-reports
```

:::

## Absturzberichte lesen {#reading-crash-reports}

Absturzberichte sind sehr lang und können verwirrend zu lesen sein. Allerdings enthalten sie viele Informationen über den Absturz, die beim Finden der Ursache sehr hilfreich sind.

Für diesen Leitfaden werden wir [diesen Crash-Report](/assets/players/crash-report-example.txt) nutzen.

:::details Absturzbericht anzeigen

<<< @/public/assets/players/crash-report-example.txt{log}

:::

### Abschnitte des Absturzberichts {#crash-report-sections}

Absturzberichte bestehen aus mehreren Abschnitten, jeder ist mit einer Überschrift getrennt:

- `---- Minecraft Crash Report ----`, die Zusammenfassung des Berichts. Dieser Abschnitt enthält den Hauptfehler, der den Absturz verursacht hat, den Zeitpunkt an dem das Problem aufgetreten ist und den relevanten Stacktrace. Dies ist der wichtigste Abschnitt des Absturzberichts, da der Stacktrace normalerweise Referenzen zu der Mod enthält, die den Absturz verursacht hat.
- `-- Last Reload --`, dieser Abschnitt ist nicht wirklich hilfreich, es sei denn, der Absturz ist während dem Neu-Laden von Resourcen (<kbd>F3</kbd>+<kbd>T</kbd>) aufgetreten. Dieser Abschnitt enthält den Zeitpunkt des letzten Neu-Ladens, den relevanten Stacktrace und alle Fehler, die währenddessen aufgetreten sind. Diese Fehler werden normalerweise durch Ressourcenpakete ausgelöst und können ignoriert werden, wenn sie keine Probleme mit dem Spiel verursachen.
- `-- System Details --`, dieser Abschnitt enthält Informationen über dein System, wie das Betriebssystem, die Java-Version, und die Speichermenge, die dem Spiel zugewiesen wurde. Dieser Abschnitt ist nützlich, um festzustellen, ob du die korrekte Java-Version verwendest und ob du dem Spiel genug Speicher zugewiesen hast.
  - In diesem Abschnitt fügt Fabric eine eigene Zeile ein, die mit `Fabric Mods:` beginnt und darauf folgend alle installierten Mods auflistet. Dieser Abschnitt ist nützlich, um festzustellen, ob Konflikte zwischen Mods aufgetreten sein könnten.

### Den Absturzbericht herunterbrechen {#breaking-down-the-crash-report}

Da wir jetzt alle Abschnitte des Absturzberichts kennengelernt haben, können wir nun beginnen, den Absturzbericht herunterzubrechen und die Ursache des Absturzes zu finden.

Mit dem obigen verwiesenen Beispiel können wir den Absturzbericht analysieren und die Ursache für den Absturz herausfinden und welche Mods den Absturz verursachen.

Der Stacktrace im `---- Minecraft Crash Report ----`-Abschnitt ist in diesem Fall der wichtigste, da er den Hauptfehler, der diesen Absturz verursacht, enthält. In diesem Fall ist der Fehler `java.lang.NullPointerException: Cannot invoke "net.minecraft.class_2248.method_9539()" because "net.minecraft.class_2248.field_10540" is null`.

Mit der Anzahl an Mods, die sich in diesem Stacktrace befinden, kann es schwierig sein, den Schuldigen zu finden, aber das Erste, was zu tun ist, ist die Mod zu finden, die den Absturz verursacht.

<!-- TODO: show part of this file -->

<<< @/public/assets/players/crash-report-example.txt{8-9,14-15 log}

In diesem Fall ist die Mod, die den Absturz verursacht `snownee`, da es die erste Mod im Stacktrace ist, die erwähnt wird.

Mit der Anzahl an erwähnten Mods kann es aber auf Kompatibilitätsprobleme zwischen den Mods hindeuten, und die Mod, die den Absturz verursacht hat, muss nicht die Schuldige sein. In diesem Fall ist es das Beste, den Absturz dem Autor zu melden und ihn den Absturz weiter untersuchen zu lassen.

## Abstürze durch Mixins {#mixin-crashes}

:::info
Mixins sind eine Möglichkeit, das Spiel zu verändern, ohne den Quellcode des Spiels zu verändern. Sie werden von vielen Mods genutzt und sind ein mächtiges Werkzeug für Mod-Entwickler.
:::

Wenn ein Mixin abstürzt, wird normalerweise das Mixin im Stacktrace erwähnt, mit der Klasse, die das Mixin verändert.

Mixins für Methoden enthalten `modid$handlerName` im Stacktrace, wobei `modid` die ID der Mod ist und `handlerName` der Name des Mixin-Handlers ist.

```:no-line-numbers
... net.minecraft.class_2248.method_3821$$$modid$handlerName() ... // [!code focus]
```

Du kannst diese Information nutzen, um die Mod zu finden, die den Absturz verursacht und den Absturz an den Mod-Autor zu melden.

## Was macht man mit Absturzberichten {#what-to-do-with-crash-reports}

Das Beste, was man mit Absturzberichten machen kann, ist sie auf eine Paste-Seite hochzuladen und einen Link mit dem Mod-Autor entweder über den Issue-Tracker oder durch eine andere Art der Kommunikation (Discord etc.) zu teilen.

Das ermöglicht es dem Mod-Autor den Absturz zu untersuchen, potenziell zu reproduzieren und das Problem zu lösen, das den Absturz verursacht hat.

Bekannte Paste-Seiten, die oft für Absturzberichte genutzt werden, sind:

- [GitHub Gist](https://gist.github.com/)
- [MCLogs](https://mclo.gs/)
- [Pastebin](https://pastebin.com/)
