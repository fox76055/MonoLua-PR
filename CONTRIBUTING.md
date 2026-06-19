# Вклад в разработку Monolith DS

Если вы собираетесь внести вклад в разработку Monolith DS, обратитесь к [руководству по Pull Request’ам от Wizard's Den](https://docs.spacestation14.com/en/general-development/codebase-info/pull-request-guidelines.html) - оно послужит хорошей отправной точкой по качеству кода и работе с ветками. Обратите внимание, что у нас нет разделения на master/stable ветки.

Постарайтесь придерживаться правила **"Одно изменение - одиин PR"** (например, не совмещайте никак не связанные между собой багфиксы и изменения баланса). При создании крупных работ делите изменения по коммитам (например, изменения карт - один коммит, добавление скафандров - уже другой).

> ⚠️ **Не используйте веб-редактор GitHub.** Pull Request’ы, созданные через веб-редактор, могут быть закрыты без рассмотрения. Исключением являются мелкие правки в коде **уже существующего** PR.

"Upstream" означает [репозиторий space-wizards/space-station-14](https://github.com/new-frontiers-14/frontier-station-14/), из которого был сделан форк.

---

## Контент, специфичный для Фронтира

Всё, что вы создаёте с нуля (в отличие от изменений в существующем upstream-коде), должно размещаться в подкаталогах с префиксом `_LuaM`.

**Примеры:**
- `Content.Server/_LuaM/Shipyard/Systems/ShipyardSystem.cs`
- `Resources/Prototypes/_LuaM/Loadouts/role_loadouts.yml`
- `Resources/Audio/_LuaM/Voice/Goblin/goblin-scream-03.ogg`
- `Resources/Textures/_LuaM/Tips/clippy.rsi/left.png`
- `Resources/Locale/en-US/_LuaM/devices/pda.ftl`
- `Resources/ServerInfo/_LuaM/Guidebook/Medical/Doc.xml`

---

## Изменения файлов из upstream

Если вы вносите изменения в C# или YAML-файлы из upstream, **обязательно добавляйте комментарии около или вокруг изменённых строк**. Это поможет упростить разрешение конфликтов при будущих обновлениях.

Если вы изменяете значения, используйте формат комментария `LuaM: Старое > Новое`.

**Для YAML:**
- Если вы добавляете прототип или набор прототипов подряд - используйте блочные комментарии.
- Если изменяете отдельные поля прототипа - комментируйте каждое по отдельности.

**Для C#:**
- Если вы добавляете много кода, рассмотрите возможность вынесения в `partial class`, когда это уместно.
- Если изменяете отдельную строчку кода - комментируйте каждое изменение по отдельности.

> ⚠️ Fluent-файлы (.ftl) **не поддерживают комментарии на одной строке с переводом** - оставляйте комментарии строкой выше.

---

## Примеры комментариев

**Изменение поля YAML:**
```yml
- type: entity
  id: TorsoHarpy
  name: "harpy torso"
  parent: [PartHarpy, BaseTorso] # LuaM: added BaseTorso
```

**Изменение значения:**
```yml
  - type: Gun
    fireRate: 4 # LuaM: 3 > 4
```

**Добавление нового прототипа:**
```yml
  - type: ItemBorgModule
    moduleId: Gardening #Lua
    items:
    - HydroponicsToolMiniHoe
    - HydroponicsToolSpade
    - HydroponicsToolClippers
#   - Bucket # Commented by LuaM
# LuaM-start:
  - type: DroppableBorgModule
    moduleId: Gardening
    items:
    - id: Bucket
      whitelist:
        tags:
        - Bucket
# LuaM-end.
```

**Добавление using'а в C#:**
```cs
using Content.Client._NF.Emp.Overlays; // LuaM
```

**Обёртка над блоком нового кода:**
```cs
component.Capacity = state.Capacity;

component.UIUpdateNeeded = true;

//LuaM-start:
if (TryComp<StampComponent>(uid, out var stamp))
{
    stamp.StampedColor = state.Color;
}
//LuaM-end
```

---

## Карты

По кораблям и POI читайте [Ship Submission Guidelines](https://frontierstation.wiki.gg/wiki/Ship_Submission_Guidelines) на вики Frontier.

В общих чертах:

- Frontier использует специальные прототипы для POI и кораблей, содержащие информацию о спавне, цене и категориях.
- Для кораблей используйте `VesselPrototype` в `Resources/Prototypes/_LuaM/Shipyard`, для POI - `PointOfInterestPrototype`.

Если вы вносите изменения в существующую карту, согласуйте это с её мейнтейнером или автором. Избегайте одновременной работы нескольких людей над одной картой - это создаёт конфликты, которые сложно разрешить.

---

## Перед отправкой PR

Перед отправкой проверьте diff на GitHub: убедитесь, что нет случайных изменений, лишних коммитов или иных нежелательных ошибок.

---

## Дополнительные ресурсы

Если вы новичок в разработке SS14:
- Посмотрите [документацию SS14](https://docs.spacestation14.io/)

---

## Контент, созданный ИИ

Контент, созданный ИИ (код, спрайты и т.п.), **запрещено** добавлять в репозиторий.

В случае подозрения на использование подобного контента ваш PR может быть закрыт без объяснения причин.
