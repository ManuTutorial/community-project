# Coding Conventions

Wir nutzen als Basis die [.NET Standards](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-type-members) und folgen diesen weiteren Anpassungen.

> Angepasst und übersetzt vom [Unity Open Project](https://docs.google.com/document/d/1-eUWZ0lWREFu5iH-ggofwnixDDQqalOoT4Yc0NpWR3k/edit).

## Code

### Bezeichner

* Beschreibbare und präzise Namen, auch wenn diese länger werden. Lesbarkeit ist wichtiger als kurze Bezeichner.
* Verwende _keine_ Abkürzungen.
* Verwende anerkannte Akryonme, z.B. UI oder IO.
* Präfixe boolsche Variablen mit "Is", "Has", "Can", etc. z.B. `CanJump`, `IsActive`.
* Vermeide das Nummerieren von Namen, z.B. `Animator1`, `Animator2`, etc. Verwende sinnvolle Bezeichner, um den Unterschied erkenntlich zu machen, z.B. `PlayerAnimator`, `EnemyAnimator`.

### Groß-/Kleinschreibung

> **camelCase**: Erster Buchstabe ist kleingeschrieben, der jeweils erste Buchstabe der Folgewörter ist großgeschrieben.
> **PascalCase**: Der erste Buchstabe eines jeden Wortes ist großgeschrieben.

* Klassen, Methoden, Enums, Namespaces, öffentliche Felder und Eigenschaften: PascalCase.
* Lokale Variablen, Methodenparameter: camelCase.
* Private Felder: camelCase und Unterstrich-Präfix, z.B. `_gameControls`.

### Programmierung

* Halte den Code in englischer Sprache (dict.cc hilft beim Übersetzen), Ausnahme [siehe Kommentare](#kommentare)
* Felder und Methoden bleiben private, außer man benötigt öffentlichen Zugriff.
* Wenn Du Felder im Inspektor sehen willst, aber den öffentlichen Zugriff nicht benötigt, dann verwende `[SerializeField]` zusammen mit `private`.
  Falls Du dann die Warnung erhälst _"Field is never assigned to, will always have its default value"_, dann mache eine `= default` Zuweisung
* Versuche Singletons zu vermeiden, in dem du z.B. ein ScriptableObject ([1](https://www.youtube.com/watch?v=TjTL-MXPnbo), [2](https://www.youtube.com/watch?v=qqzZZfgtQyU), [3](https://www.youtube.com/watch?v=QkVpYHc1s60)) implementierst.
* Vermeide statische Variablen.
* Vermeide Magic Numbers ("magische Nummer"), z.B. `value * 0.08`, warum wird hier der Wert mit 0,08 multipliziert? Nutze stattdessen eine Konstante oder ein Feld, um der Zahl einen Namen zu geben.
* Nutze Namespaces, wie es in C# üblich ist, jeder Ordner ist automatisch ein Namespace. Das Basis-Namespace ist `BoundfoxStudios.CommunityProject`.

### Formatierung

* Verwende **1 Tab** pro Spalte, keine Leerzeichen.
  Das gibt einfach jedem die Möglichkeit, den Code visuell so darzustellen, wie man sich wohlfühlt.

### Kommentare

* Auch wenn der Code in Englisch gehalten wird, schreibe Deine Kommentare auf Deutsch.
* Versuche Kommentare zu vermeiden, der Code sollte für sich sprechen. 
* Füge Kommentare dort hinzu, wo es wirklich sinnvoll ist, bspw. wenn eine gewisse Ablaufreihenfolge besteht, die eingehalten werden muss.
* Nutze VSDoc für Beschreibungen von Klassen, Methoden, etc.
* Beschreibe jede öffentliche Klasse, Methode und Eigenschaft welchen Zweck sie erfüllt, z.B.
  ```csharp
  /// <summary>
  /// Diese Klasse kümmert sich um das Abspielen von Kamerafahrten.
  /// </summary>
  ```
* Verwende keine `#region`-Direktiven oder Kommentare, die eine visuelle Trennung erzeugen, wie z.B. `//-------`.
  Falls Du sowas brauchst, ist das oft ein Hinweis, dass die Klasse zu viele Zuständigkeiten hat.

## Scene & Hierarchy

### Organisation

* Nutze leere GameObjects auf der obersten Ebene, um die Hierarchy visuell in logische Bereiche zu trennen, z.B. `----Environment----`, `----Managers----`. 
  Nutze für diese GameObjects das `EditorOnly`-Tag, sodass Unity beim Bauen des Projekts diese GameObjects entfernt.
* Nutze leere GameObjects als Container, sobald Du mehr als 2 zusammenpassende Kind-Objekte hast.

### Benamung

* Nutze keine Leerzeichen innerhalb von GameObject-Namen.
* Nutze **PascalCase**, z.B. `MainDoor`, `LeverTrigger`.
* Benenne auch Prefab-Instanzen passend in der Hierarchy um.

## Projektdateien

### Benamung

* Gleiche Regeln wie bei [Scene & Hierachy](#scene--hierarchy)
* Benenne Deine Objekte so, dass sie auf natürliche Art und Weise gruppiert werden, wenn sie im gleichen Ordner sind.
  * Start beim Namen mit dem "Ding" zu dem es gehört, z.B. `PlayerAnimationController`, `PlayerIdle`, `PlayerRun`, ...
  * Wenn es sinnvoll ist, können Objekte so benannt werden, dass ähnliche Objekte zusammenbleiben oder durch ein Adjektiv anders gruppiert werden würden. Beispiel: In einem Ordner mit Requisiten würde man Tische nach dem Schema `TableRound` und `TableRectanngular` benennen statt `RectangularTable` und `RoundTable`, sodass alle Tische logisch gruppiert werden.
* Vermeide Dateitypen in Namen, z.B. nutze `ShinyMetal` statt `ShinyMetalMaterial`.

### Ordnerstruktur

Beispielstruktur:

```
- Assets
    |- _Game [1]
        |- Art
            |- Buildings
                |- LightningTower
                    |- Materials
                    |- Prefabs
            |- Environment
                |- Nature
                    |- Materials
                    |- Prefabs
        |- Scenes [2]
            |- Examples [3]
            |- Menus
            |- Levels
        |- ScriptableObjects (Instanzen) [4]
        |- Scripts [5]
            |- Events
                |- ScriptableObjects (Definition)
        |- UI
            |- Materials
    |- ... (eventuelle Drittanbieterintegrationen)
```

1. `_Game`-Ordner, das ist unser Root-Ordner für das Spiel. Wir platzieren keinerlei Assets direkt im `Assets`-Ordner von Unity. Diesen halten wir frei für Drittanbieterintegrationen, z.B. Steam.
2. Im Ordner `Scenes` legen wir alle Scenen des Spiels ab, logisch gruppiert in weiteren Unterordnern.
3. Im Ordner `Examples` kannst Du, wenn Du neue Systeme für das Spiel implementierst, eine Beispielszene ablegen, um anderen zu zeigen, wie es funktioniert.
4. Instanzen von ScriptableObjects legen wir separat in diesem Ordner ab.
5. In diesem Ordner legen wir alle Skripte ab, gruppiert nach jeweiligem System. 

Generell gilt, dass zusammengehörende Dinge in einem Ordner gruppiert werden sollen. Im Zweifel lieber einen Ordner mehr als zu wenig. 