---
title: Краш-репорты
description: Узнайте, что делать с краш-репортами и как их читать.
authors:
  - IMB11
---

# Краш-репорты

:::tip
Если у вас возникли какие-либо трудности с поиском причины краша, вы можете обратиться за помощью в [дискорд Fabric](https://discord.gg/v6v4pMv) в каналах `#player-support` или `#server-admin-support`.
:::

Краш-репорты являются очень важной частью устранения проблем с вашей игрой или сервером. Они содержат много информации о краше и могут помочь вам найти причину краша.

## Поиск краш-репортов

Краш-репорты находятся в папке `crash-reports` в папке вашей игры. Если у вас сервер, то они хранятся в папке `crash-reports` в папке сервера.

Для сторонних лаунчеров вам следует посмотреть в их документацию, чтобы найти краш-репорты.

Краш-репорты можно найти по этим путям:

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

## Чтение краш-репортов

Краш-репорты очень большие, их чтение может быть непонятным. Однако они содержат много информации о краше и могут помочь вам найти причину краша.

Для этого гайда, мы используем [этот краш-репорт в качестве примера](https://github.com/FabricMC/fabric-docs/blob/main/public/assets/players/crash-report-example.txt).

### Разделы краш-репорта

Краш-репорты состоят из нескольких разделов, разделённых заголовками:

- `---- Minecraft Crash Report ----`, краткое описание краша. Этот раздел содержит основную ошибку, вызвавшую краш, время возникновения и трассировку стека. Это главный раздел краш-репорта, так как трассировка стека обычно содержит мод, который вызывает краш.
- `-- Last Reload --`, этот раздел на самом деле бесполезен, если только краш не произошёл во время перезагрузки ресурсов (<kbd>F3</kbd>+<kbd>T</kbd>). Этот раздел содержит время последней перезагрузки и трассировки стеков любых ошибок, возникших в процессе перезагрузки. Эти ошибки обычно происходят из-за ресурс-паков, и их можно игнорировать, если только они не вызывают проблем с игрой.
- `-- System Details --`, этот раздел содержит информацию о вашем компьютере, такую как операционная система, версия Java и объем памяти, выделенный для игры. Этот раздел полезен для определения того, используете ли вы правильную версию Java и выделили ли вы игре достаточно памяти.
  - В этом разделе Fabric добавит строку `Fabric Mods:` со списком всех установленных модов. Этот раздел полезен для определения конфликтов между модами.

### Разбор краш-репорта

Теперь, когда мы знаем, что представляет собой каждый раздел, мы можем начать разбирать краш-репорт и находить причину краша.

Используя файл выше, мы можем проанализировать краш-репорт и найти причину краша, включая моды, которые вызвали сбой.

Трассировка стека в разделе `---- Minecraft Crash Report ----` является наиболее важной в данном случае, поскольку она содержит основную ошибку, вызвавшую краш. В нашем случае ошибка - `java.lang.NullPointerException: Cannot invoke "net.minecraft.class_2248.method_9539()" because "net.minecraft.class_2248.field_10540" is null`.

Учитывая количество модов, упомянутых в стеке, может быть трудно указать на конкретную причину, но первое, что нужно сделать, это найти мод, который вызвал сбой.

```:no-line-numbers
at snownee.snow.block.ShapeCaches.get(ShapeCaches.java:51)
at snownee.snow.block.SnowWallBlock.method_9549(SnowWallBlock.java:26) // [!code focus]
...
at me.jellysquid.mods.sodium.client.render.chunk.compile.pipeline.BlockOcclusionCache.shouldDrawSide(BlockOcclusionCache.java:52)
at link.infra.indium.renderer.render.TerrainBlockRenderInfo.shouldDrawFaceInner(TerrainBlockRenderInfo.java:31)
...
```

В данном случае мод, вызвавший сбой, называется `snownee`, поскольку это первый мод, упомянутый в трассировке стека.

Однако, учитывая количество упомянутых модов, это может означать, что между модами проблемы с совместимостью, и мод, вызвавший краш, может быть не тем модом, который виноват. В этом случае лучше всего сообщить о краше автору мода и позволить ему изучить краш.

## Краш миксина

:::info
Миксины - это способ для модов модифицировать игру без необходимости изменять исходный код игры. Они используются многими модами и являются очень мощным инструментом для разработчиков модов.
:::

Когда происходит краш миксина, в трассировке стека обычно упоминается миксин и класс, который он модифицирует.

Метод миксина будет содержать `modid$handlerName` в трассировке стека, где `modid` - айди мода, а `handlerName` - название обработчика миксина.

```:no-line-numbers
... net.minecraft.class_2248.method_3821$$$modid$handlerName() ... // [!code focus]
```

Вы можете использовать эту информацию, чтобы найти мод, вызвавший краш, и сообщить о нём автору мода.

## Что делать с краш-репортами

Лучшее, что можно сделать с краш-репортами - это загрузить их на сайт для текстов, а затем отправить ссылку автору мода либо в раздел с проблемами мода, либо через какую-либо форму связи (дискорд и т.д.).

Это позволит автору мода изучить краш, потенциально воспроизвести его и решить проблему, которая его вызвала.

Распространенными сайтами для текстов, которые часто используются для краш-репортов, являются:

- [GitHub Gist](https://gist.github.com/)
- [Pastebin](https://pastebin.com/)
- [mclo.gs](https://mclo.gs/)