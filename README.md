# # Второе тестовое задание Unreal Engine Developer от Fntastic

 **Задание, в котором на сцене расположены три “гнезда”: “гнездо-1”, “гнездо-2” и “гнездо-3”и подразумевается, что из гнезд могут появиться черепашки.**


# Запуск

При запуске файла **.uproject** открывается уровень с выполненным заданием. Нажмите на кнопку **Play** чтобы проверить. Кнопки пронумерованы, нажмите на кнопки и посмотрите как появляются и двигаются ~~черепашки~~!

## Привязка клика

Привязка клика реализована через **level blueptint**, При **Event BeginPlay** биндятся события на нажатия кнопок и запуск спавна из гнезд. События биндятся через референсы уровня. По клику на кнопку запускается анимация (сделана через timeline) и запускается ивент **Spawn Character** у привязанного **NestBP**.

## Как происходит спавн

Спавн реализован в **NestBP**. При запуске ивента **Spawn Character** сперва активируется система частиц, затем проигрывается звук, которые имитируют действие портала. Также проигрывается анимация портала **StargateAnim** и на 3 секунды меняется один из его материалов (внутренний, с молниями) на более яркий. 

После этого вызывается нода **SpawnActor** Она спавнит **JoleenBP** на расстоянии 250 единиц впереди портала. После спавна деактивируется система частиц, и у персонажа поочередно вызываются функции **Initial** (голосовой эффект персонажа и активация частиц шагов), **Set Destination**(передается аргумент **Character Destination** из **NestBP**) и **Move to Point**.

## Перемещение персонажа

В **NestBP** задается Move Type. Принимаемые значения от 0 до 2, где:

 - 0 -  черепашка приходит в движение и проходит от своей позиции вперед, до точки назначения.
 - 1 - черепашка приходит в движение и проходит от своей позиции вперед, до точки назначения по следующим правилам: идет секунду вперед и на полсекунды останавливается, опять идет секунду вперед и на полсекунды останавливается, и т.д.
 - 2 - черепашка приходит в движение и проходит от своей позиции вперед, до точки назначения по следующим правилам: идет секунду вперед и затем полсекунды назад, опять секунду вперед и полсекунды назад, и т.д.
Этот аргумент передается в функцию **Move to Point**, которая отвечает за выбор типа передвижения и запускает выбранное событие

Все виды передвижения запускаются через разные **Custom Events** и описаны в **Event Graph** блюпринта **JoleenBP**
При любом из трех способов передвижения производится проверка достижения конечной точки (с погрешностью **tolerance** и при достижении конечной точки вызывается функция **Arrive at Destination**, которая проигрывает звук и запускает систему частиц, после чего уничтожает персонажа

