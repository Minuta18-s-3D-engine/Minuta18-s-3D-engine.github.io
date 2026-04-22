---
layout: default_with_comments
title: "Кружок геймдев: конспект занятия 1"
parent: "2026"
comments: true
nav_order: 1
---

<span class="text-grey-dk-000">Автор: ![Ivanmaxx](https://github.com/Ivanmaxx)</span>

<span class="text-grey-dk-000">Лицензия: Creative Commons Attribution 4.0 International</span>

О жанре игры: ![https://docs.google.com/presentation/d/1wmIz9v3b...]([https://docs.google.com/presentation/d/1wmIz9v3b04m8vIhyfr7VNmuycM93pMEf98-MUHdf-Ok/edit?usp=sharing])

Начнём работу с файла, который был приложен в чате 6 апреля.

## Заменяем прямоугольник текстурой 

Для начала заменим прямоугольник на основной сцене дельной текстурой. Удалим узел `ColorRect` и добавим `AnimatedSprite2D` (даже учитывая, что объект не анимирован). В нём добавим новый `SpriteFrames`.

<здесь скоро будет пикча>

Выделим созданный `SpriteFrames` и добавим текстуру на пути `assets/Tiny Swords (Free pack)/Buildings/[любой цвет]/Barracks.png`.

<здесь скоро будет пикча>

Готово.

## Завершаем создание базового игрока

Добавим помимо анимации `default` анимацию `run`. Для этого сначала нажмите в открытом `SpriteFrames` кнопку слева-сверху с зелёным знаком `+`. Затем переименуем новую анимацию на `run` (1) и выберем спрайт для этой анимации (2). 

![](/assets/images/guests/2026/gamedev-precis-1/1.png)

Выберем нужную текстуру (`assets/Tiny Swords (Free pack)/Units/[цвет юнитов]/Warrior/Warrior_run.png`).  Для добавления спрайтов сначала нажмите `автонарезка`, а потом `выделить все`. После добавьте кадры в анимацию.

![](/assets/images/guests/2026/gamedev-precis-1/2.png)

Добавим в скрипт игрока возможность проигрывать и сменять анимации. 

Все новые строки прокомментированы:

```gdscript
extends CharacterBody2D
var is_flipped: bool = false # Переменная того, в какую сторону направлен игрок (false - вправо, true - влево)

const SPEED = 300.0
func _physics_process(delta: float) -> void:
    var move_dir: Vector2 = Input.get_vector(
            "left", "right", "up"," down")
    
    if move_dir:
        velocity = move_dir * SPEED
        $AnimatedSprite2D.play("run") # Если игрок двигается, то проигываем анимацию бега
    else:
        velocity = Vector2.ZERO
        $AnimatedSprite2D.play("default") # Если игрок стоит, то проигрываем анимацию стояния на месте
    
    if move_dir.x: # Если мы сменили направление по горизонтали, то:
        is_flipped = move_dir.x < 0 # Двигаемся ли мы влево (x<0)?
        $AnimatedSprite2D.flip_h = is_flipped # Если мы двигаемся влево, переворачиваем игрока по горизонтали
    
    move_and_slide()
```
